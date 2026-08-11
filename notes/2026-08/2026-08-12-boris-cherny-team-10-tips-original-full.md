---
title: Boris Cherny 原版全文——Claude Code 团队 10 技巧（2026-01-31 X 帖逐条原文）
date: 2026-08-12
discovery_source:
  type: 群聊线索
  title: 刘涛Todd 视频「Claude Code 10个高效使用技巧」（二手解读，要求追原版）
  url: ""
primary_object:
  type: methodology
  name: Boris Cherny X 帖「Claude Code 团队 10 技巧」
  url: https://x.com/bcherny/status/2017742741636321619
object_type: [methodology]
source_type: [群聊线索]
business_tags: [ITBP, 个人能力]
problem_tags: [流程提效, 知识沉淀, 组织协同]
method_tags: [Agent, Prompt, 自动化, 知识库]
tool_tags: [Claude Code, Git Worktree, MCP, Skill]
value_stage: 已沉淀
risk_tags: [国内可用性, 成本]
public_level: public
---

# Boris Cherny 原版全文——Claude Code 团队 10 技巧（2026-01-31 X 帖逐条原文）

## 1. 这是什么

Boris Cherny（Claude Code 创建者）2026-01-31 在 X 发布的团队 10 技巧原帖逐条完整内容存档。群内视频是该帖的二手解读，本笔记为权威原文版本（中英对照要点），并注明与二手解读的差异。

## 2. 原始来源

- 原帖：https://x.com/bcherny/status/2017742741636321619 （9.2M 浏览）
- 各条推文链接见下方逐条标注
- 完整版交叉核对源：https://howborisusesclaudecode.com （聚合站，标注每条原帖链接）
- 注意：X 页面长推文在网页端有截断显示，本笔记按聚合站 + X 抓取交叉补齐

## 3. 核心观点 / 原版逐条全文

开场白（Boris）："I'm Boris and I created Claude Code. I wanted to quickly share a few tips for using Claude Code, sourced directly from the Claude Code team. The way the team uses Claude is different than how I use it. Remember: there is no one right way to use Claude Code -- everyones' setup is different. You should experiment to see what works for you!"

### 1. Do more in parallel（并行处理更多任务）
> Spin up 3–5 git worktrees at once, each running its own Claude session in parallel. It's the single biggest productivity unlock, and the top tip from the team. Personally, I use multiple git checkouts, but most of the Claude Code team prefers worktrees -- it's the reason @amorriscode built native support for them into the Claude Desktop app! Some people also name their worktrees and set up shell aliases (za, zb, zc) so they can hop between them in one keystroke. Others have a dedicated "analysis" worktree that's only for reading logs and running BigQuery.
- 原帖：https://x.com/bcherny/status/2017742743125299476

### 2. Start every complex task in plan mode（复杂任务从 Plan Mode 开始）
> Pour your energy into the plan so Claude can 1-shot the implementation. One person has one Claude write the plan, then they spin up a second Claude to review it as a staff engineer. Another says the moment something goes sideways, they switch back to plan mode and re-plan. Don't keep pushing. They also explicitly tell Claude to enter plan mode for verification steps, not just for the build.
- 原帖：https://x.com/bcherny/status/2017742745365057733

### 3. Invest in your CLAUDE.md（用心经营 CLAUDE.md）
> After every correction, end with: "Update your CLAUDE.md so you don't make that mistake again." Claude is eerily good at writing rules for itself. Ruthlessly edit your CLAUDE.md over time. Keep iterating until Claude's mistake rate measurably drops. One engineer tells Claude to maintain a notes directory for every task/project, updated after every PR. They then point CLAUDE.md at it.
- 原帖：https://x.com/bcherny/status/2017742747067945390

### 4. Create your own skills and commit them to git（自建 Skill 提交 Git 复用）
> Tips from the team: If you do something more than once a day, turn it into a skill or command. Build a /techdebt slash command and run it at the end of every session to find and kill duplicated code. Set up a slash command that syncs 7 days of Slack, GDrive, Asana, and GitHub into one context dump. Build analytics-engineer-style agents that write dbt models, review code, and test changes in dev.
- 原帖：https://x.com/bcherny/status/2017742748984742078

### 5. Claude fixes most bugs by itself（Claude 自己修大部分 Bug）
> Enable the Slack MCP, then paste a Slack bug thread into Claude and just say "fix." Zero context switching required. Or, just say "Go fix the failing CI tests." Don't micromanage how. Point Claude at docker logs to troubleshoot distributed systems — it's surprisingly capable at this.
- 原帖：https://x.com/bcherny/status/2017742750473720121

### 6. Level up your prompting（提升提问技巧）
> a. Challenge Claude. Say "Grill me on these changes and don't make a PR until I pass your test." Make Claude be your reviewer. Or, say "Prove to me this works" and have Claude diff behavior between main and your feature branch.
> b. After a mediocre fix, say: "Knowing everything you know now, scrap this and implement the elegant solution."
> c. Write detailed specs and reduce ambiguity before handing work off. The more specific you are, the better the output.
> Don't accept the first solution. Push Claude to do better — it usually can.
- 原帖：https://x.com/bcherny/status/2017742752566632544

### 7. Terminal & Environment Setup（终端与环境配置）
> The team loves Ghostty! Multiple people like its synchronized rendering, 24-bit color, and proper unicode support. For easier Claude-juggling, use /statusline to customize your status bar to always show context usage and current git branch. Many of us also color-code and name our terminal tabs, sometimes using tmux — one tab per task/worktree. Use voice dictation. You speak 3x faster than you type, and your prompts get way more detailed as a result. (Hit fn x2 on macOS)
- 原帖：https://x.com/bcherny/status/2017742753971769626

### 8. Use subagents（使用子代理）
> a. Append "use subagents" to any request where you want Claude to throw more compute at the problem.
> b. Offload individual tasks to subagents to keep your main agent's context window clean and focused.
> c. Route permission requests to Opus 4.5 via a hook — let it scan for attacks and auto-approve the safe ones.
- 原帖：https://x.com/bcherny/status/2017742755737555434

### 9. Use Claude for data & analytics（用 Claude 做数据分析）
> Ask Claude Code to use the "bq" CLI to pull and analyze metrics on the fly. We have a BigQuery skill checked into the codebase, and everyone on the team uses it for analytics queries directly in Claude Code. Personally, I haven't written a line of SQL in 6+ months. This works for any database that has a CLI, MCP, or API.
- 原帖：https://x.com/bcherny/status/2017742757666902374

### 10. Learning with Claude（用 Claude 学习）
> a. Enable the "Explanatory" or "Learning" output style in /config to have Claude explain the why behind its changes.
> b. Have Claude generate a visual HTML presentation explaining unfamiliar code. It makes surprisingly good slides!
> c. Ask Claude to draw ASCII diagrams of new protocols and codebases to help you understand them.
> d. Build a spaced-repetition learning skill: you explain your understanding, Claude asks follow-ups to fill gaps, stores the result.
- 原帖：https://x.com/bcherny/status/2017742759218794768

结尾（Boris）："Hope these tips are helpful! What do you want to hear about next?"

## 4. 我学到了什么

- 原版 10 条与视频解读的对应关系：视频把第 1/2/3/4 步的界面设置类命令（/allowed-tools、/terminal-setup、/theme、/install-github-app）当成了重点，但原版只有第 7 条涉及终端配置，且 Boris 提到的是 /statusline、Ghostty、语音输入，没有 /theme /install-github-app 这些具体命令——那些是视频作者自己补充的
- 视频第 6 步「explore→plan→confirm→code→commit」三种工作流出处为 GitHub Copilot CLI 官方文档，非本原帖内容（跨工具混编）
- 原版最强调的三件事：并行（第 1 条是"single biggest productivity unlock"）、Plan Mode（复杂任务必须）、CLAUDE.md 复利
- 原版没有提国内封号问题，那是视频作者的现实观察，属外部风险信息

## 5. 它是否可信，哪些需要验证

- 可信：X 原帖直接抓取 + 聚合站逐条标注原帖链接交叉验证
- 第 1 条 X 页面显示被截断（-- 结尾），聚合站补全为完整句，内容与上下文一致
- 待验证：/terminal-setup、/theme、/install-github-app 等命令存在于 Claude Code 官方命令文档（已查证：/allowed-tools 是 /permissions 的别名，/theme、/terminal-setup、/install-github-app 均真实存在），但不在本原帖内

## 6. 对个人能力有什么价值

- 拿到权威原文后，可据此纠正二手视频带来的认知偏差（工作流出处、命令归属）
- 三条可立即实践：Plan Mode 先规划、纠正后让 Agent 更新规则、高频操作封装 Skill

## 7. 对企业 AI 落地有什么价值

- 团队级 Agent 治理的参考基线：共享 CLAUDE.md、共享 Skill、共享权限配置（settings.json）、Agent 审 Agent 的质量门禁
- 数据分析 Agent 化降低业务取数门槛（任何有 CLI/MCP/API 的数据库都适用）

## 8. 可做的小实验

- 对照原版第 2 条：选一个复杂任务先出计划，另开一个 Agent 审计划，再实现，记录返工率
- 对照原版第 3 条：建立「纠正后更新规则文件」习惯，2 周观察同类错误是否下降
- 对照原版第 6 条：用 "Grill me" 模式让 Agent 反向考自己，再提交

## 9. 风险和边界

- 国内可用性：官方账号区域限制（视频作者观察，非原帖内容），团队推广前需确认合规路径
- 成本：并行多会话 + 大模型高 effort token 消耗快
- 二手内容混编风险：以本存档（原帖原文）为引用基准

## 10. 当前结论

已拿到原版全文并完整存档。原版与视频解读存在两处关键差异：① 视频把设置类命令当重点，原版只有第 7 条涉及终端且命令不同；② 视频第 6 步工作流出自 GitHub Copilot 文档，非原帖。后续引用以本存档为准。
