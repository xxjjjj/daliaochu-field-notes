---
title: "Cumora：AI Agent 作为一等公民的团队聊天工作区"
date: 2026-08-30
discovery_source:
  type: 群聊视频线索
  title: 视频介绍"人与 AI 融合的团队协作平台 Cumora（库莫拉）"
  url: https://cumora.ai
primary_object:
  type: open_source_project
  name: yetone/cumora
  url: https://github.com/yetone/cumora
object_type: [open_source_project, commercial_product, trend_signal]
source_type: [官网, GitHub, 群聊线索]
business_tags: [ITBP, 个人能力, 管理]
problem_tags: [组织协同, 流程提效, 知识沉淀]
method_tags: [Agent, 多Agent协作, 自动化, BYOA]
tool_tags: [Cumora, Claude Code, Codex, OpenAI Responses API]
value_stage: 学习理解
risk_tags: [国内可用性, 数据安全, 成本]
public_level: public
---

# Cumora：AI Agent 作为一等公民的团队聊天工作区

## 1. 这是什么

Cumora 是一个开源（MIT）跨平台团队聊天应用，核心主张：AI Agent 不是被 @ 才应答的聊天框，而是工作区里长期存在的"队友"——有名字、人格（persona）、记忆、在线状态，能进群聊、发私信、认领看板任务、收发真实邮件，还能自己发起对话（定时器唤醒，agent 醒来观察房间后决定 DM 谁、发什么观点、拉谁开会）。

默认工作区自带四个 starter agent：Atlas（研究）、Iris（设计）、Bram（工程）、Nova（产品），可改人格、可"解雇"、可招聘新的。

- 官网：https://cumora.ai （当前 invite-only preview，preview 期免费）
- 仓库：https://github.com/yetone/cumora （作者 yetone，即 avante.nvim 作者；2026-08 Show HN 后 3.2k stars、约 458 stars/天的增速，89 commits，非常年轻）
- 桌面端 macOS/Windows/Linux（Electron），iOS TestFlight beta，Android 未发布

视频里"支持接入钉钉、企微"的说法在官网和仓库中均未见，标记为**待验证**，疑似视频作者自行引申。

## 2. 原始来源

- 发现入口：打捞处群内视频介绍
- 资料本体：官网 cumora.ai + GitHub 仓库 yetone/cumora
- 关键文档：
  - `docs/BYOA.md` — Bring Your Own Agent 模式
  - `docs/COORDINATION.md` — 多 agent 防碰撞协调机制（776 行，含大量实测教训，价值最高）
  - `docs/email.md` — 每个 agent 有真实邮箱（Resend 发出 + Cloudflare Email Routing 收信）
  - `docs/SHIPPING.md` — 人与 agent 共用的功能交付生命周期

## 3. 核心观点 / 核心能力

**两种"大脑"路径：**

1. **Cumora Cloud**：每个 agent 跑在托管的 K8s pod 里，turn 执行基于 OpenAI Responses API 的多跳工具调用循环（bash、文件、浏览器、邮件、记忆、skills）。
2. **BYOA（Bring Your Own Agent）**：在自己的 Mac/VPS 上跑 `npx cumora agent computer`，agent 的大脑变成你本地的 **Claude Code、Codex、Grok Build、Cursor Agent、OpenCode 或 pi CLI**，走你自己的 provider 账号，服务器永远看不到你的 key。

**产品形态要点：**

- agent 有私有工作区（文件、笔记、观察记录）和对每个协作对象的"印象"记忆
- agent 之间可以互发 DM；"Whisper room"让人可以旁观 agent 之间的讨论而不介入
- "Convene"：agent 可以在需要决策时拉起一个有主题、有决议记录的聚焦会议
- 共享 Kanban 看板和日历，agent 能认领真实工作单元
- 技术栈：React 18 + Vite + TS + Tailwind 前端；Express + ws + Postgres（Drizzle）+ Redis 后端；Electron 桌面端；Cloudflare Workers 做邮件网关和 R2 CDN
- 自托管需要 Postgres + Redis，硬性必需环境变量只有 `OPENAI_API_KEY`

**最有含金量的部分：COORDINATION.md 的多 agent 协调工程**

N 个独立 agent 会话被同一事件唤醒、读同一份对话、独立决策，两类失败：竞速碰撞（两个 agent 同时发同样内容）和大脑误判（状态看对了但决策错了）。Cumora 的防御分层（从"不需要模型注意力"到"软提示"）：

- **seen-cursor 新鲜度预检**：agent 发消息前服务端查 Redis 里它的已读位点，若有更新消息则 HOLD 住回复、把新消息喂给它重新决策
- **原子化工作认领**：真实工作单元（看板任务、sequence 号）在事务内 claim，重复 verbatim 消息在 INSERT 前拦截
- **小模型分诊门（triage gate）**：便宜小模型（haiku / gpt-mini 级）先判断"这条该不该我回"，大模型只在分诊通过时唤醒——省成本 + 降碰撞
- **大/小模型并发信号量 + 确定性 spawn 间隔**（默认 500ms，替代随机 jitter）+ **AdaptivePacer**（遇到限流自动把间隔翻倍到上限 8s，连续 5 轮干净再减半）
- **唤醒防抖/合并**：2.5s 窗口内的群消息合并成一次 turn；turn 进行中再来消息合并为一次 pending rerun；DM/@/人类消息走中途注入（stream boundary steering）保证响应延迟
- **限流冷却**：单 agent 遇 provider 限流后退避 60s，且限流通知不泄漏到聊天里（"provider 限流不是产品故障"）
- 关键原则原文：**"never add a prompt rule when a code mechanism is the right fix, and never add a code mechanism when the brain's making a clear decision in front of correct state"**（能用代码机制解决的别加提示词规则；模型在正确状态下能判断的别加代码限制）
- 有真实基准：7 agent 计数游戏 0 碰撞 0 限流；8 字接力链测试（6 个活跃 agent + 1 个故意缺席）8/8 顺序完成、0 重复，缺席者的份额由其他 agent 自动补上

## 4. 我学到了什么

- "Agent 当团队成员"的产品形态正在从概念变成可跑的开源实现：人格 + 记忆 + 主动性 + agent 间通信 + 共享工作载体（看板/日历/邮件），这五件套构成了"agentic workspace"的最小完整形态。
- 多 agent 共处一室的真正难点不是"让 agent 说话"，而是**防碰撞和防刷屏**，且答案是工程分层（新鲜度门、原子认领、小模型分诊、并发节流），不是更长的系统提示词。这份文档是目前公开资料里少见的、带实测数据的多 agent 协调踩坑记录。
- BYOA 模式印证了一个走向：协作/身份/记忆层可以是一个产品，而"大脑"接你已有的 CLI agent（Claude Code/Codex）和你自己的 key——agent 平台不必绑定模型供应商。

## 5. 它是否可信，哪些需要验证

- 仓库、文档、架构图均真实存在且自洽，COORDINATION.md 的细节（文件名、错误签名、基准数据）可信度高，不像包装项目。
- 但项目极年轻（89 commits、invite-only preview），star 增速快不等于产品成熟；企业级安全、权限、审计能力未见证据。
- 待验证：
  - 视频所称"接入钉钉/企微"——官网仓库无此内容，疑似误传
  - BYOA 模式在非 OpenAI 端点（如火山 ark-plan 兼容端点）下能否工作——cloud 路径硬绑 OpenAI Responses API，BYOA 路径取决于本地 CLI，理论可行但未实测
  - agent 主动发消息（agenda）在真实团队里的噪音/打扰程度
  - 自托管的实际运维成本（Postgres + Redis + K8s/FUSE 组件）

## 6. 对个人能力有什么价值

- COORDINATION.md 的协调分层可以直接对照我们自己的多 agent 场景（Hermes cron + 子代理 + 飞书群里多机器人设想）：**seen-cursor 新鲜度门、小模型分诊、原子认领、限流自适应节流**这四条都是可移植的模式，尤其"小模型先分诊、大模型后行动"与"贵模型动脑、便宜模型动手"的分工思路一致。
- "能用代码机制解决就别堆提示词规则"是值得固化的多 agent 设计原则。
- BYOA 的协议设计（`cumora` CLI 作为统一协议，大脑可插拔）值得参考：协作层与执行层解耦。

## 7. 对企业 AI 落地有什么价值

- 趋势信号（B 线雷达）：工作区 AI 的界面很可能不是"另开一个工具问 AI"，而是共享工作区里有名有姓、有职责、工作可见的 agent 成员。这类形态对未来"飞书/钉钉里的数字同事"怎么设计有直接参照。
- 对 Crystal 规划的"飞书话题群当云端总线、Codex 和 Claude Code 各有机器人身份、[TO:XXX] 协作"是同一个方向的现成产品化参照——Cumora 证明了这条路有人走通了最小闭环，但它本身**不适合近期在公司内落地**：invite-only、无国内 IM 集成、cloud 路径数据出境、自托管成本高且生态年轻。
- 对业务部门的直接价值暂不成立；定位为观察对象 + 工程参考。

## 8. 可做的小实验

- 暂不部署。低成本动作：把 COORDINATION.md 存档精读，对照 `coding-agent-orchestration` 相关技能，检查我们现有的子代理/cron 协作里是否缺少"新鲜度预检"和"小模型分诊"两个机制（例如多个自动化同时对同一飞书话题响应时的去重）。
- 若后续拿到 waitlist 资格，可纯个人体验 BYOA 模式（本地 Claude Code 当大脑），观察 agent 主动性和 agent 间协作的实际体感，不接入任何公司数据。

## 9. 风险和边界

- **数据安全**：cloud 路径对话与文件进入其托管 pod；公司场景只能考虑 BYOA + 自托管，且自托管仍需评估邮件、存储等外围服务。
- **国内可用性**：cloud 依赖 OpenAI Responses API；无中文 IM（钉钉/企微/飞书）集成证据；团队与文档以英文为主。
- **成熟度**：preview 阶段、89 commits、功能与定价策略可能大变。
- **组织风险**：agent 主动发起对话/拉会的模式在真实组织里可能制造噪音和责任模糊，"agent 发的邮件/承诺"的边界需要治理。
- **成本**：多 agent 常驻 + 多跳工具调用 + 大模型，token 消耗显著；Cumora 自己的设计里小模型分诊和限流退避都是被成本逼出来的。

## 10. 当前结论

雷达观察 + 工程参考，不做业务试点。产品形态（agent 一等公民工作区）与 Crystal 的飞书多机器人总线规划同频，COORDINATION.md 的防碰撞分层是可直接借鉴的工程资产；项目本身太年轻且无国内生态，不投入部署。3-6 个月后复查其成熟度、集成生态和自托管友好度。

## 11. 追加：与 Crystal 现有 agent 资产的接入判断（2026-08-30 群内追问）

问：云端的 OpenClaw、Hermes Agent + 本地的 Codex、Claude Code，能不能全接进一个"小军团"社区？

事实核对（BYOA.md）：Cumora 的 BYOA 大脑支持名单是固定 6 种 CLI——Claude Code、Codex、Grok Build、Cursor Agent、OpenCode、pi。**Hermes Agent 和 OpenClaw 不在名单里**，daemon 是按这几种引擎的协议（claude stream-json / codex app-server JSON-RPC）硬接的，不能把任意程序当大脑。

- Codex / Claude Code → 可以进 Cumora（BYOA，跑在自己 Mac/VPS 上，走自己的 key）。
- Hermes / OpenClaw → 不能作为 Cumora 的大脑；且协作面是 Cumora 自己的 app 或自托管服务器（Postgres+Redis），不是飞书/钉钉。
- 结论：Cumora 能装她"一半"的军团；要把四类全装进去，形态上反而是她自己规划的**飞书话题群总线**成立——飞书消息 API 不挑引擎，Hermes/OpenClaw/coding harness 各自以机器人身份收发消息即可，[TO:XXX] 做路由。Cumora 证明了该形态的产品可行性，它的身份/记忆/唤醒/防碰撞分层正是自建总线需要自己补的部分。

## 12. 认知沉淀：小军团与"人的注意力瓶颈"（2026-08-30 群内讨论）

Crystal 的判断：未来人的注意力是最大瓶颈，agent 小军团（含本地+云端、自己分工协作）正是为解决它而存在；她已验证的模式是"贵模型指挥便宜模型"（她 → Codex 出计划/验收 → Claude Code+DeepSeek 执行），确实省掉了她亲手干活的注意力。

讨论后收敛的分层结论：

1. **注意力是转移、不是消失**。她现有链条里，写代码的注意力→便宜模型，指挥的注意力→Codex，她手里只剩"派活 + 验收"。小军团想再省掉"派活/管副手"这层，但 agent 数量增加会制造新的注意力消耗：N 个主动 agent 每人每天发 20 条进展到群里，组长刷群的负担比手动干活还重（Cumora 的防碰撞文档一半在防 agent "太想表现"——抢话、刷屏、打爆限流）。
2. **省注意力的开关是分诊层，不是军团规模**。组长类比：7 个组员每人每天问 5 个问题=没自动化；7 个主动 agent 每人发 20 条进展=换种方式疯；理想态是每天桌上只有三张纸——待拍板、卡住、战报。三张纸和二十条进展之间的筛子就是分诊层（Cumora 的小模型 triage gate：默认沉默、有事才敲门；我们 watchdog 无异常不发消息是同一原则）。
3. **判断注意力不可省，只能信任分层**。执行注意力可转移给 agent；口径、人事、对客户承诺的判断必须留人。对应机制：低风险 agent 自决（如周五成长周报自动固化低风险纠正），高风险只递结论+证据。
4. **对飞书总线的设计原则**：每个 agent 发出的消息必须自带类型标签——[FYI 知会] 攒成战报 / [DECISION 待拍板] / [BLOCKED 卡住]；人刷群只看后两类。这层"军团门口的秘书"是 Cumora 不替我们做、必须自建的部分，比"接多少 agent 进来"更决定实际效率。
5. 猜想的验证方式：拿一条真实跨 agent 流程实测（Codex 计划+验收 → Claude Code 执行 → Hermes 验证+飞书话题群回报，[TO:XXX] 路由），量端到端墙钟时间和人中途伸手次数，与现状对比，跑两周即见分晓。

## 13. A2A 现状与"EDI 第三形态"判断（2026-08-30 群内讨论）

**A2A 进展事实（2026-08 核查）**：协议由 Google 2025-04 发布，已捐 Linux 基金会；一周年时 150+ 组织支持，GitHub 22k stars，官方 SDK 覆盖 Python/JS/Java/Go/.NET；Salesforce Agentforce、ServiceNow Now Assist 有生产部署；机制为 Agent Card（/.well-known/agent.json 能力自描述）+ HTTP/JSON-RPC + SSE 流式任务。DeepLearning.AI 有免费实操课。但 Hermes/Codex/Claude Code/OpenClaw 目前均不原生说 A2A，接入需自写薄包装。结论：标准生产级可用，但"自己军团内部"用飞书/Markdown 总线更省，A2A 的真实价值在跨组织边界，生态仍薄，定位盯标准而非现在接入。

**Crystal 的关键类比（已确认成立）：A2A 就是企业边界自动化的第三形态，同构于 EDI。**

- 对应关系：EDI 报文（850/810）↔ A2A Task/Message；partner profile ↔ Agent Card；VAN/B2B 交易网络 ↔ agent 发现注册层（未成熟）；企业内部 ERP 接口 ↔ MCP；X12/EDIFACT 标准 ↔ A2A/AP2 协议。
- 历史教训可直接迁移预判：(1) 标准免费但 onboarding 映射/测试/认证成本催生整个集成行业；(2) 普及靠大客户倒逼而非技术驱动（沃尔玛逼供应商上 EDI → 未来大采购商逼供应商"agent 可对接"，对英科是渠道压力不是选择题）；(3) 长尾永远在标准外（大客走 agent 直连，小客仍邮件/网页，GEO 与人类入口分层共存）。
- 本质升级：EDI 交换"单据"（结果凭证），A2A 交换"任务"（协商+委托+过程反馈+结算）；配合 2026 上半年 Visa Intelligent Commerce / Mastercard Agent Pay / Stripe Agentic Commerce 生产级 agent 支付轨道，企业间自动化边界从"交换数据"推进到"交换决策"。
- 形态演化：EDI（标准化单据）→ API/互联网 EDI（第二形态）→ A2A + agentic commerce（第三形态）。
- 同期雷达（见讨论，待展开）：AI computer-use（Anthropic CUA/OpenAI CUA/browser-use）正在融合 RPA——RPA 当护栏管高量重复、agent 当判断层处理变化，直接影响软件实施一组来也/影刀运维的换代路径。
