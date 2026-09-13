---
title: OpenAI Agents API——Codex Harness 托管化，一次 API 调用跑起云端长任务智能体
date: 2026-09-14
discovery_source:
  type: 群聊线索
  title: 打捞处群内转杨博士讲解视频的文字总结（无原始视频链接）
  url:
primary_object:
  type: commercial_product
  name: OpenAI Agents API
  url: https://openai.com/index/introducing-the-agents-api/
object_type: [commercial_product, trend_signal]
source_type: [官网, 视频二手总结, 媒体报道]
business_tags: [ITBP, 产品]
problem_tags: [流程提效, 知识沉淀]
method_tags: [Agent, MCP, 多智能体, Harness, 沙箱]
tool_tags: [OpenAI Agents API, Codex]
value_stage: 待验证
risk_tags: [数据安全, 合规, 国内可用性, 成本]
public_level: public
---

# OpenAI Agents API：Codex Harness 托管化

## 1. 这是什么

OpenAI 于 **2026-09-10** 发布的 Agents API（public beta）：把驱动 Codex / ChatGPT for Work 的那套 agent harness（agent 循环、编排、上下文管理、沙箱基础设施）整体托管，开发者一次 API 调用（`client.beta.agents.sessions.create`，指定任务、模型、工具、环境）即可创建可持续运行数小时到数天的云端智能体，无需自己部署运行环境和编排层。无额外平台费，按 token、工具调用和容器时长付费。

群内线索是"杨博士讲解视频"的二手文字总结。已用 OpenAI 官网公告与官方文档核对，视频总结的主干准确，但有两处偏差（见第 5 节）。

## 2. 原始来源

- 官方公告（2026-09-10）：https://openai.com/index/introducing-the-agents-api/
- 官方文档：https://developers.openai.com/api/docs/guides/agents-api/overview
- 开源底层：https://github.com/openai/codex（harness 核心逻辑开源可见，托管版由 OpenAI 运维）
- 媒体二手（含客户数据与限制整理）：https://www.marktechpost.com/2026/09/10/openai-launches-the-agents-api-in-public-beta-putting-the-codex-harness-behind-one-api-call
- OpenAI Devs X 公告：https://x.com/OpenAIDevs/status/2098130570048045453

## 3. 核心观点 / 核心能力

**四个基本概念**：Agent（模型+指令+工具+MCP server）、Environment（可选沙箱：文件、技能、命令执行）、Session（持久化智能体实例）、Events/Items（流式或 webhook 跟踪进度，可中途 steer）。

**Harness 内置的四件事**（以前都要开发者自己写）：

1. **自动上下文压缩（compaction）**：session 接近上下文窗口上限时自动压缩早期历史，跨多个上下文窗口连续跑，开发者不用自己写摘要逻辑。
2. **工具搜索（tool search）**：不再把全部工具定义塞进上下文，按需动态加载相关工具定义——降 token、保缓存；配合 programmatic tool calling 可并行调用、链式执行、在代码里过滤/合并结果后只把相关结果带回上下文。支持 MCP、自定义函数、web search 等内置工具。
3. **多智能体并行（subagents）**：主 agent 自动把复杂任务拆给独立上下文的子 agent 并行执行（官方示例：事故调查拆成部署/错误/依赖三路分析），`max_concurrent_subagents` 可控，编排由平台负责。
4. **持久会话与恢复**：长任务、跨天 session、中断恢复由平台管理。

**三种执行环境**：OpenAI 托管沙箱（可挂文件、包、skills、plugins）／自托管（在自己环境跑 `codex exec-server`，受限密钥注册、仅出向 WebSocket 连接）／9 家伙伴沙箱（Blaxel、Cloudflare、Daytona、DigitalOcean、E2B、Modal、Oracle、Runloop、Vercel），也可不用沙箱。

**商业逻辑**：Codex harness 开源（让人看懂核心逻辑、建立生态）→ Agents API 卖官方托管运维+工具对接，按用量计费。与 Agents SDK（跑在你自己应用里）、Responses API（自己管历史和编排）构成三档：集成成本低/中/高，状态托管分别是平台/自己/手动。

## 4. 我学到了什么

- "Harness 层"正在从各家长任务实践里沉淀出来，变成与模型解耦、随模型版本一起发布的独立产品层。模型每升级一次，harness 由 OpenAI 同步维护，应用不用重写——这正对应我们自己踩过的坑：上下文压缩、工具膨胀、子代理编排、长任务恢复，Hermes 体系里都是手搓的，且换模型/加能力时要反复改。
- **工具搜索**直接印证了"注意力是瓶颈"：工具定义全量塞上下文 = 每个 agent 都带着全公司的 SOP 上班；按需加载才是规模化方向。我们 Skill 的按需加载与这个思路同构，差距在"检索匹配"是否由平台模型化完成。
- 官方对 multi-agent 的适用边界写得很克制：任务能切成**相互独立**的工作流才用（分头调研、对比方案、独立模块实现、并行排查不同故障因）——与我们 delegate_task 的使用纪律一致，不是什么任务都该多 agent。
- 自托管沙箱"仅出向连接"的设计，是企业内网接云智能体的关键合规姿势：不用开入向端口。

## 5. 它是否可信，哪些需要验证

可信部分：产品发布、能力清单、环境选项、计费口径均来自 OpenAI 官网与开发者文档，事实成立。

二手视频总结的两处偏差（已核对修正）：

1. **生态伙伴不是"付费增值插件/免配第三方服务"**：9 家是**沙箱（算力/运行环境）提供商**的一等集成，解决"agent 的代码在哪跑"（含 VPC 内部署、GPU/存储选项），不是替你配好第三方工具。伙伴沙箱各自按容器时长等计费。
2. **"封装 Codex AppServer"表述不精确**：Agents API 是托管整个 Codex harness（agent loop + 编排 + 上下文 + 恢复），App Server 只是 Codex 的组件之一；自托管模式才是跑 `codex exec-server` 接入。

待验证：

- 客户效果数字（Ciridae 评分 0.71→0.85、SafetyKit 单案成本降 60%、Hypha 失败响应降 86%）均为**厂商自报**，非独立基准，不能当实证引用。
- 国内可用性、延迟、企业采购通道未验证；当前数据驻留仅美国、不支持 ZDR（Zero Data Retention），受监管/涉客户数据场景直接不可用。
- 自动压缩的信息保真度（长任务里早期关键约束会不会丢）、工具搜索的召回准确率，均需实测。

## 6. 对个人能力有什么价值

- 这是观察"harness 产品化"走向的标杆样本：我们自己在做的上下文管理、技能检索、子代理编排，都可以拿它的公开文档当对照系，看官方把哪些机制做成了默认、暴露了哪些参数（如 `fork_turns` 控制向子代理传播多少上下文、`max_concurrent_subagents`）。
- 可以读开源的 github.com/openai/codex 中 compaction 与 tool-search 的实现，作为我们后续优化 Hermes 长任务压缩和技能匹配的参考实现。

## 7. 对企业 AI 落地有什么价值

- **短期（英科场景）不具备直接落地条件**：数据驻留仅美国、无 ZDR、境外服务，CRM/客户/生产数据不能进。
- 间接价值在架构参照：集团/工厂若要私有化长任务智能体，"开源 harness 内核 + 自托管 exec-server（仅出向）+ VPC 内沙箱"这套拓扑是可借鉴的企业内网形态；国内对标可关注火山/阿里等是否推出同构托管 harness。
- 对"RPA 是补丁、接口优先"的既有判断无冲突：Agents API 的程序化工具体系仍以 MCP/函数调用（正经接口）为主，沙箱内执行代码是兜底，不是把 UI 自动化扶正。

## 8. 可做的小实验

- 只读公开文档，梳理一张《Agents API harness 机制 ↔ Hermes 现有机制对照表》（compaction / tool search / subagents / session recovery 四行），输出"我们缺什么、哪些值得抄"——不涉及任何内部数据，随时可做。
- 用非敏感公开任务（如纯公开网页调研）申请 API 实测一次长 session 的自动压缩与子代理行为，观察 token 成本；需晶晶确认后再动（会产生外部账号与费用）。

## 9. 风险和边界

- 数据安全/合规：beta 期 US-only 数据驻留、无 ZDR，内部与客户数据严禁接入。
- 成本：长 session + 并行子 agent + 容器时长叠加，厂商公布的降本数字为自报；真实成本需实测。
- 厂商锁定：session 状态、skills/plugins 配置托管在 OpenAI，迁移成本高。
- beta 质量：公告明确会快速迭代、API 可能变。

## 10. 当前结论

视频总结主干准确，产品本体已核实：OpenAI 把 Codex harness 变成托管服务，内置上下文压缩、工具搜索、多智能体编排和持久会话，代表了 agent 基础设施"harness 层产品化、与模型解耦"的明确方向信号。对我们当前是**认知与架构参照价值**，不是可直接采用的工具——数据驻留和 ZDR 限制挡住企业场景，保持观察，优先抄它的机制设计而非接入服务本身。
