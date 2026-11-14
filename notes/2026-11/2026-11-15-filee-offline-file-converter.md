---
title: Filee——本地离线拖拽式文件转换器（270 格式，带 CLI 与 watch，仓库自带 Agent Skill）
date: 2026-11-15
discovery_source:
  type: 群聊线索
  title: 打捞处群内 GitHub 链接（许晶晶）
  url: https://github.com/KnifeLemon/Filee
primary_object:
  type: open_source_project
  name: KnifeLemon/Filee
  url: https://github.com/KnifeLemon/Filee
object_type: [open_source_project, trend_signal]
source_type: [GitHub, 官网]
business_tags: [ITBP, 运营, 个人能力]
problem_tags: [流程提效, 组织协同]
method_tags: [RPA, 自动化, Agent, 本地优先]
tool_tags: [Filee, dotnet, Avalonia, LibreOffice, FFmpeg]
value_stage: 待验证
risk_tags: [数据安全, 合规, 版权]
public_level: public
---

# Filee：不上传、不装 Office 的本地文件转换器

## 1. 这是什么

Filee 是一个免费开源（MIT）的桌面文件转换器：按住 Ctrl（macOS 为 Option）拖拽文件，光标处弹出环形格式菜单，松手即转。覆盖图片、PDF、Office 文档、表格、演示、HWP/HWPX（韩文文档）、电子书、音视频、字幕、压缩包、字体、CAD、3D、地理矢量等约 270 种格式，全部在本机完成，不上传文件、不依赖安装 Office。

技术栈 C# / .NET 10 / Avalonia，跨 Windows 10/11、Linux x64/ARM64、macOS 13+（预览，尚未公证）。引擎组合：Magick.NET、PDFsharp、PDFium、rhwp，按需下载 LibreOffice / Pandoc / calibre / FFmpeg / Ghostscript，下载件锁定 SHA-256。

## 2. 原始来源

- 发现入口：打捞处群消息（GitHub 链接，提示 Releases 选安装版/便携版）
- 资料本体：https://github.com/KnifeLemon/Filee （24.9k star / 1.4k fork，MIT）
- 官网：https://filee.sh/ ；最新版 2.4.0（2026-11-09），共 32 个 release
- 安装：Windows 有自研安装版 exe、便携版 zip、winget（`winget install KnifeLemon.Filee`）、Microsoft Store；macOS/Linux 解包跑 install.sh

## 3. 核心观点 / 核心能力

- **纯本地、零上传**：结果存在原文件旁边，离线可用；对涉密/客户文件这是相对在线转换网站（iLovePDF 类）的根本差异。
- **格式覆盖面极广且有中国场景**：可读 OFD（中国电子发票/公文格式）、WPS/ET、简繁中文本地化、多语言 OCR（可简中+英文混合识别）、HEIC/iPhone 实况照片。
- **批量与自动化入口齐全**：多文件并行转换、watch 文件夹（落盘即转）、右键菜单、`filee` 命令行；CLI 支持 `convert --json`、`watch --json` 流式输出进度事件，还有 `--pages` 选页。
- **2.4.0 安全加固值得注意**：修复两个 GHSA 公告；更新器对 checksums 做 ECDSA P-256 签名校验并锁定安装包发布者；保留 Mark of the Web；宏不执行并提示；拒绝解压炸弹（超深/超大嵌套）；浏览器拖入链接默认拒绝本机和内网地址。
- **本地 AI 能力**：接入 Upscayl 引擎做 2–4 倍图片放大（Vulkan 本地跑，不联网）；Whisper 本地转写字幕/音视频为 TXT。
- **趋势信号——桌面工具原生为 Agent 设计**：仓库内带 `skills/filee` 目录（给编码 Agent 用的技能说明），明确建议 Agent shell 用 `--json --quiet`（因为 Agent 常把 stderr 并入 stdout，包装器读 stdout 到结束会卡死长任务）。一个 6 周龄的项目把"被 Agent 调用"当作一等使用场景来设计。

## 4. 我学到了什么

- 这个项目 2026-09-29 才首次提交，到 11 月已 2299 次提交、32 个 release、24.9k star，并冲上 Trendshift C# 周榜第一。提交记录大量出现 "Co-Authored-By: Claude Opus 5.5"——单人作者（韩国开发者 Myeonggu Kim）+ AI 编程高速产出的典型样本，迭代速度本身就是产品力的一部分。
- 它的架构是"薄主程序 + 按需下载的成熟引擎"，自己不重造转换内核，而是编排 Magick/PDFium/LibreOffice/FFmpeg 等既有开源件，并做校验和固定版本。这与本组"确定性后端操作工具化、编排层做路由"的口径一致。
- 安全设计的成熟度（签名更新、MOTW、zip 炸弹防护、宏隔离）远超一般六周龄个人项目，说明作者把"处理不可信文件"当核心威胁模型——文件转换器天然是恶意文件入口，这个认知在线上同类工具里不常见。

## 5. 它是否可信，哪些需要验证

可信度较高但需实测，依据与待验项：

- 证据：MIT 开源、代码与 CI 公开、有 SECURITY.md 和已披露并修复的安全公告（响应透明）、67 名贡献者、引擎校验和固定哈希。
- 待验证：
  1. 项目极年轻（首提至今约 6 周），110 个 open issue，长期维护与单作者依赖风险尚不可知；
  2. macOS 版是预览且未公证（首次启动需手动放行），企业内 Mac 推广有合规摩擦；
  3. 中文场景实测未做：OFD 发票转 PDF/Excel 的保真度、简中 OCR 准确率、WPS 格式兼容；
  4. 企业杀软/EDR 对未普及安装包的拦截情况、便携版在受控终端的运行权限。

## 6. 对个人能力有什么价值

- 替代各种在线转换网站和零散小工具：HEIC 转 JPG、PDF 合并/拆分/压缩、OFD 发票处理、视频转 GIF/抽音频，一个工具全收，且不把文件送出本机。
- CLI + watch 文件夹可以直接接进个人自动化：下载目录落盘自动转 PDF、截图批量转 WebP 等。

## 7. 对企业 AI 落地有什么价值

- **RPA/自动化管线的转换环节**：来也/影刀流程里遇到格式转换（如供应商来件 OFD/PDF 转可处理格式、图片 OCR、批量 HEIC），可用 Filee CLI 作为本机执行节点，比调在线转换 API 更安全、无按量费用，也避免在 RPA 里硬编 Office 自动化。
- **数据安全叙事**：金融/医疗/客户资料类文件不能上公网转换工具，本地优先架构是企业内推广的硬理由；与市场部网站曾被植入恶意 JS 的安全巡检口径同源——能不出内网就不出。
- **Agent 工具化样板**：仓库自带 Agent skill、CLI 输出 JSON 事件流，是"桌面应用如何让 AI 助手可靠调用"的现成参考，对本组设计飞书机器人后端工具有直接借鉴（结构化事件、quiet 模式、长任务不阻塞）。

## 8. 可做的小实验

1. 找一台 Windows 测试机装便携版，用真实样本跑一组：OFD 电子发票→PDF、手机 HEIC 批量→JPG、扫描版 PDF 简中 OCR→TXT、DOCX→PDF 保真度对照。
2. 试 `filee watch <文件夹> --to pdf --json`，验证落盘即转与 JSON 事件流能否被脚本/计划任务稳定消费。
3. 评估便携版在公司 EDR 下的运行情况与安装包签名（Certum 证书，作者 Myeonggu Kim）是否被信任。

## 9. 风险和边界

- 项目年龄与单作者风险：功能迭代极快但也意味着 breaking change 和未发现缺陷的概率高，不宜直接上生产关键链路，先个人/边缘场景用。
- 已有安全公告史（2.4.0 修了两个），处理不可信来源文件时必须保持最新版，不要拿它打开来历不明的文档当杀毒防线。
- macOS 未公证、Linux 拖拽依赖 X11（Wayland 只能用拖放区和右键），跨平台体验不均。
- 可选引擎（LibreOffice、calibre 等）首次使用时联网下载，受控网络需提前离线预置。
- 许可证 MIT 允许商用，但分发时需保留版权与声明；企业内统一打包前过一遍 THIRD-PARTY-NOTICES。

## 10. 当前结论

雷达观察 + 可小实验。本地优先、270 格式、CLI/watch/JSON 与自带 Agent skill 的组合，在"文件转换"这个老赛道里做出了 Agent 时代的新形态，且安全模型清醒。建议先在 Windows 测试机用 OFD/HEIC/OCR 三个中国高频场景实测，验证保真度与 EDR 兼容性后再考虑纳入 RPA 工具箱；暂不建议在生产流程和全员范围推广，主要约束是项目仅 6 周龄。
