---
title: Ornith-1.0：Self-Scaffolding RL 与 Agentic Coding 模型
date: 2026-07-12
discovery_source:
  type: 短视频线索
  title: 杨博士说AI：Ornith 经 TraceRL 和重构 harness 后
  url: https://v.douyin.com/T3OxyJGrZjY/
primary_object:
  type: open_source_model_family
  name: Ornith-1.0
  url: https://github.com/deepreinforce-ai/Ornith-1
object_type: [open_source_project, trend_signal, methodology]
source_type: [抖音, GitHub, 官网, HuggingFace, 群聊线索]
business_tags: [ITBP, 研发效能, AI落地, 个人能力]
problem_tags: [流程提效, 代码生成, 自动化验证, 工程可靠性, 知识沉淀]
method_tags: [Agent, RL, Harness, TraceRL, GRPO, 自动化]
tool_tags: [Ornith, DeepReinforce, Terminal-Bench, SWE-Bench, OpenClaw]
value_stage: 待验证
risk_tags: [幻觉, 数据安全, 权限, 成本, 评测污染, Reward Hacking]
public_level: public
---

# Ornith-1.0：Self-Scaffolding RL 与 Agentic Coding 模型

## 1. 这是什么

Ornith-1.0 是 DeepReinforce 发布的开源 agentic coding 模型家族，重点不是“又一个代码模型”，而是提出了 **Self-Scaffolding RL**：让模型在强化学习过程中不只生成解法，也生成/改进用于解决任务的 scaffold / harness。

它和普通 coding model 的区别在于：传统 agentic coding 常依赖人工写好的固定 harness；Ornith 的路线是把 scaffold 也变成可学习对象，让模型在任务、历史 scaffold 和反馈之间迭代出更适合当前任务的执行方式。

## 2. 原始来源

- 发现入口：打捞处群聊中的抖音短视频线索，标题提到“Ornith 经 TraceRL 和重构 harness 后”
- 短视频入口：https://v.douyin.com/T3OxyJGrZjY/
- GitHub：https://github.com/deepreinforce-ai/Ornith-1
- 官方文章：https://deep-reinforce.com/ornith_1_0.html
- Hugging Face collection：https://huggingface.co/collections/deepreinforce-ai/ornith-10

## 3. 核心观点 / 核心能力

1. **Harness 从静态工程配置变成可学习对象**  
   Ornith 的关键变化是：模型先提出/refine 一个 task-specific scaffold，再基于这个 scaffold 生成 solution rollout，最终 reward 同时回传给 scaffold 生成和 solution 生成两个阶段。

2. **Agent 能力不只取决于模型本体，也取决于执行外壳**  
   这与“harness engineering”的判断一致：同一个模型放在不同 tool loop、上下文压缩、验证机制、权限边界里，结果可能差很多。Ornith 把这个外壳的一部分纳入训练优化。

3. **对 reward hacking 做了专门防护设计**  
   官方提到三层防线：固定的信任边界、确定性 monitor、冻结 LLM judge。这个点非常关键，因为一旦模型能影响自己的训练 scaffold，就更容易出现“钻评测漏洞”而不是“真正解决问题”。

4. **开源模型开始追求 agentic coding 端到端能力**  
   官方给出 9B、31B、35B MoE、397B MoE 多个版本，并强调 Terminal-Bench、SWE-Bench、OpenClaw 等 agentic coding 评测成绩。是否可复现还需要独立验证，但方向本身值得关注。

## 4. 我学到了什么

这条资料真正有价值的地方，不是某个榜单分数，而是提醒我们：

- AI 编程能力正在从“模型会不会写代码”转向“模型 + harness + 工具 + 验证 + 安全边界”的系统能力；
- 企业内部做 AI Agent，不能只换模型，还要把任务拆解、上下文组织、工具调用、测试验证、权限控制做成稳定工程；
- 一个好的 Agent 系统，应该允许 workflow/harness 被持续优化，但关键边界必须由外部系统固定，不能让模型自己改验证标准、改权限边界。

## 5. 它是否可信，哪些需要验证

可信线索：

- 已追到 GitHub、官方文章和 Hugging Face collection；
- 官方 README 给出 OpenAI-compatible serving、tool calling、Hermes Agent / OpenHands / llama.cpp / Ollama / Unsloth 等接入方式；
- 官方对自举 scaffold、reward hacking 防护、异步 RL staleness weighting 给出较完整解释；
- 公开仓库显示项目以开源模型家族方式发布，具备进一步复现实验入口。

待验证问题：

- 榜单成绩是否能被独立复现，尤其是 Terminal-Bench / SWE-Bench 的具体运行条件；
- “TraceRL”在短视频语境里指向的具体实现和官方表述之间是否完全一致；
- 9B / 35B 版本在本地或企业内网场景中的真实 tool calling 稳定性；
- 是否存在 benchmark contamination、solution leakage 或 evaluator gaming；
- MIT / 模型权重 / 基座模型许可之间的商业使用边界，需要以 Hugging Face 模型卡和 license 文件为准。

## 6. 对个人能力有什么价值

这条资料适合用来训练三个判断能力：

1. **不要只看模型榜单，要看评测方法和执行外壳**。Agentic coding 的成绩高度依赖 harness、工具权限、时间限制、parser、上下文窗口和验证器。
2. **理解“自我改进”背后的工程边界**。凡是让模型改自己的 scaffold / workflow，都必须同时设计不可被模型篡改的 trust boundary。
3. **把 AI Agent 看成系统工程**。模型只是组件，真正稳定落地还要有任务环境、工具协议、日志、回放、验证、权限和风险监控。

## 7. 对企业 AI 落地有什么价值

对企业内部 AI 落地，它的参考价值主要在研发效能和自动化治理：

- **研发效能方向**：可以用来研究“模型 + 本地 harness + 测试验证 + Git 工作流”的组合，帮助内部脚本、CRM AI 化、Hermes 工程能力做更稳的代码修改和回归验证。
- **ITBP 能力方向**：需求不是直接部署 397B 模型，而是学习它把任务执行、验证、边界控制系统化的思路。
- **飞书 / CRM / RPA 自动化方向**：自动化不是让模型自由发挥，而是让模型在固定权限、固定数据边界和可回放日志里生成执行策略。

## 8. 可做的小实验

建议后续做一个不接内部真实系统的最小实验：

1. 选 Ornith-1.0-9B 或可用的量化版本，本地/隔离环境启动 OpenAI-compatible endpoint；
2. 接入一个最小 coding harness：只允许读写测试 demo repo；
3. 设计 5 个小任务：修 bug、补测试、改 CLI 参数、读日志定位失败、生成 patch；
4. 对比同一 harness 下 Ornith、Qwen、Claude/Codex 的表现；
5. 记录 tool calling JSON 稳定性、是否会绕过测试、是否会编造文件、是否能遵守权限边界；
6. 如果表现稳定，再考虑把“可学习/可迭代 scaffold”的思路抽象到 Hermes skill / playbook 设计中。

## 9. 风险和边界

- **不要把官方榜单直接等同于企业可用性**：coding benchmark 与内部真实系统、权限、依赖、历史代码质量差异很大。
- **防 reward hacking 是核心风险**：让模型参与改 scaffold 时，验证器、权限和数据边界必须在模型外部固定。
- **不要直接接生产代码和真实凭证**：必须先用公开 demo repo 或脱敏样例验证。
- **模型许可需二次确认**：开源宣传不等于所有权重、基座、衍生使用都无商业风险。
- **公开边界**：本笔记只使用公开资料和结构化判断，不包含群聊原文、内部系统、客户数据或未脱敏流程细节。

## 10. 当前结论

Ornith-1.0 值得继续跟进，价值重点在“self-scaffolding / harness 可学习化”这条路线。对我们更重要的不是马上替换现有模型，而是借它反推内部 Agent 工程：模型、工具、权限、验证、日志、回放、风险监控必须一起设计。

当前标记为“待验证”。下一步优先追 Hugging Face 模型卡和 license，并做一个隔离 demo repo 的最小 coding harness 对比实验。
