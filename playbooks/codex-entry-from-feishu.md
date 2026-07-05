---
title: Codex 编程入口选型对比
date: 2026-05-14
source: 打捞处群 oc_ca0d61c219081373b70a03bcc01f9101
public_level: sanitized
value_stage: 待验证
tags: [codex, openclaw, hermes, cc-connect, feishu]
---

# Codex 编程入口选型对比

## 1. 它是什么

从飞书聊天侧触发 Codex 编程任务的多种路径对比，及社区最佳实践梳理。

## 2. 解决什么问题

用户在群中明确诉求：「我要实现从飞书聊天过去写代码」，
但对「由谁启动 Codex」存在分歧——是 Hermes/OpenClaw 中转，还是 cc-connect 直挂，还是 Codex CLI 原生。

## 3. 三种路径

### A. Hermes / OpenClaw 中转

- 入口：飞书群 → Hermes Gateway → Hermes Agent → Codex CLI（`acp_command=codex`）。
- 优点：复用 Hermes 会话管理、模型路由、上下文持久化；与其他任务统一调度。
- 缺点：Hermes 要兼顾「编码 + 杂事」，上下文可能互相干扰。

### B. cc-connect 直挂

- 入口：飞书群 → cc-connect bridge → 本地 Codex CLI。
- 优点：低延迟、配置简单、直连。
- 缺点：apply_patch 反复卡死（见 `experiments/2026-05/2026-05-09-apply-patch-stuck-debug.md`）；版本升级被频繁弹窗打断。

### C. Codex CLI 原生

- 入口：开发者本机直接 `codex` 命令。
- 优点：稳定，无桥接层。
- 缺点：不在飞书群上下文，需要手动搬运提示词和结果。

## 4. 选型建议

| 场景 | 推荐路径 | 理由 |
|------|----------|------|
| 单任务短调试（< 5 分钟） | B / cc-connect | 启动快 |
| 跨任务长上下文 | A / Hermes 中转 | 会话持久化 |
| 个人开发，不需群协作 | C / CLI 原生 | 最稳定 |
| 需要在群里跟客户/同事展示 | A / Hermes 中转 | 上下文可控 |

## 5. 已知痛点

- Codex 升级提示反复弹出，需要 `Skip until next version` 长期压制。
- `apply_patch` 在 B 路径下反复卡死。
- A 路径下 Hermes 与 Codex 上下文是否串线未完全验证。

## 6. 关联

- 群消息 `om_x100b6f7961a9b4a0b399c0b550514ba`（Codex 入口之争长讨论）
- 群消息 `om_x100b6f7faec7fca4b29e64bd6c08c4b`（smoke 报错）
- 群消息 `om_x100b6f79b138c8a0b345b5bfa2ecd22`（Codex 工具链不完整的吐槽）
- `notes/2026-05/2026-05-13-remote-sandbox-vs-codex-sandbox.md` — Codex 沙箱的设计原则；本卡的"沙箱实测 A 路径串线风险"是该卡的延伸验证

## 7. 下一步

1. 在沙箱实测 A 路径的串线风险。
2. 把 B 路径的 apply_patch 问题彻底解决（升级或替换工具）。
3. 评估 D 路径：飞书 → Codex MCP Server（如果 Codex 提供 MCP 接口）。

> ← 属于 [Hermes 三件套](../indexes/hermes-trio-navigation.md) 之「Codex 入口」；同套还有 [自我介绍](../notes/2026-06/2026-06-28-hermes-agent-self-introduction.md) 与 [心跳机制](../notes/2026-06/2026-06-28-hermes-heartbeat-mechanism-finding.md)。
