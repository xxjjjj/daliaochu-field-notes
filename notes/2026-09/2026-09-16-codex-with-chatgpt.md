---
title: "Codex with ChatGPT：让订阅版 ChatGPT 当脑子、Codex 当手"
date: 2026-09-16
discovery_source:
  type: 群聊线索
  title: 晶晶在打捞处要求找 GitHub
  url: https://github.com/XiaoDuoYa/codex-with-chatgpt
primary_object:
  type: open_source_project
  name: XiaoDuoYa/codex-with-chatgpt
  url: https://github.com/XiaoDuoYa/codex-with-chatgpt
object_type: [open_source_project, methodology]
source_type: [GitHub]
business_tags: [ITBP, 个人能力]
problem_tags: [流程提效, 成本]
method_tags: [Agent, Vibe Coding, MCP]
tool_tags: [ChatGPT, Codex, MCP, cloudflared, OAuth]
value_stage: 待验证
risk_tags: [数据安全, 国内可用性, 合规]
public_level: public
---

# Codex with ChatGPT：ChatGPT 思考，Codex 干活

## 1. 这是什么

一个非官方社区项目（MIT，4.2k star / 441 fork / 51 commits，作者 XiaoDuoYa，非 OpenAI 官方）。
把**已付费订阅的网页版 ChatGPT（Plus/Pro）变成 Codex 编码会话的规划与审查大脑**，
Codex 只负责执行（改代码、跑 shell、测试）。口号："ChatGPT thinks. Codex works."

## 2. 原始来源

- 发现入口：群聊线索（晶晶点名找的 skill）
- 资料本体：https://github.com/XiaoDuoYa/codex-with-chatgpt
- 中文文档：README.zh-CN.md
- 关键设计文档：docs/architecture.md / protocol.md / security.md / troubleshooting.md

## 3. 核心观点 / 核心能力

**要解决的问题**：ChatGPT 订阅版网页额度大量闲置，而 coding agent 却在烧紧张的 API
额度做 plan / review。思路是把"思考"挪到已付费的订阅额度上，执行留在 Codex。
明确宣称：不用 API Key、不搞逆向代理，走官方网页 + 只读 MCP 桥接。

**架构分两个平面**：

- 控制面（Computer Use）：Codex 与 ChatGPT 交换极小的结构化 `[C2C]` 状态消息，
  状态机 `INIT → PLAN → EXECUTED → REVIEW → DONE`，不贴 diff、不传文件内容。
- 数据面（只读 MCP）：ChatGPT 自己通过 9 个只读工具按需拉取——workspace_info、
  list_directory、read_file、search_workspace、git_status、git_diff、test_status、
  execution_summary、execution_output。

**独立审查**：Codex 执行完后，ChatGPT 通过 MCP 直接看真实 git diff 和测试记录，
不盲信"测试通过了"的自述。

**桥的实现**：本地 loopback HTTP server + Cloudflare Quick Tunnel 暴露临时公网地址；
OAuth 2.1（PKCE S256 + 动态客户端注册 + refresh token 轮换）；首次配对只用一次性
配对码（5 分钟、5 次尝试、用完即毁）。有 Cloudflare 域名可配稳定 hostname。

**安全模型**：服务器端根本不存在写/删/shell/commit 工具（只读是构造出来的，不靠
prompt 自觉）；单 workspace 边界 + realpath 规范化防符号链接/`../` 逃逸；
`.env*`、密钥默认拒绝读取；声称有 150 个 vitest（路径安全、OAuth、配对、MCP e2e）。

## 4. 我学到了什么

1. 这与 Crystal 已在实践的"贵模型动脑、便宜模型动手 / harness bridge 文档总线"
   是同一模式的另一种实现：**把推理预算和执行预算拆到两个不同计价的模型上**。
   差别在她现在用 Markdown 文档做总线，这个项目用"MCP 只读数据面 + 极小控制面状态机"
   做总线——后者更工程化，控制面不传 diff 只传状态标签的设计值得借鉴。
2. "只读是构造出来的，不是靠提示词约束"——服务器压根不注册写工具，prompt injection
   无法提权。这是比系统提示词里写"你只能读"高一个段位的安全设计。
3. 审查环节让大脑自己拉 git diff 验证，而非采信执行者的汇报——agent 互信应基于
   可验证产物，这和她要求"自报成功不算数、要拿可验证 handle"一致。
4. 安装形态本身也是信号：给非技术用户的"一段话粘贴给 agent 自动装完"，
   skill 每日自检 GitHub 自动更新——agent skill 的分发正在 app 化。

## 5. 它是否可信，哪些需要验证

- 代码本体未审计：安全声明（只读、OAuth、密钥隔离）目前是作者自述 + 自带测试，
  未见第三方安全审计。要真用需读 src/auth、src/mcp、src/workspace 的路径控制代码。
- 非官方项目，依赖 ChatGPT 网页端 connector + Computer Use 能力，OpenAI 改版可能随时打断。
- Cloudflare tunnel 把本地工作区暴露成公网 MCP 端点，即便有 OAuth，也是真实攻击面；
  企业代码仓库场景需安全评审，不能在含客户数据/内部系统的仓库上直接试。
- 商业可持续性未知：个人项目，4.2k star 但仅 51 commits。
- star 数截图时点为 2026-09-16，热度真实性未交叉验证。

## 6. 对个人能力有什么价值

- 对 Crystal 现有 harness bridge 的直接参考：控制面/数据面分离、状态机协议、
  只读 MCP 九个工具的粒度划分，可抽出来改进她"文档总线 + 飞书话题群总线"的设计。
- 若她有 ChatGPT Plus/Pro 订阅且用 Codex CLI，可在**无敏感数据的玩具仓库**上
  做一次端到端实测，评估 plan/review 质量与额度节省的真实幅度。

## 7. 对企业 AI 落地有什么价值

- 模式可迁移到内部：用强模型（贵额度）做规划审查、便宜模型/固定规则做执行，
  在英科场景里对应的是"doubao-evolving 出方案，lite/代码规则执行确定性步骤"，
  中间用只读接口拉真实数据核验，而不是让执行 agent 自报成功。
- 不建议在公司环境直接部署该社区项目（tunnel 暴露 + 非官方 + 凭证走浏览器），
  但架构模式可以在内部 agent 编排里复用。

## 8. 可做的小实验

- 静态：clone 后审 src/mcp 工具白名单与 src/workspace 路径 containment 代码，
  验证"只读构造安全"是否名副其实（半天）。
- 动态：新建一个只含公开练习代码的仓库，跑 setup 全流程，抓控制面 `[C2C]` 消息，
  实测状态机和 review 环节是否真的自己拉 diff。
- 国内可用性待验：cloudflared quick tunnel 与 ChatGPT 登录在国内网络下的可达性。

## 9. 风险和边界

- 数据安全：本地代码经公网 tunnel 对 ChatGPT 端可读；严禁用于含客户数据、
  销售易配置、内部凭据的工作区。
- 合规：非官方，可能违反 ChatGPT 服务条款对自动化接入的解释，有封号风险。
- 国内可用性：ChatGPT 与 Cloudflare 隧道均需自备网络条件。
- 稳定性：上游网页改版即可能瘫痪。

## 10. 当前结论

值得作为**架构参考**深读（控制面/数据面分离、只读 MCP、状态机协议），
与 Crystal 的双 harness 实践高度同构；是否实际安装使用，建议先在公开玩具仓库
做安全审代码 + 端到端实测，且绝不在内部仓库上开 tunnel。
