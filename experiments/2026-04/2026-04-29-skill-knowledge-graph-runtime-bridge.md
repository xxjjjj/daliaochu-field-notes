---
title: Skill Knowledge Graph Runtime Bridge：从"skill wiki"到"skill runtime"
date: 2026-04-29
related_note: experiments/2026-04/2026-04-30-skill-graph-autopilot.md
experiment_type: Agent工作流
business_tags: [ITBP, 个人能力, 知识沉淀]
problem_tags: [skill发现, skill路由, skill上下文, skill记忆]
method_tags: [Skill, Knowledge Graph, Mermaid, Hermes, skill-wiki]
tool_tags: [Hermes, llm-wiki, skill-wiki, Mermaid, Mermaid graph]
value_stage: 可业务试点
risk_tags: [维护成本, schema漂移, 死链]
public_level: sanitized
---

# Skill Knowledge Graph Runtime Bridge：从"skill wiki"到"skill runtime"

## 1. 测试背景

打捞处群聊 2026-04-29，用户回顾并继续推进一个已经进行了一段时间的实验：**把本地 Hermes 的所有 skill 整理成知识图谱**（skill-wiki），并尝试在 Agent runtime 里以图谱的方式查询、推荐、组合、记忆 skill。

本卡是这个长周期实验 2026-04-29 当时的进度盘点 + 后续方向。

## 2. 测试假设

1. Skill 不是"原子命令"，而是"可积累、可关联、可演化的知识对象"，可以挂触发条件、用途、边界、依赖、替代、组合、冲突、上下文、历史案例。
2. 把 skill 建成 graph（不是普通目录树）能让 Agent 在运行时做更精准的 skill 发现和组合。
3. Machine-readable graph（Mermaid + JSON）+ query/recommend 原型能让 skill 不再是"装了就忘"的资产。

## 3. 测试对象

- **根目录**：`/Users/crystalxu/.hermes/skill-wiki`
- **数据规模**：截至 2026-04-29 19:21 共 123 个 skill、25 个 category、173 个 graph node、若干 graph edge
- **核心 schema**：
  - skill = 节点
  - 触发条件、用途、边界 = 属性
  - 依赖、替代、组合、冲突 = 关系
  - 历史案例、失败模式、上下文 = 附加 metadata

## 4. 测试步骤（截止 2026-04-29 已完成）

1. ✅ 扫描本地 Hermes 的 skills 清单
2. ✅ 建立 skill-wiki 根目录
3. ✅ 设计最小 schema（节点 + 关系 + 属性 + metadata）
4. ✅ 建 index 文件
5. ✅ 批量编译至少一个 skill（llm-wiki）
6. ✅ 产出 machine-readable graph（Mermaid + JSON 两种格式）
7. ✅ 实现 query/recommend 原型
8. ✅ 补上进度报告 + Mermaid 图谱概览

## 5. 输入材料

- 本地 Hermes skills 目录：`~/.hermes/skills/`
- Skill 编译脚本：未公开，需在 Hermes 仓内维护
- Mermaid 模板：标准 flowchart + classDiagram

## 6. 结果记录

### 6.1 当时已落地的能力

| 能力 | 状态 | 备注 |
|------|------|------|
| 节点构建 | ✅ | 123 个 skill 全量入库 |
| 关系抽取 | ✅ | 依赖 / 替代 / 组合 / 冲突 |
| machine-readable graph | ✅ | Mermaid + JSON 双格式 |
| query 原型 | ✅ | 按触发条件 / 用途 / 边界 检索 |
| recommend 原型 | ✅ | 基于当前任务推荐相关 skill |
| 进度报告 | ✅ | 自动生成 |
| Mermaid 概览图 | ✅ | 全量图谱可视化 |
| 上下文继承 | ⚠️ 部分 | 能查 skill 历史，但 session 级上下文未打通 |
| 失败模式记忆 | ❌ | 计划中 |
| 跨会话状态 | ⚠️ 部分 | 依靠 session_search |

### 6.2 当时遇到的关键挑战

1. **context 撑爆**：跑完整图谱概览时 context 使用率从 81% 飙到 96%，最后被 auto-compaction 强制压缩
2. **schema 漂移**：早期 schema 没有版本控制，不同时间编入的 skill 字段不完全一致
3. **死链问题**：被删除 / 改名的 skill 在 graph 里留下悬挂引用
4. **运行时未打通**：graph 是离线产物，Agent runtime 里能不能基于 graph 自动选 skill，当时还没有完整跑通

## 7. 失败点 / 异常点

### 7.1 context 压力

graph 全量生成 Mermaid 概览时，几乎把所有上下文都吃掉了。这暴露了一个根本问题：**graph 必须分层、按需查询，而不是全量加载**。

### 7.2 runtime 没接通

有 graph 不等于 runtime 能用。要做到"Agent 在收到用户请求时自动查 graph → 选 skill → 调用"，还有一段路要走。

### 7.3 skill 命名空间混乱

不同 skill 之间存在命名冲突（如两个 skill 都叫"reminder"），需要在 graph schema 里强制 namespace。

## 8. 是否值得继续

**值得**。这个实验的价值不在"做出 graph"，而在"为 skill 提供一个可查询、可推荐、可记忆、可演化的运行时基础设施"。后续阶段重点：

1. **runtime bridge**：让 Agent 在每次任务开始时自动 query graph
2. **失败模式记忆**：把 skill 调用失败的案例反哺到 graph，避免重复踩坑
3. **跨实例同步**：多人 / 多机器共享同一份 skill graph（基于 git + JSON）

## 9. 下一步动作

### 9.1 短期（1-2 周）

- 接入 `experiments/2026-04/2026-04-30-skill-graph-autopilot.md` 的自进化路由
- 把 query/recommend 从命令行工具升级为 Skill
- 建立 schema 版本号（v1 / v2 / ...），所有 node 必须挂 schema 版本

### 9.2 中期（1 个月）

- 实现 runtime bridge：Agent 收到请求 → 查 graph → 选 skill → 调用 → 回写状态
- 解决 context 撑爆问题：graph 必须分层加载，只加载当前任务相关的 subgraph
- 建立 skill 健康度指标：调用频率 / 失败率 / 用户反馈

### 9.3 长期（3 个月）

- 跨实例同步：把 skill graph 放到 git repo 里，多人协作编辑
- 自动 schema 漂移检测：每次新 skill 接入前先校验 schema 版本
- Graph → UI 可视化：开发一个简单的 web UI，让非技术用户也能浏览 / 推荐 / 编辑 skill graph

## 10. 公开边界

- graph 节点的具体 skill 名称不公开
- graph 的 Mermaid 概览图脱敏后只展示结构，不展示具体内容
- 编译脚本和 schema 不公开（涉及 Hermes 内部架构）
- 群聊 message_id 仅作回查用途，不公开

## 11. 相关链接

- 配套实验：`experiments/2026-04/2026-04-30-skill-graph-autopilot.md`
- skill-wiki 根目录：`/Users/crystalxu/.hermes/skill-wiki`（本地）
- llm-wiki Skill 说明：`notes/2026-04/2026-04-19-llm-wiki-skill-karpathy-methodology.md`
