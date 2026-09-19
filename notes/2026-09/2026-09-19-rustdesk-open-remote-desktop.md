---
title: RustDesk——可自托管的开源跨平台远程桌面（对照参照）
date: 2026-09-19
discovery_source:
  type: 小红书视频线索（误判，真实项目为 BilldDesk）
  title: 一款非常好用的跨平台的远程桌面操控开源项目
  url: https://xhslink.cn/o/AjeLVyrnPcw
primary_object:
  type: open_source_project
  name: RustDesk
  url: https://github.com/rustdesk/rustdesk
object_type: [open_source_project]
source_type: [GitHub, 官网]
business_tags: [ITBP]
problem_tags: [远程运维]
method_tags: [远程桌面, 自托管]
tool_tags: [RustDesk, Rust, Flutter]
value_stage: 学习理解
risk_tags: [数据安全, 权限, 合规]
public_level: public
---

# RustDesk——企业级自托管远程桌面的正经选项

> 勘误说明（2026-09-19）：本篇最初把小红书视频误判为 RustDesk，晶晶姐补截图后确认真实项目是 **BilldDesk Pro**，正式拆解见同目录 `2026-09-19-billddesk-webrtc-remote-desktop.md`。本篇保留为对照参照——评估 BilldDesk 时 RustDesk 正是企业场景的成熟对照组。

## 本体事实（2026-09-19 GitHub 实查）

- 仓库 https://github.com/rustdesk/rustdesk：⭐ 123,983 / fork 19,147 / open issues 164；License AGPL-3.0；Rust + Flutter；2020 年立项。
- 当天仍有提交，维护极其活跃；最新 release 1.4.9（2026-07-06），有 nightly。
- 全平台 Windows/macOS/Linux/iOS/Android；ID + 密码直连，P2P 打洞优先、失败走自建/官方中继（hbbs/hbbr，默认端口 21115-21119）。
- E2EE，可强制公钥校验；文件传输、剪贴板、音频、多显示器、无人值守、TCP 隧道齐全；Server Pro 提供 Web 控制台、设备分组、通讯录、品牌定制。
- AGPL 注意：内部使用无碍，对外 SaaS / 分发衍生版本有开源传染性。

## 与 BilldDesk 的关键差异

| 维度 | RustDesk | BilldDesk |
| --- | --- | --- |
| 成熟度 | 124k stars，有正式 release，活跃社区 | 7.9k stars，**无稳定版，官方明确不建议生产**，单人维护 |
| 开源边界 | 核心全开源（AGPL），Pro 是服务端增值 | 开源版 MIT 但能力受限，**Pro 闭源、源码付费订阅** |
| 主控形态 | 客户端为主 | **浏览器可直接当主控**，零安装 |
| 主打 | 通用远控 / 自托管 / 企业支持 | 高帧率游戏串流 + WebRTC，形态新 |
| 企业可用性 | 可评估自建 + Server Pro | 不满足生产准入 |

## 结论

个人/企业远程支持的可落地方案看 RustDesk；BilldDesk 的价值在浏览器主控和串流形态的启发。网络层坑相同：跨运营商 UDP 封禁时需自建中继并考虑 443 兜底，部署前必须实测 P2P 成功率。
