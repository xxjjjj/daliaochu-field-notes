---
title: Hermes Agent 心跳机制现状与"心跳 + 自愈"补充建议
date: 2026-06-28
discovery_source:
  type: 群聊讨论
  title: "你有心跳机制吗"
  url:
primary_object:
  type: open_source_project
  name: Hermes Agent
  url: https://github.com/just-every/hermes-agent
object_type: [open_source_project, methodology]
source_type: [群聊线索, GitHub, 代码层调研]
business_tags: [ITBP, 运维]
problem_tags: [流程提效, 稳定性, 监控告警, 组织协同]
method_tags: [Agent, 监控, 心跳, watchdog, launchd]
tool_tags: [Hermes, gateway/platforms/, launchd, watchdog, SSE]
value_stage: 学习理解
risk_tags: [维护风险, 国内可用性]
public_level: public
---

# Hermes Agent 心跳机制现状与"心跳 + 自愈"补充建议

## 1. 这是什么

打捞处群聊 2026-06-28 用户问"你有心跳机制吗"，Hermes 实例基于源码做了现场调研，回答了"有，但不完整"的判断，并给出补充建议。

本卡把这次现场调研的核心结论保留下来，给后续做 Hermes 部署、运维、稳定性建设的人一份参考。

## 2. 核心结论（一句话）

> Hermes **有连接保活 + 运行状态记录**，但**没有完整独立的"全局守护心跳"机制**。要做发布级稳定性，需要在 gateway 之上补一层 watchdog / launchd 自愈。

## 3. 已有机制盘点（代码层确认）

### 3.1 平台连接层：心跳 / keepalive ✅

以下平台 / 链路已经有显式心跳代码：

| 模块 | 心跳机制 | 备注 |
|------|----------|------|
| `gateway/platforms/wecom.py` | `_heartbeat_task` + `_heartbeat_loop()` | 企业微信 WebSocket 保活 |
| `gateway/platforms/homeassistant.py` | `ws_connect(..., heartbeat=30)` | Home Assistant WS 心跳 30s |
| `gateway/platforms/mattermost.py` | `ws_connect(..., heartbeat=30.0)` | Mattermost WS 心跳 30s |
| `gateway/platforms/api_server.py` | SSE 流里 30 秒无事件写 `: keepalive` | SSE 长连接防中间设备超时 |
| Discord 语音链路 | UDP keepalive | 语音会话保活 |

**含义**：每条消息链路自身的连接是健康的，不会因为中间网络设备超时而掉线。

### 3.2 Gateway 运行状态层：状态记录 ⚠️

`gateway/status.py` 里有 runtime status 文件，会记录：

- 当前 gateway pid
- gateway_state
- active_agents
- 各平台连接状态
- updated_at

**但这只是"运行状态快照 / 健康状态记录"，不是独立 watchdog 那种"定时打点、超时自动拉起"的完整心跳机制。**

**关键缺口**：

- 没有人主动"每 N 秒确认自己活着"
- 没有"超时后自动重启 / 自动告警" 的兜底
- gateway 进程崩了之后，靠人工发现，而不是自愈

## 4. 为什么这不是"完整心跳"

判断一个系统是否有"完整心跳机制"，要看四点：

| 维度 | Hermes 现状 | 完整心跳应该有 |
|------|-------------|----------------|
| 主动打点 | ❌ 没有定时打点 | 每 N 秒写一次 heartbeat timestamp |
| 超时检测 | ❌ 没有独立 watcher | watchdog / launchd 周期读取并比对 |
| 自动恢复 | ❌ 异常时不会自动重启 | 超过阈值自动拉起 gateway |
| 告警通知 | ❌ 不会有主动告警 | 失败时通过飞书 / 邮件 / webhook 告警 |

Hermes 现在只能算"有保活"，还不能算"有自愈"。

## 5. 推荐的"心跳 + 自愈"补充方案

如果要补到发布级稳定性，建议这样搭：

```
┌──────────────────────┐
│  launchd / systemd   │  ← 系统级 watchdog
│  每 10-30s 检查一次   │
└──────────┬───────────┘
           │ 读取
           ▼
┌──────────────────────┐
│  gateway heartbeat   │  ← 新增文件
│  文件                 │  ~/.hermes/runtime/gateway-heartbeat.json
│  updated_at < 60s?   │
└──────────┬───────────┘
           │ 若超时
           ▼
┌──────────────────────┐
│  恢复流程             │
│  1. 写告警日志        │
│  2. 重启 hermes       │
│  3. 推飞书告警卡片    │
│  4. 附最近错误 +      │
│     平台状态 +         │
│     active agent 数    │
└──────────────────────┘
```

### 5.1 心跳文件建议结构

```json
{
  "pid": 12345,
  "started_at": "2026-06-28T10:00:00+08:00",
  "last_heartbeat_at": "2026-06-28T15:35:30+08:00",
  "gateway_state": "running",
  "active_agents": 2,
  "platforms": {
    "feishu": "connected",
    "telegram": "connected",
    "discord": "disconnected"
  },
  "last_error": null
}
```

### 5.2 watchdog 实现思路

#### 方案 A：launchd（macOS 推荐）

```xml
<!-- ~/Library/LaunchAgents/com.hermes.watchdog.plist -->
<plist version="1.0">
<dict>
  <key>Label</key><string>com.hermes.watchdog</string>
  <key>ProgramArguments</key>
  <array>
    <string>/usr/local/bin/hermes-watchdog.sh</string>
  </array>
  <key>StartInterval</key><integer>30</integer>
  <key>RunAtLoad</key><true/>
</dict>
</plist>
```

```bash
#!/bin/bash
# /usr/local/bin/hermes-watchdog.sh
HEARTBEAT_FILE="$HOME/.hermes/runtime/gateway-heartbeat.json"
THRESHOLD=60  # 60 秒无更新则拉起

if [ ! -f "$HEARTBEAT_FILE" ]; then
  launchctl kickstart -k gui/$(id -u)/com.hermes.gateway
  exit 0
fi

LAST=$(jq -r '.last_heartbeat_at' "$HEARTBEAT_FILE")
NOW=$(date +%s)
LAST_TS=$(date -j -f "%Y-%m-%dT%H:%M:%S" "$LAST" +%s)
DIFF=$((NOW - LAST_TS))

if [ "$DIFF" -gt "$THRESHOLD" ]; then
  launchctl kickstart -k gui/$(id -u)/com.hermes.gateway
  # 发飞书告警
fi
```

#### 方案 B：cron + Python（Linux 通用）

```python
# /usr/local/bin/hermes-watchdog.py
import json, os, time, subprocess
from pathlib import Path

HEARTBEAT = Path.home() / ".hermes/runtime/gateway-heartbeat.json"
THRESHOLD = 60

def check():
    if not HEARTBEAT.exists():
        return False
    data = json.loads(HEARTBEAT.read_text())
    last = data.get("last_heartbeat_at")
    # 解析 ISO 时间，比较 diff
    ...
    return diff < THRESHOLD

if not check():
    subprocess.run(["systemctl", "restart", "hermes-gateway"])
    # 发飞书告警
```

## 6. 对 BP / ITBP 的实用价值

- **生产环境前必做**：Hermes 接入真实业务前，先把"心跳 + 自愈"补上，否则 gateway 一崩，业务方不知道，只能靠用户主动报障。
- **多实例共用 profile 时**：每个实例必须有自己的 heartbeat 文件和 watchdog，否则多个 gateway 写到同一文件会互相干扰。
- **告警通道要分层**：自愈成功不发告警（避免噪音）；自愈失败 3 次才升级告警；连续 5 次失败升级到电话 / 短信。
- **要保留现场**：重启前先抓 stack trace + 最近 50 条日志 + platform 状态快照，否则重启完根本不知道为啥挂。

## 7. 可做的小实验

1. **最小验证**：在 Hermes gateway 启动逻辑里加一行 `touch heartbeat_file`，观察是否每 N 秒更新。
2. **手动触发 watchdog**：把 Hermes 进程 `kill -9`，看 watchdog 是否能自动拉起 + 发告警。
3. **故障演练**：模拟某个消息平台连接断开，看告警链路是否打通。
4. **告警降噪**：连续一周观察告警频率，调阈值避免"半夜被假告警吵醒"。

## 8. 风险和边界

- **写心跳本身不能太重**：gateway 主循环里加 `time.sleep + 写文件` 会拖慢响应。最好用独立线程，每 10-30s 写一次。
- **watchdog 自身也要被监控**：如果 watchdog 进程也挂了，要靠系统级 supervisor（launchd / systemd）拉起，否则形成单点故障。
- **重启风暴**：gateway 因为某个 bug 频繁崩溃时，watchdog 不能无限重启，要加退避策略（首次 30s 后拉起，第二次 2min，第三次 10min）。
- **数据一致性**：重启过程中如果有正在处理的任务，要有 graceful shutdown 逻辑，否则会丢消息或重复执行。

## 9. 公开边界

- 涉及的具体模块路径、文件命名均为公开代码层信息。
- 不含任何内部环境配置、生产地址、监控告警渠道。

## 10. 相关卡

- `playbooks/feishu-to-github-auto-sync.md` — 类似的"自动化守护"模式
- `playbooks/auto-sync-validation.md` — 自动同步任务的健康检查思路
- `notes/2026-06/2026-06-28-hermes-agent-self-introduction.md` — Hermes 整体能力拆解
- `risks/2026-05-wechat-personal-bridge-risk.md` — 类似"个人号桥接"场景的稳定性风险

> ← 属于 [Hermes 三件套](../../indexes/hermes-trio-navigation.md) 之「心跳机制」；同套还有 [自我介绍](../../notes/2026-06/2026-06-28-hermes-agent-self-introduction.md) 与 [Codex 入口](../../playbooks/codex-entry-from-feishu.md)。
