---
title: "PhotoCraft / ArtCraft：纯 Rust 净室重实现 Adobe 全家桶的十天开源引爆"
date: 2026-10-10
discovery_source:
  type: 短视频截图
  title: "索亚加德《Adobe真的要哭晕在厕所里了》"
  url: "（小红书/短视频平台，群内截图，未追到原始帖子链接）"
primary_object:
  type: open_source_project
  name: PhotoCraft (storytold/photocraft)
  url: "https://github.com/storytold/photocraft"
object_type: [open_source_project, trend_signal]
source_type: [小红书, GitHub, 官网]
business_tags: [ITBP, 个人能力, 市场]
problem_tags: [流程提效]
method_tags: [Vibe Coding, Agent, 开源工具]
tool_tags: [PhotoCraft, ArtCraft, Rust, 图像编辑]
value_stage: 待验证
risk_tags: [版权]
public_level: public
---

# PhotoCraft / ArtCraft：纯 Rust 净室重实现 Adobe 全家桶

## 1. 这是什么

一个名为 **ArtCraft** 的开源组织（storytold），在 2026-09-30 一天内把闷头开发四年多的"艺术家 IDE"引擎以十几个独立产品的形式开源，其中 **PhotoCraft** 定位为"用纯 Rust 净室重实现 Adobe Photoshop"，十天内冲到 GitHub 34.7k stars。短视频以"不到十天造出 Photoshop、Adobe 哭晕在厕所"为传播点，但"十天"是开源曝光时间，不是开发时间。

## 2. 原始来源

- 发现入口：打捞处群内短视频截图（作者"索亚加德"，5927 赞 / 9164 收藏）
- 资料本体：
  - https://github.com/storytold/photocraft（34.7k stars / 4.9k forks，Apache-2.0）
  - https://github.com/storytold/artcraft（11.2k stars，底层引擎，2022-03 创建）
  - 官网 https://getartcraft.com/
- 相关链接（同组织同期开源矩阵，均为纯 Rust 净室路线）：
  - filmcraft 7.0k（Premiere）、lightcraft 7.6k（Lightroom）、pdfcraft 6.3k、
    vectorcraft 5.1k（Illustrator）、effectcraft 3.8k（AE）、wordcraft 1.8k、
    cadcraft 1.3k、gridcraft 1.1k（Excel）、deckcraft 0.9k（PPT）、
    soundcraft、designcraft、craft-fonts、craft-libs

## 3. 核心观点 / 核心能力

- PhotoCraft：图层、蒙版、调整层、图层样式、文字、矢量、笔刷、真实 PSD 读写；
  macOS / Windows / Linux / FreeBSD / Web；离线本地；MIT OR Apache-2.0。
- 开发速度事实：仓库 2026-09-30 创建，10 天内 1049 次提交、100+ 贡献者、
  v0.1→v0.5 五个版本、23 个安装包资产；截至 10-09 仍在高频合并 PR。
- 贡献者列表里有名为 "claude" 的账号（28 commits），是人类 review + AI 生成
  的大规模协作开源形态。
- storytold 不是营销新号：组织内最早项目追溯到 2020 年（动作捕捉、Unreal 插件、
  语音转换），引擎 artcraft 仓库 1.37GB、开发四年半；官网/Discord/YouTube/
  LinkedIn 齐全，公司化运作（ArtCraft AI）。

## 4. 我学到了什么

- "十年功 + 十天引爆"的开源发布节奏：长期私有开发核心引擎，选一个时间点
  以产品矩阵形式集中开源，用"对标 Adobe/微软全家桶"的叙事一次性吃下关注度。
- 净室重实现（clean-room）是规避版权的标准打法：不看对方源码、按公开行为
  规范重写，配合开源协议商业化（官网卖安装体验 + 社区生态）。
- AI 辅助协作已经渗透到顶级开源项目的提交历史本身，"人均产出速度"的基线
  在被重写。

## 5. 它是否可信，哪些需要验证

可信部分（已实查 GitHub API）：仓库、star 数、提交数、版本、组织历史、
贡献者构成均真实。

待验证：
- PhotoCraft 实际完成度：PSD 复杂文件兼容性、滤镜/色彩管理/印刷工作流缺口
  （官方自认 early alpha）。
- 各 craft 产品是否共享成熟引擎、还是部分仅为占位仓库（多数同期创建）。
- 组织融资/商业化背景与长期维护可持续性。

## 6. 对个人能力有什么价值

- 市场/设计侧可作为免费离线工具储备观察，降低 Adobe 订阅成本的潜在替代。
- 技术侧是学习 Rust 桌面应用架构（wgpu/WGSL 渲染、图层引擎、跨平台安装分发）
  的大型一手样本。

## 7. 对企业 AI 落地有什么价值

- 间接价值为主：展示"AI 辅助 + 小团队 + 开源"可以把过去需要数百人年的
  基础软件以新速度推进，启发内部工具自研的成本预期。
- 企业场景（设计资产、印刷、与 Adobe 生态协作）短期不建议替换，alpha 阶段
  兼容性和稳定性风险高。

## 8. 可做的小实验

- 在独立测试机下载 macOS 安装包，用真实复杂 PSD（多层/调整层/智能对象/
  文字/矢量蒙版）做往返打开-编辑-保存兼容性测试。
- 对照 Photoshop 常用操作清单做完成度矩阵。
（注：按晶晶安排，实战测试在另一台电脑上从资料拉取执行，本笔记不含测试结果。）

## 9. 风险和边界

- 版权：净室重实现本身合法，但 PSD/字体/第三方插件兼容、商标使用仍有灰色地带。
- 成熟度：early alpha，不能进生产工作流。
- 数据安全：本地离线软件风险低，但仍需核验安装包签名与构建来源（已出现
  第三方非官方安装器仓库）。

## 10. 当前结论

项目真实且是 2026 年 10 月最值得关注的开源事件之一，但看点不是"十天造
Photoshop"，而是**四年半引擎积累 × 矩阵式开源发布 × AI 辅助社区协作**
三者叠加的新软件生产范式。实用替代 Adobe 为时过早，保持跟踪，测试结论
待独立测试机补充。
