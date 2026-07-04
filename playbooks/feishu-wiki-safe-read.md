---
title: 飞书 Wiki 安全读取 Playbook
date: 2026-05-18
source: 打捞处群 oc_ca0d61c219081373b70a03bcc01f9101
public_level: sanitized
value_stage: 待验证
tags: [feishu, wiki, security, codex-workspaces]
---

# 飞书 Wiki 安全读取 Playbook

## 1. 它是什么

在受控工作目录（如 `codex-workspaces/feishu`）下读取、整理飞书 Wiki 内容的标准工作流，
强调 token / cookie / .env / 私钥 的强制隔离。

## 2. 解决什么问题

- AI Agent 直接读取飞书 Wiki 时可能泄露鉴权信息。
- 工作目录与代码目录混在一起时，敏感文件易被意外 commit。
- 多实例/多 Agent 共享 wiki 链接时缺乏审计边界。

## 3. 强制约束

1. **不读取**：任何 `.env`、`token`、`cookie`、私钥文件。
2. **不输出**：读取 Wiki 时如果附带鉴权信息，必须脱敏。
3. **不复制**：原文不进入工作目录，只写入结构化摘要。
4. **不外发**：wiki 内容不通过 IM 群转给第三方。

## 4. 工作流

1. 在 `codex-workspaces/feishu` 下创建独立子目录，按 wiki 标题命名。
2. 用 `lark-cli wiki nodes get` / `lark-cli docs +fetch` 拉取内容。
3. 解析为 markdown，保留标题层级 + 关键引用，剔除账号 ID、邮箱等 PII。
4. 写入 `notes/YYYY-MM/` 下的结构化笔记，附 `source_wiki_id` 字段。
5. 工作目录加 `.gitignore`，避免误 commit。

## 5. 工具

- `lark-cli wiki nodes get --params '{"space_id":"...","node_id":"..."}'`
- `lark-cli docs +fetch --params '{"document_id":"..."}'`
- 自研 Python 脚本：调用 lark-cli + 解析 + 脱敏 + 落盘。

## 6. 风险与边界

- Wiki 权限变更时需要重新评估可读范围。
- 跨租户 wiki 读取需要额外授权。
- 工作目录隔离不彻底时仍可能误 commit，需 `.gitignore` + pre-commit hook 双保险。

## 7. 复用条件

- 任何需要把飞书 Wiki 内容沉淀为笔记的场景。
- 多 Agent 协作读取同一 Wiki 时的边界划分。

## 8. 关联

- 群消息 `om_x100b6f9d0c167508b122018ffde6908`（2026-05-18 安全读取指令）

## 9. 下一步

1. 在 `codex-workspaces/feishu/.gitignore` 模板中固化。
2. 写一个 pre-commit hook 检测敏感字段。
3. 与 `playbooks/lark-cli-usage.md`（如存在）合并。
