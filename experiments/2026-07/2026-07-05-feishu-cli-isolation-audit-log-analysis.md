---
title: 实验 - 飞书 CLI 多用户隔离审计日志 30 分钟快读
date: 2026-07-05
discovery_source:
  type: 实验记录
  title: 把 292 条审计日志（2026-05-05 → 2026-07-03）当一手数据源，分析实际部署状态
  url:
primary_object:
  type: experiment
  name: audit-log-driven status verification
  url:
object_type: [experiment, audit_analysis, status_verification]
source_type: [代码层盘点, 审计日志分析, Python 一次性脚本]
business_tags: [ITBP, 安全合规, 个人能力]
problem_tags: [权限管控, 隐私安全, 审计日志分析, 数据驱动决策]
method_tags: [jsonl 日志解析, Counter 聚合, 设计目标 vs 现状对比]
tool_tags: [Python stdlib, json, collections.Counter, datetime]
value_stage: 已验证
risk_tags: [隐私, 凭证泄露, 日志泄露, 路径暴露]
public_level: sanitized
---

# 实验 - 飞书 CLI 多用户隔离审计日志 30 分钟快读

## 1. 这是什么

打捞处 Round 10 的一次"用日志当一手数据源"实验：
不靠设计师自评、不靠对话回忆，
直接拉 `~/.hermes/audit/` 下两条 jsonl，
跑 30 行 Python 聚合，
判断 2 个月前的设计目标到底哪些真在跑。

## 2. 目标

验证三个假设：

1. **H1**：lark-cli guard wrapper 真的在拦所有调用（allow / deny 都有日志）
2. **H2**：OAuth Device Code 链路至少跑通过一次成功闭环
3. **H3**：scope 级 capability 控制有真实执行路径

## 3. 实验步骤（30 分钟 / 一次性脚本）

### 3.1 数据源

```bash
~/.hermes/audit/lark-cli-guard.jsonl       166 条  5/5 13:08 → 5/22 14:36
~/.hermes/audit/feishu-delegate-auth.jsonl 126 条  5/5 17:49 → 7/3 14:26
```

### 3.2 一次性 Python 脚本（不进库）

```python
import json
from collections import Counter
import datetime

for path in [...]:
    decisions = Counter()
    reasons = Counter()
    profile_use = Counter()
    with open(path) as f:
        for line in f:
            entry = json.loads(line)
            decisions[entry['decision']] += 1
            reasons[entry.get('reason','?')] += 1
            profile_use[entry.get('profile') or '(none)'] += 1
    print(path, decisions, reasons.most_common(5))
```

### 3.3 关键发现

**H1：guard wrapper 在拦（部分成立）**

```text
lark-cli-guard.jsonl: 120 allow / 46 deny (72% allow rate)
feishu-delegate-auth.jsonl: 全部都有 decision 字段 (无丢日志)
```

**结论**：guard wrapper 的日志记录链路 100% 工作。

但 5/22 之后 guard log 完全静默（最长 43 天无新记录），
delegate log 7/3 还在涨，说明 5/22 之后 lark-cli 调用大部分**没走 wrapper**。
可能原因：PATH 没生效 / 新 shell 绕过 / 用户已经习惯手动 export。

**H2：OAuth Device Code 链路从未成功闭环（推翻）**

```text
auth_poll_started        28
auth_poll_failed_or_timeout  28   ← 100% 失败
auth_required            35
profile_config_ensure    33
```

**结论**：28 次启动后台轮询，28 次全部 timeout 或失败。
意味着任何人想从飞书侧第一次触发 lark-cli 授权，**现在这条路是断的**。

**H3：scope 级 capability 控制无执行路径（推翻）**

```bash
# 在 wrapper 里搜 capability 相关逻辑
grep -n "capabilit" ~/.hermes/bin/lark-cli
# → 只看到 SAFE_COMMANDS = {"help", "schema", "doctor"} 等粗粒度判断
# → 没有按 capabilities 字段做拦截
```

**结论**：`feishu_cli_profiles.json` 里 capabilities 字段是装饰性元数据，
wrapper 代码只做 profile 级拦截，没做 capability 级。

## 4. 实验结论

| 假设 | 判定 | 证据 |
|---|---|---|
| H1：wrapper 在拦 | 🟡 部分成立 | 5/22 前日志完整，5/22 后静默需排查 |
| H2：Device Code 闭环 | 🔴 推翻 | 28/28 失败 |
| H3：capability 拦截 | 🔴 推翻 | 代码无对应逻辑 |

## 5. 投入产出

| 项目 | 数值 |
|---|---|
| 投入 | 30 分钟 + 30 行 Python + 2 次 grep |
| 产出 | 1 张现状盘点卡 + 3 个 actionable 安全 finding |
| 可复用度 | 高 — 同样的「聚合审计 jsonl → 设计目标 vs 现状」方法可以推广到任何带 jsonl 审计的系统 |

## 6. 下一步动作

1. **优先修复 H2**（OAuth Device Code 链路）
   - 写一个 5 行的最小测试：手动跑一次 + 等 60s + 检查落库
   - 失败的话定位是 lark-cli 版本差异还是 Hermes 端轮询 bug

2. **优先修复 H3**（capability 级拦截）
   - 给 wrapper 加 `if capability not in profile.capabilities: deny`
   - 风险：capabilities 字段是手工写的，没自动对 OpenAPI scope 做对齐，字段名漂移风险高

3. **次优先 H1**（5/22 后 guard log 静默）
   - 抽查 `which lark-cli` 在 zsh / bash / IDE 内嵌终端里分别指向哪
   - 如果指向原始 lark-cli 而非 wrapper，PATH 优先级失效

4. **可复用方法论沉淀**
   - 本实验可以泛化成「审计日志 → 设计目标 vs 现状」的快速验证方法
   - 适合任何 jsonl / JSON 审计日志的定期健康检查

## 7. 公开边界

- 实验脚本不进库（一次性 30 行 Python）
- 审计统计用聚合数字，不放单条原始记录
- 不暴露完整 deny argv 避免反推内部系统路径
- 仅公开方法论和判定结果，不公开具体 token 状态

## 8. 相关卡 / 跳转

- 实验产出：`notes/2026-07/2026-07-05-feishu-cli-multi-user-isolation-status-check.md`
- 同源设计卡：`notes/2026-05/2026-05-05-feishu-cli-multi-user-isolation.md`
- 同源 OAuth Device Code 流程：`playbooks/codex-device-code-auth-flow.md`
- 离职资源回收（横向验证）：`playbooks/leaver-resource-cleanup-checklist.md`
- 打捞历史工作台：`indexes/history-backfill-2026-04-to-2026-07.md`
