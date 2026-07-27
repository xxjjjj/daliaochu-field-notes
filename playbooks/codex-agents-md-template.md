---
title: Codex AGENTS.md 自定义指令最小可用模板
date: 2026-07-26
source_notes:
  - notes/2026-07/2026-07-26-codex-custom-instructions.md
reference_sources:
  - https://github.com/agentsmd/agents.md
  - https://github.com/Anbeeld/AGENTS.md
  - https://github.com/zeke/agents.md
  - https://github.com/github/awesome-copilot
  - https://github.blog/ai-and-ml/github-copilot/how-to-write-a-great-agents-md-lessons-from-over-2500-repositories
public_level: sanitized
value_stage: 已沉淀
risk_tags: [数据安全, 权限, 幻觉, 成本]
---

# Codex AGENTS.md 自定义指令最小可用模板

## 适用场景

用于给 Codex、Claude Code、OpenCode、Cursor、Copilot 等 coding agent 准备项目级或个人全局指令。目标不是写一份很长的 prompt，而是让 Agent 每次开工前知道：这个项目是什么、怎么跑、怎么验证、哪些不能碰、完成标准是什么。

## 使用方式

- 个人全局指令：放到 `~/.codex/AGENTS.md`，写通用偏好和工作边界。
- 项目级指令：放到项目根目录 `AGENTS.md`，写本项目事实、命令、结构和风险。
- 子目录特殊规则：在子目录增加 `AGENTS.md` 或 `AGENTS.override.md`，只写局部差异。
- 不建议把客户数据、token、账号、真实业务截图、生产配置细节写进公开仓库。

## 推荐结构

### 1. 项目定位

```md
# AGENTS.md

## Project Overview

这个项目用于：<一句话说明业务目标/技术目标>。
主要使用者：<业务人员/开发/运维/内部团队>。
当前最重要的质量目标：<稳定性/准确性/交付速度/安全/可维护性>。
```

### 2. 常用命令

把命令写清楚，Agent 才不会乱猜。

```md
## Commands

- Install: `<安装依赖命令>`
- Dev: `<本地启动命令>`
- Test all: `<全量测试命令>`
- Test single: `<单文件/单用例测试命令>`
- Lint: `<lint 命令>`
- Typecheck: `<类型检查命令>`
- Build: `<构建命令>`

优先使用项目已有脚本、lockfile 和本地工具链，不要随意引入第二套包管理器。
```

### 3. 项目结构

```md
## Project Structure

- `src/`: <核心代码>
- `tests/`: <测试>
- `scripts/`: <脚本>
- `docs/`: <文档>
- `config/`: <配置>
- `<特殊目录>`: <说明>

修改前先看同目录已有风格和相邻实现。
```

### 4. 工作方式

```md
## Working Method

- 简单、低风险问题直接处理。
- 复杂任务、架构变化、依赖变化、生产影响、权限变化，先给方案和影响面，再等确认。
- 先读相关文件、调用路径、配置和测试，再改代码。
- 保持最小改动，不做无关重构、不顺手格式化无关文件。
- 如果发现相邻问题，单独指出，不要擅自扩大范围。
```

### 5. 代码标准

```md
## Code Style

- 复用现有抽象、命名、错误处理和目录结构。
- 能删就不加，能复用就不新建。
- 不为一次性需求增加复杂抽象或配置项。
- 新增依赖前先确认项目已有依赖能否满足。
- 遵守当前文件已有风格，即使不是个人偏好的写法。
```

### 6. 验证标准

```md
## Verification

完成前必须运行与改动最相关的验证：

- 文档改动：读回文件，确认链接/格式/示例不明显错误。
- 代码改动：运行相关测试、lint 或 typecheck。
- API/行为变化：覆盖调用路径和回归面。
- 配置/部署变化：先 dry-run、validate、plan 或本地等价验证。

不能运行时，明确说明：哪个命令没跑、为什么没跑、还有什么风险。
不要用“看起来没问题”代替验证结果。
```

### 7. 禁止事项和安全边界

```md
## Boundaries

- 不编造路径、API、配置项、测试结果、版本号和运行结果。
- 不暴露、打印、提交 token、密码、密钥、客户数据、内部截图。
- 不擅自删除数据、改生产配置、部署、推送主分支、force push。
- 不用降低测试、删断言、缩小检查范围来制造通过。
- 不擅自新增生产依赖或改变兼容性。
```

### 8. Git 与交付

```md
## Git & Delivery

- 只有用户明确要求时才 commit / push / open PR。
- 提交前检查 `git diff`，确认只包含本任务相关改动。
- PR/提交信息描述问题和解决方式，不夸大、不营销。
- 禁止 `--no-verify`，除非用户明确批准。
```

### 9. 自我更新规则

```md
## Keeping This File Useful

当本项目出现以下变化时，更新本文件：

- 新增或更换构建、测试、部署命令；
- 发现重复踩坑的项目约束；
- 新增关键目录、脚本、服务、配置来源；
- 用户纠正了 Agent 的错误工作方式。

更新前先检查是否已有相同规则；能改旧规则就不要新增。文件过长时优先合并或删除过期规则。
```

## 个人全局 AGENTS.md 可加的短规则

```md
## Communication

- 直接回答，不谄媚，不复述需求，不写空泛承诺。
- 不确定就查证；查不到就说明缺口。
- 简单问题给结论；复杂问题先给关键判断和下一步。

## Default Engineering Behavior

- 先证据，后改动。
- 小步修改，验证后再说完成。
- 不扩大范围，不擅自引入依赖。
- 不伪造运行结果。
```

## 不建议照搬的内容

- 作者个人技术栈偏好，比如 Cloudflare、Hono、特定包管理器。
- 过强的流程约束，比如每个任务都必须先问、必须多 Agent、必须创建 spec。
- 和当前工具不兼容的文件名或 frontmatter，例如 Copilot 的 `.instructions.md` 不能直接等同于 Codex 的 `AGENTS.md`。
- 任何含内部系统、客户、账号、token、真实截图、生产路径的内容。

## 评估一份 AGENTS.md 好不好

可用 8 个问题检查：

1. Agent 能不能知道这个项目是做什么的？
2. 能不能知道安装、测试、lint、构建命令？
3. 能不能知道应该改哪里、不该改哪里？
4. 能不能知道项目的代码风格和复用原则？
5. 能不能知道什么情况要先问人？
6. 能不能知道完成前如何验证？
7. 有没有明确禁止伪造、泄密、破坏性操作？
8. 文件是否足够短，关键规则是否排在前面？

## 推荐落地路径

1. 先给一个低风险个人仓库写项目级 `AGENTS.md`。
2. 同一小需求分别在有/无 `AGENTS.md` 下跑一次 Codex。
3. 记录返工次数、误改文件、验证执行、输出废话比例。
4. 有效果后，再抽成团队模板。
5. 每月清理一次过期规则，避免记忆污染。
