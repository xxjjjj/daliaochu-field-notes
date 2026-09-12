---
title: DeskcommCRM——AI 原生、WhatsApp 优先的开源自托管销售 OS
date: 2026-09-12
discovery_source:
  type: GitHub
  title: DeskcommCRM：开源自托管 AI 销售 OS
  url: https://github.com/melgarafael/DeskcommCRM
primary_object:
  type: open_source_project
  name: DeskcommCRM
  url: https://github.com/melgarafael/DeskcommCRM
object_type: [open_source_project, trend_signal]
source_type: [GitHub, 官网文档]
business_tags: [销售, ITBP, 产品]
problem_tags: [转化, 获客, 流程提效, 用户洞察]
method_tags: [Agent, MCP, RAG, 自动化, 多租户, CRM]
tool_tags: [Next.js, Supabase, WAHA, WhatsApp, Vercel AI SDK, Docker]
value_stage: 学习理解
risk_tags: [合规, 数据安全, 成本, 账号风险]
public_level: public
---

# DeskcommCRM——AI 原生、WhatsApp 优先的开源自托管销售 OS

## 1. 这是什么

巴西开发者 Rafael Melgaço 主导的开源项目（MIT，2026-04-28 创建），定位
"AI Sales OS"：AI Agent 是 CRM 里的一等操作员（接客、资格判定、推动漏斗、
触发自动化），WhatsApp 是一等渠道，整套系统自托管、多租户、LGPD（巴西版
GDPR）by-design。对标 Kommo / Octadesk / Intercom 这类聊天式销售工具。

2026-09-12 实查：1,348 stars / 496 forks（群线索说 829 星、日增 126，与
GitHub API 当日数据方向一致，处于快速上升期），3,805 commits，当天仍在
合并 PR，Issue 90 个开放。

## 2. 原始来源

- 仓库：https://github.com/melgarafael/DeskcommCRM
- 架构一页纸：ARCHITECTURE.md（已读）
- 定位文档：VISION.md（已读，2026-07-19 刚从"电商 CRM"重定位为多行业 AI Sales OS）
- WhatsApp 引擎：WAHA（https://waha.devlikeapro.com/，Plus 版基于 NOWEB 非官方协议）

## 3. 核心观点 / 核心能力

**架构层最值得看的设计：**

1. **Agent 是 assignee，不是外挂聊天框。** Agent 轮值走完整管道：
   WhatsApp 入站 → HMAC 校验+幂等 → event_log 表 → worker → runAgentTurn
   （租户级 RAG + MCP tools）→ 发送前 guardrails → WAHA 适配器 → 命中条件
   转人工。Agent 和人类客服遵守同一套 RBAC、分配、审计规则，每组织有 AI
   预算上限（teto de gasto）。
2. **MCP 当神经系统。** 整个 CRM 的能力以 MCP tools 暴露，内部 Agent 先用，
   roadmap 上再开放公共 MCP——任何外部 Agent 接入后可直接"操作"CRM
   （建 lead、回客户、查订单）。CRM 从记录系统变成 Agent 的操作基础设施。
3. **Postgres trigger 绝不直接发 HTTP。** 所有副作用先落 `event_log` 表，
   worker（cron 每 1 分钟 drain）消费，用唯一约束 + 捕获 23505 做幂等。
   这是事件驱动可靠性的经典正确姿势。
4. **自进化飞轮是产品功能，不是口号：** 已解决对话回流为 RAG 知识；
   转人工点标记 Agent 能力缺口；"Evolução da IA"屏展示 Agent 是否在变好；
   Agent 自己提出改进提案（Propostas），但必须人工 gate 才生效。
5. **多租户隔离有测试兜底：** 所有 tenant-aware 表 organization_id not null
   + RLS；CI 里有隔离不变量测试（建两个组织，证明 A 用户对 B 的
   conversations/messages/contacts/crm_leads 看到 0 行，且先证明 B 的行真实
   存在，防空表假绿）。
6. **自托管工程化程度高：** 一条命令 VPS 安装（Docker 镜像，不编译）、
   屏内一键升级（先备份 DB → 换镜像 → 健康检查，失败自动回滚到旧镜像）、
   baseline.sql 幂等可"自愈"旧版脏数据、backup/restore/healthcheck 脚本齐全。

**渠道：** WhatsApp 双轨——WAHA 扫码（多号、限速+jitter+时间窗防封）或
Meta Cloud API 官方通道（模板审批）；另有 STOP detection、媒体私有 bucket。
电商侧已接 Nuvemshop（巴西 Shopify），VTEX/Shopify 在 roadmap。

**栈：** Next.js 16 + React 19 + TS、Supabase（Postgres/RLS/pgvector/
Auth/Realtime/Storage）、Vercel AI SDK（OpenRouter/Anthropic/OpenAI/Google，
可按"对话模型/索引模型"分别配置并屏内切换）、Upstash Redis 限流、Sentry
（PII scrub，opt-in）。

## 4. 我学到了什么

- **"AI 原生 vs AI 外挂"的具体分野可被架构审查**：看 Agent 有没有系统内
  身份（assignee）、受不受同一套权限审计约束、能不能写业务对象、有没有
  预算与 guardrails、转人工是否被审计。拿这五条去量任何"AI CRM"，比看
  宣传稿有效。
- **自进化飞轮要配人工 gate 才敢上线**：Agent 提案→版本化→人工批准，
  与"自动改 prompt"的激进路线形成对照，企业场景这个克制是对的。
- **自托管产品的竞争力一半在运维体验**：幂等安装、屏内升级+自动回滚、
- **巴西市场与我们的海外 B2B 在渠道结构上同构**：生意在 WhatsApp 里发生，
  CRM 必须长在聊天渠道上，而不是把聊天记录同步进 CRM 了事。

## 5. 它是否可信，哪些需要验证

已核实（README + ARCHITECTURE.md + VISION.md + GitHub API，2026-09-12）：
MIT、TypeScript 主语言、提交极度活跃、CI 门禁（typecheck/lint/单测/
Postgres 不变量/Playwright 48 spec/三个 Docker 镜像构建）。

需要打问号的：
- **巴士因子 = 1。** 3,493/3,805 commits 来自作者 melgarafael 一人
  （第二名 88），高度个人驱动，社区治理尚未形成。
- 文档自己承认的未完成项：限流只覆盖 2 个点（auth 公开面裸奔，见其
  threat-model §T1）；Idempotency-Key 只在 1 条路由实现；196 个路由
  handler 的表述在架构文档里还写着 166（文档与代码有漂移）。
- 新鲜安装 onboarding 的 P0 e2e 需要真实 WAHA+Redis+Resend+Nuvemshop，
  不在常规 CI 内——首装体验靠 VPS 手测保证。
- migrations 0001-0009、0013 是 stub，真实 schema 只在 baseline.sql，
  `supabase db push` 会得到空库——贡献者陷阱明显。
- "日增 126 星"等热度数字未独立复核时间序列，仅确认当日总量与上升趋势。
- 生产实战案例数量、单租户规模上限未见可验证数据。

## 6. 对个人能力有什么价值

- 一份现成的"AI 原生业务系统"参考实现：Agent turn 管道、guardrails、
  预算控制、handoff 审计、RAG 按租户隔离，读代码比读概念文章高效。
- 多租户 RLS + CI 隔离测试的做法可直接迁移到我们设计任何 SaaS 化/
  多客户内部系统的安全评审清单。
- 它的 docs/specs + HANDOFF*.md（人机/Agent 交接文档）目录是观察
  "AI harness 深度参与软件开发"的活样本（仓库带 .claude/.codex/AGENTS.md）。

## 7. 对企业 AI 落地有什么价值

- **海外销售数字化的渠道命题被再次验证**：英科海外销售日常主战场就是
  WhatsApp/邮件。"CRM 与聊天渠道双向打通、AI 在渠道里直接干活"是真实
  痛点方向；销售易这类成熟 CRM 走的是记录系统+叠加 AI 的路，差异值得在
  选型/自研讨论中作为对照坐标。
- **CRM AI 化的形态参考**：我们在销售易上做 AI 助手时，可以借它的
  五条判据（系统内身份、统一权限审计、可写业务对象、预算/guardrails、
  审计化 handoff）定义目标态与差距。
- **私有化叙事的完整样板**：医疗等合规敏感客户问"数据能不能不出门"时，
  开源 AI 原生方案多了一个对比维度（但它绑定巴西生态：Nuvemshop、
  HostGator、LGPD，落地中文/中国合规场景需要改造评估）。
- 选型咨询场景：它是"聊天优先 + 开源自托管"象限的代表性样本，适合
  拉美/中小电商/客服型销售，不适合复杂 B2B 长周期、强 ERP 集成的大客户。

## 8. 可做的小实验

1. 读 `lib/agent-engine/` 与 agent-turn 管道代码，整理一份"Agent 写操作
   前置 guardrails 清单"，对照我们自己的 Agent 项目查漏（低成本，纯读码）。
2. 本地 Docker 起一套（不连真实 WhatsApp），体验多租户 + RAG + 自动化
   QUANDO/SE/ENTÃO 规则引擎，评估其事件/worker 设计可否复用到飞书侧
   自动化（需 4GB 内存、Supabase、一个 LLM key）。
3. 暂不建议：用公司 WhatsApp 业务号扫码接 WAHA（非官方协议有封号风险）。

## 9. 风险和边界

- **WhatsApp 非官方协议封号**：WAHA Plus/NOWEB 是逆向 Web 协议，项目自带
  throttle+jitter+时间窗防封但不消除风险；业务号只能走 Meta Cloud API 官方
  通道（有模板与 24h 会话窗口限制）。与我们对微信生态非官方接口的谨慎
  同构。
- **合规区域错配**：LGPD ≠ 中国个保法/医疗行业监管，"合规 by-design"
  不能直接当中国合规证据；自托管意味着数据控制者责任全在部署方。
- **单点维护 + as-is 无 SLA**：社区支持，无商业兜底；安全面尚未闭环
  （自承认 auth 限流缺失）。
- 供应链：依赖 Supabase 云（虽可自托管）、Upstash、境外 LLM provider，
  国内网络与数据出境均有现实约束。
- 生态绑定拉美支付/电商（Nuvemshop、HostGator 巴西），中文本地化空白。

## 10. 当前结论

作为**行业观察样本和架构参考**价值高：它把"下一代 CRM"的一种答案
（AI 原生 + 聊天优先 + 开源自托管）做到了可运行、有测试门禁、有运维
工程化的完整度，且迭代极快。作为**直接选型/部署对象**要谨慎：巴士因子、
自承认的安全缺口、WhatsApp 封号风险和拉美生态绑定都不支持今天就上
生产。建议保持雷达跟踪（watch release + 公共 MCP 落地），优先做读码
级借鉴而非部署级采用。
