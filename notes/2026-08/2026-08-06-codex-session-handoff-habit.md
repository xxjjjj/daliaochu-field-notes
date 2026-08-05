---
title: Codex 长会话交接习惯：HANDOFF.md 模式
date: 2026-08-06
discovery_source:
  type: 小红书帖子
  title: Codex 有个很值得养成的收尾习惯：关掉长会话前，先让它写一份交接文档
  url: 截图线索（原始链接未直接获取，已追溯至 GitHub 配套 Skill）
primary_object:
  type: methodology
  name: AI 会话交接（Session Handoff）模式
  url: https://github.com/Liu-Bot24/codex-session-handoff-skill
object_type: [methodology]
source_type: [小红书, GitHub]
business_tags: [ITBP, 个人能力]
problem_tags: [知识沉淀, 流程提效]
method_tags: [Agent, Prompt]
tool_tags: [Codex, Claude Code, Hermes]
value_stage: 学习理解
risk_tags: [成本]
public_level: public
---

# Codex 长会话交接习惯：HANDOFF.md 模式

## 1. 这是什么

一条小红书帖子（以及相关的 GitHub Skill 项目）提出的 AI 代理工作流模式：**在关闭一个长会话之前，让 AI 代理生成一份结构化交接文档（HANDOFF.md），供新会话继续接手。**

核心做法是直接输入一段提示词，让 Codex / Claude Code 等写一份包含以下内容的交接文档：
- 当前任务目标
- 已完成的工作
- 当前卡点在哪儿
- 下一步计划
- 踩过的坑（绝对不要重蹈覆辙）

新会话的第一条消息就是让 AI 读 HANDOFF.md。

## 2. 原始来源

- 发现入口：小红书截图（许晶晶群发到打捞处）
- 资料本体：https://github.com/Liu-Bot24/codex-session-handoff-skill（Codex 专用 Skill，27 stars，MIT License）
- 相关链接：
  - https://x.com/Meta360DAO/status/2076174971202896331（X/Twitter，提及"Claude Code 有个很值得养成的收尾习惯"）
  - https://github.com/cholf5/random/issues/36（GitHub Issue "继任者 | Successor"）
  - https://www.jdhodges.com/blog/ai-session-handoffs-keep-context-across-conversations（2026 年博客：跨会话保持上下文）
  - https://www.reddit.com/r/ClaudeAI/comments/1tjzqrx/（Reddit ClaudeAI 讨论交接模式）
  - 掘金文章：https://juejin.cn/post/7633658714650116137（"别再靠长对话硬扛了"系列）

## 3. 核心观点

1. **不要把项目背景只留在聊天记录里**，而是把真正重要的信息沉淀成结构化文档。
2. 每次会话结束前做阶段总结，让 AI 帮你整理成交接文档。
3. 新会话第一条消息就读交接文档，实现"无痛转场"。
4. 配套工具：GitHub 上有 `codex-session-handoff-skill`（27 stars），自动创建交接快照到 `~/.handoffs/`。
5. 更通用的做法：`AGENTS.md` / `CLAUDE.md` 存持久化指令，`HANDOFF.md` 存当前会话状态，两者分工。

## 4. 我学到了什么

**其实 Hermes 已经原生支持 session_search，但缺少主动交接习惯。**

Hermes 有 session_search（FTS5 全文检索历史会话）和 `@session:` 链接，本质上是一个更好的"事后追溯"方案。但这个模式给的是"事前交接"——主动在会话结束时生成可读的交接文档，让新会话不用翻历史，直接读摘要就能上手。

两者不是替代关系，而是互补：
- session_search：事后找"我记得讨论过什么"
- HANDOFF.md：事前留"接下来要做什么"

## 5. 它是否可信，哪些需要验证

- 此模式在 Claude Code 和 Codex 社区中已广泛讨论和实践，不是单一来源的情况。
- GitHub 项目 `codex-session-handoff-skill` 有 27 stars，MIT License，维护状态一般（8 commits），但核心思路简单可靠，不需要依赖这个 Skill 也能手工执行。
- 交接文档的"踩坑记录"部分价值最高但最难写准确——AI 可能不知道哪些坑对"人类接手"最关键。
- 需验证：交接文档本身占用 token，新会话先读文档可能消耗约 3-5K tokens，但对比完整上下文续接的成本，仍然划算。

## 6. 对个人能力有什么价值

**直接可用。** 我们每天用 Hermes 处理大量长会话（排查、代码、研究），尤其在打捞处和 CRM 诊断场景。这个模式可以立刻用起来：

- 打捞处研究一个主题后，让小马生成总结 → 下次再聊同主题时直接读总结
- CRM 排查做到一半需要中断 → 生成交接文档，下次恢复更快
- 编写 Skill 或自动化脚本的长会话 → 生成 HANDOFF.md 便于后续迭代

## 7. 对企业 AI 落地有什么价值

- **团队协作**：当多个成员用 AI 工具处理同一项目时，交接文档是统一的"项目状态书"，降低沟通成本
- **知识沉淀**：交接文档经过结构化整理后，可以直接沉淀为知识库条目或 Skill，从"一次性交接"变为"可复用资产"
- **审计与追溯**：交接文档记录了关键决策路径和踩坑记录，比纯聊天记录更有审计价值

## 8. 可做的小实验

**对小马（Hermes）的适配实验：**

可以在 Hermes 中实现一个类似的「会话交接」Skill——当用户说"会话结束，生成交接文档"时，小马自动：
1. 调用 session_search 回顾当前会话的关键决策
2. 生成 HANDOFF.md 包含：目标、已完成、卡点、下一步、踩坑记录
3. 保存到约定的目录（如 `~/.hermes-v019/handoffs/`）
4. 返回一段简短提示词，供新会话使用

或者更简单的做法：直接写一个 Prompt 模板，每次结束会话前让小马执行。

## 9. 风险和边界

- 交接文档只记录**当时的状态**，项目文件变更后文档可能过时——需要约定"先读文档，再看实际状态"的习惯
- 明文密钥/凭证不应写入交接文档（GitHub Skill 已考虑这一点，默认不写明文）
- 交接文档本身也是 token 成本——如果每次结束都生成但从不使用，就是浪费
- 适用于"有明确目标和阶段性成果"的长会话，不适合零散问答

## 10. 当前结论

**推荐采纳。** 这个模式思考成本低、执行成本低、收益明显。建议先在小马的长会话中手工试用这个模式，验证效果后再考虑是否沉淀为 Hermes Skill。

下一步：起草一个简短的 Hermes 版交接 Prompt 模板，下次打捞处长会话结束后试用。