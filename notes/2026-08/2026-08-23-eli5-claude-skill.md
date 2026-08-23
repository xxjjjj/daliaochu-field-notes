---
title: "ELI5：Anthropic 社区的 10 行极简解释 Skill"
date: 2026-08-23
discovery_source:
  type: 抖音短视频转述
  title: 抖音 @未知作者 视频（链接见下）
  url: https://v.douyin.com/MYAhQRD57W4/
primary_object:
  type: open_source_project
  name: eli5 (anthropics/claude-plugins-community)
  url: https://github.com/anthropics/claude-plugins-community/blob/main/eli5/skills/eli5/SKILL.md
object_type: [open_source_project, methodology, trend_signal]
source_type: [GitHub, X/Twitter, 短视频]
business_tags: [ITBP, 个人能力, 运营]
problem_tags: [知识沉淀, 组织协同, 流程提效]
method_tags: [Agent, Prompt, Skill, 知识可视化]
tool_tags: [Claude Code, Claude Plugins]
value_stage: 可小实验
risk_tags: [幻觉, 成本, 类比过度简化]
public_level: public
---

# ELI5：10 行代码的零基础解释 Skill

## 1. 这是什么

ELI5（Explain Like I'm Five）是 Anthropic 社区插件市场 `anthropics/claude-plugins-community` 中的一个 Claude Code Skill。它把"面向零基础、用大图少字的 HTML 网页做解释"这套工作习惯封装成一个 `/eli5 <主题>` 斜杠命令。

整个 Skill 本体只有 7 行有效内容（321 字节）：

```yaml
---
name: eli5
description: Explain a topic like I'm a 5 year old. Use when the user types /eli5
  or asks for a dead-simple picture explainer of how something works.
---
# eli5
Explain like I'm someone who knows nothing about this topic, using a HTML artifact
with big pictures and few words.

Topic: $ARGUMENTS
```

作者是 Thariq Shihipar（@trq212，Claude Code 团队），MIT 协议，版本 1.0.0。Thariq 于 2026-08-21 在 X 上发帖推荐，称"Anthropic 内部很多人最近高频在用"；Vaibhav Sisinty 转推之后数小时内浏览量破 26 万（原帖约 2.8 万），社区在讨论是否将其转为官方插件。

## 2. 原始来源

- 发现入口：抖音短视频 <https://v.douyin.com/MYAhQRD57W4/>（用户许晶晶转述视频核心内容）
- 资料本体（已核读 raw 源文件）：
  - SKILL.md：<https://github.com/anthropics/claude-plugins-community/blob/main/eli5/skills/eli5/SKILL.md>
  - plugin.json：<https://github.com/anthropics/claude-plugins-community/blob/main/eli5/.claude-plugin/plugin.json>
- 相关链接：
  - Thariq 原推：<https://x.com/trq212/status/...>（2026-08-21）
  - Vaibhav 转推：<https://x.com/VaibhavSisinty/status/2091093560942883270>
  - 市场仓库说明：<https://github.com/anthropics/claude-plugins-community>

## 3. 核心观点 / 核心能力

- **三要素极简约束**：同时锁定"受众（零基础）+ 介质（HTML artifact）+ 信息密度（大图少字）"，让模型从"写答案"切换到"做解释"。
- **输出形态**：可滚动 HTML 网页，大示意图配少量文字，分步骤呈现；而非长段 Markdown 文本。
- **典型用法三步法**：
  1. `/eli5 <主题>` 生成零基础全景地图；
  2. 追问"这里简化了什么假设？有哪些例外？"；
  3. 再要技术版解释、代码和证据。
- **本质**：把一条稳定的团队工作习惯（"先建立概念地图再深挖"）固化成可复用快捷键，减少提示词遗漏和沟通成本。

## 4. 我学到了什么

- **好 Skill 不必长**：10 行就能产生显著行为改变，关键是把"受众 / 形式 / 认知任务"三件事一次说清，而不是堆指令。
- **介质即指令**：指定"HTML + 大图 + 少字"本身就在强制模型做信息分层和视觉化，比单纯说"请简单解释"有效得多。
- **Skill 是团队习惯的快捷键**：Thariq 的描述反复强调"people at Anthropic have been using a lot"——它不是炫技，是把内部已经验证的协作姿势产品化。这与打捞处"把流程沉淀成 Skill / playbook"的思路完全一致。
- **两阶段提示比一阶段更省**：先用 ELI5 建图、再追问细节，比一上来就让模型输出长篇技术解释更容易对齐认知，也更容易发现自己哪里没懂。

## 5. 它是否可信，哪些需要验证

可信度高：

- 已直接读取 GitHub 上的 SKILL.md 和 plugin.json 原文，与视频/推文描述一致；
- 仓库是 Anthropic 官方名下的社区市场镜像（README 说明：只读镜像，插件经自动安全扫描和人工审核后从内部流水线 nightly 同步）；
- 作者 Thariq Shihipar 确为 Anthropic Claude Code 团队成员，X 原帖可查。

需要注意：

- "23 分钟合并""3 小时 17 万浏览"等具体数字来自抖音视频转述，X 公开数据显示的是 26 万+ 浏览（24 小时口径），具体时间线未在官方 commit / X 中完全对应，不作为精确事实引用；
- 仓库 README 明确说"是否转为官方插件仍在 debate"，目前仍是社区插件，不是 Anthropic 一等公民；
- 安装依赖 Claude Code 2.1.233+（第三方实测），老版本不可用。

## 6. 对个人能力有什么价值

- **快速啃陌生系统**：新接手一个模块 / 代码库 / 业务流程时，先 `/eli5` 出一张图，比直接读文档快。
- **跨团队沟通降维**：把"RPA 是怎么跑的""CRM 公海池权限为什么这么配"这类话题用 ELI5 出图，给非技术同事或新成员做 onboarding。
- **自检理解深度**：如果 ELI5 图自己看着都别扭，说明概念还没真懂；再配合第二步追问假设和例外，能暴露认知盲区。
- **提示词写作范本**：写任何新 Skill / 指令时，对照"受众—形式—认知任务"三要素检查，避免写成又长又散的愿望清单。

## 7. 对企业 AI 落地有什么价值

- **内部知识 onboarding**：把 SOP、系统架构、业务规则用 ELI5 模式生成图文版入门材料，降低新员工和跨部门同事的理解门槛。
- **需求澄清前置**：业务方提需求前，先让其看一张 ELI5 版"当前系统是怎么工作的 /  proposed change 会怎么动"，对齐后再进实施，减少返工。
- **故障复盘沟通**：故障初因用 ELI5 出图给非技术干系人看，技术细节另出 RCA 文档，两层并存。
- **Skill 文化示范**：这是一个"极简 Skill 也能产生高杠杆"的真实案例，可以作为内部推广"把高频习惯沉淀成 Skill"的样板——不是只有复杂自动化才值得做 Skill。

## 8. 可做的小实验

- **实验 A（个人，零成本）**：在 Claude Code 中安装 ELI5，对三个真实陌生主题各跑一次（建议：①一个陌生开源项目架构；②一条 RPA 流程；③一个业务概念如"公海池领取规则"），记录输出质量和耗时，判断是否值得引入团队日常。
- **实验 B（Hermes 侧复刻）**：在 Hermes / 打捞处环境写一个等价的轻量 Skill `eli5-html`，不依赖 Claude Code 插件市场，让任意模型都能用"HTML + 大图少字"模式做零基础解释；对比 Claude 原生插件和 Hermes 版输出差异。
- **实验 C（业务 onboarding 试点）**：挑一个团队反复要解释的主题（如 CRM 权限模型、RPA 交接表怎么查），用 ELI5 出一版图文入门页，让 1-2 位新同事试用并给反馈。

## 9. 风险和边界

- **类比会隐藏技术边界**：ELI5 为了易懂会做简化和类比，不能替代架构评审、故障根因结论、权限审批等需要精确证据的决策。作者本人也明确说了这一点。
- **双重 token 成本**：先生成简化版、再追问技术版，确实会产生两轮调用；但相比"一上来写长答案但方向错了"的返工成本，通常仍是赚的。对成本敏感的场景应选择性使用。
- **HTML artifact 依赖宿主**：Skill 的输出效果依赖 Claude Code / Claude.ai 对 HTML artifact 的渲染能力；在纯文本终端或不支持 artifact 的客户端里，大图体验会打折。Hermes 侧复刻时需要确认 HTML 的落地形态（保存为文件 / 在浏览器打开 / 转图片）。
- **不是官方稳定 API**：目前仍是社区插件，作者团队在讨论是否转官方；接口和形态未来可能调整，不宜作为强依赖绑进生产流程。

## 10. 当前结论

**结论：值得跟，可小实验。**

ELI5 不是技术突破，而是一个把"先建概念地图再深挖"这套好习惯压缩成 10 行的极简产品。它的真正价值有两层：一是作为个人和团队的学习/沟通工具，直接可用；二是作为"Skill 应该怎么写"的范本——短、准、把受众和输出介质一起说清，比绝大多数又长又散的提示词工程示范更有说服力。

短期建议：在 Claude Code 里装上试三个真实主题；同时在 Hermes/打捞处侧复刻一个等价轻量 Skill，验证跨模型通用性。不建议直接绑进任何生产或对客流程。
