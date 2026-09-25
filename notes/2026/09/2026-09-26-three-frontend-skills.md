---
title: 三个前端神级 Skill 核查——web-design-guidelines / design-taste-frontend / 截图转码
date: 2026-09-26
discovery_source:
  type: 小红书
  title: 分享几个神级skill（AI有点意思）
  url: https://www.xiaohongshu.com/explore/6a98c5f9000000002802966c
primary_object:
  type: open_source_project
  name: 三个 agent skill（Codex/Claude Code 通用）
  url: https://github.com/vercel-labs/agent-skills
object_type: [open_source_project, trend_signal]
source_type: [小红书, GitHub]
business_tags: [产品, ITBP, 个人能力]
problem_tags: [流程提效]
method_tags: [Agent, Vibe Coding]
tool_tags: [Codex, Claude Code, skills]
value_stage: 可小实验
risk_tags: []
public_level: public
---

# 三个前端"神级 Skill"核查

## 1. 这是什么

小红书视频"分享几个神级skill"（博主 AI有点意思，4847 赞 / 1.1 万收藏，四川）介绍 3
个给 Codex 用的前端 skill。评论区 AI 助手"点点"的总结在网上流传，但只说了功能、没说机制。
本次核查到三个 skill 的一手 SKILL.md，结论如下。

## 2. 原始来源

- 发现入口：https://www.xiaohongshu.com/explore/6a98c5f9000000002802966c
- ① web-design-guidelines（Vercel 官方）：
  https://github.com/vercel-labs/agent-skills/blob/main/skills/web-design-guidelines/SKILL.md
  规则本体：https://github.com/vercel-labs/web-interface-guidelines（command.md）
- ② design-taste-frontend（社区 Leonxlnx/taste-skill，v2 实验版）：
  https://github.com/Leonxlnx/taste-skill
- ③ 截图转码（视频未给确切名称，最匹配的 agent skill）：
  https://smithery.ai/skills/OneWave-AI/screenshot-to-code
  同名但不同物：abi/screenshot-to-code 是独立 Web 应用，不是 skill。

## 3. 核心观点 / 核心能力

**① web-design-guidelines —— 审查器，Vercel 官方，约 61.3 万安装**
SKILL.md 本身不含任何规则，每次 review 前用 WebFetch 实时拉取
vercel-labs/web-interface-guidelines 的 command.md，读你的代码，按规则检查，以
`file:line` 的精简格式输出问题。本质是"把 Vercel 的界面规范接成 agent 的审查动作"。
缺点：审查时必须联网，规则永远是当天 main 分支版本（不可复现）。

**② design-taste-frontend —— 生成器，社区作品，SKILL.md 约 30KB**
核心不是空泛的"提高审美"，而是一套强制流程：
- Brief inference：先读题（页面类型、vibe 词、参考、受众、品牌资产），输出一行
  "Design Read" 再动手；
- 三个拨盘：DESIGN_VARIANCE（对称→艺术）/ MOTION_INTENSITY（静态→电影感）/
  VISUAL_DENSITY（空旷→驾驶舱），按场景给预设表；
- Brief→设计系统映射（何时用 Material/Carbon/Polaris/shadcn/原生 CSS）；
- 明令禁止 LLM 默认套路：紫色渐变、暗色网格居中 hero、三等分卡片、无脑玻璃拟态、
  Inter+slate-900；禁用 em-dash；redesign 必须先审计。
边界写得很清楚：只服务 landing page / portfolio / redesign，不服务
dashboard、数据表、多步产品 UI。当前为 v2 experimental，仍在迭代。

**③ screenshot-to-code（OneWave-AI）—— 复刻器**
工作流：识别项目现有技术栈（读 package.json，不问废话）→ 看图写短 spec
（注意 Retina 2x 截图要除 2）→ 语义化构建、抽组件、token 集中 → 响应式 →
**渲染截图与原图并排对比、按"布局→间距→字号→颜色"顺序修 2-3 轮** → 交付假设清单。
最有价值的是最后的 render-and-compare 自检闭环。

## 4. 我学到了什么

三个 skill 恰好是前端产出的三个工位，不是重复关系：
**① 事后审查（QA） ② 事前品味约束（生成时的方向） ③ 视觉复刻（spec→码→自检）**。
"神"的共同点不是知识多，而是**把动作流程化**：实时拉规范、强制先读题、强制渲染对比——
都是在堵模型"不看条件直接给默认答案"的本能。

## 5. 它是否可信，哪些需要验证

- ① 可信度高：Vercel 官方仓库、60 万+安装。但规则实时拉 main，企业用应把
  command.md fork 固定版本，否则审查结果不可复现。
- ② 中高：社区个人项目，星标高、内容扎实，但 v2 自述 experimental；另有
  sickn33/agentic-awesome-skills 等多个转载源，安装时认准原仓库 Leonxlnx/taste-skill。
- ③ 有不确定：视频没有给第三个 skill 的名字，"点点"也只写了功能描述。
  OneWave-AI/screenshot-to-code 是最匹配的 agent skill，但是否就是视频所指无法证实；
  需看原视频画面确认。评论区"GitHub 上找不到"即因名称不明。

## 6. 对个人能力有什么价值

做 landing / demo / 内部小工具前端时，可组合使用：②定方向 → ③复刻参考 → ①出审查报告，
把"好不好看"从主观争论变成有规则、有 diff 的流程。

## 7. 对企业 AI 落地有什么价值

内部管理后台不在②的适用范围（其作者明确排除 dashboard/数据表）；
但市场部落地页、活动页、对外小站点这类场景三个 skill 都能直接上。
① 的模式值得借鉴：企业可把自己的设计规范/前端规约做成"实时拉取+file:line 审查"的
内部 skill，比写一篇没人看的规范文档有效。

## 8. 可做的小实验

- 用现有 HTML 产物跑一次 ①，看 Vercel 规则对内部页面的命中率；
- 给 ② 喂同一 brief 对比装/不装的产出差异；
- 用一张真实页面截图验证 ③ 的 render-compare 两轮后还原度。

## 9. 风险和边界

- ① 联网依赖 + 规则漂移；国内访问 GitHub raw 可能不稳。
- ② 审美规则有强主观倾向，不应覆盖已有品牌系统（SKILL.md 自己也这样声明）。
- ③ 复刻他人页面有版权风险，仅限自有/授权页面。
- 三者均为前端营销页取向，不要误用于企业复杂业务系统。

## 10. 当前结论

评论区总结方向正确但信息过浅。三个 skill 真实分工是审查器 / 生成器 / 复刻器，
①Vercel 官方可信度最高，②内容最扎实但仍是实验版，③视频所指具体哪一个尚未确认。
定级"可小实验"，建议先在市场部页面或个人 demo 上各跑一次实测。
