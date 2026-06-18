# AI 商品数据 Feed 体检员｜AI Product Feed QA Auditor

## 一句话定位

它每天帮老板检查商品 Feed 里那些会让广告拒登、商品少曝光、价格库存出错的隐形坑。

## 适合谁

- Shopify / WooCommerce / 独立站卖家
- 正在跑 Google Shopping、Meta Catalog Ads、TikTok Shop、Amazon 多渠道商品同步的团队
- SKU 多、变体多、活动价和库存经常变动的跨境团队
- 老板经常听到“平台拒登、商品没展示、广告跑不起来、价格不一致”的公司

## 老板痛点

商品页面明明在独立站能打开，广告平台却拒登；活动价已经设置，渠道里还挂着旧价格；ERP 里有库存，平台目录显示缺货；主图链接坏了，几十个 SKU 一起没展示。

运营看到的是后台报错代码，老板承受的是广告预算花不出去、爆品少曝光、客户问价解释不清。

## 岗位职责

AI 商品数据 Feed 体检员每天接收商品 Feed CSV/XML/JSON、平台诊断导出、广告商品状态、活动排期和只读商品资料。它把商品 ID、标题、描述、图片、价格、币种、库存、品牌、GTIN、MPN、类目、变体关系、配送/税费字段逐项检查，输出问题清单、证据、影响渠道、建议修复和必须人工确认的事项。

它不是自动改价工具，也不是平台申诉机器人。它的价值在于：在广告烧钱和商品同步前，提前把脏字段和高风险 SKU 翻出来。

## 可接手任务

1. 导入商品 Feed CSV/XML/JSON 或平台只读导出。
2. 统一 Shopify、Google Merchant Center、Meta/TikTok Catalog、Amazon listing 的字段映射。
3. 检查必填字段、空值、格式、价格、币种、库存、URL、图片、GTIN/MPN/brand 组合。
4. 检查标题和描述里的过长、关键词堆砌、促销词、全大写、疑似夸大声明。
5. 对比独立站后台、ERP、平台目录和广告账户里的关键字段。
6. 读取平台诊断结果，转成运营工单。
7. 输出高风险 SKU、可直接修字段、需人工确认字段和复查记录。
8. 每周统计问题类型、重复 SKU、修复后通过率。

## 能力模块

| 模块 | 做什么 | 可对应工具 / 工作流 |
|---|---|---|
| 字段完整性检查 | 查商品 ID、标题、图片、价格、库存、品牌、GTIN、MPN、类目等字段 | Frictionless、Great Expectations、Google Sheets、Python |
| 跨平台一致性比对 | 对比 Shopify/ERP/Google/Meta/TikTok/Amazon 的价格、库存、图片、活动字段 | Shopify Product API、Google Merchant API、Meta Catalog API、Amazon SP-API |
| 图片与链接体检 | 检查图片 URL、商品页链接、重定向、失效图、占位图 | Python requests、OpenRefine、n8n |
| 平台诊断分诊 | 把 Merchant Center / Catalog Diagnostics / listing issue 变成工单 | Google Merchant Center、Meta Commerce Manager、TikTok Catalog Diagnostics |
| 人工确认队列 | 把价格、促销、品牌授权、功效声明、类目裁决等高风险项推给人审 | Airtable / Baserow / Notion / Google Sheets |

## 工作机理

```text
商品 Feed / 平台诊断 / 广告商品状态 / 活动排期
        ↓
字段映射：SKU、标题、描述、价格、库存、图片、品牌、GTIN、类目、变体
        ↓
规则体检：缺字段、格式错、URL 失效、价格币种冲突、库存不一致
        ↓
AI 软审：标题描述风险、类目疑似错配、平台拒登原因翻译
        ↓
输出：SKU 问题表、影响渠道、建议修复、责任人、人工确认项
        ↓
人类主管：确认价格/库存/品牌/合规/账号操作，批准后再修改
```

## 可复制提示词

### 1. Feed 字段体检

```text
你是商品 Feed QA 审核员。请只基于我提供的 CSV 字段检查，不要编造缺失数据。

输出表格：SKU/id、问题字段、问题类型、严重级别（阻断/高/中/低）、证据、建议修复、是否需要人工确认。

重点检查：title、description、link、image_link、price、currency、availability、brand、gtin、mpn、condition、category、shipping、tax。

规则：不得建议自动改价或自动改库存；GTIN/MPN 不能凭空补；涉及品牌、功效、合规声明必须标记人工确认。

数据如下：
[粘贴样本]
```

### 2. 平台拒登原因分诊

```text
请把以下 Google Merchant Center / Meta Catalog / TikTok Catalog 诊断结果转成运营可执行工单。

输出：SKU、拒登原因、对应字段、可能根因、修复建议、需要谁确认、优先级。

边界：不要提交申诉，不要改 Feed，不要承诺价格/库存/配送时效；只做诊断和建议。

诊断数据：
[粘贴导出]
```

### 3. 标题与描述初审

```text
请审核以下商品标题和描述是否适合商品目录和购物广告。

检查：过长、关键词堆砌、促销词、全大写、属性缺失、品牌/型号不清、疑似夸大功效或敏感声明。

输出：SKU、当前标题、问题、建议标题方向、必须人工确认的风险点。

不要虚构认证、功效、材质、品牌授权或 GTIN。
```

### 4. 类目映射风险检查

```text
请对比 source_category、google_product_category、meta_category、tiktok_category、amazon_product_type。

找出疑似错配或过粗的类目。

输出：SKU、当前类目组合、疑似问题、建议检查方向、是否需要人工确认。

只提出检查建议，不做最终类目裁决；受监管商品必须标为人工确认。
```

## 第一周试点

- **Day 1：选范围**。选 1 个站点 + 1–2 个广告/销售平台，导出 50–200 个 SKU，不含客户或订单数据。
- **Day 2：建字段表**。统一 SKU、标题、描述、图片、价格、币种、库存、品牌、GTIN、MPN、类目、配送/税费字段。
- **Day 3：规则体检**。查空值、格式、URL、价格币种、库存、图片、GTIN/MPN/brand 组合。
- **Day 4：AI 软审**。检查标题、描述、类目和平台拒登原因，生成建议但不自动写回。
- **Day 5：业务复盘**。老板/运营确认 Top 20 高风险项，统计问题率和预计节省时间。
- **Day 6：修低风险项**。运营先修明确缺字段、失效链接、格式错误。
- **Day 7：复查与扩展**。复查已改 SKU，决定是否扩大到全量 SKU 和每日巡检。

## 人类主管验收

老板 / 运营负责人每天只看 5 件事：

1. 今天多少个 SKU 可正常投放，多少个被阻断。
2. 哪些字段影响最大：图片、价格、库存、GTIN、类目、变体、配送。
3. Top 20 高风险 SKU 的证据和影响渠道。
4. 哪些问题运营可以当天修，哪些必须老板、品牌、合规或财务确认。
5. 昨天修过的 SKU 今天是否通过复查。

## 不能自动化的事

- 不能自动改价、改库存、发布商品、删除商品、暂停账号或提交申诉。
- 不能替商家承诺价格、折扣、库存、配送时效、税费。
- 不能处理客户个人信息、订单数据、支付信息、账号密码、API secret。
- 不能凭空补 GTIN、MPN、brand、认证、材质、功效或品牌授权。
- 医疗、儿童、食品、化妆品、电子烟、品牌授权、知识产权等合规判断必须人审。

## 相关 Skill / repo / 工作流

| 工具 / 工作流 | 公共链接 | 可用于 | 风险边界 |
|---|---|---|---|
| Google Merchant API + Google API Python Client | https://developers.google.com/merchant/api/guides/products/overview / https://github.com/googleapis/google-api-python-client | 只读拉取商品、状态、报告，做 GMC 体检 | 使用只读权限；不插入、更新、删除商品；OAuth secret 不入库 |
| Meta Catalog API + Facebook Business SDK | https://developers.facebook.com/docs/marketing-api/catalog/reference/ / https://github.com/facebook/facebook-python-business-sdk | 检查 Catalog 字段、商品状态、目录数据结构 | 不改广告预算、不改目录、不上传客户数据；token 放密钥管理器 |
| TikTok Catalog Diagnostics | https://ads.tiktok.com/help/article/catalog-diagnostics?lang=en | 按 TikTok 参数和诊断结果分诊商品问题 | 不自动同步，不自动改 pixel/catalog 设置 |
| Shopify Product API / Shopify Python API | https://shopify.dev/docs/api/admin-graphql/latest/objects/Product / https://github.com/Shopify/shopify_python_api | 从 Shopify 源头核对标题、价格、库存、图片、变体 | 优先 CSV 或只读 scope；不写库存、不改价格、不发布商品 |
| Amazon SP-API Listings Items + samples | https://developer-docs.amazon.com/sp-api/docs/listings-items-api-v2021-08-01-reference / https://github.com/amzn/selling-partner-api-samples | 读取 Amazon listing 状态、字段、问题提示 | 不调用 pricing/feeds 写入接口；不处理买家信息；不提交合规申诉 |
| Great Expectations / Frictionless / OpenRefine / n8n | https://github.com/great-expectations/great_expectations / https://github.com/frictionlessdata/frictionless-py / https://github.com/OpenRefine/OpenRefine / https://github.com/n8n-io/n8n | 离线 CSV 规则校验、清洗、异常推送和巡检流程 | 不保存密钥，不把敏感商品成本/供应商报价推到公开仓库 |

## 发布边界

本页只作为群内预览和 GitHub 归档素材；外部发布仍需郝敬确认。
