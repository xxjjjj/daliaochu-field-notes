---
title: 人话翻译机 Skill 跨平台发布包设计：core + adapters 三段式
date: 2026-06-27
discovery_source:
  type: 群聊讨论
  title: "人话翻译机这个 skill 我记得让你整过发布包是吧"
  url:
primary_object:
  type: methodology
  name: 人话翻译机 Skill 跨平台发布包结构
  url:
object_type: [methodology, case_or_media]
source_type: [群聊线索, 飞书]
business_tags: [ITBP, 个人能力]
problem_tags: [知识沉淀, 流程提效]
method_tags: [Skill, 跨平台, 发布, 适配器模式, 可复用]
tool_tags: [plain-language-concept-translator, Hermes, OpenClaw, Codex]
value_stage: 学习理解
risk_tags: [维护风险]
public_level: sanitized
---

# 人话翻译机 Skill 跨平台发布包设计：core + adapters 三段式

## 1. 这是什么

打捞处群聊 2026-06-27 用户问"人话翻译机这个 skill 我记得让你整过发布包是吧"，Agent 确认曾整理过一份**可跨平台分发**的发布包，而不是只给 Hermes 用的单点 Skill。

本卡沉淀这个**Skill 发布包结构设计**，作为后续其他 Skill（飞书多维表格、PPT 生成、报价计算等）跨平台发布的参考模板。

## 2. 原始来源

- 飞书群 message_id：`om_x100b6cc3203a40bcb2b061f654bc543`（2026-06-27 15:08，需求提问）
- 飞书群 message_id：`om_x100b6cc33e1b3ca8c05d7a168643152`（2026-06-27 15:09，结构回忆）
- 飞书群 message_id：`om_x100b6cc33c9520a8b1a76a84a6ed3c1`（2026-06-27 15:09，上传发布包）
- Skill 内部名：`plain-language-concept-translator`
- 发布包路径（已脱敏）：`~/.hermes/skill-packages/human-language-translator.zip`

## 3. 发布包结构

```
human-language-translator/
├── core/                   # 平台无关的核心翻译逻辑
│   ├── SKILL.md            # Skill 说明书（一句话定义 / 用法 / 场景）
│   ├── templates/          # 翻译模板（人话版 / 类比版 / 故事版）
│   └── references/         # 参考资料（术语表 / 案例库 / 失败教训）
├── adapters/               # 各 Agent 平台适配层
│   ├── hermes/             # Hermes Agent 适配（路径、命令、权限）
│   ├── openclaw/           # OpenClaw 适配
│   └── codex/              # Codex CLI 适配
├── manifest.json           # 元数据（名称、版本、作者、依赖）
└── README.md               # 安装说明 + 平台切换指南
```

## 4. 核心设计原则

### 4.1 core / adapters 分离

- **core 放平台无关的人话翻译逻辑**：翻译模板、术语表、类比库、失败教训。这些内容 99% 不依赖平台。
- **adapters 放 Hermes / OpenClaw / Codex 的适配说明或配置**：包括安装路径、调用命令、权限声明、平台特有参数。

**好处**：当 core 升级（如新增"翻译模式 v3"）时，不需要改 adapters；当新增一个 Agent 平台时，不需要改 core，只要新加一个 adapter 目录。

### 4.2 manifest.json 标准元数据

建议 manifest.json 至少包含：

```json
{
  "name": "plain-language-concept-translator",
  "display_name": "人话翻译机",
  "version": "1.0.0",
  "author": "Hermes 自维护",
  "license": "MIT",
  "description": "把抽象、黑话化、不说人话的概念转译成人能听懂、记得住、能复述的人话版解释",
  "tags": ["translation", "explanation", "plain-language", "education"],
  "core_version": "1.0.0",
  "adapters": {
    "hermes": "^1.0.0",
    "openclaw": "^0.5.0",
    "codex": "^1.2.0"
  },
  "entry_point": "core/SKILL.md",
  "platform_specific_paths": {
    "hermes": "~/.hermes/skills/productivity/plain-language-concept-translator/"
  }
}
```

### 4.3 SKILL.md 作为入口契约

不管在哪个平台，Skill 的入口契约都是 SKILL.md，至少包含：

1. **一句话定义**：这个 Skill 是干嘛的
2. **触发条件**：用户说什么、传什么参数时会调用
3. **输入约定**：传文本 / 传术语 / 传受众画像
4. **输出结构**：翻译模板的几段（人话版 / 类比 / 现实场景 / 误区）
5. **失败处理**：术语太新 / 找不到类比 / 受众过于专业时怎么办

## 5. 为什么是"发布包"而不是"单文件 Skill"

| 维度 | 单文件 Skill | 跨平台发布包 |
|------|--------------|--------------|
| 跨平台复用 | ❌ 要重写 | ✅ 一次打包，多平台用 |
| 维护成本 | ❌ 每个平台一份 | ✅ core 改一次，所有平台受益 |
| 新平台接入 | ❌ 重新设计结构 | ✅ 加一个 adapter 目录 |
| 版本管理 | ⚠️ 散落在各平台 | ✅ 统一 manifest.json |
| 团队协作 | ⚠️ 改一处忘了改另一处 | ✅ PR 提一个 adapter 即可 |

**关键判断**：当一个 Skill 在 2 个以上平台使用时，就值得做成"core + adapters"发布包。

## 6. 适配器层该放什么

以 Hermes adapter 为例，需要包含：

```
adapters/hermes/
├── install.md              # 安装步骤（cp -r 到 ~/.hermes/skills/...）
├── invoke.md               # 调用方式（自动触发 / 手动触发）
├── permissions.md          # 需要的权限（读写文件 / 调网络 / 调其他 Skill）
├── hooks.md                # 和其他 Skill 的协同（如先调用 llm-wiki-skill 取资料）
└── examples.md             # 真实使用样例
```

OpenClaw 和 Codex adapter 结构类似，但具体命令和路径不同。这样**新人接手某个平台时，看 adapter 就能上手**，不用回头读 core 全部逻辑。

## 7. 复用价值：其他 Skill 怎么抄这个结构

适合用同样结构做的 Skill 类型：

- **跨平台工具型 Skill**：飞书多维表格操作、邮件发送、日历管理 → 必须 core + adapters
- **方法论沉淀型 Skill**：GEO 翻译、TDD 翻译、Loop Engineering 翻译 → adapters 主要是"触发方式 + 输出格式"
- **业务专属 Skill**：报价计算、CRM 操作、行业报告生成 → adapters 可能要加权限声明、合规边界

**不适合**用同样结构的：

- 单平台一次性脚本（用完即弃）
- 强依赖某个平台特有 API 的 Skill（没必要抽象）

## 8. 对 BP / ITBP 的实用价值

- **跨团队复用**：一个 Skill 在 A 团队的 Hermes 上能用，到 B 团队换 OpenClaw 也能用，不用重写
- **降低"换 Agent 框架"的迁移成本**：当组织决定从 OpenClaw 迁到 Hermes 时，已有的 core 直接复用，只换 adapter
- **Skill 治理**：manifest.json 让 Skill 有版本、可审计、可降级，避免"团队里某天突然 Skill 行为变了"
- **培训新人**：新人按 README 一步步操作，比"跟着老员工口口相传"靠谱

## 9. 可做的小实验

1. **结构验证**：拿一个现有 Skill（哪怕只有一个平台在用），按 core + adapters 重构一遍，看是否真的解耦
2. **adapter 模板化**：写一份通用的 adapter 模板（install.md / invoke.md / permissions.md），让团队其他人套用
3. **跨平台真跑**：把同一个 Skill 同时部署到 Hermes + Codex，看 SKILL.md 是否真的"一次定义多平台生效"
4. **版本回滚演练**：故意在 core 改一个有 bug 的翻译模板，演练怎么回滚到上一版

## 10. 风险和边界

- **过度设计**：如果 Skill 只在一个平台用，做成发布包是过度工程，单文件更简单
- **adapter 维护成本**：当平台 API 大改时，adapter 全部要重写，需要评估值不值
- **manifest 漂移**：当 adapters 各自加新依赖时，manifest.json 容易过时，需要 CI 检查
- **License 边界**：core 用 MIT，但 adapter 可能继承平台协议，要注意兼容性

## 11. 公开边界

- 涉及的具体路径为本机私域配置，不暴露绝对路径细节。
- Skill 名称 / 适配器命名按公开命名空间记录。
- 不含内部业务数据、用户身份、真实调用记录。

## 12. 相关卡

- `playbooks/plain-language-translator-patterns.md` — 人话翻译机实战模式
- `notes/2026-06/2026-06-02-colleague-skill-runtime-design.md` — Skill Runtime 产品定位
- `notes/2026-06/2026-06-02-openskills-colleague-skill.md` — OpenSkills + Colleague.skill 数字分身
