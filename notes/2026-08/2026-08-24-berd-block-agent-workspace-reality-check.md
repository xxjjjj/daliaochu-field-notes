---
title: "Berd (Block) 真实使用反馈核验：发布6天的早期桌面Agent工作台"
date: 2026-08-24
discovery_source:
  type: 群聊线索
  title: "8月23日AI资讯盘点中提到的Berd智能体工作台"
  url: https://github.com/block/berd
primary_object:
  type: open_source_project
  name: Berd (by Block)
  url: https://github.com/block/berd
object_type: [open_source_project, trend_signal]
source_type: [GitHub, 官网, 公众号, YouTube, HackerNews]
business_tags: [ITBP, 个人能力, 产品]
problem_tags: [流程提效, 组织协同, 知识沉淀]
method_tags: [Agent, 多智能体, ACP, MCP, Vibe Coding]
tool_tags: [Berd, Goose, Claude-Code, Codex, ACP, Tauri]
value_stage: 待验证
risk_tags: [早期项目, 品牌混乱, 外部贡献受限, 成本]
public_level: public
---

# Berd (Block) 真实使用反馈核验

## 1. 这是什么

Berd 是 Block（Square/Cash App/Goose 母公司）8月18日开源的桌面应用，定位是**统一多个 coding agent harness 的前端**。你自带 Claude Code / Codex / Copilot / Amp / Cursor agent / Goose 的订阅或 API key，Berd 用一个 GUI 管它们——项目、会话、上下文、agent 配置、文件、技能。

技术栈：Tauri 2 + React 19，后端通过 ACP（Agent Client Protocol）接 Goose sidecar。Apache 2.0 许可。

**但要先注意一个品牌混乱**：GitHub 仓库叫 `block/berd`，官网是 `berd.xyz`，产品欢迎屏和部分媒体报道叫 "Bird"，Instagram/部分帖子又叫 "Buzz"（Buzz 实际是 Block 另一个 Nostr 协议的多Agent协作项目，不是同一个东西）。这说明发布期品牌没对齐，搜索真实反馈时容易串。

## 2. 原始来源

- 仓库：https://github.com/block/berd（2026-08-11 创建，8-18 v0.6.2 发布，截至 2026-08-24 共 713 star / 79 fork）
- 官网：https://berd.xyz
- Block 设计博客：https://block.xyz/inside/designing-ai-with-character-what-we-learned-building-berd
- HN 帖：https://news.ycombinator.com/item?id=49352040（3 points, 1 comment）
- YouTube 尝鲜测评："Berd: The Most FUN Coding Agent YET!"（发布期上手，非长期使用）
- 媒体：Dataconomy、CryptoBriefing、MindStudio、explainx.ai 等8月18-20日通稿

## 3. 核心能力（已核验）

- 多 harness 并排：CC、Codex、Copilot、Amp、Cursor agent、Goose，每个会话独立选 harness+model。
- ACP 协议解耦：任何说 ACP 的 harness 理论上可即插即用，不写定制集成。
- 持久项目/会话/上下文：不只是临时聊天框，有项目、附件、skills、配置面板。
- 3D 角色 + open canvas 风格 UI（不是传统 IDE 布局），Block 设计团队强调"agent with character"。
- 内置 `buzz-handoff` skill：能把 Buzz 频道上下文拉进私有对话、经审批回复（说明 Berd 和 Buzz 是 Block 同一 agent 栈的两个产品）。
- Block 内部团队已自用，不是"扔过墙"的开源（commit 活跃，8/11-8/24 持续推送）。

## 4. 真实用户反馈：目前能拿到什么

**关键判断：发布到核验时仅6天，没有"长期使用反馈"，只有早期bug和尝鲜反应。** 数据来源：

### 4.1 GitHub issues（175个，最有信息量）

按类型聚合：

- **harness 接入类**（最核心，真实痛点）
  - #148 GitHub Copilot Agent (copilot-acp) 一直卡在 not_ready
  - #159 Windows 上 cursor-agent 配置 curl 失败
  - #140 保存自定义 provider 不写 Goose 默认值，导致 Goose 不可用
  - #144 LM Studio provider 重复拼 `/v1`
  - #109 Codex in-app browser 因 node_repl 拒绝 node:process 失败
  - #108 Windows 终端用 `\\?\` 长路径前缀，cmd.exe 子进程全挂
- **文件/链接处理类**（密集，影响日常）
  - #166 聊天文件链接报"Not allowed to open path outside $HOME"
  - #169 `./file` 相对链接从文件系统根解析
  - #167 $HOME 外的内联图片 asset protocol 未配置
  - #171 Markdown 文件外部打开而非内置查看器
- **UI/UX 类**
  - #132 思考/工具活动时不自动滚动
  - #111 agent 跑的时候 transcript 滚动锁死、回弹
  - #134 多工作区时终端入口难发现
  - #119 inline MCP Apps CSP frame-src 缺失不渲染
- **功能请求（说明用户真在用、真有期待）**
  - #175 要原生 computer-control / 浏览器自动化
  - #174 要 OpenViking 记忆集成
  - #152 要支持外部终端（Ghostty）
  - #126 要自动 worktree 创建
  - #138 要本地 STT/TTS
  - #121 要支持 OpenCode

### 4.2 外部社区（几乎静默）

- **Reddit**：r/ClaudeAI、r/LocalLLaMA、r/ChatGPTCoding、r/AI_Agents、r/singularity 在过去30天搜索 "berd block" **0 结果**（2026-08-24 核验）。没有真实用户的长期体验帖。
- **Hacker News**：1帖 3分 1评论，唯一评论是质疑："I'm struggling to understand the use case? How is this better than an Agents.md file in each repo and switching in skills as required? Are people really switching between many different harnesses?"（不知道用例，比仓库里放 AGENTS.md 强在哪？真有人频繁切harness吗？）
- **YouTube**：1个发布期尝鲜视频，语气偏正面但承认是 first release，"doesn't get in your way... for a first open source release, quite great"——不是深度评测。
- **X/Twitter**：主要是转发官方和媒体通稿，未见长期使用复盘。

### 4.3 一个值得注意的外部贡献者信号

issue #166 和 #169 下有位 `AnobleSCM` 用户独立复现、定位根因、写好 patch、跑了 6790 个测试通过，**但因为仓库不接受外部 PR，只能把 patch 贴在评论里等维护者取**。这说明：
- 产品质量本身吸引了硬核用户；
- 但 Block 采取"开源但不接受PR"策略（Apache 2.0 允许 fork，但主线贡献门槛高）。

## 5. 我学到了什么

- **"多harness统一GUI"是真痛点还是伪需求，目前没定论**。Block 内部有这个痛点（多团队同时用Goose/CC/Codex），但 HN 质疑代表另一派：直接终端 + AGENTS.md 就够，统一GUI收益有限。这取决于你是否真的在一个项目里频繁切harness。
- **ACP 协议是真正的架构亮点**。Berd 不是写死绑几个harness，而是用 ACP 解耦——任何 ACP-compatible 的 agent 都能接。这跟 LSP 之于编辑器是同一个思路，长期价值可能在协议层而非GUI层。
- **发布期品牌混乱是减分项**。Berd/Bird/Buzz 三个名字混着用，搜反馈时容易把 Block 另一个项目（Buzz，基于Nostr的agent协作）的讨论错当成 Berd。
- **早期项目不要被媒体通稿和713 star迷惑**。发布6天175个issue说明迭代快、关注度高，但issue密集在Windows路径、文件链接、provider配置这些"日常能碰到"的点，意味着现在用会踩坑。

## 6. 可信度与待验证

- 仓库数据、issue、release 全部来自 GitHub API，可信。
- 媒体通稿信息基本准确，但"fun""best yet"这类评价是发布期营销语气，不代表长期使用。
- 待验证：
  - macOS（尤其Intel）source build 路径（#125 仍 open）
  - 非 Goose harness（Copilot/Cursor/OpenCode）的实际稳定性
  - 真实团队连续用2-4周后的项目/上下文管理体验
  - 与 Claude Code 原生 worktree、Codex 原生项目管理相比的净增益

## 7. 对个人能力的价值

- 如果你已经在同时用 CC + Codex（Crystal 正是这个状态：Codex写计划+审查，CC做执行harness），Berd 提供的"一个GUI看两个harness的会话和上下文"可能有实际价值——但目前和你已有的"Markdown文档总线"方案是替代关系而非互补，值得评估是否真比文档总线顺。
- ACP 协议值得单独看：未来如果要在飞书机器人场景接入多个 coding agent，用 ACP 比自己写适配层更可持续。
- 设计层面"agent with character"思路可参考：Block 故意给 agent 做3D形象和个性，不是玩具化，而是让用户对"哪个agent在干什么"有空间感知。这对多agent协作的UX设计有启发。

## 8. 对企业AI落地的价值

- **短期不建议企业级引入**：v0.6.2、发布6天、Windows路径和provider配置bug密集、不接受外部PR、品牌未对齐。
- **中期可关注**：Block 内部自用 + Apache 2.0 + ACP 开放协议，这三点组合意味着它不像随时会弃的玩具。如果6个月后 issue 曲线下降、release 稳定到 v1.x、有真实团队复盘，再评估。
- **架构思路可直接借用**：企业内部多agent部署也可以走"ACP协议 + 统一前端 + 自带模型订阅"的路线，不必绑定单一vendor。

## 9. 可做的小实验

- 不装 Berd，只读它的 ACP 实现和 `buzz-handoff` skill 代码，理解"agent 间如何安全地交接上下文"。
- 等 v0.7 或 v0.8 后在一台非主力机装 Berd，只用 CC+Codex 两个harness跑一个小项目，对比"文档总线"方案的实际步数和上下文丢失率。
- 列一张"我的harness切换频率"表：一周内真的在CC和Codex之间切几次？如果少于5次，统一GUI收益可能低于学习成本。

## 10. 风险和边界

- **早期项目风险**：v0.6.x，API和UI都可能变，不适合放生产工作流。
- **品牌/搜索风险**：搜 "Berd" 会混入 "Bird"（产品昵称）和 "Buzz"（Block另一产品），评估时务必认准 github.com/block/berd。
- **贡献模式风险**：Apache 2.0 但不接受外部PR，社区修复只能fork或等维护者取，响应速度取决于Block内部优先级。
- **成本不透明**：Berd本身免费，但每个harness背后是你自己的订阅/API key，多harness并行容易在不同账单间累积费用。
- **Windows 支持明显弱于 macOS**：issue 中 Windows 路径、cmd.exe、curl 问题集中。

## 11. 当前结论

Berd 是 Block 用开源策略推 ACP 协议和自家 Goose 生态的一步棋，架构方向正确（协议解耦、多harness、自带模型），但**目前还在发布头一周，没有真实长期使用反馈可参考**。GitHub issues 显示它在 macOS 上基本可用、Windows 上有较多硬伤，外部社区讨论近乎静默。

对Crystal的直接建议：**现在不装、不迁工作流，但把 ACP 协议和 buzz-handoff skill 的设计读了**；等9月底/10月看第二批用户复盘和 v0.8+ 稳定性再决定是否试。对"多harness统一GUI"这个需求本身，先用一周数据量化自己的切换频率，再判断是不是真痛点。
