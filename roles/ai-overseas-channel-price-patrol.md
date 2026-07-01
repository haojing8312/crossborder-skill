# AI 海外渠道价格巡检员

> 每天盯重点 SKU 的海外公开价、促销、运费和币种，让老板早上先知道谁把价格卖穿了。

## 这个岗位适合谁

- 有 Amazon、eBay、TikTok Shop、独立站、经销商官网等多渠道销售的出海品牌。
- 有 MAP / 最低售价 / 促销期管理要求，但渠道经理每天查不过来的团队。
- SKU 数量不小，老板担心海外乱价、过期促销和汇率运费吞掉毛利的公司。
- 能提供公开 URL、授权 API、商家后台导出或人工样本的团队。

## 老板痛点

很多老板不是没价盘，是没人每天盯。一个经销商把价格卖穿，平台上挂着过期 sale，广告还在往那个页面导流。等销售月底汇报，利润已经被吃掉，渠道关系也开始变麻烦。

老板真正想知道的只有几件事：哪个 SKU、哪个渠道、便宜了多少、有没有截图、谁今天要去复核。

## 岗位职责

这个数字员工每天接收 SKU 清单、官方价 / MAP、促销期、公开渠道 URL、授权 API 或商家导出数据，抽取公开价格、币种、运费、库存和促销状态。它把这些信息和价盘规则做对比，标出低于 MAP、促销过期、价差异常和毛利风险，并生成可复核的截图证据包与渠道经理工单。

## 可接手任务

1. 每天巡检 20–200 个重点 SKU 的公开渠道价格。
2. 抽取独立站 JSON-LD、Shopify 商品 JSON 或授权 API 里的公开报价字段。
3. 对比官方价、MAP、促销期、运费、币种和毛利阈值。
4. 标出过期 sale、异常 coupon、页面价与 feed 价不一致。
5. 保存 URL、截图、抓取时间、原始字段和规则命中原因。
6. 生成渠道经理可分派工单。
7. 每周汇总重复异常 SKU、渠道和待人工处理事项。
8. 记录误报原因，持续修正 SKU 匹配和页面字段规则。

## 能力模块

| 模块 | 具体动作 | 公开工具 / Skill 线索 |
|---|---|---|
| 价格采集 | 从公开页面、结构化数据、授权 API 抽价格、币种、促销、运费 | Playwright、Scrapy、Crawlee、Shopify Ajax API、Schema.org Offer |
| 截图取证 | 保存页面截图、URL、抓取时间、原始字段 | Playwright、Selenium、browser-use |
| 规则对比 | 和 MAP、促销期、价差阈值、毛利底线做对比 | DuckDB、Python、NocoDB / Airtable / 飞书表格 |
| 异常工单 | 把可疑记录变成渠道经理能处理的工单 | n8n、Windmill、飞书 / Slack webhook |
| 人工复核 | 标注真异常 / 误报，修正字段和规则 | NocoDB、Airtable、GitHub Issues |

## 工作机理

SKU 基线与授权价盘 → 公开页面 / 授权 API / 人工样本采集 → 提取价格、币种、运费、促销和库存 → 对比 MAP、促销期、价差阈值和毛利底线 → 保存截图与原始字段 → 生成异常清单和工单 → 渠道经理 / 老板人工复核后决定处理动作。

## 可复制任务指令

### 1. 建立巡检基线

```text
你是跨境电商价格巡检分析员。请根据下面的 SKU 清单和渠道资料，生成价格巡检基线表。只使用我提供的公开 URL、授权 API 字段或企业内部价盘，不补充未知数据。输出字段：sku、brand、channel、country、currency、official_url、channel_url、msrp、map_price、promo_start、promo_end、cost_basis、shipping_rule、platform_fee_rule、gross_margin_floor、risk_notes。如果缺少关键字段，用 MISSING 标记，并说明需要谁补充。不要输出客户隐私、订单信息、私人沟通内容或非授权数据。
输入：[粘贴 SKU 清单、价盘规则、渠道 URL]
```

### 2. 抽取公开价格字段

```text
你是商品价格字段抽取器。请从以下公开网页文本、JSON-LD、Shopify Ajax JSON 或授权 API 响应中抽取价格信息。不要猜测；不要从评论、广告文案或相似商品中取价；如果无法确认 SKU 匹配，返回 uncertain。只抽取商品价格、币种、促销、运费、库存、有效期等字段，不抽取个人信息、订单信息或非公开内容。输出 JSON 数组，字段为：sku_or_handle、title、url、currency、list_price、sale_price、coupon_or_discount、shipping_price、availability、promo_valid_through、extracted_from_field、evidence_quote、confidence。
原始片段：[粘贴 HTML / JSON / API 响应片段]
```

### 3. 识别乱价与毛利风险

```text
你是渠道价格风控分析员。请把“采集价格表”与“SKU 基线表”对比，找出异常。异常规则：折后价低于 MAP；促销结束日期已过但页面仍展示 sale / coupon；同一 SKU 同国家不同渠道到手价差超过 {threshold_percent}%；按给定汇率、运费、平台费估算后毛利率低于 {margin_floor}%；页面币种或商品型号与基线不一致。输出字段：severity、sku、channel、url、observed_price、expected_rule、delta、reason、evidence、recommended_owner、recommended_next_step。只引用表中事实；不做法律定性；无法确认的异常标记为“待复核”。
SKU 基线表：[粘贴]
采集价格表：[粘贴]
汇率 / 费用规则：[粘贴]
```

### 4. 生成可复核工单

```text
你是价格异常工单助理。请把下面的异常记录改写成可分派给渠道经理的工单。不做法律定性，只写“疑似 / 需复核”；必须包含 URL、截图文件名、抓取时间、规则命中原因、建议动作；不包含客户个人信息、订单信息、登录后数据或私人沟通内容；语气专业、克制。最后列出“需要人工确认的问题”。
异常记录：[粘贴异常表行]
```

## 人类主管验收

- 老板每天看：高风险异常数、涉及 SKU、渠道、差价幅度、建议负责人。
- 渠道经理抽查：截图是否对应正确 SKU、价格是否真实存在、促销期是否已结束。
- 运营抽查：页面币种、运费、优惠券、库存状态是否被正确识别。
- 财务 / 商务抽查：毛利阈值、汇率和平台费规则是否是最新版本。
- 一周复盘：误报率、有效异常数、处理时长、是否值得扩大 SKU 范围。

## 不能自动化的事

- 不登录平台后台、买家账号、经销商门户抓非公开数据，除非企业明确授权且符合平台规则。
- 不绕过验证码、反爬、IP 封禁、robots/TOS 限制。
- 不采集客户姓名、地址、订单、付款信息、私聊记录、合同价、私有报价。
- 不自动下单、改价、关广告、处罚经销商、发送律师函或对外承诺。
- 不做法律结论，只输出“疑似异常，建议人工复核”。

## 第一周试点

- Day 1：选 20–50 个高销量 / 高风险 SKU，补齐官方价、MAP、促销期、渠道 URL、国家和币种。
- Day 2：给每个来源标注“公开 / 授权 / 禁止自动化”，marketplace 优先走授权 API 或人工样本。
- Day 3：跑第一轮采集，保存价格、币种、运费、促销、库存、截图和失败原因。
- Day 4：配置异常规则：低于 MAP、促销过期、价差超阈值、毛利低于底线、币种或型号不一致。
- Day 5：渠道经理复核真异常和误报，修正 SKU 匹配、选择器和促销字段。
- Day 6：生成飞书 / Slack / 邮件提醒模板和工单模板。
- Day 7：看采集成功率、有效异常数、误报率和处理时长，再决定是否扩到 100+ SKU。

## 相关 Skill / 工具栈

| 工具 / repo | 地址 | 干什么 | 风险边界 |
|---|---|---|---|
| Playwright | https://github.com/microsoft/playwright | 公开页面截图、DOM 抽取、价格定位 | 只测公开或授权页面 |
| Scrapy | https://github.com/scrapy/scrapy | 批量公开页面巡检、队列、重试、解析 | 遵守 robots/TOS 与频率限制 |
| Crawlee Python | https://github.com/apify/crawlee-python | HTTP/浏览器混合采集公开页面 | 不绕过访问控制 |
| browser-use | https://github.com/browser-use/browser-use | 低频人工授权网页任务原型 | 不用于验证码、登录墙或反爬规避 |
| Shopify Ajax API | https://shopify.dev/docs/api/ajax/reference/product | 抽取公开 Shopify 商品价格字段 | 仅用于公开/授权店铺 |
| Amazon PA-API | https://webservices.amazon.com/paapi5/documentation/ | 授权请求商品报价字段 | 需要申请与合规授权 |

## 安全边界

只用公开页面、公开结构化数据、企业授权 API、官方导出和人工样本；每条异常必须保留 URL、截图、抓取时间和规则命中原因。调价、联系经销商、处罚渠道、法务动作、外部承诺、客户隐私和合同信息都必须人审。
