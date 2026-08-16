---
title: "diagram-design：面向 AI Agent 的编辑级图表 Skill"
source_type: 小红书截图（作者「有方只做AI」评论区引流）
source_url: https://github.com/cathrynlavery/diagram-design
author: cathrynlavery（littlemight.com / BestSelf.co）
verified: true
value_stage: 已追源
public_level: public
tags: [diagram, agent-skill, claude-code, codex, mermaid, svg, design-system, editorial]
captured: 2026-08-16
---

## 一句话

不是图表库，也不是独立 App，而是一个装在 Claude Code / Codex / Pi 里的 **Agent Skill**——让 Agent 按"杂志/编辑级"标准生成自包含 HTML+SVG 图表，明确反对 Mermaid 默认的"渐变圆角方块 AI 味"。

## 事实核验（GitHub API，2026-08-16）

- Repo：`cathrynlavery/diagram-design`，MIT，主语言 HTML
- Stars：**19,056**（小红书截图写 18,822，发帖后仍在涨）
- Forks：1,155
- 创建：2026-04-16；最近 push：2026-08-14（4 个月内活跃迭代到 2.3）
- Repo 描述写 "29 editorial diagram types"，但 README 类型表实际列 **27 种**（Loop、Medallion 等是后加的，文案没完全对齐，属于小瑕疵，不影响使用）
- 小红书帖的"GitHub 趋势榜第一""封神"是引流话术，star 数真实，但不要被情绪带节奏

## 它到底产出什么

27 种视觉类型，每种三种静态变体（minimal light / minimal dark / full-editorial）：

- 结构类：Architecture、Flowchart、Sequence、State machine、ER、Timeline、Swimlane、Nested、Tree、Org chart、Layers、Process、Data flow、High-Level、IT current-state
- 决策/比较类：Quadrant、Consultant 2×2、Venn、Pyramid/Funnel、Radar、Loop（飞轮）
- 数据类：Bar、Line、Scatter、Gantt、Medallion（数据分层）
- 治理类：DP integration、DP security matrix（数据产品权限矩阵）

输出是**单文件 HTML+SVG**，无构建步骤、无 JS 依赖、无外部图片，浏览器直接打开。动效是可选的（reveal/step/loop），默认关闭，且控制器代码被 pin 住、禁止 inline 事件和远程资源。

## 三个真正值得抄的设计

### 1. 语义模式 vs 视觉类型分离
`semantic-patterns.md` 定义了 7 种"行为模式"（fan-in 队列/瓶颈、重复阶段槽、非结构化输入转换、配对 policy trace、安全铺装路、治理目录、补偿安全层），每种模式声明触发器、原语、预算、反模式、静态回退、最近视觉类型。

→ 加新模式**不增加类型数**，避免"每来一个需求就加一种图"的膨胀。这对我们自己写 Skill 很有参考价值：行为和布局解耦。

### 2. 渐进式加载，控 Agent 上下文
启动时 Agent 只看到 skill 名+描述；匹配到请求才加载 SKILL.md；选中类型后才加载那一个 `type-*.md`；需要行为才加 semantic-patterns；要动效才加 animation。README 明确列了"你问什么→Agent 加载什么"的对照表。

→ 这正是 Agent Skill 应该长的样子。我们自己的 skill 越来越多，值得照这个模式拆 references/。

### 3. draw.io / Mermaid 导入有"保真台账"
导入不是黑盒转换，四个旋钮：
- Format：html/svg/png/html+png
- Size：doc-inline / doc-wide / slide-16x9 / social-og / print-a4 等（同时改 viewBox 和字号）
- Detail：faithful(≤24) / balanced(≤12) / simplified(≤7)，固定降级阶梯（先删装饰→重复→叶子簇→基础设施）
- Audience：engineer/mixed/executive，改的是**措辞**不是节点数

结束输出 fidelity ledger：合并了什么、折叠了什么、丢了什么、什么完整保留。坐标/配色/字体/连线 spaghetti 不带过去，组件/关系/分组/方向保留。

→ 这比"AI 一键转图"靠谱得多，因为它承认转换有损并把损失告诉你。

## 品牌适配

60 秒 onboarding：给一个网址，Agent 抓首页 → 提主色和字体栈 → 映射到 paper/ink/muted/accent/link 语义角色 → 出 diff 预览 → 写入 `style-guide.md`。自动做 WCAG AA 对比度检查，不达标给调整值。支持多 client profile（`~/.diagram-design/profiles/<slug>.md` + 项目里放 `.diagram-design` marker）。

## 怎么用 / 怎么接到我们这边

- Claude Code：`/plugin marketplace add cathrynlavery/diagram-design` 然后 `/plugin install`
- Codex：`codex plugin marketplace add` + `codex plugin add`
- Pi：`pi install https://github.com/...`
- 它本质是符合 Agent Skills 规范的一个目录（`skills/diagram-design/` 下 SKILL.md + references/ + assets/ + scripts/）。**Hermes 自己的 skill_manage 也是同一套规范**，理论上可以直接把 `skills/diagram-design/` 放进 `~/.hermes-v019/skills/` 试用，或 fork 一份改造成中文/内网品牌版。

## 对 INTCO / 软件实施组的实际价值

1. CRM / RPA 交付文档里的架构图、流程图、权限矩阵、数据流向图，Mermaid 默认效果在客户 PPT 里偏"玩具感"，这个出图直接是演示级。
2. 客户提案、内部汇报里的 2×2、金字塔、飞轮、timeline，不用再 Figma 调半小时。
3. 自包含 HTML+SVG 可以直接嵌飞书文档/知识库，也能导出 PNG 进 PPT。
4. 更值钱的是它的 **Skill 架构本身**——语义模式、渐进加载、保真台账、品牌 token，这四点值得在我们自己写飞书/CRM/RPA 相关 Skill 时借鉴。

## 风险与待验证

- **不是即装即用的工具**：脱离 Claude Code/Codex/Pi 它只是一堆模板 HTML，没有"帮你画图"的智能体。在 Hermes 里接需要验证 Agent Skills 兼容性。
- **版本数对不上**：宣传 29、表列 27，小问题但说明营销文案和代码有漂移，引用时以实际类型表为准。
- **PNG 导出依赖 Playwright + Chromium**（`pip install playwright && playwright install chromium`），SVG 导出直接抽 `<svg>` 节点。
- **品牌抓取会访问外部网址**，内网项目别直接喂内部 URL；手动改 style-guide.md 更安全。
- 小红书引流帖作者身份未核实，star 数真实但"趋势榜第一"未独立验证。

## 下一步

- [ ] 把 `skills/diagram-design/` clone 下来，丢进 `~/.hermes-v019/skills/creative/diagram-design/` 跑一张真实架构图试试效果
- [ ] 重点读 `references/semantic-patterns.md` 和 `SKILL.md` 的加载路由，作为我们重技能拆 references/ 的参考样板
- [ ] 如果出图质量稳定，做一个 INTCO 品牌 profile（英科蓝/医疗配色），给实施组交付文档统一出图风格
