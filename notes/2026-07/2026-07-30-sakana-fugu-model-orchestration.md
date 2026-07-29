---
title: "Sakana AI Fugu — 从自然启发的模型编排系统"
date: 2026-07-30
discovery_source:
  type: 抖音视频
  title: 晓辉博士「当AI学会了管理」
  url: https://v.douyin.com/84Pr3FxigxQ/
primary_object:
  type: open_source_project
  name: Sakana Fugu
  url: https://github.com/SakanaAI/fugu
object_type: [open_source_project, research_paper, product]
source_type: [抖音, GitHub, arxiv, 技术报告]
business_tags: [ITBP, 业务赋能]
problem_tags: [模型选型, Agent 编排, 成本优化]
method_tags: [多智能体, 模型编排, 强化学习, 进化算法]
value_stage: 可用观察
risk_tags: [延迟, 供应商锁定, 自评基准]
public_level: public
---

# Sakana AI Fugu — 从自然启发的模型编排系统

## 1. 这是什么

Sakana AI（日本）推出的 Fugu（河豚）系列模型，核心不是自己回答，而是**训练一个"编排模型"来调度池子里多个前沿 LLM 协同工作**。对外暴露为一个标准 OpenAI 兼容 API，内部却是一个完整的多智能体编排系统。

两个变体：
- **Fugu**：快速模式，每次只选一个最优 worker，延迟接近直接调用
- **Fugu Ultra**：深度模式，多 agent 协作，适合复杂多步任务

开源仓库：[github.com/SakanaAI/fugu](https://github.com/SakanaAI/fugu)，技术报告：[arxiv 2606.21228](https://arxiv.org/abs/2606.21228)

## 2. 核心观点

**不做单一超级模型，而是训练"模型的经理"。**

Fugu 的核心差异在哪：它不是静态路由规则，而是通过强化学习和进化算法训练出的编排策略，能动态决定用什么模型、分配什么角色、怎么通信、怎么合成结果。这跟我们现在手写 Multi-Agent 流程的最大区别——编排本身变成了可训练的能力。

两篇 ICLR 2026 论文支撑：
- **Trinity** (arxiv 2512.04695)：0.6B 小模型通过进化策略学习在简单任务上分配 Thinker / Worker / Verifier 三角色
- **Conductor** (arxiv 2512.04388)：7B 模型通过 RL 学习自然语言协调策略，设计 agent 间通信拓扑

## 3. 基础技术指标

| Benchmark | Fugu Ultra | Fugu | Claude Fable 5 | GPT-5.5 |
|---|---|---|---|---|
| SWE-Bench Pro | 73.7 | 59.0 | - | 58.6 |
| LiveCodeBench | 93.2 | 92.9 | 89.8 | 85.3 |
| GPQA-Diamond | 95.5 | 95.5 | - | 93.6 |
| Humanity's Last Exam | 50.0 | 47.2 | 53.3 | 41.4 |
| TerminalBench 2.1 | 82.1 | 80.2 | - | 78.2 |

两点注意事项：(1) 这些是 Sakana 自评，非独立审计；(2) Humanity's Last Exam 上 Fugu Ultra 50.0 vs Opus 4.8 的 49.8，差距在误差范围内。

## 4. 对 ITBP / 业务的可用点

**第一层：业务价值**

Fugu 的模式非常适合 INTCO 这种"多个供应商模型共存 + 按任务特性选最优路径"的场景。我们现在已经在用多模型路由（不同任务用不同模型），但路由规则是手写的。Fugu 验证了一条关键路径：**编排能力可以通过训练而不是手写规则获得**。这降低了维护成本，也能捕捉人不容易发现的协作模式。

三个直接启发：
1. 我们现在对飞书消息处理、CRM 查询、代码审查等场景的选择模型策略，可以考虑从"静态规则路由"演进为"学习型路由"
2. Fugu 的"池子可配置、无需重训"设计，意味着新增模型（如国内模型接入）成本很低
3. 成本结构清晰：按顶层模型定价，不叠加池子里所有模型的费用——对我们这种多任务混合场景有参考价值

**第二层：IT/BP 自身能力**

对 Hermes 当前架构的直接对照——我们现在已经在做 agent 协作（多个 Skill、子代理、工具链），但 orchestrator 是隐式的：靠 system prompt + Skill 指令 + 人工经验。Fugu 的思路是把 orchestrator 显式化、可训练化。我们现在未必需要训自己的 orchestrator，但这个方向值得持续跟踪。

**第三层：通用认知**

这是 AI 行业从"单模型竞赛"转向"系统思维"的标志性案例。Sakana 的创始人之一是 Llion Jones（Transformer 论文作者之一），另一位来自 Google Brain 做复杂系统研究——两人把"自然群体智能→模型编排"这条线做成了产品。比 Fugu 本身更重要的是，它证明了：**编排能力本身就是一个可优化的维度，不一定要堆更大的模型。**

## 5. 源码 / 工具线索

- GitHub: https://github.com/SakanaAI/fugu — 102 commits，含 configs/demos/docs/scripts 目录，README 提供了 Codex 和 Claude Code 的一行安装命令
- 技术报告: [arxiv 2606.21228](https://arxiv.org/html/2606.21228v1)
- Trinity 论文: [arxiv 2512.04695](https://arxiv.org/abs/2512.04695)
- Conductor 论文: [arxiv 2512.04388](https://arxiv.org/abs/2512.04388)
- 深度分析文章: [teamcopilot.ai](https://teamcopilot.ai/blog/sakana-ai-fugu-explained-how-the-multi-agent-model-orchestrates-frontier-llms)、[outcomeschool.com](https://outcomeschool.com/blog/decoding-sakana-fugu)、[datacamp.com](https://www.datacamp.com/blog/sakana-fugu)

## 6. 风险与待验证

1. **自评基准风险**：所有 benchmark 数据来自 Sakana 自评，没有独立第三方审计。部分差距极小（HLE 差 0.2），实际差异可能不显著。
2. **延迟问题**：Fugu Ultra 走多 agent 协作，延迟明显高于单模型调用，不适合实时 UI 场景。
3. **编排黑箱**：Sakana 不暴露内部路由逻辑，你看到结果但不知道选了哪个模型、为什么选它。对需要审计和可解释性的场景不利。
4. **地区限制**：Fugu API 目前不支持 EU/EEA；国内用户能否直接调用待确认。
5. **模型锁定风险**：虽然池子可配置，但 orchestrator 的训练依赖特定 frontier 模型的表现分布。当池子里模型升级或替换时，编排策略可能需要重训。Sakana 声称会持续更新，但这意味着依赖他们的更新节奏。
6. **SciCode 反常**：在部分 benchmark 上，Fugu（快速版）反而比 Fugu Ultra 得分高——更多编排不总是更好。

## 7. 后续行动

- **近期可做**：用 Fugu 思路审视 Hermes 当前的模型选择和 agent 编排策略，看是否有可以从"硬编码规则"转为"数据驱动/学习型路由"的切入点
- **需要条件**：如果要在 INTCO 环境落地类似架构，需要：(1) 国内可调用的 model pool（通义/DeepSeek/文心等）；(2) 足够多的真实任务数据来评估路由效果；(3) 延迟预算评估
- **长期观察**：Fugu 是否能真正改变"模型编排是一个产品而非学术项目"的格局；OpenAI、Anthropic 等是否会推出类似编排产品
