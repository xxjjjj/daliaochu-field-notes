---
title: "Agency Agents：16.7 万星的岗位级 AI Agent persona 开源库（抖音视频核验）"
date: 2026-11-11
discovery_source:
  type: 短视频截图
  title: "抖音账号「AI小蔡狗」：16.7万颗星标AI智能体库，200多个岗位Agent"
  url: "无原始链接，群内截图（img_867a59e188d8.jpg）"
primary_object:
  type: open_source_project
  name: "msitarzewski/agency-agents"
  url: "https://github.com/msitarzewski/agency-agents"
object_type: [open_source_project, trend_signal]
source_type: [GitHub, 短视频]
business_tags: [ITBP, 个人能力, 管理]
problem_tags: [流程提效, 知识沉淀, 组织协同]
method_tags: [Agent, Prompt, Vibe Coding]
tool_tags: [Claude Code, Cursor, Codex, Gemini CLI, OpenClaw, Tauri]
value_stage: 可小实验
risk_tags: [幻觉, 版权]
public_level: public
---

# Agency Agents：岗位级 AI Agent persona 开源库核验

## 1. 这是什么

一个按"公司部门 + 岗位"组织的 AI Agent persona（角色提示词）合集：每个 agent 是一个带
frontmatter 的 Markdown 文件（name / description / color / emoji / vibe + 身份、任务流、
关键规则、成功指标），可一键安装到 Claude Code、Cursor、Codex、Gemini CLI、OpenCode、
Qwen、Osaurus、Copilot、Antigravity、Aider、Windsurf、Kimi Code、**OpenClaw** 等
coding agent 工具。配套 Tauri 桌面 app（agencyagents.app，支持 brew 安装）做浏览和部署。

**本质判断：它不是可执行的 Agent 系统，而是结构化提示词/角色库 + 多工具格式转换器
（scripts/convert.sh）。** 能力上限仍取决于底座模型，价值在"岗位知识的 prompt 工程沉淀
与分发格式"。

## 2. 原始来源

- 发现入口：抖音「AI小蔡狗」短视频截图（营销号二创，赛博风包装，非项目官方内容）
- 资料本体：https://github.com/msitarzewski/agency-agents （MIT，2025-10-13 创建）
- 配套 app：https://github.com/msitarzewski/agency-agents-app （Tauri，946 星，v0.3.2 / 2026-10-03）
- 中文本地化 fork：https://github.com/jnMetaCode/agency-agents-zh （2.2 万星）
- 起源佐证：项目 README 与多个外部帖均称源自 Reddit 帖子；LinkedIn 帖记录上线初期
  "单日 1,349 星"的病毒增长；r/tauri、r/BestGitHubRepos 有作者本人发帖

## 3. 实测数据（2026-11-11 GitHub API + tarball 实测，非转述）

- 星标 **167,647**，fork 27,185，watchers 仅 1,181（watcher/star ≈ 0.7%，典型病毒爆款
  特征：围观者多、深度跟踪者少）
- 840 commits、21 分支、**0 tag**、237 open issues；contributors 列表 112 页（社区 PR 驱动）
- tarball 实测 330 个 .md 文件，22 个顶层目录；其中 19 个 division 含 persona 源文件，
  数量前三：engineering 66、specialized 59、marketing 37；integrations/ 19 个是脚本转换
  产物不是源文件。社区口径"约 280 个 agent"，视频所称"200 多个"在其制作时点成立
- 截图中的 PR #1177（ayobamiseun 的 Electrical Engineer agent）真实存在，
  2026-10-25 合并；截图里 6 个文件夹名、20 branches、0 tags、MIT 均与 API 一致
- 抽样 engineering/Software Architect：frontmatter + 身份/记忆/任务/关键规则结构，
  写法是成熟的 persona prompt，不是空模板

## 4. 视频截图本身的问题（媒介素养点）

视频核心指向的项目真实，但包装素材粗糙，截图内五个位置出现 **14.7万 / 0.0万 / --万 /
16.7万 / 一万** 五个互相打架的星标数，其中 `0.0万` 和 `--万` 是模板默认值/占位符未清理；
仿 GitHub 窗口地址栏只有 `github.com/agency-agents`，缺 owner 前缀（真实地址是
`msitarzewski/agency-agents`，OCR 还把作者名 msitarzewski 误识成 nstarzewski）。
结论：营销号拼接模板时数据没对齐，不能拿视频里的数字当事实，必须回 GitHub 核验。

## 5. 我学到了什么

1. **persona-as-markdown + 多工具转换**是一条被验证的轻量路线：一份角色源文件，
   convert.sh 转各 agent 工具的目录格式，避免为每个工具维护一套。
2. **按部门（division）组织岗位 agent** 的目录学：divisions.json 作为单一事实源，
   CI 校验目录与配置一致性（check-divisions.sh），新增部门必须先改配置再落文件——
   治理思路和我们用 Skill 管理流程资产同构。
3. **社区共创的增长机制**：Reddit 起源 → PR Welcome → 本地化 fork 矩阵（日/越/俄/印尼/
   阿拉伯/韩/中），主仓 112 页 contributors；但 0 tag、issue 积压 237 个，说明高速扩张下
   发布治理偏弱，质量参差是必然代价。
4. 星标数 ≠ 工程质量。一年 16.7 万星靠的是话题红利（"AI 员工"叙事）+ 社交传播，
   watcher 比例和 0 tag 才是更真实的成熟度信号。

## 6. 对个人能力有什么价值

- 直接可借鉴 persona 文件的写法骨架（vibe 一句话人格 + 身份记忆 + 任务清单 + 硬规则 +
  成功指标），对比我们自己的 Skill frontmatter 做结构化升级参考。
- 它支持 OpenClaw 是一个实测对照样本：可以看官方转换脚本如何把 persona 映射到
  OpenClaw 的 agent 格式，检验我们自建 Skill 体系与社区生态的兼容性。

## 7. 对企业 AI 落地有什么价值

- 与英科"数字员工建档"口径（头像、名称、直属上级、入职部门四要素）方向一致：
  这个仓库证明了"按岗位建 AI 角色"已是社区主流组织方式，且部门化目录可扩展到
  200+ 角色不混乱。
- 但企业内不能照搬公开 persona：内容是通用英文语境、社区 PR 质量无 SLA、无企业知识
  对接。正确用法是**借结构不借内容**——参考其 division 治理和 frontmatter 规范，
  角色知识仍以本租户业务事实填充。
- 多工具一份源的分发思路，适用于未来同一岗位角色要同时进 coding agent、飞书机器人、
  工作流 Agent 的场景，避免三套提示词各自漂移。

## 8. 可做的小实验

1. 装桌面 app（brew install --cask msitarzewski/agency-agents/agency-agents），
   观察它如何向 ~/.claude/agents 等目录部署 persona，与 Hermes Skill 目录做对照。
2. 抽 sales / marketing division 各 3 个 persona，与本组现有带教/营销类 Skill 做
   结构 diff，看 frontmatter 和规则颗粒度差异。
3. 读 scripts/convert.sh，评估其 OpenClaw target 输出格式能否为我所用。

## 9. 风险和边界

- 幻觉/质量：社区贡献的 persona 无事实校验，岗位流程描述可能过时或想当然，
  生产引用前必须逐条评审。
- 版权：MIT 允许复用和修改，但保留许可声明；本地化 fork 的协议合规需各自核验。
- 数据安全：persona 本身不含数据，但安装脚本会写本地 agent 工具目录，
  企业环境试用应在沙盒目录进行，不直接混入生产 Skill 库。
- 营销号信息污染：同类短视频可能继续用夸大数字带节奏，结论一律以 GitHub API 实测为准。

## 10. 当前结论

项目真实、活跃、体量属实（16.7 万星 API 实测），是 2025-2026 "岗位 Agent"叙事下
最成功的开源 persona 库；视频包装有数据硬伤但不影响本体价值。对我们的价值在
**目录治理结构、persona 写法和多工具分发机制**三个可迁移点，而非直接使用其角色内容。
定级"可小实验"，先做 app 部署观察和 convert.sh 研读，不进生产资产库。
