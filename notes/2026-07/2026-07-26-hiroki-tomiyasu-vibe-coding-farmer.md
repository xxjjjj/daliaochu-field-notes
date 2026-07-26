---
title: "北海道农民富安宽树的VibeCoding实践：100公顷农场的AI自动化"
date: 2026-07-26
discovery_source:
  type: 抖音短视频
  title: "一个北海道农民竟通过VibeCoding实现了500万营收？"
  url: https://v.douyin.com/6Vrf8BVw4pQ/
primary_object:
  type: 人物案例
  name: Hiroki Tomiyasu（富安宽树）
  url: https://chatgptpro.substack.com/p/hiroki-tomiyasu
object_type: [case_or_media]
source_type: [抖音, OpenAI官方社区, note.com]
business_tags: [ITBP, 个人能力, 管理]
problem_tags: [流程提效, 知识沉淀]
method_tags: [Vibe Coding, Agent, 自动化, IoT]
tool_tags: [ChatGPT, Codex, ESP32, Cloudflare Workers, LINE Bot, Airtable, D1]
value_stage: 学习理解
risk_tags: [国内可用性]
public_level: public
---

# 北海道农民富安宽树的VibeCoding实践

## 1. 这是什么

OpenAI 官方 ChatGPT Pro 社区 2026年6月1日发布的正式人物案例。主角是北海道的农民富安宽树（Hiroki Tomiyasu，@tomiyasu16），前公务员、零编程/农业背景，通过 ChatGPT + Codex 自主搭建了一整套农场数字化管理系统，经营约100公顷农田（西兰花、南瓜、大葱、大豆）。抖音视频中提到的“500万人民币营收”来自博主柱子哥TzFilm的口述，OpenAI 原文未提及具体营收数字，需标注为二手信息待验证。

富安被 OpenAI 邀请参加官方直播活动，并在 OpenAI 官方视频中被作为自动化应用代表案例推广。他在 note.com 上以“農家のAI開発ログ - Vibe Coding実践ラボ”为题持续记录开发过程。

## 2. 原始来源

- **发现入口**：抖音「柱子哥TzFilm」视频。视频中的“VibeCoding”框架是中国博主加的，OpenAI 原文未使用该词；但富安本人在 note.com 上确实使用了“Vibe Coding”标签。
- **资料本体**：OpenAI ChatGPT Pro 社区文章《A broccoli farmer in northern Japan shares his chats》(2026-06-01)
- **富安本人**：[X @tomiyasu16](https://x.com/tomiyasu16)｜[note.com/tomiyasu16](https://note.com/tomiyasu16)
- **GitHub**：[github.com/tomiyasu0428](https://github.com/tomiyasu0428) — 103 个公开仓库，87 contributions/year，5 followers
- **相关链接**：
  - OpenAI 原文：https://chatgptpro.substack.com/p/hiroki-tomiyasu
  - 富安 greenhouse demo 推文（702K views）：https://x.com/tomiyasu16/status/2038922364956914167
  - Gotchaa Lab 分析文章：https://gotchaa-lab.com/blog/2026-06-21-ai-non-technical-founders-broccoli-farmer

## 3. 核心观点 / 核心能力

OpenAI 原文列出了富安用 AI 实现的 **8 个具体用例**（带原始 Prompt，从日文翻译）：

### 3.1 作物病害诊断
拍照问 ChatGPT：黑斑是什么病、要不要处理。轻症不用找专家、重症及时干预。

### 3.2 卫星遥感监测
基于自有地块数据拉取 NDVI 植被指数，叠加到已建地图 App 上，实现地块级决策支持。

### 3.3 设备接线图自动标注
拍一张控制柜内部照片，让 Images 2.0 标注每个部件的名称、归属和系统工作原理。日文标注完全正确。

### 3.4 温室卷帘远程控制（核心案例）
- **硬件**：ESP32 + BTS7960 电机驱动 + 24V DC 卷帘电机 + ON-OFF-ON 手动开关
- **软件栈**：Cloudflare Workers + D1 数据库 + LINE Bot
- **流程**：手机 LINE 发送“open/close/stop” → 命令存入 Workers → ESP32 轮询拉取 → 驱动电机
- **安全**：Prompt 中要求了 safety considerations

### 3.5 农场群聊 Bot
在 LINE 群聊里加 Bot，功能：查各温室温度、操作卷帘、查作业排期。设计约束：按钮少、日文简单、手机好按、不易误操作。

### 3.6 群聊日志自动统计播种量
从农场群聊历史记录中自动计算各轮次西兰花育苗盘数。

### 3.7 RTK-GPS 自动导航原理学习
先让 ChatGPT 解释 RTK 原理、所需组件、开源项目，发现自建仅需几十万日元，大幅扩展了选择空间（而非直接买几十万的商用系统）。

### 3.8 农场管理 App 数据库设计
假设用 Airtable，设计地块、作物、计划任务、已完成任务、工人、物料、农药、化肥、大棚、传感器数据等表结构，关联查询支持“今天有什么任务？”“这个地块下一步做什么？”“这个温室现在多少度？”

## 4. 技术架构全貌

富安的技术栈非常具体且可验证：

| 层级 | 技术 |
|------|------|
| AI / 代码生成 | ChatGPT, Codex (OpenAI), Cursor, Claude |
| IoT 硬件 | ESP32, BTS7960 电机驱动, 24V DC 电机, 树莓派 |
| 后端 / API | Cloudflare Workers (边缘计算), Cloudflare D1 |
| 数据库 | Cloudflare D1, Airtable, MongoDB |
| Agent 框架 | LangChain |
| 消息通道 | LINE Bot / LINE Messaging API |
| 前端 / 地图 | 自建地图 App, PWA (GitHub Pages), Android WebView |
| 文档记录 | note.com（日文博客平台） |
| 开发方法 | CLAUDE.md / Skills 目录（Claude Code 体系），AGENTS.md 风格 |

**GitHub 仓库已确认公开，核心仓库：**

| 仓库 | 描述 | Commits | 关键内容 |
|------|------|---------|----------|
| [agri-ai-agent](https://github.com/tomiyasu0428/agri-ai-agent) | 农场 AI Agent 主系统 | 25 | LangChain + MongoDB + LINE Bot，MIT License，CLAUDE.md，已迁移 427 条数据 |
| [ai_housefarmnae](https://github.com/tomiyasu0428/ai_housefarmnae) | "Project Nae" 大棚 IoT | 4 | ESP32 ADC 扫描、土壤校准、树莓派、Cloudflare Workers，含 openclaw-raspberry-pi-setup.md |
| [agri_line1](https://github.com/tomiyasu0428/agri_line1) | 拖拉机 GNSS 直线导航 PWA | 45 | "StraightBar Lite" v0.2.6，navigator.geolocation swath 偏移计算，Android WebView 封装 |
| [Farm_AIagent3~7](https://github.com/tomiyasu0428/Farm_AIagent7) | 农场 Agent 多版本迭代 | 13 | TypeScript 栈，要件定义书 + 任务列表 |
| satelite_agri / satelite_everyone | 卫星遥感 NDVI 监测 | — | 对应 OpenAI 文章中的卫星监测功能 |
| 100try85~95 系列 | 实验日志 | — | MCP 日历、Google SDK、LINE 管理、智能财务等至少 95 次实验 |

仓库命名含 `100try85` 到 `100try95` 系列，说明至少做了 95 次实验尝试。note.com 杂志描述写的是"AI（Cursor/Claude）を相棒に"——主力工具是 Cursor + Claude，不是 Codex（OpenAI 文章强调 Codex 是因为那是他们的产品）。ai_housefarmnae 里有 `skills/` 目录和 CLAUDE.md，说明在用 Skill + Claude Code 体系做物理世界自动化。

**身份交叉验证**：`ai_housefarmnae/project-nae-update-log.md` 中设备标识为 `tomiyasu16@raspberrypi.local`（X 账号），Instagram 帖子里出现了 `tomiyasu0428.github.io`，GitHub 资料显示 Hiroki Tomiyasu + Hokkaido。三重交叉确认是同一人。

## 5. 我学到了什么

### 对 BP 服务的业务启发
1. **“群聊作为操作中心”这个模式可以直接借鉴**：业务团队日常就在飞书群里，如果能把 CRM 查询、流程触发、数据统计直接嵌入群聊 Bot，学习成本接近于零。
2. **Airtable + Bot + 自然语言查询**的模式，对应到 INTCO 场景就是：多维表格/CRM + 飞书 Bot + NL2SQL，这个方向技术上已经可行。
3. **“先让 AI 讲原理，再决定买还是造”**——RTK-GPS 那段的思路非常好。IT 帮业务评估采购时，可以让 AI 先拆解技术原理和自建成本，作为谈判基准。

### 对 IT 自身能力的启发
1. **ESP32 + Cloudflare Workers + 消息 Bot 是一条已验证的轻量 IoT 链路**，成本可控、无需专业工控设备。
2. **Prompt 质量来自领域知识**：富安每一个 Prompt 都包含精确的硬件型号（ESP32、BTS7960、24V DC），AI 才能生成可用的代码和接线方案。不懂业务的 Prompt 做不到这个精度。
3. **从单体 App 思维转向 Chat-first 思维**：富安没有做一个独立 App，而是在已有的 LINE 群里加 Bot、用 Airtable + 聊天日志做数据源。这在企业场景同样适用——与其推一个新系统，不如在已有的飞书群里加能力。

### 通用认知
1. 这个案例证明了 Andrej Karpathy 和 OpenAI 一直强调的方向：**AI 最大的价值不是让工程师更高效，而是让领域专家获得工程师级别的技术能力。**
2. 抖音视频中“懂行业比懂技术更重要”这个判断是对的，但不够精确。更准确的是：**精确的领域知识 × AI 工具 = 可用的工程产出**。前提是领域知识必须精确到能写进 Prompt 的程度。
3. 500万营收这个数字未在 OpenAI 原文中确认，但即使打折扣，一个非技术背景的人用业余时间搭建出可运行的 IoT + 数据库 + Bot 系统，本身就是强信号。

## 6. 可信度与待验证

| 维度 | 判断 |
|------|------|
| OpenAI 原文 | ✅ 可信，官方社区一手材料 |
| 8 个用例 Prompt | ✅ 原文提供了完整 Prompt |
| 技术栈细节 | ✅ ESP32/BTS7960/Cloudflare Workers 具体可查 |
| GitHub 代码公开 | ✅ 103 仓库已确认，核心仓库有完整结构 |
| 500万人民币营收 | ⚠️ 抖音博主的二手数字，OpenAI 原文未提；需打问号 |
| 实际运行效果 | ⚠️ 有视频 Demo，代码已公开可查，但未见系统级验证报告 |

## 7. 可做的小实验

1. **飞书群 Bot + 多维表格 + 自然语言查询**：选一个业务群，用现有飞书 Bot 能力做一个“查数据/查排期/查状态”的最小可用版本，验证群聊作为操作界面的可行性。
2. **“先问 AI 再买”的采购辅助流程**：选一个业务方正在评估的软件/硬件采购项，让 AI 先拆技术原理、开源替代和自建成本，输出一份对比表，看能帮业务省多少谈判空间。

## 8. 风险和边界

- LINE Bot ↔ 飞书 Bot 的 API 差异较大，不能直接照搬技术栈。
- Cloudflare Workers 在国内访问可能有延迟问题。
- ESP32 IoT 部分属于物理世界自动化，INTCO 的核心场景更多在软件/流程/数据层，硬件自动化不是当前重点。
- 富安案例是个人实践，未涉及团队协作、权限管理、合规审计等企业级需求。

## 9. 当前结论

这是一个高质量的、官方背书的一手案例。核心价值不在于农业本身，而在于它证明了 **“领域专家 + AI 工具”可以产出专业级工程系统**这个范式。对 INTCO IT 的启发集中在“群聊 Bot + 数据库 + 自然语言查数据”这个模式的可复制性上。500万营收数字暂不采信。
