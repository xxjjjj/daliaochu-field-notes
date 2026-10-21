---
title: "Felix 的三层上移法与 Karpathy 漏斗：AI 使用野心的差距"
date: 2026-10-21
discovery_source:
  type: 视频
  title: "AI教学：往上想一层、往下挖一层、往外借一招（B站/视频号自媒体，含UP主本人三个实测）"
  url: 群内分享，未附原始链接（章节：月报实测/盲猜播放量/5个AI分身排片单）
primary_object:
  type: methodology
  name: "Felix Rieseberg：go one abstraction layer up；Karpathy：AI understanding funnel"
  url: "https://www.lennysnewsletter.com/p/how-i-ai-how-the-engineer-behind ; https://x.com/karpathy (2026-10-11)"
object_type: [methodology, trend_signal, case_or_media]
source_type: [YouTube, 群聊线索, 官网]
business_tags: [运营, 管理, ITBP, 个人能力]
problem_tags: [流程提效, 知识沉淀, 组织协同]
method_tags: [Agent, Prompt, 自动化]
tool_tags: [Claude Cowork, Codex]
value_stage: 学习理解
risk_tags: [幻觉]
public_level: public
---

# Felix 的三层上移法与 Karpathy 漏斗：AI 使用野心的差距

## 1. 这是什么

一条自媒体视频对两个一手观点的二次演绎：

- **Felix Rieseberg**（Anthropic 工程主管，负责 Claude Cowork / Claude Code Desktop，曾任 Slack、Notion、Stripe，Electron 核心维护者）在 Lenny's Podcast 网络节目 "How I AI" 中讲自己怎么用 Claude，并给出核心心法。
- **Karpathy** 2026-10-11 在 X 重发并锐化了他 4 月的"AI 理解漏斗"：公众对 AI 的认知不是两个阵营互相对不上话，而是一个越来越陡的漏斗。

UP 主把 Felix 的方法翻成三句话——**往上想一层、往下挖一层、往外借一招**，并用自己账号后台数据让 Codex 跑了三个实测（自动做月报、藏起播放量让 AI 盲猜、开 5 个 AI 分身排片单）。

## 2. 原始来源

- 发现入口：打捞处群内视频简介（2026-10-21，晶晶转）
- 一手本体（Felix）：Lenny's Newsletter 节目页 https://www.lennysnewsletter.com/p/how-i-ai-how-the-engineer-behind
  - YouTube: https://youtu.be/-tdNsYi8AXs
  - 官方图文工作流拆解: https://www.chatprd.ai/how-i-ai/felix-rieseberg-claude-code-cowork-workflows-for-3d-house-design-and-hardware-buddy
  - 同主题长访谈：Latent Space（2026-03-17，"Why Anthropic Thinks AI Should Have Its Own Computer"）
- 一手本体（Karpathy）：2026-10-11 X 帖（原帖 4 月发布，10 月重发）；第三方逐条解读与数据核对 https://cellcog.ai/blog/karpathy-llm-understanding-funnel
- UP 主原视频链接：群消息未附，待补。

## 2b. 一手原文摘录（2026-10-21 补，均为本人原话）

### Felix Rieseberg（How I AI 节目，YouTube -tdNsYi8AXs，字幕逐字）

抽象层上移（家具例子原话）：
> "There was a brief moment where I started entering manual furniture... In the middle of it, I was like, 'What am I doing? I'm just going to tell Claude I have this piece of furniture.' And then I went another step up. I was like again, what am I doing? Why do I need to tell Claude what kind of furniture I have? ... applying this step up in abstraction of the task and doing that over and over and over again, and then spending all of my energy on the piece that is more fun and more creative."

Anti-to-do list（他给高管带教时的规则）：
> "My rule I call the anti-to-do list: anytime you're doing something that's tedious, I smack their hands — why are you doing this, your hands are not allowed to touch this, you need to go ask again, go that abstraction layer up, how could Claude do this, and then how could I never do this again."

再上一层=系统化：
> "The next abstraction would be what's the system that in a year or two, if I have to ask this question again, it's already there and ready for me to go."

调试工作流不是调试模型：
> "I usually try to debug my workflow... 'Here's what I actually expected. Can you walk me through at what point something went different, or how we can prevent this in the future?' ... The learning I take away is not 'Claude can't do this'... I can change the harness, the prompt... Usually my answer is to also involve Claude and say, 'Claude, what do you think we can do here?'"

模型选择：
> "For most tasks Sonnet 4.6 is very very good... I reach for Opus if I have self-identified as someone who doesn't really know yet what they're asking for."（Opus 的价值是把笼统问题分解成具体问题）

### Karpathy 原推全文（X @karpathy，2026-10-11 19:12 UTC，6.6M views，本人 napkin math）

> "Dusting off this tweet from April because this gap in shared understanding of LLM capability is *widening*. It's now less 'two groups of people speaking past each other' and more a sharp funnel.
> - Napkin math somewhere around 6B people (~75%) have barely come in contact with LLMs at all.
> - Around 1-2B (~20%) are casual and infrequent users of free-tier ChatGPT-like products. This group sees derpy chatbots and treats them like a better Google search, a writing aid... Even many professionals outside math&code are in this tier. Execs spend a lot of time talking to other people, so while they understand intellectually, it is still second-hand and abstract.
> - Somewhere around 20M people (0.2%) see first-hand that large, complex projects that used to take them weeks/months can now be completed by agents with a prompt. Building apps, copying apps, translating apps, decompiling apps from binaries... Less than 1 year ago, I was writing code manually by hand, occasionally pressing Tab to autocomplete.
> - ~5,000 people (~0.00006%) with access to frontier-grade systems internally... swarms of thousands of agents collaborating over weeks on software mega projects: minting zero days, cyber attacks and defenses at machine speeds, discovering new science... human review and comprehension are starting to fall behind.
>
> The funnel is driven by: 1. Impact scales with ambition, problem size, and horizon. A question with a paragraph answer barely stresses the system. You need a reservoir of big, difficult problems that you really care about. 2. The jaggedness of the system. Capability peaks in domains that are digital, verifiable and economically valuable — RL on verifiable rewards. 3. Access. Free-tier, paid-tier, internal.
> ... And it is all happening at the same time."

原推：https://x.com/karpathy/status/2109361546505966046（4 月原推 /status/2042334451611693415）

## 3. 核心观点 / 核心能力

### 3.1 Felix 的一手框架（节目原话核实）

1. **最大的障碍是心理的，不是技术的**：工具已经很强，但用户没有"遇事就想到交给 AI"的肌肉记忆。他的触发规则——每当你在做一件烦且不creative的事，停下来问：Claude 能不能干？
2. **Go one abstraction layer up, then do it again（往上抽象一层，然后再上一层）**：搬家做户型图时他先手输家具尺寸→停，让 Claude 自己推断他有哪些家具→再上一层，让 Claude 直接去他的邮件收据里找家具和尺寸。关键模式：每次发现自己在做 tedious work，先问 AI 怎么做，再问**AI 能不能不用你告诉就知道该做什么**。
3. **邮件是未被开采的个人数据库**：收据、确认单、行程、病历都是结构化数据，你缺的只是把 AI 指过去。
4. **Live artifacts（活的产物）**：接入 Gmail/Calendar/Spotify 等数据源后，报告、dashboard 自动刷新，不用再手动更新。
5. **模型选择启发式**：问题边界清楚用 Sonnet；"我自己都没想清楚要什么、需要它帮我分解问题"时用 Opus。
6. **AI 犯错时调试的是工作流而不是模型**："这是我预期的结果，你能复盘哪里走偏了、以后怎么防吗？"——改提示词、清理数据源、加 dry run，而不是骂模型。
7. 小孩是最好的 AI 用户，因为成年人被 20 年"电脑做不到"的心智监狱关住了。

### 3.2 Karpathy 漏斗（2026-10-11，数字为其本人 napkin math）

| 层 | 人数 | 占比 | 看到的 AI |
|---|---|---|---|
| 几乎没接触过 LLM | ~60 亿 | ~75% | 无 |
| 免费聊天机器人用户 | 10–20 亿 | ~20% | 更好的搜索/写作助手 |
| 在数学和代码中专业使用前沿模型 | ~2000 万 | **~0.2%** | 一个 prompt 换来 agent 完成数周到数月的工作 |
| 前沿实验室内部 | ~5000 人 | ~0.00006% | agent 集群连续工作数周 |

漏斗的脖子在变窄：普通用户看到的 AI 和专业用户在用的 AI，差距是被构造出来的（有自动判分器的领域 RL 进步最快，钱也集中在那里）。

### 3.3 UP 主的"三句话"翻译——可信度分层

- **往上想一层**：✅ 与 Felix 一手框架直接对应（abstraction layer up）。月报实测是该模式的标准应用（不要手工导数据做表，让 AI 直接对接后台/数据源产出成品）。
- **往下挖一层**（藏起答案让 AI 盲猜播放量，用预测偏差暴露自己对数据的理解盲区）：⚠️ 一手节目中没有直接对应，更像 UP 主自己的方法论演绎。思路本身成立（用"先预测再揭晓"做校准训练），但不能挂 Felix 的名字当原话。
- **往外借一招**（开 5 个 AI 分身、各给一个角色/视角排片单）：⚠️ 同理，是"多角色/persona 编排"的民间说法，非 Felix 原话。对应到实践是多 agent 分工。
- 三个实测（月报、盲猜差近 10 倍、5 分身排片单）均为 **UP 主自报、无独立复测**；"差 10 倍"是节目效果，盲猜偏差高度依赖给了什么背景数据，不能当作模型能力结论。

## 4. 我学到了什么

- 真正的分水岭不是"会不会写 prompt"，而是**把多少层的元工作也交出去**：做表 → 取数 → 判断该取什么数 → 判断该回答什么问题。大多数人停在第一层。
- "往上一层，再上一层"有明确的停止条件：再往上就是**定目标和担责任**，那一层不能交。这与我们已有的口径一致——确定性逻辑工具化、模型只贴判断点、人事/审批/对客户承诺不替人拍板。
- Felix 的"调试工作流不是调试模型"与本组排查实践同源：AI 做错先查数据源、权限、指令、有没有 dry run，而不是换模型或怪模型笨。

## 5. 它是否可信，哪些需要验证

- Felix 身份、节目、七条 takeaway：已对一手节目页核实，可信。
- Karpathy 漏斗：原帖为其本人 napkin math，口径明确（他自己承认是估算）；Menlo Ventures 2026 消费者 AI 报告（约 20 亿用户、24%）侧面支持前两层；第三层 2000 万与 OpenAI 自报 Codex 周活 500 万+、Copilot 5000 万（含自动补全，口径不同）只能粗略对齐。
- 待验证：① UP 主原视频与频道（链接待群里补）；② "5 万人收藏"这一传播数字未核实平台与口径；③ 三个实测的具体做法和提示词未公开，无法复测；④ "往下挖/往外借"归属 Felix 的说法应视为二次创作。

## 6. 对个人能力有什么价值

- 自检题：手头任何一件重复烦事，连问两遍"能不能再往上交一层"。
- 月报类工作的正路不是"AI 帮我写得好看"，而是把数据源接好、让产物变成 live artifact——本组的周期巡逻、网站安全巡检、CRM 取数问答已是这个形态，可继续向"群里说句话事就办了"推进。
- "先盲猜再揭晓"可作为个人数据直觉的校准练习，但注意它是思维训练不是能力评测。

## 7. 对企业 AI 落地有什么价值

- 推广 AI 的最大杠杆是**心理解锁和肌肉记忆**，不是培训 prompt 语法——给员工看见"整件事被交出去"的真实样板比发手册有效，这与 J-Herm 带教材料应突出"牛的机制+真实数据"一致。
- 英科场景里最值得"再上一层"的是报表和台账类工作：不是让 AI 辅助填表，而是让系统直接从销售易/RPA/后台取数产出成品，人只做异常确认。
- 漏斗的组织含义：企业内部也有自己的漏斗，0.2% 那层的人（能委托整周/整月工作的）和大多数员工的生产力差距会继续拉大；团队带教应把人往漏斗下一层带，而不是停在"聊天机器人用法"。
- 边界：live artifact 直连业务系统涉及权限与数据安全，企业内必须走最小权限、dry run、操作审计；个人邮件数据库那套玩法不能照搬到客户数据。

## 8. 可做的小实验

- 挑一个本组月报/周报，按"再上一层"重画委托边界：从"AI 帮我排版"上移到"AI 自己取数、自己发现异常、只把需要人判断的点列出来"，对比耗时和错误率。
- 对一个我们凭经验判断的指标（如某类工单量、某功能使用率），先各自盲猜再拉真实数据，季度做一次校准。
- 多分身编排暂不需要专门实验——现有 delegate/多 agent 机制已覆盖，等有真实任务（如多方案评审）时顺带用即可。

## 9. 风险和边界

- 二手演绎污染：视频把 UP 主自创的两条方法与 Felix 原话混讲，引用时必须分开标注，勿当成 Felix 官方三方法对外传播。
- "0.2%""差 10 倍""5 万收藏"都是带传播目的的数字，对内分享时说明口径。
- 向上委托止于目标设定与责任承担；数据源接入涉及客户数据时走既有权限与脱敏红线。

## 10. 当前结论

一手内核真实且高质量：Felix 的"抽象层上移 + 调试工作流 + live artifact"和 Karpathy 的漏斗，合起来讲的是同一件事——**AI 差距主要是委托野心的差距**。视频的三句话翻译好记、适合做科普钩子，但其中两句是 UP 主演绎、三个实测未复测，转引时要分层。对本组最直接的行动含义：继续把报表/台账类工作往"数据源直连+异常才找人"推进，带教时给真实样板而不是 prompt 课。
