---
title: "nanoMuse：开源版 Meta Muse 个人智能体（手机+桌面+浏览器一个 agent）"
date: 2026-10-22
discovery_source:
  type: 短视频
  title: 抖音 @哈工大Peter「nanoMuse 把个人 Agent 装进所有设备」（合集 Peter的AI工具大全 第229集，点赞3.2k）
  url: ""
primary_object:
  type: open_source_project
  name: nanoMuse
  url: https://github.com/nano-muse/nanoMuse
object_type: [open_source_project, trend_signal]
source_type: [GitHub, 官网, 论文, 短视频二手线索]
business_tags: [ITBP, 个人能力, 市场]
problem_tags: [流程提效, 组织协同]
method_tags: [Agent, computer-use, MCP, 自动化, 跨端协同]
tool_tags: [nanoMuse, DeepSeek-Harness, Android, GPL-3.0]
value_stage: 待验证
risk_tags: [数据安全, 权限, 国内可用性, 合规]
public_level: public
---

# nanoMuse：开源版 Meta Muse，一个 agent 跨手机/电脑/浏览器

## 1. 这是什么

2026-09 Meta 发布闭源个人智能体 Muse（有名字、有形象、常驻云端、跨设备、能操作账号和设备）。nanoMuse 是它的开源对位实现（论文 arXiv:2610.08699，12 页，2026-10），定位"个人智能体不该只长在别人的云里"：

- **一个 agent 跨所有设备**：Android APK（8.0+/arm64，APK 内打包 Alpine Linux + shell + 浏览器 + MCP + skills）、Windows/macOS/Linux 桌面端、浏览器版；手机说一句、电脑上执行，双向。
- **动手而非只答**：shell、浏览器、MCP server、技能，对没有 API 的 App 直接"屏幕当手"（computer-use，仅截图驱动）。
- **Sentinel 审批层**：删除/发送/付款等不可逆操作前停下来问，授权可记范围（一次/本对话/对某收件人或目录永久）。
- **App 关了也继续干**：定时 goals、routines、每天早上写 feed。
- **记忆即文件**：身份/了解你的信息/记忆/唤醒时机 = 四个可读可改的 Markdown 文件。
- **模型自选**：任何 OpenAI 兼容端点；官方 Cloud 免费额度由项目方出钱（默认模型演进到 DeepSeek V4 Pro，推荐阿里云百炼 BYOK）；中继（relay）可完全自建，数据不出门。
- **可远程指挥 coding agent**：电脑上的 Cursor/Codex/Claude Code 会话能从手机看和操控，不往它们里装东西、不经第三方服务器——这条和 Crystal"飞书话题群当云端总线、手机指挥 coding harness"的构想是同方向。
- 桌面端建在 DeepSeek 开源的 DeepSeek Harness 之上（nanoMuse 是其 bundle/插件包形态）。

## 2. 原始来源

- 代码：https://github.com/nano-muse/nanoMuse （GPL-3.0-or-later，全仓库含中继都开源）
- 官网/在线 demo：https://nanomuse.cn 、https://nanomuse.cn/web/ （手机号或邮箱登录，浏览器里是模拟手机）
- 论文：https://arxiv.org/abs/2610.08699 （HF papers 页面显示作者挂浙江大学（ZJU）标识；论文含从 Meta 公开资料+一份泄露的 Muse 生产 prompt 反推的架构拆解，每条主张标了出处）
- 发现入口：抖音二手视频（未看口播，只凭截图+一手仓库核实）

## 3. 一手核查到的事实（2026-10-22 GitHub API 实查）

- 仓库 **2026-09-23 创建，刚满一个月**；2,153 stars / 244 forks / 75 open issues；最近推送 2026-10-21（昨天），迭代极快（版本从 0.1.26 到 0.1.31 每个小版本都有完整中文 changelog）。
- **巴士因子≈1**：主力提交者 lgy0404（Guangyi Liu）1482 次提交，第二名仅 35 次；本质上是一个人的高强度项目 + 少量外部 PR。非营利、无公司主体迹象。
- 语言体量：Swift 12.8MB（iOS/桌面）、Kotlin 11.8MB（Android）、Python 4.5MB（runtime）、TS 3.5MB（relay/网页）。
- 0.1.31「Locks」版本的 changelog 显示刚做完一轮 27 点安全审计：URL 不带 token、被控设备先同意、Enter 审批语义、CLI 环境清洗、relay 限流、展示站会话隔离、收窄 Docker socket——说明作者安全意识在线，但也反证此前攻击面不少。
- 安装包**未经 Apple 公证/Windows 签名**，需手动"仍要打开"；iOS 暂无原生 App（只能浏览器版），路线图上下一步是 iOS 和眼镜。

## 4. 视频与一手信源的矛盾点（待验证）

- 视频截图网页显示 "stars 609" 和 "最新版本 1.0.5 Lookout"；但实查官网当前版本是 **0.1.31**，release 序列里没有 1.0.5。star 数视频时点更早可以解释 609，但版本号对不上——可能是视频剪辑套用了错误/自制素材，或截图 OCR 误差。**视频里的具体数字不要引用，以 GitHub 为准。**
- Trendshift "#7 Repository of the Day" 是单日榜，属营销徽章，不代表成熟度。

## 5. 我学到了什么（认知价值）

- **个人 agent 的形态共识正在收敛**：有身份（名字/形象）、跨设备一个会话、常驻+定时主动开口、记忆可审计、不可逆操作分级审批、模型与云可替换。这套定义和我们给 J-Herm/Hermes 沉淀的做法（记忆文件、scoped 审批、relay、技能、cron）高度同构——方向被外部独立项目再次验证。
- 论文把"个人 agent"用五个问题+三个阶段做了学术化定义，并把 Muse 生产 prompt 当一手材料拆——是了解闭源 Muse 架构的低成本入口。
- **"手"的分层**：API/shell/MCP 优先，屏幕操作（截图驱动）只兜底无 API 的 App——与我们 RPA"无接口补丁论"一致。

## 6. 对企业 AI 落地 / ITBP 的价值

- 雷达观察级：这是"个人 agent 开源平替"赛道的早期信号（同类还有 openminis/openmuse 话题），值得观察其 relay 自托管、Sentinel 审批、手机端 computer-use 的工程做法，反哺我们自己的多端/审批设计。
- 不建议企业场景直接采用：一个月新项目、单维护者、未签名安装包、APK 内带完整 Linux+shell 且能读屏操作，企业设备上攻击面和供应链风险都过高。
- 个人想试：只用 nanomuse.cn/web 浏览器 demo（不装 App、不给手机屏幕权限、不接含公司数据的 key），成本为零、风险可控。

## 7. 风险和边界

- **权限**：Hands 开了之后能看屏幕、点 App、执行 shell；登录/付款/删除虽暂停审批，但误授权范围（"always allow"）后风险大。禁止在工作手机上装。
- **数据安全**：用官方 Cloud = 手机号/邮箱注册、会话过项目方中继（作者自掏腰包，可持续性无保障）；要隐私须自建 relay。
- **GPL-3.0 传染性**：若未来想借鉴/搬代码进内部项目，注意 copyleft 约束，只能看思路不要抄代码。
- **可持续性**：非营利 + 单主力 + 免费模型额度烧钱，存在停更/额度终止风险；论文作者背书（浙大）≠ 组织承诺。

## 8. 当前结论

价值阶段：**待验证 / 雷达观察**。本体真实存在、迭代活跃、工程完成度比一般一周项目高（有论文、测试、多端安装包、安全审计），但年龄一个月、巴士因子 1、视频宣传素材有事实硬伤。先不安装不接入；浏览器 demo 可作为体验"个人 agent 产品形态"的样本；其 Sentinel 审批和 relay 架构文档值得后续单独细读，对照我们自己的 scoped 审批设计。

## 9. 下一步

- 若要深挖：读 harness/README.md 与 Sentinel 审批实现、relay 自部署文档，评估可借鉴点（只看不抄，GPL）。
- 观察指标：3 个月后维护者是否仍是单人、iOS 版是否落地、免费额度政策是否变化。
