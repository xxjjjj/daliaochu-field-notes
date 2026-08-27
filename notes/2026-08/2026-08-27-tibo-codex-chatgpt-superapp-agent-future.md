---
title: "Tibo（Codex 负责人）访谈：Agent 原生形态、Codex/ChatGPT 合并与额度重置增长术"
date: 2026-08-27
discovery_source:
  type: 抖音视频（二手文字梳理）
  title: "OpenAI Codex 负责人 Tibo 播客访谈梳理"
  url: https://v.douyin.com/yI_MA_Wg3Jo/
primary_object:
  type: trend_signal
  name: "Thibault Sottiaux (Tibo) 播客访谈：Agent 未来形态与 Codex/ChatGPT 合并"
  url: https://www.youtube.com/watch?v=XpKs3ClxyAY
object_type: [trend_signal, case_or_media]
source_type: [抖音, 官网, X, 播客]
business_tags: [个人能力, ITBP, 产品]
problem_tags: [流程提效, 知识沉淀, 用户洞察]
method_tags: [Agent, 自动化, 知识库]
tool_tags: [Codex, ChatGPT, Claude Code]
value_stage: 学习理解
risk_tags: [二手转述偏差]
public_level: public
---

# Tibo（Codex 负责人）访谈：Agent 原生形态、Codex/ChatGPT 合并与额度重置增长术

## 1. 这是什么

打捞处群内的抖音视频文字梳理，内容是 OpenAI Codex 负责人 **Thibault "Tibo" Sottiaux** 的播客访谈二手总结。Tibo 因频繁在 X 上亲自为用户重置用量额度被开发者称为"重置之神"。2026 年 7 月他被任命为 OpenAI 核心产品负责人，统管 ChatGPT 和 Codex，直接向 Greg Brockman 汇报。

视频本体最可能是 Dev Interrupted #259（"Scaffolding is coping, not scaling, and other lessons from Codex with Thibault Sottiaux"），Tibo 同期还上过 Software Engineering Daily、Every.to AI&I 等播客，观点互有重叠。

## 2. 原始来源

- 发现入口：抖音短视频梳理 https://v.douyin.com/yI_MA_Wg3Jo/
- 一手核验来源：
  - WIRED 访谈《Meet the OpenAI Engineer Leading ChatGPT's Biggest Transformation Yet》
    https://www.wired.com/story/model-behavior-interview-with-openai-codex-lead-tibo-sottiaux
  - Tibo 本人 X 帖（Codex 2000 万活跃用户、banked reset）
    https://x.com/thsottiaux/status/2090766694897619318
  - Tibo X 帖（GPT-5.6 Sol 发布重置额度）https://x.com/thsottiaux/status/2075452680760443190
  - OpenAI 官方：前沿模型训练节奏与网络安全能力
    https://openai.com/index/pacing-model-development-cyber-capabilities
  - Anthropic Institute：递归自我改进（RSI）与行业暂停提议
    https://www.anthropic.com/institute/recursive-self-improvement
  - Dev Interrupted #259 播客 https://www.youtube.com/watch?v=XpKs3ClxyAY
  - 9to5Mac：ChatGPT/Codex 统一后 5 小时用量限制恢复
    https://9to5mac.com/2026/08/24/openai-restores-5-hour-codex-and-work-limits-for-chatgpt-plus-users

## 3. 核心观点

1. **Agent 的原生形态**：skill、memory、子智能体等机制都是过渡形态的"脚手架"（Tibo 名言："Scaffolding is coping, not scaling"）。未来模型会原生地深度理解用户目标、日常和团队场景，跨平台跨工具，既能响应也能主动行动。
2. **ChatGPT 与 Codex 合并为"super app"**：OpenAI 正在把 Codex 的 agent 内核改造成通用 agent 并并入 ChatGPT，不再区分"程序员专用"和"普通用户专用"；底层让 agent 写代码、调 API、浏览网页，用户只看到自然语言交互。Sora 等独立产品已关停，资源向 super app 集中。
3. **交互变革**：打字速度会成为瓶颈，语音交互走向主流；界面应自动适配用户而非反过来。
4. **额度重置是增长手段**：重置最初是 bug 补偿，官方发现能显著提升留存后制度化——里程碑庆祝重置 + banked reset（存入账户、用户自选时间使用）。Codex 已达 **2000 万周活跃用户**（Tibo 本人 X 帖确认）。
5. **前沿训练节奏与安全**：OpenAI 在 Hugging Face 事件后暂停过可执行代码/联网的前沿推理、强化学习训练暂停约两周以加固安全环境；模型速度已超过人工审查能力，仅靠测试分数不足以保障安全。
6. **递归自我改进（RSI）**：模型开始参与优化推理系统和基础设施，形成"更强模型→更高效率→更多算力→更强模型"飞轮。RSI 概念与行业暂停呼吁主要由 Anthropic 公开提出，OpenAI 在实践但官方口径更偏"按能力节奏加固安全"。
7. **普通人使用建议**：重点是清晰表达需求而非学复杂工具；多用语音、截图求助；不必过度算计成本，能力会快速普及降价；职业标签不再重要，目标拆解、流程控制、工程控制成为核心能力。

## 4. 我学到了什么

- **对我们自建 Skill/脚手架体系是直接的方向判断**：Tibo 的"scaffolding is coping"意味着当前靠 skill 文件、记忆、子 agent 拼出来的能力，会逐块被模型原生能力吸收。脚手架仍要建（今天的模型确实需要），但设计时应假设 1-2 年内被原生能力替代，保持轻量、可迁移，不要在脆弱的工程拼接上重投入。
- **Agent 能力正从程序员向全员扩散**：Codex 内核驱动 ChatGPT super app，说明"非技术用户用自然语言驱动 agent 干活"是头部厂商的主攻方向。这对我们给业务部门（市场、销售）做 AI 落地是利好：用户教育重点从"教工具"变成"教表达目标"。
- **"先小步发布、持续拿反馈"是头部厂商的明确策略**：Tibo 原话"你承受不起大张旗鼓然后做错"（you can't afford to do a big splash and be wrong）。我们内部试点也应小切口、快反馈。
- **额度/补偿机制产品化的运营案例**：从被动补偿→发现留存效果→制度化（banked reset、里程碑重置），是增长运营的真实案例，且负责人在 X 上直接面对用户抱怨、亲自操作，形成人格化社区运营。
- **能力要求的迁移得到头部从业者确认**："目标拆解、流程控制、工程控制"比"会不会某个工具"更重要——与我们已沉淀的"贵模型动脑、便宜模型动手"、harness 协作流方向一致。

## 5. 它是否可信，哪些需要验证

二手抖音梳理与一手来源交叉核验结果：

| 说法 | 核验状态 |
|---|---|
| Tibo = Codex 负责人、"重置之神" | ✅ 属实（WIRED、X 多源确认） |
| Codex 2000 万用户 | ✅ 属实，Tibo 本人 X 帖：20M active users |
| ChatGPT 与 Codex 合并为统一产品 | ✅ 属实（WIRED 详报，9to5Mac 报道已统一计费限制） |
| 重置额度提升留存 | ✅ 方向属实（重置已成例行营销动作）；"提升留存"的量化数据未公开 |
| Tibo 有独立重置权、无需市场/财务审批 | ⚠️ 二手说法，公开来源只见他亲自重置，未见审批流程细节 |
| OpenAI 暂停前沿模型训练 | ⚠️ 需修正：不是全面暂停，是 HF 事件后暂停可联网/执行代码的前沿推理、RL 训练暂停约两周加固安全环境 |
| 递归自我改进飞轮 | ✅ 行业确在讨论（Anthropic 有长文）；OpenAI 具体飞轮细节为播客口述 |
| "Boris Cheney" | ❌ 名字有误，Claude Code 负责人是 **Boris Cherny**（本组 2026-08-12 已沉淀其团队 10 条 tips 笔记） |
| 语音成为主流、看医生前先问 AI 等 | ⚠️ Tibo 个人观点/建议，非事实判断 |

## 6. 对个人能力有什么价值

- 把"清晰表达需求 + 目标拆解 + 流程控制 + 验收控制"当作核心能力练，而不是追某个工具的操作手册——头部厂商产品路线和我们的 harness 工作流都指向同一方向。
- 语音输入值得刻意多用：飞书语音、手机端语音发指令，验证"打字成为瓶颈"的判断在自己工作流里是否成立。

## 7. 对企业 AI 落地有什么价值

- 给业务部门做 AI 宣导时，"不用学复杂工具、会说清需求就行"有了 OpenAI 官方口径背书，可降低非技术同事的抵触。
- Agent 走向全员化意味着：未来选型时"是否绑定程序员场景"不再是门槛，CRM/市场/销售场景可以直接等通用 agent 能力成熟接入，不必为每个场景定制重系统。
- 安全信号：头部实验室已因模型能力（尤其网络安全能力）超过审查节奏而主动降速。我们对 agent 自主执行（尤其能联网、能执行代码、能接触业务数据的）要保持最小权限和审计，不能因为"模型更强了"就放开。

## 8. 可做的小实验

- 语音指令实验：一周内用飞书/手机语音向 agent 派发 5 个以上真实工作任务，记录语音相比打字的效率和信息损耗。
- 脚手架替代观察：每隔一个季度复盘一次现有 Hermes skills，哪些已被新模型原生能力覆盖（如格式转换、简单检索），标记可退役，验证"scaffolding is coping"在我们环境的节奏。

## 9. 风险和边界

- 本资料主体是趋势判断和个人观点，不是可直接采购/部署的工具；不要据此做平台切换决策。
- 二手短视频梳理存在事实走样（已发现人名错误、暂停训练被夸大），引用具体数字和说法时以一手来源为准。
- "不用考虑成本"是厂商视角的乐观说法；企业侧 token 成本仍需实测管控。

## 10. 当前结论

价值在**认知校准**而非工具引入：OpenAI 的产品路线（Codex 内核 → ChatGPT 通用 agent super app、脚手架过渡论、语音交互、全员化）与我们已在做的 agent 工作流方向一致，且给出了"脚手架会被原生能力吸收"的明确预期——我们自建体系保持轻量、可迁移、按季复盘退役。安全侧记住头部实验室已在为"模型超过审查节奏"降速，自主执行权限保持克制。
