---
title: Perplexity Portable Computer——全本地 Agent 运行时（云端只做用户门控的升级）
date: 2026-09-14
discovery_source:
  type: 抖音转述视频
  title: Perplexity 本地算力混合模式解析
  url: https://v.douyin.com/wj_Bg9LkBVs/
primary_object:
  type: commercial_product
  name: Perplexity Portable Computer (on NVIDIA DGX Spark)
  url: https://www.perplexity.ai/hub/blog/a-local-first-agent-for-private-and-cost-effective-knowledge-work
object_type: [commercial_product, trend_signal]
source_type: [官网, 群聊线索]
business_tags: [ITBP, 个人能力]
problem_tags: [流程提效, 知识沉淀]
method_tags: [Agent, 本地推理, 成本分层, Harness]
tool_tags: [Perplexity, Qwen3.8-27B, PPLX-27B, DGX-Spark, vLLM]
value_stage: 学习理解
risk_tags: [成本]
public_level: public
---

# Perplexity Portable Computer：云端当"升级顾问"，本地跑完整 Agent

## 1. 这是什么

2026-08-25 Perplexity 发布 **Portable Computer**：把 Perplexity Computer 的
**整套运行时——orchestrator LLM（编排）、subagent LLM（子任务执行）、
agent harness（工具调度）——全部跑在本地 NVIDIA DGX Spark 上**，默认无云端依赖。

- 本地模型：Qwen 3.8 27B（阿里 2026-08 中旬发布，Apache 2.0，17GB，视觉+工具调用）
  与 Perplexity 在其上后训练的 **PPLX 27B**；NVIDIA Nemotron 3.5 Lightning 即将支持，
  也可自带模型和推理服务（本地经 vLLM 推理）。
- 云端角色被压缩到一个点：只有任务需要 frontier 级推理时，orchestrator **先请求用户批准**，
  才把问题以纯文本指引形式发给云端模型；云端模型碰不到本地文件和工具。
  敏感文档（合同、人事档案）全程不出设备，PII 本地分类。
- 门槛：DGX Spark 约 **$4,679** 硬件 + 仍需 Perplexity Pro/Max 订阅；
  发布时仅支持 Linux（DGX OS/Ubuntu）。本地执行不消耗积分，只有走云端的部分计费。

## 2. 原始来源

- 发现入口：抖音转述视频（群内线索，二手）
- 资料本体（一手）：
  - Perplexity 官方博客《A Local-First Agent for Private and Cost-Effective Knowledge Work》
    https://www.perplexity.ai/hub/blog/a-local-first-agent-for-private-and-cost-effective-knowledge-work
  - Perplexity 官方 X 发布帖（2026-08-25）https://x.com/perplexity_ai/status/2092268398319481039
- 旁证：
  - Moor Insights 现场速记 https://moorinsightsstrategy.com/field-notes/perplexity-computer-goes-local-with-portable-computer
  - Simon Willion 对 Qwen 3.8 27B 的独立评测（2026-08-16）
    https://simonwillison.net/2026/Aug/16/qwen-38-27b
  - NVIDIA 开发者论坛真机反馈（独占显存、桌面共存困难）
    https://forums.developer.nvidia.com/t/382132

## 3. 核心观点 / 核心能力

1. **架构不是"云规划+本地执行"，而是"全本地+云端门控升级"**（见第 5 节对视频的纠正）。
   编排器、工具路由、调度器、持久任务队列都在本地，长任务不被云服务繁忙拖死。
2. **为小模型短板做协同设计**：Qwen 3.8 27B 标称 260K 上下文但 ~100K 后开始吃力，
   所以 Computer 保持核心系统提示和工具集很小、专用 skill 按需加载、长任务压缩陈旧上下文；
   常用连接器做成紧凑 CLI 而不是完整 MCP（省 token）。
3. **后训练针对 harness 而非通用对话**：PPLX 27B 就是把 Qwen 3.8 27B 往
   "在这个 harness 里干活"方向调，自家 53 任务 Local Knowledge Work Bench 上
   82.6% → 85.4%，且 token 消耗最少（520k vs Pi 681k、Hermes 634k）。
4. 典型适用：批量 PDF 处理、仓库迁移等大规模本地操作不产生 token 费用；
   隐私文档处理是主打卖点。

## 4. 我学到了什么

- **成本分层的产品化范式**：订阅费买的是"升级通道 + 编排品牌"，
  绝大部分执行的边际成本被甩到用户一次性购买的本地硬件上。
  这与本组"贵模型动脑、便宜模型动手"的分层是同一个结构，
  只是它把边界划在"本地 27B vs 云端 frontier"并加了用户审批闸门。
- **27B 级开源模型 + 专用 harness 后训练，正在成为 agent 落地的默认配方**：
  Qwen 3.8 27B（17GB、视觉、工具调用、Apache 2.0、NVFP4 在 24G 卡上 1.5x 提速）
  被 Perplexity、Pi、Hermes 等多家 harness 同时选为基准底座，
  "27B 是本地甜点尺寸"有独立社区证据（Simon Willison、HN），视频这点成立。
- **Harness 设计要倒过来迁就模型**：小工具集、skill 懒加载、上下文压缩、CLI 代替 MCP——
  本地 agent 省 token 的手段和云端多塞能力的思路相反。

## 5. 它是否可信，哪些需要验证

对视频转述的三处校正（以官方一手信息为准）：

1. **视频说"云端负责任务设定、高层规划与 Harness 分配，本地只执行"——方向说反了。**
   Portable Computer 的编排器和 harness **本身就在本地**；云端只在本地模型判断需要
   frontier 推理、且用户逐次批准后接收纯文本问题。云是"顾问"，不是"规划者"。
2. **"订阅只用于高层规划、执行全交本地"是趋势断言，不是当前事实。**
   目前订阅仍是入场券（Pro/Max 才能用），本地硬件还要 ~$4,679，双重付费；
   "硬件涨价是因为大家囤算力"是视频的宏观演绎，无证据支撑，DGX Spark 涨价与否
   受 GB10 供给等多因素影响，不能当结论。
3. **跑分是厂商自家内部榜单**：Local Knowledge Work Bench（53 任务）为 Perplexity 自建，
   对比对象 Hermes/Pi 的配置由其选取，称将开源但截至本文未见；
   ParseBench-100 差距（65.1% vs 34.6%/13.9%）同样出自其博客。
   趋势可信，具体数字待第三方复测。

待验证：真机独占显存问题（论坛反馈 vLLM 占满 DGX Spark 显存、与桌面应用难共存）
是否已在后续版本改善；PPLX 27B 后训练数据/蒸馏来源官方未细说。

## 6. 对个人能力有什么价值

- 自己的"驾驶舱"（贵模型规划 + 便宜模型执行）可直接借鉴它的三条工程手法：
  核心工具集保持小、skill 按需加载、长任务自动压缩上下文。
- 评估本地 agent 时，把"模型分数"和"harness×模型协同分 + token 消耗 + 单任务耗时"
  分开看（它的数据里 Pi 最快 176s、Computer 218s 但分更高）——选型是多维权衡。

## 7. 对企业 AI 落地有什么价值

- **数据不出域的 agent 形态有了商业样板**：合同、人事、客户数据处理时
  "本地全跑、云端只收脱敏文本且需审批"对制造业 IT 是可对合规/安全讲的架构，
  比纯 RAG 私有化部署轻、比全靠公有云稳。
- 但当前成本结构（近 5000 美元/台 + 订阅）决定它仍是试点极客盘，
  不具备全员铺开条件；企业更现实的路径是同构思想、自选硬件：
  一台 24–32G 显存的本地工作站 + Qwen 3.8 27B（GGUF/NVFP4）+ 现有 harness 即可复刻八成体验。
- 对 RPA/接口路线的关系：这仍是"桌面 agent"路线，脆弱性和可审计性问题不变，
  不改变本组"有正经接口走接口、agent 是补丁层"的判断。

## 8. 可做的小实验

- 在现有 Mac/本地 GPU 上用 Qwen 3.8 27B（Unsloth GGUF 或 vLLM NVFP4）
  挂一个轻 harness 跑 3–5 个本组真实知识工作任务，记录成功率/token/耗时，
  与云端模型基线对比（暂不动手，待明确批准）。

## 9. 风险和边界

- 成本：硬件 + 订阅双付；本地模型 100K 以上上下文质量衰减。
- 可信度：内部 benchmark 未开源，分数仅作参考。
- 运维：本地推理服务倾向独占机器，和日常桌面使用冲突。
- 锁定：PPLX 27B 为厂商后训练版本，可迁移性未知；Qwen 底座本身 Apache 2.0 无此问题。

## 10. 当前结论

视频抓住了真趋势（27B 本地底座 + 云端按需升级的混合算力），但把架构方向讲反了：
**Perplexity 的落地方案是"全本地 agent、云端做用户门控的升级顾问"，不是"云规划、地执行"。**
对本组的价值在架构思想与工程细节（小工具集、懒加载、上下文压缩、成本分层闸门），
不在采购该产品；27B 成为本地 agent 默认底座这一观察有多方独立证据，可作为后续选型基线。
