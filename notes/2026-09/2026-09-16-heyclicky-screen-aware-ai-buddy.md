---
title: "HeyClicky：光标旁的屏幕感知语音 AI（消费级 computer-use 样本）"
date: 2026-09-16
discovery_source:
  type: 短视频
  title: 2026年值得关注的AI输入层工具heyclicky（抖音）
  url: https://v.douyin.com/MDwRyzvNeqc/
primary_object:
  type: commercial_product
  name: HeyClicky
  url: https://www.heyclicky.com
object_type: [commercial_product, open_source_project, trend_signal]
source_type: [抖音, 官网, GitHub]
business_tags: [ITBP, 个人能力, 市场]
problem_tags: [流程提效, 知识沉淀]
method_tags: [Agent, computer-use, 语音交互, 屏幕感知]
tool_tags: [HeyClicky, GPT-Realtime, Codex, macOS]
value_stage: 待验证
risk_tags: [数据安全, 国内可用性, 成本, 合规]
public_level: public
---

# HeyClicky：光标旁的屏幕感知语音 AI

## 1. 这是什么

Mac 常驻 AI 助手（menu bar / 刘海处的"小动物"），按热键才读取当前屏幕，
用语音回答、在屏幕上画指引箭头，并可语音唤起后台 agent 代做任务
（研究、操作 Google Workspace、操作 Apple Notes/日历/提醒事项、甚至构建 Mac 小程序）。

创始人 Farza Majeed（buildspace 创始人），公司入列 YC 2026 春季批次。
2026 年 4 月首发，5 月 30 日一条 104 秒全语音操控 Mac 的 demo 播放量近 300 万，
OpenAI Greg Brockman 转评"real magic"。官网自称 25,000+ 用户。

**定位纠偏（相对抖音原视频）**：官方定位是"an AI buddy on your mac /
下一代 AI 界面"，不是输入法。听写（dictation）只是其中一个模块。
博主拿它和豆包输入法、千文输入法等做"输入层"对比，品类上有错位——
它真正对标的是"chat box / terminal / workflow builder 之外的第四种 AI 入口"。

## 2. 原始来源

- 发现入口：抖音视频 https://v.douyin.com/MDwRyzvNeqc/ （二手解读，含个人体验对比）
- 官网：https://www.heyclicky.com
- 更新日志（事实最密集）：https://www.heyclicky.com/changelog
- 开源旧版（MIT，Swift，停在 2026-04-27 前代码）：https://github.com/farzaa/clicky
- 闭源新版发行通道：https://github.com/farzaa/clicky-releases
- 技术栈（据开源版）：Swift 客户端 + Claude API + AssemblyAI 实时转写 +
  ElevenLabs TTS + Cloudflare Worker 代理；5 月 demo 基于 GPT-Realtime 2.0；
  agent 的 computer-use 早期与 Cua 合作，1.0.48 换成自研原生驱动

## 3. 核心观点 / 核心能力

- **热键触发的屏幕感知**：不持续看屏，只在按键时截屏；语音提问，
  语音回答，并在屏幕元素上画箭头/光标做"手把手"教学（典型场景：学 DaVinci、Figma）。
- **两种交互深度**：talk（当场问答/指引）与 agent（后台真的去做，限额计费）。
- **听写**：任意输入框语音转文字插入，已适配微信/企微/Chromium/Electron 系应用，
  热键可自定义（支持双击 Ctrl 这类组合），中文尾部空格等问题专门修过。
- **后台 agent**：意图自动识别，不必说固定唤醒词；可操作浏览器
  （"研究 SSD 然后加购到 Amazon"）、Gmail/Drive/Sheets/Calendar、
  Apple 原生应用；computer-use 用独立 overlay 光标，**禁止移动用户真实指针**，
  后台点击限定目标窗口；长任务有 5 秒取消窗、"始终批准"开关、失败重试。
- **模型路由**：简单问题走快模型、复杂问题走前沿模型，用户无感；
  1.0.47 起支持整份 PDF/网页/文件作为上下文。
- **团队版 v0**（1.0.44）：混合席位订阅、团队私有 skills 共享，人工开通；
  已适配 IT 托管 Codex 策略的公司 Mac。

## 4. 我学到了什么

- **"同样的前沿模型，界面决定释放程度"**——Farza 的核心论点。模型同质化后，
  竞争点移到入口形态：光标旁、看得见屏幕、语音直达，比"截图发 ChatGPT"少三步。
- **隐私是屏幕感知产品的生死线，且用户会用脚投票**：上线初做过 proactive agents
  （持续追踪 app 名/标签页/辅助功能数据的本地活动库），用户表达不适后，
  1.0.46 整个功能下线、本地数据库自删，官方原话"bring it back when it feels
  magical without feeling watched"。这是"主动感知→信任崩塌→回退到显式触发"
  的完整真实案例，比任何隐私设计理论都直观。
- **computer-use 的安全护栏产品化细节值得抄**：独立 overlay 光标、不动真实指针、
  点击限定目标窗口、动作前取消窗、托管设备策略适配。
- **changelog 即增长素材**：每个修复带"上月多少用户受影响"，署名上报用户，
  高频透明更新是早期产品建立信任的标准打法。
- 开源策略：用 MIT 旧代码库做传播和信任（"抓屏路径可自己读"），新能力闭源商业化，
  是个人开发者转型创业公司的典型路径。

## 5. 它是否可信，哪些需要验证

可信（官网/仓库/changelog 交叉确认）：公司与创始人背景、Mac-only、
免费+付费定价、热键截屏机制、agent 能力边界、开源范围。

需要验证：

- 抖音所说"听写 ~450ms"在官网未找到出处，疑为博主自测或转述，待实测。
- 博主弃用豆包/千文等输入法的结论是个人体验，且品类对比错位，不构成通用结论。
- 视频称"正式上线"，实际仍早期：官方自承 computer-use "sometimes it breaks"，
  changelog 高频修稳定性（麦克风失效、高空闲 CPU 45%、蓝牙音频截断等）。
- "6.3K stars"指开源旧仓库热度，不代表闭源新版的工程成熟度。

## 6. 对个人能力有什么价值

- 新交互范式的一手体验样本：屏幕感知 + 语音 + 就地指引，值得亲自用免费版感受
  "不用切换窗口的 AI"与 chat 框的体感差。
- 对我们自建任何 computer-use / RPA 智能体，其护栏设计（第 4 节）是现成参照。

## 7. 对企业 AI 落地有什么价值

- **短期不建议公司设备直接铺开**：屏幕内容（含 CRM、客户数据、邮件）要出域到
  第三方模型链路（OpenAI/AssemblyAI/ElevenLabs/Cloudflare）；官方承诺仅热键触发、
  不存截图，但保留文本摘要，企业侧没有私有部署/审计承诺，团队版也还是人工开通的 v0。
- 市场部内容岗位的听写+研究 agent 是最贴近的个人提效场景，但应先过安全评估，
  且只能用于非敏感内容。
- 对我方"AI 入口"规划的启示：业务系统里的助手不必都做成聊天框——
  就地感知当前页面 + 显式触发 + 语音，是销售/车间场景值得评估的形态
  （对照我们已有的 RPA 定位：computer-use 同样是"无接口时的补丁层"）。

## 8. 可做的小实验

1. 个人 Mac（非公司托管设备）装免费版：25 talk/25 agent 额度内，
   实测中文听写延迟（验证 450ms 说法）、企微/飞书里的插入兼容性。
2. 用一个无敏感数据的学习场景（如剪辑软件/表格函数）对比"看教程 vs 问 Clicky"
   的完成时间。
3. 观察其 overlay 光标/取消窗/范围限定在真实任务里的可靠性，作为我方
   computer-use 护栏设计的竞品参照。

## 8b. 补充对比：Typeless（2026-09-16 追问）

抖音原文写的"tableless"即 Typeless（typeless.com）。核完官网：**两者不在同一层**，
博主把它们放在一起比，比的其实只是"语音转文字"这一个交集功能。

- **Typeless 是 AI 听写键盘**：全平台（Mac/Win/iOS/Android，品类里平台覆盖最广，
  少数有原生 Android），核心是把口语实时转成"可直接发送"的书面文字——
  去赘词/重复、听懂改口（"0912…不对，0922"只留正确版本）、自动列点排版、
  按所在应用调语气（邮件正式/聊天随意）、个人字典、100+ 语言混说、
  选中文字后语音改稿/翻译/提问（ask anything）。纯云端、单次会话上限 6 分钟、
  无离线模式；隐私口径是云端零留存/不用作训练/历史仅存本机，
  ISO 27001 + GDPR + HIPAA；免费档各来源说法不一（2,000~4,000 词/周），
  Pro 约 $12-15/月。两次 Product Hunt #1 Product of the Day。
- **HeyClicky 是屏幕感知的 AI 伙伴**：Mac only，听写只是它的一个模块；
  独占价值是读屏后在屏幕上画箭头做手把手教学、以及后台 computer-use agent
  真的替你操作浏览器/Google/Apple 应用。
- **选品逻辑**：只要"说→干净文字、全平台、手机也要用、关心合规认证"→ Typeless
  （同类还有 Wispr Flow，代码场景识别更强；要纯本地离线看 VoiceInk/OpenWhispr）。
  要"看着我的屏幕教我操作 / 语音派 agent 代做"→ HeyClicky，且无替代品在同一形态。
- 对博主"Typeless 复杂场景上下文不足所以弃用"的说法要打折扣：Typeless 的上下文
  是**当前应用/选中文字**层面，本来就不看全屏，拿"屏幕理解"要求它属于跨品类比较；
  它的 ask-anything 能问选中文字，但不能看见整个界面。
- 企业视角：Typeless 的合规姿态（零留存、HIPAA、有 Enterprise 档）比 HeyClicky
  成熟，听写这一狭窄用途进公司评估的门槛相对低；但仍是纯云端处理，
  语音内容出域这一条不变。

## 9. 风险和边界

- **数据安全/合规**：屏幕即数据，客户信息、合同、内部系统界面存在出域风险；
  公司设备使用前必须有明确结论，不默认安全。
- **国内可用性**：changelog 1.0.47 自承亚洲/中东请求曾被其 AI 供应商经香港
  数据中心阻断，现自动绕回美国——意味着依赖不稳定的跨境链路，且语音实时性受网络影响。
- **平台**：仅 macOS Sonoma 14.2+，Windows 只有 waitlist。
- **成本**：免费额度很轻（各 25 条/月），日常用 $20/月，重度 agent $100/月。
- **开源错觉**：能审计的只是 4 月前旧代码，当前商用版本闭源，
  "开源所以安全"的推理不成立。
- 语音 agent 误操作风险虽有护栏，仍不适合在生产系统上挂真实写权限。

## 10. 当前结论

值得作为"AI 入口形态演进"的雷达标的跟踪，并可在个人设备上小实验；
暂不引入公司环境。抖音成稿价值一般：视频把它窄化成输入法对比、且有未经验证的
数字，若要做新媒体内容应先实测再写，角度建议放在"AI 入口第四形态与
屏幕感知的隐私边界"，而不是"又一个更强的输入法"。
