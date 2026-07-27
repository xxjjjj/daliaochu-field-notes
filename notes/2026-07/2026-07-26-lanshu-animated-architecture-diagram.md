---
title: "岚叔动态架构图 Skill（lanshu-animated-architecture-diagram）"
date: 2026-07-26
discovery_source:
  type: 抖音短视频
  title: "六叔ultra：动态流程图 Skill，让流程图动起来了"
  url: https://v.douyin.com/dnQ9v_9uV-0/
primary_object:
  type: open_source_project
  name: cclank/lanshu-animated-architecture-diagram
  url: https://github.com/cclank/lanshu-animated-architecture-diagram
object_type: [open_source_project]
source_type: [GitHub, 抖音]
business_tags: [ITBP, 个人能力, 市场]
problem_tags: [知识沉淀, 流程提效]
method_tags: [Prompt, Skill, 本地渲染]
tool_tags: [Python, Pillow, Excalidraw, Codex, GIF]
value_stage: 可小实验
risk_tags: [国内可用性]
public_level: public
---

# 岚叔动态架构图 Skill（lanshu-animated-architecture-diagram）

## 1. 这是什么

一个 Codex/Claude Code Skill + 本地 Python 渲染器，输入一份 JSON spec，输出三份产物：

- 可编辑的 `.excalidraw` 源文件
- 静态 `.png` 预览图
- 真正动起来的 `.gif`（带流动光效、模块呼吸高亮、暗画布手绘风标题）

视觉风格是黑底手绘技术图（类似 DailyDoseOfDS 风格），固定了一套暗色画布 + 顶部输入条 + 中部核心流水线 + 底部三栏面板 + 右上角手写签名的版式，主打"技术叙事一致性"。

## 2. 原始来源

- 发现入口：抖音 @六叔ultra 短视频
- 资料本体：https://github.com/cclank/lanshu-animated-architecture-diagram
- 作者：cclank（岚叔/lanshu），X 账号 @LufzzLiz
- 数据：Star 807 / Fork 89 / 12 commits / MIT License / Python 100%
- 相关链接：
  - Trendshift 热度页：https://trendshift.io/repositories/65239
  - 作者主页还做了 tokei（AI 工具用量追踪）、Cell Architecture Studio、News Aggregator Skill 等

## 3. 核心观点 / 核心能力

1. **"一张图动起来"的叙事价值**：静态架构图在汇报/文章/视频里信息密度高但节奏平，加 2 秒循环动效（光点流动 + 模块高亮）后，观众视线被自然引导走流程，演示效果明显提升。
2. **不依赖浏览器/远程 API**：纯 Python + Pillow 本地渲染，不需要 Excalidraw 在线、不需要 ImageMagick、不需要付费 API，出图可重复、可批量。
3. **三产物同时出**：`.excalidraw` 可二次编辑，`.png` 用于文档/PPT，`.gif` 用于演示/短视频，一份 spec 同时满足三种交付场景。
4. **自带验证机制**：`--verify` 算帧差证明 GIF 真在动，`--check` 校验输出合约（尺寸、帧数、Excalidraw ID 唯一性等），防止"看起来是 GIF 实际没动"。
5. **窄设计反而稳**：故意把视觉系统收窄到固定版式，牺牲灵活度换输出一致性——这是它看起来"成品感强"的关键。

## 4. 我学到了什么

- 技术内容传播里，**"动效 + 统一视觉语言"是低成本拉开质感差距的手段**，不需要复杂动画工具，Pillow 逐帧叠加发光/透明度就能出效果。
- Skill 的价值不只是提示词，而是 **"提示词 + 本地脚本 + 可验证输出合约"** 的组合——这正是可以迁移到 Hermes Skill 的设计模式。
- 作者用窄约束换一致性的思路，和我们做飞书卡片、PPT 模板时"宁可少样式也要统一品牌感"是同一个道理。

## 5. 它是否可信，哪些需要验证

**可信的部分（已从 README 确认）**：
- 开源、MIT、纯 Python、依赖只有 Pillow，技术栈简单透明
- 仓库结构清晰（SKILL.md / scripts/render / assets/spec / tests），不是噱头项目
- 有 verify + check 双重校验，作者对"真动"这件事是认真的

**待验证**：
- 默认 spec 的字段覆盖度够不够支撑我们 CRM/系统集成类的复杂架构图（目前看 icon 只有 folder/file/scan/shield/db/hash/package 7 种，节点类型可能不够）
- 中文标题/中文正文的字体渲染效果（README 示例全英文，需要实测中文字体 fallback）
- 画布尺寸固定 1210×1138，竖版偏文章/短视频，做横版 PPT 可能要改
- 抖音里说的是"流程图"，但实际仓库定位是"架构图"，流程节点（判断菱形、泳道、循环）是否原生支持要实测

## 6. 对个人能力有什么价值

- 做技术分享/内部培训/方案汇报时，多了一个"2 秒动态流程图"的现成武器，比静态截图更抓人。
- 可以直接借鉴它的 Skill 结构（spec 模板 + 本地渲染脚本 + 验证步骤），给我们自己的图表/卡片/报告类 Skill 做参考。

## 7. 对企业 AI 落地有什么价值

- **市场/品牌**：对外技术文章、公众号、客户案例里的系统架构图，如果能统一成这种"动态手绘"风格，品牌辨识度会比千篇一律的 draw.io 图高一个档次。
- **销售/售前**：方案汇报 PPT 里插 2 秒 GIF 演示数据流/业务流，比逐页翻静态图更容易让非技术客户看懂。
- **ITBP 内部**：CRM / NC / 飞书 / 第三方系统集成关系，用动态图讲"数据怎么走"比口头解释高效得多，新成员 onboarding 也能用。
- **能力建设**：它"Skill + 本地脚本 + 校验"的工程化思路，可以直接套用到我们后续做的其他内容生成类 Skill。

## 8. 可做的小实验

1. **本地跑通**：clone 仓库，`pip install -r requirements.txt`，用 default-spec.json 跑一次 render，确认本机中文渲染、GIF 动效是否正常。
2. **改造一个内部场景**：拿"CRM ↔ 飞书 ↔ NC"的数据流，写一份 spec，看能不能在现有 7 种 icon 下表达清楚；如果不行，记录缺哪些节点类型。
3. **接 Hermes**：把它作为一个 Skill 放进 Hermes skills 目录，试一句"用岚叔动态图把这段流程画出来"，看从文字到 GIF 的链路顺不顺。
4. **对比已有方案**：和我们现有的 architecture-diagram Skill（暗色 SVG）、baoyu-infographic 做个横向对比，看各自适合什么场景。

## 9. 风险和边界

- **视觉同质化**：这种风格近期在 AI 圈快速传播，如果对外材料统一用，过半年可能被觉得"AI 味重"，需要控制使用比例。
- **国内可用性**：无网络依赖，纯本地，不涉及数据外传，**数据安全风险低**，客户数据画架构图也可以用。
- **版式固定**：固定竖版画布，不适合横版大屏、复杂泳道流程、UML 类图等场景，不要当成通用画图工具。
- **License**：MIT，可商用可修改，保留版权声明即可。

## 10. 当前结论

一个完成度很高、工程化做得很扎实的小项目。不是玩具（有 verify/check、有测试、有 spec 文档），也不是重型工具（纯 Pillow，无外部依赖）。最值得打捞的不是"能画动态图"这个功能本身，而是 **它示范了一个 Skill 怎么做到"提示词 + 本地可重复渲染 + 输出验证"三合一**——这个模式对我们做企业内 AI 自动化有直接参考价值。建议下一步本地跑一次，验证中文和复杂架构场景。
