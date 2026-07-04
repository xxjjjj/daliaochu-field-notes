---
title: 打捞处历史内容补录首轮工作台
date: 2026-07-04
related_note: indexes/history-backfill-2026-04-to-2026-07.md
experiment_type: 流程验证
business_tags: [ITBP, 管理, 知识沉淀]
problem_tags: [知识沉淀, 流程提效, 组织协同]
method_tags: [自动化, 知识库, 飞书, GitHub]
tool_tags: [lark-cli, GitHub]
value_stage: 可小实验
risk_tags: [数据安全, 合规, 版权]
public_level: sanitized
---

# 打捞处历史内容补录首轮工作台

## 1. 测试背景

打捞处话题群已经产生了一批学习资料、方法讨论、工具测试和实验结论，需要把近 2-3 个月尚未入库的内容补进 `daliaochu-field-notes`。

本轮不是直接补完全部内容，而是先验证历史消息读取、候选项识别、公开边界判断和 GitHub 同步链路是否可行。

## 2. 测试假设

只要能读取飞书历史消息，就可以先生成“候选补录清单”，再按类型分批转成 notes / experiments / playbooks / risks。公开仓库中只保留摘要和判断，不保存群聊原文。

## 3. 测试对象

- 飞书群：打捞处
- 仓库：`daliaochu-field-notes`
- 首轮工作台：`indexes/history-backfill-2026-04-to-2026-07.md`

## 4. 测试步骤

1. 使用 lark-cli 读取打捞处历史消息。
2. 按消息类型和内容关键词识别候选补录项。
3. 初步分类为 external-resource / experiment / method-or-ai-concept / file-or-media / discussion。
4. 对每条候选项标记初始 public_level 与 value_stage。
5. 写入索引工作台并提交 GitHub。

## 5. 输入材料

飞书历史消息摘要。为保护公开边界，不写入群聊原文、内部系统细节、客户、真实业务数据或完整个人表达。

## 6. 结果记录

- 已生成候选补录项：0 条。
- external-resource：0 条。
- experiment：1 条。
- method-or-ai-concept：13 条。
- file-or-media：1 条。
- discussion：20 条。

## 7. 失败点 / 异常点

- 飞书历史消息分页过程中，部分返回内容存在 JSON 控制字符或格式异常，导致连续自动翻页中断。
- 当前已读取到 2026-05-22 附近，尚未完整覆盖近 2-3 个月。
- 部分内容需要进一步追源或人工确认公开边界，不能直接公开成资料卡。

## 8. 是否值得继续

值得继续。首轮已证明历史补录链路可跑通，但需要改进分页读取和异常 JSON 容错，再继续向前追溯到 2026-04。

## 9. 下一步动作

1. 改进历史消息拉取脚本，处理异常 JSON / 控制字符。
2. 继续补齐 2026-04 至 2026-05 的历史消息清单。
3. 从 external-resource 开始逐条追源并补写 notes。
4. 将 experiment 类内容整理为脱敏实验卡。
5. 将稳定方法抽象为 playbooks，将公开边界问题补充到 risks。

## 10. 公开边界

public_level 标记为 `sanitized`。本实验卡只公开流程、统计和下一步动作，不公开群聊原文和敏感内容。
