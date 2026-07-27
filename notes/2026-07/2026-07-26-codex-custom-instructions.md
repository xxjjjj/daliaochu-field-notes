---
title: Codex 自定义指令：让 Agent 越用越顺手的项目上下文层
date: 2026-07-26
discovery_source:
  type: 群聊线索
  title: 深度使用 Codex 半年后的自定义指令经验
  url: 
primary_object:
  type: 方法论 / 工具配置
  name: Codex AGENTS.md 自定义指令与项目规则
  url: https://learn.chatgpt.com/docs/agent-configuration/agents-md
object_type: [methodology]
source_type: [群聊线索, 官网]
business_tags: [ITBP, 个人能力, 管理]
problem_tags: [流程提效, 知识沉淀, 组织协同]
method_tags: [Agent, Prompt, Vibe Coding, 知识库]
tool_tags: [Codex, AGENTS.md]
value_stage: 可小实验
risk_tags: [数据安全, 幻觉, 权限, 成本]
public_level: sanitized
---

# Codex 自定义指令：让 Agent 越用越顺手的项目上下文层

## 1. 这是什么

这条资料是一个 Codex 深度使用经验：不要只把 Codex 当一次性代码问答工具，而是通过自定义指令和项目说明文件，把“我是谁、这个项目怎么做、踩过哪些坑、完成标准是什么”固化下来，让 Agent 每次开工前都能读到稳定上下文。

群聊内容本身没有附配置文件原文，所以这里只沉淀方法判断，不搬运所谓“评论区配置”。

## 2. 原始来源

- 发现入口：打捞处群聊中的经验帖文字。
- 可验证资料本体：OpenAI / ChatGPT Learn 的 Codex `AGENTS.md` 文档。
- 相关链接：https://learn.chatgpt.com/docs/agent-configuration/agents-md

官方文档确认：Codex 会在启动时读取 `AGENTS.md` 指令链。默认全局目录是 `~/.codex`；项目内从 Git 根目录一路读到当前目录；同层级优先级为 `AGENTS.override.md`、`AGENTS.md`，也可通过配置加入 fallback 文件名。项目指令默认合并上限为 32 KiB。

## 3. 核心观点 / 核心能力

1. Agent 使用效果不只取决于模型，还取决于“开工前上下文”。
2. 自定义指令要解决具体问题：身份与工作类型、沟通风格、项目背景、执行流程、代码标准、工具偏好、验证标准。
3. 好的指令不是越长越好，而是短、稳定、可执行、可验证。
4. 对复杂任务，先计划后动手；对编码任务，实际运行验证是完成标准的一部分。
5. 项目级文件比单次 prompt 更重要，因为它能减少重复交代、降低新会话失忆成本。

## 4. 我学到了什么

这条资料的价值不在“某一份神奇 prompt”，而在一套 Agent 管理思路：

- 把 Agent 当新同事管理：先讲角色、边界、质量标准，再分配任务。
- 把踩坑经验外化：不要靠人记忆反复提醒，而要进入项目说明、记忆文件或 playbook。
- 把“少废话、先行动、必须验证”写成明确规则，减少 Agent 输出表演。
- 把上下文分层：全局偏好放全局指令；项目背景放项目指令；临时覆盖用 override；真正的长期方法沉淀成 playbook/skill。

## 5. 它是否可信，哪些需要验证

可信部分：官方文档支持 `AGENTS.md` 作为 Codex 项目指导文件，并说明了读取顺序、全局/项目分层、fallback 文件名、验证方式和排查方式。

需要验证的部分：

- 群聊提到的 `agent.md`、`memory.md` 文件名不是官方默认发现名；如果要让 Codex 自动读取，需要确认当前 Codex 版本是否配置了 `project_doc_fallback_filenames`，否则应使用 `AGENTS.md` 或显式让 Agent 读取。
- “复杂任务先出方案，确认后再动手”适合高风险改动，但对小修小补可能拖慢效率，需要按任务风险分级。
- 自动维护记忆文件容易把错误经验、临时状态、敏感信息写进去，需要治理规则。

## 6. 对个人能力有什么价值

对个人使用 AI 编程工具，最直接的提升是减少重复沟通和返工。可形成一份 200 行以内的个人 Codex 基础指令，覆盖：

- 我是谁、常做什么类型任务；
- 输出风格：直接、少废话、不谄媚；
- 工作流：复杂任务先方案，小任务直接做；
- 质量门槛：修改后必须运行最相关测试或给出无法运行原因；
- 代码原则：优先删减、复用、最小改动，不为炫技引入复杂抽象；
- 安全边界：不擅自提交、推送、删除数据、改生产配置。

## 7. 对企业 AI 落地有什么价值

对 ITBP 和企业 AI 落地，这其实是“AI 同事的 SOP 化”：

- 可以把不同系统、不同业务域的需求分析规范写成项目级 Agent 指令。
- CRM、飞书自动化、数据脚本、RPA 等项目都可以有自己的 `AGENTS.md`，明确字段口径、测试方式、权限边界、交付标准。
- 团队新人和 Agent 都读同一套项目规则，能降低交接成本。
- 适合和知识库、实施 playbook、故障复盘结合，但要避免把客户数据和内部截图原文塞进公开仓库。

## 8. 可做的小实验

建议做一个低风险实验：

1. 选一个非生产脚本仓库或个人工具仓库。
2. 新建 `AGENTS.md`，只写 6 个部分：项目背景、常用命令、代码风格、测试验证、禁止事项、完成标准。
3. 用同一个小需求分别在“无 AGENTS.md”和“有 AGENTS.md”环境下让 Codex 执行。
4. 对比返工次数、测试执行情况、输出废话比例、是否误改无关文件。
5. 如果有效，再整理为团队模板。

## 9. 风险和边界

- 指令过长会浪费上下文，且容易让关键规则被稀释。
- 指令写成空泛价值观没有用，必须是可执行规则和命令。
- 记忆文件如果无人维护，会积累过期结论，让 Agent 越用越笨。
- 不要在指令文件中放 token、账号、客户数据、内部截图、未脱敏流程。
- 对生产、权限、上线、外部承诺类动作，Agent 不能替人拍板。

## 10. 当前结论

这条资料值得沉淀为“Agent 项目指令治理”方法。可公开复用的部分是：Codex 的 `AGENTS.md` 分层机制、精简指令写法、复杂任务先计划、实际运行验证、记忆治理边界。群聊原文和任何评论区私有配置未核验，不应原样公开。

下一步可以做一份内部模板：`AGENTS.md` 最小可用版 + `memory.md` 治理规则 + 验证 checklist，用于个人脚本仓库或 CRM AI 化实验仓库。需要注意：若希望 Codex 自动读取非 `AGENTS.md` 文件名，必须先配置并验证 fallback 文件名。

## 11. GitHub 热门自定义指令线索

基于 GitHub 搜索、`gh repo view` 和官方/社区资料，当前可优先参考这些仓库和材料：

| 项目 | 热度与状态 | 适合借鉴什么 | 注意点 |
| --- | --- | --- | --- |
| `github/awesome-copilot` | 约 37k stars、4.6k forks，GitHub 官方组织下社区集合，仍活跃 | 大量 `.instructions.md`、`.agent.md`、skills、plugins，可按语言/角色/场景复用；适合找 Python、代码评审、安全、DevOps、Azure/AWS 等细分模板 | 偏 GitHub Copilot 生态，迁移到 Codex 时要改文件名、frontmatter 和触发机制 |
| `agentsmd/agents.md` | 约 23k stars、1.7k forks，AGENTS.md 格式说明站点/仓库，MIT | 最小 AGENTS.md 结构：开发环境提示、测试命令、PR 规则；适合作为 Codex 项目级说明的基础模板 | 它是格式与示例，不是完整个人工作流 |
| `botingw/rulebook-ai` | 约 600 stars、90 forks，仍活跃 | 把规则、上下文、工具打包成跨 Cursor/Copilot/Claude/Codex/Gemini 的“AI 环境”；适合研究多工具统一规则和 memory bank | 是工具型方案，不只是拿一份 md；引入前要评估复杂度 |
| `Anbeeld/AGENTS.md` | 约 140 stars，专门的全局 AGENTS.md/CLAUDE.md 指令 | evidence-first、并行化、验证、少废话、禁止伪造结果、最小变更；很适合作为个人全局指令参考 | 规则较强，需要按自己的工具能力裁剪，避免过度约束 |
| `zeke/agents.md` | 约 150 stars，个人匿名化全局指令 | 直接沟通、不编造、必须验证、最小改动、PR 写法、GitHub/GitLab 工作流；非常贴近日常工程协作 | 带有作者个人技术栈偏好，如 Cloudflare/Hono，不能照搬全部 |
| `addyosmani/agent-engineer` 的 `15-agents-md` | 约 400+ stars 的 Agent 工程课程资料 | 把 AGENTS.md 当“AI 新员工入职包”：commands、testing、structure、style、git、boundaries 六块；适合教学和模板化 | 课程材料，需结合真实项目命令落地 |
| `aws-samples/sample-codex-agent-team` | 约 30 stars，AWS Samples 示例 | Codex 多角色团队、spec-driven workflow、review gate、权限边界；适合研究复杂 Agent 团队配置 | 样例较重，不适合直接用于普通个人仓库 |
| `pamelafox/awesome-copilot-instructions` | 约 130 stars，但仓库已声明 deprecated | 早期 Copilot custom instructions 收集，可看 Python/安全/文件名等规则写法 | 已迁移/推荐看 `github/awesome-copilot`，不建议作为主来源 |

GitHub 官方博客《How to write a great agents.md: Lessons from over 2,500 repositories》给出的共性结论也值得采用：成功的指令通常具备 6 个部分：命令、测试、项目结构、代码风格、Git 工作流、边界；并且更偏“具体角色 + 可执行命令 + 明确禁区 + 好输出示例”，而不是泛泛的“你是 helpful assistant”。

当前筛选结论：

1. **最值得先看**：`github/awesome-copilot`、`agentsmd/agents.md`、`Anbeeld/AGENTS.md`、`zeke/agents.md`。
2. **最适合做个人 Codex 全局指令底稿**：`Anbeeld/AGENTS.md` + `zeke/agents.md`，再删掉不适用技术栈。
3. **最适合做项目级模板**：`agentsmd/agents.md` 的最小结构 + GitHub 博客总结的六块结构。
4. **最适合做团队/多工具规则治理**：`github/awesome-copilot` + `rulebook-ai`。
5. **不要盲目照搬**：Copilot 的 `.instructions.md` / `.agent.md` 与 Codex 的 `AGENTS.md` 读取机制不同；个人偏好、生产权限、技术栈规则要本地化。
