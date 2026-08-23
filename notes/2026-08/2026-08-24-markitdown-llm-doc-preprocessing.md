---
title: "MarkItDown：给 LLM 喂文档前的本地预处理层"
date: 2026-08-24
discovery_source:
  type: "群内分享（许晶晶）"
  title: "MarkItDown 工具确认"
  url: "https://github.com/microsoft/markitdown"
primary_object:
  type: "开源工具"
  name: "microsoft/markitdown"
  url: "https://github.com/microsoft/markitdown"
object_type:
  - 开源工具
source_type:
  - GitHub
  - 官方 README
business_tags:
  - ITBP
  - AI-Infra
problem_tags:
  - 文档入模 Token 浪费
  - RAG 文档清洗
method_tags:
  - 文档转 Markdown
  - 本地预处理
tool_tags:
  - markitdown
  - pdfminer.six
  - pdfplumber
  - python
value_stage: 已追源
risk_tags:
  - 安全-不可信输入需沙箱
  - 效果数据待自测
public_level: public
status: "已核官方 README 与 pyproject；可公开"
---

# MarkItDown：给 LLM 喂文档前的本地预处理层

## 1. 这是什么

微软 AutoGen 团队开源的 Python 工具（MIT，PyPI 包名 `markitdown`），把 PDF / Word / PPT / Excel / HTML / CSV / 图片（EXIF + OCR）/ 音频（EXIF + 转写）/ EPUB / YouTube URL / ZIP 等一批格式统一转成 Markdown，目标读者是 LLM 和文本分析流水线，不是人眼高保真阅读器。

仓库热度：GitHub 约 175k stars / 12.8k forks（2026-08 抓取，会变动）。定位上作者自己把它和 textract 对比，差异是"输出保留文档结构的 Markdown"。

## 2. 原始来源

- 仓库：<https://github.com/microsoft/markitdown>
- README：<https://raw.githubusercontent.com/microsoft/markitdown/main/README.md>
- 依赖清单（pyproject）：<https://raw.githubusercontent.com/microsoft/markitdown/main/packages/markitdown/pyproject.toml>
- License：MIT
- Python 要求：>=3.10

已核验事实：

- 出品方确实是 Microsoft AutoGen Team（README 顶部 badge）。
- PDF 解析可选依赖是 **pdfminer.six>=20251230 + pdfplumber>=0.11.9**，不止 pdfminer.six 一个。
- DOCX 走 mammoth + lxml；XLSX 走 pandas + openpyxl；PPTX 走 python-pptx。
- 扫描件/图片里的文字需要装第三方插件 `markitdown-ocr`，它走的是 **LLM Vision**（OpenAI 兼容 client），不是 Tesseract 这类传统 OCR；不提供 `llm_client` 时静默回退到内置转换器。
- README 明确安全提示：MarkItDown 以当前进程权限做 I/O，不可信输入要自己做 sanitize，建议用最窄的 `convert_stream()` / `convert_local()`。

待核验：

- "可减少 70%~80% Token"这一数字**在官方 README 里没有出现**，属于坊间/二手说法，真实压缩比高度依赖原 PDF 排版和图片占比，用前必须拿自己样本实测。
- 对复杂表格、多栏排版、数学公式、中文扫描件的实际保留质量需自测。

## 3. 核心观点 / 核心能力

直接把 PDF 丢给 Claude/GPT 时，平台通常"文本抽取 + 每页转图片视觉解析"两路并行，页眉页脚、排版指令、矢量路径、整页位图全部计入 Token。MarkItDown 做的事是在**本地**先用结构化解析器把文档拆成标题 / 段落 / 列表 / 表格 / 链接，输出 Token 效率更高的 Markdown，再交给模型。

使用方式：

```bash
pip install 'markitdown[all]'          # 或按需 [pdf,docx,pptx]
markitdown input.pdf -o output.md
cat input.pdf | markitdown            # 也支持 stdin
```

Python：

```python
from markitdown import MarkItDown
md = MarkItDown()
result = md.convert("spec.docx")
print(result.text_content)
```

插件机制默认关闭，启用用 `--use-plugins` / `enable_plugins=True`；Azure Document Intelligence、Azure Content Understanding 作为可选后端处理复杂版式。

## 4. 我学到了什么

- "喂模型前先在本地洗一遍"这个动作现在有了微软背书的标准件，不必每次自己拼 pdfplumber + markdownify。
- Markdown 之所以是 LLM 友好格式，不只是"干净"，更是因为主流模型在大量 Markdown 语料上训练，结构标签本身就是强信号。
- optional-dependencies 的设计值得抄：按格式切 `[pdf]` `[docx]` `[audio-transcription]` `[youtube-transcription]`，生产环境只装需要的，避免拖一堆重依赖。

## 5. 它是否可信，哪些需要验证

- 出品方、License、依赖、CLI/Python API 均已在官方仓库核到，可信。
- "Token 省 70%~80%"不要直接写进对外材料，需要拿真实样本（含扫描件、含大表、含图文混排）跑三组对比：直传模型 vs MarkItDown 输出 vs 其他方案（Docling / Unstructured / Azure Doc Intelligence）。
- markitdown-ocr 走 LLM Vision，等于把图片再送一次多模态模型，**不一定省钱**，算账时要把这部分 Token 算进去。

## 6. 对个人能力有什么价值

- 本地就有一个统一的"任意文档 → Markdown"入口，做资料归档、RAG 入库、飞书文档预处理都能复用。
- 可以替换目前打捞处流程里一些零散的 pdfplumber/python-docx 拼接代码。

## 7. 对企业 AI 落地有什么价值

- RAG / Agent 文档流水线的前置清洗层，部署在内网即可，文档不出域。
- 对合规敏感场景（合同、工艺单、客户资料），比把原始 PDF 直接传到公网多模态模型更可控。
- 可以和内部对象存储 / 知识库挂钩，做成"上传即转 Markdown → 切片 → 向量化"的标准链路。
- 注意：它不是"开箱即用的企业级 RAG"，没有切片、向量库、召回评测，定位就是 ETL 里的"Extract + 初步 Transform"。

## 8. 可做的小实验

1. 选 3 份 INTCO 真实文档：一份纯文字制度 PDF、一份含大表的报价单、一份扫描件（盖章/拍照件）。
2. 分别用 MarkItDown 默认、MarkItDown + OCR 插件、pdfplumber 直抽、Azure Doc Intelligence 转出 Markdown。
3. 量化对比：文件大小、Token 数、表格保留率、中文乱码率、扫描件召回率。
4. 把结论沉淀到 playbook，给后续"要不要在 RAG 流水线里上 MarkItDown"做决策依据。

## 9. 风险和边界

- **安全**：官方明确说会以当前进程权限读文件；处理外部上传的不可信文档时必须放沙箱、限制 `convert_*` 作用面，不要直接把用户文件路径塞进高权限服务。
- **不是阅读器**：输出给机器看的，高保真排版需求别用它。
- **扫描件**：默认 PDF 转换器对扫描件基本无效；要走 `markitdown-ocr` 插件并自备 LLM Vision。
- **复杂表格/公式**：pdfplumber 表格抽取在合并单元格、跨页表上仍可能错位，需要人工 spot-check。
- **坊间说法不要二次传播**：包括但不限于"省 80% Token""理解准确率提升 X%"这类带具体百分比的说法，官方没给数据，写对外材料前先自测。
- 群聊里提到的"用底层 PDF 解析组件做 fuzz / 渗透测试"是安全研究者视角，和工具本身用途无关；任何此类测试只在授权环境内做。

## 10. 当前结论

值得在打捞处 / RAG 预处理链路里做对照实验的候选工具，**不要**因为微软出品就直接定标准。先拿真实业务文档跑一轮对比，再决定是默认走它、还是只在特定格式上用、还是用 Docling/Azure Doc Intelligence 兜底。新媒体成稿方向成立，但"省 70%~80% Token"这类数据点必须换成自测数据才能发，否则就是传谣。
