---
title: "OpenAI Agents API：Codex Harness 被产品化"
date: 2026-09-16
discovery_source:
  type: 群聊线索
  title: 群内转发的二手解读稿《OpenAI Agents API 核心解读》
  url: ""
primary_object:
  type: commercial_product
  name: OpenAI Agents API (public beta)
  url: https://developers.openai.com/api/docs/guides/agents-api/overview
object_type: [commercial_product, trend_signal]
source_type: [官网, 群聊线索]
business_tags: [ITBP, 产品, 个人能力]
problem_tags: [流程提效, 组织协同]
method_tags: [Agent, 自动化]
tool_tags: [OpenAI, Codex, MCP]
value_stage: 待验证
risk_tags: [国内可用性, 数据安全, 成本, 合规]
public_level: public
---

# OpenAI Agents API：Codex Harness 被产品化

## 1. 这是什么

2026-09-10 OpenAI 公测 Agents API：把支撑 Codex / ChatGPT for Work 的整套
**agent harness（执行框架）**作为托管服务开放。开发者一次 API 调用创建一个
持久 session，给出任务、模型、工具、运行环境，框架负责上下文压缩、工具检索、
工具循环、子 agent 编排、断线恢复。底层是开源的 Codex harness
（github.com/openai/codex），OpenAI 托管运行，代码可审。

一句话：**过去要自己写的那层"agent 外壳"，现在变成了可以调用的云服务。**

## 2. 原始来源

- 发现入口：群内转发的二手解读稿（中文，无作者/出处，典型自媒体稿）
- 官方公告：https://openai.com/index/introducing-the-agents-api （2026-09-10）
- 官方文档：https://developers.openai.com/api/docs/guides/agents-api/overview
- 开源内核：https://github.com/openai/codex
- 第三方实写教程：https://flaviocopes.com/agents-api/ （含 curl/Node 示例）
- 计费：公测期无额外费用，只付 token + 内置工具用量

## 3. 核心观点 / 核心能力（以官方原文为准）

1. **持久 session + 自动 compaction**：接近上下文窗口上限时自动压缩早期上下文，
   任务可跨多个上下文窗口、按官方说法"reliably for days"；断线可从 session 恢复，
   也可中途追加输入/干预。
2. **Tool search**：工具定义按需加载，而不是一次全塞给模型——省 token、保缓存、
   降低工具过多时的选择困难。MCP 工具在支持时自动走延迟发现。
3. **Programmatic tool calling**：agent 可在代码里并行调用、串联、过滤/合并工具
   结果，只把关键结果带回上下文（大体量数据处理不撑爆窗口）。
4. **多 agent**：`multi_agent.enabled` 后主 agent 可创建/消息/等待/中断子 agent，
   每个子 agent 独立上下文；公告示例 `max_concurrent_subagents: 3`。
5. **环境解耦**：OpenAI 托管沙箱，或自有基础设施，或合作沙箱
   （Blaxel、Cloudflare、Daytona、DigitalOcean、E2B、Modal、Oracle、Runloop、Vercel）。
   沙箱可挂自己的文件、包、skills/plugins 目录、vault。
6. **原生 MCP + 自定义函数 + 内置 web search**；harness 随模型版本化演进，
   官方维护，开发者不用每次模型升级就重写外壳。

## 4. 二手稿的事实核对（审题）

| 二手稿说法 | 核对结果 |
|---|---|
| 2026 年发布、开放的是 Codex harness | ✅ 属实，2026-09-10 公测 |
| 自动压缩上下文、跨天运行 | ✅ 基本属实，但本质是 session 持久化+压缩+恢复，不是进程永生 |
| Tool search 按需加载、程序化并行调用 | ✅ 与官方文档一致 |
| 子 agent 并行、独立上下文、主 agent 汇总 | ✅ 一致；但官方示例并发上限是 3，"数百个 Agent 并行"是作者推演，非官方能力承诺 |
| 可选 OpenAI 沙箱/自有/第三方（Cloudflare、Modal） | ✅ 属实，但名单漏了一大半（共 9 家） |
| "一次调用，持续运行"、"一次调用即可实现" | ⚠️ 营销化简化。仍需自备工具/MCP/知识/环境配置/vault，省的是外壳不是业务接入 |
| "行业竞争从模型能力转向 Agent Runtime" | ⚠️ 评论观点，不是事实。可作为趋势假设观察 |
| "传统模型 API 厂商、通用编排框架将被替代" | ⚠️ 观点性预言，尚无证据；公测当天的评论稿措辞 |
| （未提）开源内核、无额外收费、MCP 原生、vault、skills 目录 | 漏点，反而是企业评估时的关键信息 |
| （未提）数据出境与国内可用性 | 重大遗漏，对国内企业是一票否决级问题 |

## 5. 我学到了什么

- **Harness 正在从"自建零件"变成"水电煤"**：context 管理、工具循环、子 agent
  编排这些每个团队都在手搓的东西，被头部厂商收进托管服务。和 RPA 层的逻辑相反——
  RPA 是没接口时的补丁，而 harness 是 agent 的本体；本体被标准化后，差异化上移到
  工具、私有知识、业务流程。
- **Tool search 承认了一个现实**：工具/能力挂太多，模型选择质量和 token 都扛不住。
  这与任何 agent 平台（包括自建助手）技能/工具数量膨胀后的治理问题是同一个问题，
  "按需检索工具定义"是可直接借鉴的设计，不一定要等托管服务。
- **"多 agent 并行"被产品化的粒度很小（3 并发、独立上下文、主 agent 汇总）**：
  印证了"幕僚长 + 少数专业兵"的科层结构，而不是大量平级 agent 社区。
- 程序化工具调用（在代码里过滤合并、只回关键结果）是省窗口的关键模式，
  比"模型自己读全部工具输出"可靠且便宜。

## 6. 对个人能力有什么价值

- 评估自建 agent 编排时多一个基准选项：**自建 harness vs 托管 harness**，
  自建的理由要能回答"官方托管已经免费送外壳，我自建多出来的是什么"
  （答案通常是：私有化模型、数据不出域、特殊工具链、可控性）。
- 可以照它的能力清单给自己的助手做体检：compaction、tool 按需检索、
  子任务隔离上下文、工具结果代码层过滤——哪些已有、哪些手搓、哪些缺失。

## 7. 对企业 AI 落地有什么价值

- 对**可使用 OpenAI 的业务场景**（海外业务、公开数据场景）：长周期调研、
  多线并行分析、跨天运维值班类 agent 的自建成本显著下降，适合小团队试点。
- 对**国内主体业务**：OpenAI 托管沙箱意味着代码、文件、过程数据出境，
  合规上基本不可用；现实路径是自有基础设施 + 开源 Codex 内核，或等国内
  模型厂商（火山、阿里、腾讯等）跟进"托管 harness"形态。**这是一个要盯的
  对标信号**：谁先把 harness 做成托管服务并跑在国内合规环境里。
- 对实施团队的长期判断：客户要的"数字员工"交付物，可能从"我们帮你搭流程"
  上移到"我们帮你接工具、接私有知识、定权限边界"——编排本身越来越不值钱。

## 8. 可做的小实验

- 读 quickstart，用最小任务（如"读一个公开网页→子 agent 分两段分析→汇总写文件"）
  实测一次 session 全流程，记录实际 token 成本、时长、失败恢复行为。
  暂不做：需要 OpenAI 账号与付费环境，列入候选，不在本期执行。
- 不依赖该服务的借鉴实验：给现有助手的工具/技能清单加一层"按需检索"机制，
  对比全量挂载时的调用准确率与 token 消耗。

## 9. 风险和边界

- **数据安全/合规**：托管沙箱数据出境，国内企业主体业务不可直接用；
  即使自有环境，调用 OpenAI 模型本身仍有数据出境问题。
- **国内可用性**：API 与沙箱的网络可用性无保障。
- **成熟度**：仍 public beta，接口会快速迭代，官方明说会根据反馈改。
- **成本不可见性**：长任务+子 agent+工具循环会放大 token 消耗，
  "无额外费用"不等于便宜，按 token 计费在跨天任务上可能失控。
- **供应商锁定**：session/vault/skills 目录都是 OpenAI 形态，迁移成本高；
  harness 虽开源，但托管 API 的会话状态不是。

## 10. 当前结论

二手稿定性正确（harness 被产品化是真信号），但混入了营销口径和未经证实的
行业预言，且漏掉开源、免费结构、MCP 与数据出境这几个企业决策关键点。
对我们的价值主要是**趋势坐标和架构参照**，不是近期可采用的工具：
盯国内厂商的同类托管形态，同时把它的四个机制（压缩、tool search、
子 agent 隔离、代码层结果过滤）作为自建助手的设计清单。

- 价值阶段：待验证（需实测 quickstart 后再升级判断）
- 下一步：关注 GA 时间、定价细则、国内厂商对标动作；需要时用海外环境做最小实测
