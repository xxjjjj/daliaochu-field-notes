---
title: lark-cli 能力对标（本地 1.0.4 vs 官方最新 1.0.95 / 飞书8.0 CLI）
date: 2026-09-16
discovery_source:
  type: 群聊线索（打捞处，飞书8.0发布会后续对标）
  title: 飞书8.0 CLI 767 功能点与我方能力 diff
  url: "https://github.com/larksuite/cli"
primary_object:
  type: open_source_project
  name: larksuite/cli（lark-cli，MIT，Go）
  url: "https://github.com/larksuite/cli"
object_type: [open_source_project, commercial_product]
source_type: [GitHub, 官网, 本地实测]
business_tags: [ITBP]
problem_tags: [流程提效, 组织协同]
method_tags: [Agent, CLI, 自动化]
tool_tags: [lark-cli, 飞书, J-Herm]
value_stage: 待验证
risk_tags: [权限, 版本兼容]
public_level: public
---

# lark-cli 能力对标：本地版本 vs 官方最新

## 1. 怎么拉的

- 本地实机：`lark-cli version 1.0.4`（路径 `/Users/crystalxu/.openclaw/tools/node-v22.22.0/bin/lark-cli`，OpenClaw 自带）
- npm 最新：`@larksuite/cli@1.0.95`（2026-09-16 查询）——**本地落后约 90 个版本**
- 官方 README（main 分支）口径：18 大业务域、200+ 精选快捷命令、26 个 AI Agent Skills、Raw API 覆盖 2500+ 端点、MIT 协议、GitHub 17.1k star / 1.4k fork
- 发布会"767 功能点"口径 = CLI 覆盖的飞书功能点数（非命令数）；200+ 是精选 shortcut 数，两者不矛盾

## 2. 本地 1.0.4 实测命令面（14 个业务域，134 个 +shortcut）

| 域 | shortcut 数 | 主要能力 |
|---|---|---|
| base 多维表格 | 68 | 表/字段/记录/视图/仪表盘/表单/角色权限/workflow，最全的一个域 |
| task 任务 | 12 | 建/派/完成/重开/评论/提醒/清单 |
| mail 邮箱 | 11 | 收发/回复/转发/草稿/triage/watch |
| im 消息 | 10 | 发/回/搜/群管理/话题消息/资源下载 |
| drive 云盘 | 8 | 上传下载/导出导入/评论/移动 |
| docs 文档 | 7 | 建读写搜/媒体/白板 |
| sheets 电子表格 | 7 | 读写追加查找导出 |
| calendar 日历 | 5 | agenda/create/freebusy/rsvp/suggestion |
| contact 通讯录 | 2 | 搜人/用户详情 |
| vc 视频会议 | 2 | 搜会议/取妙记 |
| event 事件订阅 | 1 | subscribe（WebSocket 实时推送） |
| minutes 妙记 | 1 | download |
| approval 审批 | 0 shortcut | 仅 raw：instances / tasks |
| wiki 知识库 | 0 shortcut | 仅 raw：spaces / nodes |

另：任意需求可走 `lark-cli api METHOD /open-apis/...` 通用调用 + `schema` 自省。

## 3. 官方最新版比本地多的域

| 缺口 | 能力 | 对我们的价值 |
|---|---|---|
| 审批 shortcut | 查审批任务、同意/拒绝/转交、撤回/抄送实例 | 高：实施组答疑里审批流问题不少，目前只能裸调 raw API |
| OKR | 目标/KR/对齐/进展读写 | 中：管理场景，短期不用 |
| 考勤打卡 | 查个人打卡记录 | 低：权限敏感，不建议接 |
| 幻灯片 | 建/读/改 PPT、增删页面 | 中：做材料可试 |
| 原生 Markdown | Drive 内 `.md` 文件创建/读取/局部 patch | 高：打捞处笔记、飞书文档双向同步正好用得上 |
| 妙搭应用（Spark/应用域） | 建妙搭应用、发静态站点、云端生成迭代、可用范围管理 | 观察：低代码 + Agent 建站方向 |
| 飞书项目 | 不在主包，独立 `meegle-cli`（github.com/larksuite/meegle-cli） | 中：8.0 刚宣布飞书项目开放 CLI，单独装 |
| Skills 数量 | 本地时期 19 个 → 现 26 个 | 升级后直接获得新场景 playbook |

## 4. 我们当前真实使用面（J-Herm）

高频：im（发群消息/搜消息/读群历史）、contact（查 open_id）、vc+minutes（找会议链接/妙记）、docs/drive（读文档）。
受限事实：lark-cli 应用 `cli_a95b4e05a0b9dbd9` **不在"软件实施一组"群**（230002）；日历/妙记等 user-only 能力依赖 user token，**当前 user token 已失效**（auth status：Token does not exist or has been cleared，只剩 bot 身份）——这比版本旧更影响实际能力。

## 5. 结论与下一步

1. **版本差距真实存在但不紧急**：我们高频的 im/contact/vc/docs 在 1.0.4 已可用；新增价值最大的是**审批 shortcut、原生 Markdown 同步、幻灯片**三块。
2. **不要原地升级 OpenClaw 自带的这份**（`~/.openclaw/tools/...`，可能被 OpenClaw 更新覆盖或牵连）；若要试，在 Hermes 自己的环境用 `npx @larksuite/cli@latest install` 装独立一份，双跑验证后再切换。
3. **先修 token 再谈升级**：需要晶晶重新走一次 `lark-cli auth login --recommend`（device flow，发链接给她点），user 身份恢复后日历/妙记/跨群搜索才通。
4. 飞书项目走独立 meegle-cli，等业务上真要用飞书项目数据时再评估。
5. 发布会"95% 成功率"仍无法从本地证实；升级后可拿我们现有高频命令做一轮回归对比。

## 6. 原始来源

- 仓库：https://github.com/larksuite/cli（README.zh.md，18 域/200+ 命令/26 Skills）
- 企业嵌入文档：https://open.feishu.cn/document/mcp_open_tools/feishu-cli/embed-feishu-cli-in-agent
- 本机实测：`lark-cli --help` 及各域 help（2026-09-16）
