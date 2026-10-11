---
title: "REA (Reverse Engineer Anything)：给编码 Agent 装上逆向工程眼睛的 MCP"
date: 2026-10-11
discovery_source:
  type: bilibili
  title: "比Jev更爆火！逆向灭霸 REA 一天登顶GitHub（B站短视频，BV1RCpE6NETU）"
  url: "https://b23.tv/l0cbxG8"
primary_object:
  type: open_source_project
  name: "morluto/rea"
  url: "https://github.com/morluto/rea"
object_type: [open_source_project, trend_signal]
source_type: [GitHub, 官网, 短视频, YouTube独立复测]
business_tags: [产品, ITBP, 个人能力]
problem_tags: [流程提效, 知识沉淀]
method_tags: [Agent, MCP, Vibe Coding, 逆向工程]
tool_tags: [REA, Claude Code, Codex, Cursor, Ghidra, Hopper]
value_stage: 待验证
risk_tags: [版权, 合规, 数据安全]
public_level: public
---

# REA：让编码 Agent 逆向"任何软件"的 MCP 服务器

## 1. 这是什么

REA（Reverse Engineer Anything）是 morluto 开源的一个 MCP 服务器 + CLI（npm 包 `rea-agents`），
装到 Claude Code / Codex / Cursor / Gemini CLI / Grok Build 等支持本地 MCP 的编码 Agent 后，
Agent 可以对**没有源码的目标**做逆向分析：原生二进制、JS/Electron 应用、.NET 程序集、
网站、Android APK、固件、网络抓包、进程运行时行为等，并返回**带证据**的结论（伪代码、
汇编、字符串、调用关系、脚本原文、摘要 digest），Agent 再基于证据解释功能或仿写实现。

入口是 B 站一个竖屏短视频（标题党，"比 Jev 更爆火""逆向灭霸""一天登顶"），
视频口播内容未逐字核实；但项目本体真实、可查，且热度属实。

## 2. 原始来源

- 发现入口：B 站短视频 https://b23.tv/l0cbxG8 → BV1RCpE6NETU
- 项目仓库：https://github.com/morluto/rea （MIT，TypeScript，约 46MB）
- 官网与实测案例：https://rea.tools （含恐龙游戏、Windows 计算器 % 按钮、DX-Ball、Notion clipboard bridge 等 worked case）
- 独立复测视频：YouTube "The Fastest-Rising Repo on GitHub Is a Little Scary"（OMobTlfjb8s，作者自做隐藏解锁码 Mac app 实测 Agent 能否从编译后二进制恢复规则）
- 安装：`npx rea-agents@latest setup`（Node 22.19+/24.11+/26+）；逆向引擎可选 Hopper（可代装）/Ghidra/IDA，纯 JS/.NET 静态分析不需要引擎

## 3. 核心观点 / 核心能力

- 本质不是"新反编译算法"，而是**把 Ghidra/Hopper/IDA/JADX/binblob/Chrome CDP 等成熟逆向工具
  编排成 Agent 可调用的 MCP 工具集**，加上"调查工作流指令"（agent skill），让模型按
  假设→取证据→验证的路径做逆向，而不是凭印象猜。
- 证据链是它的核心卖点：每条结论附带文件/行号/digest，官网案例还会把恢复出的规则
  回放到受控环境核对（如恐龙游戏 4000/10000 帧后速度 10.0/13.0；计算器 % 对微软开源源码
  的测试用例）。
- 覆盖 15+ 目标类型：原生二进制、ELF layout、EVM bytecode、Linux crash dump、
  JS/Electron、网站（CDP）、HAR/mitmproxy 抓包、.NET、APK（JADX/adb）、JEB、固件
  （binwalk/unblob）、Apple bundle、进程行为（PTY 对比运行）。
- 分析本地运行，不上传目标文件；但工具结果会发给 Agent 背后的模型提供商。

## 4. 热度与成熟度（2026-10-11 实查 API）

- 仓库 2026-04-14 创建，半年内 **90,002 stars / 17.6k forks / 290 watchers**，
  GitHub explore trending 在榜；2,254 commits，当日（10-11）刚发 v6.4.0，
  近一周连续 6.0→6.4 五个版本，迭代极快。
- 开放 issue 165 个；MIT License；有 CI、测试目录、SECURITY.md、多语言 README（含简中）。
- 判断：项目本体真实、工程化程度超出"一夜爆款玩具"，但发布节奏说明仍在快速变动期，
  API/行为可能不稳。

## 5. 它是否可信，哪些需要验证

可信部分：仓库、官网、案例均可交叉验证；案例给了可复现步骤和第三方源码对照；
有独立 YouTuber 做过"自己埋规则→编译→让 REA 恢复"的盲测（成功）。

待验证（对我们）：

- 未在本机实测。JS 静态分析链路最轻（Node 即可），适合先试；原生逆向依赖 Ghidra/Hopper，
  macOS 上 Hopper 商用、Ghidra 免费但吃内存。
- 逆向结论仍由 LLM 解释，复杂二进制存在误读风险，它用"证据+回放验证"缓解但不消除。
- B 站视频本身的具体说法（"比 Jev 爆火"等排名表述）未核实，属引流话术，不作事实引用。

## 6. 对个人能力有什么价值

- 是"Agent + 专业工具链编排"范式的又一个教科书案例：Ghidra 这类工具几十年前就有，
  真正新的是把专家调查流程（提假设、选工具、取证据、交叉验证）变成 Agent skill。
  这与我们自己沉淀 Skill/Playbook 的思路同构——**价值在工作流，不在单点工具**。
- 对理解闭源软件行为、排查第三方桌面客户端问题、分析 Electron 应用 IPC 边界，
  是直接可用的能力增量；安全学习/CTF 方向也有现成路径。

## 7. 对企业 AI 落地有什么价值

- 对英科这类制造企业，直接业务场景有限且敏感：**绝不能用于逆向商业软件（授权协议/法律风险）**，
  仓库自身也声明用户须自行取得授权。
- 合法场景：分析自有/合作方遗留程序（老系统无文档、厂商失联时理解行为做迁移评估）、
  审计采购来的桌面工具是否有可疑外联/数据上传（安全尽调，与市场部网站被植入 JS 的
  巡检思路互补——一个查外网，一个查客户端）、理解 Electron 类 SaaS 客户端的本地行为。
- 更重要的启示：REA 把"闭源不再是护城河"大幅拉低了门槛——我们自己产品/网页上的
  前端逻辑、接口设计被同行复制的成本进一步下降，差异化更要靠数据、供应链、服务而非代码保密。

## 8. 可做的小实验

暂不动手安装（本群线索只做研究，未获明确执行指令）。若晶晶说"试"：

1. 最轻链路：`npx rea-agents@latest setup` 只给 CLI，用官网的 Notes 示例或一个公开
   Electron 应用跑 `analyze-javascript-application --json`，看模块/IPC 证据质量。
2. 接入 Claude Code（本机已有），用"解释这个应用某功能怎么实现的"盲测一个开源小工具，
   再拿真实源码对答案，验证准确率。
3. 暂不碰任何公司在用的商业软件安装包。

## 9. 风险和边界

- 法律/合规：逆向他人商业软件可能违反 EULA 与著作权法，仅限自有软件、授权目标、
  CTF/公开案例；产出不得用于仿制竞品。
- 数据：目标文件本地分析，但分析文本会进入模型供应商（火山/Anthropic 等）通道，
  含客户信息的程序包不能随手丢给 REA。
- 供应链：npx 直装 + setup 会改写各 Agent 的配置（有备份和审批步骤），版本日更，
  接入生产编码环境前应锁版本。
- 爆款副作用：已出现冒名代币诈骗（README 明确否认发币），搜到的"REA 教程/破解版"
  可能是投毒渠道，只认 GitHub 官方仓库与 rea.tools。

## 10. 当前结论

本体真实、热度真实、工程质量在线，是 2026 年"Agent 接管专业工具链"趋势的标志性项目之一。
对我们的主要价值是**认知与安全审计能力**，不是生产工具；建议保持雷达观察，
想动手时先跑 JS 静态链路的公开示例，不接商业软件、不接含数据的内部程序。

证据状态：项目事实（star/版本/License/能力矩阵/案例）均来自 GitHub 仓库、官网与
GitHub API 一手实查（2026-10-11）；B 站视频口播未核实。
