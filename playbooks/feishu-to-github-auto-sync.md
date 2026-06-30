# 飞书打捞处 → GitHub 自动同步流水线

## 目标

把打捞处和相关话题中的资料、讨论、MVP 测试和阶段性结论，自动整理为结构化 Markdown，并同步到公开 GitHub 仓库。

不是群聊原文搬运，而是自动完成：

```text
飞书新内容
  ↓
识别内容类型
  ↓
追原始来源 / 提炼核心信息
  ↓
判断公开级别
  ↓
生成打捞卡 / 实验卡 / Playbook 草稿
  ↓
写入仓库
  ↓
git commit + push
```

## 内容类型

### 1. 资料型内容 → notes/

适用：链接、截图、视频、文章、GitHub 项目、商业产品、趋势判断、方法论。

输出：打捞卡。

### 2. MVP / 实验型内容 → experiments/

适用：试试看、跑一下、MVP、测试结果、失败记录、效果反馈。

输出：实验卡。

### 3. 稳定方法 → playbooks/

适用：已多次验证、可复用的方法、流程、模板、Checklist。

输出：Playbook。

### 4. 风险边界 → risks/

适用：版权、数据安全、权限、合规、公开边界、内部信息风险。

输出：风险卡或风险清单。

## 公开级别

- `public`：可直接公开
- `sanitized`：脱敏后公开
- `private-only`：只本地记录，不进入公开仓库
- `needs-review`：暂存，等待确认

## 自动同步原则

1. 外部公开资料可以自动入库。
2. 群聊讨论不能原文公开，只能转成摘要和判断。
3. 涉及公司内部系统、客户、真实数据、截图、流程细节时，默认 `sanitized` 或 `private-only`。
4. 技术 / 开源资料必须尽量追到 GitHub、官网、文档或 Demo；未追到本体前标记为 `待追源`。
5. 所有自动提交都要有清晰 commit message，方便回滚。

## 文件命名

```text
notes/YYYY-MM/YYYY-MM-DD-slug.md
experiments/YYYY-MM/YYYY-MM-DD-slug.md
playbooks/topic-name.md
risks/topic-name.md
```

## 后续增强

- 自动生成 indexes
- 自动去重
- 自动关联 thread/topic
- 自动生成周报
- 对 `needs-review` 内容建立审核队列
