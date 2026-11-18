---
title: "CrommaCode：把 agent 工具调用变成游戏的桌面终端（抖音线索一手核验）"
date: 2026-11-19
discovery_source:
  type: 短视频文字稿
  title: "不是哥们，Agent可视化都做成这样了？（AI前端仔 第175集）"
  url: "https://v.douyin.com/wsTE4Rhe46E/"
primary_object:
  type: commercial_product
  name: "CrommaCode (Cromma)"
  url: "https://cromma.ai/code"
object_type: [commercial_product, trend_signal]
source_type: [群聊线索, 官网, GitHub]
business_tags: [ITBP, 产品, 个人能力]
problem_tags: [流程提效, 组织协同]
method_tags: [Agent, Vibe Coding, 自动化]
tool_tags: [CrommaCode, Claude Code, Codex, Gemini CLI, Cursor CLI, OpenCode, GPUI, Rust]
value_stage: 学习理解
risk_tags: [版权, 数据安全, 权限, 成本]
public_level: public
---

# CrommaCode：把 agent 的工具调用演成一场游戏

## 1. 这是什么

抖音「AI前端仔」第 175 集（2026-11-18 08:00 发布，31 秒）介绍的 **CrommaCode**：一款免费的桌面终端，你在它里面照常跑自己的编程 CLI（Claude Code / Codex / Gemini CLI / Cursor CLI / OpenCode），它通过 CLI 的 tool hook 把每一次工具调用读出来，在终端旁边把整场会话**演成一个游戏**。

视频里三个画面全部对得上官方 README 的五个模式：

| 视频里的说法 | 官方模式 | 实际是什么 |
|---|---|---|
| "大转盘跟着转" | **Westworld** | 一台精密仪表盘，每个工具一个槽位，指针转向当前调用的工具，能量充满后激光发射 |
| "工厂模式，机械臂搬箱子" | **Factory** | 传送带 + 六轴机械臂，每次调用是一个箱子，思考越重箱子越大 |
| "多个 Agent 派一群小蜜蜂" | **Bees** | 主 agent 写 brief，派出 worker 蜂，各自落在像素草地上，落地即完成 |

另外还有 Loom（织机，一次调用织一行）和 Rhythm（音游，每个工具一条轨道，PERFECT/GREAT/GOOD 判定）以及 Chips（牌桌，工具是手牌，按 chip × multiplier 计分）。

它不只是"游戏皮肤"，本体是一个完整 IDE 壳：项目/终端/文件侧栏、tree-sitter 高亮只读阅读器、Git 分支地铁图、Traces（这次会话被什么影响）、全机 token 统计，以及一个开发者排行榜。

## 2. 原始来源

- 发现入口：打捞处群转发抖音文字稿 + `https://v.douyin.com/wsTE4Rhe46E/`（解析后为 `douyin.com/video/7709104454129257755`）
- 资料本体：
  - GitHub `CrommaAI/crommacode`（2026-11-12 创建，核验时 87 star / 8 fork）https://github.com/CrommaAI/crommacode
  - 官网 https://cromma.ai/code
  - 下载清单 https://dl.cromma.ai/code/stable/latest.json（核验时 version 0.3.28，2026-11-18 09:07 UTC 发布）
- 相关链接：
  - 作者 X：@chunxiangai（赵纯想），2026-11 两条推文介绍 CrommaCode
  - 中文二手覆盖：掘金、ai-bot.cn、watcha.cn 均为 README 转述，无独立实测

## 3. 核心观点 / 核心能力

1. **游戏是旁路，不是主路。** hook 每次调用只往本地一个文件夹写一行；App 只读不写。官方明确：不进 agent 的 context、不额外耗 token、不改变 agent 行为。"游戏不能改变 agent 做什么"——这是整个设计最关键的一句。
2. **每次调用恰好落一次。** 并行 sub-agent 不打乱顺序，重复调用折叠成 ×N，还能显示积压队列。这是把 trace 做成"可读节拍"的核心工程活。
3. **仪表盘叙事。** 官方原话："机器就是解释"——表盘转动即工具切换，织机一行即一次调用，传送带一箱即一次调用。用仪表替代日志，让人一眼读懂"在干什么、多用力、卡没卡"。
4. **Bees = 主 agent 写 brief + worktree 隔离的 worker 群。** 每只蜂是 headless agent，独占一个 git worktree 和 branch，主 agent 负责计划、写 brief、合并、验证；模型 provider 在设置里选，多 provider 优先级降级。OpenAI 兼容的走 `127.0.0.1` 本地网关转换，真 key 不离开本机进程。
5. **成本模型。** App 免费、无账号、无付费档；排行榜登录可选（Google/X via VibeLoft），只上传每日 token 合计。

## 4. 我学到了什么

- **"等待"本身是一个可设计的产品面。** 视频作者的爽点是"以前盯着终端干等，现在不用干等这么无聊"——它卖的不是监控，是把用户被迫等待的注意力接住了。这一层洞察对内部工具同样成立：任何"跑 3 分钟以上、中间没反馈"的流程（RPA、批量取数、日报生成），都缺一个这样的节拍层。
- **可视化要做成"仪表"而不是"图表"。** 我们习惯做折线/柱状/漏斗，那是在事后解释；它做的是实时仪表——指针指到哪个工具、能量充到几格。区别是：图表要读，仪表只要瞟。
- **agent 可视化的正确抽象是"每次工具调用一个 beat"，不是"每个 token 一行"。** 因为人判断"它卡住了吗"只需要节拍密度，不需要内容。
- **worktree 是最好的并行隔离单位。** 比进程隔离便宜，比共享目录安全，主 agent 合并时冲突面小。这和"贵模型动脑、便宜模型动手"天然同构：主 agent 定 brief，便宜模型在各自 worktree 里干脏活。
- **闭源也能靠"公开的架构文档"建立信任。** 仓库不放源码，但把 hook/蜂巢/网关的边界、隐私边界、更新签名机制写清楚了——这本身是一种可借鉴的对外说明方式。

## 5. 它是否可信，哪些需要验证

**一手可查（高可信）：** 仓库存在与星数、官网定位与下载清单、当前版本 0.3.28、五档 Game View 模式、Bees 的 worktree/branch 机制、本地网关与 127.0.0.1 限制、更新走 minisign 签名 + macOS 公证、免费无账号。以上均来自 GitHub API/README/官网/`latest.json`，2026-11-19 实查。

**厂商自述，未独立验证：** "不拖慢、不耗 token"（原理上成立，旁路读文件，但只在真机长会话下才能证伪）；Bees 的实际合并质量、蜂群在真实仓库上的冲突率，README 没有任何 benchmark 或案例。

**我没有验证的：** 我没有安装运行它。原因见风险第 1 条——不在晶晶的生产工作机上装来源不可审计的闭源二进制；观感只能依据官方演示素材，属于厂商素材，不算独立实测。

**样本极小：** 仓库 7 天龄、87 star、无 release（只有 latest.json 直链）、0 issue；同一天内版本从 0.3.14 迭代到 0.3.28，0.x 阶段特性与配置随时会变。判断"这东西成不成"，现在没有足够证据，只能判断"这个做法值得学"。

## 6. 对个人能力有什么价值

- 直接可搬的是 **Bees 的编排范式**：主 agent 写 brief → worker 各占 worktree → 主 agent 合并验证。这正好是我们"Codex 出计划、便宜模型动手"的现成施工图，且把"隔离"落到了 git 层，比开多个终端干净。
- 次要可搬的是 **节拍可视化的取舍**：哪些事件算一个 beat（读/改/搜/执行/派子 agent 各一次），重复怎么折叠，积压怎么显示——这套规则可以直接写进我们自己 agent 的日志/看板规范。

## 7. 对企业 AI 落地有什么价值

业务方不会读日志，只会问"它到底在干嘛、是不是卡了"。数字员工/AI 伙伴落地到业务侧，最缺的恰好是这一层"瞟一眼就知道状态"的界面。可迁移的三条：

1. **状态要分档且外显**：在干活 / 卡住 / 等人授权 / 已完成，四档用指针、能量、颜色表达，不用文字报表。
2. **事件抽象成节拍**：CRM 取数、接口调用、文件生成、审批等待各算一个 beat，人靠密度判断健康度。
3. **等待期给反馈而不是给转圈**：长流程（RPA 跑批、报表刷新）中间插实时节拍，比最后弹一个"完成"体验高一个量级。

不建议直接引入这个工具本身（见下）。

## 8. 可做的小实验

- **实验 A（推荐，成本低）**：不装 CrommaCode。用现有 Hermes 一次真实会话日志做最小"节拍看板"——每类工具一条泳道，调用即打点，超过 N 秒无事件变红。验证问题只有一个：业务方/组长看不看、看不看得懂。
- **实验 B（只在隔离环境）**：若确要试 CrommaCode，必须在**非生产测试机**、独立 CLI 配置目录下进行，事前对 `~/.claude` 等配置做快照，登录一律不联，Bees 不配公司 key。
- **实验 C（纯文档）**：把 Bees 的 brief/worktree/merge 流程写成我们 harness 的一页 SOP，先手工跑一次双 agent 并行改造，看冲突率。

## 9. 风险和边界

1. **闭源 + 自动更新敞口。** 仓库明确"holds no source code"，源码在私有 monorepo，LICENSE 为 All rights reserved；App 每 15 分钟读一次公开 manifest。虽有 minisign 签名与 macOS 公证，本质仍是"第三方服务器随时可换掉你机器上的二进制"，不符合企业软件治理。**结论：不进公司环境，不装生产机。**
2. **会自动改你的 CLI 配置。** 首次识别到 CLI 就在该 CLI 的**用户设置**里写 hook（保留备份）；Codex 还需手动 `/hooks` 信任。这与 CC Switch 管理的 `~/.claude/settings.json` 存在交叠，装之前必须先看清它写了什么。
3. **数据面。** 会话正文、路径、代码、trace、token 台账全在本地 `~/.cromma-bees`；但可选登录后每小时上传 token 合计，另有每日一次匿名 ping（版本/系统/屏幕/语言/国别）。Bees 需要在 App 内另配 provider key，等于多一处 key 存放点。
4. **品控与持续性。** 单人/小团队 0.x 产品，7 天 87 star，版本一日数跳；不要把它当基础设施。
5. **版权/商标。** LICENSE 禁止用 CrommaCode 名称/标识暗示背书，截图可引用需署名。

## 10. 当前结论

**做法值得学，工具不进公司。**

它的真正贡献不是"游戏化很酷"，而是把两件事做对了：把 agent 的一次工具调用抽象成一个可数的 beat，把等待这件事做成可消费的仪表界面。这两条可以直接用于我们自己的 agent 看板和长流程反馈设计；而它本身是闭源、自动更新、会改本地 CLI 配置的 0.x 桌面软件，7 天龄、样本极小，只适合在隔离机器上个人试玩。

Bees 的"主 agent 写 brief + worktree 隔离 + 主 agent 合并验证"是目前最值得照抄的一段工程实践，与我们"贵模型动脑、便宜模型动手"的判断一致。
