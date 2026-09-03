---
title: "EvoMap / EvoX：自进化蜂群智能体——RSI 的社会学路径"
date: 2026-09-04
discovery_source:
  type: 视频解读
  title: "EvoMap EvoX 蜂群智能体 / AI 递归自进化（RSI）深度解读视频"
  url: ""
primary_object:
  type: open_source_project
  name: "EvoMap / EvoX / evolver"
  url: "https://evomap.ai/zh"
object_type: [open_source_project, commercial_product, trend_signal]
source_type: [群聊线索, 官网, GitHub]
business_tags: [ITBP, 个人能力, 管理]
problem_tags: [流程提效, 知识沉淀, 组织协同]
method_tags: [Agent, 多智能体, 自进化, RSI, Skill]
tool_tags: [EvoMap, EvoX, evolver, GEP]
value_stage: 待验证
risk_tags: [成本, 幻觉, 合规]
public_level: public
---

# EvoMap / EvoX：自进化蜂群智能体——RSI 的社会学路径

## 1. 这是什么

视频解读 + 官网/GitHub 核实后的事实：

- **EvoMap**：2026 年初爆火的"AI 自进化基础设施"，核心是 **GEP（Genome Evolution Protocol，基因组进化协议）**——把 Agent 的成功经验封装成 **Gene（基因）/ Capsule（胶囊）/ Event（事件）**，可跨模型、跨地区共享、验证、继承。官网口径："一个 Agent 学会，百万 Agent 继承"。
- **evolver**：EvoMap 的开源自进化引擎（Node.js CLI，可作为 MCP/Skill 接入 Claude Code、Cursor、Codex、OpenCode 等），GitHub 9k stars、838 forks、145 commits、**GPL-3.0**、8 月 30 日仍在更新。
- **EvoX**：EvoMap 官方推出的"正版自进化蜂群智能体"桌面 App（macOS/Windows，Beta），主控根据任务动态生成不同人格的子节点，上下文独立、可多轮讨论、程序汇总，完整过程可观测、可审计；支持 Codex 订阅登录或接本地模型。深圳 20 人团队（95/00 后为主）。

视频把它归入 **RSI（Recursive Self-Improvement，递归自进化）** 大趋势，并列了卡帕西 AutoResearch、智谱 GLM"自己训练自己"、田渊栋 6.5 亿美元融资做 RSI 等信号（这几条视频提及、未逐一核实）。

## 2. 原始来源

- 官网：https://evomap.ai/zh ；EvoX Beta 下载页：https://evomap.ai/evox/beta
- 开源引擎：https://github.com/EvoMap/evolver （GPL-3.0，Node ≥18，含 SKILL.md，可作为 agent skill 接入）
- 官方蜂群实验博客（2026-07-25）：https://evomap.ai/zh/blog/how-ai-swarms-win-from-26-to-71-percent
- 论文/项目清单：github.com/EvoMap/awesome-agent-evolution、awesome-agent-swarm
- 注意：GitHub 上另有一个 Java/Spring Boot 的 `Leavesfly/EvoX`（同名、无关），别认错。

## 3. 核心观点 / 核心能力

官方博客的两组实验是最有信息量的部分（同一模型 Claude Haiku 4.5、同一题集 563 题）：

**实验一：组织方式决定同一个模型的成绩**
- EvoX 蜂群（任务拆成原子任务、每 Agent 独立上下文、程序按题号收结果）：**70.7%**
- Sub-Agent 模式（主 LLM 拆解→子 agent 干活→主 LLM 读 30 份报告汇总，即 Claude Code/Codex 的常见架构）：**38.5%**
- 单一上下文：**26.3%**
- 关键发现：Sub-Agent 模式中子 agent 曾做对 373 题，但经"报告→主协调 LLM 综合"的传话链后只剩 217 题——**166 个正确答案在自然语言转述中丢失，中间正确率保留率只有 55.5%**。蜂群赢在"做对的答案不再被另一个 LLM 重新理解一遍"：Agent 负责解题，程序负责汇合。

**实验二：Agent 能自己长出组织结构**
- 24 个相同配置的 Agent 做题 8 轮，靠 Gene/memory 积累自然分化出专业侧重（如 agent-8 变成"物理擅长"）。
- 断边重连实验：Agent 只能看到社交关系时，按"朋友的朋友"连成小圈子（聚类系数 0.53）；能看到专业侧重和正确率时，转为跨圈连接高分/互补者（聚类 0.28，出现枢纽节点）。
- 结论：**系统向 Agent 暴露什么信息，就奖励什么样的组织形态**。

设计哲学一句话：自由分工不等于没有规则——"谁来做、怎样组合"可以自由，任务覆盖、结果格式、错误处理必须是协议。

## 4. 我学到了什么

1. **"幕僚长 + 专业兵"架构有了量化证据**：主 LLM 当超级经理、靠自然语言汇报汇总，是当前 sub-agent 模式的最大漏点（44% 的正确答案死在汇报链上）。可靠的做法是原子任务 + 独立上下文 + **结构化接口/程序汇合**，而不是再加一个聪明的汇总模型。
2. **自进化的最小单位不是权重，是"经验资产"**：GEP 的 Gene/Capsule 本质上就是可移植、可审计、带成功/失败信号的 Skill——和我们体系里"技能 + 反馈账本 + 周五成长复盘"是同一思路，区别是它做成了跨用户网络和协议层。
3. **组织是涌现的，但信息暴露是设计的**：想让 agent 军团按能力组队而不是抱小圈子，就得让"专长、正确率"可见。这对设计飞书话题群里多机器人协作的身份/信号机制是直接参考。
4. RSI 目前有两条路径：模型层（改权重/自己训自己，田渊栋、智谱方向）和**社会学层**（不改模型，改工具、记忆、协议和组织，EvoMap 方向）。后者门槛低、人人可参与、可审计，也是企业内落地唯一现实的路径。

## 5. 它是否可信，哪些需要验证

- 开源本体真实、活跃（9k stars、8 月底仍在提交），官网博客实验方法写得克制（明确标注超时分母、未操纵位置变量、引用文献规范），可信度高于一般营销稿。
- 待验证：
  - 70.7% vs 38.5% 是官方自测，题集为可原子化的数理题；**现实中互相依赖的模糊任务（需求分析、客户沟通）能否复现存疑**，博客自己也承认实验一的完整分工是脚本保证的。
  - "累计节省 1958 亿 token"是官网计数器口径，无法独立核实。
  - evolver 为 **GPL-3.0**，企业内集成/分发有 copyleft 约束，接内部系统前要过 license。
  - EvoX App 仍是 Beta，视频和官网都承认早期 bug 多、概念复杂；任务跑在其云端（登录送 15 美元额度），企业数据出境/隐私需评估。
  - 视频中"卡帕西 AutoResearch 把 SWE-Bench Lite 测试从 2/7 推到 7/7"等说法未独立核实。

## 6. 对个人能力有什么价值

- 多 agent 编排的评估框架：以后看任何"蜂群/多智能体"产品，问三件事——任务怎么拆（原子化？）、上下文怎么隔离、结果怎么汇合（程序还是 LLM 转述）。
- evolver 可作为 MCP/skill 接到现有 Claude Code / Codex 工作流里低成本试用，亲测 Gene 沉淀机制比看文档理解得快。

## 7. 对企业 AI 落地有什么价值

- **对"驾驶舱/小军团"建设是直接参照**：Crystal 现有 Codex（计划/审查）+ Claude Code（执行）通过 Markdown 文档总线协作，本质是 sub-agent 模式。EvoX 的教训直指下一步：任务交接用结构化字段而非自由文本报告、汇合靠程序校验而非主模型"读报告"，可先在 harness bridge 的文档格式上做（固定 schema、结果位置、覆盖检查）。
- 经验资产网络思路适合内部知识体系：把成功的实施排障经验、CRM 配置套路封装成带"成功率/适用条件"信号的可继承资产，跨项目复用——我们的知识库 + Skill 体系已具雏形，差的是"成功/失败信号"和自动沉淀闭环。
- 不建议现阶段直接引 EvoX 云服务进企业场景（数据出境 + Beta）；evolver 开源自托管可作为技术预研对象。

## 8. 可做的小实验

1. 在个人 coding 工作流里把 evolver 作为 skill/MCP 接 Claude Code 跑一周，观察 Gene 沉淀质量和 token 节省口径（本地、非企业代码）。
2. 在 harness bridge 文档总线里做一次"结构化汇合"改造：子任务输出固定 JSON schema（任务 ID、状态、结果、证据），主 agent 用程序合并而非自由文本汇总，对比前后返工率。
3. 复现思路而非复现实验：给两个飞书机器人身份设计"专长标签 + 历史成功率"的可见信号，观察派活路由是否自然向能力匹配收敛。

## 9. 风险和边界

- GPL-3.0 传染性：evolver 集成进分发的内部产品需法务确认；仅 CLI/个人使用风险低。
- 云端 Beta 的数据安全：EvoX App 默认走 EvoMap 云，企业代码/客户数据不得进。
- "蜂群"成本：高并行 agent 的 token 消耗远大于单 agent，实验一里 sub-agent 模式反而是观测成本最高的；收益要按"正确送达率"算总账。
- 自进化网络的质量风险：共享 Gene 来自社区，劣质/有害经验可被继承，需要审计门（这正是 EvoMap 强调 auditable 的原因，也是我们内部体系必须保留人工 review 的理由）。

## 10. 当前结论

雷达观察 + 架构参考，暂不业务试点。EvoMap 是"社会学路径 RSI"目前最完整的公开实现：开源引擎真实活跃，蜂群实验给"程序汇合优于 LLM 汇总"提供了量化证据，与我们正在搭的多 agent 协作体系高度同构。短期价值在思想和协议设计（结构化汇合、经验资产带信号、能力可见性），工具本身以个人预研为主，企业引入受 GPL 和云数据两个约束。
