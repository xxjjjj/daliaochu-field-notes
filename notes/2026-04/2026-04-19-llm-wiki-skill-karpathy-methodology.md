---
title: llm-wiki-skill：基于 Karpathy 方法论的多平台 Agent 个人知识库
date: 2026-04-19
discovery_source:
  type: github_link
  title: sdyckjq-lab/llm-wiki-skill
  url: https://github.com/sdyckjq-lab/llm-wiki-skill
primary_object:
  type: open_source_project
  name: llm-wiki-skill (Karpathy llm-wiki 个人知识库构建 Skill)
  url: https://github.com/sdyckjq-lab/llm-wiki-skill
object_type: [open_source_project, methodology]
source_type: [GitHub, 群聊线索]
business_tags: [ITBP, 管理, 个人能力]
problem_tags: [知识沉淀, 流程提效, 用户洞察]
method_tags: [Agent, Skill, 知识库, 自动化]
tool_tags: [Claude Code, Codex, OpenClaw, Hermes, llm-wiki]
value_stage: 可小实验
risk_tags: [维护风险, 国内可用性, 数据安全]
public_level: public
---

# llm-wiki-skill：基于 Karpathy 方法论的多平台 Agent 个人知识库

## 1. 这是什么

`llm-wiki-skill` 是一个把 Karpathy 的 **LLM Wiki** 方法论落地为 Agent Skill 的开源项目。它让本地代码 Agent（Claude Code / Codex / OpenClaw / Hermes）持续维护一个结构化、可增量更新的 Markdown 知识库，而不是每次问答都重新检索原始资料。

核心思路不是"再做一遍 RAG"，而是把知识维护权交给 LLM：
- `raw/` 保留原材料；
- `wiki/` 是 LLM 编译过的结构化页面；
- schema（CLAUDE.md / AGENTS.md）规定如何 ingest / 编译 / 回答 / 维护。

代表的变化是：知识库不再依赖固定的检索管道或人工维护，而是由 Agent 持续做"增量化整理 + 交叉链接维护"。

## 2. 原始来源

- 主仓库：[sdyckjq-lab/llm-wiki-skill](https://github.com/sdyckjq-lab/llm-wiki-skill)
- 同类实现：[Astro-Han/karpathy-llm-wiki](https://github.com/Astro-Han/karpathy-llm-wiki)、[lewislulu/llm-wiki-skill](https://github.com/lewislulu/llm-wiki-skill)
- 方法论原典：[Karpathy/llm-wiki.md Gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
- 一个完整落地示例（无 API key，本地优先）：[alfadur7/llm-wiki-newsroom](https://github.com/alfadur7/llm-wiki-newsroom)
- 发现入口：打捞处群聊 2026-04-19 链接（许晶晶要求安装）

## 3. 核心观点 / 核心能力

1. **多平台覆盖**：同一份 Skill 支持 Claude Code、Codex、OpenClaw、Hermes，安装路径分别为 `~/.claude/skills/llm-wiki`、`~/.codex/skills/llm-wiki`、`~/.openclaw/skills/llm-wiki`、`~/.hermes/skills/llm-wiki`。
2. **升级入口内置**：默认安装后自带 `/llm-wiki-upgrade` 指令，可单独拉取核心主线。
3. **可选 adapter**：需要 X / 公众号 / YouTube / 知乎等抓取能力时，用 `bash install.sh --with-optional-adapters` 安装依赖（依赖 `uv`）。
4. **图谱引擎同源**：交互式图谱引擎（`packages/graph-engine/`）的离线 HTML 和工作台图谱视图是同一套引擎的两个出口，避免实现分裂。
5. **PowerShell 兼容**：提供 `install.ps1`，先解决 PS 5.1 默认 GB2312 编码导致的乱码问题，再调用 `bash install.sh`。
6. **核心失败模式是 drift**：agent 在 ingest 时未及时更新交叉引用，导致页面静默过期；这点也来自 Karpathy 原方法的总结（@alinawab 在社区里提出）。

## 4. 我学到了什么

- 个人 / 小团队知识库的真正瓶颈不是"有没有 LLM"，而是"有没有人持续维护交叉链接、确保不漂移"。把维护权交给 Agent 是结构性变化。
- Skill 化的好处是：方法论、目录约定、工作流都可以一次性 ship 到多个 Agent 运行时，避免每个工具重新造一遍知识库。
- `raw/` 不做过度清洗是核心纪律，否则后续编译页会失真；这点在群聊里被反复强调。

## 5. 它是否可信，哪些需要验证

- **可信度较高**：方法论来自 Karpathy 原 Gist，社区有多个独立实现（sdyckjq-lab、Astro-Han、lewislulu），且都已发布到 GitHub 并有提交记录。
- **需验证**：
  - 不同平台（Hermes / OpenClaw）的 Skill 加载机制是否长期兼容；
  - `--with-optional-adapters` 的可选抓取能力在国内网络环境的可用性；
  - 与本仓库已有 `llm-wiki` skill 目录、`_backup_llm-wiki_install_20260419-*` 等历史安装副本的关系，是否存在多份冲突；
  - 群聊中提到首次安装曾出现 `High Risk / 2 alerts` 的安全评估结果，需要复核供应链风险。

## 6. 对个人能力有什么价值

- 可以把自己的项目资料、外部文章、研究笔记逐步喂进同一个 wiki，由 Agent 维护结构化页面；
- 跨会话长期保留判断逻辑、避坑笔记、决策依据，而不是每次都从对话历史里挖；
- 配合笔记型 Skill 一起使用，可避免"我之前讲过这个"的反复追问。

## 7. 对企业 AI 落地有什么价值

- 团队 Wiki / 项目知识库不再依赖某个 SAAS，也不需要专门的 Knowledge Engineer；
- 适合 ITBP 把多部门（市场、销售、产品、运营）的资料按项目维度分别建库，跨库共享底层 Raw；
- 与 Agent 工作流结合后，可以做到"需求来了之后，先查自己 wiki，再决定要不要查外部"。

## 8. 可做的小实验

- 选 1 个长期维护的真实项目（已含飞书多维表格、本地文档、网页资料），按 llm-wiki 的 `raw / wiki / schema` 三层结构搭最小可用版本；
- 让 Agent 跑 3-5 次 ingest，验证交叉链接是否能自动维护；
- 故意"做错一次"（不更新交叉链接），观察 drift 出现的位置，反推 schema 是否要补约束。

## 9. 风险和边界

- **drift 风险**：agent 可能偷懒不更新反向链接；schema 需要明确约束；
- **多份安装冲突**：如群聊中提到的 `~/.agents/skills/llm-wiki` 与 `~/.hermes/skills/research/llm-wiki` 共存的情况，需要明确唯一安装位置；
- **国内可用性**：YouTube / X / 部分 RSS 抓取依赖外部网络；
- **维护风险**：仓库相对较新，长期维护活跃度需观察；
- **数据安全**：把内部资料交给本地 Agent 编译，需确保不会被自动推到云端或第三方。

## 10. 当前结论

`llm-wiki-skill` 是 Karpathy 方法论目前最成熟的 Agent Skill 化落地之一，适合作为个人 / 团队知识库基础设施候选。建议先用最小项目跑一次验证，重点观察 drift 行为与 schema 的约束强度，而不是先追求多平台覆盖。
