---
title: "Harness Engineering：把 AI 装进“公司制度”里才跑得远"
date: 2026-07-26
discovery_source:
  type: 短视频
  title: "抖音 @Miss发行人（AI版）：当心！你的 AI 偷懒了！# vibecoding"
  url: "https://v.douyin.com/2vc4FORDgBs/"
primary_object:
  type: methodology
  name: "Harness design for long-running application development（Anthropic Engineering）"
  url: "https://www.anthropic.com/engineering/harness-design-long-running-apps"
object_type: [methodology, case_or_media, trend_signal]
source_type: [抖音, 官网]
business_tags: [个人能力, 管理, ITBP]
problem_tags: [流程提效, 知识沉淀, 组织协同]
method_tags: [Agent, Vibe Coding, Prompt, 自动化]
tool_tags: [Claude Agent SDK, Playwright MCP]
value_stage: 学习理解
risk_tags: [幻觉, 成本]
public_level: public
---

# Harness Engineering：把 AI 装进“公司制度”里才跑得远

## 1. 这是什么

一条抖音短视频用“你的 AI 为什么会偷懒/交差/跑偏”做引子，把 Anthropic 2026 年 3 月发布的工程文章 *Harness design for long-running application development* 重新打包成通俗概念——**Harness Engineering（驾驭工程）**：不是换更强的模型，而是给模型外面套一层“公司制度”（拆分任务、角色分工、交接与验收规则），让它能在几小时级别的长任务里稳定产出。

视频里的三角色（Planner / Generator / Evaluator）和“验收合同 / Sprint Contract”对应 Anthropic 原文的三代理架构，核心机制（context reset、独立 evaluator、按验收标准打分）在原文中都有对应，不是完全口胡，但术语是中文自媒体再包装。

## 2. 原始来源

- 发现入口：抖音短视频 https://v.douyin.com/2vc4FORDgBs/ （@Miss发行人 AI 版）
- 资料本体（一手）：
  - Anthropic Engineering, *Harness design for long-running application development*, Prithvi Rajasekaran, 2026-03-24  
    https://www.anthropic.com/engineering/harness-design-long-running-apps
  - 前作：*Effective harnesses for long-running agents*  
    https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents
  - 相关：*Effective context engineering for AI agents*
- 相关二手解读：Cole Medin（YouTube）、Ruh AI、Understanding Data、Shiplight 等均有拆解，术语“Planner/Generator/Evaluator”“Sprint Contract”在二手文章里被广泛使用。

## 3. 核心观点 / 核心能力

1. **模型 ≠ 产品，Harness 才是产品。** 长任务（几小时、跨多个 context window）失败，绝大多数不是模型不够聪明，而是没有外部约束。
2. **三代理分工（Planner / Generator / Evaluator）**
   - Planner：把一句话需求扩成产品蓝图 + 验收标准（原文附录 RetroForge 示例就是 planner 的输出）。
   - Generator：按蓝图一个 feature 一个 feature 实现，每个 sprint 签合同。
   - Evaluator：独立、刻意调得“挑剔”，用 Playwright MCP 真去点页面、截图、按分项打分，不通过就打回。
3. **两个关键故障模式和对应解法**
   - **Context anxiety（上下文焦虑）**：context 快满时模型会草率收尾。解法不是 compaction（压缩上下文），而是 **context reset**——新会话 + 结构化交接文档，像员工离职交接。
   - **自我评估宽松**：generator 自评永远给自己高分。解法是 evaluator 与 generator 物理分离，单独微调 prompt 让它“刻薄”，把主观审美（design/originality）拆成可打分维度。
4. **Sprint Contract**：每轮开工前 planner 与 generator 对齐“这轮做到什么算完”，验收标准必须是可测试/可打分的具体项（原文举例有 4 个加权维度，二手解读提到 27 条指标是放大后的说法）。
5. **结果**：frontend design 场景跑 5–15 轮迭代、单任务 up to 4 小时，全栈 demo 也能在多小时无人值守下跑完，但延迟、token 成本、编排复杂度明显上升。

## 4. 我学到了什么

- “prompt engineering → context engineering → harness engineering” 这条演进线是实的：先把 prompt 写好，再把上下文喂对，最后才是把**执行循环、角色、交接、验收**工程化。抖音把它总结成“给 AI 建公司制度”是非常接地气的类比。
- **独立 evaluator 比“让模型反思”有效得多**——这跟人做 QA 是一个道理：自己写的代码自己测，天然放水。
- **Context reset + 交接文档** 这个动作非常反直觉（明明可以压缩，非要重开会话），但原文明确说 Sonnet 4.5 上 compaction 解决不了“收尾焦虑”，这对我们自己跑长任务 agent 是直接可抄的做法。
- 别被“vibecoding 回来了”这种标题党带偏：Anthropic 的结论恰恰是**裸 vibe coding 跑不长**，必须有 harness 兜着。

## 5. 它是否可信，哪些需要验证

可信度较高：
- 一手来源是 Anthropic 官方工程博客，作者是 Labs 团队成员，附了具体架构、prompt 思路、Playwright MCP 实测和 demo 附录。
- OpenAI、社区（Cole Medin、Manus context engineering 文）在同一时期独立收敛到类似结论（planner-executor-reviewer、separation of generation from evaluation）。

需要打折扣/待验证：
- 抖音里“27 条具体验收指标”“Anthropic 的方案”这类绝对化表述是二手包装，原文是 4 个加权评分维度（design quality / originality / craft / functionality），指标数量是按场景自定义的。
- 原文实验场景是“Claude Agent SDK + 前端/全栈 demo + Playwright MCP”，**不等于**任何复杂业务任务（比如 CRM 实施、数据迁移、客户沟通）都能直接套三角色。
- 成本：单任务 4 小时、多轮迭代、evaluator 还要开浏览器截图，token 和时间成本在企业场景必须算账。

## 6. 对个人能力有什么价值

- 写 prompt / 搭 agent 时，先问自己三个问题，基本就是一套微型 harness：
  1. 这次任务拆成几步？每步交付物是什么？
  2. 谁来做、谁来验？（哪怕只有一个模型，也要把“执行 prompt”和“验收 prompt”物理分开跑两次。）
  3. 每步的“完成标准”能不能写成可检查的 checklist？
- 做长任务（比如让 Hermes 跑多步自动化、Codex/Claude Code 长时间改代码）时，主动设计 **交接文档 + 新会话接力**，不要指望一个 context 撑到底。
- 评估类工作（文档质量、方案 review、CRM 配置校验）单独开一个“挑刺角色”，不要让执行模型自己打分。

## 7. 对企业 AI 落地有什么价值

- **ITBP / 实施场景**：销售易 CRM 配置、数据清洗、报表搭建这类多步骤任务，可以把 harness 思路落成内部 SOP——不是做一个万能 agent，而是把“需求拆解→配置→验收用例→打回”变成固定环节，AI 在每个环节当助手，人在 evaluator 位做最终把关。
- **飞书自动化 / Hermes 工作流**：现有 cronjob、delegate_task、subagent 本身就是 harness 的零件，可以尝试给长任务加一个独立“验收子代理”，按预设 checklist 打分，不合格自动返工一轮再交人。
- **管理启发**：视频里“给 AI 搭公司制度”的类比反过来也成立——很多新人/外包交付翻车，本质也是缺 planner（方向）、缺 contract（验收标准）、缺独立 evaluator（QA）。AI 这套脚手架可以反哺团队管理话术。

## 8. 可做的小实验

1. **微型三角色脚本**：挑一个内部小任务（比如“把一篇会议纪要整理成待办 + 风险清单”），同一个模型跑三次——planner 出标准、generator 出稿、evaluator 按标准打分并打回，对比一把梭版本的质量差。
2. **Context reset 对照实验**：让 agent 连续处理 10 个相似 CRM 字段配置，一组用长上下文压缩，一组每 3 个任务 reset 一次 + 交接文档，看正确率和“偷懒收尾”出现频率。
3. **Hermes 侧加 evaluator 子代理**：在现有某条自动化流程（比如打捞处笔记生成）末尾串一个独立 reviewer，按固定 rubric 给分，低于阈值自动重写，观察是否能减少“AI 味”空话。

## 9. 风险和边界

- **成本与时延**：多角色、多轮、开浏览器验收 = token 和墙钟时间都翻倍，不能无脑套到所有场景；短任务单 agent 反而更划算（Anthropic 自己也建议 single agent 跑不动再升级）。
- **Evaluator 调不好就是形式主义**：如果评分维度太模糊、few-shot 没校准，evaluator 也会放水或乱杀，等于多烧钱没提升。
- **企业场景的“正确性”比 demo 场景硬**：CRM/ERP/财务场景有客观对错（字段映射错了就是错了），evaluator 必须接真实系统校验（API 查、页面点、数据对账），不能停留在“看起来对”。
- **不要把 harness 当万能框架**：原文针对的是 agentic coding/前端设计，照搬去做客户沟通、合同审阅这类高风险任务需要重新设计评估标准和人工兜底。

## 10. 当前结论

这条抖音抓的方向是对的，背后有 Anthropic 一手工程文章支撑，不是玄学。**核心 takeaway 就一句：长任务 AI 翻车，大概率不是模型笨，是没有 harness。** 对我们最直接可抄的三件事是：
1. 执行和评估分离，别让 AI 自己给自己打分；
2. 长任务主动 reset context + 交接文档，不硬撑上下文；
3. 每轮开工前先把“完成标准”写成可检查项，再动手。

下一步：可以在 8 月挑一个 Hermes 现有长流程（打捞处笔记/CRM 配置辅助）做一次 3 角色 harness 小实验，量化成本和质量收益再决定是否沉淀成 skill。
