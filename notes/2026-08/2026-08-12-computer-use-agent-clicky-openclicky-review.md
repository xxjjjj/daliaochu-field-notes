---
title: "Computer Use Agent 实测对比：heyclicky vs openclicky（屏幕级跨应用操作）"
date: 2026-08-12
discovery_source:
  type: "抖音短视频（公开博主评测），本体经官网与 GitHub 核验"
  title: "当 AI 开始操作整台电脑，贾维斯真的在路上了？"
  url: "https://v.douyin.com/-LkBOb_7mII/"
  author: "赛博老徐折腾AI（2026-06-11 发布）"
primary_object:
  type: "software_category_review"
  name: "Computer Use / 桌面级 AI Agent（heyclicky 与 openclicky 对比）"
  url: "https://www.heyclicky.com"
object_type:
  - commercial_product
  - open_source_project
  - case_or_media
source_type:
  - 群聊线索
  - 官网
  - GitHub
business_tags:
  - 流程提效
  - 数据搬运
  - 自动化
problem_tags:
  - 跨系统操作
  - 权限与安全
  - 模型可控性
  - 结果验收
method_tags:
  - Computer Use Agent
  - Voice-to-action
  - 动态风控
  - 开源替代
tool_tags:
  - heyclicky
  - openclicky
  - Clicky
  - farzaa/clicky
  - gpt-5.5
value_stage: 已追源（官网+GitHub 已核验；博主实测结论待自测复核）
risk_tags:
  - 数据安全
  - 账号风控
  - 权限放大
  - 模型误判
  - 成本
  - 效果口径
public_level: public
status: "已核验两款产品本体与开源仓库；博主实测为单任务样本，准确率结论需自行 PoC 复核。"
---

# Computer Use Agent 实测对比：heyclicky vs openclicky（屏幕级跨应用操作）

## 1. 这是什么

一条抖音博主评测：实测两款"让 AI 看见整块屏幕、跨应用操作整台电脑"的桌面级 Agent。普通 AI 在聊天框里给答案，Codex/Claude Code/Cursor 这类 Coding Agent 在固定工作区内干活，而这两款产品更进一步——AI 直接操作 macOS 桌面：自己打开应用、建笔记、填数据。本质是 Computer Use（视觉驱动 GUI 自动化）产品的消费级落地。

## 2. 原始来源与本体核验

- 发现入口：抖音短链视频（博主：赛博老徐折腾AI，2026-06-11）。
- **heyclicky（视频称"heyclick"）**：[官网 heyclicky.com](https://www.heyclicky.com) —— Farza Majeed（buildspace 创始人，YC S20，a16z 背景）的商业产品，macOS 菜单栏 AI 伙伴（Sonoma 14.2+，Windows 排队中）；免费起步，Pro $20/月，Max $100/月（约 1000 条 agent 消息/月）；屏幕感知、语音优先、指向 UI 元素、实时流式（AssemblyAI + Claude）。
- **openclicky**：[GitHub jasonkneen/openclicky](https://github.com/jasonkneen/openclicky) —— Clicky 的开源版（MIT），Swift 原生 macOS 应用，471 stars / 91 forks / 253 commits（活跃维护）；本地配置，无托管 key 同步，模型可选（本地 secrets：ANTHROPIC/OPENAI/ELEVENLABS）；screen-aware 协议 `[POINT:x,y:label]` / `[TYPE:x,y:label]`；child worker 子任务；Composio MCP 的 GitHub 集成；computer-use 仅作为 last-mile fallback；另提供 OpenClickySDKSession 供宿主应用内嵌。
- 上游开源项目：farzaa/clicky（原版，MIT），另有 Windows 移植版 clicky_windows。
- 已核验事实：两款产品本体、定价、开源许可、依赖与架构均可从官网/GitHub 直接追溯；视频转写存在口误（"fable"应为 Claude 类模型，"gptrealtime二点一"是实时语音模型而非执行模型）。

## 3. 核心观点

1. **同一任务、同一材料，商业版与开源版结果差异明显**：heyclicky 后台自动跑、能标注"该看哪/该点哪"，但出现店名写错、添加原文没有的内容；openclicky 用博主自选模型（gpt-5.5），64 秒完成建笔记任务，标题、三项数据、行动全部对上且没有多写。
2. **模型不可选是商业版的信任短板**：heyclicky 的模型路由不透明（普通任务与复杂任务分属不同模型），"权限给得很大，智力却没配套"。
3. **动态风控是 Computer Use 的核心设计原则**：低风险任务让 AI 自动做，高风险任务停下来问人；安全不是把门焊死，便利也不是把钥匙全交出去。
4. **方向被验证，边界仍明确**：目标明确、材料明确的单步任务可跑通；多窗口、模糊上下文的复杂任务仍会出错。每一步都要有确认、验收、失败恢复。

## 4. 对业务与团队的价值

- **跨系统数据搬运是真实痛点**：销售/运营/财务经常在 OA、业务系统、CRM、报表之间来回打转。Computer Use Agent 对应"没有 API 的老系统"这类 RPA 脚本难以覆盖的长尾场景——视觉驱动替代脚本驱动，不怕按钮 ID 变化。
- **与现有 RPA（来也/影刀）互补而非替代**：RPA 是确定性脚本自动化，适合高频稳定流程；Computer Use Agent 是非确定性自动化，适合低频、多变化、界面复杂的任务。评估顺序应该是：有 API 走 API → 有稳定流程走 RPA → 长尾界面操作才考虑 Computer Use。
- **评测方法论可复用**：同一任务、同一材料，分别测商业版与开源版，对比准确性、耗时、模型可控性与成本——这套 A/B 实测法可用于我们评估任何 AI 工具。
- **对我们自己的 computer-use 能力是市场侧实证**：Hermes 已有桌面控制（computer-use skill）能力，本篇提供了一线用户的失败模式样本（写错、编造、多窗口失焦）和产品级验收标准。

## 5. 风险与待验证问题

- 博主单任务样本，不能代表整体准确率；"gpt-5.5 64 秒全对"需自行 PoC 复核。
- heyclicky 商业版模型路由黑盒、月消息量限流（Max 档 $100/月），成本与可控性需评估。
- openclicky 自带 API Key，按量付费成本自担；仅支持 macOS 14.2+；本地 secrets 管理不当有泄露风险。
- Computer Use 类工具权限极大（屏幕内容、键鼠、剪贴板、跨应用操作），企业内部使用涉及数据安全、误操作与责任边界，不得直接放开给业务用户。
- 屏幕内容会发送给第三方模型，涉密窗口/客户数据不得在未受控环境下使用。

## 6. 后续行动

- 若需评估：用沙箱 Mac + 自选模型跑 openclicky，以"建笔记→填报表→跨应用搬运"三个受控任务做 PoC，记录准确率、耗时与 token 成本。
- 关注 heyclicky Windows 版与 Clicky 生态（farzaa/clicky 上游、clicky_windows 移植）。
- 将"动态风控"三原则（低风险自动、高风险确认、失败可恢复）沉淀为 AI 自动化类项目的通用验收框架。

## 7. 关联项目

- 打捞处资料研究（daliaochu-field-notes）
- Hermes computer-use / 桌面自动化能力评估
- RPA（来也/影刀）与 AI Agent 的边界梳理
