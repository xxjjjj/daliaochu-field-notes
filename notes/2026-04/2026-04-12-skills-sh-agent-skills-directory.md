---
title: skills.sh：跨 Agent 的 Skill 注册中心 / 包管理（Agent 时代的 "npm"）
date: 2026-04-12
discovery_source:
  type: 群聊讨论
  title: 通过 registry.skills.sh/index.json 抓取并分类 151 个 Skill
  url: https://skills.sh
primary_object:
  type: open_source_directory
  name: skills.sh - The Agent Skills Directory
  url: https://skills.sh
object_type: [open_source_directory, package_manager]
source_type: [官方网站, 群聊线索]
business_tags: [ITBP, 个人能力, 知识沉淀, 流程提效]
problem_tags: [Skill发现, 选型对比, 流程提效, 自动化]
method_tags: [Skill, Agent, 包管理, 自动发现, 分类索引]
tool_tags: [skills.sh, Claude Code, Codex, Cursor, Windsurf, MCP]
value_stage: 可小实验
risk_tags: [国内可用性, 安全审计, 质量参差, 维护风险]
public_level: public
---

# skills.sh：跨 Agent 的 Skill 注册中心 / 包管理（Agent 时代的 "npm"）

## 1. 这是什么

skills.sh 自称 "The Agent Skills Directory"，是一个**跨 Agent 的 Skill 注册中心 + 包管理工具**。它的定位类似于 npm / PyPI / VS Code Marketplace，但对象是 AI Agent 的 Skill（一个文件夹 + `SKILL.md`）。一条命令 `npx skills add <owner/repo>` 即可在多种 Agent 里安装。

**核心口号**：Skills are reusable capabilities for AI agents. Install them with a single command to enhance your agents with access to procedural knowledge.

## 2. 原始来源

- 官方主页：[skills.sh](https://skills.sh)
- 注册中心 API（2026-04 当时）：`https://registry.skills.sh/index.json`（目前 404，已切换到新的目录页）
- 早期第三方索引：[mcpmarket.com/tools/skills/skills-registry](https://mcpmarket.com/tools/skills/skills-registry)
- 类似生态：[openskills.cc](https://openskills.cc)（中文向，更聚焦 persona / colleague skill）、[github.com/majiayu000/claude-skill-registry](https://github.com/majiayu000/claude-skill-registry)（每日更新的 Claude Skill 注册）
- 发现入口：打捞处群聊 2026-04-12，user 用 `curl + python` 直接抓 `registry.skills.sh/index.json`，解析了 151 个可拉取的 skill

## 3. 核心观点 / 核心能力

1. **跨 Agent 安装**
   - `npx skills add <owner/repo>` 同时支持：
     - IDE：VS Code、Zed、Cursor、Windsurf、OpenCode、Kiro CLI
     - AI Coding：Claude Code、GitHub Copilot、Codex、Gemini、Cline、AMP、Antigravity、ClawdBot、Droid、Goose、Kilo、Trae、Nous Research、Roo
   - 一处发布，多 Agent 复用。

2. **技能分类清晰（leaderboard 式目录）**
   - Development & Architecture：code quality（`tdd` / `systematic-debugging` / `verification-before-completion`）、架构重构、frontend design。
   - Cloud & Infrastructure（Microsoft Azure 官方投放）：`azure-cost` / `azure-kubernetes` / `entra-agent-id` 等十几个。
   - Media：视频生成、图像生成（flux / gpt-image / nano-banana / wan）、音频 / 音乐。
   - Productivity & Documentation（Lark/Feishu）：`lark-approval` / `lark-event` / `lark-okr` 等官方投递的飞书集成 skill。

3. **支持人工评测 / 排行榜**
   - 网站提供 leaderboard，按下载 / 安装量排序，便于发现"被广泛验证"的 skill。

4. **以 SKILL.md 为契约**
   - 所有 skill 都遵循"一个文件夹 + SKILL.md"格式（YAML frontmatter + Markdown body），与 Anthropic / Claude 的 skill 规范对齐。

## 4. 我学到了什么

- **Skill 已经成为 Agent 时代的事实插件标准**：2026 年已经出现多个 Skill 注册中心（skills.sh、openskills.cc、majiayu000/claude-skill-registry）+ 数十种 Agent 兼容。这意味着写一个 Skill 比写一个独立插件更有复用价值。
- **官方厂商已经开始在 Skill 市场做投放**：Microsoft（Azure skill 一整组）、Anthropic（`frontend-design`）、Vercel（`vercel-react-best-practices`）、shadcn 等都在 skills.sh 上有官方 skill。Skill 市场已经不只是"民间玩具"，是厂商触达开发者的新渠道。
- **飞书生态在 Skill 时代有机会**：目前 skills.sh 上 Lark/Feishu 官方 skill 数量不多（lark-approval / lark-event / lark-okr 等），相对于 Slack / 微软 / Anthropic 投放量明显偏少；这是国内生态可以补位的机会。
- **抓 registry 当数据源很危险**：registry.skills.sh/index.json 已经 404，说明这类目录 API 不一定稳定；自建 skill 路由系统时建议直接拉 GitHub / 官方主页 HTML，不依赖单一索引 API。

## 5. 它是否可信，哪些需要验证

- **可信度**：skills.sh 已经有 leaderboard + 分类 + 跨 Agent 安装能力，是当前最完整的 Skill 目录之一；但 `registry.skills.sh/index.json` 已 404，说明其内部索引机制不稳定。
- **需要验证**：
  - Skill 的安全审计：是否对每个 skill 做恶意代码 / 注入扫描？用户安装时是否有风险提示？
  - License：每个 skill 自身的 license 是否一致？商业项目用前需要逐个核查。
  - 质量参差：leaderboard 排序是否能反映实际质量？还需要看 skill 维护频率、最近一次更新时间。
  - 国内可用性：`npx skills add` 走 npm 国际网络，国内可能受限；skills.sh 官网是否需要外网访问？

## 6. 对个人能力有什么价值

- **Skill 设计能力**：理解"一个 Skill = SKILL.md + 文件夹"的极简抽象后，可以快速把日常工作流沉淀为 skill（参考 cc-connect 笔记里的踩坑案例）。
- **跨 Agent 复用**：写一次 skill，能在 Claude Code / Codex / Cursor / Windsurf 等多个 Agent 里跑。
- **发现 / 选型能力**：skills.sh 的 leaderboard 是快速发现"被验证过的好 skill"的入口。

## 7. 对企业 AI 落地有什么价值

- **内部 Skill 投放**：如果有大量内部流程沉淀为 skill（CRM AI 化 / 飞书自动化 / 数据导出 / 报告生成等），可以在 skills.sh 上架，方便团队成员 `npx skills add <company>/skills` 一键安装。
- **统一 Skill 市场**：避免每个团队重复造 skill 轮子；公开的 skill 市场 + 公司内部私有 skill registry 双层结构是合理设计。
- **质量分层**：skills.sh leaderboard 给出了"被广泛验证"的参考；内部 skill 上架时也可以参考这种"按下载 / 评分排序"的机制。

## 8. 可做的小实验

- 在 Claude Code / Codex / Cursor 任一环境里 `npx skills add anthropics/skills`，安装 Anthropic 官方 skill 集合，对比安装前后的使用体验。
- 从 leaderboard 选 1-2 个高频 skill（如 `tdd` / `systematic-debugging` / `verification-before-completion`），在 Hermes 内部 skill 体系中对照看是否值得集成。
- 评估把公司内部已有的 1-2 个 skill（如人话翻译机 / 候选人评估）按 skills.sh 规范重新打包，看是否能在多 Agent 环境跑通。
- 写一个 `hermes-skill-search` 脚本，从 skills.sh / openskills.cc / GitHub 抓 skill，按业务标签过滤，给出"本周新增的相关 skill"摘要。

## 9. 风险和边界

- **国内可用性**：npx + skills.sh 都需要外网；如果团队纯国内，需要在 npm 镜像 + 私有 skill registry 之间做隔离。
- **安全审计**：从 skills.sh 安装 skill 等于从 GitHub 拉代码 + 自动运行；恶意 skill 可能窃取 API key / 注入代码。必须：
  - 安装前人工 review SKILL.md；
  - 用 sandbox 环境先跑；
  - 公司内部署时建议维护"白名单 registry"。
- **质量参差**：leaderboard 排序不等于"质量好"；很多新 skill 没经过实际业务验证。
- **License 不一致**：每个 skill 自带 license，混用前必须逐一核查，避免商业合规问题。
- **维护风险**：依赖外部 registry，单一来源故障（如 registry.skills.sh 已 404）会导致依赖其的脚本全部失效。
- **覆盖度偏差**：英文 skill 占绝对多数，中文 / 国内 IM 集成类 skill 偏少；如需国内场景，往往需要自己造。

## 10. 当前结论

skills.sh 是 2026 年 Skill 生态的事实基础设施之一，跨 Agent 安装 + leaderboard + 分类目录三个能力是核心价值。落地建议：

1. **个人 / 小团队**：用 `npx skills add` 试装 leaderboard 上几个高频 skill，体验 SKILL.md 抽象的优势。
2. **企业内部**：自建私有 skill registry（参考 skills.sh + openskills.cc 设计），把内部 skill 集中管理；同时把 1-2 个高质量 skill 上架到 skills.sh 提升影响力。
3. **持续监控**：每月扫一次 skills.sh / openskills.cc 的新增 skill，按业务标签过滤，避免错过好工具。

落地前在 sandbox 环境跑一次 `npx skills add <某个高频 skill>`，观察安装行为是否安全（是否会偷偷写 `~/.bashrc` / 拉敏感目录文件等）。
