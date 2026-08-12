---
title: "HTML 在 AI 时代的回潮：从 Markdown 到单文件可交互工具"
date: 2026-08-13
discovery_source:
  type: 抖音视频
  title: "大李书房一盏灯：我宣布进入HTML时代"
  url: https://v.douyin.com/srN3jEHSEiw/
primary_object:
  type: trend_signal
  name: "HTML as AI-era artifact format"
  url: https://www.lennysnewsletter.com/p/how-i-ai-html-is-the-new-markdown
object_type: [trend_signal, methodology, case_or_media]
source_type: [抖音, 播客, 技术博客]
business_tags: [ITBP, 个人能力, 运营]
problem_tags: [流程提效, 知识沉淀, 工具自制]
method_tags: [Agent, Vibe Coding, 自动化, Prompt]
tool_tags: [Claude Code, HTML, Canvas API, Squoosh]
value_stage: 可小实验
risk_tags: [版权, 数据安全]
public_level: public
---

# HTML 在 AI 时代的回潮：从 Markdown 到单文件可交互工具

## 1. 这是什么

一条中文技术短视频用一个很直观的例子切入“HTML 是 AI 时代更合适的文件格式”这个观点：作者让 Claude Code 花两三分钟生成了一个纯前端 HTML 图片压缩工具，支持轻度/中度/强力/自定义四档压缩、批量处理、单张或打包下载，整个过程在浏览器本地完成，无需后端、无需安装、双击即可分享使用。

视频背后的更大趋势，来自 Anthropic Claude Code 团队工程师 Thariq Shihipar 提出的“HTML is the new Markdown”：当模型上下文足够大、AI 产出从短文本变成几千行的规划与规格时，Markdown 容易变成“文字墙”，而 HTML 可以承载标签页、颜色标注、内联 SVG、交互控件、可滚动区域，让人真正愿意去读、去评审 AI 的产出。

## 2. 原始来源

- 发现入口：抖音 @大李书房一盏灯《我宣布进入HTML时代！》https://v.douyin.com/srN3jEHSEiw/
- 观点源头：Thariq Shihipar 在 Lenny's Podcast 的访谈《HTML is the new Markdown: How Anthropic engineers are building with Claude Code》
  - https://www.lennysnewsletter.com/p/how-i-ai-html-is-the-new-markdown
  - YouTube: https://www.youtube.com/watch?v=Qrpm7E80wQ0
- 二手解读：
  - UXHack《Markdown is Dead? Why Claude is Pivoting back to HTML》https://uxhack.substack.com/p/markdown-is-dead-why-claude-is-pivoting
  - 36氪中文综述 https://m.36kr.com/p/3808885041274376
- 同类成熟工具：Google Squoosh（纯前端图片压缩，开源）https://squoosh.app

## 3. 核心观点 / 核心能力

1. **HTML 不是要取代 Markdown，而是分层。** Markdown 仍适合作为仓库里的工作语言、纯文本规格和版本对比；HTML 更适合作为给人看的交付物——规划、报告、讲解页、交互式原型。
2. **单文件 HTML 是天然的“一次性微软件”容器。** HTML + CSS + JS 可以塞进一个 `.html` 文件，双击在浏览器打开，无需部署、无需后端、无需账号。对 AI 来说，生成一个自包含文件比生成完整应用简单得多。
3. **图片压缩工具的技术原理很朴素。** 浏览器原生 Canvas API 读取本地图片 → 绘制到 canvas → 调整尺寸和 `toBlob` 质量参数 → 导出压缩后的图片；整个过程不上传服务器，速度快且隐私友好。
4. **人在 AI 工作流里的角色从“写代码”变成“分配计算”。** Thariq 的说法是 engineer as compute allocator：决定什么值得做、边界在哪、怎么跟 agent 同步，而这些大量发生在 spec 和规划阶段。可读的 HTML 产出让人能持续介入，而不是把几千行 Markdown 丢给 AI 自己改。
5. **典型适用场景。**
   - 项目规划、技术方案讲解、事故复盘、周报
   - 一次性专用编辑器（编辑某个配置表、某段 JSON）
   - 交互动效原型、设计系统 living spec
   - 本地小工具：图片压缩、格式转换、CSV 清洗、文本处理

## 4. 我学到了什么

- 以前习惯把 AI 产出默认存成 Markdown，但当产出需要给别人看、需要交互、需要可视化时，一个单文件 HTML 反而更容易被打开和使用。
- “让 AI 写一个完整应用”门槛高，但“让 AI 写一个自包含 HTML 小工具”门槛极低，且产物可立即分享给非技术同事双击使用。
- 浏览器本身就是一个被低估的运行时：Canvas、File API、Blob、Web Workers、WebAssembly 已经能支撑相当多的本地处理工具，不需要为每个小需求都走后端。
- 对实施团队来说，这是一种“轻量 RPA”的补充思路：不是所有自动化都要进 RPA 平台，一次性、低风险、纯本地的小工具可以用 HTML 快速解决。

## 5. 它是否可信，哪些需要验证

- 趋势源头可追溯到 Anthropic 工程师公开访谈和 Simon Willison 等技术博主的讨论，不是凭空营销。
- Canvas 前端图片压缩是成熟技术，Squoosh 等开源项目已验证多年。
- 需要保留判断的地方：
  - “HTML 取代 Markdown”是标题党式表达，主流观点其实是分层使用，不是二选一。
  - HTML 体积更大、token 成本更高、版本对比不如 Markdown 干净，不适合作为仓库里的主要 source of truth。
  - 复杂前端能力（大文件、并发压缩、WebP/AVIF 编码）在不同浏览器上有兼容性差异，真正要推广的工具需要实测。
  - AI 生成的 HTML 工具可能引入 XSS、外部 CDN 依赖、数据泄露等问题，处理敏感文件前要检查是否真的纯本地。

## 6. 对个人能力有什么价值

- 多一个“AI + 浏览器”的工具心智：遇到重复的小处理任务，先想能不能让 AI 生成一个单文件 HTML，而不是找在线工具或写脚本。
- 提升对 AI 产出的评审能力：把长文档让 AI 转成可视化 HTML，更容易发现逻辑漏洞和遗漏。
- 给非技术同事交付成果时，HTML 比 Markdown 更友好，尤其是带图表、流程图和交互的内容。

## 7. 对企业 AI 落地有什么价值

- **轻量内部工具自制。** 业务团队可以让 AI 生成单文件 HTML 工具处理图片、CSV、文本、配置，不必每个需求都排 IT 开发，也不必把数据传到外部在线工具。
- **内部知识交付升级。** 方案、周报、事故报告可以用 HTML 做成可交互、带图表的版本，提升管理层和业务方的阅读意愿。
- **数据安全友好。** 纯前端处理意味着文件不出本机，对客户数据、合同、截图等敏感场景比在线工具更可控——前提是生成的 HTML 确实不依赖外部服务。
- **边界。** 单文件 HTML 不适合承载复杂业务逻辑、权限控制、审计日志，也不能替代正式系统和 RPA 流程；它的位置是“一次性、低风险、本地优先”的长尾工具。

## 8. 可做的小实验

1. **图片压缩工具实测。** 让 Claude Code / Cursor 按视频描述生成一个单文件 HTML 压缩器，测试：
   - 批量 20 张以上图片是否稳定
   - 强力压缩能否稳定压到 1MB 以内
   - PNG 透明通道是否保留
   - 是否真的无网络请求（DevTools Network 验证）
2. **内部常用小工具合集。** 选 2-3 个团队高频小需求（如 CSV 去重、文本批量替换、飞书消息格式排版），让 AI 各生成一个单文件 HTML，放在内部共享盘，统计是否有人用。
3. **HTML 周报模板。** 让 AI 把一周工作总结生成一个带图表和标签页的 HTML 周报，对比 Markdown 版本的阅读反馈。
4. **敏感数据检查清单。** 对 AI 生成的 HTML 工具统一检查：是否引用外部 CDN、是否有 fetch/XHR、是否把数据写入 localStorage，作为内部使用前的轻量 review 标准。

## 9. 风险和边界

- **数据安全：** 任何处理客户截图、合同、员工信息的 HTML 工具都必须先断网验证无外发，不能只凭作者说“本地处理”。
- **版权与合规：** AI 生成的代码可能夹带不兼容许可证的片段，内部广泛使用前需做基础检查。
- **浏览器兼容：** 移动端 Safari、旧版 Edge 在 Canvas、WebP、AVIF 上行为可能不一致，面向多人分发的工具要测主流浏览器。
- **维护成本：** 一次性工具生成快，但如果长期使用，需要有人负责修复 bug 和更新依赖，否则会变成“野生工具”。
- **不要神化。** HTML 适合交付和交互，Markdown 适合版本管理和纯文本协作，二者应按场景分层。

## 10. 当前结论

这条视频的价值不在于“HTML 要取代 Markdown”，而在于它展示了一个很实用的工作方式：**AI 让单文件 HTML 成为低成本的微软件交付格式，浏览器本身就是运行时。** 对个人和实施团队来说，这是对 RPA、脚本和在线工具的一个轻量补充，尤其适合一次性、低风险、本地优先的数据处理和可视化任务。下一步可以做一个最小的图片压缩 HTML 工具实测，并沉淀一套“AI 生成 HTML 工具上线前检查清单”。
