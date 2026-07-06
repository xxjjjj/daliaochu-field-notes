---
title: Skill2Loop：从 Agent Skill 到可持续改进的 Loop Engineering
date: 2026-07-06
discovery_source:
  type: short_video_lead
  title: 抖音线索：skill只是agent学习的起点，把 skill 做成 loop engineering
  url: https://v.douyin.com/eQ6NOqEqohU/
primary_object:
  type: methodology
  name: Skill to Loop Engineering
  url: https://addyosmani.com/blog/loop-engineering
object_type: [methodology, case_or_media]
source_type: [短视频, 文章, GitHub, 群聊线索]
business_tags: [ITBP, 管理, 个人能力]
problem_tags: [流程提效, 知识沉淀, 组织协同, 质量控制]
method_tags: [Agent, Skill, Loop Engineering, 自动化, 反馈回路]
tool_tags: [GitHub Actions, GitHub Issues, MCP, Agent Skills]
value_stage: 可小实验
risk_tags: [成本, 权限, 幻觉, 数据安全, 自动化失控]
public_level: sanitized
---

# Skill2Loop：从 Agent Skill 到可持续改进的 Loop Engineering

## 1. 这是什么

这条资料的核心不是单个 skill 文件怎么写，而是一个更高层的工程范式：**skill 只是 Agent 学习和执行的起点，真正产生复利的是把 skill 放进可观察、可反馈、可审查、可迭代的 loop 里。**

可以理解为：

- Skill：把一次经验写成可复用规则。
- Loop：让规则在真实任务中反复运行、收集反馈、更新规则。
- Skill2Loop：从“我写一个技能给 Agent 用”，升级为“我设计一个系统，让 Agent 执行、观察、复盘、提 PR，人审后持续改进技能”。

## 2. 原始来源

- 发现入口：打捞处群内抖音短视频线索，关键词为 “skill2loop / Skill 的 loop engineering”。
- 可追到的公开资料：
  - Addy Osmani: Loop Engineering — https://addyosmani.com/blog/loop-engineering
  - Warp / Zach Lloyd: How to build a self-improvement loop for your Skills（LinkedIn 文章，指向示例仓库）
  - 示例仓库：warpdotdev-demos/issue-triage-loop
  - 相关平台文档：warpdotdev/oz-for-oss docs/platform.md
- 当前状态：短视频本体未直接解析到完整文本，但已追到同主题公开文章、GitHub 示例和方法论解释。

## 3. 核心观点 / 核心能力

Loop Engineering 的核心观点是：不要只手动 prompt Agent，也不要只沉淀静态 skill，而是设计一个能持续触发、执行、检查、记录、学习的循环。

典型 loop 包含：

1. **Trigger / Automation**：由定时任务、Webhook、GitHub Issue、飞书消息、CI 失败等触发。
2. **Skill / Procedure**：Agent 按照明确的 Skill 或 Playbook 执行任务。
3. **Execution / Work Unit**：对具体对象做处理，例如分诊 issue、生成回复、修复报错、整理资料。
4. **Observation / Feedback**：收集人类修改、反应、评论、标签变化、测试结果、业务确认。
5. **Reflection / Generalization**：把一次性反馈转成可泛化规则，而不是记录琐碎个例。
6. **Skill Update / PR Review**：由 Agent 提出 skill 更新，但通过 PR 或人审进入主流程。

## 4. 我学到了什么

这条资料对我们最有价值的点是：**Skill 不应该停在“写给 Agent 的说明书”，而应该成为一个可运营资产。**

静态 skill 的问题：

- 写完后容易过期。
- 人类纠错散落在群聊、评论、测试记录里，没有回写。
- Agent 每次仍可能重复踩坑。

Skill2Loop 的改进：

- 把人的纠错、业务反馈、测试失败变成下一版规则。
- 让经验沉淀留在 Git 历史里，能审查、能回滚、能比较版本。
- 把“AI 学习”从黑盒参数更新，变成可读、可控、可管理的组织知识更新。

## 5. 它是否可信，哪些需要验证

可信点：

- Addy Osmani 对 Loop Engineering 的定义较系统，强调自动化、worktree、skill、connector、sub-agent、memory 等基本组件。
- Warp 的 Issue triage loop 有较清晰的公开案例：内循环负责分诊，外循环基于维护者反馈更新 skill，并通过 PR 人审。
- GitHub Issue/PR 天然有结构化反馈：label 变化、评论、reaction、关闭原因、review 结果，适合作为 loop 的观察层。

待验证点：

- 公开案例主要面向开源工程协作，不等同于企业内部飞书/CRM/RPA 场景可以直接照搬。
- 自动改 skill 有“学偏”的风险，必须区分强信号、弱信号、一次性个例和可泛化规则。
- 成本与安全需要验证：循环频率、上下文大小、工具权限、失败重试、停止条件都可能造成不可控消耗。

## 6. 对个人能力有什么价值

这类方法训练的是“设计反馈系统”的能力，而不是单纯写 prompt：

- 学会把工作拆成可重复任务、观察信号和改进规则。
- 学会把人的判断变成可版本化知识。
- 学会给 Agent 设计停止条件、校验条件和人审节点。
- 学会从“我来盯 Agent 做事”，转向“我设计一个系统帮我盯”。

## 7. 对企业 AI 落地有什么价值

企业 AI 落地最难的不是跑通一次，而是持续稳定、可控、能积累。Skill2Loop 正好对应这个问题。

可迁移方向：

- **飞书群聊自动化**：把每次回复是否被纠正、是否被追问、是否命中业务语境，沉淀回回复 skill。
- **CRM AI 化**：把意图识别失败、字段缺失、权限拦截、业务改口，沉淀为意图识别和追问规则。
- **RPA/NC 错误闭环**：把报错类型、修复动作、回归结果，沉淀为诊断 playbook。
- **打捞处资料沉淀**：把资料解析、追源失败、公开边界判断、价值转译，沉淀为学习 loop。
- **审批/问前搭子类场景**：把人类最终处理方式作为反馈信号，持续修正“什么时候直接答、什么时候追问、什么时候不承诺”。

## 8. 可做的小实验

建议先做一个低风险 MVP：

1. 选一个反馈天然清晰的场景，例如打捞处资料沉淀或 GitHub issue/实验卡片。
2. 定义内循环：收到资料 → 追源 → 摘要 → 判断 public_level → 写 note → commit。
3. 定义外循环：定期读取最近 notes/experiments 的人工修改、补充评论、后续结论。
4. 只提取可泛化规则，例如“短视频线索必须追到公开文章/GitHub/官网，否则 value_stage=待追源”。
5. 外循环不直接改主规则，先生成 PR 或单独 playbook 更新，人工确认后合并。

## 9. 风险和边界

- **不要让 loop 无限制运行**：必须有预算、频率、最大迭代次数、停止条件。
- **不要让 Agent 自己给自己判满分**：关键场景需要 maker/checker 分离，或者至少有独立验证步骤。
- **不要把个例写成规则**：必须要求证据数量、信号强度和适用范围。
- **不要把内部数据直接喂进公开仓库**：公开沉淀只写脱敏后的方法、结构和判断，不写群聊原文、客户数据、系统截图。
- **不要绕过人审改核心 skill**：外循环可以建议修改，但不能自动扩大权限或修改关键风控规则。

## 10. 当前结论

Skill2Loop 是值得继续跟的方向。它把“Agent Skill”从静态说明书升级为可运营、可反馈、可审查的持续改进系统。对我们来说，最适合先落在低风险、高频、有反馈的场景：打捞处学习沉淀、飞书自动化回复质量改进、CRM AI 化意图识别纠偏、RPA/NC 报错闭环。

下一步建议做一个“打捞处学习 loop”的小实验：用 GitHub 历史修改和人工补充作为反馈信号，定期提炼对 salvage-card、experiment-card、playbook 的改进建议。