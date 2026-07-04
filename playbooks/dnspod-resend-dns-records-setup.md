---
title: DNSPod + Resend 域名 DNS 三连记录配置（SPF / DKIM / Verification）
date: 2026-04-12
source: 打捞处群 oc_ca0d61c219081373b70a03bcc01f9101
public_level: public
value_stage: 可小实验
tags: [dns, resend, dnspod, spf, dkim, edm, agent, transactional-email]
---

# DNSPod + Resend 域名 DNS 三连记录配置（SPF / DKIM / Verification）

## 1. 它是什么

把自有域名（如 `intco.online`）接入 Resend 时，DNS 控制台（国内推荐 DNSPod / 腾讯云 DNSPod）必须配置三条记录，缺一不可：

1. **SPF（TXT）**：声明哪些 IP 可以代发该域名邮件。
2. **DKIM（CNAME / TXT）**：邮件签名密钥，让收件方验证邮件未篡改。
3. **Verification（TXT）**：Resend 平台验证"你确实拥有这个域名"。

## 2. 解决什么问题

- 没有 SPF：被识别为伪造邮件，落进垃圾箱。
- 没有 DKIM：被识别为可疑邮件，部分大厂直接拒收。
- 没有 Verification：Resend 拒绝发送（Domain status: Not verified）。

## 3. 前置条件

- 域名已托管到 DNSPod（或可写记录的 DNS 服务商）。
- 已注册 Resend 账号（[resend.com](https://resend.com)）。
- 域名前缀可达（如 `mail.intco.online`）。

## 4. 操作步骤（DNSPod 视角）

### 步骤 1：在 Resend 添加域名

1. Resend 控制台 → `Domains` → `Add Domain`。
2. 输入域名（如 `intco.online`）。
3. Resend 自动给出 3 条待添加的 DNS 记录（SPF / DKIM / Verification）。

### 步骤 2：在 DNSPod 添加记录

进入 `DNSPod 控制台 → 域名管理 → 记录管理`，添加 3 条：

| 类型 | 主机记录 | 记录类型 | 记录值 | TTL |
|------|----------|----------|--------|-----|
| SPF | `@` | TXT | `v=spf1 include:resend.com ~all` | 600 |
| DKIM | `resend._domainkey`（以 Resend 实际给值为准） | CNAME | `xxxxxxx.resend.com` | 600 |
| Verification | `_resend`（以 Resend 实际给值为准） | TXT | `resend-verification=xxxxxxxxxxxx` | 600 |

> ⚠️ **主机记录必须严格照抄**：Resend 给的是 `resend._domainkey`，不要自己改成 `_dmarc.resend` 之类。

### 步骤 3：回到 Resend 验证

1. 等待 DNS 生效（通常 5-30 分钟，最长 24 小时）。
2. Resend 控制台 → `Verify DNS Records`。
3. 三条全部 ✅ 后，Domain 状态变为 `Verified`。

### 步骤 4：发测试邮件

1. Resend 控制台 → `Send Test Email`。
2. 用任意外网邮箱（Gmail / Outlook / 企业邮箱）接收。
3. 检查三个地方：
   - 收件箱是否收到（不是垃圾箱）。
   - 邮件头里 `SPF=pass`。
   - 邮件头里 `DKIM=pass`。

## 5. 常见坑

| 坑 | 现象 | 解决 |
|----|------|------|
| 主机记录多了个点 | DNS 永远解析不到 | DNSPod 主机记录默认会补 `.`，不要手动加 |
| 记录值没引号包起来 | 解析异常 | DNSPod TXT 记录**不需要**手动加引号，控制台会自动加 |
| Verification 写成了 CNAME | Resend 一直 Not verified | Verification 一定是 TXT |
| DKIM 写成了 TXT | 报错 "record type mismatch" | 必须严格按 Resend 给的记录类型添加 |
| 域名带 www 前缀 | 部分记录会失效 | Resend 的 DNS 配置都是 `@`，**不要**带 `www` |
| 国内 DNS 缓存久 | 明明已生效但 Resend 还显示失败 | 等 30 分钟或换公共 DNS（114.114.114.114 / 8.8.8.8）查询 |

## 6. 验证通过的速查

```bash
# SPF 查询
dig TXT intco.online +short
# 应该看到: "v=spf1 include:resend.com ~all"

# DKIM 查询（替换为你自己的 selector）
dig CNAME resend._domainkey.intco.online +short
# 应该看到: xxxxxxx.resend.com.

# Verification 查询
dig TXT _resend.intco.online +short
# 应该看到: "resend-verification=xxxxxxxxxxxx"
```

## 7. 与 AI Agent / 业务系统集成

DNS 验证通过后，Resend 可作为：

- **AI Agent 发信通道**：用 Resend API Key 让 Hermes / Codex 直接发通知、警告、确认信。
- **CRM 自动化触发**：销售易 → 关键节点 → Resend → 客户经理 / 客户邮箱。
- **EDM 入门工具**：相比 Mailchimp / Brevo，Resend 的 API 文档更友好、对开发者门槛更低。
- **Hermes / OpenClaw 通知通道**：作为飞书之外的备用推送通道。

## 8. 已知风险

- **国内可用性**：Resend 服务端在海外，发送速度取决于发件 IP 和收件方通道。
- **合规**：事务邮件要符合 CAN-SPAM / GDPR / 国内《反垃圾邮件规范》；营销邮件必须留退订。
- **域名风控**：新域名 + 高频发送 + 低打开率 = 进垃圾箱高风险。前 2 周低频"暖域名"。
- **DKIM 轮换**：Resend 偶尔轮换 DKIM key，需要重新添加；订阅 Resend status page。

## 9. 关联

- `notes/2026-04/2026-04-12-resend-agent-friendly-email-api.md`：Resend 与 Mailchimp / Brevo 横向对比。
- 群消息 `om_x100b5285d68b72fcb21d2824dc37532`（Resend DNS 首条线索）
- 群消息 `om_x100b528c0dd848a8b4bf1e4b666fd62`（DNSPod 控制台截图）
- 群消息 `om_x100b528c21d86cb8b3d1799a6333020`（Resend Domain 添加成功回执）

## 10. 公开边界

- 不公开具体业务域名、不公开具体 Resend API Key、不公开客户邮箱。
- 流程通用、可公开复用。
