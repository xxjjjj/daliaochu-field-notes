# AnySearch：给 AI Agent 用的统一搜索基础设施

- 打捞日期：2026-09-16
- 来源：群内线索（许晶晶）→ 官网 / GitHub / 百度百科 / SkillHub / 博客园新闻
- public_level: public
- value_stage: 已追源（官网+GitHub 本体已核），实测待做

## 一句话

AnySearch 不是给人用的搜索引擎，而是给 AI Agent 调用的"统一搜索 API"：一次接入，把通用网页搜索 + 20 多个垂直领域数据源（金融、法律、学术、安全、代码、企业工商、能源、医学、知识产权等）+ 网页正文提取，打包成结构化结果返回，省掉 Agent 自己解析一堆 HTML 和对接几十个接口的成本。

## 本体信息（一手）

- 官网：https://anysearch.com/home（英文站，产品定位 "AI Search Infrastructure for Agents"）
- GitHub：https://github.com/anysearch-ai/anysearch-mcp-server（1.8k stars / 182 forks / 仅 16 commits，Apache-2.0）
- 主体：厦门苏哒智能科技有限公司（软件著作权 2024-05 登记），自称"应用型 AI 实验室"
- 当前版本 v2.1.0，分发渠道：GitHub、skills.sh、ClawHub、SkillHub（腾讯）、Glama、SkillsLLM
- 免费额度：宣称每日 1000 次；GitHub 文档写匿名可直接用（低限流），API key 免费注册（只填邮箱、无验证码，Agent 可一步代注册）

### 技术形态（4 个工具）

| 工具 | 作用 |
| --- | --- |
| `search` | 自然语言搜索，可指定 domain / sub_domain（垂直源必须先查目录取合法枚举，禁止瞎编参数） |
| `get_sub_domains` | 垂直领域目录，返回子域和参数 schema |
| `batch_search` | 一次并行 1–5 条独立查询，单条失败不阻塞 |
| `extract` | URL 抓全文转 Markdown，截断 5 万字符，仅 HTML |

- 单一 JSON-RPC 2.0 端点 `https://api.anysearch.com/mcp`，原生 Streamable HTTP（MCP 2025-03-26），SSE/stdio 走 mcp-remote / supergateway 代理
- 三种接入：MCP（Claude Desktop / Cursor / OpenCode / Cline / VS Code Copilot）、Skill 插件（Python/Node）、REST API
- 架构自述："通用索引补长尾 + 高价值垂直领域自建深度索引"的联邦多源搜索

## 主张 vs 证据（按审题口径分列）

1. **"高价值信息大多不公开，在登录后的专业系统里"**——是行业共识方向（类似 Firecrawl/Exa/Tavily 都在讲 agent-native search），但"绝大多数高价值信息不可搜"是厂商立场表达，无独立证据。
2. **基准优于 Parallel 和 Brave**：Frames / FreshQA / WebwalkerQA 共 300 题，同一 LLM 只换搜索接口，综合准确率 76.4%，端到端延迟 47.8 秒。来源是百度百科与官网自述，**未见论文、第三方复测或评测方法/数据集切分细节，按厂商自报对待**。
3. **"匿名、零追踪、零遥测、零留存"**：官网安全页承诺；GitHub 仓库实质只有 README+配置（16 commits、无服务端代码，闭源 SaaS），承诺无法从代码验证。

## 对我们的意义（B 主线·市场部雷达 + 本组工具判断）

- 它解决的痛点是真实的：我们现有 web_search/web_extract 已经覆盖"通用搜索+正文提取"，AnySearch 的差异点只在**垂直数据源聚合**（金融/法律/学术/企业工商等）和给 Agent 的结构化干净输出（省 token、少 HTML 清洗）。
- 国内主体 + 中文 Skill 生态分发（腾讯 SkillHub、ClawHub），对中文 Agent 用户是加分项；但垂直源是否覆盖国内权威数据（如国内工商、裁判文书、A 股）需实测，宣传材料主要举英文场景。
- 成本模式：免费额度大（宣称 1000 次/日），适合先白嫖实测；商业模式和超额价格未公开，存在后期收费/SLA 不确定风险。
- 与本组已有能力重叠度高：在 Hermes 里接它就是一个远程 MCP 的配置量，不是新能力建设。

## 风险

- 闭源 SaaS，搜索查询全部经过其服务器；企业客户/内部业务数据类查询**不能走**，只适合公开信息调研。
- 冷启动项目：GitHub 仅 16 commits、2026-05 才在海外圈传播，主体是国内小公司，持续性与数据合规（跨境）存疑。
- Skill 包在多个第三方市场转载，安装时应只认官方 GitHub/官网端点，避免装到被改过的包。

## 三层定位对比（2026-09-16 追问补充）

| | Google 搜索（Custom Search / SERP API） | Tavily | AnySearch |
| --- | --- | --- | --- |
| 为谁设计 | 人（SERP 是给人看的页面/结构化广告位） | AI Agent | AI Agent |
| 数据面 | Google 公开网页索引，返回链接+摘要 | 开放网页聚合（自研爬虫+三方源），重排后返回干净结果，可选直接生成带引用答案 | 开放网页 + 23 个垂直域（金融/法律/学术/安全/代码/工商等），宣称接登录后专业数据源 |
| 返回物 | 原始 SERP 数据，Agent 自己洗 | 干净内容片段、extract 全文、crawl 整站、research 多跳综合 | 结构化 markdown + 垂直源结构化字段 + extract |
| 生态 | 老牌、稳定、合规 | LangChain 默认搜索，MCP/框架集成最成熟，2026 年 agent 搜索事实标准之一 | 新玩家（2026-05 起量），铺 Skill 市场 |
| 价格（2026 公开资料） | Custom Search 100 次/日免费，之后约 $5/1K | 免费 1000 credits/月，约 $8/1K 次；extract/advanced 更贵 | 宣称 1000 次/日免费、可匿名；超额价格未公开 |
| 可信度 | Google 官方服务 | 美国公司，独立基准多（Brave 官方基准中 Tavily 与第一梯队差约 1 分） | 厦门小公司，基准全为自测、无第三方复测 |

一句话区分：**Google 给的是"网页地址清单"，Tavily 给的是"开放网页上洗干净的内容和答案"，AnySearch 想多给一层"Tavily 没有的垂直专业数据库"。** 前两层 Tavily 与 AnySearch 是同类，AnySearch 的全部差异化赌注都在垂直源的覆盖和质量上——而这一点恰恰还没被独立验证，也是实测时唯一值得看的东西。

## 下一步（待晶晶决定，未执行）

1. 花 10 分钟实测：匿名调 `get_sub_domains` 拉全部 23 个垂直域清单，看国内源覆盖；用 2–3 个我们真实的调研问题（如某外贸客户背调、某医疗器械法规检索）对比现有 web_search 结果质量。
2. 若实测有增量价值，以 remote MCP 方式加到 Hermes（只读、公开查询场景），不写任何内部数据。
3. 不建议在市场部对外方案里引用其基准数字，除非有第三方复测。
