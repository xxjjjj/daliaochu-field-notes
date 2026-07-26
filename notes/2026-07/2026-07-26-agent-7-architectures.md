---
title: "Agent 7种架构：从入门到企业级 — AlunTalk 视频笔记"
date: 2026-07-26
discovery_source:
  type: 抖音短视频
  title: Agent的7种架构，从入门到企业级一次讲清
  url: https://v.douyin.com/r2aoAs1aj4o/
primary_object:
  type: 知识型短视频（方法论梳理）
  name: AlunTalk 阿伦 — Agent 7种架构全景归类
  url: https://v.douyin.com/r2aoAs1aj4o/
object_type: [methodology]
source_type: [抖音]
business_tags: [ITBP, 个人能力]
problem_tags: [流程提效, 知识沉淀]
method_tags: [Agent, Prompt, 自动化]
tool_tags: [LangGraph, Temporal, n8n, Claude Code, Cursor, Hermes]
value_stage: 可小实验
risk_tags: []
public_level: sanitized
---

# Agent 7种架构全景：从单Agent到企业级Workflow

## 1. 这是什么

AlunTalk（抖音博主阿伦）制作的一期 Agent 架构科普短视频。将当前主流的 Agent 架构从简单到复杂梳理为 7 种，并给出了一条清晰的演进路径。不是原创研究，而是对业界现有实践的系统化归类，适合作为入门认知框架和架构选型参考。

## 2. 原始来源

- 发现入口：抖音 AlunTalk 账号，视频发布于 2026-07-10 前后
- 资料本体：抖音短视频，约 5-8 分钟，已由群友提供详细文字总结
- 补充参考来源（本文追加）：
  - Wayland Zhang《AI Agent 架构模式与实战》开源书（CC BY-NC-SA 4.0，33章，GitHub: ai-agent-book，配套参考实现 Shannon OSS + Kocoro 运行时）
  - arXiv:2602.12430v3 "Agent Skills for Large Language Models" 综述（2026年2月，系统性论述 Skill 抽象层、安全治理、26.1%社区Skill含漏洞）
  - MLflow "Building Production-Ready AI Agents in 2026"（强调 Microservices 式多Agent架构和运行时治理）

## 3. 核心观点 / 核心能力

视频将 Agent 架构按复杂度递增分为 7 种，核心主张是：

1. **单 Agent** — 一个 LLM 包办一切，适合简单任务和快速验证（ChatGPT/Copilot 模式）
2. **ReAct** — Thought→Action→Observation 循环，适合多步骤推理，但 Token 消耗大、不稳定
3. **Plan & Execute** — 先规划后执行，稳定性高，适合长流程工程化任务，但计划错误会全局失败
4. **多 Agent** — Orchestrator 分配任务给多个专业 Agent，解耦清晰但协调成本和延迟高
5. **Router + Skill** ⭐（博主最推荐） — 不让模型"想"，让模型"选"：意图路由到预定义 Skill，稳定性极强，适合 AI Coding/技能系统
6. **Blackboard** — 多 Agent 共享状态，通过状态变化驱动执行，适合复杂协作但调试困难
7. **Graph/Workflow** — 基于 DAG 编排工作流，支持分支、并行、回溯、重试，企业级生产首选（LangGraph/Temporal/Airflow/n8n/Prefect）

演进路径推荐：单Agent → ReAct → Plan&Execute → 多Agent → Router+Skill → Blackboard → Graph/Workflow，按场景复杂度逐步升级。

## 4. 我学到了什么

- Router + Skill 之所以被认为是最佳实践，不是因为技术更"先进"，而是因为它把不确定性限制在路由这一步，后续执行是确定性的——这对企业场景的可控性和可评估性至关重要。
- 7 种架构不是替代关系，而是分层关系：生产系统往往是多种架构的组合。比如 Hermes 本身就是 Router+Skill + 单Agent（每个Skill内做实时的ReAct）。
- 学术补充：arXiv 综述指出 2025年10月 Anthropic 发布 Agent Skills → 12月开源标准化 → 4 个月内 GitHub 62K star，这说明 Skill 抽象层正在成为行业共识。但同时 26.1% 社区 Skill 含安全漏洞，治理框架是下一个必答题。
- Wayland Zhang 的书补充了一个关键视角：模式优先于框架。框架会过时，但架构模式不会。

## 5. 它是否可信，哪些需要验证

- 视频属于高质量的二手知识整理，框架本身在业界有广泛共识，可信度较高。
- 需注意：
  - "Router+Skill 是最佳实践"这个结论有场景前提（AI Coding、Copilot），并非所有场景的最优解。
  - 视频未提供每种架构的代码示例或基准对比，更多是概念级梳理。
  - 未找到 AlunTalk 本人在 B站/YouTube/GitHub 的原始身份和补充材料，抖音短链接难以直接检索。
  - Blackboard 架构在视频中归类为独立架构，但在实际生态（如 LangGraph）中更多是 Graph/Workflow 的一种实现模式，不是并列关系。

## 6. 对个人能力有什么价值

- **架构判断力**：以后看一个 Agent 产品或框架（Dify/Coze/LangGraph/CrewAI），能快速判断它属于哪种架构组合，理解其适用边界和坑。
- **选型语言**：用"Router+Skill vs 多Agent vs Graph"这套语言和开发者/架构师沟通，比说"这个好用那个不好用"专业得多。
- **直接相关**：我们的 Hermes 已经是 Router+Skill 的实践，理解这个架构的理论基础有助于后续扩展和优化。

## 7. 对企业 AI 落地有什么价值

- **销售易 CRM AI 化**：CRM 场景天然适合 Router+Skill——用户意图有限（查客户、建商机、写跟进、做报表），每个意图对应一个确定性 Skill，比让 LLM 自由发挥可控得多。
- **飞书自动化**：复杂审批流、多步骤协同流程更适合 Graph/Workflow 架构（n8n/Temporal），而不是让一个 Agent 硬扛。
- **BP 服务业务部门**：
  - 市场部：内容生成和分发走 Router+Skill（品牌一致性可控）
  - 销售部：客户洞察和跟进建议走多 Agent 协作（搜索+分析+写作）
  - 管理层：报表和决策辅助走 Plan & Execute（先确认需求范围再执行）

## 8. 可做的小实验

1. **对照诊断**：拿现有的 Hermes Skill 列表，按这 7 种架构归类，看看我们实际在用哪些、缺哪些。预计结果：Router+Skill（主体）+ 单Agent（每个Skill内）+ 少量 Graph/Workflow（自动化流水线）。
2. **Router 命中率评估**：如果 Hermes 的 Skill 路由是基于 LLM 的意图识别，可以设计一个小脚本统计路由命中率、错误路由模式和模糊意图的处理情况——这是 Router+Skill 架构的核心可观测性指标。
3. **CRM 场景的 Skill 分级设计**：把销售易 CRM 的 10 个最高频操作设计成 Skill（查询客户、创建线索、写跟进、查报表等），评估哪些适合 Router+Skill、哪些需要 Graph/Workflow。

## 9. 风险和边界

- 视频本身无风险，属于公开知识整理。
- Router+Skill 的隐藏成本：Skill 设计需要持续的领域知识维护，Skill 数量膨胀后会出现命中冲突和维护负担——这恰好是 Hermes 的 curator 机制要解决的问题。
- Graph/Workflow 架构在国产环境中的可用性：Temporal 需要自建基础设施，n8n 国内部署和飞书集成需要验证。

## 10. 当前结论

这是一期质量较高的 Agent 架构入门视频，核心价值在于**提供一个完整的分类框架**，而不是技术深度。对 ITBP 来说最大的收获是：理解了 Router+Skill 为什么是当前 AI Coding 系统的主流选择，以及何时应该从 Router+Skill 升级到 Graph/Workflow。

下一步：对照 Hermes 当前 Skill 体系做一次架构映射，验证 Router 的命中率和错误模式——这是最容易落地的实践动作。
