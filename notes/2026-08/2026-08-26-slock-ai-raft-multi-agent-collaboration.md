---
title: "Slock.ai（现 Raft）— 多 Agent 协作基础设施"
date: 2026-08-26
tags: [AI-Agent, 多Agent协作, 开发者工具, 红杉中国, Kimi, coding-agent]
value_stage: 已追源
public_level: public
source_urls:
  - https://slock.ai
  - https://raft.build
  - https://www.pingwest.com/a/313580
---

# Slock.ai（现 Raft）— 多 Agent 协作基础设施

## 一句话

Slock.ai（2026年8月已更名 **Raft**，官网 raft.build）是一个"Agent 原生"的团队协作平台：把散落在本地 IDE、远程服务器、Slack/Discord 里的 Coding Agent（Claude Code、Codex、Kimi CLI 等）统一拉进一个类似 Slack 的工作空间，人和 Agent 在同一个 channel / thread / task 里协作。

## 团队背景

- **创始人 & CEO 钱宇超（Richard）**：前 Moonshot AI（月之暗面）Kimi CLI 负责人。做 Kimi CLI 后期发现单 Agent 在上下文管理、多任务协作和团队知识共享上的瓶颈，2026 年出来创业。
- **联合创始人 & CTO 庄天翼（Tenny）**：清华计算机系，前阿里巴巴 PolarDB-X 团队，做交易相关功能。
- 红杉中国（HongShan）董事总经理 Justin Li（前 App Store 负责人）在官网写了 testimonial，确认投资关系。

## 核心解决什么问题

当一个人同时开多个 Claude Code / Codex / Kimi CLI session 时：
1. **记不住**每个 Agent 在干什么、做到哪一步
2. **可能冲突**——多个 Agent 改同一份代码
3. **经验无法复用**——每个人调教 Agent 的 prompt、偏好、技能沉淀在自己机器里
4. **团队无法共享** Agent 的产出和上下文

Slock / Raft 的解法：把 Agent 放进同一个频道，共享上下文，人类可以管理、调度、review 它们的协作过程。

## 产品形态

- **Chat is the workspace**：channel / DM / thread，人和 Agent 在消息流里协作
- **Long-running agents**：每个 Agent 是持久化进程，有自己的记忆（代码库、偏好、历史对话），不是无状态 session
- **Your computers, your agents**：Agent 通过轻量 daemon 跑在用户自己的机器上，代码和数据不出本地
- **Task 系统**：可以给 Agent 派任务，它们 claim、并行执行、互相 hand off
- **多模型支持**：Claude、Codex、DeepSeek、Hermes 等，按角色选最合适的模型
- **外部 Agent 接入**：支持把已有的 Hermes 等 Agent 连进来当团队成员

## 定价

- Free：channel、task、本地 Agent、30天消息历史、100MB/月上传
- Pro：$8.80/seat/月（年付），Agent 只占 0.1 seat，无限历史、联合 channel
- Enterprise：私有化部署、SSO、高级权限（即将推出）

## 用户参考

官网展示的用户来自：ByteDance、Microsoft、Alibaba、PingCAP、HeyGen、Airwallex、Yale、Berkeley、CMU、清华、上交大、NUS 等。

TiDB 联合创始人 & CTO  Ed Huang 的 testimonial：高峰期每天烧 12 亿 token，自己已经不写代码了，角色变成"ARM（Agent Resource Manager）"。

## 与同类产品的区别

| 维度 | Raft (Slock) | Slack + AI 插件 | Claude Code / Codex | OpenClaw |
|------|-------------|-----------------|---------------------|----------|
| 定位 | Agent 原生协作空间 | 人聊天为主，AI 是外挂 | 单个 Agent 写代码 | 单个 Agent runtime |
| 多 Agent 协作 | 原生支持，共享上下文 | 弱 | 不支持 | 不支持 |
| Agent 记忆 | 持久化 | 无 | session 级 | session 级 |
| 执行位置 | 用户自己机器 | 云端 | 本地 | 本地 |

## 风险与待验证

1. **Token 成本**：多个 Agent 同时在线、共享上下文，token 消耗会指数级增长。TiDB CTO 说日烧 12 亿 token——这个成本不是所有团队能承受的。
2. **管理噪音**：Agent 多了以后，"管理 Agent"本身可能变成新的负担。
3. **竞争格局**：Slack、飞书、钉钉都在往 IM 里加 Agent；Claude Code、Cursor 也在往"Agent 团队管理"方向长。Raft 需要证明独立平台的价值大于插件。
4. **刚改名 Raft、发 1.0**：产品仍在早期，企业级功能（SSO、私有化）还没上线。
5. **数据隐私**：虽然 Agent 跑在用户机器上，但协作元数据（消息、任务状态）是否经过 Raft 服务器，需要确认。

## 对我们的启发

- 这跟晶晶姐正在做的"飞书话题群作为云端总线，Codex 和 Claude Code 各建机器人身份、通过 [TO:XXX] 协作"的思路高度一致——Raft 相当于把这套模式产品化了。
- 核心洞察：**Agent 多了以后，瓶颈不是单 Agent 能力，而是协作管理**。这个判断在我们自己用多 Agent 时也验证过。
- "Agent 占 0.1 seat"的定价设计值得关注——承认 Agent 不是人，但也不是免费资源。
- Raft 支持 Hermes 作为外部 Agent 接入，理论上可以把小马连进去试用。

## 下一步

- [ ] 注册 Raft 免费版实测，重点看：外部 Agent 接入体验、多 Agent 协作效率、token 消耗
- [ ] 关注红杉中国具体投资轮次和金额（目前未公开披露）
- [ ] 对比 FloatIM（红杉中国种子基金+微光创投 200万美元种子轮）的路线差异
