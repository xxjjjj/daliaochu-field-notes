---
title: Hermes 三件套导航（自我介绍 / 心跳 / Codex 入口）
date: 2026-07-05
discovery_source:
  type: 群聊讨论
  title: "介绍一下 hermes agent / 你有心跳机制吗 / Codex 编程入口之争"
  url:
primary_object:
  type: methodology
  name: Hermes Agent 实战导航
  url: https://github.com/just-every/hermes-agent
object_type: [methodology, navigation, index]
source_type: [群聊线索, GitHub, 代码层调研]
business_tags: [ITBP, 运维, 个人能力]
problem_tags: [流程提效, 稳定性, 组织协同, 知识沉淀]
method_tags: [Agent, Skill, 监控, 心跳, watchdog, acp]
tool_tags: [Hermes, gateway/, launchd, Codex, acp_command, run_agent.py]
value_stage: 已沉淀
risk_tags: [维护风险, 国内可用性, 上下文污染]
public_level: public
---

# Hermes 三件套导航

## 1. 这是什么

把打捞处群 2026-05 / 06 期间被反复追问、与 Hermes 工程结构强相关的三个核心问题，集中到一个导航页：

1. **自我介绍**：Hermes Agent 是什么、能做什么、不能做什么？
2. **心跳机制**：Hermes 现在是「活着的」吗？挂了怎么办？
3. **Codex 入口**：从飞书侧触发 Codex 写代码，到底走哪条路？

这三件事共同决定了 Hermes 是否值得被纳入团队的「稳定基建」。单看任何一张都只是一面之词；三张合在一起，才形成一份「上手 + 部署 + 接入」的最小决策包。

## 2. 三件套对应卡

### 2.1 自我介绍

- 卡：`notes/2026-06/2026-06-28-hermes-agent-self-introduction.md`
- 来源：2026-06-28 用户问「介绍一下 hermes agent」
- 一句话定义：**Hermes Agent = 一个 CLI / Gateway / Tools / Skills / Cron / ACP 多层栈的可编程 AI 代理框架，能在本地和多端（飞书、Telegram、Discord、Slack、ACP）跑通同一个 Agent 实例。**
- 适合看的人：
  - 第一次接触 Hermes，想 5 分钟内知道它是不是「又一个聊天客户端」
  - 想跟同事 / 老板解释 Hermes 价值，准备 30 秒电梯版

### 2.2 心跳机制

- 卡：`notes/2026-06/2026-06-28-hermes-heartbeat-mechanism-finding.md`
- 来源：2026-06-28 用户问「你有心跳机制吗」
- 当前判断：**有"准心跳"，没有完整自愈**。Gateway 用 SSE 长连接 + 平台重连；进程级保活需要 launchd / systemd 兜底。
- 适合看的人：
  - 想把 Hermes 部署到生产环境，必须先确认它不会静默挂掉
  - 想自己加 watchdog、alerting、auto-restart 的工程师

### 2.3 Codex 入口

- 卡：`playbooks/codex-entry-from-feishu.md`
- 来源：2026-05-14 用户提问「我想从飞书那边直接驱动 Codex 写代码」
- 当前判断：**3 条路（A. Hermes / OpenClaw 中转；B. cc-connect 直挂；C. Codex CLI 原生）**，各有权衡；目前主流实践是 A + C 组合（Hermes 收口，Codex CLI 作为子进程）。
- 适合看的人：
  - 已经在用 Codex CLI，想把它接到飞书侧的人
  - 在 cc-connect 和 Hermes Gateway 之间犹豫的人
  - 想搞清楚 `acp_command=codex` 这个参数到底干了什么的人

## 3. 三个问题的内部关系

```
                ┌──────────────────────────────────┐
                │  用户：「我想让 Hermes 帮我干活」 │
                └────────────────┬─────────────────┘
                                 │
              ┌──────────────────┼──────────────────┐
              ▼                  ▼                  ▼
       自我介绍（What）    心跳机制（Live）    Codex 入口（How）
       ──────────────     ──────────────     ──────────────
       它是什么 / 不能      它会不会悄悄挂      它怎么接外部工具
       是什么              掉，挂了怎么醒
              │                  │                  │
              └──────────────────┴──────────────────┘
                                 │
                                 ▼
                ┌──────────────────────────────────┐
                │  决策：要不要把 Hermes 纳入团队基建 │
                └──────────────────────────────────┘
```

- **自我介绍**回答「要不要用」：能力边界、定位差异。
- **心跳机制**回答「能不能稳定用」：可观测性、自愈能力、维护成本。
- **Codex 入口**回答「怎么真正用起来」：接入路径、命令路由、上下文继承。

如果只回答第一个就拍板上 Hermes，等于在裸跑一个没有监控的 Agent，迟早出事故。
如果只回答第二个，会变成「我知道它能跑稳，但跑什么活？」。
如果只回答第三个，会变成「我有入口，但不知道入口背后这个 Agent 还能干啥」。

## 4. 看这三个问题要注意的边界

1. **它们都是 2026-05/06 当时的判断**，Hermes 代码一直在演进，最新结论以 `git log --since='2026-07-01'` 后的源码为准。
2. **心跳机制那张卡要小心「理论心跳 ≠ 实际心跳」**：现有 SSE + 重连对平台侧可靠，对进程崩溃、OOM、磁盘满无感。生产部署必须额外加 `launchd KeepAlive` 或 `systemd Restart=always`。
3. **Codex 入口这张卡只在「OpenAI Codex CLI」语境下成立**，如果你想接 Claude Code / Gemini CLI，路径完全不一样（见 `notes/2026-05/2026-05-14-chatgpt-codex-cloud-and-claude-code-slack.md` 的对照段）。
4. **自我介绍里的"六层能力"是从代码结构反推的**，不是营销口径。某层能力暂时没用到，不代表不存在；用了，不代表成熟。

## 5. 关联卡 / 跳转

- Hermes 入口之争上游：`notes/2026-06/2026-06-02-colleague-skill-runtime-design.md` ——「数字分身」赛道跟 Hermes 是什么关系
- Hermes 长期记忆 / Skill 体系：`notes/2026-04/2026-04-19-llm-wiki-skill-karpathy-methodology.md`
- Hermes 在「人话翻译机」里的实际表现：`playbooks/plain-language-translator-patterns.md`
- Hermes 模型切换 / 多 provider 编排：`playbooks/hermes-default-model-rotation.md` + `playbooks/intco-modelhub-provider-setup.md`
- 群历史补录工作台：`indexes/history-backfill-2026-04-to-2026-07.md`

## 6. 适用人群一句话索引

| 我是... | 我应该先看 |
|---|---|
| 业务方，第一次听说 Hermes | §2.1 自我介绍 → §4 边界 |
| 准备部署 Hermes 的工程师 | §2.2 心跳机制 → `playbooks/hermes-default-model-rotation.md` |
| 想把 Codex 接到飞书的开发者 | §2.3 Codex 入口 → `notes/2026-05/2026-05-14-agent-im-bridges-landscape.md` |
| 想知道 Hermes 在「人话翻译」「自动催款」里到底做了什么的业务 ITBP | §2.1 自我介绍 → `playbooks/plain-language-translator-patterns.md` |
| 老板 / 高层，5 分钟判断 Hermes 是不是噱头 | §3 关系图 + §5 跳转 |
