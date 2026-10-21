---
title: "AI 雷达监测名单（13 人）：为什么值得跟、跟什么"
date: 2026-10-21
discovery_source:
  type: 内部整理
  title: "打捞处 AI 大神雷达 cron（6d54ed28e61c）配套名单"
  url: ""
primary_object:
  type: methodology
  name: "AI 意见领袖一手信源清单"
  url: ""
object_type: [trend_signal, methodology]
source_type: [官网, 社交媒体, 内部整理]
business_tags: [ITBP, 个人能力]
problem_tags: [知识沉淀, 用户洞察]
method_tags: [Agent, 信息雷达]
tool_tags: [cron]
value_stage: 已沉淀
risk_tags: []
public_level: public
---

# AI 雷达监测名单（13 人）

配套定时任务：Hermes cron `6d54ed28e61c`「AI 大神雷达」，每周二、周五 9:30 扫描，只推一手实质内容（新论文/新框架/新开源/有料长文/实战数据），过滤转推、吵架、卖课、二创盘点；项目/论文级内容自动追源落库。名单调整直接改本文件 + cron prompt。

搭配逻辑：**Karpathy 教趋势和方法，LeCun/Chollet 唱反调，Hassabis/Noam/Jim Fan 给前沿，Tobi 给真实战报，后面五个帮省读论文的时间。**

## 第一梯队：神级研究者

### 1. Andrej Karpathy
- 身份：OpenAI 创始成员、特斯拉 AI 前总监，现 Eureka Labs 做 AI 教育
- 一手源：karpathy.bearblog.dev、github.com/karpathy、X @karpathy
- 为什么跟：顶级研究员 + 行业最好的表达者；每隔几个月造一个引爆全网的极简项目（nanoGPT、llm-wiki、AutoResearch，2026-03）；本人博客会用 LLM 整理自己的演讲全文稿
- 注意：他说的"12 月拐点"是个人体感不是统计结论

### 2. Yann LeCun
- 身份：Meta 首席 AI 科学家，CNN 之父，图灵奖得主
- 一手源：X @ylecun
- 为什么跟：主流狂热里最强的反方辩手——主张 autoregressive LLM 路线走不通，押注世界模型 JEPA、开源路线；跟他不同意见也必须知道反方最强论点

### 3. Demis Hassabis
- 身份：DeepMind 创始人（AlphaGo/AlphaFold，AlphaFold 获诺贝尔化学奖）
- 一手源：DeepMind 官方博客、X @demishassabis、重大访谈
- 为什么跟：展示 AI 能力边界——AI 做科学发现（蛋白质、材料、数学）

### 4. Noam Brown
- 身份：OpenAI 研究员，扑克 AI Libratus 与 o 系列推理模型核心人物
- 一手源：X @noamthebrown
- 为什么跟：发言极少而硬，推理与多智能体最前沿，一条都不漏

## 第二梯队：工程一线与清醒派

### 5. Jim Fan
- 身份：NVIDIA 高级研究总监
- 一手源：X @DrJimFan
- 为什么跟：Agentic AI 最高密度布道者之一（团队作品 GEPA 自动提示词优化），既懂研究又懂落地，近乎日更，跟踪"agent 现在实际能干什么"的最高效信源

### 6. François Chollet
- 身份：Keras 作者，现做 ARC Prize 抽象推理竞赛
- 一手源：X @fchollet、ARC Prize 博客
- 为什么跟：圈里最著名的冷水——坚持当前 LLM 是统计模仿、无真正推理，用 ARC 硬基准举证；防止被 demo 忽悠

### 7. Tobi Lütke
- 身份：Shopify 创始人/CEO（名单中唯一非研究者）
- 一手源：X @tobi
- 为什么跟：唯一真把 AI 灌进几千人公司生产系统的老板，发的是生产战报而非畅想（0.8B 反超 1.6B 19%、买家画像服务成本 $27M→$1M、日产量 200 万→7200 万）
- 注意：部分战果自报且有 PR 被标过拟合未合并，引用须区分口径

## 第三梯队：高质量策展/解读信源

### 8. Simon Willison
- 身份：Django 联合创始人，独立 LLM 实操博主
- 一手源：simonwillison.net
- 为什么跟：新模型当天出真实测评和坑；更新极多，雷达只筛重大条目

### 9. Nathan Lambert
- 身份：Interconnects 通讯主理人，RL/后训练方向
- 一手源：interconnects.ai
- 为什么跟：公开写作里 RL/后训练最深，想懂模型"怎么被训出来"看他

### 10. Lilian Weng
- 身份：前 OpenAI 安全负责人
- 一手源：lilianweng.github.io
- 为什么跟：一年只发两三篇，篇篇教科书级综述（RAG、agent、扩散模型），发文即必报

### 11. Jack Clark
- 身份：Anthropic 联创，Import AI 周报
- 一手源：importai.substack.com
- 为什么跟：政策 + 研究双视角，常有 Anthropic 方向感；每次只挑 1-2 条

### 12. swyx
- 身份：Latent Space 主理人
- 一手源：latent.space、X @swyx
- 为什么跟：AI 圈最好的策展/访谈者，行业风向最快入口

### 13. 宝玉
- 身份：中文圈 AI 博主
- 一手源：X @opinion_365、baoyu.io
- 为什么跟：英文长推/长论文当天翻译+划重点+配例子，中文阅读加速带

## 过滤原则（雷达执行口径）

- 只认本人一手：本人博客 > GitHub > 本人 X > 权威访谈转述；二创盘点号、"最佳工具 Top N"不作证据
- 营销引流（转售 API key、知识星球、卖课）直接丢弃
- 单次推送 ≤3 条，宁缺毋滥；无合格内容完全静默
- 去重状态：~/.hermes-v019/cron/state/ai-guru-watch.json
- 项目/论文级内容自动按打捞处卡片落库 notes/ 并判断 public_level
