---
title: "微软 UFO 系列：从 Windows GUI Agent 到 UFO³ Galaxy 跨设备编排"
date: 2026-10-10
discovery_source:
  type: 群聊线索
  title: 打捞处群内分享的《微软UFO智能体解决方案核心信息梳理》
  url: ""
primary_object:
  type: open_source_project
  name: microsoft/UFO
  url: https://github.com/microsoft/UFO
object_type: [open_source_project, article, trend_signal]
source_type: [GitHub, 论文, 群聊线索]
business_tags: [运营, 管理, ITBP]
problem_tags: [流程提效, 组织协同]
method_tags: [Agent, RPA, 自动化, computer-use]
tool_tags: [UFO, UFO2, UFO3-Galaxy, MCP, WebSocket]
value_stage: 待验证
risk_tags: [国内可用性, 权限, 成本, 合规]
public_level: public
---

# 微软 UFO 系列：从 Windows GUI Agent 到 UFO³ Galaxy 跨设备编排

## 1. 这是什么

微软研究院的开源 GUI Agent 系列（Python，MIT 协议，github.com/microsoft/UFO），三代演进：

- **UFO（2024-02）**：单设备 Windows GUI 自动化框架，LLM 理解界面控件后执行任务。
- **UFO²（2025-04，arXiv:2504.14603）**：定位"桌面 AgentOS"。HostAgent 做任务分解、AppAgent 专精各应用；深度系统集成（Windows UIA / Win32 / COM），GUI+API 混合执行（不再只靠截图点坐标），投机式多动作规划降低每步 LLM 开销，PiP 画中画在隔离虚拟桌面执行、不干扰用户。论文在 20+ 真实 Windows 应用上评估。
- **UFO³ Galaxy（2025-11，arXiv:2511.11332）**：跨设备编排框架，7.3 万+行代码，覆盖 Windows / Linux / Android。核心是 Task Constellation——把请求拆成动态 DAG（节点 TaskStar、边 TaskStarLine），ConstellationAgent 负责构图与改图，TaskOrchestrator 基于 asyncio 事件驱动异步并行执行；设备间走 WebSocket 的 AIP（Agent Interaction Protocol），带心跳、断线重连、故障时重试/任务迁移/DAG 局部重写，并有 DAG 一致性的形式化不变量证明。设备 Agent 支持 MCP server 做工具扩展。UFO² 已进入 LTS，可独立使用，也可作为 Galaxy 的 Windows 设备节点。

## 2. 原始来源

- 发现入口：群内整理稿（二手梳理）。
- 论文：UFO² https://arxiv.org/abs/2504.14603 ；UFO³ https://arxiv.org/abs/2511.11332
- 代码与文档：https://github.com/microsoft/UFO 、https://microsoft.github.io/UFO/
- 微软研究院页面：https://www.microsoft.com/en-us/research/publication/ufo2-the-desktop-agentos

## 3. 核心观点 / 核心能力

- 主张把"自动化"提升为 OS 级原语：GUI 控件、应用 API、任务编排都成为可编程、可检视、可组合的系统服务，而非外挂脚本。
- GUI+API 混合是务实关键：有 API/COM/UIA 可用时不走视觉点击，兼顾鲁棒性与表达力。
- Galaxy 层把单设备能力推广为分布式 DAG 编排：声明式分解、结果驱动的运行时图改写、按设备能力画像（AgentProfile）分派、并行加速、形式化安全保证。

## 4. 我学到了什么

- **"深集成 > 纯视觉截图"是 computer-use 落地的分水岭**：UFO² 用 UIA 拿控件树、用原生 API 执行，和本组 RPA"UI 自动化脆弱"的痛点是同一个教训。
- **编排与执行分层是可借鉴的架构**：ConstellationAgent（规划）+ 设备 Agent（执行）+ 统一通信协议，本质是"幕僚长 + 专业兵"在设备维度的落地；DAG 运行时改写（诊断节点、回退路径、依赖重连）对应故障自愈的工程化。
- **MCP 正在成为 Agent 工具层事实标准**，微软系项目也在全面接入。

## 5. 它是否可信，哪些需要验证

- 版本、时间、功能描述与官方论文/GitHub 一致，整理稿主体可信。
- 群稿中两处需打折：
  - "效率提升 3-5 倍"：官方 README/论文摘要未见该数字，疑似二手添加，**待核论文实验章节**。
  - "支持 macOS"：UFO³ 论文摘要明确工程实现为 Windows/Linux/Android；官网对比表虽列出 macOS，但代码成熟度存疑，按"规划中/社区扩展"看待。
- "形式化验证"范围是 DAG 一致性不变量（结构正确性），不是任务结果正确性，别被措辞误导。
- 论文为微软研究团队自报，基准与评测未经第三方独立复测。

## 6. 对个人能力有什么价值

- 学习多 Agent 桌面系统的标准架构：Host/App 分工、混合控制检测、speculative execution。
- 学习异步 DAG 编排的工程实现：事件驱动调度、并发锁、原子批量编辑、失败后图重写。

## 7. 对企业 AI 落地有什么价值

- 与本组 RPA 立场对照：UFO² 路线说明"深系统集成 + API 优先"正是降低 UI 自动化脆弱性的方向，和我们"能走接口就不上 UI 自动化"的口径一致；它仍属补丁层，适用老系统/无接口场景。
- Galaxy 的跨设备 DAG 编排可作为内部"多机协同流程"（如一台取数、一台分析、一台出报告）的开源参考实现，比商业 RPA 编排器更贴近 LLM-native。
- 微软生态（Windows + Office + Azure OpenAI）绑定明显，公司桌面环境以 Windows 为主时部署门槛低；但需 Azure OpenAI 或兼容 LLM 端点。

## 8. 可做的小实验

- Windows 测试机上跑通 UFO²：配置 OpenAI 兼容端点（可试火山 ark-plan 端点），跑 1-2 个 Office/文件整理任务，记录成功率、步数与 token 消耗。
- 阅读 `galaxy/` 源码，评估 AIP 协议与设备注册机制能否复用到自研编排；暂不上多设备集群。

## 9. 风险和边界

- 数据安全：Agent 可读取屏幕与本地文件，接外部 LLM 端点时存在数据外泄面，企业内应走私有/合规端点。
- 权限与合规：自动执行具备真实操作权限，需限定账户权限范围并保留审计轨迹。
- 国内可用性：默认面向 Azure OpenAI / OpenAI，需自行替换端点；微软生态绑定带来迁移成本。
- 成本：每步多模态调用的 token 消耗，需实测后评估与传统 RPA 的成本差。

## 10. 当前结论

雷达观察，价值明确：UFO 系列是目前开源 GUI Agent 里工程化程度最高的一支，UFO³ Galaxy 的"DAG 编排 + 设备 Agent + MCP"架构值得对标。暂不业务试点，先在 Windows 测试机做 UFO² 小规模实测，并核实 3-5 倍提速说法与 macOS 支持现状。
