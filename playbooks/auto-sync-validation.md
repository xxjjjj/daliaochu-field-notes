# GitHub 自动同步验证清单

## 目的

用于验证“打捞处飞书内容 → GitHub 结构化入库 → commit/push → 可追溯反馈”是否真实运行，而不是只在群里口头承诺。

## 最小验证闭环

每次命中自动同步后，应能看到四类证据：

1. **群内反馈**
   - 回复中说明本次写入了哪个文件
   - 说明 public_level 和 value_stage
   - 说明 commit 是否已推送

2. **GitHub 文件变化**
   - 仓库中出现新增或更新的 Markdown 文件
   - 文件路径符合约定：
     - `notes/YYYY-MM/`：资料型内容
     - `experiments/YYYY-MM/`：MVP / 实验 / 测试
     - `playbooks/`：可复用方法
     - `risks/`：风险边界

3. **Git 提交记录**
   - GitHub commits 中出现对应提交
   - commit message 能看出本次同步主题

4. **内容质量检查**
   - 不是群聊原文搬运
   - 有结构化摘要、判断、业务转译、风险/待验证点
   - 有 `public_level` 和 `value_stage`
   - 涉及内部信息时已脱敏或未公开

## 推荐验收方式

### 方式 1：发测试消息

在打捞处发一条明确测试内容，例如：

```text
同步测试：请把这条作为 experiments 记录，验证飞书到 GitHub 自动同步是否正常。测试目标是确认文件写入、commit、push 和群内反馈链路。
```

期望结果：

- GitHub 新增 `experiments/YYYY-MM/YYYY-MM-DD-auto-sync-test.md`
- 群里回复包含文件路径和同步状态
- GitHub commit 中能看到对应提交

### 方式 2：看仓库最新提交

打开：

https://github.com/xxjjjj/daliaochu-field-notes/commits/main

检查最新 commit 是否和群内最近一次资料/讨论对应。

### 方式 3：看文件内容

打开仓库，检查本次新增文件是否满足：

- 有 frontmatter
- 有主题判断
- 有业务价值
- 有风险/待验证问题
- 有下一步动作

## 失败判断

以下情况视为没有好好运行：

- 群里只回复“已同步”，但 GitHub 没有文件或 commit
- 只复制群聊原文，没有结构化整理
- 没有 public_level / value_stage
- 涉及内部敏感信息却直接公开
- 文件路径混乱，后续无法检索
- commit message 看不出同步内容

## 当前验证点

本文件本身就是一次验证样例：

- 类型：Playbook
- 目的：验证自动同步机制
- public_level：public
- value_stage：已沉淀
