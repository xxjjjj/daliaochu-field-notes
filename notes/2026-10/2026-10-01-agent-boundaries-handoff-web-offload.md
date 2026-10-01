---
title: "Agent 四层工作边界与两个常驻 Skill（Handoff / Web Offload）"
date: 2026-10-01
discovery_source:
  type: 视频二手梳理
  title: 作者自称烧 42.3 亿 token 后只常驻两个 Skill 的经验分享
  url: ""
primary_object:
  type: 方法论
  name: Agent 工作边界分层（Project / Chat / Subagent / Worktree）
  url: ""
object_type: [methodology, trend_signal]
source_type: [群聊线索]
business_tags: [ITBP, 个人能力]
problem_tags: [流程提效, 知识沉淀, 组织协同]
method_tags: [Agent, Prompt, 自动化]
tool_tags: [Codex, ChatGPT, Claude Code]
value_stage: 待追源
risk_tags: [成本, 合规, 数据安全]
public_level: needs-review
---

# Agent 四层工作边界与两个常驻 Skill

## 1. 这是什么

一条重度 Agent 用户的方法论短视频（群内为文字梳理，一手视频/作者未追到）。
作者自称累计烧掉 42.3 亿 token 后，只保留两个常驻 Skill，并把 Agent 使用经验归纳为
四层工作边界。核心论点：Agent 用久了出错，多数不是模型能力不够，而是
"什么活都往同一个上下文/同一个目录里塞"。

## 2. 原始来源

- 发现入口：打捞处群，许晶晶提供的视频内容梳理（2026-10-01）
- 资料本体：未追到。检索 "42.3亿 token"、"网页替身 skill"、四层边界等关键词，
  未命中对应视频；命中的相近生态线索：
  - handoff-cli（uv tool install handoff-cli）：多后端（claude/codex/deepseek）
    的对话交接 CLI，skillsllm.com 有收录，与视频"Handoff"概念高度相近
  - cxuan（X/Twitter, 2026-09）实测 ChatGPT 网页版经本地 MCP（WebCodex /
    DevSpace / codex-with-chatgpt 等）操作本地文件，明确提到网页版与 Codex
    用量相互独立——与"网页替身"原理一致
  - 腾讯云《单 Agent vs 多 Agent》、OpenAI Harness engineering 文章中
    worktree 隔离已是主流做法
- 待验证：视频作者身份、42.3 亿 token 口径、其两个 Skill 是否开源

## 3. 核心观点 / 核心能力

两个常驻 Skill：

1. **Handoff（对话交接）**：对话过长时自动整理"干了什么、定了什么规矩、
   下一步从哪开始"，新开对话无缝衔接，对抗上下文自动压缩导致的信息失真。
2. **Web Offload（网页替身）**：把资料搜索、调研、长文本分析等独立杂活
   分流到 ChatGPT 网页端（网页端与 Codex 用量不共享），只回收结论，
   降低主 Agent token 成本。

四层边界：

| 层级 | 含义 | 类比 | 解决的问题 |
|---|---|---|---|
| Project | 长期固定空间（AGENTS.md、DESIGN.md、规则材料） | 固定办公室 | 项目经验沉淀，新对话即插即用 |
| Chat | 单次工单 | 当天桌面上的工单 | 上下文过长→自动压缩→失真，需及时交接 |
| Subagent | 独立子任务 | 派小弟跑杂活 | 上下文隔离，脏活留在外面，只带回结论 |
| Worktree | 同仓库独立工作目录 | 每个对话一份项目副本 | 多任务并行改同一仓库互不覆盖 |

## 4. 我学到了什么

- "边界"本质是两类隔离：**上下文隔离**（Chat/Subagent 防污染、防失真）和
  **文件系统隔离**（Worktree 防互相覆盖）；Project 是跨边界的记忆层。
- token 成本优化的主流手段已经从"换便宜模型"演进到"架构性分流"：
  跨产品额度池（网页端 vs Codex）、子 agent 隔离、只回收结论。
- 与本组现状对照：Project≈我们的 AGENTS.md/Skill/知识库；Subagent≈Hermes
  delegate_task（已经在用，且子代理只回总结）；Worktree 概念我们在多 harness
  协作里部分承担，但没有形成"每对话一 worktree"的纪律；Handoff 目前靠
  session_search + 记忆，没有自动整理交接件的固定动作。

## 5. 它是否可信，哪些需要验证

- 四层边界与 OpenAI 官方 harness engineering、Claude Code worktree 实践方向
  一致，框架本身可信，不是个人独创玄学。
- 但以下仅为作者单方说法，需一手核验：
  - "42.3 亿 token"数字无凭据，属流量型标题口径；
  - "网页替身节省大量 token"：原理成立（两套额度独立），但 OpenAI 已有
    因高调薅网页端额度而收紧/制裁的先例（cxuan 帖评论区有实例），
    "自动分流"有被判定滥用的风险；
  - 视频中两个 Skill 的具体实现未看到，不能判断是否可直接用。

## 6. 对个人能力有什么价值

- 拿到一个判断纪律：任务出错率上升时，先查"是不是该换对话/该派子 agent/
  该开 worktree"，而不是先怀疑模型或加提示词。
- Handoff 交接件三段式（已完成 / 已定规则 / 下一步起点）可以直接用作
  长任务的手动交接模板。

## 7. 对企业 AI 落地有什么价值

- 给团队推广 Agent 时，"边界纪律"比"提示词技巧"更值得教：新成员最常见的
  失败模式就是单对话无限拖、所有任务混着做。
- 对成本敏感的内部场景（如本组 ark-plan 用量），子 agent 隔离+只回收结论
  已在 Hermes 体系内验证有效；跨额度池分流（网页端薅用量）不适合进入
  企业正式方案，只能作为个人技巧观察。

## 8. 可做的小实验

1. 给本组长任务定一个 Handoff 交接模板（已完成 / 已确认口径 / 下一步起点
   / 相关文件链接），在换对话时强制产出一个交接件——低成本，可马上试。
2. 观察当前 delegate_task 使用中"只回收结论"是否执行到位，对比主对话
    token 消耗。
3. Worktree 纪律暂不引入现有 harness 协作（当前文档总线模式够用），
   等出现真正并行改同一仓库冲突时再上。

## 9. 风险和边界

- **账号/合规风险**：Web Offload 类做法依赖"网页端与 Codex 额度不共享"的
  灰色地带，高调自动化分流已有被制裁先例；企业账号不要照搬。
- **数据安全**：把内部资料丢给网页端处理，涉及公司数据出域，企业场景
  不可接受；只能用于公开资料。
- handoff-cli 等第三方 CLI 会把各家凭据集中到 ~/.handoff/config.yaml，
  引入前要评估凭据管理。
- 本笔记基于二手梳理，一手视频未追到，结论按"方向可信、细节待核"使用。

## 10. 当前结论

四层边界框架与主流 Agent 工程实践一致，值得提炼成本组的工作纪律
（尤其是 Handoff 交接件和"出错先查边界"的判断习惯）；两个 Skill 本身
未追到实现，Web Offload 的灰色薅额度做法不进入企业方案。待一手视频或
对应开源仓库出现后再升级本卡。
