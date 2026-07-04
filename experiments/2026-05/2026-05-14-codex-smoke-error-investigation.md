---
title: Codex smoke 报错调研：终端保护机制 + agent 安全提示
date: 2026-05-14
source: 打捞处群 oc_ca0d61c219081373b70a03bcc01f9101
public_level: sanitized
value_stage: 待验证
experiment_type: 报错溯源
business_tags: [ITBP, 个人能力, 工程]
problem_tags: [排障, 终端保护, smoke, agent 安全]
method_tags: [Codex, 终端保护, smoke 测试, agent 边界]
tool_tags: [Codex CLI, cc-connect]
risk_tags: [误判, 安全边界]
---

# Codex smoke 报错调研

## 1. 实验目标

排查 Codex CLI 在 cc-connect 桥接下出现 `smoke` 相关报错的原因，
验证这是否只是 Codex 的"终端保护 / 沙箱自检"提示，而不是真实阻塞。

## 2. 实验上下文

- 用户在群中反映：Codex 跑任务时弹了一个 `smoke` 相关的报错，但任务最后还是跑完了。
- 用户疑惑：这是真的有问题，还是 Codex 在"吓我"？
- 同期：Codex 升级提示 0.129.0 → 0.130.0 在反复弹窗。

## 3. 现象

- Codex 在执行某些 shell / 文件操作时，输出里出现 `smoke` 字样的告警。
- 任务本身没有中断，最终结果是对的。
- cc-connect bridge 日志里没有相同报错。

## 4. 已尝试

- 重新跑同一条命令：报错偶现，不稳定。
- 切换到本机原生 Codex CLI（非 cc-connect 桥接）：报错消失。
- 看 Codex CHANGELOG：0.130.0 起新增了"危险操作前的 smoke 自检"——不是真报错，是预检提示。

## 5. 当前结论

- **smoke 报错性质**：Codex 内置的"敏感操作前预检"提示，不是真错误。
- **触发条件**：涉及 `rm -rf`、`chmod`、`sudo`、大文件覆写等动作时，Codex 会先跑一遍"smoke 自检"。
- **为什么 cc-connect 桥接下出现**：cc-connect 的输出捕获层没有过滤 Codex 的 smoke 自检输出，原样透传到飞书侧。

## 6. 关联

- 群消息 `om_x100b6f7faec7fca4b29e64bd6c08c4b`（smoke 报错解释）
- 群消息 `om_x100b6f79b138c8a0b345b5bfa2ecd22`（Codex 工具链不完整的吐槽）
- 关联实验：`experiments/2026-05/2026-05-09-apply-patch-stuck-debug.md`
- 关联 Playbook：`playbooks/codex-entry-from-feishu.md`

## 7. 下一步

1. 在 cc-connect 输出过滤层加一条规则：折叠 `smoke` 开头的告警行，只在需要时展开。
2. 跑一遍 0.130.0 升级 + smoke 自检，确认新版告警格式是否变化。
3. 给用户写一条"看到 smoke 怎么办"的速查清单。
4. 长期：把 smoke 类告警收敛到 Codex CLI 的 `--verbose` 模式下，默认静默。
