---
title: Next AI Draw.io：用自然语言生成和编辑 draw.io 图表
date: 2026-07-04
discovery_source:
  type: 群聊线索
  title: next-ai-drawio
  url: 
primary_object:
  type: open_source_project
  name: DayuanJiang/next-ai-draw-io
  url: https://github.com/DayuanJiang/next-ai-draw-io
object_type: [open_source_project, commercial_product]
source_type: [GitHub, 官网, VSCode Marketplace, 群聊线索]
business_tags: [ITBP, 产品, 销售, 市场, 个人能力]
problem_tags: [流程提效, 知识沉淀, 组织协同, 方案表达]
method_tags: [Agent, MCP, Prompt, 自动化]
tool_tags: [draw.io, Next.js, React, TypeScript, MCP, VSCode]
value_stage: 可小实验
risk_tags: [数据安全, 权限, 成本, 幻觉, 国内可用性]
public_level: public
---

# Next AI Draw.io：用自然语言生成和编辑 draw.io 图表

## 1. 这是什么

Next AI Draw.io 是一个把 AI 对话能力接入 draw.io 图表编辑器的开源项目。它的核心能力是：用户用自然语言描述架构、流程或草图，系统生成或修改 draw.io 的图表内容；同时提供 Web 应用、桌面端、MCP Server，以及 VS Code 扩展入口。

这个线索不是单纯的“画图工具”，更像是把“方案表达”从手工拖拽，推进到“自然语言 → 可编辑结构化图表”的工作流。

## 2. 原始来源

- 发现入口：打捞处群聊关键词 `next-ai-drawio`
- 资料本体：GitHub 项目 `DayuanJiang/next-ai-draw-io`
- GitHub：https://github.com/DayuanJiang/next-ai-draw-io
- 在线入口：https://next-ai-drawio.jiang.jp/
- VS Code 扩展：https://marketplace.visualstudio.com/items?itemName=warm3snow.next-ai-drawio
- License：Apache-2.0
- 项目活跃度快照（2026-07-04 查询）：约 32.7k stars、3.4k forks，最近一次 push 为 2026-07-03。

## 3. 核心观点 / 核心能力

1. **AI 生成的是可继续编辑的图表，而不是一次性图片。** 这点对方案沟通很关键，因为企业内部的流程图、架构图、系统边界图经常需要反复改。
2. **draw.io 是成熟图表编辑底座。** 它降低了自研图形编辑器的成本，AI 主要负责生成和修改底层结构。
3. **MCP Server 支持值得关注。** 这意味着 Claude Desktop、Cursor、VS Code 等 Agent 可以把“画图”作为一个工具调用，而不只是网页里聊天。
4. **多模型和私有化部署是企业落地前提。** 项目支持多 AI provider、服务端模型配置、Docker/Vercel/Cloudflare 等部署方式，具备进一步内网化试验的可能。

## 4. 我学到了什么

传统流程图工具解决的是“人会画但画得慢”；这类 AI + draw.io 工具解决的是“业务/技术人员能先用语言把结构表达出来，再由人校正”。它的价值不在于替代专业画图，而在于缩短从想法、会议讨论、需求描述到第一版可视化图的时间。

对 ITBP 来说，很多时候卡点不是没有方案，而是方案讲不清楚、边界画不出来、跨部门理解不一致。这个方向可以作为“需求澄清”和“方案共识”的辅助工具。

## 5. 它是否可信，哪些需要验证

已确认的可信点：

- 找到 GitHub 本体和 VS Code Marketplace 入口；
- 项目使用 Apache-2.0 License；
- 近期仍有提交，社区热度较高；
- README/项目说明覆盖 Web、MCP、桌面端、部署和模型配置。

待验证点：

- 中文复杂业务流程的生成质量；
- 对销售、CRM、RPA、审批流等企业内部流程图的准确性；
- 生成 draw.io XML 的稳定性，以及复杂图二次编辑是否容易失真；
- 私有化部署时 API key、模型调用、日志与图表内容是否会泄露；
- MCP 与现有 Hermes / Cursor / VS Code 工作流的接入成本。

## 6. 对个人能力有什么价值

- 帮助把“口头需求”快速转成流程图/架构图，提升需求澄清能力；
- 训练自己用结构化语言描述系统边界、数据流和角色职责；
- 对写方案、做汇报、和业务方对齐都有直接帮助；
- 可作为学习 MCP 工具化工作流的轻量样本。

## 7. 对企业 AI 落地有什么价值

可优先考虑三个场景：

1. **ITBP 需求澄清：** 把业务方一句话需求转成流程图，再反问关键节点和例外情况。
2. **CRM AI 化方案表达：** 把自然语言操作 CRM 的用户旅程、权限边界、数据流转画成图，便于和业务/管理层同步。
3. **RPA/NC 闭环流程梳理：** 把错误发现、分流、处理、复核、沉淀 Playbook 的链路可视化，帮助识别自动化断点。

它更适合做“第一版图”和“共识草图”，不应直接作为正式制度流程或系统架构的唯一依据。

## 8. 可做的小实验

建议做一个 30 分钟小实验：

1. 选一个已知内部场景，例如“CRM 终端客户保护申请”或“NC/RPA 错误闭环”；
2. 用自然语言让 Next AI Draw.io 生成流程图；
3. 检查角色、系统、判断节点、异常分支是否准确；
4. 导出 draw.io 文件，验证是否能继续人工编辑；
5. 对比人工画第一版图的耗时和修改成本。

成功标准不是图好不好看，而是：是否能减少第一版方案图的时间，并帮助发现遗漏的业务节点。

## 9. 风险和边界

- **数据安全：** 不应直接输入客户名称、内部系统截图、真实业务数据或未脱敏流程细节到公开 Demo。
- **幻觉风险：** AI 可能补不存在的流程节点、角色或技术组件，需要人工校验。
- **权限边界：** 企业流程图可能暴露组织分工、系统结构和权限设计，公开仓库只能记录脱敏结论。
- **成本与稳定性：** 复杂图表依赖强模型，弱模型可能生成不稳定 XML 或不可用图形。
- **落地边界：** 它是表达和澄清工具，不是流程治理本身；不能替代业务确认和制度审批。

## 10. 当前结论

Next AI Draw.io 值得进入“可小实验”阶段。它对我们的价值不只是 AI 画图，而是把需求、流程、架构、权限边界这些难讲清楚的内容，快速变成可讨论、可编辑、可复核的图。

下一步优先用一个真实但脱敏的业务流程做小实验，验证中文流程理解、draw.io 可编辑性、私有化/本地模型接入和 MCP 工具链适配情况。
