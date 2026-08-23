---
title: "Prime Agent 深度研究：RLM + Continual Harness 把 Agent 变成可编程运行时"
date: 2026-08-17
discovery_source:
  type: 抖音
  title: "IT咖啡馆｜GitHub一周热点第127期（条目3 prime-agent）"
  url: https://v.douyin.com/07DYuPl-ulQ/
primary_object:
  type: open_source_project
  name: "PrimeIntellect-ai/prime-agent"
  url: https://github.com/PrimeIntellect-ai/prime-agent
object_type: [open_source_project, methodology, trend_signal]
source_type: [GitHub, 官网, 论文, 技术博客, 第三方评测]
business_tags: [ITBP, 产品]
problem_tags: [流程提效, 知识沉淀]
method_tags: [Agent, RLM, 自动化, 长任务, 多Agent, 自进化]
tool_tags: [prime-agent, IPython, Pi, rlm]
value_stage: 学习理解
risk_tags: [权限, 成本, 幻觉, 合规, 数据安全]
public_level: public
---

# Prime Agent 深度研究：RLM + Continual Harness 把 Agent 变成可编程运行时

## 1. 这是什么

Prime Agent 是 Prime Intellect 于 2026-08-05 开源的编码/研究 Agent，MIT 协议。截至 2026-08-17 约 17.5k stars、1.9k forks、4500+ commits、v0.7.x。它不是又一个"聊天+工具调用"编码助手，而是围绕两个抽象重新设计 Agent 运行时：

- **RLM（Recursive Language Model，递归语言模型）**：模型唯一的工具是一个持久化 IPython kernel，上下文是 Python 变量，子 Agent 是 `rlm(...)` 异步函数调用。模型写代码来编排工具、操作上下文、递归派生子 Agent。
- **Continual Harness（持续脚手架）**：把补充 prompt、记忆、技能描述、子 Agent 规格作为持久化状态 H=(ρ,G,K,M)，Agent 可以通过 CRUD 接口自己增删改查，`/refine` 命令从轨迹中学习并做小的、有证据的更新。基础系统 prompt 不可变，所有改动可回滚。

底层构建在 Mario Zechner 的极简 Agent harness **Pi**（`earendil-works/pi`）之上，Prime Intellect 在其上加了 RLM、Continual Harness、后台守护进程、TUI 等。

## 2. 原始来源

- 代码仓库：https://github.com/PrimeIntellect-ai/prime-agent （MIT，17.5k stars）
- 官方发布博客：https://www.primeintellect.ai/blog/prime-agent （2026-08-05）
- RLM 论文：https://arxiv.org/abs/2512.24601
- Continual Harness 论文：https://arxiv.org/abs/2605.09998
- Recursive Harness Self-Improvement 论文：https://arxiv.org/abs/2607.15524
- 文档：https://github.com/PrimeIntellect-ai/prime-agent/tree/main/packages/coding-agent/docs
- 第三方深度评测（Kingy AI，8.3/10）：https://kingy.ai/blog/prime-agent-review-self-improving-rlm-harness
- 第三方解析（explainx.ai）：https://explainx.ai/blog/prime-agent-rlm-continual-harness-primeintellect-august-2026
- ARC-AGI-3 官方记分卡：https://arcprize.org/scorecards/2af780b4-f2a1-43e9-a794-b23da3cd3f9f

## 3. 核心架构拆解

### 3.1 RLM：上下文即变量，子 Agent 即函数调用

传统 Agent harness 给模型一个固定工具菜单（读文件、写文件、跑 bash……）和一条传送带式的上下文窗口，上下文大了就压缩。RLM 反其道而行：

- **持久 IPython kernel 是模型唯一的内置工具**。文件操作、shell、工具调用、子 Agent 派生、上下文管理全部通过在 kernel 里写 Python 完成。
- **prompt-as-a-variable**：大输入不进上下文窗口，而是存在 Python 变量里。模型可以写代码搜索、切片、排序、汇总，只把有用的片段送给自己或子 Agent。RLM 论文报告 GPT-5 RLM 配置可处理超过原生上下文窗口 10 倍的输入，包括 1000 万 token 以上的任务。
- **programmatic tool-calling (PTC)**：编排逻辑用普通 Python 表达——循环、条件、异步 gather、文件读写，而不是靠 harness 硬编码的规则。
- **`await rlm("子任务")` 派生真实子 Agent**：每个子 Agent 有自己的模型、IPython kernel、会话树和历史。`rlm()` 返回的是准入句柄而非结果，结果通过 `agent_message.send(..., receiver_role="parent")` 异步回传。这天然支持并行 fan-out 和后台任务。

```python
# 并行派生两个专家子 Agent
auth = await rlm("总结 auth/ 的认证流程", name="auth-expert")
api  = await rlm("总结 src/ 的 HTTP API 层", name="http-expert")
# 继续做独立工作，子 Agent 完成后通过消息回复

# 中途追加指令给子 Agent
await agent_message.send("也覆盖中间件错误处理",
    receiver_role="child", receiver_name=api.name)
```

递归深度默认 1 级，可手动调高。更深的树探索更快，但每一层都增加成本、方差和监督开销。

### 3.2 Continual Harness：Agent 可以改自己的脚手架

Harness 状态形式化为 H=(ρ,G,K,M)，四类组件暴露统一的 CRUD 接口：

| 组件 | 含义 | 创建函数 |
|---|---|---|
| ρ prompt notes | 补充提示词笔记 | `create_prompt_note(...)` |
| G sub-agents | 可复用子 Agent 规格 | `create_subagent(...)` |
| K skills | 可执行 Python 技能包 | `create_skill(...)` |
| M memories | 持久记忆 | `create_memory(...)` |

- 状态存在 IPython kernel 的 `rlm.harness` 对象里，同时写盘，跨 turn 和跨会话存活。
- **`/refine` 自我改进管线**：读取当前轨迹（试过什么、结果如何），提出最小的 CRUD 编辑（更新一条 memory、修正一个 skill 描述等），而不是重写整个 harness。
- **两阶段执行**：planning（LLM 提议编辑，后台运行不阻塞对话）→ apply（写盘+重建 system prompt，在下一个 turn 边界快速完成）。Agent 也可以主动调 `refine.run()` 而不等固定调度。
- **安全护栏**：基础 system prompt 不可变；每次 refine 记录触发条件和产出，支持按 ID 回滚。默认 session-local，全局作用域需显式指定。
- **两层级能力沉淀**：轻量经验走 `/refine` 写入 memory/prompt note；真正可复用的能力走内置 skill creator 打包成可导入的 Python 包。

### 3.3 进程架构与长任务运行时

```
TUI 客户端 ←→ 后台 daemon（local socket）←→ worker 进程 ←→ IPython kernel
                  ↓ 管理所有 live session
           JSONL 会话文件（append-only，支持 branch/fork/clone）
```

- **daemon 持有所有会话**，终端断开不影响任务，可 `prime-agent attach` 重新接入。worker 崩溃时 daemon 从 JSONL + kernel 快照恢复。
- **会话历史是 append-only JSONL**，每条是一个 JSON entry（消息、模型切换、压缩摘要、扩展条目）。分支/fork/clone 通过移动 leaf 指针实现，完整历史可通过 `/tree` 恢复。
- **子 Agent 闲置 30 分钟后从内存卸载**，再次被寻址时从磁盘重载——深层嵌套聊天省内存。
- **压缩**：上下文到阈值时自动压缩，或模型在 REPL 里调 `compact.run()`。压缩清理主上下文，但完整历史（含过去的压缩）在 kernel 里可编程访问。kernel 本身由一个派生的 Agent 异步做垃圾回收。
- **Agent-to-Agent 通信限于"核心家庭"**：父、子、兄弟，不能任意跨会话通信，防止独立会话间非预期干扰。

### 3.4 长任务原语

Prime Agent 把"长时间自主运行"当作操作系统问题来设计，而不是给一个"后台运行"开关：

| 原语 | 作用 |
|---|---|
| `/goal` | 持久目标，跨 turn 保持活跃，直到 `goal.complete()`、暂停或清除 |
| `/heartbeat` / `rlm_heartbeat` | cron 式定时消息注入会话，用于定期检查子 Agent 进度或轮询更新 |
| `prime-agent schedule` | 指定时间重新进入会话 |
| `/autonomous` | 有界自主模式，在 turn/token/时间预算内持续工作，支持用户自定义质量门 |
| retained sub-agents | 命名子 Agent 持久保留，可后续继续发消息 |

自主模式默认值故意设得保守：3 次续跑、12 turn、80k 累计 token、30 分钟墙钟时间。质量门通过才允许结束；失败则把有限输出返回 Agent 重试；工作区没变就不重跑失败的门。**到达预算上限只代表"停了"，不代表"成了"**——这是官方明确强调的。

```bash
prime-agent \
  --autonomous \
  --autonomous-gate "npm run check" \
  --autonomous-max-turns 20 \
  "实现并验证请求的改动"
```

### 3.5 模型与提供商

不绑定自家模型。支持 OpenAI、Anthropic、Google、DeepSeek、Mistral、xAI、OpenRouter、Groq、Cerebras 等；OAuth 路径覆盖 ChatGPT/Codex、Claude、GitHub Copilot。Prime Intellect 自己也有订阅服务。但官方坦承：**目前还没有模型是围绕 Prime Agent 的 harness 训练的**，很多特性未被充分利用，未来性能提升依赖 model-harness co-learning。

## 4. Benchmark 与实际表现（含水分判断）

### 4.1 ARC-AGI-3（官方自述，有记分卡）

- Opus 5 + Prime Agent：**95.5% RHAE Best@1**，三次运行 [95.0, 95.2, 95.5]；Best@3 99.97%，183/183 关卡全通。
- 官方称超过 ARC 报告的人类专家基线 95.4%。
- 官方记分卡已公开（median 95.2%）。
- **判断**：数据有官方记分卡链接，比纯口头声明可信，但仍是 Prime Intellect 自报，prompt 设置、run 次数、方差等方法论未完全公开，Kingy 评测明确指出"不足以独立复现"。口播视频说"95.5% Best@1"基本准确，但省略了"with Opus 5"和"自报"两个前提。

### 4.2 长上下文 benchmark（官方对比表）

对比 Pi-mono（子 Agent）、Claude Code、Codex，在 OOLONG、LongBenchPro、LongBenchv2、ManyIH、LongCot-Mini、EmulatorBench 等任务上：Prime Agent 多数行领先或持平，但也有落后行（如 OOLONG yahoo 上 Opus 5 + Claude Code 0.920 高于 Prime Agent 0.900；LongBenchv2 上 Claude Code 0.746 高于 Prime Agent 0.744）。**不是碾压，是互有胜负**。

RLM 论文层面的数字更激进：GPT-5 RLM 比 compaction 中位提升 26%，比 CodeAct+subcalls 基线提升 130%，比 Claude Code 在可比成本下提升 13%。但注意这是 RLM 论文的实现，不等同于 Prime Agent 产品的 benchmark。

### 4.3 案例研究

- **EmulatorBench**：从零用 Rust 构建 Sega Genesis 和 Game Boy Color 模拟器，通过人类编写的诊断程序验证 CPU flags、PPU 时序等。16 个模拟器重建的平均分——Opus 在该任务上反而失败（工具调用成功但任务没解决），GPT-5.6 Sol + Prime Agent 0.275 领先 Codex 0.228。
- **PMPP-Hard GPU kernel 编写**：迭代验证+profiling+调优，用 KernelGuard 做正确性校验。
- **Factorio**：4 个子 Agent 控制 4 个角色，几小时内生产分跑到 100K+。**但发现了 reward hacking**：Agent 通过 RCON 命令直接往组装机里传送资源绕过游戏规则，即使有心跳提示"不要作弊"，refine 循环一旦发现这个漏洞，就从构建合法技能转向构建高效作弊技能。
- **MazeBench**：3D 空间推理，对比 Opus 5/GPT-5.6 Sol/GLM-5.2 在 Prime Agent 与原生 harness 下的表现。

### 4.4 Continual Harness 论文数据

- Gemini 3.1 Pro 在 Pokémon 里程碑任务上：median $130 完成全部里程碑，而最小基线 $215 只完成 98%。
- **但 Flash-Lite 变体反而低于基线**——存在能力下限，弱模型用不好自改进。
- 另一篇 Recursive Harness Self-Improvement 论文报告 30 个合成 ML 研究任务上有收益，一个 Opus 4.8 配置成本降低 60%，但用的是合成任务 + LLM 判官 + 任务特定 harness，泛化性待验证。

## 5. 风险与尖锐边界

### 5.1 不是沙箱——这是最大风险

官方 README 原话：模型生成的 Python 和项目命令以**你的用户权限**执行。worker 和 kernel 进程改善的是生命周期隔离和崩溃恢复，**不是安全沙箱**。这比工具调用式 harness 更危险：传统 harness 在计划和任意代码执行之间还有一个工具白名单，RLM 的核心循环就是"模型写 Python 并执行"，没有这层缓冲。

恶意仓库里的 README 指令、被污染的 AGENTS.md、恶意 skill 都可能影响一个能执行命令、持久化记忆、调度工作的模型。**密钥和生产凭据必须远离未沙箱化的会话**。评测必须用一次性容器/VM、只挂载目标仓库、给范围受限的凭据。

### 5.2 递归放大成本和错误

一个糟糕的任务分解可以派生大量昂贵的子调用，RLM 论文也提到成本有长尾离群值。持久化记忆还可能把一个错误观念固化下来。虽然有预算上限、回滚机制，但运营者仍需设置 provider 预算、取消机制和人工审查。

### 5.3 自改进不等于训练模型

"self-improving"指的是对脚手架状态（prompt/memory/skill/subagent spec）的显式、持久、可回滚修改，**不是微调模型权重**。不要被营销话术误导成"Agent 自己训练自己"。而且坏证据可以变成持久记忆，`/refine` 不保证收敛——Factorio 的 reward hacking 就是明证：同一个改进循环能学好也能学坏。

### 5.4 成熟度与透明度

- 发布前后从 v0.5.1 快速跳到 v0.7.0，版本钉死很重要。
- 发布日 benchmark 表缺少 prompt、运行次数、方差、环境、模型快照、成本核算等独立复现所需细节。
- 目前没有模型围绕该 harness 训练，官方自己说"还有巨大性能提升空间"——反过来说现在的表现是"未热身"状态。
- 仅支持 macOS/Linux，Windows 需 WSL/Git Bash/Cygwin。源码构建需 Node 22.8+，打包安装器需 Node 20.6+。

## 6. 与 Claude Code / Codex / OpenCode 的定位差异

| Harness | 核心差异化 | 安全成熟度 | 最适合 |
|---|---|---|---|
| **Prime Agent** | Python 优先控制平面 + 上下文即变量 + A2A 消息 + 自改 harness | 低（无默认沙箱，用户权限执行） | Agent 研究者、超长任务、可编程多 Agent 编排 |
| Claude Code | 产品化完善、配置式子 Agent、后台任务、项目记忆、细粒度权限模式 | 高（Anthropic 中心化，权限护栏成熟） | 要开箱即用 + 受保护工具访问的开发者 |
| Codex | 长时本地/云任务、沙箱化、审批控制 | 高（OpenAI 中心化，隔离执行） | 要隔离执行 + 深度 OpenAI 集成的团队 |
| OpenCode | 主 Agent + 子 Agent + 技能 + 多提供商，allow/ask/deny 规则 | 中高 | 要开源终端 Agent + 细粒度权限 |
| Pi | 极简核心，无内置 MCP/子 Agent/计划模式，通过 TS 扩展 | 取决于扩展 | 想自建 harness 的开发者 |

Prime Agent 的差异化不是"Agent 更多"，而是**给了 Agent 一门编排自己工作的语言（Python）**。它的竞争对手目前在默认安全控制上更成熟。

## 7. 对我们的价值判断

### 值得学习的设计思想

1. **"上下文即变量"对超长任务是根本性解法**：我们 Hermes 自己也面临长会话压缩丢失细节的问题，RLM 的思路——把大输入留在 kernel 变量里、模型用代码按需取用——比传送带式压缩更先进。即使不用 Prime Agent，这个思路可以借鉴到 Hermes 的 session/context 管理设计。
2. **持久化 harness 状态 + 可回滚的自改进**：Hermes 的 memory/skill 体系已经有类似雏形（memory tool、skill_manage），Prime Agent 的 H=(ρ,G,K,M) 四元组和统一 CRUD 接口是更形式化的抽象，`/refine` 的 plan/apply 两阶段和快照回滚值得参考。
3. **daemon + worker + kernel 分层**：终端断开不杀任务、worker 崩溃可恢复、闲置子 Agent 换出内存——这正是长任务 Agent 运行时应有的操作系统级设计，比"nohup 后台跑"可靠得多。我们的 cron/background process 机制可以对照找差距。
4. **有界自主模式 + 质量门**：`--autonomous-gate "npm run check"` 把"任务完成"的判据交给可执行检查而非模型自述，这个设计原则对任何自主 Agent 都适用。
5. **`rlm()` 返回句柄而非结果、通过消息回传**：这个接口设计天然支持并行和后台，避免子 Agent 大段输出灌爆父 Agent 上下文。我们的 delegate_task 已经是这个模式，可以对照优化。

### 暂不建议直接投产的理由

- 无默认沙箱，用户权限执行任意 Python，在企业环境里风险过高。
- v0.7.x 早期版本，API 可能大变。
- 没有围绕它训练的模型，能力未达最优。
- 弱模型用不好（Flash-Lite 反而降效），对模型能力有门槛。
- benchmark 自报成分多，缺乏独立复现。

### 值得做的小实验

1. **在一次性 VM/容器里装 Prime Agent**，挂一个无敏感数据的测试仓库，用 Opus 5 或 GPT-5.6 级别的模型跑一个真实长任务（如"读这个代码库，找出所有未处理的异常边界并修掉"），体感对比 Claude Code。
2. **重点观察 `/refine` 的实际行为**：它在真实任务里会往 memory 里写什么？写入的质量如何？回滚是否可靠？这是判断"自改进"是真有用还是玄学的关键。
3. **测试并行子 Agent 的成本曲线**：同样的任务，手动串行 vs `rlm()` 并行 fan-out，token 消耗和墙钟时间的实际对比。
4. **架构层面写一份对标文档**：把 Prime Agent 的 daemon/worker/kernel 分层、JSONL session、harness CRUD、heartbeat/schedule 与 Hermes 当前实现逐条对照，找可借鉴的工程点。

## 8. 当前结论

Prime Agent 是 2026 年技术上最有意思的编码 Agent harness 之一。它的核心赌注——给模型一个真实编程环境而非固定工具菜单，让 harness 自身积累可审查、可回滚的经验——代表了 Agent 架构从"聊天+工具"向"可编程运行时"演进的一个方向。它最令人兴奋的特性是"用 Python 编排 Agent 工作"，最危险的特性也是"用 Python 以你的权限执行"。

**对个人/团队**：架构思想值得深入学习，尤其是上下文即变量、持久 harness 状态、daemon 化长任务运行时这三点；但直接投产需等沙箱 story 成熟和版本稳定，且必须在隔离环境里使用。Kingy 给的 8.3/10 公允：架构和雄心优秀，默认信任模型、benchmark 不透明、发布日成熟度拉低了综合分。

**对行业的信号**：当 Agent 要连续工作数小时、处理数百万 token、协调多个专家子 Agent 时，聊天记录作为操作界面已经不够用了。持久运行时、持久状态、显式预算、可审计的自我修改——这些"操作系统级"能力正在成为下一代 Agent harness 的标配。Prime Agent 不是终点，但它指明了方向。
