---
title: Skill Graph Autopilot v0：让 skill 路由自进化的实验记录
date: 2026-04-30
related_note: notes/2026-04/2026-04-12-skills-sh-agent-skills-directory.md
experiment_type: 流程验证
business_tags: [ITBP, 个人能力, 知识沉淀]
problem_tags: [Skill发现, 选型对比, 流程提效, 自进化系统]
method_tags: [Skill, 自动化, Graph, 路由, 自进化, Autopilot]
tool_tags: [Hermes, skill_graph_autopilot.py, xhs, xiaohongshu-skills]
value_stage: 可小实验
risk_tags: [自动化风险, 误判, 日志噪音, 边界]
public_level: sanitized
---

# Skill Graph Autopilot v0：让 skill 路由自进化的实验记录

## 1. 实验背景

随着打捞处 / Hermes 内部 skill 数量增多（数十个），出现两个问题：

1. **路由不准确**：用户输入"小红书热门文案"时，skill graph 推荐的 Top1 是 `xiaohongshu-skills`，但预期应该是 `xhs`。
2. **人工评测集无法持续**：不能每次新 skill 进来都靠人工标注评测集；需要一个能"自动观察真实使用日志 + 自动判断推荐错位"的系统。

本实验记录 v0 的设计思路：先把"路由问题"自动抓出来（**观察-only**），再考虑自动改配置 / 自动改 skill。

## 2. 实验假设

只要能解析"用户真实 query + skill graph 推荐结果 + 实际响应"三条线，就能用脚本自动化发现路由错位问题，而不需要人工标注。

## 3. 实验对象

- 飞书群：打捞处
- 脚本：`/Users/crystalxu/.hermes/skill-wiki/skill_graph_autopilot.py`
- 数据源：Hermes 真实 chat 日志

## 4. 实验步骤

1. **解析用户真实 query**：从 chat 日志中抽取真实 query。
2. **拉取 skill graph shadow recommendations**：对比当前 skill graph 给出的推荐结果。
3. **采集响应指标**：响应耗时、api_call_count、是否触发 fallback、用户是否追问等。
4. **diff 推荐 vs 期望**：用规则或 embedding 算"推荐是否合理"。
5. **每日摘要推送**：09:00 把昨日的扫描结果以"日报"形式发到飞书，不刷原始日志。

## 5. 输入材料

- Hermes 真实 chat 日志
- skill graph 当前权重 / 配置
- skill metadata（标签 / 触发关键词 / 优先级）

## 6. 结果记录（脱敏示例）

> Skill Graph OS 日报
>
> 昨日扫描：
> - 解析对话：27
> - 有图谱推荐：11
> - 检测到路由问题：1
>
> 主要问题：
> 1. 小红书热门文案场景
>    - 当前 Top1: xiaohongshu-skills
>    - 期望: xhs
>    - 错位原因：标签冲突 / 触发关键词相似

## 7. 失败点 / 异常点

- **规则判定 vs embedding 判定**：v0 用关键词相似度判定"推荐是否合理"，对长 query / 多意图 query 判定不准。
- **真实期望值缺失**：v0 还没有"哪条 query 应该用哪个 skill"的 ground truth；判定标准实际是"人工抽查"。
- **日志噪音**：把原始日志直接刷群会刷屏，必须先做摘要 + 异常提醒。
- **新 skill 进入流程不闭环**：v0 还没解决"新 skill 进入时如何自动画像 / 自动归类 / 自动试跑"。

## 8. 是否值得继续

值得继续，但下一阶段应该把"自进化"拆成 6 步独立模块，避免一上来就全做：

1. **A. 自动画像**：读 SKILL.md，抽"它能干 / 不能干 / 标签"。
2. **B. 自动归类**：按业务标签自动归档到 skill graph 节点。
3. **C. 自动试跑**：用一组 synthetic query 跑 skill，验证基本功能。
4. **D. 自动观察**：从真实日志中按 query 统计"这个 skill 被命中多少次 / 准确率 / 用户反馈"。
5. **E. 自动判断**：当某个 skill 实际命中率 < 阈值时，自动降低它在 skill graph 中的权重。
6. **F. 自动迭代**：每周 / 每月根据 D+E 的统计自动调整 skill graph。

## 9. 下一步动作

1. 把 v0 的"每日摘要"流程跑满一周，统计"检测到路由问题"的真实占比。
2. 设计 v1：把 6 步（A-F）拆成独立函数，逐步上线，每步独立验证。
3. 引入 embedding-based 判定（替代纯关键词相似度），提升长 query / 多意图 query 的判定准确率。
4. 把日报输出从"飞书消息"升级为"飞书文档 + 卡片链接"，避免信息沉底。
5. 评估是否需要"skill ground truth"数据集——用 LLM 自动生成 synthetic query + 期望 skill 对，作为评测基准。

## 10. 公开边界

public_level 标记为 `sanitized`。本实验卡只公开流程、统计指标、失败点和下一步动作；不公开：

- 具体 query 内容；
- 具体 skill 内部实现；
- 用户真实聊天原文；
- 错位路由的具体 skill 名称在公开仓库以脱敏代号表示。
