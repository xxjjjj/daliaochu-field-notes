# 止损评审清单 · 2026-08-15

> 配合 `focus-map.md` 使用。这两层是当前知识库的「水位线」：
> 第一节是真正想清楚的底盘；第二节是积压的小实验池，需要逐档做决定。

## 一、能力底盘：已沉淀 / 已验证 / 可业务试点（23）

评审动作：对每一篇回答「这一篇能不能在团队里直接用？」

- 能 → 推到业务里（记录首个真实使用场景）
- 不能 → 诚实降级为 `学习理解` 或 `已追源`

### 已沉淀 · 10

| 路径 | 一句话 | 评审（待填） |
|---|---|---|
| `notes/2026-06/2026-06-28-hermes-agent-self-introduction.md` | 打捞处群聊 2026-06-28 用户问"介绍一下 hermes agent"，Hermes 实例从自身代码和工程结构出发，给出一份"自指式"自我介绍。 | |
| `notes/2026-07/2026-07-05-feishu-cli-multi-user-isolation-status-check.md` | 2026-05-05 群聊里 12:50~19:35 那次「嫂子开门」事件触发的整套设计， | |
| `notes/2026-07/2026-07-05-fmhy-free-resource-index.md` | FMHY（FreeMediaHeckYeah）是一个社区维护的免费资源导航站，主站为 https://fmhy.net，GitHub 组织为 https://github.com/fmhy，核心仓库为 https://github.com/ | |
| `notes/2026-07/2026-07-25-5w1h-model.md` | 5W1H 是一套通用的提问框架：What、Why、Who、Where、When、How。它广泛用于新闻采访、项目管理、问题分析和持续改进，作用是先把模糊的描述变成可共同确认的事实与边界，再讨论方案。 | |
| `notes/2026-08/2026-08-12-boris-cherny-team-10-tips-original-full.md` | Boris Cherny（Claude Code 创建者）2026-01-31 在 X 发布的团队 10 技巧原帖逐条完整内容存档。群内视频是该帖的二手解读，本笔记为权威原文版本（中英对照要点），并注明与二手解读的差异。 | |
| `notes/2026-08/2026-08-12-boris-cherny-team-10-tips-plain-chinese.md` | 团队认为，最明显的效率提升来自于：同时开 3～5 个独立工作目录，每个目录跑一个 Claude Code 会话。 | |
| `notes/2026-08/2026-08-13-shake-ads-and-sensor-permissions.md` | 一条吐槽"摇一摇"开屏广告的小红书爆款笔记。借由网友对"发明者"的人肉，复盘了一种广告形态如何在监管整治点击型开屏广告之后，利用手机加速度计、陀螺仪等运动传感器，把用户的无意识动作（走路、掏手机、地铁扫码翻转）转译成"广告点击"，从而绕过" | |
| `playbooks/candidate-evaluation-framework.md` | 面向「实施 + 数据」方向应届生 / 初级候选人，把「值不值得培养 / 潜力在哪 / 风险在哪 / 未来路径适配」四个维度的判断沉淀为可复用模板，配套「聊天式面试官脚本」生成方法。 | |
| `playbooks/codex-agents-md-template.md` | 用于给 Codex、Claude Code、OpenCode、Cursor、Copilot 等 coding agent 准备项目级或个人全局指令。目标不是写一份很长的 prompt，而是让 Agent 每次开工前知道：这个项目是什么、怎么 | |
| `risks/2026-05-wechat-personal-bridge-risk.md` | 把所有"AI Coding Agent → 微信个人号"的桥接方案（包括 cc-connect 的 wechat 适配、Claude-to-IM-skill 的 weixin 通道等）都会经过 ilinkai.weixin.qq.com 这 | |

### 已验证 · 7

| 路径 | 一句话 | 评审（待填） |
|---|---|---|
| `experiments/2026-07/2026-07-05-feishu-cli-isolation-audit-log-analysis.md` | 打捞处 Round 10 的一次"用日志当一手数据源"实验： | |
| `playbooks/codex-device-code-auth-flow.md` | 在 Hermes 里绑定 OpenAI Codex（openai-codex provider）的标准流程。 | |
| `playbooks/feishu-wiki-pdf-export-no-pagebreaks.md` | 把飞书 Wiki 节点（docx）转成 PDF，并通过飞书文件消息发回对话窗口的标准流程。专指用户明确要求"不分页"（适合手机/单页阅读）的版本。 | |
| `playbooks/hermes-default-model-rotation.md` | Hermes 实例在 ~/.hermes/config.yaml 中切换默认模型的标准化流程。 | |
| `playbooks/intco-modelhub-provider-setup.md` | 把 Hermes 的内网代理 provider 从临时命名 intco-gpt 重命名为标准化命名 IntcoModelHub，并把默认模型从 gpt-5.5 切换到 intco-proxy 的标准流程。 | |
| `playbooks/leaver-resource-cleanup-checklist.md` | 当员工离职 / 外部合作终止 / 顾问合同时，在第一时间识别并回收该员工在企业内部已经拿到的所有资源（账号、API Key、Token、订阅、权限、应用、bot） 的标准化流程。 | |
| `playbooks/plain-language-translator-patterns.md` | plain-language-concept-translator（人话翻译机）这个 Skill 在打捞处群里被反复调用，用于把抽象 / 黑话化 / 不说人话的概念转译成"普通人能听懂、记得住、能复述"的解释。本卡汇总 2026-06-27 | |

### 可业务试点 · 6

| 路径 | 一句话 | 评审（待填） |
|---|---|---|
| `experiments/2026-04/2026-04-29-skill-knowledge-graph-runtime-bridge.md` | 打捞处群聊 2026-04-29，用户回顾并继续推进一个已经进行了一段时间的实验：把本地 Hermes 的所有 skill 整理成知识图谱（skill-wiki），并尝试在 Agent runtime 里以图谱的方式查询、推荐、组合、记忆  | |
| `experiments/2026-05/2026-05-11-auto-reminder-requirement-extraction.md` | 打捞处群聊 2026-05-11，用户希望从历史聊天记录 + 会议纪要中抽取"跟单提效｜自动催款"这条业务需求，并填入飞书 Bitable 项目表。这是一个典型的"Agent 从分散信息源抽取结构化业务需求"实验。 | |
| `notes/2026-05/2026-05-05-feishu-cli-multi-user-isolation.md` | 打捞处群聊 2026-05-05 12:50~19:35 出现的一次严肃的安全讨论： | |
| `notes/2026-05/2026-05-26-internal-tool-launch-wechat-announcement.md` | 打捞处群聊 2026-05-26 下午，用户要在公司微信大群里发布一个内部 AI 工具（管理者八大心智训练助手），并附上一份 PDF 操作文档。本卡沉淀"功能介绍" + "操作指南"双轨文案的写法骨架，以及两轮迭代中暴露出的常见陷阱。 | |
| `notes/2026-05/2026-05-28-half-year-report-content-method.md` | 打捞处群聊 2026-05-28 上午，用户（组长身份）要为 2026 上半年工作汇报生成 PPT 内容。Agent 经过 3 轮迭代，最终产出"成果收拢 + 问题洞察 + 改进方向"三段式内容结构。本卡沉淀这套半年度汇报的内容生产方法论。 | |
| `notes/2026-06/2026-06-02-colleague-skill-runtime-design.md` | 这是打捞处群聊 2026-06-01 → 2026-06-02 凌晨连续讨论中，从一个普通黑客松作品选题碰撞出来的产品方向：Colleague Skill Runtime（同事 Skill 运行时）。 | |

## 二、小实验池：可小实验（57）

评审动作：按「3 个月内会不会真做」分三档，直接改 frontmatter：

- ✅ 会做 → 排进 `experiments/`，给 deadline
- ↪️ 可能做 → 降级回 `学习理解`
- 🗑 不会做 → 归档或删

当前小实验池分布：

- A · AI Coding Agent 工程化: 27
- B · 企业 AI 落地 / 业务转译: 12
- C · 学习方法 / 知识沉淀: 12
- D · Hermes / 飞书 / 内部运维: 4
- E · 其他单次扫描 / 未归类: 2

| 路径 | 主线 | 一句话 | 三档（✅/↪️/🗑）|
|---|---|---|---|
| `experiments/2026-07/2026-07-02-codex-skills-source-review-mvp.md` | A | 前序资料是一张“Codex 不装这 8 个你都在亏”的社媒截图，初步判断它本质是 Agent Skill 商业化方向的传播素材。此前已经做了初步追源，但仍停留在“继续研究建议”，没有把后续动作真正推进到仓库本体阅读。 | |
| `notes/2026-04/2026-04-12-skills-sh-agent-skills-directory.md` | A | skills.sh 自称 "The Agent Skills Directory"，是一个跨 Agent 的 Skill 注册中心 + 包管理工具。它的定位类似于 npm / PyPI / VS Code Marketplace，但对象是  | |
| `notes/2026-05/2026-05-09-cc-connect-local-agent-to-messenger-bridge.md` | A | cc-connect 是一个本地运行的小工具，定位是把本机的 AI 编程 Agent（Claude Code / Cursor / Gemini CLI / Codex）桥接到飞书、钉钉、微信、Slack、Telegram、Discord、 | |
| `notes/2026-05/2026-05-14-agent-im-bridges-landscape.md` | A | "AI Coding Agent 不再被锁在终端里"，而是可以在 IM（飞书 / Lark / Slack / Telegram / Discord / 微信）里被 @ 触发后远端执行。这个方向上 2026 年集中爆发了一批互有差异的开源项 | |
| `notes/2026-05/2026-05-14-chatgpt-codex-cloud-and-claude-code-slack.md` | A | 2026 年两大 AI 编程 Agent（OpenAI Codex、Anthropic Claude Code）都不再只活在本地终端，而是把"执行入口"放到了云端 / IM： | |
| `notes/2026-07/2026-07-02-codex-hyperframes-remotion-video.md` | A | 这条资料的主题不是传统“文生视频”，而是“程序化视频生成”：让 Codex / Claude Code / Cursor 等代码 Agent 按提示词生成 HTML、CSS、React 或动画代码，再通过 HyperFrames / Rem | |
| `notes/2026-07/2026-07-04-ag-ui-protocol.md` | A | AG-UI（Agent-User Interaction Protocol）是一个开源、轻量、事件驱动的 Agent 与用户界面交互协议。它试图解决的问题不是“模型怎么更聪明”，而是“Agent 如何从纯文本聊天窗口进入真实业务应用界面”。 | |
| `notes/2026-07/2026-07-04-matt-pocock-skills.md` | A | 这是 Matt Pocock 公开的 Agent Skills 仓库，定位是“AI Skills For Real Engineers / Skills for Real Engineers”，不是一个完整接管流程的大框架，而是一组小型、可 | |
| `notes/2026-07/2026-07-04-page-agent.md` | A | Page Agent 是阿里开源的网页内 GUI Agent 项目，核心定位是“让 AI 操作员直接住在 Web 应用里”。它不是传统外部浏览器自动化脚本，而是作为前端组件嵌入页面，让用户通过自然语言控制当前网页界面。 | |
| `notes/2026-07/2026-07-04-palmier-pro-ai-video-editor.md` | A | Palmier Pro 是一个 macOS 原生的视频编辑器，核心卖点不是单纯“生成视频”，而是把 AI 生成、剪辑和导出放进同一个 timeline 工作流里，并通过 MCP 让 Claude、Codex、Cursor 等 Agent 读 | |
| `notes/2026-07/2026-07-05-canvas-hand-codex-image-editing.md` | A | Canvas Hand 是一个面向 Codex 的本地无限画布插件。它用 tldraw 做画布，把 Codex 的图像生成、画布标注、局部修改指令和再次生成串成一个循环：先生成图片，再直接在图上用箭头和文字标出要改哪里，Codex 读取标注 | |
| `notes/2026-07/2026-07-06-skill-to-loop-engineering.md` | A | 这条资料的核心不是单个 skill 文件怎么写，而是一个更高层的工程范式：skill 只是 Agent 学习和执行的起点，真正产生复利的是把 skill 放进可观察、可反馈、可审查、可迭代的 loop 里。 | |
| `notes/2026-07/2026-07-14-ego-lite-browser-automation.md` | A | 群内线索来自抖音短视频，标题指向“ego browser / ego lite”的浏览器自动化测评。已追到原始项目与官网：ego lite 是一个基于 Chromium 的浏览器，目标不是传统意义上的 Playwright/Puppetee | |
| `notes/2026-07/2026-07-16-agent-skills-github-stars.md` | A | 这是一组高热度 Agent Skills / AI coding workflow 项目的公开线索，核心不是“提示词合集”，而是把资深工程师、产品负责人、设计、QA、发布、安全等角色的工作习惯，沉淀成可被 AI 动态调用的操作规程。 | |
| `notes/2026-07/2026-07-26-ai-video-pipeline-fishaudio-heygen-hyperframes.md` | A | 抖音博主「鱼亦乐」发布的一条视频，演示了一条 AI 自主视频生产流水线： | |
| `notes/2026-07/2026-07-26-codex-custom-instructions.md` | A | 这条资料是一个 Codex 深度使用经验：不要只把 Codex 当一次性代码问答工具，而是通过自定义指令和项目说明文件，把“我是谁、这个项目怎么做、踩过哪些坑、完成标准是什么”固化下来，让 Agent 每次开工前都能读到稳定上下文。 | |
| `notes/2026-07/2026-07-26-harness-engineering-agent-runtime.md` | A | 这是一条抖音视频整理出来的 AI 工程方法论：核心观点是“AI Agent 不等于 LLM”，真正决定企业级可用性的，是模型之外的一整套驾驭系统——工具权限、上下文装配、状态管理、校验循环、失败重试、人工闸门、日志追踪和持续反馈。 | |
| `notes/2026-07/2026-07-26-lanshu-animated-architecture-diagram.md` | A | 一个 Codex/Claude Code Skill + 本地 Python 渲染器，输入一份 JSON spec，输出三份产物： | |
| `notes/2026-07/2026-07-27-claude-code-certified-architect.md` | A | Anthropic 于 2026 年推出的官方认证体系，目前共 4 个认证： | |
| `notes/2026-07/2026-07-28-codexmaxxing-jason-liu.md` | A | Jason Liu 的公开文章与 OpenAI 白皮书讨论一种 Agent 工作方法：不要把 Agent 当作“一问一答”的生成工具，而是为一项工作建立能持续推进的操作循环（operating loop）。循环由长期上下文、可执行工具、定时 | |
| `notes/2026-07/2026-07-29-taste-skill-impeccable.md` | A | 两个开源的 Agent Skill 项目，给 AI 编程工具注入设计判断力，解决 AI 生成前端 UI 千篇一律（"AI slop"）的问题。合计 GitHub 119k+ 星标，均采用 SKILL.md 格式，可直接注入 Claude C | |
| `notes/2026-08/2026-08-12-claude-code-team-10-tips.md` | A | 方法论文本。群内线索是一段二手解读视频，经追源确认其本体是 Claude Code 创始人 Boris Cherny 2026 年 2 月在 X 上公开的「Claude Code 团队 10 个内部使用技巧」，且视频存在跨工具混编，需与官方 | |
| `notes/2026-08/2026-08-12-vibecoding-two-prompts-first-principles-adversarial-review.md` | A | AI 自媒体博主「数字生命卡兹克」（X: @Khazix0918）分享的 Vibe Coding（AI 编程）方法论，核心是两个 Prompt 技巧： | |
| `notes/2026-08/2026-08-13-esl-epaper-ai-desktop-status-screen.md` | A | 抖音博主"刘大雁AI版"分享的一个低成本 AI 硬件 DIY 项目：从闲鱼（海鲜市场）花约 9 元淘来二手超市电子价签（4.2寸电子墨水屏，300×400 分辨率），刷固件后改造成一块放在桌面的 AI 状态屏，通过 Codex 辅助编写代码 | |
| `notes/2026-08/2026-08-13-html-ai-era-single-file-tools.md` | A | 一条中文技术短视频用一个很直观的例子切入“HTML 是 AI 时代更合适的文件格式”这个观点：作者让 Claude Code 花两三分钟生成了一个纯前端 HTML 图片压缩工具，支持轻度/中度/强力/自定义四档压缩、批量处理、单张或打包下载 | |
| `notes/2026-08/2026-08-14-ponytail-lazy-senior-dev-ai-skill.md` | A | Ponytail（马尾辫）是一个跨 AI 编程工具的 Skill / 插件，不是新模型，也不是压缩工具。它把一套「懒惰资深工程师」的决策规则注入到 Agent 的上下文里，让 Agent 在动手写代码之前先走完 7 级「偷懒阶梯」，能不写就 | |
| `playbooks/skill-to-loop-engineering.md` | A | 当一个 Agent Skill 已经能稳定完成某类任务，但仍依赖人工反复纠正、补充、提醒时，可以考虑从静态 Skill 升级为 Loop。 | |
| `experiments/2026-07/2026-07-03-locate-anything-nc-gui-automation.md` | B | 围绕 NVIDIA LocateAnything 的 GUI grounding 能力，进一步判断它是否能用于“抢救 NC”这类传统企业系统自动化场景。这里的“抢救”不是替换 NC，也不是绕过系统逻辑，而是看它能否帮助识别界面元素、报错位置 | |
| `experiments/2026-07/2026-07-04-flat-box-to-3d-mvp.md` | B | 围绕 TRELLIS / TRELLIS.2 的 3D 生成能力，群内提出一个更具体的业务问题：如果已有盒子的平面图，是否可以生成盒子的立体图。 | |
| `experiments/2026-07/2026-07-22-ai-open-source-roundup-validation-plan.md` | B | 本次资料包含多个开源 AI 项目和行业报告线索，信息密度较高。为了避免只做热点收藏，需要把其中最有业务迁移价值的项目转成可验证的小实验。 | |
| `experiments/2026-08/2026-08-04-revor-ai-poc-template.md` | B | Revor AI 是一款 B2B/外贸销售智能体：以目标客户画像（ICP）为输入，完成企业/联系人调研，经 Email、LinkedIn、WhatsApp 多渠道触达，客户进入可购买状态后交人工销售接管。产品与文档本体已核验（见相关笔记）， | |
| `notes/2026-07/2026-07-04-chattts-dialogue-tts.md` | B | ChatTTS 是 2noise 开源的文本转语音模型，定位不是传统朗读型 TTS，而是更偏“日常对话 / LLM 助手 / 多轮交流”的语音生成。它支持中英文，强调对话里的停顿、笑声、语气断点等自然表达。 | |
| `notes/2026-07/2026-07-04-trellis-3d-generation.md` | B | 群里线索指向“高精度 3D 建模轻松生成”，关键词为 TRELLIS。追源后确认主要本体是 Microsoft 开源的 TRELLIS 系列 3D 生成模型： | |
| `notes/2026-07/2026-07-13-exercises-dataset.md` | B | 这是一个公开 GitHub 项目 hasaneyldrm/exercises-dataset，提供结构化健身动作数据集。资料线索来自小红书截图，已追到 GitHub 本体。 | |
| `notes/2026-07/2026-07-13-roboflow-supervision.md` | B | 这是 Roboflow 维护的开源 Python 计算机视觉工具库 supervision。它不是一个单一模型，而是一个“模型输出后的统一工具层”：把不同视觉模型的检测结果转成统一数据结构，再提供标注、追踪、计数、区域过滤、数据集格式转换、 | |
| `notes/2026-07/2026-07-17-douyin-ai-hidden-aha.md` | B | 这条抖音线索补充关键词为 AhaCreator。已追到官网与公开资料：AhaCreator 是面向 influencer marketing / creator collaboration 的 AI-native 平台，定位为用 AI Ag | |
| `notes/2026-07/2026-07-22-unlimited-ocr.md` | B | Unlimited-OCR 是百度开源的 OCR / 文档解析模型，主打 One-shot Long-horizon Parsing：尽量把长文档、多页 PDF、高分辨率图片的解析从“逐页识别”推进到“长上下文一次性解析”。 | |
| `notes/2026-08/2026-08-02-moneyprinterturbo-ai-video-pipeline.md` | B | 截图指向开源项目 [MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo)：输入主题、关键词或自有文案后，串联 LLM 文案生成、素材检索/本地素材、TTS 配音 | |
| `notes/2026-08/2026-08-07-prophet-holiday-features.md` | B | Prophet 是 Facebook 开源的时间序列预测工具，采用趋势、周/年季节性、节假日效应和额外回归变量等可解释组件建模。群里提供的是官方 Quick Start，补充阅读了官方的“季节性、节假日效应与回归变量”文档及 GitHub  | |
| `experiments/2026-04/2026-04-30-skill-graph-autopilot.md` | C | 随着打捞处 / Hermes 内部 skill 数量增多（数十个），出现两个问题： | |
| `experiments/2026-06/2026-06-01-plain-language-village-translator.md` | C | 2026-06-01 晚上打捞处群聊，用户从一条抖音短视频（$150K 黑客松）切入，连环追问"什么是黑客松、Web3、0G、Web4、OG"等黑话。用户明确表态： | |
| `notes/2026-04/2026-04-19-llm-wiki-skill-karpathy-methodology.md` | C | llm-wiki-skill 是一个把 Karpathy 的 LLM Wiki 方法论落地为 Agent Skill 的开源项目。它让本地代码 Agent（Claude Code / Codex / OpenClaw / Hermes）持续 | |
| `notes/2026-07/2026-07-02-notebooklm-code-research.md` | C | 群内线索来自小红书短链，标题为“NotebookLM 2.0：会写代码的 AI 科研神器”。已追到 Google 官方资料：NotebookLM 在 2026-06-08 宣布升级，定位从“基于资料的问答/摘要工具”进一步转向“具备主动研究 | |
| `notes/2026-07/2026-07-03-karpathy-autoresearch.md` | C | AutoResearch 是 Andrej Karpathy 开源的一个自动化机器学习研究实验项目。它不是普通的参数搜索工具，而是把“研究员提出假设、改代码、跑训练、看指标、保留好结果、回滚坏结果”的循环交给 AI Coding Agent | |
| `notes/2026-07/2026-07-04-graphify-knowledge-graph.md` | C | Graphify 是一个面向 AI 编程助手的开源 Skill / CLI 工具，目标是把一个项目目录里的代码、文档、PDF、图片、视频等材料转成可查询的知识图谱。它不是简单向量检索，而是把函数、类、文档概念、设计原因、跨文件关系等抽成节点 | |
| `notes/2026-07/2026-07-04-next-ai-drawio.md` | C | Next AI Draw.io 是一个把 AI 对话能力接入 draw.io 图表编辑器的开源项目。它的核心能力是：用户用自然语言描述架构、流程或草图，系统生成或修改 draw.io 的图表内容；同时提供 Web 应用、桌面端、MCP Se | |
| `notes/2026-07/2026-07-05-claude-video-skill-short-video-analysis.md` | C | 这条资料的核心不是“生成视频”，而是给 Claude Code / Codex / Cursor 等 Agent 补上“看懂视频”的能力：把短视频拆成字幕、关键帧、时间轴和结构化笔记，让 Agent 能分析爆款视频的开头钩子、节奏、画面变化 | |
| `notes/2026-07/2026-07-13-stanford-storm-research-method.md` | C | 这条资料是 WaytoAGI 对 Stanford STORM 方法的整理，核心不是“让 Claude 写得更像博士”，而是把研究任务拆成更可靠的前置研究流程：多视角提问、基于检索的模拟对话、结构化大纲、再生成长文。 | |
| `notes/2026-07/2026-07-14-520520-industry-learning-method.md` | C | 这是一套面向“快速进入新行业 / 新领域”的学习路径。核心是用 5 组“20”建立行业认知：20 个核心关键词、20 本专业书、20 家头部公司 / 机构、20 家咨询 / 管理机构、20 位行业大牛。 | |
| `notes/2026-07/2026-07-26-four-sentences-find-talent.md` | C | 一条在小红书传播的视频（博主"问一问 ai 创业者"），内容实际介绍的是科技博主数字生命卡兹克于 2025 年 12 月在虎嗅发布的 "深度天赋挖掘机" Prompt 模板。 | |
| `notes/2026-08/2026-08-13-mineru-pdf-document-parsing.md` | C | MinerU 是上海人工智能实验室 OpenDataLab 团队开源的一站式高质量文档解析工具，将 PDF、图片、DOCX、PPTX、XLSX 等复杂文档转换为机器可读的 Markdown 和 JSON，面向 LLM 预训练、RAG 和 A | |
| `experiments/2026-07/2026-07-04-history-backfill-workbench.md` | D | 打捞处话题群已经产生了一批学习资料、方法讨论、工具测试和实验结论，需要把近 2-3 个月尚未入库的内容补进 daliaochu-field-notes。 | |
| `notes/2026-04/2026-04-12-resend-agent-friendly-email-api.md` | D | Resend 是一个面向开发者和 AI Agent 的现代邮件发送服务。它的核心定位不是"邮件营销 SaaS"，而是"开发者友好、API first 的邮件基础设施"——因此非常适合由 Agent / 脚本直接驱动发送、追踪、模板管理。 | |
| `playbooks/company-public-osint-boundary.md` | D | 公司场景下，OSINT 类工具的合理方向不是“还原个人画像”，而是做组织公开信息的合规核验和风险自查。 | |
| `playbooks/dnspod-resend-dns-records-setup.md` | D | 把自有域名（如 intco.online）接入 Resend 时，DNS 控制台（国内推荐 DNSPod / 腾讯云 DNSPod）必须配置三条记录，缺一不可： | |
| `experiments/2026-07/2026-07-12-multi-model-coding-sandbox.md` | D | 《人民公园说AI》文字稿提出两个值得验证的判断： | |
| `notes/2026-07/2026-07-26-agent-7-architectures.md` | D | AlunTalk（抖音博主阿伦）制作的一期 Agent 架构科普短视频。将当前主流的 Agent 架构从简单到复杂梳理为 7 种，并给出了一条清晰的演进路径。不是原创研究，而是对业界现有实践的系统化归类，适合作为入门认知框架和架构选型参考。 | |