---
title: "DeepSeek V4 Pro 与国产开源模型围攻闭源旗舰——抖音研报速览与事实核验"
date: 2026-08-13
discovery_source:
  type: 短视频
  title: 口罩哥研报60秒：AI模型密集发布，DeepSeek V4 Pro低价围攻Claude
  url: https://v.douyin.com/DuYP27tYNPM/
primary_object:
  type: trend_signal
  name: DeepSeek V4 Pro / Qwen 3.8 Max / Grok 4.6 vs Claude Fable 5
  url: https://simonwillison.net/2026/Apr/24/deepseek-v4/
object_type: [trend_signal, article]
source_type: [抖音, 官网, 技术博客]
business_tags: [ITBP, 个人能力, 管理]
problem_tags: [流程提效, 知识沉淀, 成本]
method_tags: [Agent, 大模型, 开源, 长上下文, MoE]
tool_tags: [DeepSeek, Qwen, Claude, Grok]
value_stage: 待验证
risk_tags: [成本, 幻觉, 合规]
public_level: public
---

# DeepSeek V4 Pro 与国产开源模型围攻闭源旗舰

## 1. 这是什么

抖音"口罩哥研报60秒"的一条 60 秒行业速报，把近期三件事打包在一起：

1. xAI 发布 Grok 4.6，与 Claude Fable 5 做基准对比；
2. 阿里 Qwen 3.8 Max 宣布开源（8 月 3 日发布，权重下周开放）；
3. DeepSeek V4 Pro 正式版上线，主打低价 + 开源 + Agent 能力。

作者结论：DeepSeek 为代表的国产开源模型已经用"高性能 + 1/10~1/20 价格"对海外闭源旗舰形成围攻。

## 2. 原始来源

- 发现入口：抖音短视频 https://v.douyin.com/DuYP27tYNPM/ （"口罩哥研报60秒"）
- DeepSeek V4 官方发布与定价核验：
  - Simon Willison 实测与定价表：https://simonwillison.net/2026/Apr/24/deepseek-v4/
  - 技术解读：https://yudonglee.me/deepseek-v4-explained
- Qwen 3.8 Max 官方博客：https://qwen.ai/blog?id=qwen3.8
- 第一财经报道：https://www.sohu.com/a/1058643173_114986
- Grok 4.5/4.6 vs Fable 5 基准：https://benchlm.ai/compare/claude-fable-vs-grok-4-5

## 3. 核心观点 / 核心能力

**DeepSeek V4 Pro（2026-04-24 发布 Preview，MIT 开源）**

- 1.6T 总参 / 49B 激活 MoE，1M token 上下文，另有 V4-Flash（284B/13B）。
- API 定价：Pro $1.74/M input、$3.48/M output；Flash $0.14/M、$0.28/M。
- SWE-bench Verified 80.6%，与 Claude Opus 4.6 / Gemini 3.1 Pro / GPT-5.5 在 ±1.5pt 内。
- 官方定位 Agent：内部已用 V4 替换 Claude 做编码工作；支持 Responses API，可对接 Codex。
- 训练成本估算 $14-18M，约为同期闭源旗舰的 1/10。

**Qwen 3.8 Max（2026-08-03 发布）**

- 2.4T 总参 / 95B 激活 MoE，1M 上下文，原生多模态。
- 海外定价 $2/M input、$6/M output，隐式缓存 $0.25/M；国内 12 元 / 36 元。
- Max 级别首次开源，权重下周放 Hugging Face / ModelScope。
- 支持 `reasoning_effort` 参数，兼容 OpenAI 和 Anthropic 两套协议，可直接接 Claude Code 等生态。

**Grok 4.6 vs Claude Fable 5**

- 视频称"10 项评测 Grok 胜 3、Fable 5 胜 7"，目前公开可查的是 Grok 4.5 数据：Artificial Analysis 编码任务成本 $2.49 vs Fable 5 $11.80；SWE Marathon Grok 4.5 29% > Fable 5 24%；Terminal Bench 2.1 两者仅差 1 分。
- Fable 5 在 SWE-Bench Pro 仍领先（80.3% vs Grok ~75%），但单次任务成本是 Grok 的 4.7 倍。

## 4. 我学到了什么

1. **开源模型与闭源旗舰的差距已收敛到 3% 以内**（Mozilla《开源 AI 现状报告》），OpenRouter 上开源模型 token 占比从 2026 年 1 月 34% 涨到 6 月 65%，前五 API 消费模型全部是开源权重。选型决策的权重正从"用哪个模型"转向"怎么用模型、怎么配数据、怎么设计 Agent 系统"。
2. **真正的成本杀手是长上下文 + 缓存命中**。V4 Pro 在 1M 上下文下推理 FLOPs 只有 V3.2 的 27%、KV cache 10%，把"长上下文"从高端卖点打成基础设施。Agent 循环里 70% 以上 token 命中缓存时，实际单任务成本可以再压一个数量级。
3. **"Max 级开源"是分水岭事件**。阿里把过去闭源变现的旗舰直接开放，说明闭源溢价已经守不住，算力自主（平头哥 + 灵骏超节点）+ 开源生态成为新的竞争组合。
4. **Agent 编码场景的性价比差距比聊天场景更大**。Fable 5 单次编码任务 $11.80、Grok $2.49，差 4.7 倍；而 V4 Pro 按 token 单价算还要再低一档。对需要大量并行编码 Agent 的团队，模型选择直接决定商业模型能否成立。

## 5. 它是否可信，哪些需要验证

视频本身有多处数字错误或夸大，不能直接当事实引用：

| 视频说法 | 核验结果 |
|---|---|
| "SpaceXAI 推出 Grok 4.6" | 应为 **xAI**，不是 SpaceXAI。 |
| "DeepSeek V4 Pro 输入价是 Claude Fable 5 的 1/23、输出 1/57" | 按公开定价：Fable 5 $10/$50，V4 Pro $1.74/$3.48，实际约 **1/5.7 input、1/14.4 output**。1/23、1/57 两个数字对不上任何公开档位。 |
| "缓存命中输入 $0.003625/M tokens" | 公开数据：V4 Pro 缓存命中约 1 元/M（~$0.14），Flash 缓存命中约 0.2 元/M（~$0.028）。$0.003625 这个数字查不到对应来源，疑似口误或把某档折扣算错。 |
| "DeepSeek V4 Pro 整体超越 OPPO 4.8" | 应为 **Claude Opus 4.8**，"OPPO"是口误。 |
| "并发上限 500" | DeepSeek 官方定价页未明确公开这一数字，需以平台账户实际配额为准。 |
| "DeepSeek V4 Pro 正式版" | V4 于 2026-04-24 以 Preview 形式发布并开源 MIT；是否已脱离 Preview 需查 deepseek.com 当前模型列表确认。 |
| "Grok 4.6 胜 3 项、Fable 5 胜 7 项" | 公开可查的是 Grok 4.5 对比数据，4.6 的 10 项分项评测原始来源未在视频中标注，需追 xAI 官方或 Artificial Analysis 报告。 |
| "最大输出 384K tokens" | V4 官方文档标注 1M context，最大输出窗口需以 API 文档为准，未在 Simon Willison 摘录中看到 384K 这一数字。 |

基准测试数字也应注意：多数是厂商自报，第三方独立复测仍在进行；Artificial Analysis、SWE-Bench Pro、Terminal Bench 等不同榜单结果不一致，不能只挑对自己有利的一组。

## 6. 对个人能力有什么价值

- 建立模型选型的"单价 × 上下文 × 缓存命中率 × 任务完成率"四维对比框架，不再只看 benchmark 排名。
- 理解 MoE / 混合注意力 / KV cache 优化对实际成本的影响，能在和业务方讨论"AI 能不能大规模铺开"时算清账。
- 跟踪 Responses API、`reasoning_effort`、多协议兼容等 Agent 工程接口变化，判断我们自己的 Hermes / CRM AI / RPA 方案何时该切换底层模型。

## 7. 对企业 AI 落地有什么价值

1. **可以开始把"长文档 + 多轮 Agent"场景从闭源旗舰迁到国产开源 API**，尤其是日志分析、合同/招标文件批量阅读、CRM 长对话历史总结、RPA 流程文档比对这类高 token 低单次价值场景。
2. **编码 Agent / 自动化脚本批量跑**的成本可以下降 5-10 倍，意味着原来因为成本不敢做的"每个业务流程配一个 Agent 持续巡检"变得可行。
3. **数据合规与自主可控**：Qwen 3.8 Max、DeepSeek V4 都可在阿里云百炼 / 私有化部署，对 INTCO 这类有客户数据和海外业务的制造企业，比直接调 OpenAI / Anthropic 更可控。
4. **但不能盲目追新**：模型在编码、长程 Agent 上仍有失败率，且开源版本地部署需要 865GB（V4 Pro）级显存，实际自建不现实；对外报价、客户数据、生产流程相关场景仍需保留人工审核和回退。

## 8. 可做的小实验

1. 拿 3-5 个我们真实的 CRM 长对话总结 / RPA 日志分析任务，用同一组 prompt 在 V4 Pro、Qwen 3.8 Max、当前生产模型（豆包 / INTCO-Flash）上跑盲测，记录完成质量 + 实际 token 成本 + 耗时。
2. 在 Hermes 里给 V4 Pro 或 Qwen 3.8 Max 加一个低优先级 provider，把摘要、会话压缩、批量分类这类高容错任务切过去跑一周，看实际账单和失败率。
3. 对 Qwen 3.8 Max 的 `reasoning_effort` 参数做对照实验，验证"低 reasoning + 缓存命中"在简单分类任务上能否再省 50% 成本。

## 9. 风险和边界

- 短视频研报为了冲击力会凑整数、放大对比倍数，关键数字必须回到官方定价页和第三方复测核验，不能直接转发给业务方。
- DeepSeek V4 Flash 上线后曾因流量过大出现容量不足，开源模型 API 的 SLA 与并发稳定性仍弱于闭源大厂，生产关键链路要有 fallback。
- 模型权重开源 ≠ 可以随便商用二次分发，MIT 许可证允许，但阿里 / DeepSeek 的服务条款、数据使用政策、出口管制仍需单独确认。
- 国产模型在中文场景强，但英文合同、海外法规、多语种客服等场景仍需实测，不能假设"全面超越"。

## 10. 当前结论

这条视频指向的趋势是真的——开源旗舰已经在编码和长上下文 Agent 场景把性价比打到闭源模型的 1/5 到 1/15，Qwen 3.8 Max 开源和 DeepSeek V4 系列一起把"用什么模型"从战略问题降级成工程问题。但视频里多个具体数字是错的或夸大的，内部讨论时要带核验表用，不能直接引用视频原数据。对我们最实际的动作是：拿 3-5 个真实高 token 任务做一次国产开源 API vs 当前生产模型的盲测，用自己的数据决定哪些场景可以切。
