---
title: 手机变随身 AI 工作站——玄戒 O3 + Termux 本地 Agent 实战（小天fotos）
date: 2026-09-13
discovery_source:
  type: 短视频
  title: 小米玄界O3 + 18 Fold 本地AI Agent 实战：把手机变成随身AI工作站（群内文字稿，原视频在抖音）
  url: https://www.douyin.com/（作者：小天fotos）
primary_object:
  type: 实践者案例 + 趋势信号
  name: 小天fotos 安卓本地 Agent 工作流（Termux + arm Codex + 本地 ASR + ADB 自举开发 APK）
  url: https://github.com/xiaotianfotos
object_type: [case_or_media, trend_signal, open_source_project]
source_type: [群聊线索, 抖音, GitHub, 官网]
business_tags: [ITBP, 个人能力]
problem_tags: [流程提效, 知识沉淀]
method_tags: [Agent, Vibe Coding, 端侧AI, 自动化]
tool_tags: [Termux, codex-termux, llama.cpp, Qwen3-ASR, ADB, DeepCode, 玄戒O3, 小米18Fold]
value_stage: 待验证
risk_tags: [数据安全, 权限, 合规, 夸大宣传]
public_level: public
---

# 手机变随身 AI 工作站：玄戒 O3 + Termux 本地 Agent 实战

## 1. 这是什么

抖音创作者"小天fotos"的一期硬核折腾视频文字稿：在小米 18 Fold（首发玄戒 O3）和小米平板 9 Pro Max 上，用 Termux 搭 Linux 环境、跑 arm 原生 Codex 类 Agent、本地 llama.cpp 跑 Qwen3 ASR 语音输入、PS5 手柄切换/控制多个 Agent session，最后通过 ADB 让 Agent 直接在手机上编译并安装自己开发的 APK（如专注计时器）。作者下周将带设备出差实测。

作者身份已核实：GitHub `xiaotianfotos`（731 followers），代表作 homerail（语音优先本地 Agent 编排）、OPC（一人公司教程）、run-qwen3-omni、indexed；2026-06 发过多 Agent 无人值守 4 天重构 14 万行代码的实战视频（项目 Niuma）。是真实的一线 Agent 工程实践者，不是营销号。

## 2. 原始来源

- 发现入口：打捞处群内文字稿（作者自述"玄界 O3"为听写错误，正确名称是**玄戒 O3**，XRing O3）
- 作者 GitHub：https://github.com/xiaotianfotos
- 原视频：抖音"小天fotos的作品"（未直接抓到；YouTube 同作者频道有其多 Agent 编排视频 https://www.youtube.com/watch?v=aa3foQWgPnk）
- 硬件事实（多源交叉确认）：小米 18 Fold 2026-09-07 发布、10999 元起，首发玄戒 O3，安兔兔 522 万分，十核全大核、G2-Ultra NX GPU、长鑫 LPDDR6、NPU 200 TOPS Tensor / 3.13 TFLOPS Vector（IT之家 https://www.ithome.com/0/999/441.htm、快科技评测）
- arm Codex 社区适配确实存在：DioNanos/codex-termux（活跃，跟随上游里程碑）、wallentx/codex-termux
- 视频中的 "Deepcode" 可能指 HKUDS/DeepCode（开源 agentic coding harness，2026-07 v1.3.0 支持 CLI/Web/headless、Skills、沙箱），也可能是作者自建 harness，待看原视频确认

## 3. 核心观点 / 核心能力

1. **Playground 下沉**：跑 Agent 不一定要 Mac mini/PC，旗舰安卓设备（Termux + proot Debian）已能承载完整 coding agent 工作流——写代码、改文件、剪视频（FFmpeg）、出图。
2. **输入层创新**：本地 ASR（Qwen3，llama.cpp）+ PS5 手柄按键映射（扳机录音、L1/R1 切 session、叉圆确认/删除）= 躺着手柄办公，语音驱动 Agent。
3. **多 Agent 并行**：折叠屏/平板分屏跑多个 session，手柄在最多 4 个 Agent 间切换。
4. **Agent 自举闭环（最有信息量的一点）**：Termux 内 Debian 用户态 + 本机 ADB（无线调试连本机 loopback）→ Agent 能配安卓构建环境、编译 APK、ADB 安装到宿主系统。设备从"运行 Agent"变成"Agent 给自己造工具并装上"。
5. **作者的平台判断**：此场景安卓胜 iOS/iPadOS（侧载、Termux、ADB 开放性决定）；且不绑定玄戒 O3，同档性能安卓机均可。结尾呼吁小米开放玄戒 NPU SDK。

## 4. 我学到了什么

- **本地 Agent ≠ 本地模型**。这是本稿最容易被标题误导的点：本地的是 harness/运行时和 ASR，主力 coding 模型（Codex 接云端 API）和"GPT image2.5 出图"大概率走云端，200 TOPS NPU 在这套 Termux 工作流里基本没被调用——llama.cpp 走的是 CPU/GPU。也就是说，玄戒 O3 的芯片卖点和这次折腾的实际算力来源是两回事，手机价值更多在大内存、LPDDR6 带宽、屏幕形态和安卓开放性。
- **ADB 自举是真实可复制的模式**：安卓无线调试允许设备 ADB 连自身，Termux 装 android-tools 即可获得安装应用、看日志、shell 的能力，这是"手机 Agent 操作手机自身"的最短路径（业界 UI Agent 也多用 ADB/无障碍服务，作者说"点外卖 Agent 都是这个原理"方向对，但过度简化）。
- 折叠屏 √2:1 比例分屏后每半仍是同比例，对"多 session 并排"确有形态合理性，不只是营销。
- 端侧开放度的竞争正在从"芯片算力"转向"NPU/工具链对开发者的开放度"——小米官方澎湃 OS 开发者文档目前没有公开 NPU SDK，作者的呼吁侧面印证端侧 AI 生态卡在工具链而非算力。

## 5. 它是否可信，哪些需要验证

可信（多源证实）：玄戒 O3 全部硬件参数；18 Fold 已上市；Termux 跑 arm Codex（社区 fork 存在且活跃）；Termux+ADB 自控路线有公开教程；作者工程可信度。

待验证 / 存疑：
- "集成 Chrome、Electron"：Electron 官方不支持 Android，proot Debian 内跑桌面栈实用性存疑，疑为口头打包或重度妥协方案。
- "HyperFrames" 具体指什么未核实（可能指 JS 视频动画库），视频全流程"稿子进→成片出"的真实耗时和稳定性未知。
- ASR 提速 1.6× 的具体方法（粉丝给的代码库）未见到。
- 本地 ASR 之外，哪些环节真离线、哪些走云 API，需要原视频/作者 GitHub 仓库核实。
- 连续生产力可信度：编译 APK 的资源占用、发热、续航、Termux 后台保活，作者自己也说出差实测后再更新——以实测为准。

## 6. 对个人能力有什么价值

- 出差/无电脑场景的兜底方案：安卓旗舰 + Termux + codex-termux 即可应急改代码、跑脚本、操作文件，值得作为"知道并能在 1 小时内搭起来"的备用能力，不必提前投入。
- 手柄/语音 + 多 session 切换是个可迁移的交互思路：我们自己的多 Agent 编排（贵模型动脑/便宜模型动手）目前靠桌面端，语音驱动+低成本输入设备在移动端、车间巡检等双手占用场景有想象空间。

## 7. 对企业 AI 落地有什么价值

- 对英科这类制造企业，直接业务价值低：这是个人极客工作流，不是企业 IT 方案，数据安全和 MDM 管控上反而与企业要求冲突（见风险）。
- 间接信号价值：① 端侧设备已具备跑完整 Agent harness 的能力，未来车间/仓储场景"平板+语音+本地小模型+云端大模型混合"是可行形态，选型时安卓开放生态优于封闭系统；② 评估端侧 AI 硬件时，应问"NPU 有没有开放 SDK/能否被 llama.cpp、ONNX Runtime 调用"，而不是只看 TOPS 宣传值；③ 手机 Agent 自举安装应用提示企业 MDM 必须能管控"无线调试/ADB/未知来源安装"，否则这就是一条绕过管控的通道。

## 8. 可做的小实验

- 低成本验证（任意现有安卓机即可）：Termux(F-Droid) + proot-distro Debian + DioNanos/codex-termux，接现有 ark-plan 端点，验证 arm 端 coding agent 可用性；记录安装耗时、发热、典型任务耗时。
- 进阶可选：Termux 装 android-tools，本机 ADB 配对，验证"Agent 装一个自编译 APK"闭环（仅在个人设备上做）。
- 暂不建议投入：本地 ASR + 手柄方案，等作者出差实测反馈。

## 9. 风险和边界

- **数据安全**：Termux/proot 环境绕过安卓沙箱与企业 MDM 管控，若接入公司代码、CRM、飞书数据，属于明确的影子 IT 风险；企业设备应禁用或受控开放无线调试。
- **ADB 自举是双刃剑**：同一能力也是银行木马、黑灰产自动化的技术路径，个人设备开启无线调试后需注意配对授权。
- 续航/发热/后台保活决定它目前是"能 demo"而非"能替代笔记本"，作者本人也未声称已完成替代。
- 标题"本地 AI"有混淆视听成分（云端模型 API 仍是主力），转述时应拆清。
- 平台依赖：Termux 生态对 Android 版本、厂商杀后台策略敏感，不可当稳定生产环境。

## 10. 当前结论

雷达观察，暂不试点。价值不在"玄戒 O3 多强"（NPU 没开放、工作流主要吃 CPU/内存），而在两个被验证的方向：旗舰安卓已能承载完整 Agent harness；ADB 自举让移动设备具备 Agent 自我扩展能力。对我们是端侧形态判断和 MDM 风险提醒，对个人是一个可在出差时 1 小时搭起的应急方案。等小天fotos出差实测视频和其 GitHub 脚本发布后再复核离线/云端边界与真实生产力。
