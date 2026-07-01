# 每日进仓简报机制

## 目标

每天早上 9 点自动发送前一日打捞处 GitHub 入库简报，并做基础错漏核对。

## 简报范围

默认统计前一自然日（Asia/Shanghai）：

- Git commit
- 新增 / 修改文件
- notes / experiments / playbooks / risks 的分类数量
- 关键主题
- value_stage 分布
- public_level 分布
- 明显错漏和待补项

## 核对项

每日简报至少检查：

1. 是否有 commit 但没有对应文件变化。
2. 新增 Markdown 是否包含 frontmatter。
3. 是否缺少 `public_level`、`value_stage` 等关键字段。
4. 文件路径是否符合约定：
   - `notes/YYYY-MM/`
   - `experiments/YYYY-MM/`
   - `playbooks/`
   - `risks/`
5. 是否出现疑似群聊原文搬运。
6. 是否有 `needs-review`、`private-only`、敏感信息风险。
7. 是否有资料标记为 `待追源`、`待验证` 但缺少下一步。

## 输出格式

```text
昨日进仓简报（YYYY-MM-DD）

1. 入库概况
- commits：
- 新增/更新文件：
- 分类：notes / experiments / playbooks / risks

2. 主要内容
- ...

3. 价值判断
- 已沉淀：
- 可小实验：
- 待追源/待验证：

4. 错漏核对
- 正常：
- 需补：
- 风险：

5. 今日建议
- ...
```

## 注意

简报只基于公开仓库和本地仓库可见内容做核对。若某些飞书消息没有成功入库，需要在后续接入飞书消息日志或自动化运行日志后进一步比对。
