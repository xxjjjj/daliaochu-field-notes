---
title: "Harness Engineering：把 Agent 从模型能力升级为可控运行系统"
date: 2026-07-26
discovery_source:
  type: "抖音视频笔记转贴"
  title: "大李书房一盏灯 | 第7集：Harness Engineering"
  url: "https://v.douyin.com/P8PzPOX9Fcg/"
primary_object:
  type: "methodology / trend_signal"
  name: "Harness Engineering"
  url: "https://openai.com/index/harness-engineering"
object_type:
  - methodology
  - trend_signal
source_type:
  - 抖音
  - 二手讲解
  - 官方文章
business_tags:
  - ITBP
  - 管理
  - 产品
  - 运营
problem_tags:
  - AI落地
  - 流程提效
  - 风险控制
  - 知识沉淀
  - 组织协同
  - 系统可靠性
method_tags:
  - Agent
  - Harness Engineering
  - Context Engineering
  - Prompt Engineering
  - 自动化
  - 质量门禁
  - 人工闸门
tool_tags: []
value_stage: 可小实验
risk_tags:
  - 概念包装
  - 权限
  - 成本
  - 幻觉
  - 合规
  - 组织协同
  - 审计
public_level: sanitized
status: "已本地沉淀；已追到 OpenAI 官方文章与 Martin Fowler/SIG 解读，Mitchell Hashimoto 原帖链接待补全；未提交远程"
---

# Harness Engineering：把 Agent 从模型能力升级为可控运行系统

## 1. 这是什么

这是一条抖音视频整理出来的 AI 工程方法论：核心观点是“AI Agent 不等于 LLM”，真正决定企业级可用性的，是模型之外的一整套驾驭系统——工具权限、上下文装配、状态管理、校验循环、失败重试、人工闸门、日志追踪和持续反馈。

它本质上在提醒：企业做 AI，不要只盯模型能力和提示词，更要设计 Agent 运行的“轨道”和“刹车”。视频里用都江堰类比岷江/水利工程/李冰，是为了说明：强大但不稳定的能力，需要通过工程化分流、约束、泄洪和取水口，才能变成持续生产力。

## 2. 原始来源

- 发现入口：打捞处群内转贴的抖音视频笔记，链接为 https://v.douyin.com/P8PzPOX9Fcg/ 。
- 资料本体：Harness Engineering 是 2026 年 2 月开始在 AI coding/agent 圈快速流行的工程术语。公开文章普遍将其归因于 Mitchell Hashimoto 的个人实践与文章；随后 OpenAI 在 2026-02-11 发布官方工程文章。
- 已核验公开来源：
  - OpenAI, Ryan Lopopolo, *Harness engineering: leveraging Codex in an agent-first world*, 2026-02-11：https://openai.com/index/harness-engineering
  - Martin Fowler / Birgitta Böckeler, *Harness engineering for coding agent users*, 2026-04-02：https://martinfowler.com/articles/harness-engineering.html
  - SIG, *What is harness engineering?*：https://www.softwareimprovementgroup.com/blog/what-is-harness-engineering
- 待补全：Mitchell Hashimoto 原帖具体 URL。SIG 指向 `mitchellh.com/writing/harness-engineering`，当前直接访问返回 404，可能是路径变更或站点调整，需要继续追原文。
- 谨慎点：群内整理中的公式 “Harness = Agent - LLM” 与公开常见表达 “Agent = Model + Harness” 是等价改写；“百万行代码实验”基本对应 OpenAI Codex 内部产品实验，但要避免把它泛化成“所有企业立刻零人工写代码”。

## 3. 核心观点 / 核心能力

可以把相关概念按职责边界拆开：

1. **Prompt Engineering**：优化单次提问和单次输出，解决“这次怎么问得更准”。
2. **Context Engineering**：管理模型看到什么知识、历史、文件和外部信息，解决“模型拿什么来判断”。
3. **Harness Engineering**：设计 Agent 所处的完整运行环境，解决“它能做什么、不能做什么、错了怎么发现、如何不重复犯错、如何可审计”。

Martin Fowler 文章里的框架更适合工程落地：

- **Guides（前馈控制）**：在 Agent 行动前给它规则、目录、模板、命令、架构边界，降低第一次犯错概率。
- **Sensors（反馈控制）**：在 Agent 行动后用测试、lint、类型检查、安全扫描、人工 review、AI review 等发现问题，并让 Agent 自我修正。
- **Computational controls**：确定性、快速、低成本的校验，例如测试、lint、type check、结构约束。应尽量左移，高频运行。
- **Inferential controls**：语义判断、AI review、LLM as judge，更慢更贵，适合补充判断，不宜替代确定性门禁。

OpenAI 文章给出的实践信号更激进：人类工程师从“手写代码”转向“设计环境、澄清意图、搭建反馈环、维护架构口味”，让 Agent 在 repository knowledge、review loop、plan/log、agent legibility 等约束下持续执行。

## 4. 我学到了什么

对企业 AI 最有启发的不是“马具/都江堰”的比喻，而是工程重心的迁移：

- 以前我们担心“模型聪不聪明”；现在更要担心“系统有没有边界、校验和审计”。
- 单次提示词优化只能改善一个回合；Harness 改善的是长期运行的稳定性。
- Agent 越强，越需要 deterministic checks：权限、测试、日志、回滚、审批、结构化结果校验不能交给模型自觉。
- 错误不应只在聊天里纠正一次；如果某类错误反复出现，就应该沉淀成规则、脚本、检查器、Skill 或流程闸门，让同类错误下次结构性被拦住。
- 这和我们当前做飞书自动化、CRM AI、Skill 体系、脚本校验、人工闸门的方向是一致的：不是“让 AI 更自由”，而是“让 AI 在可验证轨道里跑得更快”。

## 5. 它是否可信，哪些需要验证

可信部分：

- “Agent = Model + Harness” 与 OpenAI、Martin Fowler、SIG 等公开材料一致。
- “人类掌舵，Agent 执行”是 OpenAI 文章的明确表达：Humans steer. Agents execute.
- “模型决定上限，Harness 决定底线”虽然是中文转述，但符合公开文章对质量门禁、反馈环、架构约束的强调。
- 对企业系统而言，工具权限、状态管理、失败重试、人工闸门、日志追踪确实是生产可用性的关键，不是新概念包装。

待验证或不宜过度泛化的部分：

- Mitchell Hashimoto 原文链接和确切表述需补全；当前主要依赖二手引用和后续文章。
- OpenAI 百万行/1500 PR 是内部工程实验，外部企业不能直接复制为“零人工写代码适合所有业务”。
- “Prompt/Context/Harness 三代范式替代”容易被讲成营销叙事；更准确说，三者是分层关系，不是后者完全替代前者。
- 都江堰类比适合传播，但不能把复杂组织治理、数据权限、合规责任简单类比成水利工程；企业系统里责任主体、数据边界和误操作成本更硬。

## 6. 对个人能力有什么价值

对 ITBP、实施、CRM/企业系统交付来说，这是一个很实用的方案评审框架：

- 看一个 AI 方案，不只问模型是谁、准确率多少，还要问：
  - 它能调用哪些工具和数据？
  - 权限最小化怎么做？
  - 失败后如何重试/回滚？
  - 哪些动作必须人工确认？
  - 输出是否被测试、规则、日志或业务校验验证？
  - 重复错误如何沉淀成规则，而不是每次人工兜底？
- 做需求沟通时，可以把“我们要一个智能 Agent”翻译成“我们要一个有边界、有校验、有审计、能复盘的业务执行系统”。
- 做内部自动化时，要把“好用”进一步拆成：可复现、可追踪、可暂停、可回滚、可解释。

## 7. 对企业 AI 落地有什么价值

第一层，业务价值：

- 销售/CRM：批量线索清洗、客户跟进建议、字段补全、报表生成等流程，如果加好 Harness，可以减少人工重复劳动，同时避免错误写入客户数据。
- 运营/市场：内容生成、活动数据汇总、渠道复盘可以自动化，但必须有品牌口径、数据权限、发布审批和结果抽检。
- 产品/交付：需求拆解、测试用例、实施文档、代码/配置变更可以由 Agent 辅助，但必须接入测试、review、环境隔离和上线闸门。
- 管理：AI 可以提供决策辅助，但不能让它在人事、资金、合同、客户承诺、权限审批上无闸门执行。

第二层，IT 能力价值：

- 推动团队从“写提示词的人”升级为“设计 Agent 运行系统的人”。
- 把项目经验沉淀成 Skills、模板、校验脚本、门禁规则、回滚流程，而不是散在个人聊天里。
- 把模型不确定性挡在确定性工程之外：能脚本/规则验证的，不要靠模型自觉。

第三层，认知价值：

- AI 工业化不是“更像人”，而是“更像可控系统”。
- 模型升级会降低部分提示词成本，但不会消灭工程约束；越高级的 Agent，越需要成熟 Harness。

## 8. 可做的小实验

可以先选一个低风险内部流程做 Harness 化试点：

1. 选一个重复任务：例如 CRM 数据巡检、周报汇总、飞书群问题分类、会议纪要行动项提取。
2. 写出前馈规则：输入格式、字段含义、禁止动作、输出模板、必须引用的来源。
3. 加三个低成本传感器：
   - 字段/JSON schema 校验；
   - 空值/异常值规则检查；
   - 结果落盘或发送前人工确认开关。
4. 记录每次失败原因，把重复错误补成规则或脚本，而不是只在对话里纠正。
5. 两周后复盘：错误率、人工干预次数、耗时、是否可把某一步从“人工确认”升级为“自动校验后执行”。

判断是否进入业务试点的标准：

- 连续多次任务结果可复现；
- 错误能被传感器拦住而不是流到业务侧；
- 有日志知道 Agent 做了什么；
- 有权限边界保证它不能越权操作；
- 有回滚或撤销方案。

## 9. 风险和边界

- **概念泡沫风险**：容易被包装成“新范式”卖咨询/工具；要回到具体控制点和业务结果。
- **权限风险**：Agent 一旦能操作系统、发消息、改 CRM、写数据库，最小权限和操作审计必须先到位。
- **幻觉/错误自动化风险**：没有测试和规则门禁时，Agent 会把错误规模化复制。
- **成本风险**：Inferential sensors（LLM judge、多轮 review）好用但贵，应优先用确定性脚本拦截低级错误。
- **组织风险**：Harness 需要业务、IT、合规、实施共同定义边界，不是 IT 单独写个提示词就能解决。
- **合规风险**：涉及客户数据、合同、价格、财务、人事、外部承诺的动作，不能因为“模型很聪明”就跳过审批。

## 10. 当前结论

这条资料值得吸收。它不是在讲一个新工具，而是在提醒企业 AI 落地的工程重心：Prompt 和 Context 解决“模型怎么说/看什么”，Harness 解决“系统能不能放心让它持续做事”。

对我们当前工作，最直接的转译是：所有长期运行的 Agent/自动化/Skill，都要逐步补齐六件事——边界规则、工具权限、确定性校验、失败重试/回滚、人工闸门、日志审计。模型可以迭代，Harness 才是生产底线。
