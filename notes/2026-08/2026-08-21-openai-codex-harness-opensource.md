---
title: "OpenAI 开源 Codex Harness：Agent 竞争下移到执行层"
date: 2026-08-21
discovery_source:
  type: 小红书
  title: "AI 亮晶晶《Codex Harness 正式开源！》图文帖"
  url: ""
primary_object:
  type: open_source_project
  name: "OpenAI Codex Harness（CLI / SDK / App Server）"
  url: "https://github.com/openai/codex"
object_type: [open_source_project, trend_signal, article]
source_type: [GitHub, 官网, 公众号, 小红书]
business_tags: [ITBP, 产品]
problem_tags: [流程提效, 知识沉淀, 组织协同]
method_tags: [Agent, Vibe Coding, 自动化, MCP]
tool_tags: [Codex, OpenAI, DeepSeek-Harness, App-Server, SDK]
value_stage: 学习理解
risk_tags: [数据安全, 国内可用性, 成本, 合规]
public_level: public
---

# OpenAI 开源 Codex Harness：Agent 竞争下移到执行层

## 1. 这是什么

OpenAI 于 2026-08-19 在开发者博客发布《Codex as a platform: build on the open agent harness》，把支撑 Codex（Web/CLI/IDE/桌面 App）的同一套底层"harness（执行框架）"作为开放集成面正式推出，包含三个开源组件：

- **Codex CLI**：命令行入口，`codex exec` 支持非交互式跑任务，也可交互运行。
- **Codex SDK**：官方编程接口，TypeScript 库为主，Python 库通过 JSON-RPC 控制本地 app-server；用于把 Codex 编进 CI/CD、内部工具和自有产品。
- **Codex App Server**：本地常驻 HTTP/stdio 服务，双向 JSON-RPC（JSONL over stdio 的 "JSON-RPC lite"），支持持久会话、实时事件流、diff 推送和人在回路审批（server 可主动发起 approval 请求并暂停 turn）。

代码仓库 `github.com/openai/codex`，Apache-2.0 协议，约 106.6k stars / 16.2k forks。核心 agent 逻辑在 `codex-rs/core`，App Server 在 `codex-rs/app-server`。

**关键澄清**：开源的是 harness 和集成层，不是 Codex 模型本身，也不是 ChatGPT 托管服务。跑 Codex 仍需 Sign in with ChatGPT（Plus/Pro/Business/Enterprise）或 OpenAI API key。社交帖"正式开源"的说法略有噱头——Codex CLI 仓库此前已是 Apache-2.0，8 月 19 日的重点是把 harness 作为稳定平台面（App Server 协议 + 官方 SDK）对外承诺，并配合合作伙伴落地。

## 2. 原始来源

- 发现入口：小红书"AI 亮晶晶"图文帖（2026-08-21，截图）。
- 资料本体（官方）：
  - https://developers.openai.com/blog/codex-as-a-platform （2026-08-19）
  - https://openai.com/index/unlocking-the-codex-harness （App Server 架构，2026-02-04）
  - https://github.com/openai/codex （Apache-2.0）
  - https://learn.chatgpt.com/docs/codex-sdk （SDK 文档）
- 对照：DeepSeek Harness（2026-08-13 发布 v0.1 developer preview，MIT，Node/TS，基于 Cordis，"Everything is a Plugin"）：
  - https://thenewstack.io/deepseek-harness-open-source-plugins
  - https://github.com/deepseek-ai/deepseek-harness

## 3. 核心观点 / 核心能力

1. **Agent = 模型 + Harness**。模型决定"智商"，harness 决定能不能安全、可靠、可控地把事做完。harness 负责：收集上下文、调用工具、在沙箱/权限边界内执行、请求审批、流式回传进度、跨多轮持久化工作。
2. **三个协议原语**（App Server）：`Item`（started/delta/completed 生命周期，承载用户消息、agent 消息、工具调用、审批请求、diff）、`Turn`（一次用户输入触发的完整工作单元）、`Thread`（可创建/恢复/fork/归档的持久会话，历史可重连渲染）。
3. **嵌入而非替代**已有产品界面。OpenAI 明确反对"万能聊天框"路线：产品继续拥有自己的 dashboard、记录和控件，harness 作为底层 agent loop，通过 MCP 工具读取/变更业务记录，在用户审批后执行动作；视图由产品侧刷新。
4. **已公开的落地模式**：GitHub/JetBrains 把 Codex 接进 IDE；Cisco 在 Cloud Control 的 App Builder 里用 Codex SDK；Thrive Holdings/Crete 的税务申报试点处理 7000 份申报，准备时间下降约 1/3。同一模式可推广到客服、运维、安全事件分诊、销售账户调研、营销活动。
5. **路线分野**：Codex Harness 偏生产级执行层，强调 sandbox/approval/可靠执行，但模型侧绑定 OpenAI；DeepSeek Harness 偏灵活调度层，模型/工具/沙箱/UI 皆插件、可自由替换、MIT 更宽松，但仍 v0.1 preview，接口会 breaking change。

## 4. 我学到了什么

- "Harness"这个概念被单独拎出来命名，说明 2026 年 Agent 工程的主战场已经从"调哪个模型"下沉到"执行循环怎么做"。上下文组装、工具权限、审批、中断恢复、事件流、会话持久化才是企业真正难抄的部分。
- App Server 的 Item/Turn/Thread 三层 + server 主动发起 approval 的双向协议，是一个比"request/response + function calling"更成熟的 agent 客户端协议范式，值得作为自研 agent 平台的参考架构。
- "嵌入已有系统界面 + MCP 取数 + 审批后执行 + 产品侧刷新视图"这条模式，和我们做 CRM AI 化时"在记录页里加 agent 能力、不另起聊天入口"的方向一致。
- 开源 harness ≠ 开源模型。Apache-2.0 的代码可商用可审计，但模型调用仍走 OpenAI，数据安全与供应商锁定要单独评估。

## 5. 它是否可信，哪些需要验证

- 已核验：官方博客日期、组件清单、仓库协议（Apache-2.0）、App Server JSON-RPC/stdio 架构、Item/Turn/Thread 原语、合作伙伴案例均来自 OpenAI 官方与 GitHub 仓库页面。
- 已核验：DeepSeek Harness 发布时间、MIT、Cordis、"Everything is a Plugin"定位来自 DeepSeek 官方 X 与 TheNewStack 报道。
- 待验证：
  - 36氪/新智元等中文二手稿提到的"ARC-AGI-3 上仅靠 harness 调整（保留 reasoning、压缩 context）大幅提分"等具体数据，未在官方博客摘录中直接看到，引用前需回查原文。
  - "Codex Harness 正式开源"的表述需限定：CLI 早已开源，8/19 是平台化承诺 + SDK/App Server 稳定面，并非首次放出全部代码。
  - 税务案例"7000 份 / 降 1/3 时间"为厂商口径，未见独立评估。

## 6. 对个人能力有什么价值

- 建立"agent 协议层"的认知框架：以后评估任何 coding agent / 企业 agent 产品，都可以拆成模型、上下文、工具、沙箱、审批、会话、事件流七层来看，而不是只看 demo。
- 可以直接读 `codex-rs/app-server` 的 README 和协议定义，学习一个工业级双向 agent 协议怎么设计（包括断线重连、item 生命周期、server-initiated request）。

## 7. 对企业 AI 落地有什么价值

- **直接对应 CRM AI 化**：在销售易/CRM 记录页嵌入 agent，用 MCP 工具读取客户、商机、工单数据，agent 给出下一步建议，关键写操作走审批后执行——Codex 官方博客的 Relay demo 就是这个模式的样板。
- **自建 agent 平台的参考**：如果 INTCO 要做统一的企业 agent 执行层，codex-rs 的 core + app-server 拆分、thread 持久化、approval 双向协议是可直接借鉴的架构；MCP 接入方式也和我们已有的 MCP 配置一致。
- **权限边界可被工程化**：harness 把 sandbox、approval、tool policy 作为一等公民，这和 CRM 权限治理（最小范围、审批、数据边界）是同一思路，可用于回答"agent 能不能动生产数据"的治理问题。
- **选型参照**：要灵活换模型、接受 preview 风险可关注 DeepSeek Harness（MIT、全插件化）；要生产级稳定性、且能接受 OpenAI 模型绑定，Codex harness 的架构参考价值更高。两者都不是"装上就能在企业内网跑"的方案。

## 8. 可做的小实验

1. clone `github.com/openai/codex`，重点读 `codex-rs/app-server/README.md` 和 `codex-rs/core`，整理一份"agent harness 七层清单"对照我们现有 Hermes/Claude Code/影刀的实现差异。
2. 用 `codex exec` 在一个非敏感 demo 仓库跑非交互式任务，观察事件流格式，和我们自己的 gateway 事件模型对比。
3. 起一个最小 App Server（stdio），手写客户端发 initialize + 一次 turn，抓 Item/Turn/Thread 的真实 JSONL 报文，作为自研协议的参考样本。
4. 评估 DeepSeek Harness 的 plugin 模型（models/tools/sandbox 皆可替换）是否适合做内网可替换模型的 agent runtime 原型。

## 9. 风险和边界

- **数据安全 / 合规**：Codex 模型调用走 OpenAI，客户数据、CRM 记录、生产代码不能直接送入；企业版需走 OpenAI Enterprise/数据处理协议，或只用其开源 harness 架构、对接内网模型。
- **国内可用性**：OpenAI 服务在国内直连受限，ChatGPT 登录与 API 调用都有网络与合规成本。
- **成本**：Sign in with ChatGPT 走订阅额度，API key 走 token 计费；长会话 + 工具循环的 token 消耗需要压测。
- **DeepSeek Harness 成熟度**：v0.1 preview，明确提示会有 breaking change，不适合现在 pin 到生产。
- **二手信息噪音**：中文社交流量稿有"震惊体"和未标注来源的数据，本笔记只采信官方博客、GitHub 和可追溯的技术媒体。

## 10. 当前结论

Codex Harness 开源是 2026 年 agent 工程从"卷模型"转向"卷执行层"的标志性事件。对 INTCO 的短期价值不在"立刻用 Codex 替我们写代码"，而在：(1) 提供了一个可审计的工业级 harness 架构参考；(2) 验证了"agent 嵌入已有业务系统 + MCP 取数 + 审批执行"的企业落地范式，和 CRM AI 化方向一致；(3) 给出了 App Server 这种双向协议的设计样板。模型仍绑定 OpenAI、国内可用性和数据合规是硬约束，实际试点建议从读源码、在非敏感仓库跑 `codex exec`、对比 DeepSeek Harness 插件架构开始，不直接接生产数据。
