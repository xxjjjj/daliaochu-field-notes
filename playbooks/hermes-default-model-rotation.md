---
title: Hermes 默认模型切换 Playbook（OpenAI / 内网代理）
date: 2026-04-27
source: 打捞处群 oc_ca0d61c219081373b70a03bcc01f9101 (2026-04-12, 2026-04-27)
public_level: sanitized
value_stage: 已验证
tags: [hermes, model-config, openai, gpt-5.4]
---

# Hermes 默认模型切换 Playbook（OpenAI / 内网代理）

## 1. 它是什么

Hermes 实例在 `~/.hermes/config.yaml` 中切换默认模型的标准化流程。
本 Playbook 专指走「OpenAI 兼容 + 内网 proxy + GPT-5.4」路径的切换。

## 2. 解决什么问题

- Hermes 多模型并存时，default 模型切换没有统一脚本路径。
- 内网 proxy 与公网 OpenAI 端点的格式兼容问题。
- API key 通过环境变量注入，避免硬编码。

## 3. 核心配置（已脱敏）

```yaml
# ~/.hermes/config.yaml
model:
  default: gpt-5.4
  provider: openai
  base_url: http://<INTERNAL_PROXY_HOST>:<PORT>/v1
  api_key: ${INTCO_GPT54_API_KEY}
  context_length: 128000

providers:
  openrouter:
    api_key: ${OPENROUTER_API_KEY}
```

## 4. 切换步骤

1. 修改 `~/.hermes/config.yaml` 中 `model.default`。
2. 确认 `providers.openai.api_key` 环境变量已注入。
3. 重启 Hermes 实例。
4. 用 `/model` slash command 验证 default 模型显示。
5. 用 `/status` 确认 API 连通性。

## 5. 验证

- `sed -n '1,12p' ~/.hermes/config.yaml` 输出与目标一致。
- Hermes CLI 启动后 spinner 提示 default 为 `gpt-5.4`。
- 一次对话成功返回，无 401/403。

## 6. 风险与边界

- 内网 proxy 可用性直接决定 default 模型是否可达：proxy 宕机时回退到 fallback 模型。
- `${INTCO_GPT54_API_KEY}` 环境变量命名需与 `.env` 一致，命名差异会导致 silent 401。
- `context_length: 128000` 与实际模型窗口对齐；超出窗口会被自动压缩，可能影响上下文连续性。

## 7. 复用条件

- Hermes 任意实例（CLI / Gateway / ACP）适用。
- 任何使用 OpenAI 兼容接口的模型（含第三方代理、自部署 vLLM）可参考同结构。

## 8. 已知替代方案

- 通过 OpenRouter 走公网：去掉 `base_url`，保留 `providers.openrouter`。
- 本地 llama.cpp / vLLM：使用 OpenAI 兼容 server 模式即可。

## 9. 来源

- 群消息 `om_x100b528fec238ca0b2469da72480d12`（2026-04-12 配置首条）
- 群消息 `om_x100b51c238250138b2b845aaf12179b`（2026-04-27 验证截图）

## 10. 下一步

- 把此 Playbook 与 `~/.hermes/.env` 中 `INTCO_GPT54_API_KEY` 命名对齐。
- 若 proxy 经常抖动，沉淀「fallback 链路」子 Playbook。
