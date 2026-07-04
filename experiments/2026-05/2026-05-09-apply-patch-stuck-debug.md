---
title: Codex apply_patch 反复卡死调试
date: 2026-05-09
source: 打捞处群 oc_ca0d61c219081373b70a03bcc01f9101
public_level: sanitized
value_stage: 待验证
tags: [codex, apply_patch, debugging, cc-connect]
---

# Codex apply_patch 反复卡死调试

## 1. 实验目标

排查 Codex 在 cc-connect 配置调试过程中 `apply_patch` 工具反复卡死的原因，
确认是否与 cc-connect bridge、Codex 版本（0.129.0 → 0.130.0 升级）或权限边界相关。

## 2. 实验上下文

- Codex 通过 cc-connect（`chenhg5/cc-connect`）作为飞书 ↔ 本地 agent 桥。
- 用户在群中反映「改个这个又半天改不成」，多次 Edit `.cc-connect/config.toml` 反复失败。
- Codex CLI 弹出升级提示 `0.129.0 -> 0.130.0`，但用户拒绝升级。

## 3. 现象

- `apply_patch` 工具调用长时间无返回。
- 编辑 `.cc-connect/config.toml` 时偶发修改未生效。
- Codex 升级提示持续弹出。

## 4. 已尝试

- 升级 Codex → 用户拒绝。
- 反复修改 `allow_from`、`reasoning_effort`、`model` 配置字段。
- 询问 Codex 自身能力边界。

## 5. 当前结论

- 短期止血：禁用 `apply_patch`（在 Codex 配置中关闭该工具）。
- 长期方案：替换工具路径（直接用 `sed` / `python -c` 改文件，绕开 apply_patch）。
- 待验证：升级到 0.130.0 是否解决问题。

## 6. 关联

- 群消息 `om_x100b50d9d5dbe9a0b3b4ffb696abfdd`（cc-connect config.toml Edit）
- 群消息 `om_x100b50c50ed798a4b285e8994dc2bda`（apply_patch 是什么）
- 群消息 `om_x100b50c5e688a4a0b229c5fd421bcba`（Codex 升级提示）
- 群消息 `om_x100b50c597eba4a0b3074582e3303a9`（以后怎么用）

## 7. 下一步

1. 在沙箱中跑一次 0.130.0 升级，验证 apply_patch 是否还卡死。
2. 写一个 `sed-based` 文件修改 wrapper，作为 apply_patch 的替代。
3. 把 cc-connect 的最佳实践沉淀到独立 Playbook。
