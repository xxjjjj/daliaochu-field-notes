---
title: 用 last30days 研究 last30days：自举首测
date: 2026-08-21
related_note: notes/2026-07/2026-07-01-codex-skills-money-list.md
related_experiment: experiments/2026-07/2026-07-02-codex-skills-source-review-mvp.md
experiment_type: 工具实测
business_tags: [市场, 销售, 运营, ITBP]
problem_tags: [用户洞察, 知识沉淀, 流程提效]
method_tags: [Agent, 自动化, Skill, 趋势雷达]
tool_tags: [Codex, last30days]
value_stage: 可小实验
risk_tags: [数据安全, 国内可用性, 合规]
public_level: sanitized
---

# 用 last30days 研究 last30days：自举首测

## 1. 测试背景

`last30days` 已安装并通过预检。用户要求“用这个技能研究一下这个技能”，即用 last30days 对它自身做一次真实运行，验证安装是否真正可用，同时观察输出质量、数据源覆盖和失败模式。

## 2. 测试假设

1. 如果 last30days 在当前机器上真正可用，它应当能对一个已知主题返回结构化证据和综合摘要。
2. 用“研究它自己”作为首测，可以同时验证：GitHub 项目模式、本地 corpus 模式、输出契约、安全默认模式。
3. 首测结果能帮助判断是否值得继续配置更多源，还是当前安全默认模式已足够支撑内部 MVP。

## 3. 测试对象

- Skill：`last30days` v3.21.1
- 安装路径：`/Users/crystalxu/.agents/skills/last30days`
- 主题：`last30days skill`
- 模式：安全默认模式（不读取浏览器 Cookie）

## 4. 测试步骤

### 第一轮：仅公开源

```bash
python3.12 /Users/crystalxu/.agents/skills/last30days/scripts/last30days.py \
  "last30days skill" \
  --github-repo mvanhorn/last30days-skill \
  --quick --no-browser-cookies --web-backend keyless \
  --emit compact \
  --save-dir /tmp/daliaochu-last30days-self-test
```

结果：5.8 秒完成。GitHub 返回 1 条项目证据，HN/Reddit/Polymarket/Web 均无有效结果。Reddit RSS 因 Python 环境缺少 `expat` 模块失败。输出被保存到 `/tmp/daliaochu-last30days-self-test/last30days-skill-raw.md`。

### 第二轮：加入本地 corpus

```bash
python3.12 /Users/crystalxu/.agents/skills/last30days/scripts/last30days.py \
  "last30days skill" \
  --github-repo mvanhorn/last30days-skill \
  --corpus /Users/crystalxu/.agents/skills/last30days \
  --corpus-all-time \
  --quick --no-browser-cookies --web-backend none \
  --emit compact \
  --save-dir /tmp/daliaochu-last30days-self-test
```

结果：8.2 秒完成。GitHub 返回 1 条项目证据，本地 corpus 返回 2 个文件（`SKILL.md`、`references/save-html-brief.md`）。Reddit 被 HTTP 429 限流。输出被保存到 `/tmp/daliaochu-last30days-self-test/last30days-skill-raw-2026-08-21.md`。

## 5. 输入材料

- 已安装的 last30days Skill 本体。
- GitHub 仓库 `mvanhorn/last30days-skill`。

## 6. 结果记录

### 6.1 它确实能跑

两次运行均成功完成，没有崩溃。安全默认模式下，它不会读取浏览器 Cookie，不会主动写配置，也不会读取项目内 `.env`。输出包含固定徽章、日期范围、源覆盖、证据聚类、统计、footer 和原始结果保存路径。

### 6.2 当前源覆盖很薄

安全默认模式下实际可用的源只有：

- GitHub：能返回项目星级、issue 数、语言和 README 摘要。
- 本地 corpus：能扫描指定目录下的 Markdown/TXT/PDF 文件。
- Reddit：当前被 HTTP 429 限流，且 RSS 路径依赖 Python `expat` 模块。
- HN：搜索到 1 条但被前缀过滤判定为误报。
- Polymarket、Web：无结果。
- X/Twitter、YouTube、TikTok、Instagram：未配置，不可用。

这说明：**不配置额外源时，last30days 在当前环境下更像“GitHub + 本地文件 + 部分公开源”的研究工具，而不是它宣传的全平台趋势雷达。**

### 6.3 本地 corpus 模式有业务价值

第二轮加入 `--corpus` 后，它能把本地 Skill 文件作为私有证据源，并标记为 `🔒 LOCAL ONLY`，不会进入发布或 agent JSON。这对企业内部场景很关键：

```text
公开源：GitHub/Reddit/HN 等外部趋势
本地 corpus：内部文档、历史笔记、客户资料（脱敏后）
综合输出：外部信号 + 内部知识的结构化摘要
```

这正是内部“趋势雷达 + 知识库”的理想形态。后续可以把打捞处仓库的 `notes/`、`playbooks/` 作为 corpus 输入，让 last30days 在研究外部主题时同时参考本地沉淀。

### 6.4 输出契约很强，但也很重

它的 SKILL.md 有 2040 行，包含大量“LAW”来约束模型不要跑偏、不要编造标题、不要漏 footer、不要输出 Sources 块。这说明：

- 作者花了大量精力修正模型在使用该 Skill 时的失败模式。
- 这个 Skill 的效果高度依赖宿主模型是否严格遵守输出契约。
- 内部改造时，如果宿主模型不稳定，需要进一步简化契约或做后处理校验。

### 6.5 GitHub 项目数据

运行时获取到的 GitHub 项目状态：

- 仓库：`mvanhorn/last30days-skill`
- Stars：约 58,838（运行时显示 59K）
- Open issues：147
- 语言：Python
- 最近更新时间：2026-08-18

这是一个高关注度、活跃维护的开源项目，不是玩具。

## 7. 失败点 / 异常点

1. **Reddit RSS 失败**：系统 Python 3.12 环境缺少 `expat` 模块，导致 RSS 解析回退失败。这是 Homebrew Python 3.12 的已知问题，需要安装 `python@3.12` 的 XML 依赖或改用系统 Python。
2. **Reddit HTTP 429**：第二轮被限流。说明 keyless Reddit 路径在当前网络/IP 下不稳定。
3. **HN 误报过滤**：唯一一条 HN 结果被前缀过滤移除。可能是关键词匹配策略较保守。
4. **无 Web 搜索后端**：第一轮用 `keyless` 模式没有返回 Web 结果。要获得更好的 Web 覆盖，需要配置 Brave/Exa/Serper/Parallel API Key。
5. **证据稀薄**：对于“last30days skill”这种特定工具主题，公开讨论本身可能分散在 GitHub issues、X、YouTube 等平台，而当前未配置这些源。

## 8. 是否值得继续

值得继续，但要分层：

- **当前阶段**：安全默认模式 + 本地 corpus 已足够支撑“外部公开信息 + 内部知识库”的轻量研究 MVP。
- **下一阶段**：如果要做真正的趋势雷达，需要至少补一个 Web 搜索后端（Brave 最便宜）和 `yt-dlp`（YouTube 转录）。
- **暂缓**：X/Twitter Cookie、TikTok/Instagram API Key 涉及账号风控和费用，等 MVP 验证通过后再考虑。

## 9. 下一步动作

1. 修复 Python 3.12 的 `expat` 问题，恢复 Reddit RSS 能力。
2. 安装 `yt-dlp`，启用 YouTube 转录。
3. 用一个真实业务关键词（非敏感）跑第一次“趋势雷达”测试，例如“AI agent skills”或“企业知识库 AI”。
4. 把打捞处仓库作为 `--corpus` 输入，验证外部信号 + 内部沉淀的综合效果。
5. 如果输出质量可接受，形成一个固定的“打捞处趋势雷达” Playbook。

## 10. 公开边界

本实验只使用公开 GitHub 数据和本地已安装的 Skill 文件，不包含群聊原文、内部客户、真实业务数据或账号登录信息。本地 corpus 内容被 last30days 标记为 `LOCAL ONLY`，不会进入外部发布。
