---
title: OpenSkills 生态与 Colleague.skill（titanwings）：把同事"蒸馏"成 AI Skill
date: 2026-06-02
discovery_source:
  type: 群聊讨论
  title: 数字分身 / 同事 Skill / IM 里的 AI 同事
  url:
primary_object:
  type: open_source_ecosystem
  name: OpenSkills + Colleague.skill
  url: https://openskills.cc/skills/titanwings-colleague-skill
object_type: [open_source_project, skill_marketplace]
source_type: [GitHub, openskills.cc, 群聊线索]
business_tags: [ITBP, 个人能力, 知识沉淀, 组织协同]
problem_tags: [知识沉淀, 用户洞察, 流程提效, 数字分身, 团队延续]
method_tags: [Skill, Agent, Persona, 蒸馏, IM, Memory]
tool_tags: [OpenSkills, Colleague.skill, Feishu, DingTalk, Slack, WeChat]
value_stage: 学习理解
risk_tags: [隐私, 合规, 国内可用性, 边界, 版权, 误判]
public_level: public
---

# OpenSkills 生态与 Colleague.skill（titanwings）：把同事"蒸馏"成 AI Skill

## 1. 这是什么

"数字分身 / 同事 Skill / IM 里的 AI 同事"不是一个空白赛道——已经有 OpenSkills 这类 Skill 市场 + Colleague.skill 这类"把同事蒸馏成 AI Skill"的工具在跑。本卡聚焦两件事：

1. **OpenSkills**（[openskills.cc](https://openskills.cc)）：一个跨作者的 Agent Skill 市场 + 路由，覆盖 PDF 处理、Colleague、curator-skill 等多个垂直 Skill。
2. **Colleague.skill / Create-Colleague**（`titanwings/colleague-skill`，上架在 openskills.cc）：定位"Distill a colleague into an AI Skill"，自动从飞书 / 钉钉收集数据，生成 Work Skill（工作能力）+ Persona（沟通风格），并支持持续进化。

## 2. 原始来源

- OpenSkills 市场：[openskills.cc](https://openskills.cc)、[openskills.cc/skills](https://openskills.cc/skills?sort=latest)
- Colleague.skill 页面：[openskills.cc/skills/titanwings-colleague-skill](https://openskills.cc/skills/titanwings-colleague-skill)
- 镜像 / 安装源：[github.com/titanwings/colleague-skill](https://github.com/titanwings/colleague-skill)
- 衍生介绍：[openagentskill.com/skills/colleague-skill](https://www.openagentskill.com/skills/colleague-skill)
- 发现入口：打捞处群聊 2026-06-02，用户调研"数字分身 / 同事 Skill / IM 里的 AI 同事"赛道时搜出

## 3. 核心观点 / 核心能力

### 3.1 OpenSkills 市场

- Skill 即"文件夹 + SKILL.md（YAML frontmatter + Markdown body）"，符合 Anthropic / Claude / Codex 的 skill 规范。
- 跨平台：理论上 Claude Code / Codex / 其他支持 Skill 的 Agent 都能消费。
- 内置路由：例如 `curator-skill` 是"跨作者 persona skill 路由"，能识别"Naval / Munger / Musk / Jobs / Feynman"等真实人设的 persona skill 并按需推荐。

### 3.2 Colleague.skill

- **核心流程**：从飞书 / 钉钉（也支持 Slack / 微信 / 邮件）抓取指定同事的聊天记录 + 文档，生成两个维度的 Skill：
  - **Work Skill**：技术能力、工作流程、输出标准；
  - **Persona**：沟通风格、决策模式、人际偏好。
- **典型场景**：
  - 新人 onboarding：新人直接调用 senior 的 Skill 学代码风格、CR 偏好、技术决策逻辑；
  - 离职交接：把即将离职同事的飞书 / 钉钉聊天 + 文档转成可交互 Skill，保留关键知识；
  - 跨时区协作：在对方时区用 ta 的 Style 反馈（"看起来像 ta 本人在回复"）。
- **持续进化**：Skill 不是一次性产物，可以根据新数据持续更新。
- **安装方式**：`npx skills add https://github.com/titanwings/colleague-skill`。

## 4. 我学到了什么

- **数字分身不是空白赛道**：OpenSkills + Colleague.skill 已经把"同事 → AI Skill"做成可用产品形态，比"自己造轮子"省半年时间。
- **真正的差异化不在"蒸馏同事"本身**：而在于"本人授权装载 + @触发 + 上下文继承 + 边界控制 + 活人兜底 + 持续学习"这套闭环。光把数据灌进去不算数字分身，能不能在 IM 里被自然触发、能不能识别边界（如"这条不能替我回"）、能不能让真人兜底才是难点。
- **Skill 即"轻量插件"**：一个 Skill 就是一个文件夹 + SKILL.md，能跨平台被多个 Agent 复用——这个抽象比"为每个场景写一个 bot"健康得多。
- **Persona ≠ 简单 prompt 模仿**：Colleague.skill 把 Persona 拆成"沟通风格 / 决策模式 / 人际偏好"三个独立维度，比"你说话像某某"这种 prompt trick 严肃得多。

## 5. 它是否可信，哪些需要验证

- **可信度**：openskills.cc 市场已上线多个 Skill，Colleague.skill 给出明确安装命令与 GitHub 仓库；openagentskill.com 也有镜像介绍。
- **需要验证**：
  - 实际数据抓取范围与精度：从飞书 / 钉钉抓个人聊天涉及重大权限与合规问题，工具是否需要管理员授权？是否会被平台风控？
  - 生成 Skill 的质量：Work Skill / Persona 两个维度如何保证不被"性格化"覆盖？测评机制是什么？
  - 国内合规：把同事的聊天记录（含客户对话 / 内部讨论）转成 AI Skill 是否符合个保法 / 公司合规？需法务介入。
  - 持续进化的触发机制：是每次新数据自动覆盖，还是版本化？
  - "活人兜底"如何实现：Colleague.skill 是否提供 escalation 机制（如 Skill 回答不确定时转回真人）？

## 6. 对个人能力有什么价值

- **Skill 设计模式**：理解"一个 Skill = 一个文件夹 + SKILL.md"的极简抽象，能迁移到自己设计 Agent 时复用。
- **跨平台 Skill 复用**：一个 Skill 写好后能在 Claude Code / Codex / 其他支持 Skill 的 Agent 里跑，不必重写。
- **Persona 三维拆解**：把"模仿某人"拆成沟通风格 / 决策模式 / 人际偏好三个独立维度，是更结构化的"蒸馏同事"方法论。

## 7. 对企业 AI 落地有什么价值

- **离职知识不流失**：当 senior 工程师 / 销售冠军 / 资深 HR 离职前，把 ta 的工作 Skill 沉淀下来；新人在 onboarding 阶段就能"问 senior 的 AI 副本"。
- **跨时区协作提效**：用 AI 副本先回答对方的紧急问题，真人醒来后再补充。
- **客户经营场景**：把"金牌销售"的沟通风格沉淀为 Skill，新销售在 IM 里和客户对话时，AI 实时给建议（而不是直接替回）。
- **培训加速**：新人第一周就能调用 senior 的 Skill 问"我这段代码这样写 OK 吗"，缩短学习曲线。

## 8. 可做的小实验

- 选 1 个非敏感的"内部讲师 / 老法师"作为试点（必须本人书面同意），跑一次 Colleague.skill 的数据抓取 + Skill 生成。
- 对比生成的 Skill 在"代码评审 / 文档撰写 / 客户回复建议"三个场景的实际可用度。
- 在飞书 / 钉钉里实验 @ Skill 触发 + escalation 回真人 的闭环是否流畅。
- 评估 OpenSkills 上的其他 Skill（如 PDF 处理、curator-skill）是否值得在公司内部署。

## 9. 风险和边界

- **隐私 / 合规**：把同事 / 客户的聊天记录转成 AI Skill 是高敏感操作，必须本人书面授权 + 法务审核 + 数据脱敏；不能"先抓了再说"。
- **误判 / 幻觉**：Persona 类 Skill 容易"形似神不似"或在被错误场景触发时给出不当建议；必须保留真人 override 通道。
- **边界**：哪些场景允许 AI 副本自动回复，哪些必须真人确认，需要事先划清。
- **客户对话版权**：客户与员工的对话涉及第三方权益，转成 Skill 时是否需要客户同意？法务先评估。
- **国内可用性**：openskills.cc 与 openagentskill.com 是否需要外网？GitHub 仓库可访问但 Skill 安装是否需要 npm 国际网络？
- **持续进化失控**：如果 Skill 自动抓取新数据持续训练，可能在某个时间点偏离本人意图；需要版本化 + 人工审核。
- **平台政策**：飞书 / 钉钉 / 企微对"批量抓取个人聊天"有平台风控；走官方开放 API 还是灰产通道，决定了项目能否长期跑。

## 10. 当前结论

数字分身 / 同事 Skill 方向已有 OpenSkills 市场 + Colleague.skill 工具在跑，不算空白赛道。落地前要先回答三个根本问题：
1. **合规问题**：把同事 / 客户的对话转成 AI Skill 是否需要本人 / 客户授权？
2. **差异化**：相比直接用 Colleague.skill，自己造一套"数字分身"的差异化在哪里？
3. **边界**：哪些场景允许 AI 副本自动回复，哪些必须真人确认？

如果答案是"客户场景用 Colleague.skill 等成熟工具 + 内部知识沉淀自己造"，那自研部分的真正壁垒应该在"边界控制 + 真人兜底 + 持续学习"这套闭环，而不是"蒸馏同事"本身。
