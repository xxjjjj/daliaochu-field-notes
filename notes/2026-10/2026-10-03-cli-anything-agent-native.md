---
title: "CLI-Anything：把任意软件封装成 Agent 可调用的 CLI（港大 HKUDS）"
date: 2026-10-03
discovery_source:
  type: 抖音短视频
  title: AIGC成也《软件变AI调用工具》
  url: ""
primary_object:
  type: open_source_project
  name: HKUDS/CLI-Anything
  url: https://github.com/HKUDS/CLI-Anything
object_type: [open_source_project, methodology]
source_type: [GitHub, 论文, 短视频]
business_tags: [ITBP, 产品, 运营]
problem_tags: [流程提效, 组织协同]
method_tags: [Agent, 自动化, Vibe Coding, RPA]
tool_tags: [CLI-Anything, CLI-Hub, Claude Code, Cursor, Codex, MCP]
value_stage: 待验证
risk_tags: [成本, 幻觉]
public_level: public
---

# CLI-Anything：让所有软件"Agent 原生"

## 1. 这是什么

港大数据科学实验室（HKUDS，黄超团队）开源的项目：把任意**有源码的软件**自动分析、封装成一个命令行工具（CLI harness），让 AI Agent（Claude Code、Cursor、Codex、Pi、OpenClaw、nanobot 等）通过结构化命令直接驱动软件，而不是靠截图识别 + 模拟点击的 GUI 方式。

配套有 CLI-Hub（`pip install cli-anything-hub`，`cli-hub install ...`），类似"Agent 时代的软件包管理器"，可浏览、安装社区贡献的上百个 CLI harness。Apache 2.0 协议。

数据（2026-10-03 实查）：51.1k stars、4.7k forks、893 commits、115 contributors，最新 release v0.4.0（2026-06-25），最新提交 2026-09-22。论文 arXiv:2606.03854（2026-06-02）。

## 2. 原始来源

- 发现入口：抖音博主「AIGC成也」短视频《软件变AI调用工具》（截图入库，属二手转述）
- 资料本体：https://github.com/HKUDS/CLI-Anything
- 论文：https://arxiv.org/abs/2606.03854 《CLI-Anything: Towards Agent-Native Computer Use》
- Hub：https://hkuds.github.io/CLI-Anything/

## 3. 核心观点 / 核心能力

- **核心主张**：GUI Agent（截屏→找元素→点坐标）是在强迫 Agent 模仿人的感知方式，天然脆弱（像素级交互、时序依赖、界面一改就崩）。Agent 的强项是结构化数据和程序化控制，应该给它"agent-native"的接口：结构化命令 + 显式状态 + 确定性反馈。
- **做法**：7 阶段流水线，读目标软件源码 → 生成 Click（Python）CLI，输出 JSON + 人类可读双格式，同时生成 SKILL.md（供 Agent 渐进式加载的技能说明）和测试。
- 已覆盖 18+ 专业软件 demo：Blender、Audacity、ComfyUI、QGIS/ArcGIS、Godot、Unreal、Calibre、Zotero、Obsidian、LibreOffice、Inkscape、n8n、VideoCaptioner 等，官方称 2,461 个测试通过。
- 以插件形式接入 Claude Code（`/cli-anything`、`/refine` 命令）、Cursor（一等适配）、Codex 等。

## 4. 我学到了什么

- "别造更聪明的屏幕阅读器和点击模拟器，而要围绕 Agent 的强项重设计交互范式"——这和我们组 RPA 治理结论（RPA 是无接口时的脆弱补丁，优先推 API/EDI/中间表）是**同一个判断的更上游版本**：能拿到接口/源码就不要走 UI 自动化。
- CLI + SKILL.md + 测试一起产出，本质是"软件能力的 Agent 可读封装"，与 Hermes 自身的 skill/工具体系思路一致。
- CLI-Hub 把"发现→安装→更新"做成包管理器，是 agent-native 软件分发的一个具体形态，值得观察。

## 5. 它是否可信，哪些需要验证

可信但需打折：

- 一手出处扎实（GitHub 仓库 + arXiv 论文 + 港大团队，非仅短视频说法），社区热度真实。
- **关键限制（README 自述）**：① 生成质量依赖前沿模型（点名 Claude Opus/Sonnet 4.6、GPT-5.4 级别），弱模型产出的 CLI 残缺，需要 `/refine` 多轮——时间和 token 成本不低；② 严重依赖**源码可得**，只有编译二进制、需反编译的闭源软件质量大幅下降；③ 一次生成往往不能覆盖全部能力。
- 官方测试数、demo 效果属自报，真实任务完成率未见第三方独立评测；roadmap 里"agent 任务完成率 benchmark"还是未完成项。
- 短视频把它说成"一键把软件变 AI 工具"，弱化了上述门槛。

## 6. 对个人能力有什么价值

- 理解 GUI Agent vs Agent-native 两条路线之争的代表性论据，可作为评估一切"电脑操控"类产品（computer-use、RPA+AI）的参照框架。
- 自己做本地软件自动化时多一个选项：有源码/有 Python API 的软件，可考虑用它生成 CLI 后挂进现有 harness。

## 7. 对企业 AI 落地有什么价值

- 对内部自研系统：与其等 RPA 点 UI，不如直接让开发侧暴露 CLI/API——本项目提供了方法论和生成工具，可降低"给老系统补 agent 接口"的成本。
- 对英科大量闭源商业软件（NC 等）：受"源码可得"限制，适用性有限，仍应走官方接口/中间表路线；不建议因此重开 UI 自动化口子。
- 与现有 RPA 退场策略互补：它论证了"程序化接口优于界面操控"应作为默认架构原则。

## 8. 可做的小实验

- 选一个本机已装、轻量且有源码/脚本接口的软件（候选：Audacity 或 Obsidian），用 Claude Code 跑一次 `/cli-anything` 生成，实测：生成耗时、token 消耗、生成 CLI 的测试通过率、能否真完成一个端到端任务。
- 浏览 CLI-Hub 注册表，看是否已有与本组工作直接相关的 harness，避免重复造轮子。

（纯咨询阶段未执行，待明确指令再动手。）

## 9. 风险和边界

- 成本：生成 + refine 依赖贵模型，可能比直接写脚本更贵。
- 幻觉/质量：弱模型生成的 CLI 可能命令错误或覆盖不全，需测试把关。
- 供应链：社区贡献的 harness 来自第三方，安装进生产环境前需审代码。
- 闭源软件场景基本不适用。

## 10. 当前结论

方向判断价值高、一手证据扎实，是"agent-native 接口 > GUI 操控"路线的旗手级项目；但"通用、一键、零门槛"是短视频的简化说法，实际依赖强模型、源码和多轮打磨。先定位为**雷达观察 + 待小实验**，不进业务试点，也不据此调整 RPA 既定策略。
