---
title: "AI Design Skills——给 AI 编程工具装"审美"的设计规则包"
date: 2026-11-08
discovery_source:
  type: 短视频
  title: "《好看哭了的神级网页Skill》（B站/短视频，博主「青青王-AI」，8310 粉，4365 播放）"
  url: ""
primary_object:
  type: open_source_project
  name: "elayadesign/ai-design-skills"
  url: "https://github.com/elayadesign/ai-design-skills"
object_type: [open_source_project, trend_signal]
source_type: [GitHub, 群聊线索]
business_tags: [市场, 产品, ITBP, 个人能力]
problem_tags: [转化, 流程提效]
method_tags: [Agent, Vibe Coding, Prompt]
tool_tags: [Claude Code, Cursor, Codex, Windsurf, Figma, skills.sh]
value_stage: 待验证
risk_tags: []
public_level: public
---

# AI Design Skills——给 AI 编程工具装"审美"的设计规则包

## 1. 这是什么

一个给 AI 编程工具（Claude Code、Cursor、Codex、Windsurf 等所有读 Markdown 规则的工具）用的**设计技能集合**。每个 skill 是一个自包含文件夹，本质是一份 400~500 行的 `SKILL.md` 规则文件，把专业设计师的决策（字体、间距、圆角、动效曲线、落地页结构、转化文案）固化成 AI 必须遵守的硬性规则，解决"AI 生成网页一眼假/土"的问题。

通过 skills.sh CLI 安装：Claude Code 落到 `.claude/skills/`，其他工具落到 `.agents/skills/`；也可以手动把 SKILL.md 贴进 AGENTS.md / .windsurfrules / .clinerules。

## 2. 原始来源

- 发现入口：群内手机短视频截图，博主「青青王-AI」，标题《好看哭了的神级网页Skill》，点赞 380 / 收藏 1116（收藏显著高于点赞，说明工具属性强、用户存着待用）
- 资料本体：https://github.com/elayadesign/ai-design-skills
- 配套仓库：旧站改造 skill 在独立 repo `elayadesign/redesign-skill`（528 stars）
- 第三方评测提到的在线审计站 SlopScan（https://slopscan.co），同作者，用同一套规则给现成网页挑"AI 味"并生成修复 prompt

## 3. 核心观点 / 核心能力

**一手核验事实（GitHub API + SKILL.md 原文，2026-11-08 拉取）：**

- 仓库 2026-07-29 创建，**14,885 stars / 1,128 forks，MIT 协议**（可商用、无需署名），最近 push 2026-11-01，活跃维护中
- 当前 skills 目录只有两个 skill：
  1. `landing-page-design`（479 行）：分 A/B 两部分。A 部分是营销策略——先做 intake（一个主行动、ICP、三大异议、流量来源、证据资产），再定页面结构（首屏五要素、中段论证、尾部异议处理）、四种布局选型、转化文案公式（如标题公式"{Outcome} without {pain}"）、SEO/AEO；B 部分是不可临场发挥的视觉系统——禁用 Inter/Roboto/Arial、字体只许 Geist/Manrope/Poppins、字号必须吸附 Tailwind 刻度、动效只许 transform/opacity 且禁用浏览器默认曲线、遵守 prefers-reduced-motion
  2. `figma-designer`（405 行，11-01 新增）：在 Figma 里建真实结构——变量、命名文本样式、auto layout、全状态组件
- 设计价值观（字体/配色/间距/动效）集中在文件底部一个区块，fork 后改这一处，整套规则自动跟随
- 规则里明确："规则与框架默认值冲突时规则赢；与用户明确指令冲突时用户赢"

**与短视频截图的出入（已核验）：** 截图里展示的第二个 skill `design-to-motion`（静态稿转动效演示视频/GIF）在 2026-10-27 的 commit 中被移到独立仓库，但作者 GitHub 名下目前查不到对应公开 repo（API 404），疑似改名私有或尚未公开。视频拍的是 10-27 之前的版本，信息已过时。

## 4. 我学到了什么

- **"Skill 化"正在从编程能力扩展到审美能力**：设计判断这种过去被认为不可编码的隐性知识，被拆成 intake 问卷 + 硬性视觉规则 + 构建顺序的 markdown 文件，而且靠纯规则（非模型微调）就能显著改善产出，这是 agent 生态里一个验证中的方向。同赛道还有 MengTo/Skills、LottieFiles/motion-design-skill、Nutlope/hallmark（anti-AI-slop）等，skills.sh 正在成为事实分发渠道。
- 规则写法值得借鉴：不是"要好看"这种空话，而是**可判定的禁令与数值**（不许用某字体、时长不许低于 200ms、字号必须落在刻度表上），加上"用户指令优先于规则"的逃生阀。这套写法和我们自己写 Skill 的原则一致——给模型的约束必须可执行、可检查。
- 它的 Part A（转化策略、文案公式、FAQ 异议处理）比 Part B（视觉）更有复用价值：视觉规则绑定 Tailwind/英文排版审美，但营销结构和 intake 方法是跨技术栈的。

## 5. 它是否可信，哪些需要验证

- 可信度：高 stars 增长、MIT、有 CI 校验 frontmatter、有独立第三方评测（Codearia Academy、HDATF）、credit 了上游 Leonxlnx/taste-skill，来源链清晰
- 待验证：
  1. 实测效果——规则文件对豆包/DeepSeek 等非 Claude 模型的约束力是否一样强（SKILL.md 是英文且按 Claude Code 语境写的，第三方模型遵循长规则的能力有差异）
  2. `design-to-motion` 独立仓库的真实去向（目前 404）
  3. 视觉规则的英文排版偏好（Geist 字体、无连字符、无斜体）直接用于中文页面是否合适，大概率需要本地化改写
  4. 视频博主宣称的效果未经我们复现，"好看哭了"是营销话术

## 6. 对个人能力有什么价值

- 做内部工具原型、汇报用 landing page、飞书之外的 HTML 演示页时，直接装 skill 或把 SKILL.md 的 Part A 当 checklist，能把"页面结构+文案"这一步的质量拉到专业线上
- 它的 intake 问卷（一个主行动、ICP、三大异议、证据资产）可以直接搬到需求沟通里，和我们"先问碰到什么事"的挖根口径互补——一个问场景，一个问转化要素

## 7. 对企业 AI 落地有什么价值

- 市场部场景最直接：活动落地页、产品宣传页的初稿生成，外部供应商/自研都能用，MIT 协议无版权负担
- 更大的启示是**企业可以把自己的设计规范（品牌色、字体、组件用法、红线）写成同构 SKILL.md**，让任何 AI 编程工具产出的页面自动符合 VI，而不是靠事后设计评审。这比买设计类 SaaS 更贴合"在现有工具链里加规则"的落地路径
- 风险可控：纯文本规则、可 fork 内化、不联网、不碰数据

## 8. 可做的小实验

- 小实验（待晶晶点头再做）：clone 仓库，把 landing-page-design 装到一个测试项目，分别用 Claude Code（接 DeepSeek V4 Flash）和豆包端点各生成一个同一需求的落地页，对照无 skill 基线盲评，验证规则跨模型的约束力；同时把字体/排版规则替换成中文友好版本，测改写成本
- 轻量做法：不安装，只把 Part A 的结构和文案公式抽成一份中文 checklist 备用

## 9. 风险和边界

- 无数据安全风险（纯 markdown 规则、本地安装），MIT 可商用
- 审美是英文 SaaS 风格（类 Linear/Vercel），与英科官网风格未必一致，不能直接当品牌规范
- 视频信息已过时（design-to-motion 已移出），传播时以 GitHub 当前 README 为准
- 依赖 Node.js + skills.sh 安装链路，内网环境需手动拷贝 SKILL.md

## 10. 当前结论

值得标记为"可小实验"的设计类 agent skill：一手事实清楚、协议干净、社区验证充分，短期价值是落地页生成质量 checklist，中期价值是"企业设计规范 SKILL 化"的参考样板。暂不安装，等实测需求出现时按第 8 节做对照实验。
