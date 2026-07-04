---
title: AG-UI：Agent-User Interaction Protocol，把 Agent 从纯文本聊天推向可交互应用
date: 2026-07-04
discovery_source:
  type: 小红书线索
  title: "Agent还在纯文本聊天？AGUI开源协议来了！3种「刹车」..."
  url: http://xhslink.com/o/6p7FzBMRav7
primary_object:
  type: open_source_project
  name: ag-ui-protocol/ag-ui
  url: https://github.com/ag-ui-protocol/ag-ui
object_type: [open_source_project, protocol, trend_signal]
source_type: [小红书, GitHub, 官网, 文档]
business_tags: [ITBP, 产品, 运营, 管理]
problem_tags: [流程提效, 组织协同, 用户体验, AI落地]
method_tags: [Agent, 自动化, 人在回路, 事件流]
tool_tags: [AG-UI, CopilotKit, MCP, A2A]
value_stage: 可小实验
risk_tags: [数据安全, 权限, 合规, 成本, 幻觉]
public_level: public
---

# AG-UI：Agent-User Interaction Protocol，把 Agent 从纯文本聊天推向可交互应用

## 1. 这是什么

AG-UI（Agent-User Interaction Protocol）是一个开源、轻量、事件驱动的 Agent 与用户界面交互协议。它试图解决的问题不是“模型怎么更聪明”，而是“Agent 如何从纯文本聊天窗口进入真实业务应用界面”。

它把 Agent 后端运行过程拆成标准事件流，例如文本增量、工具调用开始、工具调用结果、状态变化、人类确认、取消/中断等，让前端能实时展示 Agent 正在做什么，并在关键节点让用户接管或刹车。

## 2. 原始来源

- 发现入口：小红书笔记标题线索：“Agent还在纯文本聊天？AGUI开源协议来了！3种「刹车」...”。
- 资料本体：GitHub 仓库 `ag-ui-protocol/ag-ui`。
- GitHub：https://github.com/ag-ui-protocol/ag-ui
- 官网/文档：https://ag-ui.com/ ，https://docs.ag-ui.com/introduction
- 相关文章：CopilotKit 发布文《Introducing AG-UI: The Protocol Where Agents Meet Users》。

## 3. 核心观点 / 核心能力

1. **Agent 交互层正在标准化**  
   AG-UI 补的是“Agent ↔ 用户界面”这一层。它和 MCP、A2A 不是同一个层面：MCP 偏 Agent 连接工具/数据，A2A 偏 Agent 之间协作，AG-UI 偏 Agent 面向用户的实时交互。

2. **从聊天回复变成共享工作区**  
   用户不只是等一个最终答案，而是能看到 Agent 的执行过程、工具调用、状态变化，并在中途确认、修改、停止。

3. **“刹车”价值很关键**  
   对企业场景来说，Agent 不能只会自动执行。可取消、可审批、可回退、可人工接管，是从 Demo 走向生产的关键条件。

4. **协议本身偏基础设施，不是单一产品**  
   它的价值在于统一前后端事件契约，减少每个项目都自造一套 WebSocket / SSE / JSON 格式的重复劳动。

## 4. 我学到了什么

这条资料真正有价值的点不是“又来了一个新协议”，而是提醒我们：企业 AI 落地不能长期停留在文本机器人形态。业务用户需要的是可看见、可打断、可确认、可追踪的协作界面。

如果 Agent 只能在聊天框里给建议，它很难进入 CRM、审批、报价、售后、需求处理这类真实业务流程；如果能把执行状态、风险节点、用户确认、结果落库都做成标准交互，Agent 才更像一个可控的业务系统能力。

## 5. 它是否可信，哪些需要验证

可信部分：

- 有公开 GitHub 仓库与文档。
- 有 MIT License。
- 文档中列出 LangGraph、CrewAI、Mastra、Pydantic AI、LlamaIndex、Google ADK、AWS Strands 等集成或支持状态。
- 协议定位清晰：补用户交互层，不和 MCP/A2A 强行抢同一层。

待验证部分：

- 生态支持深度：列出支持不等于企业生产可直接使用。
- 前端复杂度：动态 UI、状态同步、取消/恢复机制会显著增加系统复杂度。
- 和现有 Hermes / 飞书 / CRM 链路的适配成本，需要小实验验证。
- 国内网络、鉴权、审计、日志留存、权限隔离等企业要求需要单独评估。

## 6. 对个人能力有什么价值

- 帮助建立 Agent 协议栈分层意识：MCP 管工具，A2A 管多 Agent 协同，AG-UI 管用户交互。
- 看 AI 应用时不只看模型和 Prompt，而要看“交互契约”和“可控执行”。
- 训练产品化判断：一个 Agent 能不能进入业务，不是看回答多漂亮，而是看它能不能被用户安全地监督、介入和纠偏。

## 7. 对企业 AI 落地有什么价值

对 CRM AI 化、审批辅助、报价助手、RPA/NC 错误闭环这类企业场景，AG-UI 的启发非常直接：

- **CRM AI 化**：用户用自然语言发起操作后，界面应展示解析结果、待写入字段、影响对象、风险提示，并允许确认/修改/取消。
- **审批/流程辅助**：Agent 可先给判断依据和建议动作，但关键节点必须保留人工审批。
- **错误闭环自治**：自动修复前应展示诊断链路、拟执行脚本、影响范围和回滚方式。
- **飞书自动化**：从“机器人在群里回一句话”升级为“群消息触发任务状态卡片 + 人工确认 + 结果追踪”。

## 8. 可做的小实验

建议先做一个最小实验，不急着全量引入协议：

1. 选一个低风险场景：例如 CRM 查询/线索匹配/报价预检查，只读或半自动。
2. 定义 5 类事件：任务开始、解析结果、工具调用、等待确认、任务结束。
3. 在本地或 Web 小页面中用 SSE/JSON 事件流展示过程。
4. 加入 3 种刹车：取消执行、人工确认后继续、出错后回退/转人工。
5. 评估是否值得进一步对齐 AG-UI SDK 或只吸收其事件模型。

## 9. 风险和边界

- 不要把 AG-UI 理解成“装上就有智能 UI”，它只是交互协议，业务规则、权限控制、审计追踪仍要自己设计。
- 企业系统里最重要的是权限、确认、日志和可回滚，不能为了“丝滑体验”牺牲安全边界。
- 如果接入 CRM、飞书或内部系统，公开沉淀只能保留通用方法论，不能暴露真实客户数据、内部字段、群聊原文或未脱敏流程细节。
- 对外公开仓库只记录 sanitized / public 级别摘要，不放内部截图、真实业务样本和私有配置。

## 10. 当前结论

AG-UI 值得继续跟踪，尤其适合作为“企业 Agent 交互层”的参考。它给我们的核心启发是：下一阶段的 Agent 产品不应只是更会聊天，而要具备可视化过程、可人工刹车、可状态同步、可审计追踪的交互结构。

当前判断为 **可小实验**：先吸收事件流和刹车机制，验证其对 CRM AI 化、飞书任务卡片、审批辅助等场景的实际价值，再决定是否引入完整协议或 SDK。
