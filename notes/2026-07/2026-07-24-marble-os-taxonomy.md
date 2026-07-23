---
title: Marble os-taxonomy：小学知识图谱开源资料
date: 2026-07-24
discovery_source:
  type: short_video_signal
  title: 码农下班聊AI：小学知识拆成1500点，还开源了
  url: https://v.douyin.com/bgk7xk-0KZQ/
primary_object:
  type: open_source_dataset
  name: withmarbleapp/os-taxonomy
  url: https://github.com/withmarbleapp/os-taxonomy
object_type: [open_source_project, education_dataset, methodology]
source_type: [抖音, GitHub, 媒体报道]
business_tags: [产品, ITBP, 个人能力]
problem_tags: [知识沉淀, 个性化学习, 用户洞察, 流程提效]
method_tags: [知识图谱, DAG, 结构化数据, AI教育]
tool_tags: [GitHub, JSON]
value_stage: 待验证
risk_tags: [版权, 合规, 适用性, 数据质量]
public_level: sanitized
---

# Marble os-taxonomy：小学知识图谱开源资料

## 1. 这是什么

这条资料线索指向 Marble 开源的 `os-taxonomy`：一个面向小学/基础教育阶段的结构化知识图谱数据集。它把儿童学习内容拆成细粒度 micro-topics，并用先修依赖关系组织成图，而不是传统的平铺课程目录。

公开资料显示，该数据集包含约 1,590 个 micro-topics、3,221 条 prerequisite dependencies，覆盖 Science、Mathematics、English、History、Personal & Social Development、Life Skills、Computing、Learning to Learn 等 8 个学科/领域。

## 2. 原始来源

- 发现入口：抖音短链，标题线索为“小学知识拆成1500点，还开源了”。
- 资料本体：GitHub `withmarbleapp/os-taxonomy`
- 相关链接：
  - GitHub: https://github.com/withmarbleapp/os-taxonomy
  - 可视化入口: https://withmarble.com/curriculum
  - 媒体报道: https://unwire.hk/2026/07/10/marble-os-taxonomy-open-source/life-tech/school

## 3. 核心观点 / 核心能力

1. **把“知识”拆成可计算节点**  
   每个 micro-topic 有名称、描述、年龄区间、学科/领域、掌握证据、评估提示、课程标准映射等字段。

2. **把学习顺序显式化**  
   dependencies 用 DAG 表达“某知识点依赖哪些先修知识”，且区分 hard / soft dependency，并给出依赖理由。

3. **把课程内容变成 AI 可操作的数据结构**  
   对 AI 教育产品而言，重点不只是生成讲解，而是能根据学生掌握状态动态规划学习路径、补齐前置知识、生成评估问题。

4. **开源的是知识组织方式，不只是题库**  
   它更像一套“学习目标和路径规划底座”，可用于个性化学习、诊断测试、课程推荐、家长沟通解释等场景。

## 4. 我学到了什么

这类资料真正有价值的地方，不在“把小学知识点列出来”，而在于它把隐性专家经验拆成了机器可读的结构：

- 节点：一个可教、可测、可解释的小概念；
- 边：概念之间的先修关系；
- 证据：怎样判断已经掌握；
- 标准：它对应什么外部课程体系；
- 解释：为什么这个概念依赖另一个概念。

这套思路可以迁移到企业内部知识体系：例如 CRM 操作、报价规则、流程审批、实施交付、财务/RPA 异常处理等，都可以从“文档堆积”升级成“任务能力图谱”。

## 5. 它是否可信，哪些需要验证

当前已追到 GitHub 和公开报道，项目本体明确；但是否适合直接复用，需要继续验证：

- GitHub 仓库的完整 README、数据目录、校验脚本、License 文本需要进一步本地拉取检查；
- 需确认 ODbL 1.0、CC BY-SA 4.0、上游课程标准许可之间的边界；
- 数据质量需要抽样看节点描述、依赖关系是否可靠，是否存在过度简化或文化/地区课程偏差；
- 对中文教育场景或企业培训场景不能直接套用，需要重新建模和本地化。

## 6. 对个人能力有什么价值

对 AI 产品/企业应用设计者的启发是：不要只会让模型“回答问题”，而要学会把领域知识拆成：

- 能力单元；
- 前置条件；
- 掌握证据；
- 评估问题；
- 路径推荐；
- 风险边界。

这比单纯写 prompt 更接近“可长期运营的 AI 能力系统”。

## 7. 对企业 AI 落地有什么价值

可借鉴到以下方向：

1. **业务流程学习地图**  
   把新员工需要掌握的 CRM、ERP、OA、报价、审批、客户保护等知识拆成节点和依赖，形成可诊断、可推荐的学习路径。

2. **智能问答从 FAQ 升级为能力图谱**  
   当用户问一个问题时，系统不只给答案，还能判断他缺的是哪一个前置概念，并推荐补充资料。

3. **销售/产品知识训练**  
   对产品卖点、竞品差异、适用场景、风险边界建立知识图谱，辅助销售培训和新人上手。

4. **AI Agent 规划任务前的“先修检查”**  
   Agent 在执行复杂业务任务前，可先判断缺少哪些字段、权限、流程前置条件，而不是盲目执行。

## 8. 可做的小实验

建议做一个小型企业版验证：

- 选一个低风险领域，例如“终端客户保护申请”或“CRM 线索录入”；
- 拆 30-50 个知识/操作节点；
- 为每个节点补齐：说明、前置条件、掌握证据、常见误区；
- 用 JSON/YAML 表达依赖关系；
- 让 AI 根据用户问题判断“当前卡在哪个节点”，并生成下一步解释或操作建议。

## 9. 风险和边界

- 许可边界：ODbL 和 CC BY-SA 都有署名与 share-alike 要求，不能无感嵌入闭源商业数据产品；
- 适用性边界：英美课程标准不等于国内教育或企业培训标准；
- 数据质量边界：知识依赖关系可能有主观性，必须抽样验证；
- 产品边界：知识图谱只是底座，真正有效还需要用户画像、掌握度评估、反馈闭环和内容生成质量控制。

## 10. 当前结论

这是一个值得继续研究的开源教育知识图谱案例。短期价值不是直接拿来做教育产品，而是学习它的“知识拆解 + 依赖图 + 掌握证据 + AI 路径规划”方法。下一步应拉取 GitHub 仓库本体，检查数据结构、License、校验脚本和样本质量，再判断能否沉淀为企业知识图谱/培训路径设计 playbook。
