---
title: "Taste Skill & Impeccable：两个反 AI 模板化设计的 Agent Skills"
date: 2026-07-29
discovery_source:
  type: 抖音
  title: "2个skill提升你的AI审美 # AI新星计划"
  url: https://v.douyin.com/GI1tfE0TiY8/
primary_object:
  type: open_source_project
  name: "taste-skill + impeccable (联合分析)"
  url:
    - https://github.com/Leonxlnx/taste-skill
    - https://github.com/pbakaus/impeccable
object_type: [open_source_project, trend_signal]
source_type: [抖音, GitHub]
business_tags: [市场, 产品, ITBP]
problem_tags: [内容生产提效, 品牌体验, 知识沉淀]
method_tags: [Agent, Skill, Vibe Coding, 前端自动化]
tool_tags: [taste-skill, impeccable, Claude Code, Cursor, Codex CLI]
value_stage: 可小实验
risk_tags: [国内可用性]
public_level: public
---

# Taste Skill & Impeccable：两个反 AI 模板化设计的 Agent Skills

## 1. 这是什么

两个开源的 Agent Skill 项目，给 AI 编程工具注入设计判断力，解决 AI 生成前端 UI 千篇一律（"AI slop"）的问题。合计 GitHub 119k+ 星标，均采用 SKILL.md 格式，可直接注入 Claude Code、Cursor、Codex、Gemini CLI 等主流 AI 编程工具。

## 2. 原始来源

- 发现入口：抖音「粑粑boy」视频
- 资料本体：
  - **Taste Skill**：GitHub [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill)，68.4k ⭐，MIT License，144 commits，作者 @lexnlin，官网 [tasteskill.dev](https://tasteskill.dev)
  - **Impeccable**：GitHub [pbakaus/impeccable](https://github.com/pbakaus/impeccable)，51.1k ⭐，Apache 2.0，1,191 commits，作者 Paul Bakaus（@pbakaus），官网 [impeccable.style](https://impeccable.style)

## 3. 核心观点 / 核心能力

### Taste Skill
- **本质**：给 AI 注入"设计审美"的便携 Skill 包，10 个子 skill 覆盖不同风格（默认、极简、野蛮主义、高端视觉、GPT 强化版等）+ 图像生成 skill（用于先出参考图再写代码的 pipeline）
- **工作机制**：三个 1-10 旋钮——DESIGN_VARIANCE（布局实验性）、MOTION_INTENSITY（动画深度）、VISUAL_DENSITY（信息密度），让 AI 从"中心对齐+Inter 字体+紫蓝渐变"模板中跳出来
- **v2 实验版**刚发布，从 v1 大幅重写，增加了 brief 推断、设计系统映射、em-dash 硬禁用、GSAP 动画骨架等
- 安装：`npx skills add https://github.com/Leonxlnx/taste-skill`

### Impeccable
- **本质**：从 Anthropic 的 frontend-design skill 演化而来，升级为完整的设计语言系统
- **23 个命令**如 `/impeccable polish`、`audit`、`critique`、`distill`、`animate`、`bolder`、`quieter`、`typeset`、`layout`、`delight` 等，让开发者用一句话调整设计方向
- **60 条确定性检测规则**（无需 LLM/API key），通过 CLI 和浏览器扩展运行，自动识别 AI slop 特征（侧边栏边框、紫色渐变、bounce easing、深色发光等）和通用设计问题（行宽、触摸目标、跳级标题等）
- 安装：`npx impeccable install`，新项目先跑 `/impeccable init` 写入 PRODUCT.md 和 DESIGN.md

### 关键区别
| | Taste Skill | Impeccable |
|---|---|---|
| 定位 | 审美注入 + 风格变体 | 设计语言系统 + 质量门禁 |
| 规模 | 1 个主 skill + 9 个变体 | 1 个 skill + 23 个命令 + 60 条检测规则 |
| 核心价值 | 让 AI 不千篇一律 | 让 AI 设计可审查、可迭代、可落地 |
| 特色 | 图片→代码 pipeline | 浏览器扩展实时检测 |

## 4. 可借鉴的设计思路

两个工具都用了同一套方法论解决问题：

1. **共享词汇表**——不给 AI 模糊的"做好看点"，而是给精确的命令（polish / bolder / distill / typeset），这跟给非设计师同事提反馈的逻辑一样
2. **确定性 + LLM 混合**——Impeccable 的 60 条检测规则不用调 LLM，纯规则匹配，成本低、速度快；LLM 只用在需要判断力的 critique 环节
3. **设计上下文先行**——Impeccable 的 `init` 先写 PRODUCT.md + DESIGN.md，后续所有命令都基于这个上下文，避免每次重新推断设计意图
4. **从 Anthropic skill 分叉演化**——Impeccable 证明了好的 Agent Skill 可以像开源代码一样被 fork 和改进

## 5. 对 ITBP / INTCO 业务的价值

### 市场/品牌侧
- 如果需要快速搭建活动落地页、产品介绍站、品牌官网，这些 Skill 可以让 AI 编程工具生成的页面直接达到商业可用水平，不用每次都找设计师重做
- Taste Skill 的图像生成 pipeline（先出参考图→分析→写代码）适合快速出多个设计方向供决策

### 产品侧
- Impeccable 的 60 条检测规则可以当设计系统质量门禁用，接入 CI 后每次前端改动都能自动检测是否引入 AI slop 或违反设计规范

### ITBP 自身
- 两个项目都是 SKILL.md 格式，跟 Hermes 的 Skill 体系在概念上一致——这是"如何写好 Agent Skill"的顶级案例，结构和命令设计值得参考
- Impeccable 的"确定性规则 + LLM"混合模式对设计 CRM AI 化的审核/检测类功能有直接启发

## 6. 风险和边界

- **国内可用性**：两个工具依赖的 AI 编程工具（Claude Code、Cursor、Codex 等）部分在国内需要特殊网络条件
- **License**：Taste Skill 是 MIT（宽松），Impeccable 是 Apache 2.0（也宽松但需保留 NOTICE），都可以安全使用和修改
- **不存在代币/发币项目**：Taste Skill README 特别声明没有官方的 token、coin 或 crypto 项目，但鉴于 68k 星标的热度，需要注意假冒项目
- **仍在快速迭代**：Taste Skill 刚出 v2 实验版，Impeccable 有 1,191 次提交，API 和行为可能在短期内变化

## 7. 可做的小实验

1. 在本地 Hermes 环境中安装其中一个 Skill，用 Claude Code / Codex 生成一个 INTCO 风格的示例页面，看实际效果
2. 研究 Impeccable 的 60 条 detector 规则，看哪些可以转成 CRM UI 或飞书卡片的质量检查规则
3. 研究两个 Skill 的 SKILL.md 写法，提炼出写好 Agent Skill 的模式（命令设计、上下文管理、错误处理）

## 8. 当前结论

两个项目都是 2026 年 AI + 设计领域的标志性开源项目，合计 119k+ 星标说明市场对"AI 产出质量"的焦虑和需求都很真实。对 INTCO 的直接业务价值在于：如果需要快速搭建面向客户的 Web 页面（活动页、产品站、品牌内容），它们可以让 AI 生成的页面从"能用"提升到"好看且可用"。对 ITBP 自身，这是学习和借鉴 Agent Skill 设计模式的优质案例。
