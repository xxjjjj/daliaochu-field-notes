---
title: "深度而非体量：Felix Rieseberg 的四问元提示与四种 AI 野心"
date: 2026-10-26
discovery_source:
  type: 群聊截图
  title: 打捞处群内中英双语截图（无署名）
  url: ""
primary_object:
  type: article
  name: "How to be more ambitious: A tutorial for humans with models"
  url: "https://felixrieseberg.com/how-to-be-more-ambitious-a-tutorial-for-humans-with-models"
object_type: [methodology, article]
source_type: [官网, 群聊线索]
business_tags: [ITBP, 个人能力]
problem_tags: [知识沉淀, 流程提效]
method_tags: [Prompt, Agent]
tool_tags: []
value_stage: 已沉淀
risk_tags: []
public_level: public
---

# 深度而非体量：Felix Rieseberg 的四问元提示与四种 AI 野心

## 1. 这是什么

群里截图流传的一条"简单提示词"，出处是 Anthropic Claude 团队工程师 Felix Rieseberg
2026 年 10 月的文章《How to be more ambitious: A tutorial for humans with models》。
截图中的提示词只是文章"Depth（深度）"一节里的一个工具，文章本身价值更大：
它给"用 AI 要有野心"这句口号提供了可操作的分类法。

提示词原文：

> I want a deeper version of this task, not a bigger one. Before rewriting it:
> (1) say what a top professional in this field would do here that I'm not asking for;
> (2) name the rawest material they would start from instead of what has already
> been digested for me; (3) name the specific methods and tools they would use;
> (4) say what, other than you or me, would show the result is right.
> Then rewrite my prompt to ask for exactly that.

## 2. 原始来源

- 发现入口：打捞处群内手机截图（中英对照，无署名无水印），2026-10-26
- 资料本体（一手）：https://felixrieseberg.com/how-to-be-more-ambitious-a-tutorial-for-humans-with-models
  作者 Felix Rieseberg，自述"I work on Claude"，文章标注 2026 年 10 月时间语境
  （"in October 2026, I implore you to insist on outside validation"）
- 传播链：X 用户 @xerocooleth 以"Lead engineer at Claude uses this prompt"转述后病毒式扩散，
  Threads、prompt 聚合站（thesolocreators.com 等）大量搬运，截图即二手转述版本，
  原句的"rewrite my prompt"在中文圈被译成"修改我的题目"
- 衍生：有人把它做成 Claude/Agent 技能 `/better`
  （gist.github.com/joshp123，区分"任务未开始"和"工作进行中"两种触发场景）

## 3. 核心观点 / 核心能力

文章把"用 AI 的野心"分成四类，Speed 略过，重点讲后三种：

1. **Altitude（高度）**：上升抽象层，盯住任务背后的结果而不是任务本身。
   不是"修好这个 crash"，而是"让 App 保持无 crash"；不是"做这份 QBR PPT"，
   而是"设计一个比季度 PPT 更早暴露问题的机制，并用历史数据证明它有效，
   双轨跑一个季度"。前提是改造工作流——让模型能拿到崩溃报告、
   对应 git commit、各平台机器去独立复现验证。
2. **Depth（深度）**：像领域顶尖专家那样干活，四问提示词就服务于此：
   - 专家会做但我没要求的环节是什么（逼出被省略的专业步骤）
   - 从什么最原始的素材开始（拒绝被嚼过的二手摘要，要原始数据）
   - 用什么具体方法和工具（防止模型只换词汇不换方法——"act like a
     professional" 时它常常只是换了术语，markdown 表格照用）
   - 除你我之外什么能证明结果正确（外部判据，见下）
   配套手法：坚持用行业同款工具（数据库+统计模型而非表格）、
   设定"不合理"目标并坚称可解、用原始数据对抗性质验既有假设、
   让模型产出新知识（如用日志建客户模拟并找出未知的流失早期信号、
   再在未见过的数据上验证预测力）。
3. **Range（广度）**：引入跨领域视角。两个手法：
   - 把名词换成动词（"一份 deck"→"让忙碌的人在 30 分钟内相信一个因果故事
     并在不确定中做决策"），动词会自然牵出其他学科；
   - "组队"：分析师+记者+编辑+动效声音设计+做空研究员+法务会计红队。
   关键警告：是让模型**做**做空研究员做的事（研究方法），不是**扮演**角色
   （role play 只会让对话变怪，不提升产出）。

文章后半用心理学 self-efficacy（自我效能感）解释"为什么需要被教才有野心"：
人只在很窄的领域里相信自己；Dantzig 把未解难题当作业做出来、班尼斯特破四
分钟英里后 46 天内第二个人也破了——具体而难的目标、见过他人做到、
允许失败，都能提升自我效能。模型能力每几周跳一次，需要技术来敢提要求。

## 4. 我学到了什么

- **四问的真正杀伤力在第 (2) 和第 (4) 问**：要原始素材 = 治"二手综述病"；
  要外部判据 = 治"自我背书病"。作者明确说 2026 年 10 月的模型不要让它自评，
  最好是模型能直接运行的工具给即时反馈（测试、数据回测、真实用户反应）。
- 这和本组已确立的口径高度同构，不是外来新道理：
  - "一手材料优先、无证据不猜"≈ 第 (2)(4) 问；
  - "跑真实数据出初稿再 review，不要反过来开会讨论维度"≈ Depth 里
    "insist on the same instruments"；
  - 报价/问答必须多系统取数+计算+编排 ≈ Altitude（盯结果不盯交付物）；
  - 群里"说句话事就办了"零感知自动化 ≈ Altitude 改造工作流而非单任务提速。
- 容易被忽略的一点：**Altitude 往往要求先改工作流/给工具权限**，
  不是改个提示词就能升维。作者让 Claude 保 crash-free 之前，
  先铺了崩溃报告接入、版本-commit 映射、三台系统机器。
- Range 的"做角色做的事，不要演角色"是对多 Agent 组队热的清醒注脚。

## 5. 它是否可信，哪些需要验证

- 作者身份可信：Felix Rieseberg 确为 Anthropic/Claude 团队成员
  （Windows 95 网页模拟器作者，科技圈知名工程师），文章为本人官网一手发布。
- 截图无署名，属典型的"金句脱离语境"传播——只拿到提示词的人丢失了
  四分类框架和所有配套手法，这正是本文入库的理由。
- 心理学引用（Locke & Latham 目标设置研究、self-efficacy、Dantzig 轶事）
  属成熟常识级引用，未逐条核验原文。
- `/better` gist 是第三方衍生产物，未实测。

## 6. 对个人能力有什么价值

- 直接可用：把四问提示词存成自己的"任务升维器"，在重大方案/分析/选型
  任务前先跑一遍，让模型先回答四问、改写任务，再开工。
- 自检清单：下次拿到模型产出觉得"平"的时候，对照——
  它用的是专家工具还是 markdown 表格？吃的是原始数据还是我的摘要？
  有没有除它自己之外的验证？
- 对带教同理：新成员问"XX 能不能实现"时，我们已有"挖具体事"的口径；
  四问提示词可以作为让 AI 帮业务方挖需求的辅助话术（第 1 问尤其好用）。

## 7. 对企业 AI 落地有什么价值

- 给"AI 提效"叙事提供了对内培训的清晰框架：别只讲 Speed（省时间），
  真正的差异化在 Altitude/Depth/Range，且后三者都依赖数据接入和工具权限
  （MCP/连接器/可执行环境）——这论证了本组做系统打通而不是只发提示词的方向。
- 外部判据（第 4 问）应成为内部 Agent 验收标准：CRM/RPA 场景里
  判据就是租户真实数据回查、管理员镜像、日志与可复算结果，
  而不是模型自述"已完成"。与本组"做事要跑完闭环、真实输出为准"一致。
- Altitude 的前提条件值得在规划 AI 伙伴时显式列出：
  想让数字员工承担结果级目标，先把数据源、权限、验证工具铺好，
  否则只能停留在任务级。

## 8. 可做的小实验

- 在一次真实的选型调研或方案任务中，先贴四问提示词让模型改写原始 brief，
  对比改写前后的产出深度（主观评分即可，样本 3-5 次）。
- 观察第 (3) 问在销售易知识库里的效果：问"该领域专家用什么工具"时
  模型是否会编造不存在的功能（幻觉风险点，正好验证第 4 问的必要性）。
- 暂不建议把 `/better` 装成正式技能：本组已有审题/挖根问的既定口径，
  重复建设前先在日常对话里自然试用四问。

## 9. 风险和边界

- 提示词是通用方法论，不涉及数据安全；但第 (2) 问"给我最原始的素材"
  在企业场景意味着把真实业务数据喂给模型，须遵守租户数据边界，
  外部模型不得喂客户数据。
- "设定不合理目标并坚称可解"在事实型/合规型任务里有害
  （会诱导模型硬编可行性），只适合工程攻坚类任务。
- 文章是单一作者经验贴，非实证研究；四分类是好的心智模型，不是验证过的理论。

## 10. 当前结论

已追到一手本体，价值超出截图金句：四问提示词可直接收为个人任务升维工具，
四种野心（尤其 Altitude 的工作流前提、Depth 的外部判据、Range 的"做不演"）
与本组既有实践口径互相印证，可作为内部分享"怎么把 AI 用深"的素材。
暂无必要做成正式技能，先在真实任务里试用。
