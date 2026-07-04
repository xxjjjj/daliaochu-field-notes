---
title: Agent-to-IM 桥接工具生态图谱：agents-to-im / Claude-to-IM-skill / ClaudeTeam / clisbot / Kagura / cc-connect
date: 2026-05-14
discovery_source:
  type: 群聊讨论
  title: 把 Codex / Claude Code 接到飞书、Slack 等 IM
  url:
primary_object:
  type: open_source_landscape
  name: Agent-to-IM Bridge 生态
  url:
object_type: [open_source_project, comparison]
source_type: [GitHub, 群聊线索]
business_tags: [ITBP, 个人能力, 管理, 流程提效]
problem_tags: [流程提效, 组织协同, MVP验证, 选型对比]
method_tags: [Agent, AI Coding, Bridge, IM, 自动化]
tool_tags: [Claude Code, Codex, Gemini CLI, Feishu, Lark, Slack, Telegram, Discord]
value_stage: 可小实验
risk_tags: [国内可用性, 数据安全, 权限, 合规, 维护风险, 多入口冲突]
public_level: public
---

# Agent-to-IM 桥接工具生态图谱

## 1. 这是什么

"AI Coding Agent 不再被锁在终端里"，而是可以在 IM（飞书 / Lark / Slack / Telegram / Discord / 微信）里被 @ 触发后远端执行。这个方向上 2026 年集中爆发了一批互有差异的开源项目，本卡是其中代表项目的横向对比与选型参考。

| 项目 | 仓库 | 核心定位 | 主要 IM | 主要 Agent |
| --- | --- | --- | --- | --- |
| cc-connect | [chenhg5/cc-connect](https://github.com/chenhg5/cc-connect) | 多平台 Agent 桥接 | 飞书、钉钉、微信、Slack、Telegram、Discord、LINE、企业微信 | Claude Code / Cursor / Gemini CLI / Codex |
| agents-to-im | [francize/agents-to-im](https://github.com/francize/agents-to-im) | DM 控制平面 + 每会话独立飞书群 | Feishu / Lark | Claude Code / Codex |
| Claude-to-IM-skill | [op7418/Claude-to-IM-skill](https://github.com/op7418/Claude-to-IM-skill) | Skill 形态的桥接 daemon | Telegram / Discord / Feishu / Lark / QQ / WeChat | Claude Code / Codex |
| ClaudeTeam | [zylMozart/ClaudeTeam](https://github.com/zylMozart/ClaudeTeam) | 从一句 prompt 起动态组队 | Lark / Feishu | Claude Code / Codex / OpenClaw / 多 CLI |
| clisbot | [longbkit/clisbot](https://github.com/longbkit/clisbot) | tmux 持久运行 + 多 channel | Slack / Telegram / Zalo Bot / Zalo Personal | Claude Code / Codex / Gemini CLI |
| Kagura | [Innei/Kagura](https://github.com/Innei/Kagura) | Slack 原生 + 持久记忆 + Web review | Slack（Socket Mode） | Claude Agent SDK / Codex CLI |

## 2. 原始来源

- 主要发现入口：打捞处群聊 2026-05-14，用户在讨论"用 IM 驱动 Codex / Claude Code"的同类方案时一次性搜出多个项目
- 同方向补充：handclaw（`deciding/handclaw`，未在本轮深入展开）
- 已存在的关联笔记：`notes/2026-05/2026-05-09-cc-connect-local-agent-to-messenger-bridge.md`

## 3. 核心观点 / 核心能力差异

1. **架构差异**
   - cc-connect / clisbot / Claude-to-IM-skill：把 IM 看作"触发面"，Agent 仍在本地 CLI / tmux 运行，本机不暴露公网端口。
   - agents-to-im：明确提出 "DM 是控制平面，每个 `/new:claude` / `/new:codex` 创建独立飞书群绑定单一 session"，避免多任务共享一个群造成状态污染。
   - ClaudeTeam：更进一步，把 Agent 抽象为"动态组队的团队"，一个 manager agent 触发后会自动 hire / fire worker agents（Claude / Codex / OpenClaw / Kimi / Gemini / Qwen 等），并自带 memory + reflection。
   - Kagura：把 IM 当作"舞台"，每个 thread 都有自己的 agent session；自带 SQLite 持久记忆、后台 reconciler、按 thread 切换 provider / model、跑完后在 Slack 给出可读 diff 的 Web review 面板。

2. **会话隔离策略**
   - 群内共享 thread：实现简单，但 Agent 输出与上下文会污染群聊（cc-connect / clisbot 默认行为）。
   - 每会话独立群：彻底隔离但群数量爆炸（agents-to-im）。
   - thread + workspace routing：用 Slack/IM 原生 thread 隔离，同时支持按 thread 切 provider（Kagura）。

3. **权限审批交互**
   - Telegram / Discord：按钮 `[Allow] / [Deny]` 直观。
   - Feishu / Lark / 微信：飞书支持 `card.action.trigger` 回调（按钮），微信 / QQ 走文本 `/perm` 命令或 `1/2/3` 数字回复（Claude-to-IM-skill 设计）。

4. **持久化与记忆**
   - cc-connect / clisbot：本地 JSON + tmux 状态，最轻量。
   - agents-to-im：`~/.agents-to-im/` 下 sessions.json / bindings.json / messages/，restart 可续。
   - ClaudeTeam：跨任务的"团队记忆"，自带 self-reflection。
   - Kagura：分层记忆（global / workspace / preference），SQLite 持久化，OpenAI 兼容 LLM 跑后台 reconciler 去重 + 过期清理。

5. **审阅 / Diff 回写**
   - Kagura 是目前唯一内置完整 "Web review panel" 的项目：每个 thread 跑完都会在 Slack 留一个按钮，打开后是 GitHub 风格的 split / unified diff、文件树、Shiki 高亮、j/k/gg/G 键盘导航。
   - 其他项目通常只在 IM 内文本回写。

6. **多 Agent / 模型调度**
   - 只有 ClaudeTeam 真正做了"动态 hire / fire worker"的多 Agent 编排。
   - 其他多数项目支持"按 thread 切 provider / model"，但仍是单 Agent 执行。

## 4. 我学到了什么

- **"AI Coding 入口"已经变成一个新的产品门类**：2026 年集中出现 5-6 个互有差异的开源项目，说明需求真实但用户场景被严重切分（个人 vs 小团队 vs 大团队；Slack vs 飞书 vs 微信；单 Agent vs 多 Agent）。
- **群聊共享 thread 是反模式**：所有支持"每会话独立群 / thread / workspace"的项目都把它作为核心卖点；共享 thread 会让 Agent 输出、上下文、别人的闲聊互相污染。
- **持久记忆 + 审阅面板 = 真正的产品壁垒**：Kagura / ClaudeTeam 的差异化在于把 Agent 输出"翻译成团队能消费的形态"，而不是只把消息搬进搬出。
- **微信个人号桥接仍然脆弱**：所有项目里只有 Claude-to-IM-skill 明确支持微信，但走 `ilinkai.weixin.qq.com` 第三方通道，注定有风控风险，不要在生产路径上依赖。

## 5. 它是否可信，哪些需要验证

- **可信度**：六个项目都活跃维护，都至少有一个示范仓库或文档；agents-to-im 与 Claude-to-IM-skill 互为 fork 关系（README 自承从 Claude-to-IM-skill 改名而来）。
- **需要验证**：
  - 国内网络下 Slack / Telegram / Discord 的可达性（部分项目要求外网）。
  - 飞书 App 权限配置：`im.message.receive_v1` / `card.action.trigger` 等事件订阅必须开齐，否则按钮 / 权限交互静默失败。
  - Codex / Claude Code 新版本兼容性（Codex 0.130.0 已出现 apply_patch hang，社区仍在跟进）。
  - 审计与日志：默认日志是否符合公司合规要求？需要补结构化改造。

## 6. 对个人能力有什么价值

- **从"会写代码"升级为"能编排 Agent"**：理解这套生态后，能判断"什么时候用 cc-connect 单点跑通就够、什么时候需要升级到 ClaudeTeam / Kagura"。
- **远程指挥 Agent 的肌肉记忆**：出差、通勤时仍然在 IM 里推项目，而不是被设备绑死。
- **跨平台 IM 工程能力**：飞书 / Slack / 微信的开放 API 各有差异，跑通一套就能迁移。

## 7. 对企业 AI 落地有什么价值

- **统一 Agent 入口**：把多个 Agent 项目集中接到一个 IM 平台（飞书国内 / Slack 海外），业务部门在群里就能触发预置 Skill。
- **降低 AI Coding 门槛**：业务方不需要学终端命令，但仍然能在受控工作目录下让 Agent 干活。
- **风险隔离**：每项目独立 work_dir / allow_from，避免一个项目的误操作影响其他项目（cc-connect / agents-to-im 都内置）。
- **可观测性**：daemon 日志 + 会话消息持久化可作为审计源。
- **多 Agent 协同**：当单一 Agent 能力不足时，ClaudeTeam 给出了"动态组队"的参考实现。

## 8. 可做的小实验

- 选 1 个非生产项目，对比 cc-connect / agents-to-im / clisbot 的最小闭环完成时间。
- 同时配置 2 个 work_dir（如 `home-claude` / `main-project`），验证会话隔离是否真的生效。
- 跑一次"权限审批交互"：在飞书 / Slack 里观察一次 allow / deny 的体验差异。
- 如果团队愿意上 Slack：试用 Kagura，重点体验 thread-aware + Web review panel。
- 如果业务需要多 Agent 协同：试用 ClaudeTeam，重点观察 manager agent 的 hire / fire 决策是否稳定。

## 9. 风险和边界

- **国内可用性**：Slack / Telegram / Discord 在国内不可达，相关项目（clisbot / Kagura）不适合纯国内团队。
- **数据安全**：Agent 暴露在 IM 入口后，敏感目录必须显式 allow_from + work_dir 隔离；allow_from 设成空或通配符等同把本机交给陌生人。
- **多入口冲突**：和 cc-connect 笔记里的踩坑一致——当 OpenClaw wrapper / npm / Homebrew / cc-connect 同时安装 Codex，会出现入口链污染，建议只留一套并用 `which` / `type` 定期校验。
- **维护风险**：所有桥接项目都依赖上游 Agent CLI（Claude Code / Codex / Gemini CLI）的稳定性，版本同步可能滞后。
- **微信个人号依赖**：Claude-to-IM-skill 的微信支持走第三方通道，随时可能被风控，不要在生产路径上依赖。
- **合规**：公司内使用需要确认 IT 资产管理、代码外发、数据出域等政策。

## 10. 当前结论

Agent-to-IM 桥接是 2026 年最值得关注的"AI Coding 入口"产品门类之一。cc-connect 已经写入笔记，本卡补充了 5 个新项目（agents-to-im / Claude-to-IM-skill / ClaudeTeam / clisbot / Kagura）的横向对比。

**初步选型建议**：
- 个人 / 小团队 / 国内飞书为主：cc-connect → agents-to-im → Claude-to-IM-skill 渐进。
- 海外 Slack 团队：Kagura（thread 隔离 + Web review panel 是差异化）。
- 需要多 Agent 协同：ClaudeTeam（目前唯一做动态组队的）。

落地前先沉淀一份"Agent 入口卫生 SOP"避免重蹈 cc-connect 笔记里多入口污染的覆辙。
