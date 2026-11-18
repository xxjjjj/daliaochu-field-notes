---
title: "智能负债"：Skill 会折旧——Felix Rieseberg 访谈与 Anthropic 折子代理指南的交叉核验
date: 2026-11-19
discovery_source:
  type: 抖音视频（AI 生成二次解读，页面原文）
  title: "\"智能负债\"正在出现 我们可能正在给\"超级智能\"上锁。"
  url: https://www.douyin.com/video/7699445292587846962
primary_object:
  type: 访谈 + 官方工程指南（趋势判断 + 方法论）
  name: Felix Rieseberg 访谈（Silicon Valley Girl, 2026-10-20）+ Anthropic《Building agents: When to fold subagents into your main agent loop》(2026-10-22, Diane Sarkis)
  url: https://claude.com/resources/articles/building-agents-when-to-fold-subagents-into-your-main-agent-loop
object_type: [trend_signal, methodology]
source_type: [抖音, YouTube, 官网]
business_tags: [ITBP, 管理, 个人能力]
problem_tags: [知识沉淀, 组织协同, 流程提效]
method_tags: [Agent, Skill, Prompt, 知识库]
tool_tags: [Claude Code, Claude Cowork, Claude Agent SDK]
value_stage: 学习理解
risk_tags: [幻觉, 二手演绎失真]
public_level: public
---

# "智能负债"：Skill 会折旧

## 1. 这是什么

一条抖音二创视频（作者"超级猫不白"，2026-10-22，6:53，标注"内容由 AI 生成"），把两个真实的
一手事件合成一个"AI 时代的新负债"叙事：Anthropic 工程负责人 Felix Rieseberg 的访谈
（2026-10-20）+ Anthropic Applied AI 团队 10-22 发布的"把大部分子代理折回主循环"指南。

视频本身的判断方向是对的，但有三处硬伤（见第 5 节）：人名串错、"砍 80%"的对象被偷换成 Skill、
"验证必须由人做"被讲得比原话更强。本卡片以两个一手源为准重写，视频只作为发现入口。

## 2. 原始来源

- 发现入口：抖音 https://www.douyin.com/video/7699445292587846962 （ASR 被平台限制，只有页面简介+章节文本，未看到口播画面）
- 一手源 A：Felix Rieseberg（Anthropic，Claude.ai / Claude Cowork / Claude Code Desktop 工程负责人，Electron 联合维护者）
  《Claude Engineering Lead: Why Giving AI Full Access Changes Everything》，Silicon Valley Girl，2026-10-20，49 分钟
  https://www.youtube.com/watch?v=D-SVFneJ_Nk
- 一手源 B：Diane Sarkis（Anthropic Applied AI），《Building agents: When to fold subagents into your main agent loop》，2026-10-22
  https://claude.com/resources/articles/building-agents-when-to-fold-subagents-into-your-main-agent-loop
- 旁证（多源交叉）：lenny's newsletter 播客页、TEDAI 2026 讲者页、appliedaihub 访谈摘要（含时间戳原话）、
  enjoy.dev / mindstudio / explainx 对官方指南的转述（互相独立，核心引用一致）

## 3. 核心观点（以一手源为准）

**A. Skill 有半衰期（视频称"智能负债"）。** Rieseberg 在访谈 21:40 附近：自定义 Skill 的半衰期很短，
因为很多 Skill 是在为上一代模型打补丁；**删掉过时的 Skill，输出质量经常会变好**。这是"负债"说法的原始出处。

**B. Agent 架构正在变成组织架构的复印件。** Anthropic 官方指南原话：公司按团队/问题类型各建一个
子代理，结果是"agent 架构成了组织架构的复印件（Conway's law at work，康威定律）"。同一份指南还给了
一个反常识数据：多代理 harness 与单主循环对比，有客户在后者上拿到**大约 2 倍**的运行速度。

**C. 正解不是"少用 Agent"，是渐进披露。** 单主循环 + Skill（教方法）+ tool search（按需取工具）：
模型先只看到简短描述，需要时才完整加载。Anthropic 用这套把 **Claude Code 的系统提示词砍掉 80% 以上**
（对象是系统提示词，不是 Skill 本身，靠的是按需加载而不是"删 Skill"）。官方还给了"什么时候该保留子代理"的
三类例外：单循环出现可测量的上下文膨胀、任务需要并行的一次性上下文、工具太多导致选错工具。

**D. 人不是"必须亲自验收"，而是"提供外部信号 + 定标准"。** 访谈 26:39 原话大意：当前这个 session
判断自己做得好不好是很差的；外部信号在紧急情况下可以是另一个 Claude session，但**我更想要硬的外部证据**
（真实指标、真实运行结果）。配套方法：adversarial review（拿 5–10 个 Claude 线程专门攻击草稿）+
一个 Judge Claude 过滤无效批评；以及"品味抽取"三步法（翻出你历史上给过的所有反馈 → 让 Claude 归纳成显式规则
→ 再跑一遍挖出你没写下来的隐性原则），把这套 rubric 交给独立 review session。

**E. MX（Model Experience）取代"人肉传送带"。** 原话："我就是 Claude 的传送带——去崩溃列表里捞一个，
搬到 Claude 面前说请修好它。" 改法是按 Claude 需要什么来设计：给它原始崩溃数据、能装包的机器环境、
自己判断先修哪个的权限。**人省下来的不是执行，而是搬运。**

**F. 执行变便宜后，稀缺的是取舍。** 一天能做出 1000 个可用原型，但人一天只能认真看一个；
"好产品是按钮更少的产品"。同场还提到 capability overhang：多数人仍按 2024 年的假设给 AI 派活，
不断把"你真正希望发生的那件事"往上交，直到模型撞墙为止，那里才是当前智能的前沿。

## 4. 我学到了什么

1. **要区分两种 Skill，不能all-in"折旧"论。** 一类是"补模型短板的脚手架"（教它怎么思考、怎么防幻觉、
   怎么遵守格式），模型换代就贬值，该删；另一类是**租户事实与企业口径**（公海池规则、销管路由、
   审批口径、字段含义），那不是模型的补丁，是外部世界的约束，不会因为模型变强而消失，删了就是丢资产。
   视频把这两类混成一类，是最容易照抄错的地方。
2. **"按任务切不按岗位切"有更硬的一层。** 之前记的 router/worker 结论是"Router 管协调、Worker 干活"；
   官方指南更进一步：**router、intent classifier、固定 pipeline 本身就建议折进主循环**，让写计划的模型
   自己去执行（"the model that writes the plan is also the one carrying it out"）。这和我们一直在治的
   "交接处信息丢失"是同一个病根。
3. **验收的正确姿势是"标准外置 + 独立审查"，不是"人肉逐条看"。** 人肉逐条验是新的瓶颈（也是新的传送带）。
   人的工作是产出 rubric 和硬证据，验的动作可以交给独立 session 拿 rubric 去打。
4. **"删了就更好"这句话自带前提：你得有可信的评测。** 官方同一篇里写明，这套动作能成立是因为
   Anthropic 有 trusted evals；而且他们自己在样例客服 agent 上折叠时踩过坑——变快了，但原本能过的一个
   流程两次里挂了一次。所以删/折的动作必须配"每个任务跑不止一次"的回归基线。

## 5. 它是否可信，哪些需要验证

| 视频说法 | 一手源实际说法 | 判定 |
|---|---|---|
| 人物"费利克斯·里泽贝格" | **Felix Rieseberg**，Anthropic Claude 工程负责人（曾任 Slack/Stripe/Notion，Electron 联合维护者） | 姓名错，AI 生成/ASR 串音；人物真实存在 |
| "删掉部分 Skill 后同一模型表现更好" | 有原话：Skill 半衰期短、很多是给上一代模型打补丁，删掉过时的常能提升质量 | 成立（定性，不是普适规律） |
| "Anthropic 砍了 80%" | 砍的是 **Claude Code 的系统提示词**（>80%），靠渐进披露按需加载 Skill/工具 | 对象被偷换，不能读成"删 Skill 的功劳" |
| "验证不能自证，必须由人来做" | 原话是**同一个 session 不能给自己打分**；解法首要是对抗性 review + Judge（都是 AI）+ 硬外部证据 | 方向对、落点错：人负责定标准与硬证据，不是逐条验收 |
| "未来最值得自动化的是分配本身" | 未找到对应原话；最接近的是 MX（别再当搬运工，把数据和权限给模型，让它自己决定修哪个） | 二手演绎，待核 |
| "Agent 团队制造部门墙" | 官方指南原话：agent 架构变成组织架构复印件（康威定律） | 成立 |
| "人类沦为任务传送带" | Rieseberg 自述原话"我就是 Claude 的传送带" | 成立（是他的自嘲，不是对所有人的断言）|

**还需要一手确认的**：官方指南里"2 倍运行速度""80%"这两个数字来自 Anthropic 自述的客户对比，
没有公开评测细节；"删 Skill 提升质量"在访谈里是经验判断，没有给出量化，不构成"Skill 越少越好"的结论。

## 6. 对个人能力有什么价值

- 多了一把判断尺：**拿到一个新实践先问"它是在补哪一代模型的短板"**。补短板的，多半会折旧；
  描述外部事实的，多半是资产。
- 学到一个可复用的验收结构：rubric 外置 + 独立 session 对抗 + 硬证据，比"我自己再看一遍"抗疲劳。
- "往上交一层目标直到撞墙"可以直接用在自己的工作上：不要问"能不能帮我把这个 PPT 排好"，
  问"这场汇报要拿到的决策是什么"。

## 7. 对企业 AI 落地有什么价值

- 队伍里已有的 Agent/Skill 资产，值得做一次**折旧盘点**而不是继续加防御性条款：模型换代后重跑基线，
  表现变差的删。已有的 TDD/对抗式评审笔记是同一个病根：用规则补模型的不足，模型变强后规则变负债。
- 组织侧最实用的一条：**不要按部门/岗位去切 Agent**。按岗位切 → 交接处丢信息 → 复现现实里的部门墙。
  按上下文需求、并行需求、验证需求去切。
- 业务方最容易接受的一句话版本：**AI 便宜了以后，贵的是"决定做什么"和"判断做得对不对"**，
  所以流程设计要往这两头加人，往中间减人。

## 8. 可做的小实验

1. **Skill 半衰期体检（本组 Skill 库）**：挑 3 个"补模型短板型"的 Skill，各选 5 个真实历史任务，
   跑"带 Skill / 不带 Skill"两版，同一个模型、同一份输入，各跑两次以上，只看任务级结果，不看过程观感。
   结论只有三种：留、改、删。
2. **rubric 抽取**：把最近 20 次自己给别人内容的批注/改动翻出来，让模型归纳成显式验收规则，
   再把规则交给一个不懂上下文的独立 session 去审新产出，比较"人肉审"和"独立 session 审"的漏检率。
3. **折叠一个子代理**：找一个"只做单一环节"的子流程，按官方顺序折叠——承诺输出格式 → 合并强耦合的两个 →
   把保留的当工具调用 → 一次折一个，每次折完对同一批任务跑两遍。出现"原本能过的挂了一次"就退回。

## 9. 风险和边界

- **不要把"删 Skill"做成教条。** 权限、口径、合规类 Skill 是硬约束，删掉的风险不是"模型变弱"而是
  "越权/答错业务口径"。折旧盘点只针对"补模型短板型"。
- 这套方法的前提是**有可信评测**。没有基线就删，等于把回归测试扔掉换手感。
- 单主循环不是万灵药：上下文膨胀、真并行、工具选择混乱这三类场景，子代理仍然是对的。
- 二手视频（尤其标注"AI 生成"的）人物名、数字、归因都要回一手核对；本条已出现人名串错、
  削减对象错位两处。

## 10. 当前结论

方向成立、值得吸收，但要按"两类 Skill"拆开用：**补模型短板的会折旧，描述租户事实的是资产**。
对团队最可落地的两个动作是——给 Skill 库建一个换代后重跑基线的体检机制；以及停止按岗位/部门切 Agent，
按上下文与验证需求切。视频里的"人必须逐条验收"要修正为"人定标准、独立审查、硬证据说话"。
