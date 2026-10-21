---
title: "Karpathy 近期有价值分享薅一遍：Sequoia 对谈、能力鸿沟漏斗、Agent-native 基建"
date: 2026-10-21
discovery_source:
  type: 群聊线索
  title: "晶晶要求追 Karpathy 最近有价值的分享"
  url: ""
primary_object:
  type: article
  name: "Sequoia Ascent 2026 fireside chat（Karpathy 本人 bearblog 整理稿）+ 2026-10-11 X 帖"
  url: "https://karpathy.bearblog.dev/sequoia-ascent-2026"
object_type: [trend_signal, article, methodology]
source_type: [官网, 视频, 社交媒体]
business_tags: [ITBP, 个人能力, 产品]
problem_tags: [知识沉淀, 流程提效, 用户洞察]
method_tags: [Agent, Software 3.0, Loop Engineering, 知识库]
tool_tags: [Claude Code, Codex, llm-wiki]
value_stage: 学习理解
risk_tags: [幻觉]
public_level: public
---

# Karpathy 近期有价值分享（2026 年 3-10 月）薅一遍

## 1. 这是什么

应晶晶要求，顺 AutoResearch 线索把 Karpathy 近半年多有实质内容的输出过一遍。一手来源是他自己的 bearblog（karpathy.bearblog.dev，他明确用 Codex 5.5 把视频转写整理成文字、供人和 LLM 阅读）和 X 账号；过滤掉二创"十個商業點子"类内容。

主要三件：
1. **Sequoia AI Ascent 2026 炉边对谈**（与 Stephanie Zhan，2026 年春，本人 4 月底发整理稿）：Software 3.0、agentic engineering、jagged intelligence 的系统阐述。
2. **Skill Issue 播客**（Sarah Guo，2026-03-20，百万播放）：AutoResearch 深度访谈，"不能 evaluate 就不能 autoresearch"。
3. **2026-10-11 X 帖**：重提"LLM 能力认知鸿沟正在从两群人各说各话，变成一个陡峭漏斗"，napkin math 约 60 亿人（75% 人口）几乎还没接触。

## 2. 原始来源

- 本人整理稿（最权威，含完整转写）：https://karpathy.bearblog.dev/sequoia-ascent-2026
- 视频：https://www.youtube.com/watch?v=96jN2OCOfLs
- Skill Issue 播客：https://www.youtube.com/watch?v=kwSVtQ7dziU
- Animals vs Ghosts：https://karpathy.bearblog.dev/animals-vs-ghosts/
- LLM Wiki gist：https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
- 能力鸿沟报道：Business Insider 2026-10，引其 10-11 X 帖
- 注意：流传甚广的"Karpathy 三层方法（spec/verifier/environment）"是 Austin Marchese 的二手整理命名，非 Karpathy 本人框架，引用时标注。

## 3. 核心观点（只留对我们有信息量的）

**a) 2025 年 12 月是他体感的 agentic 拐点**：编程单元从"敲一行行代码"变成"委托宏观动作"（实现这功能、重构这子系统、写测试并修到过）。他说自己"从未如此感觉落后"——连他都觉得 workflow 被重构了。Vibe coding 抬底线（谁都能做原型），agentic engineering 抬天花板（专业纪律：写 spec、审 diff、建评测 loop、管权限、隔 worktree）。

**b) 可验证性公式（对 AutoResearch 的上位概括，最实用的一条）**：
- 传统软件自动化的是你能**明确指定（specify）**的东西；
- LLM/RL 自动化的是你能**验证（verify）**的东西。
- 模型在某任务上的实际能力 ≈ 可验证性 × 训练关注度（厂商有没有在这个域大量喂数据/RL）。
- 两个轴解释"锯齿状智能"：同一个模型能解分布式系统难题又在简单事上翻车——不是通用智力平滑升降，而是训练混合物和有无 reward 信号决定的。
- 创业/落地楔子：**有价值、可验证、但还没被前沿实验室重点训练**的域。

**c) Software 3.0 与"有些 app 应该消失"**：context window 就是新程序，LLM 是解释器。MenuGen 案例：旧版是拍照→OCR→生图→前端渲染→部署→支付的一整套 app；3.0 版是把照片给多模态模型直接把菜画回原照片上，中间所有脚手架都不该存在。判断机会不要只问"什么流程能加速"，要问"什么信息转换以前根本没法编程、现在一句话就能做"。

**d) Agent-native 基建（他最大的 pet peee）**：文档还在写"请打开这个网址、点这个按钮"让他烦——用户越来越多是人的 agent 而不是人。产品需要 agent 面：Markdown 文档、CLI、API、MCP server、结构化日志、机器可读 schema、可粘贴给 agent 的安装指令、安全授权、可审计动作、headless 部署。他的检验标准："build MenuGen"一句话，agent 全程不手动点击把它部署上线。愿景：我的 agent 跟你的 agent 直接谈会议细节。

**e) "你可以外包思考，但不能外包理解"**：agent 干活后，人反而是瓶颈——你得知道要建什么、什么结果可疑、什么取舍可接受。他因此推崇 LLM 知识库（llm-wiki）：不是问答机，而是把原始资料增量编译成持久 wiki，是"在固定数据上持续生成合成视角"来增强理解。这条与我们打捞处笔记仓库和 llm-wiki 实践直接同源。

**f) Ghosts, not animals**：别把 LLM 当动物（有内驱、好奇、生存本能），它是统计模拟出来的"鬼魂"——在某些点上像极聪明的系统程序员，下一点像 10 岁小孩。正确姿态是经验主义地摸它哪里行哪里不行，既不盲信也不蔑视。

**g) 招聘变化**：算法笔试题过时，更好的面试是"用 agent 建一个像样项目、部署、加固，再派对抗 agent 去攻击它"——考的是分解任务、写 spec、审稿、保质量、做安全。掌握 agentic workflow 的人产出差距可能远不止 10 倍。

**h) 能力鸿沟漏斗（2026-10-11 最新）**：真正每天用 agent 重构工作的人还是极少数，且差距在加速拉开而非弥合。

## 4. 对我们可直接转译的判断

1. "可验证 × 训练关注"两轴可直接当我们评估任何"让 AI 干 X"需求的检查表：有没有自动判分信号？主流模型在这个具体动作上有没有被充分训练？两个都弱就别上无人 loop，回到人机协同。与本组既定口径（确定性逻辑用规则代码、模型只贴判断点；CRM 生产配置不做无人循环）完全互证。
2. Agent-native 基建是英科这类甲方可以反过来向供应商要的东西：新系统采购把"有没有 API/CLI/MCP、文档是否给 agent 可执行指令、动作是否可审计"列入评估，比听"我们也接入了大模型"实在。RPA 退场判据也多一条：系统有 agent 面就不需要界面自动化补丁。
3. llm-wiki 路径得到本人背书且我们已在做（打捞处仓库、知识云）；他强调 wiki 的价值是"重排资料制造新视角"，不是检索问答——我们评审自己的知识库时应按"能不能问出新洞察"而不是"能不能命中原文"验收。
4. "有些 app 应该消失"对内建工具的提醒：给业务方做东西时先问模型能不能直接输入媒体→输出媒体，别上来就搭表单+流程+前端（也呼应低频场景不做正式流程、重量放实施侧的口径）。
5. 团队带教：面试/考核新人可借鉴"带 agent 做项目 + 对抗审查"的思路，比考操作手册更能测出真实水平。

## 5. 可信度与边界

- 均为本人一手博客/视频/本人 X 帖，可信度高；bearblog 整理稿由 Codex 5.5 生成但本人校阅发布。
- 主观判断成分（如"12 月拐点"）是他个人体感，不是统计结论。
- "6B 人没接触"是 napkin math，引用时说明是粗略估算。
- 开源模型落后闭源 6-8 个月等说法有时效性，随版本变化。

## 6. 当前结论

Karpathy 这半年的主线非常一致：**验证器决定自动化边界，agent 成为软件的主要用户，理解成为人的稀缺价值**。AutoResearch 只是这套世界观在 ML 训练上的一个最小实证。对我们而言没有需要立刻动手的新工具，但提供了三件可长期复用的东西：两轴任务评估表、向供应商要 agent-native 接口的采购视角、知识库按"新视角"而非"命中原文"的验收标准。
