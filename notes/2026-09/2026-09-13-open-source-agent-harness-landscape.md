---
title: 开源生产级 Agent Harness 系统地图——没有全家满分，只有平台与组装两条路
date: 2026-09-13
discovery_source:
  type: 群内追问（晶晶在 Harness 七层文档基础上追问有无生产级开源系统）
  title: 生产级 Agent Harness 有没有开源实现
  url: ""
primary_object:
  type: commercial_product
  name: Coze Studio / Dify / Temporal / Langfuse / NeMo Guardrails 等开源 agent 栈
  url: ""
object_type: [trend_signal, commercial_product, open_source_project]
source_type: [web检索, 行业工程实践]
business_tags: [ITBP, 产品, 运营]
problem_tags: [流程提效, 组织协同, 知识沉淀]
method_tags: [Agent, Harness工程, AgentOps, Guardrail, 私有化部署]
tool_tags: [Coze Studio, Dify, Temporal, LangGraph, Langfuse, NeMo Guardrails, E2B, OpenHands, Letta]
value_stage: 学习理解
risk_tags: [供应商锁定, 成本, 数据安全, 国内可用性]
public_level: public
---

# 开源生产级 Agent Harness 系统地图（2026-09 检索）

## 1. 这是什么

在"七层管控架构"笔记基础上的追问：行业里有没有生产级、开源、真正起到 harness
作用的系统？结论：**没有任何一个开源项目把七层全部做满且通用**。现实是两条路：
全家桶平台（覆盖六七成，开箱即用、可私有化）和按层组装（每层都有生产级单点开源件）。

## 2. 路线一：全家桶平台

| 系统 | 许可 | 强项 | 七层短板 |
|---|---|---|---|
| Coze Studio 开源版（字节，2025 开源） | Apache 2.0 | 完成度最高的全家桶：编排/插件网关/知识库/评测/可观测，可完全私有化 | 沙箱强隔离、声明式硬护栏偏弱 |
| Dify（130k+ stars） | Apache 2.0+少量品牌限制 | 自托管最成熟，workflow+RAG+监控一体，100+ 模型接入 | 企业自评短板即"治理层、规模化管控"（第4/7层） |
| Bisheng 毕昇 | Apache 2.0 | 国内团队，企业级定位，私有化/中文友好 | 生态规模小于 Dify |
| n8n | fair-code（非真开源） | 400+ 集成的自动化引擎 | 本质是 workflow，不是 agent harness |
| FastGPT / RAGFlow / Flowise | 开源 | 知识库/RAG、可视化 | 通用 agent 管控更弱 |

## 3. 路线二：按层组装（生产真实栈）

1. **状态机/编排**：Temporal（最硬、自托管、事件溯源重放）；DBOS（库形态，
   checkpoint 进现有 Postgres，无编排服务器）；Restate/Hatchet/Inngest；
   LangGraph（checkpointer 断点续跑，Platform 商业部分与 MIT 框架分开）。
   关键常识：durable execution 解决崩溃续跑，但不自动解决幂等
   （"已扣款但上报超时"仍需自己处理）。
2. **工具网关**：MCP 生态 + Kong MCP Gateway（统一 schema/鉴权/限流）；
   框架钩子：OpenAI Agents SDK、Google ADK、Agno（前 Phidata）、CrewAI、AutoGen。
3. **上下文/记忆**：Letta（前 MemGPT）、Mem0、Zep/Graphiti；
   存储 pgvector / Milvus / Qdrant。
4. **护栏**：NVIDIA NeMo Guardrails（Apache 2.0，声明式硬规则、独立于 LLM 执行，
   最贴合"硬规则优先"）、Guardrails AI、Llama Guard（模型型审查）。
5. **沙箱**：E2B（核心开源+商业云）、Daytona（注意许可证细节）、
   OpenHands Docker runtime；强隔离 gVisor / Firecracker。
6. **可观测**：Langfuse（MIT、自托管、采用最广，2026 被 ClickHouse 收购，走向待观察）、
   Arize Phoenix、OpenLLMetry/Helicone；统一走 OpenTelemetry。
7. **评测回归**：Promptfoo（MIT）、DeepEval、Ragas。全家桶这层普遍最弱，基本要外接。

## 4. 参照系：OpenHands

OpenHands（前 OpenDevin，MIT，有 arXiv 论文 2407.16741）把 agent loop +
Docker 沙箱 + eval harness 成套做出来，2025 底推出可组合 SDK（agent/沙箱/
接口三层可替换），是 SWE-Bench/GAIA 参考平台。但面向 coding agent，
适合当**架构教科书**，不适合直接搬作通用业务 harness。

## 5. 对本组/Hermes 的对照

Hermes 自身即自建 harness：工具注册表（第2层）、危险动作人工确认（第4层部分）、
cron/会话持久化（第1层）、运行日志（第6层部分）。明显缺口：
- 第 7 层：无声明式评测回归（改 prompt/skill 后无自动回归闸门）；
- 第 4 层：护栏主要靠系统提示+确认，缺独立于模型的声明式策略引擎
  （NeMo 范式）；
- 第 3 层：记忆/上下文裁剪是工程化重点而非产品化组件。
对照学习路径：先看 Coze Studio 开源版（完整产品形态）→ 再看
Temporal + Langfuse + NeMo 三件套补编排/可观测/硬护栏认知。

## 6. 待验证

- Coze Studio 开源版的实际功能边界（与 SaaS 版差异）、二次开发许可证细节、
  企业内私有化部署资源需求——未实测。
- Langfuse 被 ClickHouse 收购后的开源治理走向。
- Daytona 等沙箱项目许可证与商用边界，部署前需逐个核 LICENSE。
- 全部栈均未在本机/内网实测，本卡是选型地图不是落地结论。

## 7. 当前结论

企业内选型：追求快速私有化见效→Coze Studio 开源版/Dify/Bisheng 试点；
追求高风险业务动作（报价、下单、数据导出）可控→必须按层组装，
护栏和评测不能信全家桶附赠件。
