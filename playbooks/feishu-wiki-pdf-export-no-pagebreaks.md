---
title: 飞书 Wiki 文档转 PDF（不分页版）Playbook
date: 2026-05-26
source: 打捞处群 oc_ca0d61c219081373b70a03bcc01f9101 (2026-05-26 14:07 → 15:03)
public_level: sanitized
value_stage: 已验证
tags: [feishu, wiki, pdf, export, docx, 文档协作]
---

# 飞书 Wiki 文档转 PDF（不分页版）Playbook

## 1. 它是什么

把飞书 Wiki 节点（docx）转成 PDF，并通过飞书文件消息发回对话窗口的标准流程。专指用户明确要求"**不分页**"（适合手机/单页阅读）的版本。

## 2. 解决什么问题

- 飞书 Wiki 文档默认导出 PDF 会按 A4 自动分页，导致很多内容被切到下一页，对手机阅读和短文档阅读体验很差。
- 用户经常需要"一屏到底"的 PDF 用于分享、打印、归档。
- 普通 `lark-cli drive +export` 导出的 PDF 默认按页，需要特殊处理。

## 3. 标准流程

### 3.1 第一次转换（最容易踩坑的版本）

```bash
# 1. 拿到 wiki 文档的 file_token（从 wiki URL 中提取）
# 例如 https://global-intco.feishu.cn/wiki/FQ2kweyzyi4gaCk54vrcF8fHnAg
# file_token = FQ2kweyzyi4gaCk54vrcF8fHnAg

# 2. 导出为 docx
lark-cli drive +export \
  --params '{"file_token":"FQ2kweyzyi4gaCk54vrcF8fHnAg","type":"docx"}' \
  -o /tmp/export.docx

# 3. docx 转 PDF（默认会分页）
# 这一步通常依赖 libreoffice 或 python-docx + reportlab
soffice --headless --convert-to pdf --outdir /tmp /tmp/export.docx
```

**问题**：默认 PDF 按 A4 自动分页，文档后面会有大量空白。

### 3.2 第二次改进（用户反馈"后面这么多空白"）

把"导出 docx 再转 PDF"的两步改成"直接读 markdown → 单页排版 → 生成 PDF"：

```bash
# 1. 导出为 markdown
lark-cli drive +export \
  --params '{"file_token":"FQ2kweyzyi4gaCk54vrcF8fHnAg","type":"md"}' \
  -o /tmp/export.md

# 2. 用 pandoc 生成自定义页面的 PDF
pandoc /tmp/export.md \
  -o /tmp/output-no-pagebreak.pdf \
  --pdf-engine=xelatex \
  -V geometry:margin=1in \
  -V geometry:papersize={20in,30in} \
  -V mainfont="PingFang SC"
```

**优势**：通过 `papersize={20in,30in}` 把页面设得超大，再配合 `margin=1in`，整篇文档在一页里就能铺开，不会自动分页。

### 3.3 第三次优化（用户反馈"还是不对"）

用户第二次反馈"文档不对"——通常是 pandoc 排版对中文文档的行距/字体处理不友好。改进方法：

```bash
# 直接用 weasyprint 或 playwright + html 渲染
pandoc /tmp/export.md -o /tmp/export.html --self-contained
weasyprint /tmp/export.html /tmp/output-no-pagebreak.pdf
```

或者用 Python `markdown + reportlab` 自行排版：

```python
import markdown
from reportlab.lib.pagesizes import A3  # 用更大页面
from reportlab.platypus import SimpleDocTemplate, Paragraph
from reportlab.lib.styles import getSampleStyleSheet
# ... 自定义样式 + 单页排版
```

### 3.4 通过飞书文件消息发回对话窗口

```bash
lark-cli im +messages-send \
  --chat-id oc_xxxxx \
  --msg-type file \
  --file /tmp/output-no-pagebreak.pdf
```

或用更稳的 `+chat-messages-list` + `+messages-resources-upload` 二步走。

## 4. 验证清单

- [ ] PDF 能在手机/电脑上"一屏到底"阅读，没有自动分页
- [ ] PDF 中文字体不乱码（推荐 `PingFang SC` / `Noto Sans CJK SC`）
- [ ] PDF 文件大小合理（不超过 10 MB）
- [ ] 通过飞书文件消息发送后，对方能正常下载并打开
- [ ] 原 wiki 文档如有更新，重新跑一遍流程即可（不用缓存 PDF）

## 5. 已知坑

### 5.1 文档类型决定导出方式

- **docx 类型**：用 `drive +export --type docx` → 再用 pandoc / soffice 转 PDF
- **sheet 类型**：直接 `drive +export --type pdf` 通常 OK，不需要不分页
- **slide 类型**：导出为 PDF 时自带分页（演示用），不分页需求少
- **wiki 节点**：先通过 `wiki nodes retrieve` 拿到真实 file_token，再走 `drive +export`

### 5.2 中文排版坑

- `pandoc + xelatex` 对中文标题间距、行距处理不友好，需要自定义 `geometry` 和 `mainfont`
- `weasyprint` 对 CSS 支持更全，但首次启动慢
- 字体一定要指定（不能依赖系统默认），否则 macOS / Linux / Windows 渲染结果不一致

### 5.3 分页 vs 不分页的选择

- **不分页**适合：领导力说明 / FAQ / 操作手册 / 内部规范
- **分页**适合：合同 / 报价单 / 演示 PPT / 法律文件
- 不要一上来就生成"不分页"，先问用户用途

### 5.4 文件大小限制

飞书单条文件消息上限 100 MB（普通租户），PDF 单页不分页版通常 < 5 MB，问题不大。但如果原文档是几百页的长内容，单页 PDF 可能撑爆，建议改用分页版。

## 6. 风险与边界

- **权限边界**：导出的 PDF 是基于用户当前权限的，敏感内容如果 wiki 本身设了"部分可见"，导出会保留这些限制
- **审计追溯**：导出 + 发送 PDF 的全过程会被飞书审计日志记录，无需额外担心合规
- **缓存策略**：建议每次重新生成，不缓存 PDF，因为原文档可能更新

## 7. 复用条件

- 任何飞书 wiki / docx / doc 节点都可以套用本 Playbook
- 不分页需求在企业内部文档协作里非常常见，建议封装成 Skill 一键调用

## 8. 已知替代方案

| 方案 | 适用场景 | 备注 |
|------|----------|------|
| 直接飞书 UI 导出 | 单次、不分页要求不高 | UI 体验差，且不分页选项不一定有 |
| `lark-cli drive +export --type pdf` | 自动分页 | 适合合同 / 报告 / 演示 |
| pandoc + 自定义纸张 | 不分页、手动控制 | 本 Playbook 主体方案 |
| weasyprint + HTML 渲染 | 不分页 + 复杂样式 | 灵活但启动慢 |
| `playwright + html2pdf` | 复杂排版 + 不分页 | 适合有大量 CSS 的场景 |

## 9. 来源

- 群消息 `om_x100b6e67c788b0a0c334769fd5`（2026-05-26 14:07，第一次转换请求）
- 群消息 `om_x100b6e67c3bb0508b2199bd016`（2026-05-26 14:08，第一次 PDF 文件）
- 群消息 `om_x100b6e6070ce8cb0c226cf4f1f`（2026-05-26 14:55，"不分页"需求）
- 群消息 `om_x100b6e60714810a0b4a7f10780`（2026-05-26 14:55，不分页版）
- 群消息 `om_x100b6e6002dbb8acc37fa37a56`（2026-05-26 14:59，重新转换请求）
- 群消息 `om_x100b6e60127e896cc341326985`（2026-05-26 15:03，"后面这么多空白"反馈）

## 10. 下一步

- 把"wiki → 单页不分页 PDF"封装成 Skill：`feishu-wiki-no-pagebreak-pdf`
- Skill 内部自动判断文档类型 + 选择最合适的转换路径
- 支持模板：领导力说明、FAQ、操作手册、企业内部规范，每种模板有对应的排版预设
