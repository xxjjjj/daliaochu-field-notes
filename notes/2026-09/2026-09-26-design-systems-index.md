---
title: 50+ 大厂设计系统索引（可直接取用）
date: 2026-09-26
discovery_source:
  type: 小红书笔记（视频）
  title: 50+大厂设计系统，复制就能用
  url: https://xhslink.cn/o/A5ZSaOOCcLo
primary_object:
  type: 索引/资源合集
  name: 全球主流企业设计系统（Design Systems）官方入口
  url: 见正文清单
object_type: [trend_signal, open_source_project]
source_type: [小红书, 官网, GitHub]
business_tags: [产品, 运营, 市场, ITBP]
problem_tags: [流程提效, 组织协同]
method_tags: [Vibe Coding, 自动化]
tool_tags: [design-system, figma, react, vue, ui-kit]
value_stage: 已沉淀
risk_tags: [版权]
public_level: public
---

# 50+ 大厂设计系统索引（可直接取用）

## 0. 本体核对（2026-09-26 截图追源更新）

原视频（博主：锁毅博）指的本体是 GitHub 仓库 **VoltAgent/awesome-design-md**
（https://github.com/VoltAgent/awesome-design-md ），不是泛泛的"大厂设计系统导航"。

- 仓库本体：把热门网站的设计系统逆向提取成 **DESIGN.md 纯文本文件**，每个系统一份，
  丢进项目根目录，AI 编码 agent（Claude Code / Codex / Cursor 等）读取后直接生成风格一致的 UI。
- DESIGN.md 格式由 **Google Stitch** 提出；每份含设计 token（颜色/字体/间距/圆角）、
  组件样式、布局与响应式规则，并配 preview.html / preview-dark.html 预览。
- 截至 2026-09-26 实测：**117,916★ / 13,209 forks，MIT**；2026-03-31 创建，是今年增长最快的仓库之一
  （10 天 3.5 万★）。目录下已有 **74 个**站点/品牌：Vercel、Stripe、Linear、Notion、Apple、
  Claude、Cursor、Figma、Tesla、Ferrari、Starbucks、Nike、NVIDIA、Spotify、Binance、Coinbase、
  xAI、MiniMax 等，开发者工具/SaaS/消费品牌/车厂都有。
- 每份 DESIGN.md 结构（以 Linear 为例，实测全文约 23.5k 字符）：YAML frontmatter 里是完整 token 树
  （colors / typography / rounded / spacing / components），正文是布局原则、响应式断点、迭代指南和
  "已知缺口"（如 Linear 浅色系未公开、字体为专有需开源替代）——颗粒度足以直接约束 AI 出图。
- 真正价值定位：**解决"AI 生成的前端能用但视觉像五个 prompt 拼出来"的问题**，是 Vibe Coding 的
  设计约束层；不需要 Figma 导出或 JSON schema。

## 1. 这是什么

小红书一条视频笔记推荐上述仓库，卖点是"复制就能用"——做界面/原型/前端时，
把对应风格的 DESIGN.md 复制进项目，让 AI 编码工具直接产出匹配该设计系统的界面。

原笔记网页端因小红书 IP 风控（error 300012）无法直接读取；已通过群内截图追到本体并实测。
下方第 3 节起的清单是沿"设计系统"主题扩展的一手来源补充（52 个系统官网 + 许可证实测）。

## 2. 原始来源

- 发现入口：小红书视频笔记（博主：锁毅博，310 赞 / 939 收藏），笔记 ID `6ab257a100000000350198e0`
- 资料本体：https://github.com/VoltAgent/awesome-design-md （MIT，117.9k★，74 个 DESIGN.md，实测 2026-09-26）
- 相关链接：格式提出方 Google Stitch；分析 https://ossinsight.io/blog/design-md-protocol-2026；
  知乎《各大厂设计系统/官方资源汇总》https://zhuanlan.zhihu.com/p/558318039

## 3. 核心观点 / 核心能力

设计系统 = 设计规范（色彩/字体/间距/动效/图标）+ 设计资源（Figma/Sketch kit）+ 组件代码库。
对个人和企业的价值是：统一体验、减少重复设计、让 AI/低代码产出的界面有现成的视觉骨架。

### 国际科技公司

| 系统 | 公司 | 官网 | GitHub / 许可证 |
|---|---|---|---|
| Material Design 3 | Google | https://m3.material.io | material-web，Apache-2.0；图标库 54k★ |
| Fluent 2 | Microsoft | https://fluent2.microsoft.design | fluentui，20k★ |
| Human Interface Guidelines | Apple | https://developer.apple.com/design/human-interface-guidelines/ | 闭源规范，Design Resources 提供官方 UI 素材 |
| Carbon | IBM | https://carbondesignsystem.com | carbon，9.5k★，Apache-2.0 |
| Spectrum | Adobe | https://spectrum.adobe.com | react-spectrum，16k★，Apache-2.0 |
| Lightning Design System | Salesforce | https://www.lightningdesignsystem.com | 3.7k★ |
| Atlassian Design System | Atlassian | https://atlassian.design | 官方前端 monorepo |
| Polaris | Shopify | https://polaris.shopify.com | 6.2k★ |
| Primer | GitHub | https://primer.style | primer/react，3.9k★，MIT |
| Base Web | Uber | https://baseweb.design | 9k★，MIT |
| Gestalt | Pinterest | https://gestalt.pinterest.systems | 4.4k★，Apache-2.0 |
| Protocol | Mozilla | https://protocol.mozilla.org | MPL-2.0 |
| Elastic UI | Elastic | https://eui.elastic.co | 6.4k★ |
| Clarity | VMware/Broadcom | https://clarity.design | vmware-clarity |
| Grommet | HPE | https://v2.grommet.io | 8.3k★，Apache-2.0 |
| Garden | Zendesk | https://garden.zendesk.com | Apache-2.0 |
| Canvas Kit | Workday | https://workday.github.io/canvas-kit/ | Apache-2.0 |
| GEL | BBC | https://www.bbc.co.uk/gel | 规范为主 |
| Mand Mobile | 滴滴 | https://didi.github.io/mand-mobile/ | Apache-2.0 |
| Fiori | SAP | https://experience.sap.com/fiori-design-web/ | 企业规范 |

### 政府/公共部门（表单密集型系统的优质参考）

| 系统 | 官网 | 许可证 |
|---|---|---|
| GOV.UK Design System | https://design-system.service.gov.uk | MIT |
| USWDS（美国联邦） | https://designsystem.digital.gov | 开源 |
| Australian Design System | https://designsystem.gov.au | MIT |

### 国内大厂

| 系统 | 公司 | 官网 | GitHub / star |
|---|---|---|---|
| Ant Design | 蚂蚁/阿里 | https://ant.design | 99.6k★，MIT |
| Ant Design Mobile | 蚂蚁 | https://mobile.ant.design | 12k★ |
| Fusion Next | 阿里 | https://fusion.design | 4.7k★，MIT |
| Arco Design | 字节 | https://arco.design | 5.7k★，MIT |
| Semi Design | 字节/抖音 | https://semi.design | 10k★ |
| TDesign | 腾讯 | https://tdesign.tencent.com | 4k★，MIT |
| WeUI | 微信/腾讯 | https://weui.io | 27k★ |
| NutUI | 京东 | https://nutui.jd.com | jdf-e/nutui |
| AMIS（低代码前端） | 百度 | https://aisuda.bce.baidu.com/amis | 19k★ |
| Vant | 有赞 | https://vant-ui.github.io | 24k★，MIT |
| Ocean Design |  OceanBase/华为系 | https://oceanbase.github.io/oceanbase-design/ | MIT |
| Zarm | 众安 | https://zarm.design | MIT |
| View UI (iView) | TalkingData | https://www.iviewui.com | 开源 |

### 社区/开源（实际选用率最高的一档）

| 库 | 官网 | star |
|---|---|---|
| Bootstrap | https://getbootstrap.com | 175k★，MIT |
| shadcn/ui | https://ui.shadcn.com | 125k★，MIT（复制源码进项目模式，2024 后增长最猛） |
| MUI | https://mui.com | 99k★，MIT |
| Chakra UI | https://chakra-ui.com | 41k★，MIT |
| Mantine | https://mantine.dev | 32k★，MIT |
| Naive UI | https://www.naiveui.com | 19k★，MIT |
| Radix UI | https://www.radix-ui.com | 19k★，MIT（无样式原语） |
| Element Plus | https://element-plus.org | 28k★，MIT |
| Varlet（Vue3 Material 风格） | https://varletjs.org | 5.3k★，MIT |

## 4. 我学到了什么

1. "复制就能用"真正成立的路径有三条：官方 Figma kit（设计师）、组件库 npm 包（前端）、
   shadcn/ui 式"把组件源码复制进你仓库"（可完全掌控）。
2. 国内中后台事实标准是 Ant Design 系；移动端做微信/H5 是 Vant；字节系内部用 Semi/Arco。
3. shadcn/ui + Radix 代表的新趋势：不发 npm 包、直接给可改源码，天然适合 Vibe Coding——
   AI 改起来没有依赖黑盒。
4. 政府系（GOV.UK/USWDS）在无障碍、长表单、错误提示上规范最扎实，做内部审批/填报类系统值得偷师。

## 5. 它是否可信，哪些需要验证

- 官网可达性与 star/许可证均为 2026-09-26 实测（52 个中 47 个首页 200；SAP Fiori、Audi、
  VW、Oracle Redwood 等对 HEAD 请求拒绝，浏览器访问通常正常，不代表失效）。
- star 数为时点快照，会持续变化。
- 原小红书视频的具体清单内容未读到（IP 风控），本索引为主题重建而非原笔记搬运；
  若需要逐条核对原笔记，需晶晶在手机端转发截图。

## 6. 对个人能力有什么价值

- 做汇报原型、内部小工具界面时，先选一套设计系统再动手，产出的"完成感"差一个量级。
- 用 Figma 的话直接拿 Ant Design / Arco 官方 kit，不用自己画控件。

## 7. 对企业 AI 落地有什么价值

- Vibe Coding/AI 生成内部工具时，预设一套组件库（推荐 Ant Design 或 shadcn/ui）作为约束，
  AI 产出的界面统一、可维护，避免每次生成的页面风格随机。
- 飞书卡片、BI 仪表盘之外的轻量 Web 应用（驾驶舱、台账前端）可直接基于这些系统搭建。
- 设计 token（色彩/间距变量化）是把"品牌规范"喂给 AI 的标准接口，值得在自研前端里推行。

## 8. 可做的小实验

- 让 Claude Code/Codex 分别基于 shadcn/ui 和 Ant Design 各生成同一个内部工具页面，
  对比一致性、可改造成本和 bundle 体积。
- 抽取一套"INTCO 内部前端默认栈"约定（组件库+设计 token+模板工程），供后续 AI 写前端时默认引用。

## 9. 风险和边界

- **版权**：组件库多为 MIT/Apache-2.0，可商用；但各厂"设计规范文档/Figma 素材"的品牌资产
  （商标、特定视觉）不等于可任意复用，照搬大厂品牌皮肤做对外产品有侵权风险。取代码规范可以，
  不要冒充其品牌。
- 海外库（shadcn/MUI）文档与社区英文为主；国内合规场景注意组件本身不涉及数据出境，风险低。

## 10. 当前结论

本体已核实：**VoltAgent/awesome-design-md 是 Vibe Coding 的"设计约束层"**——把 74 个热门产品的
设计系统做成 AI 能直接读的 DESIGN.md，解决 AI 生成前端风格随机、拼接感重的问题，MIT 可商用，
117.9k★ 且仍在更新。下一步值得做的小实验：挑 1-2 份（如 Linear、Stripe）放进真实项目，
让 Claude Code/Codex 按 DESIGN.md 出页面，对比无约束时的一致性；后续可考虑为英科内部工具
写一份自己的 DESIGN.md（企业蓝/字体/间距 token 化），作为所有 AI 前端产出的默认约束。
下方 52 个传统设计系统清单作为补充索引保留。
