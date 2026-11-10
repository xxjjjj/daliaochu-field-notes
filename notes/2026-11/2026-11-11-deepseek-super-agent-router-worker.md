---
title: DeepSeek Super-agent：Router/Worker 双层多智能体调度（含开源原型核验）
date: 2026-11-11
discovery_source:
  type: 抖音视频（口播二手解读，已追一手论文+开源仓库核验）
  title: DeepSeek 的多智能体调度思路（未来博士wepon）
  url: https://v.douyin.com/oJwds2OIBIc/
primary_object:
  type: article+open_source_project
  name: Exploring Super-agent Design: Towards a More Autonomous Agent System（DeepSeek Harness 团队，2026-10-31）+ dsh-superagent-prototype
  url: https://github.com/deepseek-ai/dsh-superagent-prototype
object_type: [article, open_source_project, methodology, trend_signal]
source_type: [抖音, GitHub, 官网/官方X]
business_tags: [ITBP, 管理, 个人能力]
problem_tags: [流程提效, 组织协同, 知识沉淀]
method_tags: [Agent, 自动化, Prompt]
tool_tags: [DeepSeek Harness, dsh-superagent, 飞书, GitHub]
value_stage: 可小实验
risk_tags: [成本, 数据安全, 权限, 幻觉]
public_level: public
---

# DeepSeek Super-agent：不按岗位拆 Agent，按"接待/干活"两层拆

## 1. 这是什么

DeepSeek Harness 团队 2026-10-31 发布的一篇系统设计文章《Exploring Super-agent Design:
Towards a More Autonomous Agent System》，同日开源 MIT 协议研究原型
`deepseek-ai/dsh-superagent-prototype`（截至 2026-11-01 约 5.4k stars，核验时 21.7k
stars、2.6k forks，但**只有 1 个 commit**）。抖音视频是对该文的二手口播，主体与事实
基本准确，但漏掉了成本代价、授权模型和开放问题。

核心命题：单任务干得好不好看模型聪不聪明；同时推进多任务时，瓶颈是**人的注意力**
（一个人只能同时盯 3-5 个 agent 会话）。解法不是按公司岗位封"产品 agent/前端 agent/
测试 agent"，而是按职责分两层：常驻 **Router**（只接待、派活，不执行）+ 上下文隔离的
**Worker**（一事一会、配最强模型、干完汇报，不直接对人说话）。

## 2. 原始来源

- 发现入口：抖音视频 https://v.douyin.com/oJwds2OIBIc/ （未来博士wepon，二手解读）
- 资料本体：
  - 论文（官方 X 机器翻译英文版）：https://x.com/DeepSeekHarness/status/2116857257245987236
  - 开源仓库：https://github.com/deepseek-ai/dsh-superagent-prototype （MIT）
  - 仓库文档编号 000-008：philosophy / domain / authorization / execution / prompts /
    panel / deployment / architecture / cookbook，原型原生支持 GitHub + 飞书两个 Source
- 相关链接：
  - explainx.ai 逐模块解读（含实验表与可复制清单）：
    https://explainx.ai/blog/deepseek-super-agent-architecture-router-worker-triggers-watchdog-2026
  - 基座框架 DeepSeek Harness（DSH，2026-08-13 随 V4 Pro 发布，插件式、不绑定 DeepSeek 模型）

## 3. 核心观点 / 核心能力

**双层架构（论文 §2）**
- Router：全局序点/协调点/记忆点，只做人机交互和任务分派，**可以完全不挂 Bash 工具**；
  任务比想象的简单，轻量模型即可。
- Worker：一个任务一个独立会话、上下文隔离、全套工具、最强模型；默认互不通信，
  只向 Router 汇报，由 Router 统一对外回复，避免多个会话在群里各自回话。
- Worker 冲突不靠点对点通信：涉及同一 PR 的 Worker 都订阅该 Thread 事件流，
  收到同样的事件自行发现和解决冲突（多对多订阅）。

**事件驱动 Trigger（§3），四个机制**
1. 快/慢两路过滤：程序化策略（来源/群/人白名单）先拦掉绝大多数闲聊，剩下才进 LLM
   语义判断。这是全文最便宜、今天就能抄的设计。
2. Thread 订阅直通：首事件走 Router 建工，同 Thread 后续事件程序直送 Worker，
   不再过 Router。实测为 Router 省约 37% 缓存 token（但不省任务时间）。
3. 上下文当缓存管：Inline（常用对象+相邻 ID 直接进窗口）/ Spill（长文截断留指针）/
   Chain of Recall（每个对象内联关联对象 ID+摘要，模型按需一跳跳召回）。
4. 投机执行：群里随口报个 bug，后台静默复现、修复，确认有效才出声，否则全程沉默。
   代价是后台算力，社区已质疑"大部分投机最后用不上"。

**全双工多信道（§4）**：阻塞式 run() 改成异步 start()/stop()，后台跑工具前台不堵；
按信道定输出规范——IM 群聊硬上限**每条回复 100 词以内**，GitHub/邮件/文档才写结构化长文。

**存算分离（§5）**：无状态云容器跑 Agent Loop，会话状态进数据库，本机只跑轻量
daemon 执行命令（BYOM，带本机未提交代码和凭据，上线时再补跑排队任务）。合盖不停活。

**Watchdog（§6）**：主系统之外的独立 agent 会话，定时消息驱动。不只探活，而是用模型
判断每个 Worker 有没有实质进展、是否跑偏，支持自然语言热修提示词和触发规则。

## 4. 我学到了什么（视频之外的一手增量）

1. **"靠提示词守规矩不靠谱"被消融实验坐实，不是段子**：中性提示词下单会话 agent
   5 次实验 0 次派活；强"优先派活"提示词下 4/5 派活，第 5 次判断活太小自己干，
   当场被新消息打断拖出长尾。双层设计直接取消这个选择——Router 没工具，想干也干不了。
   口诀：**不靠自觉靠制度，规则写进结构而不是写进提示词。**
2. **视频没讲的授权模型（docs/002）才是工程上最成熟的部分**，且原生以飞书为示例：
   - 主机强制白名单/规则/Mandate，消息内容只能作为决策输入，**不能改变权限**
     （天然防提示词注入提权）。
   - 强度分 explicit（白名单的人明确 @）/ inferred（推断候选）/ not admitted。
   - 接受模式三档：`off`（推断事件全不接）→ `observe`（默认，只记录"本来会接"，
     不真执行）→ `auto`（评估通过才自动接）。官方建议先 observe 审记录再切 auto。
   - Mandate 带 Policy 版本号，恢复任务前重新校验授权；人说 stop 是普通 Trigger
     要过评估，Executor 不能自己声称"用户让我停的"。
3. **性能数字的另一半是成本**：Super-agent p50 12.4 分钟 / p95 15.7 分钟，
   单会话+subagent 是 20.4 / 29.5 分钟（视频说的"12 分半、最慢 15 分"属实）；
   但每次运行 cache-read token 23.2M，是对照方案 6.1-10.8M 的 **2-4 倍**。
   用 token 买时延和稳定性，这笔账视频没提。
4. 测试是合成负载：约 300 条消息里只有 20 个工作请求，编码用只消耗时间的模拟工具，
   **测的是调度吞吐和稳定性，不是编码质量**（视频倒是诚实说了"任务是假的"）。
5. Router 压测反直觉结论：事件越忙越划算——触发速率 1→40 条/分钟时批处理合并，
   单决策分摊推理时间 6.4s→1.6s。团队当前规模下不需要分片 Router，
   分片=吞吐换全局记忆连续性。

## 5. 它是否可信，哪些需要验证

可信部分：
- 论文、仓库均在官方 `deepseek-ai` 组织下，MIT 协议，实验方法和消融口径公开；
  口播视频的关键数字（12.4/15.7 分钟、5 次实验 1 次翻车、程序先筛 9 成消息、
  watchdog、合盖不停）与一手材料逐一吻合，无硬伤。
- 仓库状态已实测：21.7k stars 但仅 1 个 commit，README 自述"研究原型、非官方产品、
  可能有 bug、公开版与内部实现不一致、文中部分设计尚未实现"。**star 数不能当成熟度。**

待验证/存疑：
- 端到端实验只有 5 次重复、合成任务、模拟编码，真实多文件代码协作的表现无公开数据。
- 投机执行的算力浪费率、误报打扰率，论文未给度量，社区已点名这是空白指标。
- 四大开放问题论文自己承认未解：Router 分片时机与跨月长程记忆、去 Session 化的
  人机界面、跨 Agent（A2A）安全通信、端云凭据隔离与提示词注入防护
  （原文警告：Agent Loop 一旦被注入，所有接入的端点设备可能被接管）。
- 第三方尖锐评价（立场是同类厂商，需打折）：架构无新颖性，Router-Worker/事件驱动/
  持久任务都是老工程思想，真正价值在"显式任务记录+程序化授权追踪+Thread 订阅"的
  集成方式，即"集成胜过架构新颖性"。

## 6. 对个人能力有什么价值

- 拿到一个判断多智能体方案的硬标准：**看它把规则写在结构里还是写在提示词里**。
  凡是"在 system prompt 里嘱咐 agent 要分工/要派活/别多话"的设计，都要追问
  翻车概率和兜底机制。
- "上下文即缓存"的 Inline/Spill/链式召回，是设计任何长周期 agent 记忆系统的通用语汇，
  比笼统说"做记忆"可操作得多。
- 注意力经济学：philosophy 文档把每条消息/通知/提问/面板元素都计为注意力成本，
  产出必须大于成本否则删掉——这也是做群机器人的产品审美。

## 7. 对企业 AI 落地有什么价值

- **与我们支持群自动化口径直接互证**：我们定的"先只看只摘要两周，再逐场景开执行"
  就是 dsh 的 `observe → auto` 模式；它的 admission table（按群/按人/按是否 @
  分 explicit/inferred/ignore 三格）给了我们一张可以直接抄的权限配置表，
  比按场景逐个口头约定更可审计。
- 飞书群场景的两条零成本规则今天就能用：①模型调用前先过程序白名单/群模式过滤；
  ②群聊回复设百词上限，长结论走文档/卡片。
- 给业务方讲"为什么不建议按岗位买一堆数字员工"时有了一手证据：岗位拆分=提示词头衔，
  接待与执行的结构分离才是并发能力的来源。数字员工建档仍按岗位命名服务于人，
  但后台编排应按任务隔离。
- Router 用轻量模型、Worker 用最强模型的分层配模型思路，与"贵模型动脑、便宜模型
  动手"的成本治理方向一致且更精细：协调层本来就该便宜。

## 8. 可做的小实验

1. （最低成本，纯规则）给现有飞书群机器人加两道代码级闸门：入站先白名单+关键词过滤
   再进模型；出站群消息限 100 词，超长转卡片/文档。
2. 把支持群各场景做成一张 admission table 草案（行=群/私聊，列=白名单内未@、
   白名单外@、白名单外未@；值=explicit/inferred/ignore），对照现有自动化场景查漏。
3. 若要落 Router 型主控 Skill，可用官方验证过的起步指令（来自一手解读，可直接改写）：
   > 你是协调者，不执行命令、不改文件。每个新请求开一个独立 worker 并给简短任务书；
   > 同一 PR/话题的后续消息交给持有它的 worker；群聊回复 100 词以内；
   > 只有需要人做决策时才提问。
   配合 cron 定时检查 worker 进展并标记卡住的任务，即最小 Watchdog。
4. 暂不建议直接部署 dsh-superagent-prototype：单 commit 研究代码、依赖 DSH 生态
  （Node 22+/pnpm/tmux/gh 等）、安全模型要求高度自控，先读 docs/002 授权章节取经即可。

## 9. 风险和边界

- 成本：2-4 倍 cache token + 投机执行的后台空转算力，规模化前必须先建 token/打扰度量。
- 安全：事件驱动=不可信输入面扩大，提示词注入可直达有本机凭据的执行环境；
  原型的防线是主机强制授权（消息无法改权限），自建时这条不能省。
- 组织：并行 Worker 的合并冲突、重复实现、矛盾解释是组织问题不是技术问题，无法根治。
- 成熟度：研究原型非产品，公开版功能 < 论文设计，勿按论文清单对供应商做验收要求。
- 二手传播风险：口播视频天然省略成本与开放问题，转发时应补上"2-4 倍 token、
  合成测试、四大未解问题"三句，避免管理层形成"已有银弹"的误判。

## 10. 当前结论

视频质量在二手解读里算上乘，核心事实经一手核验无误，但真正值得我们抄的不是
"Router+Worker"这个拓扑（老工程思想），而是三样被视频略过的东西：
**结构强制的角色约束（Router 无工具）、observe→auto 的授权灰度机制、
注意力成本优先的产品哲学**。对我们飞书群自动化和未来数字员工编排是可直接小实验的
参考，停在规则借鉴，不引入原型本体。
