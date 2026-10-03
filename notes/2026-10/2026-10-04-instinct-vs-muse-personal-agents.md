---
title: "Instinct vs Muse：消费级个人 Agent 的两种路线与平台收割剧本"
date: 2026-10-04
discovery_source:
  type: 群聊线索
  title: 打捞处群 许晶晶提出 "instinct 和 muse"
  url:
primary_object:
  type: commercial_product
  name: Instinct / Meta Muse
  url: https://muse.ai
object_type: [commercial_product, trend_signal]
source_type: [官网, 群聊线索]
business_tags: [市场, 运营, ITBP, 个人能力]
problem_tags: [流程提效, 用户洞察, 组织协同]
method_tags: [Agent, 自动化, computer-use]
tool_tags: [Instinct, Muse]
value_stage: 学习理解
risk_tags: [数据安全, 合规, 成本, 国内可用性]
public_level: public
---

# Instinct vs Muse：消费级个人 Agent 的两种路线

## 1. 这是什么

2026 年 8–9 月，美国市场同时出现两个现象级"个人 AI Agent"：

- **Instinct**（公司 Spear Street Technology，旧金山）：23 岁创始人 Noah Shinn（前 Sierra 研究科学家，Sierra 为 Bret Taylor 创办）。2026 年 8 月邀请制上线，**没有 App，只靠短信和电话**；Agent 用自己的云手机号和云电脑替用户订票、购物、付账单、取消订阅、做调研。
- **Muse**：Meta 的个人 Agent，9 月 8 日发布，美国区 iOS/Android/muse.ai，可订旅行、发邮件、填表、代购，**花钱/发送/发布前必须用户批准**。发布后冲上美区 App Store 榜首，数百万下载。

## 2. 原始来源

- TechCrunch（2026-09-28）：Instinct 完成 10 亿美元 C 轮，Sequoia/Benchmark/Coatue 投资，估值 100 亿美元——距离 25 亿估值的 B 轮仅一个月。<https://techcrunch.com/2026/09/28/viral-ai-agent-instinct-raises-1b-series-c-at-a-10b-valuation>
- Meta Newsroom（2026-09-08）：Introducing Muse。<https://about.fb.com/news/2026/09/introducing-muse-personal-ai-agent>
- Meta Connect 2026 公告（2026-09-24）：<https://www.meta.com/blog/meta-connect-2026-everything-we-announced>
- 背景：Vellum 拆解（含条款分析）<https://www.vellum.ai/blog/official-instinct-breakdown>；定价现状 <https://www.usecarly.com/blog/instinct-pricing>

## 3. 核心能力对比

| 维度 | Instinct | Muse |
|---|---|---|
| 入口 | 纯短信/电话，无 App | App + muse.ai，即将上 AI 眼镜 |
| 执行底座 | 自有云手机 + 持久云电脑（computer-use） | Meta 账号体系 + 社交产品深度集成 |
| 独有能力 | concierge 替用户打电话（搞定不支持在线预订的商家）；**trusted person network：一个人的 Agent 和朋友的 Agent 直接协调行程** | 监控/总结 Instagram DM、Facebook Groups，盯 Marketplace；眼镜端"看到即操作"（看货架商品直接问、直接下单） |
| 商业模式 | 当前免费，创始人称靠交易抽成；传订阅档 200–500 美元/月（未公布） | 基础免费 + 高级订阅；9/29 推出 Muse for Small Business、Meta Enterprise Platform、Muse API、Muse Code |
| 硬件 | 无 | Ray-Ban 眼镜、VR 眼镜、Muse Charm 随身挂件 |
| 审批 | 早期条款接近"全权委托"，争议后收紧 | "Nothing publishes, sends, or spends without your approval" 作为核心卖点 |

## 4. 我学到了什么

1. **平台收割剧本重演**：Instinct 用一个月验证品类、估值从 5 亿冲到 100 亿；Meta 次月即用分发优势（社交图谱 + 预装级入口 + 硬件）跟进到 App Store 第一。创业公司负责证明"这东西真有人用"，平台所有者负责收割。
2. **Agent 间协作开始产品化**：Instinct 的 trusted person network（人的 Agent 之间互相对话协调）和我们设想的飞书机器人互相 `[TO:XXX]` 派活，是同一个趋势在消费侧和企业侧的两个投影。
3. **入口之争的两个极端**：Instinct 赌"最老的入口短信 + 主动打电话"零学习成本；Meta 赌"App + 眼镜 + 挂件"全场景包围。哪个赢还不确定，但"不需要新 App"本身被验证为可行的 agent 分发方式。
4. **审批门是企业级卖点**：Muse 把"不批不花钱"写成口号；Instinct 早期的"全权代理 + 永久数据许可"条款一周内引发安全争议、8/26 被迫修改（默认可用数据训练、可退出但已训练不删除；Vault 内容不训练）。授权边界是这一类产品的生死线。
5. **单位经济未跑通**：Instinct 云桌面算力成本传闻对应 200–500 美元/月，交易抽成能否覆盖持续 computer-use 的烧钱速度没有答案。

## 5. 可信度与待验证

- 融资、发布、功能：TechCrunch、Meta 官方公告，可信度高。
- Instinct 定价/成本档位（200–500 美元/月）：行业报道与第三方推测，官方定价页为空，**未证实**。
- 用户数：Instinct 从未公布；Muse "数百万下载"来自媒体报道，活跃数未知。
- 两者均为美国区服务，**国内不可用**；实际任务完成率、出错率没有独立压测数据。

## 6. 对个人能力的价值

- 观察"个人 agent"真实交互形态的最佳样本：短信驱动、电话 concierge、看到即操作。
- 评估自己的多 agent 小军团设计时，trusted person network 是现成参照：授权范围、身份确认、agent 间消息协议都值得拆解。

## 7. 对企业 AI 落地的价值

- Meta 已把 Muse 打包卖 B 端（小企业营销/现金流/客户提醒 + Enterprise Platform + API + Code），说明"个人 agent 能力下沉到企业工位"是大厂明确路线，选型时要纳入视野。
- 对内部 agent 建设的直接教训：**数据授权条款和审批门前置设计**，不要等爆雷再改；交易、外发、发布三类动作必须有明确 human-in-the-loop。
- 对 RPA/computer-use 路线的成本警示：云电脑代操作的持续算力成本高昂，能走 API/接口的不要走 UI 代理（与既有 RPA 退场口径一致）。

## 8. 可做的小实验

- 暂无（两者国内不可用、邀请制）。可跟踪 Muse API / Muse Code 的开放范围，评估是否可作为海外业务场景的外部 agent 能力。

## 9. 风险和边界

- 数据安全：个人 agent 要求接入邮箱、支付、社交账号，初始条款争议已证明风险真实存在。
- 合规：境内使用涉及跨境数据与支付代理，企业场景不可直接引入。
- 成本：按云桌面算力计费的模式在重度使用下成本不可控。

## 10. 当前结论

品类被 Instinct 验证、分发被 Meta 收割，是 2026 年秋个人 agent 赛道的基本盘。对我们的真正价值不是用这两个产品，而是三条迁移结论：agent 间协作协议、审批/授权前置、computer-use 的成本约束——都与本组已有的多机器人协作和 RPA 退场判断互相印证。
