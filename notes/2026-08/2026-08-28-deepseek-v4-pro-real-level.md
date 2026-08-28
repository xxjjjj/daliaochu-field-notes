---
title: DeepSeek V4 Pro 真实水平画像（近 30 天社区实测 + Harness 匹配度）
date: 2026-08-28
discovery_source:
  type: 群聊线索
  title: 打捞处学习资料群——许晶晶要求用 last 30 days 口径研究 deepseek-v4-pro
  url: ""
primary_object:
  type: commercial_product
  name: DeepSeek-V4-Pro（含 0813 GA 正式版）+ DeepSeek Harness v0.1
  url: https://platform.deepseek.com/
object_type: [commercial_product, trend_signal, article]
source_type: [官网, 公众号, 博客, YouTube, Reddit]
business_tags: [ITBP, 个人能力]
problem_tags: [流程提效, 知识沉淀]
method_tags: [Agent, Vibe Coding]
tool_tags: [DeepSeek, Claude Code, Codex, DeepSeek Harness, 火山方舟AgentPlan]
value_stage: 待验证
risk_tags: [成本, 幻觉, 国内可用性]
public_level: public
---

# DeepSeek V4 Pro 真实水平：擅长什么、不擅长什么、在哪个 Harness 上表现最好

> 时间窗口：2026-07-28 ~ 08-28 的公开材料。V4 Pro 首发 4 月 24 日，8 月 12 日深夜静默上线 GA 正式版 `V4-Pro-0813`，8 月 13 日同步开源 DeepSeek Harness v0.1（MIT）。近 30 天的讨论全部围绕"0813 正式版 + 自家 Harness"这两件事。

## 1. 这是什么

- DeepSeek-V4-Pro：1.6T 总参 / 49B 激活的 MoE 推理模型，384 路由专家+1 共享专家，原生 1M 上下文、单轮最大输出 384K token，纯文本无视觉，MIT 开源。混合注意力（CSA+HCA）使 1M 上下文下单 token FLOPs 仅为 V3.2 的 27%、KV cache 仅 10%。
- 三档推理：非思考 / Think High / Think Max。官方跑分几乎全部来自 Max 档，Max 档 token 消耗极大（AA 智能指数一次跑分约 1.9 亿输出 token、花费 $1071）。
- DeepSeek Harness v0.1：DeepSeek 官方开源的 coding agent 运行时（Cordis 微内核 + 一切皆插件），模型无关（原生约 40 家厂商），完整事件流可回放/分叉，`dsh` CLI + 本地 Web UI，四种模式（standard/minimal/code/cordis）。官方基准（DeepSWE 66.9、Terminal-Bench 28.3）在 minimal 模式下跑出。

## 2. 原始来源

- 4sAPI 基准综述（DataLearner 2026 评测库数据）：https://blog.4sapi.com/blog/deepseek-v4-pro-benchmark-coding-llm
- DeepInfra 模型概览（含定价、幻觉率）：https://deepinfra.com/blog/deepseek-v4-pro-model-overview
- JavaGuide 一手实战（V4 Pro + Claude Code 三个真实任务）：https://www.cnblogs.com/javaguide/p/19929436
- 36 氪 Harness 实测与 0813 上线翻车复盘：https://m.36kr.com/p/3939428221205635
- CSDN/七牛云三 Harness 深度对比（截至 08-14）：https://adg.csdn.net/6a7fc36210ee7a33f29b1ea7.html
- 知乎 Harness 首发实测（V4 Pro+Codex vs V4 Pro+Harness 同任务对比）：https://zhuanlan.zhihu.com/p/2071615956632319784
- YouTube 抡锤者 0813 实测（Hermes / Codex / Harness 接入对比）：https://www.youtube.com/watch?v=OyQt9XzGBfM
- Reddit 负面反馈聚类：r/DeepSeek、r/LocalLLaMA、r/GithubCopilot 近 30 天多个热帖
- BenchLM 0813 分数页：https://benchlm.ai/models/deepseek-v4-pro-0813

## 3. 核心事实：擅长什么

**强项（benchmark + 多方实测一致）：**

1. **竞赛/算法编程是第一梯队天花板**：LiveCodeBench Max 93.5（120 个模型第一），Codeforces 3206 分，超过 Gemini-3.1-Pro High。特征是"从零造逻辑"强。
2. **数学/符号推理**：IMOAnswerBench 89.8、HMMT 2026 Feb 95.2、GPQA Diamond 90.1，国产模型里数学最强档。
3. **长上下文代码理解**：MRCR 1M 得 83.5；1M 上下文架构成本低，适合整仓/monorepo 一次性读入。
4. **Agent 工具执行**：Terminal Bench 2.0 67.9、BrowseComp 83.4，均为国产第一；GDPval-AA 1554 为开源权重模型第一。跨文件分析、工程方案生成被多个一手实战确认"上了台阶"。
5. **成本**：输出 $3.48/1M（0813 涨价后官方 API 约 $0.87 输入 / $3.96 高峰输出；缓存命中极低），约为 Claude Opus 4.7/4.8 输出价的 1/7~1/28；V4-Flash 输出 $0.28/1M，批量任务几乎没有对手。缓存命中率在长任务里实测可达 99%。

## 4. 核心事实：不擅长什么

1. **修脏活/陌生遗留仓库弱于造新代码**：SWE-bench Verified 80.6 与 Opus 4.6 持平，但 SWE-bench Pro（高难度企业仓库）只有 55.4，落后 GLM-5.1（58.4）和 Kimi K2.6（58.6），更远落后 Claude Opus 4.8（69.2）。一句话：**"建逻辑"强，"修烂摊子"弱**。
2. **广域知识/跨学科推理是明确短板**：HLE 48.2，落后 Kimi K2.6 近 6 分、GLM-5.1 4 分；人文、冷门科学、跨领域知识类任务应配检索或换模型。
3. **幻觉率高、不爱说"不知道"**：AA-Omniscience 基准幻觉率 94%——不知道时几乎总是硬答。置信度敏感场景必须加校验。
4. **文本细腻度差**：r/LocalLLaMA 高赞对比帖指出 V4（含 Flash）在"精读文本 nuance、精准概括原意、优雅措辞"上明显弱于 Gemma 4——研究摘要、文案润色不是它的活。
5. **复杂任务需要多轮调教**：JavaGuide 实测结论——复杂编码/精准问答/前沿科学推理与 Opus 4.6、GPT-5.5 有差距，DeepSeek 要调几次的活，Claude/GPT 一遍过；其个人体感编程能力 V4 Pro < GLM-5.1。
6. **纯文本模型**：无视觉，截图/UI 类任务必须配视觉模型。
7. **版本稳定性口碑差**：0813 GA 上线当天因前端接口/推理栈/权重配置错位翻车，公告撤下又重发；社区有"隔夜变笨""不遵守指令"的集中吐槽帖（部分可归因于上下文污染）。正式版发布即预告 8/17 起涨价数倍。

## 5. 在什么 Harness 上表现更好（近 30 天最有价值的信号）

多方实测指向同一结论：**同一个 V4 Pro，Harness 不同，表现差距很大，自家 Harness 明显占优。**

- **DeepSeek Harness（自家，v0.1 开发者预览）**：知乎实测同任务对比"V4 Pro + Codex 连线都对不齐" vs "V4 Pro + Harness 媲美 Claude Opus 5"；36 氪实测响应更快、缓存命中率比接 Claude Code 高 1 个百分点（98%→99%）；官方基准即在此框架 minimal 模式跑出。优势来自提示词格式、工具协议、缓存策略与模型联合调优。
- **Claude Code（Anthropic 兼容端点接入）**：可用、生态最成熟，但 V4 Pro 接进去"明显变差"（万字拆解帖与多个实测一致），且丢失 Anthropic 专有缓存/工具特性。JavaGuide 的三个真实任务在 CC 里能完成但复杂任务要多轮调教。
- **Codex（OpenAI Responses 格式）**：0813 原生支持 Responses API，接入零适配；但实测被 Harness 同题对比明显压制。
- **OpenCode / Hermes 等第三方 harness**：抡锤者实测提到换 OpenCode 表现与 CC 不同；Hermes 接入可完成基础任务，长线重活 Pro 相对 Flash 拉开差距。
- **重要副产物（方法论级）**：36 氪实测发现顶级模型 + 好 harness 时，**详尽提示词反而翻车，直白目标一次跑通**——"给模型写小作文"的时代在过去，提示词从"写剧本"变"定目标"。

选型口径：要 V4 Pro 的最佳表现 → DeepSeek Harness（但 v0.1 有破坏性变更、Bash 空循环卡死等已知 bug，不上生产关键路径）；要成熟稳生产 → Claude Code 里跑 Claude 模型，V4 Pro 只作成本敏感批量任务的备胎；要硬安全边界审外部代码 → Codex 内核沙箱。

## 6. 对我们（火山 Agent Plan 用户）的直接含义

- 我们 Plan 端点上已有 `deepseek-v4-pro`（抵扣高）和 `deepseek-v4-flash`（超低抵扣），无需额外采购即可实测。
- 与 Crystal 现有"贵模型动脑、便宜模型动手"工作流的匹配：V4-Flash 适合 Claude Code/harness 里的批量实现位（成本极低）；V4-Pro 适合算法/长上下文分析位，但**修遗留仓库、精准问答、跨领域研究不要指望它**，留给豆包/Kimi/Claude。
- Harness v0.1 值得在沙箱里自举一次（装 dsh → 跑它自己的仓库），评估其事件流回放/分叉对我们"飞书话题群当总线"多 agent 协作设计是否有借鉴价值。

## 7. 待验证（不替实测下结论）

1. 火山 Plan 端点的 deepseek-v4-pro 与官方 0813 是否同版本、Think Max 档是否开放、工具调用稳定性如何——需真实调用探测。
2. V4 Pro 接 DeepSeek Harness vs 接 Claude Code（Anthropic 协议）在我们真实仓库上的差距是否如社区所说那么大。
3. 0813 涨价后经 Plan 抵扣的实际成本曲线。
4. "长提示词翻车、短目标跑通"在我们 harness bridge 文档总线模式下是否成立。

## 8. 风险和边界

- 幻觉率 94%：无校验不得用于事实性产出。
- Harness v0.1 明确"开发者预览、将有破坏性变更"，禁止上生产关键路径；danger-full-access 模式只在可丢弃容器用。
- 模型版本波动风险：GA 上线即翻车 + 社区"隔夜变笨"反馈，生产引用需锁版本。
- 本文 benchmark 数字多来自厂商技术报告（Max 档），与日常默认档体感有差距；二手中文测评含营销成分（如七牛云导流文）。
