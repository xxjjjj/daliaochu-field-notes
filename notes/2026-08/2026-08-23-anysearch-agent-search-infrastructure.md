---
title: "AnySearch：为 AI Agent 而生的搜索基础设施"
date: 2026-08-23
discovery_source:
  type: video_summary
  title: 打捞处群内视频内容总结（AnySearch 介绍）
  url: ""
primary_object:
  type: commercial_product
  name: AnySearch
  url: https://anysearch.com/home
object_type: [commercial_product, open_source_project, trend_signal]
source_type: [官网, GitHub, Product Hunt, 媒体报道]
business_tags: [ITBP, 产品, 个人能力]
problem_tags: [知识沉淀, 流程提效]
method_tags: [Agent, MCP, 自动化, 知识库, RAG]
tool_tags: [AnySearch, MCP, Skill, API, 搜索]
value_stage: 待验证
risk_tags: [数据安全, 成本, 合规, 幻觉]
public_level: public
---

# AnySearch：为 AI Agent 而生的搜索基础设施

## 1. 这是什么

AnySearch 是一款面向 AI Agent（而非人类用户）的搜索基础设施产品，由中国团队（4 位 AI 开发者）研发，2026 年 5 月上线，7 月 6 日登顶 Product Hunt 当日/当周榜首（月度 #5）。

它不做"又一个 AI 搜索框"，而是给 Agent 提供一个统一的结构化信息入口：自动识别查询意图 → 路由到合适的通用或垂直数据源 → 并行检索 → 清洗去重 → 返回带来源标注的 Markdown/JSON 结果。Agent 不需要自己翻网页、清广告、去重，直接拿结果做推理。

三种接入方式：Skill（复制 prompt 自动安装）、MCP Server（开源，Apache-2.0，1.8k stars）、API（JSON-RPC 2.0，无需安装 MCP server）。每日免费 1000 次调用，匿名也能用但限额更低。

## 2. 原始来源

- 发现入口：打捞处群内视频总结（2026-08-23）
- 官网：https://anysearch.com/home
- Product Hunt：https://www.producthunt.com/products/anysearch
- GitHub MCP Server：https://github.com/anysearch-ai/anysearch-mcp-server （Apache-2.0，1.8k stars，182 forks）
- GitHub Skill：https://github.com/anysearch-ai/anysearch-skill
- 36氪报道：https://m.36kr.com/p/3817351306101896
- 博客园/OSChina 发布稿：https://news.cnblogs.com/n/822187
- 知乎独家：https://zhuanlan.zhihu.com/p/2048854504293000482 （称"上线一个月吸引10万开发者"，待独立验证）
- 机器之心实测视频：https://www.youtube.com/watch?v=Ad8KVWKe7Lo
- 已上架生态：GitHub、skills.sh、ClawHub、SkillHub、Glama

## 3. 核心观点 / 核心能力

1. **判断**：AI Agent 需要的高价值信息大部分不在公开可搜索网页里，而在登录后的专业数据库、金融终端、代码仓库、学术平台和结构化 API 中。传统搜索引擎（Google/Brave）给 Agent 的是 URL 列表，Agent 还得自己抓页面、清噪音。
2. **多源并行检索**：通用搜索 + 23 个垂直领域（金融、法律、学术、代码、网络安全、能源、企业工商等），一次请求可并行批量查询。
3. **智能意图路由**：分层路由（Hierarchical Routing）+ 弹性编排（Elastic Orchestration），自动识别查询领域并分发到对应数据源，减少无效检索。
4. **结构化交付**：返回 Entity-Enriched Markdown（核心事实、引用、元数据分离），单条结果 500–2000 tokens，5 条结果约 5000 tokens；同时提供 JSON API 供严格解析。
5. **性能**：官方自称 p50 ~1s，p95 < 5s（Maker Grant Han 在 PH 评论区口径）；在 Frames、FreshQA、WebWalkerQA 三个基准上准确率和延迟均优于 Parallel 和 Brave Search（自测数据，待第三方复现）。
6. **隐私设计**：匿名可用、无追踪、零遥测、零保留执行（zero-retention execution）、零知识凭据，API 请求加密传输，查询内容处理后即弃。
7. **跨域重排（Cross-Domain Reranking）**：不同来源结果统一质量打分后重排，避免单源偏差。

## 4. 我学到了什么

- "搜索 for Agent"正在成为一个独立品类，和"搜索 for human"的设计目标完全不同：人要链接列表自己判断，Agent 要干净、结构化、可直接喂给模型的上下文。这和我们打捞处自己的研究 LOOP 是同一个问题——我们现在用 `web_search` + `web_extract` 组合，本质上也是在手动做这件事。
- 输出格式设计是关键差异点：Entity-Enriched Markdown 把事实/引用/元数据分开，能显著降低下游 LLM 的认知负担和 token 浪费。这个思路值得我们在自己的笔记卡片和研究输出里借鉴。
- Skill/MCP/API 三栖接入是当前 Agent 工具的标准发行姿势，一个产品同时覆盖 Claude Code、Codex、Hermes、OpenCode 等生态。
- 中国团队做海外开发者基础设施产品，且能拿 PH 周冠，是一个值得关注的信号——说明 Agent 基础设施层仍有新品类窗口，不完全被大厂锁死。

## 5. 它是否可信，哪些需要验证

**可信度较高的部分：**
- 产品真实存在、官网/GitHub/PH 页面均可访问，MCP server 开源可审计。
- Apache-2.0 协议，核心接入层代码公开，不存在黑盒 MCP server 的安全风险。
- 隐私承诺（零遥测、零保留）写在官网和 SECURITY.md 里。

**需要验证的部分：**
- **基准测试为自测**：Frames/FreshQA/WebWalkerQA 上优于 Parallel/Brave 的数据来自官方，未见第三方独立复现。p50/p95 延迟也是 Maker 口头口径。
- **"10 万开发者"存疑**：知乎报道称上线一个月 10 万开发者，但 PH 只有 1 条用户评论（4.0 分）、1.9k followers，GitHub 1.8k stars——开发者注册数和实际活跃数可能差距较大，营销口径偏乐观。
- **垂直数据源具体清单未完全公开**：声称覆盖金融/法律/学术等 23 域，但具体接入了哪些数据库、是否有合规授权、中文数据源覆盖深度如何，文档里没有完整披露。
- **团队背景信息有限**：4 位 AI 开发者的具体身份、融资情况、公司主体未见公开资料，长期维护和商业化可持续性待观察。
- **免费额度可持续性**：1000 次/天免费很慷慨，但企业版定价、SLA、数据处理协议（DPA）均未公开。

## 6. 对个人能力有什么价值

- **可直接试用的研究加速器**：如果垂直数据源质量确实好，做行业调研、竞品分析、学术检索时可以减少在多个平台间跳转的时间。36氪报道里的例子（一条指令同时查 OpenAI 估值/定价/App Store 差评/Reddit 口碑）正是我们打捞处研究 LOOP 常做的事。
- **学习 Agent-native 工具设计**：AnySearch 的"意图路由 → 并行检索 → 结构化 Markdown 交付"链路，是一个很好的 Agent 工具设计参考案例，可以反推我们自己的工具（飞书消息解析、CRM 检索、笔记入库）应该怎么设计输出格式。
- **MCP 生态观察样本**：它同时上架 5 个 Skill/MCP 市场，是观察当前 Agent 工具分发渠道和获客方式的好样本。

## 7. 对企业 AI 落地有什么价值

- **内部 Agent 的搜索层选型参考**：INTCO 正在推 CRM AI 化和各类企业 Agent，搜索/信息获取层是共性需求。AnySearch 代表了一种"买现成搜索基础设施"的路线，对比"自建 RAG + 搜索引擎 API"路线，可以作为选型参照。
- **垂直行业场景适配**：金融、法律、企业工商等垂直数据源如果覆盖扎实，对市场调研、竞品监控、供应商尽调、合规排查等场景有直接价值。但需要验证中文/国内数据源的覆盖度——很多海外垂直库对中国企业数据覆盖薄弱。
- **不能直接用于敏感数据**：虽然官方承诺零保留，但查询内容仍需经过 AnySearch 服务器。涉及客户数据、内部业务、未公开财务信息的查询不应走第三方搜索 API，这是红线。

## 8. 可做的小实验

1. **A/B 对比实验**：拿 3 个真实的打捞处研究问题（例如"某竞品 AI 产品最新融资和口碑"、"某技术论文的工程实现"、"某行业政策变化"），分别用当前 Hermes `web_search`+`web_extract` 和 AnySearch MCP 跑一遍，对比：结果完整性、来源质量、token 消耗、端到端耗时、结构化程度。
2. **垂直域探测**：测试 AnySearch 的金融/学术/代码垂直域在中文 query 下的表现，看是否真的比通用搜索深。
3. **MCP 接入验证**：在 Hermes 里配置 AnySearch MCP server（本地 stdio 或远程 HTTP），跑通一个完整的 agentic research loop，评估安装复杂度和稳定性。
4. **输出格式借鉴**：把 AnySearch 的 Entity-Enriched Markdown 格式作为参考，审视我们自己 `salvage-card.md` 的字段结构是否可以进一步优化"事实/引用/元数据分离"。

实验优先级：先做 1（A/B 对比），用真实任务判断是否值得引入；不急着全量切换。

## 9. 风险和边界

- **数据安全**：查询经第三方服务器处理，尽管有零保留承诺，但法律管辖、数据跨境、 subpoenas 应对等未见详细说明。企业敏感查询禁用。
- **国内可用性**：产品面向海外开发者，官网/API 域名在国内访问稳定性待验证；MCP/Skill 生态（ClawHub、Glama 等）部分需要科学上网。
- **可持续性**：初创团队、免费额度高、未见明确商业模式，存在涨价、缩减免费额度甚至停止服务的风险。不能作为企业唯一搜索依赖。
- **垂直数据合规性**：声称接入登录后专业数据库，但具体授权关系不透明。如果数据来源存在灰色地带，企业使用有法律风险。
- **自测基准偏差**：官方 benchmark 不能直接采信，需自己跑真实任务评估。
- **"结构化"不等于"正确"**：结构化输出降低了 token 浪费，但如果底层数据源质量差或路由错误，Agent 会更高效地得到错误结论——垃圾进、结构化垃圾出。

## 10. 当前结论

AnySearch 是 Agent 搜索基础设施赛道里一个完成度较高、值得跟踪的中国团队产品。它的核心洞察（Agent 需要的是结构化信息而非链接列表）和设计（意图路由 + 并行检索 + Entity-Enriched Markdown）方向正确，开源 MCP server 降低了接入门槛和审计顾虑。

当前判断：**雷达观察，值得做小实验验证，但暂不建议作为企业级搜索层依赖。** 先用真实研究任务做 A/B 对比，如果在中文垂直场景和结果质量上确实优于现有方案，再考虑在非敏感场景试点。同时把它的输出格式设计作为我们自己 Agent 工具优化的参考。

标签：B 主线（市场部 BP 雷达——AI 工具/基础设施广度扫描），非待试点项目，不挂靠具体业务线。
