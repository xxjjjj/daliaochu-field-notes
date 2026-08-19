# Focus Map · 2026-08-15

> 由全仓库 168 篇非 oss markdown 自动聚合 + 人工聚焦判断生成。
> 用途：停止继续平铺打捞，按主线看清「已经厚在哪、卡在哪、下一步收在哪」。

## 一、四条主线 + 一个杂项

| 主线 | 数量 | 厚度判断 |
|---|---|---|
| A · AI Coding Agent 工程化 | 59 | **最厚**。问题、模式、作者、案例四个维度都已成型，建议作为下一阶段主战场。 |
| B · 企业 AI 落地 / 业务转译（含市场部 BP 雷达） | 31 | **第二厚，天然多触点**。BP 对象含市场部，音视频/内容生产/获客工具的扫描本身就是雷达价值，不要求每条都直接挂靠主营业务。需区分「雷达扫描」与「要落地试点」两档，前者保持广度，后者才需要业务问题驱动。 |
| C · 学习方法 / 知识沉淀 | 43 | **第三厚**。学习方法、研究方法、知识沉淀、概念翻译，密度高但偏「输入侧」，少闭环。 |
| D · Hermes / 飞书 / 内部运维 | 26 | **自身运维**。Hermes/飞书/模型/权限的运维记录，是支撑层不是学习主线。 |
| E · 其他单次扫描 / 未归类 | 9 | **杂项**。一次性扫描、未归类或仅群聊线索，建议压缩进每周 roundup。 |

## 二、按主线 × 价值阶段

### A · AI Coding Agent 工程化  (59)

**已沉淀** · 4

- `notes/2026-08/2026-08-12-boris-cherny-team-10-tips-original-full.md` 《Boris Cherny 原版全文——Claude Code 团队 10 技巧（2026-01-31 X 帖逐条原文）》
- `notes/2026-08/2026-08-12-boris-cherny-team-10-tips-plain-chinese.md` 《Claude Code 团队 10 个技巧——Boris 原帖完整说人话翻译版》
- `notes/2026-08/2026-08-13-shake-ads-and-sensor-permissions.md` 《摇一摇广告与传感器权限灰色地带》
- `playbooks/codex-agents-md-template.md` 《Codex AGENTS.md 自定义指令最小可用模板》

**可业务试点** · 1

- `notes/2026-06/2026-06-02-colleague-skill-runtime-design.md` 《Colleague Skill Runtime：把"蒸馏同事"从能力做成"上岗能力》

**可小实验** · 27

- `experiments/2026-07/2026-07-02-codex-skills-source-review-mvp.md` 《Codex Skills 清单追源 MVP：last30days 与 xhs-ops 源码/仓库初读》
- `notes/2026-04/2026-04-12-skills-sh-agent-skills-directory.md` 《skills.sh：跨 Agent 的 Skill 注册中心 / 包管理（Agent 时代的 "npm"）》
- `notes/2026-05/2026-05-09-cc-connect-local-agent-to-messenger-bridge.md` 《cc-connect：把本地 AI 编程 Agent 桥接到飞书 / 微信等聊天平台》
- `notes/2026-05/2026-05-14-agent-im-bridges-landscape.md` 《Agent-to-IM 桥接工具生态图谱：agents-to-im / Claude-to-IM-skill / ClaudeTeam / clisbot / Kagura / cc-connect》
- `notes/2026-05/2026-05-14-chatgpt-codex-cloud-and-claude-code-slack.md` 《Codex Cloud（chatgpt.com/codex）与 Claude Code in Slack：从 IM / 浏览器驱动云端编程 Agent》
- `notes/2026-07/2026-07-02-codex-hyperframes-remotion-video.md` 《Codex + HyperFrames + Remotion：用代码型 Agent 自动生成可控视频》
- `notes/2026-07/2026-07-04-ag-ui-protocol.md` 《AG-UI：Agent-User Interaction Protocol，把 Agent 从纯文本聊天推向可交互应用》
- `notes/2026-07/2026-07-04-matt-pocock-skills.md` 《Matt Pocock Skills：小型可组合 Agent 工程技能库》
- `notes/2026-07/2026-07-04-page-agent.md` 《Page Agent：嵌入网页内部的 GUI Agent》
- `notes/2026-07/2026-07-04-palmier-pro-ai-video-editor.md` 《Palmier Pro：AI 可操控时间线的视频编辑器》
- `notes/2026-07/2026-07-05-canvas-hand-codex-image-editing.md` 《Canvas Hand：把 Codex 图像生成改造成“画布标注式可控改图”》
- `notes/2026-07/2026-07-06-skill-to-loop-engineering.md` 《Skill2Loop：从 Agent Skill 到可持续改进的 Loop Engineering》
- `notes/2026-07/2026-07-14-ego-lite-browser-automation.md` 《ego lite：面向 AI Agent 的共享浏览器与浏览器自动化新形态》
- `notes/2026-07/2026-07-16-agent-skills-github-stars.md` 《GitHub 高星 Agent Skills 项目观察》
- `notes/2026-07/2026-07-26-ai-video-pipeline-fishaudio-heygen-hyperframes.md` 《AI 自主视频生产流水线：Fish Audio + HeyGen + HyperFrames + Claude Code》
- `notes/2026-07/2026-07-26-codex-custom-instructions.md` 《Codex 自定义指令：让 Agent 越用越顺手的项目上下文层》
- `notes/2026-07/2026-07-26-harness-engineering-agent-runtime.md` 《Harness Engineering：把 Agent 从模型能力升级为可控运行系统》
- `notes/2026-07/2026-07-26-lanshu-animated-architecture-diagram.md` 《岚叔动态架构图 Skill（lanshu-animated-architecture-diagram）》
- `notes/2026-07/2026-07-27-claude-code-certified-architect.md` 《Claude Code 认证架构师（Anthropic 官方认证体系）》
- `notes/2026-07/2026-07-28-codexmaxxing-jason-liu.md` 《Codex-maxxing：把单次提示转成可审查的持续工作循环》
- `notes/2026-07/2026-07-29-taste-skill-impeccable.md` 《Taste Skill & Impeccable：两个反 AI 模板化设计的 Agent Skills》
- `notes/2026-08/2026-08-12-claude-code-team-10-tips.md` 《Claude Code 团队 10 个高效使用技巧（Boris Cherny 原版 + 二手解读差异核对）》
- `notes/2026-08/2026-08-12-vibecoding-two-prompts-first-principles-adversarial-review.md` 《Vibecoding 两个核心 Prompt：第一性原理 + 对抗式审查》
- `notes/2026-08/2026-08-13-esl-epaper-ai-desktop-status-screen.md` 《二手电子价签改造 AI 桌面状态屏：9元硬件 + Codex + 自动化的三层玩法》
- `notes/2026-08/2026-08-13-html-ai-era-single-file-tools.md` 《HTML 在 AI 时代的回潮：从 Markdown 到单文件可交互工具》
- `notes/2026-08/2026-08-14-ponytail-lazy-senior-dev-ai-skill.md` 《Ponytail：让 AI 编程助手学会「不写代码」的极简主义 Skill》
- `playbooks/skill-to-loop-engineering.md` 《Skill to Loop Engineering Playbook》

**已追源** · 2

- `notes/2026-08/2026-08-12-computer-use-agent-clicky-openclicky-review.md` 《Computer Use Agent 实测对比：heyclicky vs openclicky（屏幕级跨应用操作）》
- `notes/2026-08/2026-08-13-loopx-long-running-agent-state-kernel.md` 《LoopX：长程 AI Agent 的本地状态内核与控制面板》

**待验证** · 7

- `experiments/2026-05/2026-05-09-apply-patch-stuck-debug.md` 《Codex apply_patch 反复卡死调试》
- `experiments/2026-05/2026-05-14-codex-smoke-error-investigation.md` 《Codex smoke 报错调研：终端保护机制 + agent 安全提示》
- `notes/2026-07/2026-07-24-marble-os-taxonomy.md` 《Marble os-taxonomy：小学知识图谱开源资料》
- `notes/2026-07/2026-07-26-codex-graph-engineering-multi-agent-v2.md` 《Codex Graph Engineering 与 Multi-agent V2：多模型子代理的工程化边界》
- `notes/2026-07/2026-07-26-openconnector-ai-agent-saas-connector.md` 《OpenConnector - AI Agent 的开源 SaaS 连接层》
- `notes/2026-07/2026-07-27-codex-project-session-management.md` 《Codex 不卡的秘诀：项目文件夹 + 会话隔离 + 记忆文件迁移》
- `notes/2026-07/2026-07-28-open-minis-on-device-ai-agent.md` 《Open Minis：端侧 AI Agent 开源应用——手机上的 Linux 沙箱 + 深度系统集成》

**待追源** · 5

- `notes/2026-07/2026-07-01-codex-skills-money-list.md` 《Codex/Claude Skills 赚钱清单截图的可用性判断》
- `notes/2026-07/2026-07-02-vibe-coding-handcraft-developer.md` 《小红书“教你手搓万物，成为开发师”的趋势判断》
- `notes/2026-07/2026-07-14-codex-plugin-stack.md` 《Codex 插件栈：从聊天工具到可交付工作流》
- `notes/2026-07/2026-07-17-codex-webpage-three-step.md` 《Codex 辅助小白三步制作网页的方法》
- `notes/2026-07/2026-07-26-on-device-user-agent-trend.md` 《被低估的 AI 趋势：端侧用户 Agent 与行为智能》

**学习理解** · 11

- `notes/2026-05/2026-05-04-skill-as-storefront-business-model.md` 《Skill = 卖场 / 异形 Skill / AI 即结果 业务模式脑洞》
- `notes/2026-05/2026-05-13-remote-sandbox-vs-codex-sandbox.md` 《远程沙箱 vs Codex 沙箱：Agent 沙箱边界讨论》
- `notes/2026-06/2026-06-02-openskills-colleague-skill.md` 《OpenSkills 生态与 Colleague.skill（titanwings）：把同事"蒸馏"成 AI Skill》
- `notes/2026-07/2026-07-02-build-your-own-x-vibe-coding.md` 《Build Your Own X 与 Vibe Coding 入门学习路径》
- `notes/2026-07/2026-07-12-renmin-gongyuan-ai-model-access.md` 《人民公园说AI：顶级闭源模型可得性下降与国产模型生态机会》
- `notes/2026-07/2026-07-26-amdahls-law-ai-coding-bottleneck.md` 《AI 时代的阿姆达尔定律：Dario Amodei 论代码加速后的新瓶颈》
- `notes/2026-07/2026-07-26-harness-engineering-anthropic.md` 《Harness Engineering：把 AI 装进“公司制度”里才跑得远》
- `notes/2026-07/2026-07-26-hiroki-tomiyasu-vibe-coding-farmer.md` 《北海道农民富安宽树的VibeCoding实践：100公顷农场的AI自动化》
- `notes/2026-07/2026-07-28-skillsmp-agent-skills-directory.md` 《SkillsMP：面向 Agent Skills 的公开目录与来源追溯入口》
- `notes/2026-08/2026-08-06-codex-session-handoff-habit.md` 《Codex 长会话交接习惯：HANDOFF.md 模式》
- `notes/2026-08/2026-08-14-deepseek-harness.md` 《DeepSeek Harness：DeepSeek 官方开源的全插件化 Agent Harness》

**可用观察** · 1

- `notes/2026-07/2026-07-30-sakana-fugu-model-orchestration.md` 《Sakana AI Fugu — 从自然启发的模型编排系统》

**未标注** · 1

- `risks/2026-07-12-ai-coding-agent-boundaries.md` 《AI 编码助手与多模型接入的安全边界》

### B · 企业 AI 落地 / 业务转译  (31)

**可小实验** · 12

- `experiments/2026-07/2026-07-03-locate-anything-nc-gui-automation.md` 《LocateAnything 用于 NC GUI 自动化抢救的可行性小实验》
- `experiments/2026-07/2026-07-04-flat-box-to-3d-mvp.md` 《平面盒型图生成 3D 立体图 MVP》
- `experiments/2026-07/2026-07-22-ai-open-source-roundup-validation-plan.md` 《GitHub 热点第 123 期开源 AI 项目验证计划》
- `experiments/2026-08/2026-08-04-revor-ai-poc-template.md` 《Revor AI 外贸销售智能体 PoC 模板：ICP + 指标 + 审核流程》
- `notes/2026-07/2026-07-04-chattts-dialogue-tts.md` 《ChatTTS：面向对话场景的开源语音生成模型》
- `notes/2026-07/2026-07-04-trellis-3d-generation.md` 《TRELLIS / TRELLIS.2 高精度 3D 资产生成》
- `notes/2026-07/2026-07-13-exercises-dataset.md` 《exercises-dataset：带动作 GIF 与多语言说明的健身动作开源数据集》
- `notes/2026-07/2026-07-13-roboflow-supervision.md` 《Roboflow Supervision：计算机视觉应用的统一工具层》
- `notes/2026-07/2026-07-17-douyin-ai-hidden-aha.md` 《AhaCreator：AI Native Influencer Marketing Platform》
- `notes/2026-07/2026-07-22-unlimited-ocr.md` 《Unlimited OCR：面向长文档的一次性 OCR / 文档解析模型》
- `notes/2026-08/2026-08-02-moneyprinterturbo-ai-video-pipeline.md` 《MoneyPrinterTurbo：开源短视频自动化流水线，而非“一句话出爆款”》
- `notes/2026-08/2026-08-07-prophet-holiday-features.md` 《Facebook Prophet：用节假日与特殊事件增强时间序列预测》

**已追源** · 1

- `notes/2026-08/2026-08-10-ai-weekly-roundup-aug9.md` 《AI 一周大事盘点（2026-08-03 ~ 08-09）多源核验笔记》

**待验证** · 7

- `notes/2026-07/2026-07-03-nvidia-locate-anything.md` 《NVIDIA LocateAnything：面向开放词汇视觉定位的 VLM》
- `notes/2026-07/2026-07-12-ornith-self-scaffolding-rl.md` 《Ornith-1.0：Self-Scaffolding RL 与 Agentic Coding 模型》
- `notes/2026-07/2026-07-22-github-hot-open-source-ai-roundup-123.md` 《GitHub 热点第 123 期：开源 AI 项目与行业报告线索整理》
- `notes/2026-07/2026-07-26-github-trending-ai-projects-weekly.md` 《GitHub 本周热榜 AI 开源项目速览》
- `notes/2026-08/2026-08-02-agent-reach-dy-xhs-self-evolving-knowledge-base.md` 《Agent Reach：把抖音与小红书接入 Agent 研究链路的公开方案》
- `notes/2026-08/2026-08-03-revor-ai-b2b-sales-agent.md` 《Revor AI：把 B2B 潜客挖掘、多渠道触达与商机分级交给销售智能体》
- `risks/2026-06-30-edm-tooling-naming-confusion.md` 《EDM 工具命名混淆：msmtp / Focussend / 询盘云 CRM 的边界》

**学习理解** · 8

- `notes/2026-06/2026-06-01-0g-qwen-hackathon-overview.md` 《0G APAC Hackathon + Qwen Cloud Hackathon 赛道总览（公开版脱敏）》
- `notes/2026-06/2026-06-01-ai-web3-hackathon-trends.md` 《2026 上半年 AI / Web3 黑客松赛道盘点（0G APAC、Qwen Cloud、AdventureX 等）》
- `notes/2026-07/2026-07-12-enterprise-agent-semantic-layer.md` 《企业 Agent 落地中的语义层建设共识》
- `notes/2026-07/2026-07-12-fde-adoption-timing.md` 《FDE 引入时机：先个人提效，再组织重构》
- `notes/2026-07/2026-07-12-wangziru-gree-digital-transformation.md` 《王自如格力数字化改革口播整理：传统企业数字化的组织与架构逻辑》
- `notes/2026-07/2026-07-13-excel-ai-adoption-path.md` 《全员 Excel 生态与 AI 个人提效：传统企业数字化的低阻力破冰路径》
- `notes/2026-07/2026-07-14-epiplexity-ai-data-selection.md` 《From Entropy to Epiplexity：用“可学习结构”重新理解 AI 数据价值》
- `notes/2026-08/2026-08-11-github-ai-week29-agent-native-stack.md` 《GitHub AI 2026 第29周：从单点模型热度到 Agent-Native 工具链》

**未标注** · 2

- `playbooks/enterprise-digital-transformation-layering.md` 
- `risks/2026-07-13-raw-material-private-boundary.md` 

### C · 学习方法 / 知识沉淀  (43)

**已沉淀** · 2

- `notes/2026-07/2026-07-05-fmhy-free-resource-index.md` 《FMHY：社区维护的免费资源索引与“资源采购替代”线索》
- `notes/2026-07/2026-07-25-5w1h-model.md` 《5W1H：需求与问题分析的最小澄清框架》

**已验证** · 1

- `playbooks/plain-language-translator-patterns.md` 《人话翻译机 Skill 实战模式汇总：Loop Engineering / TDD / GEO / 心跳机制》

**可业务试点** · 1

- `experiments/2026-04/2026-04-29-skill-knowledge-graph-runtime-bridge.md` 《Skill Knowledge Graph Runtime Bridge：从"skill wiki"到"skill runtime》

**可小实验** · 12

- `experiments/2026-04/2026-04-30-skill-graph-autopilot.md` 《Skill Graph Autopilot v0：让 skill 路由自进化的实验记录》
- `experiments/2026-06/2026-06-01-plain-language-village-translator.md` 《村口大白话翻译机"：把 AI / Web3 / 黑客松术语翻译给 48 岁非技术老板》
- `notes/2026-04/2026-04-19-llm-wiki-skill-karpathy-methodology.md` 《llm-wiki-skill：基于 Karpathy 方法论的多平台 Agent 个人知识库》
- `notes/2026-07/2026-07-02-notebooklm-code-research.md` 《NotebookLM 2.0：从资料问答升级为可执行代码的 AI 研究助理》
- `notes/2026-07/2026-07-03-karpathy-autoresearch.md` 《Karpathy AutoResearch：让 Agent 自动跑机器学习实验》
- `notes/2026-07/2026-07-04-graphify-knowledge-graph.md` 《Graphify：面向 AI 编程助手的项目知识图谱》
- `notes/2026-07/2026-07-04-next-ai-drawio.md` 《Next AI Draw.io：用自然语言生成和编辑 draw.io 图表》
- `notes/2026-07/2026-07-05-claude-video-skill-short-video-analysis.md` 《Claude Video Skill 与短视频复刻拆解能力》
- `notes/2026-07/2026-07-13-stanford-storm-research-method.md` 《Stanford STORM research method for AI-assisted knowledge curation》
- `notes/2026-07/2026-07-14-520520-industry-learning-method.md` 《520520 行业快速学习法》
- `notes/2026-07/2026-07-26-four-sentences-find-talent.md` 《四句话找到你的天赋（深度天赋挖掘机 Prompt）》
- `notes/2026-08/2026-08-13-mineru-pdf-document-parsing.md` 《MinerU：开源文档解析引擎，PDF/Office 转 Markdown/JSON》

**已追源** · 1

- `notes/2026-07/2026-07-26-openwiki-agent-shared-knowledge.md` 《LangChain OpenWiki — Agent 共享知识库的开源实现》

**待验证** · 7

- `notes/2026-07/2026-07-03-aliens-eye-osint-username-scanner.md` 《Aliens Eye：840+ 平台用户名 OSINT 扫描工具》
- `notes/2026-07/2026-07-04-firecrawl-no-api-agent-web-data.md` 《Firecrawl 免 API / Agent Web 数据获取线索》
- `notes/2026-07/2026-07-08-ppt-skill-routes-review.md` 《11 个 PPT Skill 横评与 PPT 生成路线判断》
- `notes/2026-07/2026-07-13-ai-digital-transformation-concept-map.md` 《AI 数字化转型概念对照表与溯源路线图》
- `notes/2026-07/2026-07-25-ai-github-organization.md` 《AI 时代组织形态：从 AI 版飞书到 AI 版 GitHub》
- `notes/2026-07/2026-07-30-alibaba-open-code-review.md` 《阿里 OpenCodeReview — 确定性工程 + LLM Agent 混合架构的代码审查 CLI》
- `notes/2026-08/2026-08-12-deeptutor-hku-ai-tutor.md` 《DeepTutor：港大 HKUDS 开源的终身个性化 AI 私教（答疑/出题/复习一体化）》

**待追源** · 1

- `notes/2026-07/2026-07-05-feishu-demoday-ppt-howto.md` 《飞书 DemoDay PPT 做法分享线索》

**学习理解** · 14

- `notes/2026-05/2026-05-12-formal-explain-once-stake-frame.md` 《「正式解释一次 + 拉起利益目标」团队规则重置框架》
- `notes/2026-05/2026-05-15-manager-eight-mindsets-question.md` 《管理者八大心智问答框架（与英领计划 Module 01 对齐）》
- `notes/2026-05/2026-05-22-tinyhumans-openhuman.md` 《OpenHuman（tinyhumansai）：本地优先的个人 AI "超级智能" 助手》
- `notes/2026-06/2026-06-06-net-ease-uu-remote-mac.md` 《网易 UU 远程 for Mac：免费 4K 144 帧远控工具（团队远程协助场景的可选项）》
- `notes/2026-06/2026-06-27-plain-language-translator-cross-platform-package.md` 《人话翻译机 Skill 跨平台发布包设计：core + adapters 三段式》
- `notes/2026-06/2026-06-30-ai-learning-youtube-videos-for-beginners.md` 《普通人入门 AI 的 3 个 YouTube 视频（按"学习曲线"排序）》
- `notes/2026-06/2026-06-30-youtube-ai-creators.md` 《YouTube 推荐的 5 个 AI / 商业化创作者》
- `notes/2026-07/2026-07-14-competing-against-luck-jtbd.md` 《与运气竞争：用 JTBD 从用户画像转向用户任务》
- `notes/2026-07/2026-07-15-bitter-lesson-era-of-experience.md` 《Bitter Lesson 与 Era of Experience：从人类经验编码转向经验驱动智能》
- `notes/2026-07/2026-07-26-cloudflare-growth-flywheel-edge-ai.md` 《Cloudflare 从反垃圾邮件到边缘 AI 基础设施的增长路径》
- `notes/2026-07/2026-07-28-xhs-weekly-open-source-july-third-week.md` 《GitHub 周榜（7月第三周）：5 个 AI 开源项目的原始仓库核验》
- `notes/2026-08/2026-08-09-llm-agent-concepts-map.md` 《从 LLM 对话到 Agent：Skill、MCP、RAG 的分层与落地判断》
- `notes/2026-08/2026-08-13-meta-new-mexico-youth-harm-ruling.md` 《Meta 新墨西哥州青少年危害案：9.42 亿美元判决与平台成瘾设计问责》
- `notes/2026-08/2026-08-14-mit-how-to-speak-winston.md` 《MIT 传奇公开课《How to Speak》——Patrick Winston 的演讲方法论》

**暂不建议** · 1

- `notes/2026-08/2026-08-07-lieflat-charts.md` 《Lieflat Charts：把 Agent 生成图表从“套模板”推进到“按阅读速度与数据契约选型”》

**未标注** · 3

- `essays/2026-07-13-ai-digital-transformation-study-doc.md` 
- `notes/2026-07/2026-07-29-vivek-how-to-be-good-at-research.md` 《Vivek Nair: How to Be Good at Research》
- `risks/paid-course-eight-mindsets-boundary.md` 

### D · Hermes / 飞书 / 内部运维  (26)

**已沉淀** · 4

- `notes/2026-06/2026-06-28-hermes-agent-self-introduction.md` 《Hermes Agent 一句话介绍 + 六层能力拆解（来自打捞处群聊现场回应）》
- `notes/2026-07/2026-07-05-feishu-cli-multi-user-isolation-status-check.md` 《飞书 CLI 多用户隔离现状盘点（2026-07-05）》
- `playbooks/candidate-evaluation-framework.md` 《候选人评估框架 Playbook（四维度 + 面试官脚本模板）》
- `risks/2026-05-wechat-personal-bridge-risk.md` 《微信个人号桥接风险：ilinkai.weixin.qq.com 通道不可控》

**已验证** · 6

- `experiments/2026-07/2026-07-05-feishu-cli-isolation-audit-log-analysis.md` 《实验 - 飞书 CLI 多用户隔离审计日志 30 分钟快读》
- `playbooks/codex-device-code-auth-flow.md` 《Codex Device Code 授权流程 Playbook（OpenAI ChatGPT Team / 个人账号）》
- `playbooks/feishu-wiki-pdf-export-no-pagebreaks.md` 《飞书 Wiki 文档转 PDF（不分页版）Playbook》
- `playbooks/hermes-default-model-rotation.md` 《Hermes 默认模型切换 Playbook（OpenAI / 内网代理）》
- `playbooks/intco-modelhub-provider-setup.md` 《IntcoModelHub Provider 配置 Playbook（重命名 + 模型清单 + 网关切换）》
- `playbooks/leaver-resource-cleanup-checklist.md` 《离职/停合作员工资源回收清单 Playbook（账号、Key、权限、订阅）》

**可业务试点** · 4

- `experiments/2026-05/2026-05-11-auto-reminder-requirement-extraction.md` 《跟单提效｜收款预警降噪与自动催款：从聊天/会议抽需求填项目表 MVP》
- `notes/2026-05/2026-05-05-feishu-cli-multi-user-isolation.md` 《飞书 CLI 多用户隔离 + 隐私安全：从「嫂子开门」到「用户授权各自存」》
- `notes/2026-05/2026-05-26-internal-tool-launch-wechat-announcement.md` 《内部工具发布到微信大群的"功能宣传 + 操作指南"双轨文案方法论》
- `notes/2026-05/2026-05-28-half-year-report-content-method.md` 《半年度工作汇报 PPT 内容方法论：从"成果陈列"升级为"问题洞察 + 管理升级》

**可小实验** · 4

- `experiments/2026-07/2026-07-04-history-backfill-workbench.md` 《打捞处历史内容补录首轮工作台》
- `notes/2026-04/2026-04-12-resend-agent-friendly-email-api.md` 《Resend：面向 AI Agent 的事务邮件 API 与 EDM 接入路径》
- `playbooks/company-public-osint-boundary.md` 《公司公开信息核验的 OSINT 使用边界》
- `playbooks/dnspod-resend-dns-records-setup.md` 《DNSPod + Resend 域名 DNS 三连记录配置（SPF / DKIM / Verification）》

**待验证** · 3

- `notes/2026-07/2026-07-22-hermes-moa-2-0.md` 《Hermes MoA 2.0：用多模型合议提升 Agent 输出质量》
- `playbooks/codex-entry-from-feishu.md` 《Codex 编程入口选型对比》
- `playbooks/feishu-wiki-safe-read.md` 《飞书 Wiki 安全读取 Playbook》

**学习理解** · 2

- `notes/2026-06/2026-06-28-hermes-heartbeat-mechanism-finding.md` 《Hermes Agent 心跳机制现状与"心跳 + 自愈"补充建议》
- `notes/2026-06/2026-06-30-agent-self-investigation-vague-user-request.md` 《Agent 自查流程：当用户说"我记得装了 X 工具"时怎么找》

**未标注** · 3

- `playbooks/auto-sync-validation.md` 
- `playbooks/feishu-to-github-auto-sync.md` 
- `playbooks/feishu-topic-history-backfill.md` 

### E · 其他单次扫描 / 未归类  (9)

**可小实验** · 2

- `experiments/2026-07/2026-07-12-multi-model-coding-sandbox.md` 《多模型编码助手与安全沙盒小实验》
- `notes/2026-07/2026-07-26-agent-7-architectures.md` 《Agent 7种架构：从入门到企业级 — AlunTalk 视频笔记》

**待验证** · 3

- `notes/2026-07/2026-07-04-esp-claw-chat-coding-iot.md` 《ESP-Claw：面向物联网设备的 Chat Coding Edge Agent》
- `notes/2026-07/2026-07-04-harness-open-source.md` 《Harness Open Source：端到端开源软件交付平台》
- `notes/2026-08/2026-08-13-deepseek-v4-pro-open-source-vs-closed-frontier.md` 《DeepSeek V4 Pro 与国产开源模型围攻闭源旗舰——抖音研报速览与事实核验》

**待追源** · 1

- `notes/2026-07/2026-07-02-ps-automation-local-ai.md` 《PS 自动干活：本土 AI/自动化工具用于设计提效》

**未标注** · 3

- `playbooks/daily-ingestion-briefing.md` 
- `risks/historical-chat-backfill-boundary.md` 
- `risks/public-boundary.md` 

## 三、需要止损 / 推进的两层

- 23 篇已沉淀 / 已验证 / 可业务试点 → 见 `stoploss-review-2026-08.md` 第一节（能力底盘）
- 57 篇可小实验 → 同文件第二节（三档止损：会做 / 降级 / 归档）

## 四、下阶段聚焦建议

1. **主战场 = A**：以 Boris Cherny 团队 tips + Anthropic Skills spec + Bitter Lesson 为锚，把「软件实施一组把 Codex/Claude Code 接进飞书并工程化」做成一篇主案例 + 一篇 playbook。
2. **B 区分雷达与试点**：音视频/内容/获客类工具继续保持扫描（服务市场部 BP 雷达），不需要每条都找主营业务对应；只有真正要推试点的条目才拉业务问题对齐。现有「可小实验」标签可拆成 `雷达观察` 与 `待试点` 两档，避免把"还没业务挂钩"误判成"待清理"。
3. **C 收敛成「研究方法」三件套**：5W1H、520520、Vivek research 方法合并成一篇 `playbooks/research-question-framework.md`，停止重复采集同类方法论。
4. **D 单独归档进 `ops/` 或保持原位但不占用学习注意力预算。**
5. **E 每周合并一篇 roundup，不再单开 notes。**
