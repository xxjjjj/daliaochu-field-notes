---
title: Codex Skills 清单追源 MVP：last30days 与 xhs-ops 源码/仓库初读
date: 2026-07-02
related_note: notes/2026-07/2026-07-01-codex-skills-money-list.md
experiment_type: 工具实测
business_tags: [市场, 销售, 运营, ITBP, 个人能力]
problem_tags: [获客, 转化, 知识沉淀, 用户洞察, 流程提效]
method_tags: [Agent, 自动化, 知识库, Skill]
tool_tags: [Codex, Claude Code, Agent Skills, last30days, xhs-ops, OpenClaw]
value_stage: 可小实验
risk_tags: [数据安全, 国内可用性, 版权, 合规, 账号风控]
public_level: sanitized
---

# Codex Skills 清单追源 MVP：last30days 与 xhs-ops 源码/仓库初读

## 1. 测试背景

前序资料是一张“Codex 不装这 8 个你都在亏”的社媒截图，初步判断它本质是 Agent Skill 商业化方向的传播素材。此前已经做了初步追源，但仍停留在“继续研究建议”，没有把后续动作真正推进到仓库本体阅读。

本次补上最小闭环：先选两个相对低风险且业务价值直接的方向做仓库级初读：

- `last30days`：趋势雷达/选题研究。
- `xhs-ops`：小红书运营工作流。

## 2. 测试假设

1. 如果一个 Skill 真有业务价值，它应当不只是一个提示词，而是有明确的 `SKILL.md`、脚本/引用资料、安装路径、输出契约和风险边界。
2. 对企业内部可先验证“研究/选题/复盘”类能力，不先碰自动发布、法律结论、金融建议等高风险动作。
3. 通过仓库结构和运行规则，可以判断它是否适合改造成内部市场/销售/产品支持流程。

## 3. 测试对象

### last30days

- 仓库：`mvanhorn/last30days-skill`
- License：MIT
- 本地初读路径：`/tmp/daliaochu-last30days-skill`
- 规模：约 327 个文件；以 Python 为主，另有 Markdown、JSON、JS、Go、Shell 等。
- 核心文件：`skills/last30days/SKILL.md`、`skills/last30days/scripts/last30days.py`、`CONFIGURATION.md`、`README.md`。

### xhs-ops

- 仓库：`Xiangyu-CAS/xiaohongshu-ops-skill`
- License：仓库初读未发现明确 License 文件，需继续确认授权边界。
- 本地初读路径：`/tmp/daliaochu-xhs-ops-skill`
- 规模：约 31 个文件；主要是 Markdown SOP、图片/GIF 示例、persona、知识库目录。
- 核心文件：`SKILL.md`、`README.md`、`persona.md`、`references/xhs-runtime-rules.md`、`references/xhs-topic-ideation.md`、`references/xhs-publish-flows.md`。

## 4. 测试步骤

1. 克隆两个公开仓库到本地临时目录。
2. 查看仓库文件结构、核心 `SKILL.md`、README、配置文档与运行规则。
3. 记录是否有 License、安装方式、依赖、风险控制、可迁移价值。
4. 形成下一步内部 MVP 建议。

## 5. 输入材料

- 原始线索：打捞处群内截图。
- 前序笔记：`notes/2026-07/2026-07-01-codex-skills-money-list.md`
- 公开仓库：
  - `https://github.com/mvanhorn/last30days-skill`
  - `https://github.com/Xiangyu-CAS/xiaohongshu-ops-skill`

## 6. 结果记录

### last30days 初读结论

它不是简单搜索提示词，而是一个较完整的 Agent Skill 产品：

- 有明确的 Skill 元数据：名称、版本、描述、主页、仓库、License、依赖环境。
- 运行对象是 `/last30days <topic>`，不是裸脚本调用。
- 数据源包括 Reddit、X、YouTube、TikTok、Hacker News、Polymarket、GitHub、Web、Perplexity 等。
- 默认免费可用源包括 Reddit、HN、Polymarket、GitHub；更多源依赖 API Key、浏览器 Cookie 或外部 CLI。
- 有非常重的输出契约，说明作者在反复修正模型跑偏、乱输出、漏引用、输出格式不稳定等问题。
- 有 `CONFIGURATION.md` 明确配置层级、API Key、保存路径、预检、诊断方式。

对我们有价值的不是直接全量安装，而是借鉴它的产品结构：

```text
多源检索 → 互动信号排序 → 证据聚合 → 模型综合 → 固定输出契约 → 可保存报告
```

这非常适合改造成“市场/销售/产品趋势雷达”。

### xhs-ops 初读结论

它更像一个小红书运营 SOP Skill，而不是重代码项目：

- 核心是 `SKILL.md` + `references/` 下的操作规范。
- 覆盖账号定位、首页推荐流分析、账号分析、选题灵感、Viral Copy、内容发布、评论回复、知识库沉淀。
- 明确要求使用 OpenClaw 内置浏览器 profile=`openclaw`。
- 运行规则强调低 token、少快照、失败最多重试一次、关键节点保留证据、发布前停手。
- README 显示它支持自动发布和自动回复，但这两项对真实业务账号风险较高。

对我们有价值的不是“全托管小红书账号”，而是拆出低风险模块：

```text
账号定位 → 对标样本拆解 → 选题池 → 内容结构 → 风险标注 → 复盘知识库
```

这适合市场内容团队做选题会辅助、内容复盘和竞品分析。自动发布、自动回复暂不建议作为第一阶段。

## 7. 失败点 / 异常点

1. `xhs-ops` 初读未发现明确 License，公开复用和二次封装前必须确认授权。
2. `last30days` 依赖多源 API、Cookie、外部 CLI，完整能力接入成本不低；国内网络环境、X/TikTok/YouTube 可用性需要单独验证。
3. 两个项目都存在“对外平台操作”问题：账号登录、Cookie、自动化访问、平台规则和风控都不能忽略。
4. `last30days` 的 Skill 契约很长，说明模型调用该类 Skill 时容易跑偏；内部改造时要缩短并强约束输出格式。
5. `xhs-ops` 的自动发布链路不适合直接用于企业账号，必须默认人工确认。

## 8. 是否值得继续

值得继续，但方向要收敛：

- 第一优先：`last30days` 类趋势雷达，用于市场/销售/产品的资料研究与选题判断。
- 第二优先：`xhs-ops` 的选题和复盘模块，用于内容运营辅助，不直接发布。
- 暂不优先：自动发帖、自动回复、法律/金融/跨境自动结论。

## 9. 下一步动作

### MVP A：趋势雷达

选一个真实但不敏感的业务关键词，跑一版“最近 30 天趋势简报”结构：

- 输入：关键词、目标人群、业务目的、平台范围。
- 输出：热点主题、证据来源、用户痛点、可做选题、销售/市场可用话术、风险。
- 验收：业务同事是否能直接拿去开选题会或做客户沟通准备。

### MVP B：小红书选题复盘

先不登录真实账号，不自动发布。用公开样本或手工提供的链接做：

- 标题/封面钩子拆解。
- 评论区需求信号。
- 可复用内容结构。
- 3-5 条安全选题。
- 版权/平台风险提示。

## 10. 公开边界

本实验只记录公开仓库和公开资料的结构化判断，不包含群聊原文、内部客户、真实业务数据或账号登录信息。后续如涉及真实账号、Cookie、内部资料或客户场景，应改为 `private-only` 或先做脱敏版本。
