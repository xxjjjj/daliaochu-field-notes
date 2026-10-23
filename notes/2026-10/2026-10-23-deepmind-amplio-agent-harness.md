---
title: "DeepMind Amplio：面向长任务的崩溃可恢复 Agent Harness（Go 单二进制开源）"
date: 2026-10-23
discovery_source:
  type: AI大神雷达
  title: "AI 大神雷达 cron 2026-10-23 扫描（监测名单：Demis Hassabis / DeepMind 官方渠道）"
  url: ""
primary_object:
  type: open_source_project
  name: Amplio
  url: https://github.com/google-deepmind/amplio
object_type: [open_source_project, trend_signal]
source_type: [GitHub, 官网]
business_tags: [ITBP, 个人能力]
problem_tags: [流程提效, 知识沉淀]
method_tags: [Agent, harness-engineering, 长任务, 多智能体, 崩溃恢复]
tool_tags: [Amplio, Go, SQLite, DeepMind, Apache-2.0]
value_stage: 待验证
risk_tags: [权限, 供应链成熟度]
public_level: public
---

# DeepMind Amplio：崩溃任意时刻都能续跑的轻量 Agent Harness

## 1. 这是什么

2026-10 下旬 Google DeepMind 在官方 GitHub 组织 google-deepmind 下开源的 agent harness（Apache-2.0，Go 写的单二进制 + 内嵌 Web 前端），定位"Robust and Long-Horizon Runs"——专门解决 agent 跑长任务时崩了就前功尽弃的问题。作者 Chiyuan Zhang、Da Huang、Chen Liang、Andrew Li 等（仓库明确标注"非 Google 官方支持产品"，即研究团队放出的研究代码，不是 GA 产品）。

三个设计要点（README 原文口径）：

- **轻量 + 多 agent**：基于简单 step 模型的多智能体框架，agent 只有四类通用工具——shell、文件编辑、生成子 agent、agent 间协调；天然支持崩溃续跑和 agent/用户/环境消息协调。
- **健壮（核心卖点）**：DB-first 持久化（本地 SQLite），agentic loop 中任意时刻崩溃都能一致恢复；工具/环境失败不由 harness 吞掉，而是回报给 agent 自己处理；服务器恢复后子 agent 会话树自动续跑。
- **自主/协作双模**：每次 run 可以全自动，也可以中途插入人工交互。
- 工程细节：首次构建下载 130MB 本地 embedding 模型做 skill/lesson 召回表（可关，关掉退化为关键词匹配）；配置 ~/.amplio/config.toml，示例配置里用 Vertex 上的 Gemini 3.1 Pro / 3.6 Flash、Claude Opus 5（支持 thinking 控制与 prompt cache TTL 参数）。
- 知识组织直接用了 AGENTS.md、briefings、skills、lessons 这套词汇——和 2026 年 harness engineering 社区（含我们自己的 Skill 体系）收敛方向一致。

## 2. 原始来源

- 代码：https://github.com/google-deepmind/amplio （Apache-2.0）
- 发现方式：2026-10-23 AI 大神雷达扫描，GitHub 组织页显示仓库 10-21/10-22 仍在推送。

## 3. 一手核查到的事实（2026-10-23 GitHub 页面实查）

- 极早期：仅 **23 次提交、98 stars、11 forks**，README 自己声明非官方支持、不参与 Google 漏洞奖励计划。
- 安全提示（README 原文）：**不做 shell 沙箱**，agent 以启动服务器的用户身份执行命令，有同等文件系统/网络/凭据权限；端口可达者默认可读（token 只挡写不挡读），需按 operations 文档自行绑定收紧。
- 技术栈：Go 后端 + Node 22 构建前端，SQLite 为唯一状态存储；含 bridges（模型适配）、cmd、docs/guide 与 docs/internals 完整文档目录。

## 4. 我学到了什么（认知价值）

- **长任务 harness 的胜负手在"状态"不在"提示词"**：DeepMind 这套的核心论断是 DB-first——把 agent loop 的每一步先落库再执行，崩溃恢复就是读库续跑；工具失败当作观察结果回喂 agent，而不是 harness 层偷偷重试。这给"agent 跑几小时的活怎么保证不白跑"一个可直接照搬的架构答案。
- **顶级实验室的内部 harness 长什么样在持续公开化**：2026 年 DeepSeek Harness（8 月，插件化运行时）、DeepMind Amplio（10 月，崩溃恢复）相继开源，harness engineering 从博客概念进入"各家把内部跑法交出来"的阶段，且共识词汇（skills、lessons、sub-agent、AGENTS.md、recall）高度趋同。
- agent 的工具面刻意收敛到 shell/edit/spawn/coordinate 四个原语，"少即是多"的接口设计再次被验证。

## 5. 对企业 AI 落地 / 实施组的价值

- **架构参考，不是可直接部署的产品**：单维护团队的研究代码、98 stars、无沙箱、读权限默认开放，企业环境不能直接上生产；但"DB-first 持久化 + 会话树续跑 + 失败回喂 agent"三条应作为我们评估/自建长任务 agent（含 J-Herm 的后台任务、cron、子 agent 恢复）的设计清单。
- 给客户做 agent 方案时，"崩了怎么办、跑一半谁接手、长任务怎么审计"是必答题，Amplio 的 docs/internals 可作为讲解材料的一手引用。
- 复装实验门槛低：单二进制 + SQLite + BYO key，隔离机器/虚拟机里可以安全试跑，读它的 step 模型与恢复协议实现比读文章更直接。

## 6. 风险和边界

- **权限风险**：无沙箱 shell + 用户级凭据，禁止在有公司数据/生产凭据的环境直接跑；试跑放一次性 VM。
- **成熟度**：非官方支持、无 SLA、提交历史极短，接口随时变；引用时说"DeepMind 研究团队开源项目"，不要说成"Google 官方 agent 平台"。
- 数据点时效：stars/提交数为 2026-10-23 扫描时点，项目可能快速升温也可能停更，引用前回查。

## 7. 当前结论

| 价值阶段：**待验证 / 架构参考**。本体真实（google-deepmind 官方组织、Apache-2.0、文档完整），但年龄以天计、非支持产品。价值主要在"DB-first 崩溃恢复"的设计范式与 DeepMind 内部 harness 词汇表，建议组内做一次代码走读，不部署、不进客户方案承诺。
