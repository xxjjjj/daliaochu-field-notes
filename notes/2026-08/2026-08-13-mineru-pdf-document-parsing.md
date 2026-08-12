---
title: "MinerU：开源文档解析引擎，PDF/Office 转 Markdown/JSON"
date: 2026-08-13
discovery_source:
  type: 小红书
  title: "开源神器❗️ pdf文档图表公式元素精准提取"
  url: "http://xhslink.cn/o/50pwl9YFBuO"
  author: "HOWTO薯"
primary_object:
  type: open_source_project
  name: "MinerU"
  url: "https://github.com/opendatalab/MinerU"
object_type: [open_source_project]
source_type: [GitHub, 小红书, 官网]
business_tags: [ITBP, 个人能力]
problem_tags: [知识沉淀, 流程提效]
method_tags: [RAG, 知识库, OCR, 文档解析, Agent]
tool_tags: [MinerU, PDF-Extract-Kit, PaddleOCR, UniMERNet]
value_stage: 可小实验
risk_tags: [版权, 数据安全, 国内可用性]
public_level: public
---

# MinerU：开源文档解析引擎

## 1. 这是什么

MinerU 是上海人工智能实验室 OpenDataLab 团队开源的一站式高质量文档解析工具，将 PDF、图片、DOCX、PPTX、XLSX 等复杂文档转换为机器可读的 Markdown 和 JSON，面向 LLM 预训练、RAG 和 Agent 工作流设计。

- GitHub：<https://github.com/opendatalab/MinerU>（77.4k stars，6.5k forks）
- 官网：<https://mineru.net>
- 文档：<https://opendatalab.github.io/MinerU>
- 最新版本：mineru-3.4.0（2026 年 6 月）
- 许可证：MinerU Open Source License（基于 Apache 2.0）

## 2. 原始来源

- 发现入口：小红书 @HOWTO薯 笔记
- 资料本体：GitHub 仓库 opendatalab/MinerU
- 上海 AI 实验室官方报道：<https://www.shlab.org.cn/news/5443982>
- 相关项目：
  - PDF-Extract-Kit（PDF 模型解析工具链）：<https://github.com/opendatalab/PDF-Extract-Kit>
  - OmniDocBench（文档解析评测基准）
  - Magic-Doc（网页/电子书提取）

## 3. 核心能力

1. **版面智能分析**：自动区分标题、正文、图片、表格、页眉页脚；适配双栏/多栏排版，按阅读顺序重组文本。
2. **表格还原**：支持跨页表格、合并单元格，输出 HTML/Markdown/CSV。
3. **公式识别**：区分行内公式和行间公式，输出标准 LaTeX/MathML；底层使用自研 UniMERNet 模型。
4. **图表截取**：插图单独导出并关联图注，适合知识库和训练数据集。
5. **OCR**：内置 PaddleOCR，支持扫描件和图片型 PDF，支持 109 种语言。
6. **多格式支持**：PDF、图片、DOCX、PPTX、XLSX、网页 URL。
7. **双引擎**：VLM（视觉语言模型）+ OCR，可按场景选择。
8. **Agent 生态**：原生支持 MCP 协议，集成 LangChain、LlamaIndex、RAGFlow、Dify、FastGPT。

输出格式：Markdown、JSON、Content list（多模态训练用）、LaTeX。

## 4. 技术架构

底层 PDF-Extract-Kit 由四个关键模块组成：

- 布局检测：LayoutLMv3 微调模型
- 公式检测：基于 YOLOv8 的自研模型（区分行内/行间）
- 公式识别：自研 UniMERNet
- OCR：PaddleOCR

3.x 版本新增 VLM 引擎和 pipeline 模式，支持 CPU 推理（`-b pipeline`），降低硬件门槛。

## 5. 使用方式

| 方式 | 适合人群 | 说明 |
|------|---------|------|
| 在线 Demo（mineru.net） | 快速测试 | 免登录可用 flash 模式，小文件 |
| 桌面客户端（Win/Mac） | 普通用户 | 可视化拖拽 |
| Python CLI | 开发者/批量处理 | `uv pip install -U "mineru[all]"` → `mineru -p input.pdf -o output/` |
| Docker / API 部署 | 生产环境 | FastAPI 服务，支持并发批量 |
| MCP Server | Agent 集成 | 原生 MCP 协议 |

## 6. 可信度评估

**已验证：**

- GitHub 仓库真实存在且高度活跃（5700+ commits，77.4k stars）。
- 上海 AI 实验室官方新闻确认其出处和开源时间（2024 年 7 月 WAIC 发布）。
- 官网 mineru.net 可访问，提供在线 Demo 和客户端下载。
- 核心技术栈（LayoutLMv3、YOLOv8、UniMERNet、PaddleOCR）在官方文档和论文中有据可查。

**小红书笔记中的偏差/需注意：**

- 笔记称"底层基于 PDF-Extract-Kit"——这是 2024 年早期架构的描述。MinerU 3.x 已发展为独立引擎，新增 VLM 模式，不再只是 PDF-Extract-Kit 的封装。
- 笔记中对比 Nougat"速度慢、硬件要求高"——Nougat 确实偏重，但 MinerU 自身的 VLM 模式也需要 GPU；CPU pipeline 模式精度会有取舍。
- 笔记未提及 3.x 新增的 DOCX/PPTX/XLSX 原生解析、MCP 支持和 Agent 生态集成。

## 7. 对个人能力的价值

- **知识库建设**：将 PDF 论文、技术文档转为 Markdown 直接导入 Obsidian/Notion，公式以 LaTeX 保留，比手动截图效率高一个量级。
- **RAG 数据准备**：JSON 输出可直接接入 RAG pipeline，表格和公式不会像 pdfplumber 那样崩坏。
- **日常办公**：财报、研报、合同中的表格提取，避免手动重录。

## 8. 对企业 AI 落地的价值

- **内部知识库 RAG**：企业内部大量 PDF 格式的制度、规范、技术文档，MinerU 可作为文档入库前的解析层，比通用文本提取保留更多结构信息。
- **可本地私有化部署**：相比 Mathpix 等付费 SaaS，MinerU 开源免费且可本地部署，适合处理含敏感信息的内部文档。
- **Agent 工具链**：MCP Server 支持意味着可以直接作为 Agent 的文档读取工具，无需额外开发。
- **成本**：本地部署无 API 调用费用；GPU 服务器有一次性投入，但 CPU pipeline 模式可处理对精度要求不极端的场景。

## 9. 可做的小实验

1. **在线 Demo 快速验证**：拿一份双栏学术论文或含复杂表格的财报 PDF，到 mineru.net 试转，对比 Markdown 输出质量。
2. **本地 CLI 批量测试**：`uv pip install -U "mineru[all]"` 后，用 `minu -p` 批量处理一个目录的 PDF，检查公式和表格还原效果。
3. **RAG 集成验证**：将 MinerU 输出的 JSON 接入一个简单 RAG pipeline（如 LangChain + Chroma），对比 pdfplumber 的检索效果。
4. **MCP Server 接入 Hermes**：配置 MinerU MCP Server，让 Agent 能直接解析用户发来的 PDF 附件。

## 10. 风险和边界

- **数据安全**：在线 Demo 会上传文件到服务器，处理内部敏感文档必须本地部署。
- **版权**：解析他人版权文档用于个人学习合理，但批量抓取和再分发需注意版权。
- **硬件要求**：VLM 高精度模式需要 GPU（建议 8GB+ VRAM）；CPU 模式可用但速度慢、精度可能下降。
- **质量非 100%**：复杂手写、极低质量扫描件、非标准排版仍可能出错，关键文档需要人工质检。
- **许可证**：MinerU Open Source License 基于 Apache 2.0，但有特定条款，商用前需阅读 LICENSE.md。

## 11. 当前结论

MinerU 是当前开源文档解析领域的头部项目，出处可信、社区活跃、功能覆盖全面。相比 pdfplumber 等基础工具，它在公式、表格、复杂版面上有代际优势；相比 Mathpix，它免费且可私有化。对于需要做知识库或 RAG 的场景，值得先花 30 分钟用在线 Demo 或本地 CLI 验证实际效果，再决定是否纳入正式工具链。

小红书笔记作为入门介绍基本准确，但信息停留在 2024 年的版本，实际能力（多格式支持、VLM 引擎、MCP 生态）已超出笔记描述。
