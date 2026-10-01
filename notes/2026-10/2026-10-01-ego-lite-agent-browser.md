---
title: "ego (lite)：人和 Agent 共用的浏览器——代码驱动的浏览器自动化新物种"
date: 2026-10-01
discovery_source:
  type: 视频截图
  title: "一个Skill让Agent自动操作浏览器，告别重复枯燥任务（技术爬爬虾，B站，33.3万播放，2026-07-21）"
  url: https://www.bilibili.com/video/BV1aDKb6pEnJ
  companion_opus: https://www.bilibili.com/opus/1225344918250061828
primary_object:
  type: open_source_project
  name: ego (lite) / citrolabs/ego-lite
  url: https://github.com/citrolabs/ego-lite
object_type: [open_source_project, trend_signal, commercial_product]
source_type: [GitHub, 官网, B站]
business_tags: [市场, 销售, 运营, ITBP, 个人能力]
problem_tags: [流程提效, 获客, 用户洞察, 组织协同]
method_tags: [Agent, Vibe Coding, 自动化, Skill]
tool_tags: [ego-lite, ego-browser, Claude Code, Codex, Chromium, CDP]
value_stage: 可小实验
risk_tags: [数据安全, 权限, 国内可用性]
public_level: public
---

# ego (lite)：人和 Agent 共用的浏览器

## 1. 这是什么

Citro Labs 出品的 **Agent 原生浏览器**（定制 Chromium，免费、闭源浏览器本体），配套开源
连接层 `ego-browser`（MIT，TypeScript，GitHub 14.4k star / 251 commits）。

它不是 browser-use、Playwright 那种"外挂驱动一个空白浏览器"的自动化框架，也不是
ChatGPT Atlas / Perplexity Comet 那种"只能用内置 Agent"的 AI 浏览器，而是**同一个浏览器
进程里给每个 Agent 任务划出独立 Space（隔离工作区）**：人在前台用自己的标签页，
Claude Code / Codex / Cursor / Hermes 等任何会写代码的 Agent 在后台 Space 里并行干活，
并直接继承人在浏览器里的真实登录态。

## 2. 原始来源

- 发现入口：B站 UP主"技术爬爬虾"视频截图——视频本体为 BV1aDKb6pEnJ（2026-07-21
  发布，33.3万播放 / 2.8万赞 / 3.7万收藏 / 777 评论）。视频内投票"最近用哪个 Agent"：
  Codex、Claude Code、WorkBuddy、其他；视频顶部露出的 opus 链接是 UP主配套动态
  （放安装命令和官网地址用），[call] 是终端调用片段。收藏数高于点赞数，典型的
  实用工具教程"先马后看"数据特征。
- 仓库本体：https://github.com/citrolabs/ego-lite （MIT，14.4k star，2026-07 第三方
  文章记录时还只有 7900 star，两个月涨到 14.4k，热度上升中）
- 官网/文档：https://lite.ego.app 、https://lite.ego.app/document/
- 第三方实测：腾讯新闻《实测ego lite，给我Codex浏览器自动化加到2.5倍速了！》
  https://news.qq.com/run/a/20260730A09SPF00
- 同 UP主前作：BV1ooDyBmE6v《告别一切重复枯燥任务，CLI+Skill搭建浏览器AI自动化框架》
  （2026-04，playwright-cli 方案，同期播放量也是 33.3 万量级）。两期视频相隔三个月，
  正好展示该赛道从 CLI 方案进化到内核定制浏览器方案的路线。

## 3. 核心观点 / 核心能力

1. **代码驱动而非 CLI 驱动**：浏览器能力被包装成页面内 JavaScript 函数（snapshot /
   click / fill / wait / navigate），Agent 一段 JS 组合多步操作一次执行，省掉
   "调命令→看结果→再调命令"的往返。官方基准：复杂任务比 Vercel agent-browser 快
   2.5×（官网首页口径已更新到 3.45×），工具调用次数和 token 大幅下降。
2. **Space 并行工作区**：同一浏览器进程内的隔离 BrowserContext。可同时开 10 个 Space
   给 10 条线索做 enrichment、再开 5 个抓 5 个竞品站，互不抢占标签页和鼠标。
3. **一键继承 Chrome 全部数据**：Cookie、登录会话、扩展、书签、密码、标签组；
   Agent 不再卡在验证码、2FA、SSO 跳转。
4. **内核级语义快照**：定制在 Chromium 引擎内而非 JS shim，官方宣称能穿透跨域 iframe、
   shadow DOM、Stripe/Salesforce/Intercom 等第三方 SDK 组件和 React portal——
   正是后台管理系统、第三方嵌入组件最容易翻车的地方。另有 Visual 模式（Figma/Google
   Docs 等 Canvas 页面用坐标点击）和 Direct CDP 模式（页面内任意 JS / 原始 CDP）。
5. **Site Skills（coming soon）**：把每个站点上成功过的操作沉淀成 manifest.json
   （域名规则+知识笔记+工具集），同类任务第二次直接复用，官方称快 5 倍。
   本质是"浏览器操作经验的站点级记忆"。

## 4. 我学到了什么

- 浏览器自动化的代际路线：**Puppeteer/Playwright（选择器，脆弱）→ MCP/CLI（语义化，
  token 贵、往返多）→ 代码驱动的内核定制浏览器（少往返、强快照、登录态共享）**。
  趋势与"抛弃 MCP 转 CLI、再从 CLI 转 in-page 代码"的社区演进完全一致。
- Agent 操作浏览器的大部分时间消耗在**试错**（找按钮、摸流程），所以"经验可复用"
  （Site Skills）比单次执行速度更有想象力——这和我们自己的 Skill/判例库思路同源。
- 关键产品判断：它把"Agent 需要一个有真实登录态、又不打扰人"的环境作为一等公民来设计，
  而不是让 Agent 去适配人的浏览器。

## 5. 它是否可信，哪些需要验证

- 已核实：仓库真实存在且活跃（14.4k star、251 commits、MIT，含 .claude/.codex 插件
  目录和 skills/ego-browser 本体）；官网与第三方实测口径一致。
- 团队背景：Citro Labs 宣称成员有近 500 个补丁合入 Chromium（CSS shape-outside 的
  path()/shape() 等，随 Chrome 149 发布）——仅见于第三方文章转述，未逐一核对
  Chromium commit 记录，可信度中等偏高。
- 待验证：2.5×/3.45× 提速基准为官方自测，未见第三方独立复测；深层 iframe 快照能力
  需在真实复杂后台上实测；Windows/Linux 尚在 roadmap（目前仅 macOS，本机恰好满足）。

## 6. 对个人能力有什么价值

- 可直接接入现有 Codex / Claude Code 工作流：`/ego-browser` + 自然语言即可，零配置，
  对写"驾驶舱/harness bridge"的人是现成的浏览器执行层。
- 官方明确把 **Hermes Agent** 列为可连接 Agent 之一，理论上可作为本组浏览器任务
  （网页抓取、竞品巡检、社媒数据）的低 token 通道。

## 7. 对企业 AI 落地有什么价值

- **销售/市场**：多 Space 并行做线索 enrichment、竞品站点抓取、社媒（X/LinkedIn/
  Reddit）互动与数据拉取——即"需要登录但公开 API 不给"的那一类任务。
- **运营/ITBP**：穿透 iframe/第三方组件的快照对后台管理系统、嵌入式 SaaS 页面的
  自动化操作价值直接；本质仍是 UI 自动化补丁，长期仍按"有无正经接口"评估退场。
- 复用各网页端 Agent 的 credit（常不计入 coding plan 额度）：让 Agent 登录 ChatGPT
  网页版跑联网检索，有直接省钱效果。

## 8. 可做的小实验

1. 下载 dmg 安装（Apple Silicon），授权迁移 Chrome 数据，确认 `ego-browser` skill
   自动写入 ~/.claude/skills。
2. 低风险任务实测：在独立 Space 让 Agent 抓一个需要登录的公开信息页面，对比
   Playwright MCP 的 token 消耗与耗时。
3. 复杂页专项：拿一个深 iframe / 第三方嵌入组件的真实后台，验证快照穿透说法。
4. （需晶晶点头才执行，当前仅记录）

## 9. 风险和边界

- **登录态继承=Agent 拿到你在浏览器里的全部身份**：必须对支付、发布、删除、转账类
  操作设暂停确认；Codex 侧需开 Full access 才能启动本机 app，权限面较大。
- 浏览器本体闭源，虽宣称零数据采集、数据只存本机，但无法从代码层完全验证；
  迁移 Chrome 资料时系统级密码提示是访问本机浏览器数据所需。
- 产品早期，API/行为可能变；社媒自动化仍受账号封禁风险约束（只读、不自动互动）。

## 10. 当前结论

浏览器自动化赛道半年内的第二次范式升级，本体真实、团队有 Chromium 内核能力、与我们
现有的 Codex/CC/Hermes 体系天然兼容，价值判断：**可小实验**。建议先跑一个只读、
低风险的登录态抓取任务做 token/耗时对比，再决定是否纳入本组工具链；不急着替换
现有 Playwright 路径。
