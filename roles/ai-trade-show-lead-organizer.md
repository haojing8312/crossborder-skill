# AI 展会线索整理员 / AI Trade Show Lead Organizer

> 给跨境老板看的岗位地图：展会结束后，把名片、扫码表、聊天截图和销售备注整理成当天能跟进的客户表。

## 这个岗位解决什么问题

展会结束后，最容易丢的是黄金 48 小时。名片在行李箱里，聊天截图在销售手机里，扫码表在后台，老板开会只能问“这个客户谁跟了”。AI 展会线索整理员负责把这些散乱资料变成可分配、可检查、可复盘的销售行动清单。

## 适合谁

- 参加广交会、海外展会、行业展的 B2B 外贸公司。
- 展会现场有多名销售接待客户，展后需要统一跟进的团队。
- 老板想知道哪些客户当天要报价，哪些要寄样，哪些只适合先养着。

## 每天接什么输入，交什么结果

| 输入 | 处理 | 输出 |
|---|---|---|
| 名片照片、扫码表、展位照片、询盘表 | OCR/转写、字段抽取 | 结构化客户表 |
| WhatsApp、邮件、LinkedIn 对话摘要 | 提取国家、产品、需求、下一步 | 线索档案 |
| 销售现场备注 | 判断意向强弱、缺失字段 | A/B/C/D 分层 |
| CRM/表格跟进状态 | 找出超时未跟和信息缺口 | 老板日报、销售待办 |

## 可接手任务

1. 名片、胸牌、询盘表 OCR 入库。
2. 把聊天截图和销售备注整理成线索字段。
3. 合并重复客户，避免多个销售重复跟。
4. 给客户贴标签：A 需报价、B 需样品、C 需资料、D 先观察。
5. 为 A/B 类客户写英文邮件或 WhatsApp 草稿，等待销售确认。
6. 列出缺失问题：采购量、型号、认证、目的港、决策人。
7. 导入 NocoDB、Twenty CRM、共享表格或现有 CRM。
8. 每天生成老板日报：要报、已报、等回复、超时未跟。

## 能力模块

| 模块 | 对应工具/Skill | 老板能看到的结果 |
|---|---|---|
| 图片文字识别 | PaddleOCR、Tesseract、EasyOCR | 名片和表单变成可筛选字段 |
| 表格清洗 | OpenRefine | 国家、电话、公司名、重复客户被整理出来 |
| 线索池 | NocoDB、Twenty CRM | 每条客户有负责人、状态和下一步 |
| 工作流 | n8n | 上传、识别、整理、提醒、日报连成流程 |
| 文档转写 | MarkItDown、Docling | PDF、Office、图片说明进入统一资料池 |

## 工作机理

上传资料 → OCR/转写抽字段 → 去重合并 → A/B/C/D 分层 → 写下一步动作 → 导入线索池 → 销售确认外发 → 老板看日报。

## 可复制任务提示词

### 1. 名片 / 询盘表结构化
```text
你是外贸展会线索整理员。请从以下 OCR 文本中抽取线索字段，输出 JSON 数组。
字段：company_name, contact_name, title, country_or_region, email, phone, whatsapp, website, product_interest, quantity, certification_need, delivery_time, source_file, missing_fields, confidence_note。
规则：不要编造缺失信息，缺失写 null；邮箱或电话识别可疑时标注“需人工核对”；多个联系人拆成多条记录；只做整理，不写营销话术。
OCR 文本如下：<<<粘贴 OCR 文本>>>
```

### 2. 展会线索去重合并
```text
你是 CRM 数据清洗助手。请根据 company_name、email domain、phone、website、contact_name 判断以下线索是否重复。
输出三张表：A merge_ready；B review_needed；C unique_records。
合并规则：同一 email domain + 公司名高度相似可合并；只有联系人姓名相同但公司/国家不同不得自动合并；WhatsApp 号码相同但公司名不同列入 review_needed。请保留 source_file，不能删除证据。
数据如下：<<<粘贴 CSV 或 JSON>>>
```

### 3. A/B/C/D 线索评分
```text
你是跨境贸易销售运营分析员。请把以下展会线索分为 A需报价、B需样品、C需资料、D先观察，并说明理由。
输出字段：lead_id, grade, score_reason, next_action, questions_to_ask, owner_suggestion, follow_up_deadline。
限制：不能承诺价格、交期、付款条件；不能生成发送指令，只能给建议。
我司主营产品：<<<填写产品范围>>>
线索数据：<<<粘贴数据>>>
```

### 4. 销售跟进草稿
```text
你是外贸销售助理。请根据以下展会线索，生成一封英文跟进邮件草稿和一条 WhatsApp 草稿。
要求：语气自然、简短；只引用客户已经提到的信息；不得编造报价、认证、库存、交期；结尾提出 2-3 个补充问题。输出前加“需销售确认后发送”。
客户线索：<<<粘贴线索>>>
我司可公开介绍的产品信息：<<<粘贴公开产品信息，不含报价/合同>>>
```

### 5. 老板日报
```text
你是外贸公司销售运营负责人。请基于今天的展会线索表，生成老板可读的一页日报。
必须包含：今日新增线索总数、A/B/C/D 数量；最值得优先跟进的 10 条线索；未跟进超过 24 小时的线索；国家/地区分布和产品兴趣 Top 5；需要老板或销售经理介入的事项。
限制：不要展示完整电话、邮箱；联系方式打码；不要包含报价、合同、客户隐私细节。
数据如下：<<<粘贴脱敏表格>>>
```

## 老板/主管怎么验收

- 老板每天看：要报价客户多少个、已报价多少个、等回复多少个、超过 3 天没人跟的是谁。
- 销售经理抽查：A/B 类线索是否分准，外发草稿是否引用真实信息，是否遗漏关键客户。
- 运营抽查：OCR 字段是否准确，重复客户是否误合并，原始证据是否保留。

## 不能自动化的事

- 不能自动承诺价格、交期、付款条件、合同条款、独家代理和质保。
- 不能自动发送 WhatsApp、邮件、LinkedIn。
- 不能绕过平台规则抓取联系人或批量骚扰。
- 不能把原始客户资料随意上传到未知第三方模型。
- 不能替代 KYC、制裁筛查、法律或合规判断。

## 第一周试点

1. 选 50 条历史展会线索并脱敏，定字段和 A/B/C/D 规则。
2. 用 OCR 处理名片和表单，人工抽查 20 条字段准确率。
3. 清洗国家名、电话格式、重复公司名，输出疑似重复清单。
4. 给线索分层并生成下一步动作，销售经理只抽查 A/B 类。
5. 导入 NocoDB、Twenty 或共享表格，保留原始证据链接和人工复核人。
6. 每天上午 10 点生成老板日报和销售待办。
7. 复盘整理耗时、OCR 可用率、去重准确率、A/B 命中率和销售是否愿意用。

## 公开工具栈

- PaddleOCR：https://github.com/PaddlePaddle/PaddleOCR
- Tesseract：https://github.com/tesseract-ocr/tesseract
- OpenRefine：https://github.com/OpenRefine/OpenRefine
- NocoDB：https://github.com/nocodb/nocodb
- Twenty CRM：https://github.com/twentyhq/twenty
- n8n：https://github.com/n8n-io/n8n
- MarkItDown：https://github.com/microsoft/markitdown
- Docling：https://github.com/docling-project/docling

## 安全说明

本岗位只建议处理授权资料和脱敏样本。涉及客户联系方式、报价、合同、付款、制裁筛查和合规判断时必须人工复核。外部发布和商业承诺仍需负责人确认。