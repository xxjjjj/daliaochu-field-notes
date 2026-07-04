---
title: OpenHuman（tinyhumansai）：本地优先的个人 AI "超级智能" 助手
date: 2026-05-22
discovery_source:
  type: 群聊讨论
  title: 个人 AI 助手 / 数字分身 / 蒸馏同事
  url:
primary_object:
  type: open_source_project
  name: tinyhumansai/openhuman
  url: https://github.com/tinyhumansai/openhuman
object_type: [open_source_project, desktop_application]
source_type: [GitHub, 群聊线索]
business_tags: [ITBP, 个人能力, 知识沉淀]
problem_tags: [知识沉淀, 用户洞察, 流程提效, 数字分身]
method_tags: [Agent, Memory, 本地优先, Auto-Fetch, Composio, MCP]
tool_tags: [OpenHuman, Ollama, Composio, Obsidian, SQLite, SuperContext, TokenJuice]
value_stage: 学习理解
risk_tags: [数据安全, 隐私, 国内可用性, 维护风险, License 待查]
public_level: public
---

# OpenHuman（tinyhumansai）：本地优先的个人 AI "超级智能" 助手

## 1. 这是什么

OpenHuman 是一个开源、本地优先的"个人 AI 超级智能"桌面应用，主打把个人数据（Gmail / Notion / Slack / Drive / Calendar / 本地文件等）持续汇入"记忆树"，再用一个统一的 Agent 在桌面 / 浏览器 / Google Meet 里调用。它由 `tinyhumansai` 团队开发，定位介于"本地知识库（Obsidian）"和"云端 AI 助手（ChatGPT）"之间。

**核心口号**：local memory, managed services where needed, simple and powerful.

## 2. 原始来源

- 主仓库：[github.com/tinyhumansai/openhuman](https://github.com/tinyhumansai/openhuman)
- 官方主页：[tinyhumans.ai/openhuman](https://tinyhumans.ai/openhuman)
- 文档：[tinyhumans.gitbook.io/openhuman](https://tinyhumans.gitbook.io/openhuman)
- 发现入口：打捞处群聊 2026-05-22，用户在调研"个人 AI / 数字分身 / 把同事蒸馏成 Skill"方向时发现

## 3. 核心观点 / 核心能力

1. **Memory Tree + Obsidian-style Vault**
   - 把接入的数据切成 ≤3k token 的 Markdown chunk；
   - 存到本地 SQLite，同时同步为 `.md` 文件到 Obsidian 兼容目录；
   - 用户可以用 Obsidian 直接编辑 memory。

2. **Auto-Fetch 循环**
   - 默认每 20 分钟自动从已连接数据源（Gmail / Notion / Slack / Drive / Calendar 等）拉新数据；
   - 不需要人工触发，memory 持续更新。

3. **SuperContext（冷启动解药）**
   - 每个新 thread 的第一轮，自动 spawn 一个只读的 `context_scout`；
   - 它扫 memory tree + 文件 + 已连接数据，组装一个 bounded context bundle；
   - prepend 到用户消息前，模型"上来就有上下文"。

4. **本地 LLM 兜底**
   - 可选配 Ollama 跑本地模型；
   - 低层任务（summarization / tooling）走本地，高层任务走托管路由；
   - 默认仍用 OpenHuman 托管的模型路由（订阅制覆盖 30+ provider）。

5. **Smart Token Compression（TokenJuice）**
   - HTML → Markdown + URL 缩短 + 去重 + 长 tool output 摘要；
   - 官方数据：成本 / 延迟降最多 80%；
   - CJK / emoji / 多字节字符按 grapheme 保留。

6. **集成生态**
   - Composio 层：100+ OAuth 托管连接器（Gmail / Notion / GitHub / Slack / Stripe 等）；
   - MCP 生态：5000+ server 可挂；
   - Skills 目录：90,000+ 条目。

7. **桌面形态 + 桌面吉祥物**
   - 非 terminal-first，提供桌面应用 + 会说话 / 反应的吉祥物；
   - 支持加入 Google Meet（会议场景的虚拟人）。

8. **Goals + Kanban**
   - `MEMORY_GOALS.md` 定义长期目标，agent 按近期活动自检；
   - 每个 thread 可携带 thread goal + token budget，空闲期可自主继续；
   - 内置 Kanban：plan / acceptance criteria / approval gate。

## 4. 我学到了什么

- **"本地优先 + 托管兜底"是新的中间路线**：纯本地（如 Obsidian + 本地 LLM）扩展性差，纯云（ChatGPT / Claude.ai）数据出域风险高。OpenHuman 选择 memory / file 本地，sign-in / model routing / OAuth 走托管，是个可参考的折中架构。
- **Auto-Fetch + SuperContext 把"上下文冷启动"从产品问题变成工程问题**：与其训练模型"学你的偏好"，不如每 20 分钟自动 fetch + 每个 thread 自动扫 memory——前者昂贵且不可控，后者稳定且可解释。
- **Memory Tree 用 Markdown chunk 落地是一个妙招**：既能用 Obsidian 人工编辑、又能结构化检索、又能避免"一次性把全量塞进 prompt"。这种"双形态持久化"思路值得复用到 Hermes 的 memory 设计里。
- **数字分身 / 同事 Skill 方向已有成型项目**：OpenHuman 不是这个方向的孤品，配合 `titanwings/colleague-skill`、`agentscope-ai/QwenPaw` 一起看，结论是"个人 AI 助手"赛道已经有完整玩家。

## 5. 它是否可信，哪些需要验证

- **可信度**：仓库活跃、文档清晰、官方主页 + gitbook + GitHub 三处一致；安装包走 brew / apt / yay 等系统包管理器（签名 / 校验链完整）。
- **需要验证**：
  - License：仓库主页未明示 license，需要进 LICENSE 文件确认（公开 / 商业可用范围）。
  - 国内可用性：托管服务（sign-in / model routing / web search proxy）是否需要外网；Composio 的 OAuth 通道对国内账号支持度。
  - 隐私边界：默认会向托管服务上传什么？是否可完全断网运行？
  - 与 Hermes / QwenPaw / OpenHuman 之间的差异化是否真值迁移成本。
  - 桌面吉祥物加入 Google Meet 这种"会议场景虚拟人"在国内协作场景下的接受度。

## 6. 对个人能力有什么价值

- **Memory Tree + Obsidian 模式**：可以参考其 ≤3k token chunk 拆分、SQLite + Markdown 双形态持久化思路，做 Hermes 长期记忆的本地镜像。
- **Auto-Fetch + SuperContext**：在"用户启动新 thread 时如何自动携带上下文"这一问题上提供了工程化模板。
- **桌面 Agent UX**：摆脱 terminal-first 的姿态值得借鉴——业务方更愿意用 GUI 而不是 CLI。
- **Composio + MCP + Skills 三件套**：集成层抽象的统一思路。

## 7. 对企业 AI 落地有什么价值

- **个人 / 团队数字分身**：把"销售冠军 / 资深 HR / 老法师"的历史沟通 / 文档接入 memory tree，沉淀为可被 junior 同事调用的"经验 agent"。
- **知识沉淀自动化**：Auto-Fetch 把"员工每周写周报"这种负担自动转成 memory，省人工。
- **本地 + 托管混合架构**：在数据合规要求高的场景（金融 / 医疗 / 政府），可以保留 memory / 文件本地，只把"路由 + OAuth"托管。
- **桌面 Agent + 会议场景**：会议自动出纪要 / 自动入会"虚拟人"——OpenHuman 已给出方向。

## 8. 可做的小实验

- 在 macOS / Linux 上跑一次 `brew install tinyhumansai/openhuman/openhuman` 或 `apt-get install ./openhuman.deb`，验证安装体验。
- 接 1-2 个数据源（如 Gmail + Notion），观察 Auto-Fetch 写入的 Markdown chunk 质量与去重效果。
- 开新 thread，验证 SuperContext 是否真的把 memory tree 的相关上下文 prepend 到消息。
- 关掉托管模型路由（如果有开关），切到 Ollama 本地模型，对比低层任务的体感。
- 评估 License 与合规后，判断是否可以在公司内部署一个"团队版 OpenHuman"作为知识沉淀实验。

## 9. 风险和边界

- **数据安全 / 隐私**：默认会向托管服务上传 sign-in / 模型路由 / OAuth 凭据；如果数据合规要求严格，需要找到完全本地模式。
- **国内可用性**：托管层（model routing / web search proxy / Composio OAuth）是否需要外网不确定；Composio 部分连接器对国内账号支持有限。
- **维护风险**：项目历史不长（仓库 issuelist 644+ 看起来运营压力大），需观察后续节奏。
- **License 待查**：公开仓库未在主页明示 license，引入前必须先核对。
- **桌面吉祥物 / 会议虚拟人**：在国内企业 / 正式场景可能被视为"不严肃"，UX 接受度需评估。
- **Overpromise 风险**：官方宣称"1 billion tokens memory" "80% cost reduction"，需以实测为准。

## 10. 当前结论

OpenHuman 是 2026 年"个人 AI 超级智能"方向上最完整、最接近产品形态的开源项目之一。它的 Memory Tree + Auto-Fetch + SuperContext 组合是值得抄的工程模式；但 License / 隐私 / 国内可用性需要实测确认。

落地策略：先在小实验环境跑通 Auto-Fetch + SuperContext 两个机制；如果体感 OK，再评估 License 和合规后考虑公司内部署。同期关注 `titanwings/colleague-skill`（数字分身）和 `agentscope-ai/QwenPaw`（个人 AI 助手）作为对照。
