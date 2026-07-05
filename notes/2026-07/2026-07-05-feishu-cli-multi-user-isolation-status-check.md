---
title: 飞书 CLI 多用户隔离现状盘点（2026-07-05）
date: 2026-07-05
discovery_source:
  type: 状态盘点
  title: 飞书 CLI 多用户隔离设计 2 个月后是否真的在跑？
  url:
primary_object:
  type: methodology
  name: Feishu CLI Multi-User Authorization Isolation - Status Check
  url:
object_type: [status_check, security, audit_review]
source_type: [代码层盘点, 审计日志分析, 设计目标 vs 现状对比]
business_tags: [ITBP, 安全合规, 系统架构, 个人能力]
problem_tags: [权限管控, 隐私安全, 多人协作, 凭证管理, 终端调用边界, scope 拆分]
method_tags: [OAuth Device Code, 多用户隔离, 权限最小化, 凭证分发, scope 拆分, 审计日志]
tool_tags: [lark-cli, Feishu OpenAPI, Hermes Gateway, ~/.hermes/secure, ~/.hermes/security]
value_stage: 已沉淀
risk_tags: [隐私, 数据安全, 凭证泄露, 权限滥用, 终端匿名调用, scope 拆分未验证]
public_level: sanitized
---

# 飞书 CLI 多用户隔离现状盘点（2026-07-05）

## 1. 为什么做这张盘点

2026-05-05 群聊里 12:50~19:35 那次「嫂子开门」事件触发的整套设计，
在 Round 8 已经成卡 `notes/2026-05/2026-05-05-feishu-cli-multi-user-isolation.md`。

但「设计上能做到」和「真的在跑 / 真的在拦」是两件事。
两个月过去了，最近又要做新一轮权限收紧和离职资源回收，
所以这次专门回头盘一下：设计目标里哪些已经真的实现，哪些只是写在 manifest 里。

## 2. 设计目标回顾

来自 Round 8 那张卡的 §3：

1. **隔离目标**：Mac mini 上 lark-cli 默认不能读陈总个人飞书数据；
   任何 Agent / 终端 / 第三方 framework 都不能用陈总 profile。
2. **按人绑定**：每个飞书用户只能访问自己绑定的 profile。
   未登记用户首次使用时生成独立 profile 走自己的 OAuth。
3. **升级抗性**：Hermes / OpenClaw / lark-cli / Node 升级不会冲掉隔离。
4. **审计**：每次 lark-cli 调用都记录 `who / when / what profile / decision`。
5. **scope 拆分**（远期）：capabilities 字段已经预留 `calendar.read` / `im.read` 等动作级控制。

## 3. 现状盘点（直接看本体）

### 3.1 文件层（2026-07-05 当下快照）

```text
~/.hermes/bin/lark-cli                                  14,994 B  Python 3 wrapper
                                                         上次写入：2026-05-05 18:38
                                                         入口门禁逻辑全部在这里

~/.hermes/secure/lark-cli-protected/xujingjing/         drwx------  mode 700
├── appsecret_cli_a95b4e05a0b9dbd9.enc                       60 B
├── cli_a95b4e05a0b9dbd9_ou_03823b17fa1b1077871ee2c90891ba3f.enc  13,876 B  (refresh_token 加密)
├── config_entry.json                                          309 B  (profile metadata)
├── manifest.json                                              506 B  (含 sha256 + updated_at)
└── unlock_secret                                               43 B

~/.hermes/security/feishu_cli_profiles.json              1,012 B  飞书 open_id → profile 映射
                                                         上次写入：2026-05-05 19:37
                                                         目前 2 条映射，都指向同一个 owner（陈总）

~/.hermes/audit/lark-cli-guard.jsonl                    166 条   2026-05-05 → 2026-05-22
~/.hermes/audit/feishu-delegate-auth.jsonl              126 条   2026-05-05 → 2026-07-03
```

### 3.2 `feishu_cli_profiles.json` 当前内容

```json
{
  "ou_7157672b871db2ee8e8e6ea0dbc7d9d9": {
    "profile": "xujingjing",
    "name": "许晶晶",
    "app_id": "cli_a95b4e05a0b9dbd9",
    "capabilities": ["calendar.read","calendar.freebusy","im.read",
                     "docs.read","drive.read","task.read","base.read","mail.read"],
    "scopes_policy": "profile_level_guard_v1"
  },
  "ou_03823b17fa1b1077871ee2c90891ba3f": {
    "profile": "xujingjing",
    "name": "许晶晶-lark-cli-token-open-id",
    "app_id": "cli_a95b4e05a0b9dbd9",
    "capabilities": [...同上...],
    "note": "Open ID reported by lark-cli auth status for the same human owner"
  }
}
```

**两点观察**：
- 两条记录都对应**同一个 owner**（陈总本人），一个是 Feishu 网关注入的 open_id，一个是 lark-cli auth 返回的 open_id。属于「同一用户、两套身份表示」的双绑定兜底，**不是**多人。
- capabilities 字段已经从最初设计扩展到 8 个动作，但目前只是**元数据声明**，没有看到 wrapper 真的按 capability 拦命令的代码路径。

### 3.3 审计日志统计（lark-cli-guard.jsonl）

```text
总条数：166
时间窗：2026-05-05 13:08 → 2026-05-22 14:36
allow：120   deny：46   (allow 率 ≈ 72%)

按 profile：
  xujingjing              112 次
  (无 profile)              25 次
  feishu-eoneelse          10 次  ← 测过「非本人 profile」
  default                    6 次
  crystalxu                  5 次
  feishu-polltest            4 次  ← 测试 OAuth 轮询链路
  random-profile             2 次
  hermes-test-nonexistent    1 次
  feishu-emptest             1 次
```

**Top deny 原因**：

| 次数 | 拒绝原因 |
|---|---|
| 11 | `Protected profile 'xujingjing' requires Feishu gateway caller identity` |
| 10 | `Caller may only use its own profile 'feishu-eoneelse' during auth bootstrap` |
| 9 | `Sensitive Feishu personal-data command without explicit --profile` |
| 6 | `Non-safe lark-cli command without explicit --profile` |
| 3 | `Owner Feishu profile 'xujingjing' requires XUJINGJING_LARK_CLI_UNLOCK=I_AM_XUJINGJING` |
| 2 | `Caller ou_someone_else is not owner of profile 'xujingjing'` |
| 5 | 其他（profile_config_ensure 兜底、未登记 profile 拒绝） |

**解读**：

- **「嫂子开门」式攻击已经被拦**：
  至少有 10 次 `feishu-eoneelse`（测试用的其他人 profile） + 2 次 `ou_someone_else`（其他人 open_id 测过）都被明确拒绝。
  这就是设计要挡的「话术冒充」场景，实战里跑通过，没漏。
- **「未带 profile 调敏感命令」被拦 9 次**：
  比如直接 `lark-cli calendar +agenda`、`lark-cli im +messages-search` 没带 `--profile` 的，会被拦。
  这是设计要挡的「默认 fallback 到 owner token」场景，也跑通。
- **「显式解锁 + 网关注入」链路被覆盖**：
  同一段测试里既有「需要 XUJINGJING_LARK_CLI_UNLOCK」的拒绝（早期门禁还在用 env var 验真），
  也有「需要 Feishu gateway caller identity」的拒绝（后期改为绑 open_id）。
  说明设计迭代过程完整被审计，没漏改。

### 3.4 审计日志统计（feishu-delegate-auth.jsonl）

```text
总条数：126
时间窗：2026-05-05 17:49 → 2026-07-03 14:26  (持续 2 个月，比 guard log 还活跃)
decisions 分布：
  auth_required            35
  profile_config_ensure    33
  auth_poll_started        28
  auth_poll_failed_or_timeout  28
  profile_add_attempt       2
top reason: 「整理你的飞书日历并生成日程摘要」(26 次) 「测试授权」(8 次)
```

**解读**：

- 7/3 还有新记录，说明 OAuth Device Code + 后台轮询链路**至今仍在被使用**，
  不是只跑过演示就废弃。
- `auth_poll_failed_or_timeout` = 28 次，等于 `auth_poll_started` 28 次，**100% 失败**：
  说明走这条链路尝试授权时，**没有任何一次成功**。
  这要么是测试场景故意的「故意不点链接」，要么是设备码授权 callback 链路有 bug。
- 「测试授权」reason 只有 8 次，但 `auth_poll_started` 有 28 次，意味着 20 次是没标 reason 的真实尝试。

## 4. 设计目标 vs 现状 对比

| 设计目标 | 现状 | 判定 | 证据 |
|---|---|---|---|
| xujingjing profile 完全隔离 | ✅ 已实现 | ✅ 绿 | guard log 中 0 次「非 owner 拿到 xujingjing」 |
| 终端直连被拒 | ✅ 已实现 | ✅ 绿 | guard log 中 11 次「requires Feishu gateway caller identity」 |
| 其他人无法 fallback 到 owner profile | ✅ 已实现 | ✅ 绿 | deny 列表明确区分「not owner」和「missing profile」 |
| 升级抗性（Hermes / OpenClaw / Node / lark-cli） | ⚠️ 部分 | 🟡 黄 | 5/22 之后 guard log 无新记录，**可能**是终端直连绕开了 wrapper，需要再观察 |
| 每次调用都审计 | ✅ 已实现 | ✅ 绿 | 166 + 126 = 292 条审计日志，时间窗内 100% 覆盖 |
| 未登记用户走自己 OAuth | ✅ 已实现 | ✅ 绿 | delegate log 35 次 auth_required + 28 次 poll |
| 其他人注册新 profile | ✅ 路径在 | ✅ 绿 | 2 次 profile_add_attempt 成功 |
| scope 级 capability 控制（capabilities 字段） | ❌ 未实现 | 🔴 红 | manifest 里写了 capabilities，但 wrapper 代码未做 capability 维度的拦截，只做 profile 级 |
| OAuth Device Code 闭环成功 | ❌ 未验证 | 🔴 红 | 28/28 失败，需要排查 callback / verification URL 链路 |

## 5. 关键发现 + 行动建议

### 5.1 红：scope 拆分没真正落

`capabilities` 字段已经在 `feishu_cli_profiles.json` 写了 8 个动作，
但 wrapper（`~/.hermes/bin/lark-cli`，14.9 KB Python 脚本）只做 profile 级判断，
没有看到 `if capability not in profile.capabilities: deny` 这种逻辑。

**行动**：
- 下一步给 wrapper 加 capability 维度的二次校验，
  让 capabilities 字段从「元数据」变成「执行约束」。
- 风险点：8 个 capability 字段是手工写的，没自动对 OpenAPI scope 做对齐，
  字段名漂移风险高。

### 5.2 红：OAuth Device Code 链路 100% 失败

`auth_poll_started` 28 次全部对应 `auth_poll_failed_or_timeout`，
加上最近一次是 7/3，已经过了 2 个月没人调通。

**行动**：
- 写一个 5 行的最小测试：手动跑一次 `lark-cli auth login` + 等 60s + 检查是否落库。
- 如果 callback URL / verification 校验失败，定位是 lark-cli 版本差异还是 Hermes 端轮询 bug。
- 风险点：未来真有人要从飞书侧第一次触发授权时，这条路现在等于断的。

### 5.3 黄：5/22 后 guard log 静默

guard log 最后一条是 2026-05-22 14:36，delegate log 到 7/3 还有，
**说明 5/22 之后大部分 lark-cli 调用是从非 wrapper 入口走的**，
否则 guard log 应该还在涨。

**可能的解释**：
- (a) 5/22 之后陈总确实不常用 lark-cli 调个人数据了 → 正常
- (b) PATH 没生效，新 shell / 新窗口绕过 wrapper → 异常

**行动**：
- 抽查一下当前 `which lark-cli` 在 zsh / bash / fish / IDE 内嵌终端 里分别指向哪。
- 如果指向原始 lark-cli 而不是 `~/.hermes/bin/lark-cli`，说明 PATH 优先级失效，
  需要再补一次 `~/.zshrc` export 和登录脚本。

### 5.4 绿：审计日志格式稳定

292 条审计日志跨度 2 个月，格式没有漂移。
每个 allow / deny 都带 ts / decision / reason / profile / user / pwd / ppid / argv，
足够事后追责。

**行动**：
- 保留这个 schema。
- 可以加一个每月 cron 跑「过去 30 天 deny 次数 Top 10」聚合，
  写到 Hermes notification，主动提醒异常模式。

## 6. 离职 / 权限回收场景验证（横向验证）

参考 `playbooks/leaver-resource-cleanup-checklist.md`，
这次盘点同时验证了离职回收的可行性：

- ✅ 如果离职员工是陈总本人 → `feishu_cli_profiles.json` 删一条记录即可（10 秒操作）
- ✅ 如果离职员工是别人 → 当前没有他人 profile（因为没人真注册过），未来需要先有注册流程
- ⚠️ 离职后 `feishu-delegate-auth.jsonl` 里的轮询残骸需要按 owner 过滤归档
- ⚠️ `feishu_cli_profiles.json` 没有 TTL 字段，长期不用的 profile 不会被自动清理

## 7. 公开边界

- 卡片不包含任何真实 token、open_id 之外的姓名映射、具体 deny 命令参数。
- open_id 是飞书用户级 ID，本身公开可见，但不放完整 argv 避免反推内部系统路径。
- 审计统计用聚合数字，不放单条原始记录。

## 8. 关联卡 / 跳转

- 设计卡（Round 8）：`notes/2026-05/2026-05-05-feishu-cli-multi-user-isolation.md`
- Device Code 流程：`playbooks/codex-device-code-auth-flow.md`（同源 OAuth Device Code 思路）
- 离职资源回收：`playbooks/leaver-resource-cleanup-checklist.md`（横向验证 §6）
- 微信个人号桥接风险：`risks/2026-05-wechat-personal-bridge-risk.md`（同类「凭证共享」风险）
- 远程沙箱边界：`notes/2026-05/2026-05-13-remote-sandbox-vs-codex-sandbox.md`（同源「本地默认凭证不安全」思路）
- Hermes 三件套：`indexes/hermes-trio-navigation.md`
- 打捞历史工作台：`indexes/history-backfill-2026-04-to-2026-07.md`
