# AI UTM 与归因口径守门员 / AI UTM and Attribution Naming Guard

> 每天替老板检查广告、红人、EDM、社媒链接，先把“这单算谁的”这件事说清楚。

## 适合谁

- 出海品牌老板、独立站负责人、投放负责人。
- 同时做广告、红人、EDM、社媒、联盟获客的团队。
- GA4、Shopify、CRM、广告后台经常来源对不上的团队。
- 月底复盘时经常争论“这个订单算哪个渠道”的团队。

## 老板痛点

广告花了，红人发了，邮件也推了，可订单进来以后，GA4 里是 direct，Shopify 里是 referral，CRM 里又写着 email。团队一起看报表，投放、运营、红人、EDM 都觉得自己有贡献，但谁也拿不出一套干净口径。

很多问题发生在链接上线那一刻：少了 campaign，medium 混写，source 大小写不统一，同一场活动用了三个名字。

## 这个数字员工每天干什么

**输入：** 链接清单、活动日历、UTM 命名规范、GA4 / Shopify / CRM 只读来源报表。  
**输出：** 缺参清单、命名冲突表、修复建议、老板 5 行摘要和人审表。

它只做只读巡检和修复建议，不改预算、不自动改广告或邮件链接、不承诺最终归因。

## 可接手任务

1. 检查广告、EDM、红人、社媒链接是否缺 `utm_source`、`utm_medium`、`utm_campaign`。
2. 统一 facebook / fb / meta 等渠道命名。
3. 检查 medium 是否符合渠道逻辑，例如 email、paid_social、influencer 不混写。
4. 检查 campaign 是否能对应活动日历。
5. 检查红人链接是否能识别达人、内容、活动和渠道。
6. 检查 EDM 链接是否重复打标、活动名是否和邮件系统一致。
7. 生成链接上线前检查清单。
8. 生成老板可读的归因口径风险周报。

## 能力模块

| 模块 | 它负责哪段活 | 公开工具 / Skill |
|---|---|---|
| 链接体检 | 看链接该有的参数有没有写 | Google Campaign URL Builder |
| 命名守门 | 把渠道、活动、素材命名统一 | Google Ads ValueTrack、Klaviyo UTM Tracking |
| 活动对齐 | 检查链接是否对应真实活动 | 活动日历 + UTM 词典 |
| 报表预警 | 提醒 GA4 / Shopify / CRM 哪些口径暂时不能直接下结论 | Segment analytics.js、Snowplow Iglu Central |
| 人审交接 | 输出修复建议、老板摘要和负责人确认表 | NocoDB / Google Sheet / 飞书表格 |

## 工作机理

```text
链接清单 / 活动日历 / UTM 命名规范 / GA4-Shopify-CRM 只读报表
→ 拆 URL，读取 source / medium / campaign / content / term
→ 对照命名字典，检查缺参、错参、大小写、渠道归类
→ 对照活动日历，检查 campaign 是否能对应真实活动
→ 标记报表口径风险，输出高/中/低优先级
→ 生成修复建议、老板 5 行摘要和人审表
→ 投放/EDM/红人运营人工确认后再改线上链接
```

## 可复制提示词

```text
你是 AI UTM 与归因口径守门员。请检查我提供的链接列表，只做只读巡检，不要修改链接，不要给投放预算建议。

请输出：每条链接是否包含 utm_source、utm_medium、utm_campaign；缺失字段；风险等级；建议补充字段并标记“待人工确认”；老板可读的 5 行摘要。

链接列表：{{links}}
```

```text
请根据以下 UTM 命名规范，检查链接里的 utm_source 和 utm_medium 是否统一。

utm_source 允许值：google, facebook, instagram, tiktok, klaviyo, youtube, linkedin, influencer
utm_medium 允许值：cpc, paid_social, email, organic_social, influencer, affiliate, referral

请输出不符合规范的链接、原始参数值、建议标准参数值、是否需要人工确认、可能影响的数据报表口径。不要承诺最终归因结果，不要修改预算。

链接列表：{{links}}
```

```text
请检查以下红人合作链接是否具备基础追踪能力。重点检查达人来源、utm_source、utm_medium、utm_campaign、utm_content、哪些字段必须人工确认。

请输出表格：原链接 | 问题 | 风险等级 | 建议 UTM | 待谁确认。只做追踪口径建议，不判断达人真实带货效果。

链接列表：{{creator_links}}
```

## 人类主管验收

- 每条高风险问题是否保留原链接、问题、建议、负责人。
- 建议 UTM 是否符合团队真实活动和渠道口径。
- 是否把需要业务判断的字段标成“待人工确认”。
- 是否没有直接修改广告、EDM、红人链接或预算。
- 老板 5 行摘要是否能说清楚：哪里错、影响什么、谁确认、何时改。

## 不能自动化的事

- 不自动修改广告后台链接、跟踪模板或预算。
- 不自动修改 EDM 正在发送或自动化 flow 中的链接。
- 不自动判断最终归因或财务归属。
- 不自动调整渠道预算、素材投放、红人分佣。
- 不处理客户姓名、邮箱、电话、合同、报价等敏感信息。
- 不绕过广告平台、邮件平台、分析工具的政策和隐私要求。

## 第一周试点方案

- Day 1：收集最近 30 天广告、EDM、红人、社媒链接样本，确认只读边界。
- Day 2：建立第一版 UTM 命名字典，统一 source、medium、campaign 格式。
- Day 3：跑第一次巡检，输出缺参清单、错参清单、命名冲突表。
- Day 4：生成修复建议，但不执行；所有建议标注“待人工确认”。
- Day 5：选一个渠道小范围试点，如 EDM、红人或 Meta 广告。
- Day 6：生成老板可读周报，说明哪些口径暂时不能用于复盘。
- Day 7：复盘误报率、修复率和团队是否愿意长期使用。

## 相关 Skill 工具栈

| 工具 / repo | 地址 | 干什么 | 风险边界 |
|---|---|---|---|
| Google Campaign URL Builder | https://ga-dev-tools.google/campaign-url-builder/ | 生成标准 UTM 链接 | 只能生成链接，不保证团队命名一致 |
| Google Ads ValueTrack | https://support.google.com/google-ads/answer/6305348 | 检查广告链接参数 | 不等于最终归因 |
| Klaviyo UTM Tracking | https://help.klaviyo.com/hc/en-us/articles/115005082547 | 检查 EDM 自动打标 | 不自动改已发送邮件 |
| Snowplow Iglu Central | https://github.com/snowplow/iglu-central | 参考 schema 与命名结构 | 不建议小团队直接搭复杂数据平台 |
| Segment analytics.js | https://github.com/segmentio/analytics.js | 理解来源字段进入数据链路 | 不替代埋点工程和归因系统 |

## 安全边界

只使用公开资料、只读链接清单、聚合来源报表和团队内部命名规范；不需要客户隐私、合同、报价或未公开客户信息。外部发布前仍需郝敬确认。
