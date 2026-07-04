---
title: IntcoModelHub Provider 配置 Playbook（重命名 + 模型清单 + 网关切换）
date: 2026-06-04
source: 打捞处群 oc_ca0d61c219081373b70a03bcc01f9101 (2026-06-03 22:31, 2026-06-04 08:23)
public_level: sanitized
value_stage: 已验证
tags: [hermes, model-config, intco, intco-gpt, gpt-5.5, intco-proxy, custom-provider]
---

# IntcoModelHub Provider 配置 Playbook

## 1. 它是什么

把 Hermes 的内网代理 provider 从临时命名 `intco-gpt` 重命名为标准化命名 `IntcoModelHub`，并把默认模型从 `gpt-5.5` 切换到 `intco-proxy` 的标准流程。

## 2. 解决什么问题

- 早期临时命名 `intco-gpt` 在 config.yaml / auth.json / 多处引用中不一致，导致文档、运行时、调用日志对不上。
- 需要把 IntcoModelHub 这个内部代理 provider 命名标准化，便于以后追加更多模型时不会撞名。
- 默认模型从 `gpt-5.5` 切到 `intco-proxy`，平衡能力和成本。

## 3. 标准流程

### 3.1 重命名 Provider（涉及 3 处）

```bash
# 1. config.yaml 中 model.provider
sed -i.bak 's/intco-gpt/IntcoModelHub/g' ~/.hermes/config.yaml

# 2. config.yaml 中 fallback_model.provider
sed -i.bak 's/intco-gpt/IntcoModelHub/g' ~/.hermes/config.yaml

# 3. config.yaml 中 custom_providers 列表里 name
sed -i.bak 's/intco-gpt/IntcoModelHub/g' ~/.hermes/config.yaml

# auth.json 同步更新 3 处（key、label、source）
sed -i.bak 's/intco-gpt/IntcoModelHub/g' ~/.hermes/auth.json
```

**注意**：auth.json 的 key 是认证条目标识，重命名后所有引用了这个 provider 的地方都要同步改。

### 3.2 查看 IntcoModelHub 可调用的模型清单

执行 `/model` slash command 或直接读 config.yaml 的 `custom_providers` 段落，可获得当前可调模型清单。当时实际暴露的 4 个模型：

| 模型 ID | 用途 | 推荐场景 |
|---------|------|----------|
| `intco-proxy` | 代理模型（默认） | 日常对话、批量任务 |
| `INTCO-VL-Thinking` | 视觉 + 思考 | 多模态 + 复杂推理 |
| `INTCO-Flash` | 轻量快速 | 短对话、低延迟 |
| `gpt-5.5` | 当前默认模型 | 复杂任务、长上下文 |

### 3.3 切换默认模型到 intco-proxy

两种方式任选：

**方式 A：直接编辑 config.yaml**

```yaml
# ~/.hermes/config.yaml
model:
  default: intco-proxy
  provider: IntcoModelHub
```

**方式 B：用 `/model` slash command**

```
/model IntcoModelHub/intco-proxy
```

### 3.4 重启网关

任何 model/provider 改动后必须重启网关才会生效：

```bash
hermes gateway restart
```

确认重启成功：

```bash
# 应该看到 "Gateway running on PID xxxx"
hermes gateway status
```

## 4. 验证清单

- [ ] `sed -n '1,30p' ~/.hermes/config.yaml` 中 model.provider 显示 `IntcoModelHub`
- [ ] `sed -n '1,30p' ~/.hermes/config.yaml` 中 model.default 显示 `intco-proxy`
- [ ] `grep -c intco-gpt ~/.hermes/config.yaml ~/.hermes/auth.json` 输出 `0`
- [ ] `hermes gateway status` 显示正常运行
- [ ] 在 Hermes 里用 `/status` 看到 default model 是 `intco-proxy`
- [ ] 一次完整对话能成功返回，无 401/403

## 5. 已知坑

### 5.1 session 缓存陷阱

切 default provider 后，**当前 session 可能还跑在旧模型上**，新会话才会走新配置。

> 「当前配置是 intco-proxy（IntcoModelHub），刚切过来的。但当前这个会话是切换之前建立的，可能还跑在旧模型上。新会话会走 intco-proxy。」

解决：用 `/new` 或 `/reset` 强制开新会话。

### 5.2 auth.json 的 key 字段含义

`key` 字段填的是内网代理的 access_token，不是 refresh_token。IntcoModelHub 走的是普通 token 而非 device-code，所以不需要 refresh 流程。

如果哪天 IntcoModelHub 切到 device-code 模式，auth.json 结构会变，要重新走 `playbooks/codex-device-code-auth-flow.md`。

### 5.3 三处同步容易漏

model.provider、fallback_model.provider、custom_providers[1].name + auth.json 三处必须全部改完，否则会出现"配置改了但实际还跑旧 provider"的诡异现象。建议用 `grep` 全文扫一遍。

## 6. 回滚方案

```bash
# 用 .bak 恢复
mv ~/.hermes/config.yaml.bak ~/.hermes/config.yaml
mv ~/.hermes/auth.json.bak ~/.hermes/auth.json
hermes gateway restart
```

或手动改回 `intco-gpt` + `gpt-5.5`。

## 7. 风险与边界

- **Provider 命名一旦定下来，所有引用都要同步改**。建议先用 IntcoModelHub 这种"业务化命名"而不是 `intco-gpt-v3` 这种"版本化命名"。
- **内网代理可用性**直接决定 default 模型是否可达。proxy 宕机时需要回退到 fallback（OpenRouter 或 OpenAI Platform API Key）。
- **批量替换字符串前先做语法检查**：sed `-i.bak` 一定要带 .bak，否则改错了没法回滚。

## 8. 复用条件

- 任何 Hermes 自定义 provider 都可以套用本 Playbook（重命名 + 默认模型切换 + 网关重启）。
- 任何"模型代号改名 / 升级 / 下线"的场景都可以套用本流程。

## 9. 来源

- 群消息 `om_x100b6d37e3e8f4a0b26ab91806`（2026-06-03 22:31，重命名指令）
- 群消息 `om_x100b6d37f81a48a4b206d34dc3`（2026-06-03 22:34，重命名 + 模型清单汇报）
- 群消息 `om_x100b6d38196528acb328af1405`（2026-06-04 08:23，切到 intco-proxy）
- 群消息 `om_x100b6d3816b000a0b1e258c402`（2026-06-04 08:23，切模型 + 网关重启）

## 10. 下一步

- 把"provider 重命名 + 模型清单同步"封装成 Skill，自动扫 config.yaml + auth.json 并给出 diff。
- 与 `playbooks/hermes-default-model-rotation.md` 合并为"Hermes 模型 + 认证统一管理"主文档。
- 把"session 缓存陷阱"加进 `/model` 的提示文案，避免用户重复踩坑。
