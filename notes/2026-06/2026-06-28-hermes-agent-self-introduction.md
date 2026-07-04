---
title: Hermes Agent 一句话介绍 + 六层能力拆解（来自打捞处群聊现场回应）
date: 2026-06-28
discovery_source:
  type: 群聊讨论
  title: "介绍一下 hermes agent"
  url:
primary_object:
  type: open_source_project
  name: Hermes Agent
  url: https://github.com/just-every/hermes-agent
object_type: [open_source_project, methodology]
source_type: [群聊线索, GitHub, 飞书]
business_tags: [ITBP, 管理, 个人能力]
problem_tags: [流程提效, 知识沉淀, 组织协同]
method_tags: [Agent, Skill, 自动化, 知识库]
tool_tags: [Hermes, Hermes Agent, run_agent.py, gateway/, tools/, skills/, cron/]
value_stage: 已沉淀
risk_tags: [国内可用性, 成本]
public_level: public
---

# Hermes Agent 一句话介绍 + 六层能力拆解

## 1. 这是什么

打捞处群聊 2026-06-28 用户问"介绍一下 hermes agent"，Hermes 实例从自身代码和工程结构出发，给出一份"自指式"自我介绍。

本卡把这次回应里最稳定的部分（能力拆解 + 一句话定义）抽出来，作为对外解释 Hermes Agent 的标准素材。不是营销文案，而是从代码结构倒推出来的"这个项目到底在做什么"。

## 2. 一句话定义（人话版）

> Hermes Agent 是一个**可本地部署、可接入多平台、可扩展工具和技能**的 AI Agent 框架，用来把大模型变成真正能处理事务、调用系统、连接业务工具的智能助手。

更"村口大白话"版：

> 给大模型装上**工作台、通讯录、电脑、工具箱和记忆本**。普通 ChatGPT 是你问它它答你；Hermes 是让它会"做事"。

## 3. 六层能力拆解

### 3.1 对话与任务执行（"有手有脚"）

不只是聊天，可以在对话里调用工具完成任务：

- 查文件、改文件
- 跑命令、执行脚本
- 搜索网页、提取网页内容
- 操作浏览器（Browserbase 自动化）
- 调用 API（外部系统）
- 生成图片、分析图片（视觉能力）
- 管理待办、定时任务（cron）
- 处理代码、测试、部署、排障

### 3.2 多平台入口（用户在哪都能用）

可接入不同消息平台，用户不一定非要在一个网页里用它，可以直接在工作流里调它：

- CLI（命令行）
- 飞书（Feishu）
- Telegram
- Discord
- Slack
- WhatsApp
- Home Assistant
- Signal、企业微信等

实际例子：在飞书群里 @ 它，它就能根据上下文回复、查资料、执行配置、推送结果。

### 3.3 Skill 技能机制（沉淀复用）

Hermes 有 Skill 系统，可以把复杂能力包装成可复用的技能。典型例子：

- 人话翻译机（`plain-language-concept-translator`）
- 飞书多维表格操作
- PPT 生成
- 小红书调研
- 报价计算
- CRM 操作
- 代码审查
- 系统排障

**Skill 的核心价值**：不是每次都重新教 AI 怎么做，而是把一套成熟流程沉淀下来，下次直接复用。这让 Hermes 从"会聊天的大模型"变成"会一套套工作方法的助手"。

### 3.4 长期记忆与个人化

支持长期记忆，可以记住：

- 用户偏好
- 工作边界
- 常用系统
- 项目背景
- 沟通风格
- 已验证过的坑和经验

所以它不是每次都从零开始，而是能越来越贴近一个人或一个团队的工作方式。

**身份隔离的实践**：可以知道某个实例是给 A 角色服务的管理助理，另一个实例是给 B 角色做 IT/测试/运维支持的，**不能混用身份和语气**。

### 3.5 工具与系统集成

底层设计允许接各种工具和 API：

- 文件工具、终端工具、浏览器工具
- Web 搜索（Parallel + Firecrawl）
- MCP 服务（Model Context Protocol）
- 飞书 OpenAPI
- GitHub
- 邮件、日历、多维表格
- CRM、自定义业务系统

适合做**企业内部的 AI 助手底座**，而不是只做一个固定场景的小 bot。

### 3.6 可部署、可扩展、可工程化

代码结构偏工程化，方便持续扩展：

```
hermes-agent/
├── run_agent.py          # 核心 Agent 对话循环
├── model_tools.py        # 工具发现与调用
├── toolsets.py           # 工具集配置
├── gateway/              # 消息平台网关
├── tools/                # 各种工具实现
├── cron/                 # 定时任务
├── agent/                # prompt、压缩、记忆、模型管理等内部能力
└── skills/               # 技能包体系
```

它不是一次性 prompt 工程，而是可以持续接工具、接平台、接业务系统的"底座"。

## 4. 对 BP / ITBP 的实用价值

- **跨部门协同**：Hermes 不是替代某个业务系统，而是给所有业务系统接一个"会说人话、能动手、能定时"的入口层。
- **AI 落地选型**：当业务部门问"我们能不能自己搞个 AI 助手"时，比起单独采购 SaaS bot，Hermes 路径的优势是**本地部署 + 数据不外流 + 可对接内部系统**。
- **个人能力杠杆**：一个人 + Hermes + Skill 包 ≈ 一个有工作台、记忆本、工具箱的"虚拟助理"，可以把"从看需求到落地"的链路压到 1/3。
- **组织知识沉淀**：Skill + 长期记忆让"团队里几个老员工脑子里的经验"有机会沉淀成可复用资产。

## 5. 它不适合的场景

- **不需要工具调用**：如果只是问答场景，普通 ChatGPT 界面够用，Hermes 反而过度工程。
- **国内强合规 + 数据本地化**：默认模型走海外 API，需要替换为内网代理或国内模型（如 `intco-modelhub-provider-setup.md`）。
- **单点一次性需求**：搭建和运维 Hermes 需要投入，长期收益才划算。

## 6. 可做的小实验

1. **安装 Hermes**：参考 `playbooks/codex-entry-from-feishu.md`，先选一个入口（Hermes 中转 / cc-connect / Codex CLI）。
2. **接入第一个 Skill**：从 `notes/2026-06/2026-06-02-colleague-skill-runtime-design.md` 或 `playbooks/plain-language-translator-patterns.md` 入手。
3. **接入第一个消息平台**：先在飞书里跑通一个小场景（比如自动记会议纪要 → 多维表格）。
4. **写一份自己的"自我介绍 prompt"**：让 Hermes 用你团队的语言习惯解释自己，比通用文案更可信。

## 7. 风险和边界

- **国内可用性**：默认依赖海外模型 API，需要切换到国内 provider 或内网代理。
- **运维成本**：本地部署 + 工具维护 + Skill 维护都有持续投入，不是一次性配置。
- **能力上限**：再工程化，它也受限于底座模型的能力和工具集的覆盖度。
- **身份边界**：多实例共用一套底层时，必须明确每个实例的服务对象、语气边界、可见工具集，否则容易串线和误发。
- **学习曲线**：对纯业务方来说，"工具调用 + Skill + 记忆"这套概念需要至少一次 1on1 培训。

## 8. 公开边界

- 本卡只引用代码层架构与能力描述，不含内部 IM 群组、用户身份、真实业务数据。
- 提到的工具名、平台名、Skill 名均为公开命名空间。

## 9. 相关卡

- `playbooks/codex-entry-from-feishu.md` — Hermes 中转 vs cc-connect vs CLI 入口之争
- `playbooks/intco-modelhub-provider-setup.md` — 国内模型 provider 切换
- `playbooks/hermes-default-model-rotation.md` — 默认模型轮换
- `notes/2026-06/2026-06-02-colleague-skill-runtime-design.md` — Skill Runtime 设计
- `playbooks/plain-language-translator-patterns.md` — 人话翻译机 Skill 实战
