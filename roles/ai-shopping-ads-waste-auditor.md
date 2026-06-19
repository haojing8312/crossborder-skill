# AI Shopping Ads 浪费巡检员｜AI Shopping Ads Waste Auditor

## 一句话定位

它每天帮老板盯住 Google Shopping / PMax 里那些花钱没出单、点击很多却没人买、商品状态拖后腿的广告漏水点。

## 适合谁

- 正在投 Google Shopping / Performance Max 的 Shopify、WooCommerce、独立站卖家
- SKU 多、国家多、预算不小，但只看总 ROAS 的跨境老板
- 投手每周做报表，老板仍然说不清钱浪费在哪的团队
- 想先用脱敏广告导出做低成本试点的增长负责人

## 老板痛点

很多老板看广告，只看总花费和总 ROAS。真正麻烦的是：一个搜索词 14 天花了 80 美元没出单，一个低毛利 SKU 抢走预算，断货商品还在投，Feed 标题写偏了，吸来一堆没有购买意图的点击。

这些问题不会自动出现在老板桌面上。投手太忙，运营只看商品，技术只看页面，最后广告账单每天在烧，没人把“钱、词、商品、页面、订单”放在一张表里讲清楚。

## 岗位职责

AI Shopping Ads 浪费巡检员每天接收已授权导出的 Google Ads、Merchant Center、GA4/站点分析、商品 Feed、库存和毛利分档数据。它把广告花费、搜索词、SKU、国家、设备、页面事件和商品状态合并，输出 Top 20 浪费项、证据、可能原因、建议动作和需要人工确认的人。

它不是自动投手，不负责调预算、改出价、加否词或改 Feed。它只做一件事：把最可能浪费广告费的地方找出来，让老板和投放负责人先处理最该止血的部分。

## 可接手任务

1. 生成每日/每周 Shopping 浪费 Top 20。
2. 找出花费高但 0 转化的搜索词、SKU、国家和设备。
3. 生成候选否定词清单，标明人工审核理由。
4. 把 Merchant Center 商品状态和广告表现合并，定位 Feed/GMC 方向的问题。
5. 把 Ads 点击与 GA4/Matomo/Plausible/PostHog 页面事件合并，找高点击低加购、低结账、低购买页面。
6. 监测断货仍投、拒登/受限仍有花费、价格或库存异常影响转化的商品。
7. 生成老板版周报：钱花在哪、证据是什么、谁来确认、今天先处理哪几项。
8. 维护浪费规则库和复查记录。

## 能力模块

| 模块 | 做什么 | 可对应工具 / 工作流 |
|---|---|---|
| 广告花费读取 | 读取 campaign、item_id、search term、country、device、cost、clicks、conversions、conversion value | Google Ads API、Google Ads Scripts、Google Ads 导出 |
| 商品状态联动 | 合并 Merchant Center 商品状态、价格、库存、标题、链接、图片、GTIN/brand 状态 | Merchant Center Product data spec、Merchant API Reports |
| 转化漏斗排查 | 把广告点击和页面事件、加购、结账、购买放在一张表里看 | GA4、Matomo、Plausible、PostHog |
| 本地合并与规则检查 | 合并 CSV/Parquet，跑 0 单高花费、低 ROAS、数据缺失等规则 | DuckDB、Great Expectations、Google Sheets |
| 自动提醒和看板 | 定时生成日报、周报、P0 异常提醒 | n8n、Grafana、Looker Studio |

## 工作机理

```text
Google Ads / PMax / Search Terms / 商品 ID
        ↓
Merchant Center 状态 / Feed 标题 / 价格 / 库存
        ↓
GA4 / Shopify / WooCommerce 订单与页面事件
        ↓
按 SKU、搜索词、国家、设备、落地页合并
        ↓
规则检查：高花费 0 单、低 ROAS、断货仍投、页面不转化
        ↓
输出：浪费清单、证据、可能原因、人工建议、负责人
        ↓
人类主管：确认预算、否词、Feed、页面和商品策略
```

## 可复制提示词

### 1. Shopping Ads 浪费 Top 20 诊断

```text
你是 Google Shopping / Performance Max 广告浪费诊断员。请只基于我提供的已授权导出数据分析，不要自动修改预算、出价、否定关键词、商品 Feed 或广告设置。

【Google Ads 商品表现数据】
字段：date, campaign, asset_group, item_id, product_title, country, device, impressions, clicks, cost, conversions, conversion_value
{{ads_product_rows}}

【Merchant Center 商品状态】
字段：item_id, status, disapproval_reason, price, availability, brand, gtin, link, image_link
{{merchant_rows}}

请输出表格：
item_id | 商品名 | 浪费类型 | 证据数据 | 可能原因 | 建议人工检查动作 | 优先级P0/P1/P2 | 需要谁确认

规则：
1. 花费高但 conversions=0 或 ROAS 显著低，标为候选浪费，不要下最终结论。
2. 如果商品状态 disapproved/limited、价格/库存/链接异常，请标出 Feed/GMC 方向。
3. 不要建议“直接暂停/调预算/改出价”，只能写“请投手人工评估”。
4. 不要编造毛利、库存、转化价值或平台原因。
```

### 2. 搜索词浪费初筛

```text
你是 Shopping Ads 搜索词审核助理。请根据商品信息和搜索词报告，找出疑似不相关或低购买意图的搜索词。你不能自动添加否定关键词，只能输出“候选否定词/需人工确认”。

【商品/品类信息】
{{product_catalog_summary}}

【搜索词报告】
字段：date, campaign, search_term, clicks, cost, conversions, conversion_value, country, device
{{search_term_rows}}

请输出：
search_term | 浪费分类（不相关/低意图/售后/教程/免费/二手/竞品/过泛/语言错配/需人工判断） | 证据 | 对应商品或品类 | 候选处理建议 | 是否建议加入否定词候选 | 人工审核理由

要求：
- 只依据输入数据，不推测 Google 未提供的查询意图。
- 品牌词、核心类目词、战略测试词必须标“投手确认”。
- 不输出任何 API 操作或自动执行步骤。
```

### 3. 广告点击高但页面不转化诊断

```text
你是广告落地页转化诊断助理。请把 Google Ads 商品点击数据与 GA4/Matomo/Plausible/PostHog 页面事件数据合并分析，找出“广告花费可能被落地页浪费”的线索。

【Ads 数据】
字段：item_id, landing_page, campaign, country, device, clicks, cost, conversions, conversion_value
{{ads_rows}}

【站点分析数据】
字段：landing_page, sessions, engaged_sessions, add_to_cart, begin_checkout, purchases, revenue, bounce_or_exit_rate, avg_load_time(optional)
{{analytics_rows}}

请输出：
landing_page | 关联SKU | 广告花费 | 点击 | 页面异常信号 | 漏斗断点 | 可能原因 | 建议人工检查 | 优先级

边界：
- 不要改页面、不改埋点、不改广告设置。
- 如果 analytics 数据缺失或口径不同，请标“数据口径需确认”。
- 不要把跳出率/停留时间直接等同于用户质量差，要说明只是诊断线索。
```

### 4. 老板版周报

```text
请把以下 Shopping Ads 巡检结果整理成老板能看懂的周报。不要使用广告黑话，不承诺节省金额，不指责任何个人。

【巡检结果】
{{audit_records}}

周报结构：
1. 本周检查范围：账号/广告系列/日期/SKU数量/花费范围
2. 本周最该看的 5 个浪费信号：每条说明“花了多少钱、问题在哪里、证据是什么”
3. Top 10 SKU/搜索词/落地页异常表
4. 需要投手确认的动作
5. 需要运营/商品/技术确认的动作
6. 不建议本周自动处理的事项
7. 下周巡检规则优化

要求：所有建议都写成“人工确认后处理”，不要写成已经执行。
```

## 第一周试点

- **Day 1：选范围**。选 1 个国家、1 个广告账户、30–100 个在投 SKU；确认只读数据，不改账户。
- **Day 2：定规则**。设定 14 天花费 > 50 美元 0 单、ROAS 低于目标线、断货仍投、商品状态受限、页面高点击低加购等规则。
- **Day 3：拉数据**。导出 Ads、Merchant Center、GA4/站点分析、库存、毛利分档和商品 Feed。
- **Day 4：出清单**。生成第一张 Top 20 浪费清单，每条写证据、建议动作、负责人。
- **Day 5：人工处理**。投手和运营只处理 10–20 个低风险动作，比如确认候选否词、暂停断货 SKU、修标题、检查页面。
- **Day 6：复查**。看处理项是否仍出现浪费信号，记录误报。
- **Day 7：扩展**。决定是否扩到更多国家、SKU 或广告系列。

## 人类主管验收

老板 / 投放负责人每天只看 5 件事：

1. 今天广告浪费 Top 20 是否有证据。
2. 每项属于搜索词、SKU/Feed、落地页、库存还是埋点问题。
3. 每项该由投手、运营、商品、技术还是老板确认。
4. 哪些动作低风险，可以人工执行；哪些要继续观察。
5. 昨天处理过的项是否复查，是否减少浪费信号。

## 不能自动化的事

- 不能自动调预算、出价、tROAS、tCPA、地域、设备、广告排期。
- 不能自动创建、暂停、删除广告系列、广告组、资产组、商品组或 listing group。
- 不能自动添加/删除否定关键词；只能生成候选清单，投手人工确认。
- 不能自动改 Merchant Center Feed、商品标题、价格、库存、图片、GTIN、品牌、类目。
- 不能自动上传转化、改归因设置、改 GA4/Tag Manager/Pixels/PostHog 事件定义。
- 不能处理客户个人信息、订单明细、支付信息、账号密码、OAuth token、API secret。
- 不能承诺节省多少广告费，不能承诺提升 ROAS；只做诊断和复盘。

## 相关 Skill / repo / 工作流

| 工具 / 工作流 | 公共链接 | 可用于 | 风险边界 |
|---|---|---|---|
| Google Ads API / Shopping Ads / Reporting | https://developers.google.com/google-ads/api/docs/reporting/overview | 读取广告账户报表、商品维度和 PMax/Shopping 表现 | 只读 scope/最小权限；不调用 mutate 改账户 |
| Google Ads Scripts Reporting | https://developers.google.com/google-ads/scripts/docs/features/reports | 定时报表、轻量巡检脚本 | Scripts 只用于报表，不执行预算/出价/否词修改 |
| Merchant Center Product data spec / Merchant API Reports | https://support.google.com/merchants/answer/7052112?hl=en | 关联商品状态、价格、库存、链接、图片和 Feed 问题 | 不自动改 Feed，不凭空补 GTIN/brand |
| GA4 Data API / Matomo / Plausible / PostHog | https://developers.google.com/analytics/devguides/reporting/data/v1 | 读取页面、事件、加购、结账、购买等聚合数据 | 不导出用户级隐私数据；口径需人工确认 |
| DuckDB / Great Expectations / Grafana / n8n | https://github.com/duckdb/duckdb | 本地合并 CSV、检查数据质量、做看板、定时提醒 | 本地文件脱敏；n8n 节点权限最小化；看板不公开敏感账号信息 |
| 付费与自然词重叠机会分析 Skill | https://support.google.com/google-ads/answer/6325025 | 交叉付费词和自然词，发现重复花钱或内容承接缺口 | 不做排名承诺；预算调整人工确认 |

## 发布边界

本页只作为群内预览和 GitHub 归档素材；外部发布仍需郝敬确认。
