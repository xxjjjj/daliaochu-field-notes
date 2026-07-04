---
title: Codex Device Code 授权流程 Playbook（OpenAI ChatGPT Team / 个人账号）
date: 2026-06-05
source: 打捞处群 oc_ca0d61c219081373b70a03bcc01f9101 (2026-06-05 16:30 → 16:44)
public_level: sanitized
value_stage: 已验证
tags: [codex, openai, device-code, hermes, openai-codex, refresh-token]
---

# Codex Device Code 授权流程 Playbook

## 1. 它是什么

在 Hermes 里绑定 OpenAI Codex（`openai-codex` provider）的标准流程。

走的是 OpenAI 的 Device Code OAuth 而不是传统 API Key——好处是不用长期保存 access_token，每次调用时自动用 `refresh_token` 换新。适合 ChatGPT Team / 个人 ChatGPT 账号场景，不适合纯 API Key 用户的 OpenAI Platform 账号。

## 2. 解决什么问题

- Hermes 原生支持 `openai-codex` 作为内置 provider，但首次绑定需要走 Device Code 授权。
- 旧 credential 容易被标记为 `exhausted`（token 过期或某次失败），导致后续调用全部失败。
- 用户在飞书群里手动完成授权 → Agent 端轮询拿 token → 自动保存 refresh_token → 后续调用自动刷新。

## 3. 核心配置（已脱敏）

### 3.1 Hermes 内置 provider

```yaml
# ~/.hermes/config.yaml
model:
  default: <按需切换>
  provider: openai-codex  # 切到这个 provider 才走 codex
```

不需要单独的 `api_key` 字段，credential 存在 `~/.hermes/auth.json` 里。

### 3.2 auth.json 关键字段（脱敏）

```json
{
  "openai-codex": {
    "key": "<refresh_token>",
    "label": "Codex (ChatGPT Team)",
    "source": "device-code"
  }
}
```

## 4. 标准流程

### 4.1 首次绑定 / 重置

1. 在飞书群里对 Hermes 说：`我想绑定 codex`
2. Hermes 检测到没有可用 credential → 触发 Device Code 流程：
   - 输出类似：`https://auth.openai.com/codex/device`
   - 输出 6 位设备码：`5HE9-PHPO1`
3. 用户在浏览器打开 URL → 用 crystalxu@intco.com（或其他目标账号）登录 → 输入设备码 → 同意授权
4. 浏览器跳到 `auth.openai.com/deviceauth/callback?code=...&scope=openid+profile+email+offline_access&state=...`
5. 用户把 callback 完整 URL 贴回飞书群（也可以让 Hermes 自己轮询）
6. Hermes 用 code 换 access_token + refresh_token → 写入 `auth.json` 的 `openai-codex` 条目

### 4.2 切换到 codex 调用

```text
/model openai-codex
```

或在 config.yaml 里把 default provider 改成 `openai-codex`。

### 4.3 后续自动刷新

每次调用时 Hermes 自动用 `refresh_token` 拉新 `access_token`，用户不需要任何操作。

## 5. 常见故障

### 5.1 ⚠️ Provider authentication failed: No Codex credentials stored

- **原因 1**：callback URL 没贴回来，或贴的不完整（缺 `code` / `state` / `scope`）
- **原因 2**：credential 被标记为 `exhausted`
- **解决**：
  - 检查 callback URL 是否完整
  - 执行 `hermes auth reset openai-codex` 清掉旧 credential
  - 重新走 4.1 流程

### 5.2 credential 被标记为 exhausted

- **触发条件**：refresh_token 过期，或某次调用被 OpenAI 端拒绝后被拉黑
- **解决**：
  - `hermes auth reset openai-codex`
  - 重新走 Device Code 流程
  - 建议同时检查 `auth.json` 里 `openai-codex.label` 是否和目标账号匹配

### 5.3 callback URL 报"invalid_scope"

- **原因**：scope 必须是 `openid+profile+email+offline_access`，漏掉 `offline_access` 就拿不到 refresh_token
- **解决**：重新发起 Device Code 时确保用户点击"全部同意"，不要手动取消某一项

## 6. 验证清单

- [ ] `hermes auth status --verify` 能看到 `openai-codex` 条目
- [ ] `cat ~/.hermes/auth.json | jq '."openai-codex".label'` 显示账号标识
- [ ] 在 Hermes 里 `/model openai-codex` 后能正常对话
- [ ] `auth.json` 里 `key` 字段是 refresh_token（长字符串），不是 access_token
- [ ] `source` 字段是 `device-code`，不是 `api_key`

## 7. 风险与边界

- **ChatGPT Team 账号和企业 OpenAI Platform 账号是两套体系**：本 Playbook 专指前者；后者请用 `OPENAI_API_KEY` 走标准 `openai` provider。
- **refresh_token 也会过期**：通常 90 天不用会失效，建议保持每 30 天至少调用一次。
- **多个账号切换**：当前 Hermes 不支持一个 provider 多个 credential 并行，切换账号需要 `auth reset` + 重绑。
- **callback 链路暴露风险**：用户在群里贴 callback URL 时，本质上是临时 code，理论上 30 秒内不换 token 就会失效，但仍建议只在自己私群里操作，不要发到公开群。

## 8. 已知替代方案

| 方案 | 适用场景 | 备注 |
|------|----------|------|
| `hermes auth reset openai-codex` | credential 失效时清空重绑 | 不会删 `auth.json` 整体，只清 codex 条目 |
| 走 OpenAI Platform API Key | 有 OpenAI Platform 付费账号时 | 改用 `provider: openai`，加 `api_key: $OPENAI_API_KEY` |
| 走 OpenRouter / 第三方代理 | 不希望直接绑定 ChatGPT 账号时 | 配置 OpenRouter 即可，详见 `playbooks/hermes-default-model-rotation.md` |

## 9. 来源

- 群消息 `om_x100b6d1cda8b9090b1326d85cb92dd1`（2026-06-05 16:31，绑定状态汇报）
- 群消息 `om_x100b6d1cfb6800b4b1d3c7580ec9134`（2026-06-05 16:39，设备码输出）
- 群消息 `om_x100b6d1cf16690b0b1a49543c112f16`（2026-06-05 16:42，callback URL）
- 群消息 `om_x100b6d1c8ee3e0a4b1c2101ce771e14`（2026-06-05 16:42，首次 authentication failed）
- 群消息 `om_x100b6d1c86cf3480b2600870aebbb44`（2026-06-05 16:44，authentication failed 复现）

## 10. 下一步

- 把 `hermes auth reset openai-codex` 命令封装成 Skill，便于在飞书群里一键调用。
- 沉淀"refresh_token 即将过期"自动告警机制，避免突然失效影响线上对话。
- 与 `playbooks/hermes-default-model-rotation.md` 合并为"Hermes 模型 + 认证统一管理"主文档。
