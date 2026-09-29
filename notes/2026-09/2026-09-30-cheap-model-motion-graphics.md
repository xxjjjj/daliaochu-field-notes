---
title: 普通大模型 + Agent Skill 跑出 Opus 5.5 级动态图形视频（RuiC-motion-reel）
date: 2026-09-30
discovery_source:
  type: 小红书短视频截图
  title: 索亚加德「普通大模型也能跑出opus5.5的那种视频效果」
  url: ""
primary_object:
  type: open_source_project
  name: HRuiCcc/RuiC-motion-reel
  url: https://github.com/HRuiCcc/RuiC-motion-reel
object_type: [open_source_project, trend_signal, methodology, case_or_media]
source_type: [小红书, GitHub, 官网]
business_tags: [市场, 运营, 产品, 个人能力]
problem_tags: [获客, 流程提效, 内容生产]
method_tags: [Agent, Vibe Coding, Prompt, 自动化]
tool_tags: [AgentSkill, Python, ffmpeg, DeepSeek]
value_stage: 可小实验
risk_tags: [版权, 幻觉, 成本]
public_level: public
---

# RuiC-motion-reel：普通模型零美术素材代码出片

## 1. 这是什么

小红书博主「索亚加德」短视频指向的 GitHub 开源项目 **HRuiCcc/RuiC-motion-reel**（Public，代码 MIT）。它是一个给编码 Agent 用的 Skill：一句「用代码做一条 15 秒动态图形片」，Agent 即从零生成 1920×1080/30fps（450 帧）成片，画面和配乐全部代码产出，不用 After Effects、不引用任何外部美术素材（字体随包，OFL 1.1）。

关键事实：**作者自称整条链路（建模、排版、分色、配乐、出片）是用 DeepSeek Flash 跑通的**，不绑定模型，任何能读写文件、执行命令的编码 Agent（Claude Code / ZCode / Codex 类）都能用。这就是「普通大模型跑出 Opus 5.5 效果」的实际本体——不是 Remotion，而是自研 Python 渲染引擎。

## 2. 原始来源

- 发现入口：小红书博主「索亚加德」两条短视频截图（2026-09-30 群内分享），第二条视频字幕「就是这个开源项目」并展示 GitHub 仓库页
- 资料本体：https://github.com/HRuiCcc/RuiC-motion-reel
- 依赖：python3（numpy + Pillow）、ffmpeg

## 3. 核心观点 / 核心能力

- 自研渲染引擎（engine/）：
  - core.py：画布、矢量/文字通道、2× 超采样、bloom/色差/颗粒、印刷原语（叠印 multiply、网点、纸纹、套印偏移）
  - three.py：真 3D——不用三角形光栅化，把曲面密采样成点云、投影后按深度排序散射（numpy 重复下标最后写入 = 画家算法），遮挡精确且自带颗粒感
  - dsp.py：代码合成配乐（FFT 时变滤波、磁带抖晃、混响、频谱分析）
- 15 张「风格牌」抽签换视觉语言：暗色科技 HUD、riso 丝网印刷、纸艺、银盐暗房、恒星普朗克配色、氰版蓝图、紫外光刻等，每种都有已渲染成片（docs/films/）。
- 模板工程含 8 个场景 + 一段配乐；new_reel.py 从模板起片并把抽中的风格写进项目 STYLE.md。
- 设计立场：时间先有网格再有镜头（一个小节一个场景，音画共用时间轴，剪辑点天然落强拍）；颜色是算出来的（分色网点乘法叠印）；品牌色必须实测，不凭印象挑。
- references/gotchas.md 记录约 40 条坑（症状都不指向真因），performance.md 有逐算子 CPU/CuPy 实测。

## 4. 我学到了什么

- 「便宜模型能出贵模型效果」的真正杠杆是一个强约束、高内聚的 Skill 脚手架：引擎把自由发挥空间收窄成风格牌+模板，模型只负责在固定语法里填充，于是模型档位可以下沉。这与本组「确定性逻辑用规则代码、模型只贴判断点」的口径完全一致。
- 点云画家算法是个聪明的工程取舍：避开 Python 逐三角形性能陷阱，还顺带获得铜版雕刻式颗粒美学——约束变成风格。
- 音画共用时间网格（120 BPM、小节即场景）解释了截图片头那行 `120 BPM` 参数：它是结构参数不是装饰。

## 5. 它是否可信，哪些需要验证

- 可信度较高：仓库真实公开、README 给出 8 支可播放的循环预览 GIF 和完整成片目录、依赖与结构具体可跑、协议清晰（MIT + OFL）。
- 作者自述（DeepSeek Flash 独立跑通）属单方主张，需自验。
- 待验证：
  1. 本地 clone 实测：用我们的普通模型（如 DeepSeek V4 Flash / 豆包 lite）按 SKILL.md 跑一条，记录轮数、耗时、CPU 渲染时长、返工点；
  2. 与 Opus 5.5 同任务对比一次成功率；
  3. 15 秒/450 帧之外的长度与定制化空间（非模板需求是否立刻失稳）；
  4. 渲染性能（未上 GPU 时全片耗时，perf_probe.py 可测）。

## 6. 对个人能力有什么价值

- 直接获得「一句话 → 可发布短片」的能力：产品 demo 片头、数据报告动态摘要、课程/分享标题卡可批量自产。
- 是研究「如何给 Agent 写好一个 Skill」的优质样本：触发条件、模板分层、风格约束、gotchas 沉淀、性能自检一应俱全，写法可迁移到本组其他 Skill。

## 7. 对企业 AI 落地有什么价值

- 市场/运营内容生产：15 秒品牌短片、展会循环视频、社媒片头，零素材采购、零 AE 人力，换风格牌即换片；品牌色按实测读取即可做企业版。
- 成本结构：模型费可下沉到 Flash 档，成本主要是一次性 CPU 渲染；全部本地运行，素材和脚本不出内网，数据安全面干净。
- 边界：仅限程序化动态图形/3D 点云风格；实拍、真人、写实场景、复杂叙事不适用。

## 8. 可做的小实验

- 待晶晶确认后再动手：
  1. clone 仓库，装 numpy/Pillow/ffmpeg，用模板先渲染默认片验证环境；
  2. 让普通模型抽一张风格牌、改一套品牌内容（标题/配色/排版）出一条 15 秒片；
  3. 记录全流程真实成本，产出一页实测纪要。

## 9. 风险和边界

- 版权：代码 MIT、随包字体 OFL 1.1，商用需保留 NOTICE；自创品牌内容时输入素材的授权仍由使用者负责。
- 零外部素材意味着画面风格局限于引擎表达力，客户定制到特定写实风格时会撞墙。
- 小红书视频是精选成片展示，普通模型真实一次成功率仍待实测，返工成本可能抵消模型差价。

## 10. 当前结论

项目本体已追到，质量证据（8+ 支成片）比上一张截图时充分得多，价值阶段从「待追源」升级为**可小实验**：这是「强 Skill 脚手架让模型档位下沉」论点的一个具体、可本地验证的样板。建议下一步直接 clone 实测普通模型跑片成本。
