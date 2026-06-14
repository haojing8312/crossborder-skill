# AI 海外社媒内容助理 / AI Overseas Social Content Assistant

> 帮跨境公司把产品资料、FAQ、展会照片、工厂视频，变成海外客户看得懂、销售敢转发、老板能验收的社媒内容日历。

## 适合谁

- LinkedIn、Facebook、Instagram、TikTok、YouTube Shorts、X 等账号开了，但长期断更的外贸公司。
- 有产品目录、工厂照片、展会素材、FAQ，却没有英文内容运营专人的跨境团队。
- B2B 工厂、外贸贸易商、DTC 品牌、消费品团队、工业品公司。
- 老板希望先用 1 周小试点验证内容产能，不想一开始就招完整海外社媒团队。

## 老板痛点

产品资料在文件夹里，展会照片在业务员手机里，FAQ 在销售聊天记录里。客户点进海外账号，只看到几条旧动态，很难判断这家公司是否还在认真经营。销售跟进客户时，也缺少一批可以随手转发的英文内容。

## 这个数字员工每天干什么

**输入：** 产品资料、FAQ、目标客户、目标市场、官网链接、展会素材、工厂图、产品短视频、品牌语气、禁用信息、平台规则。

**输出：** 周度内容日历、LinkedIn/Facebook/Instagram/X 文案、TikTok/YouTube Shorts 口播脚本、Hashtag、素材需求清单、发布前审核表、周度复盘。

## 可接手任务

1. 把一个产品系列拆成 5–10 条海外社媒内容。
2. 制作 1 周内容日历：平台、主题、发布时间、素材、审核人。
3. 把同一主题改写成 LinkedIn、Facebook、Instagram、X、短视频脚本版本。
4. 把展会照片和视频拆成展前预告、现场更新、展后回顾、产品亮点。
5. 把销售 FAQ 变成客户教育帖、短视频脚本、轮播图文案。
6. 做发布前风险检查：客户隐私、价格合同、认证声明、夸大承诺、平台敏感内容。
7. 每周记录互动、评论、私信、点击和询盘线索，给下周选题建议。
8. 沉淀品牌语气库：常用表达、禁用表达、CTA、Hashtag。

## 能力模块

| 模块 | 老板听得懂的作用 | 公开工具 / 工作流 | 风险边界 |
|---|---|---|---|
| 资料拆内容 | 把产品、FAQ、展会图拆成每天能发的主题 | ChatGPT / Claude / Gemini + 产品资料表 | 不编造客户、价格、认证、测试数据 |
| 多平台改写 | 同一件事，LinkedIn 写专业，Instagram 写短，TikTok 写口播 | 平台文案模板、品牌语气库 | 英文需人工确认，不发中式硬广 |
| 排期协作 | 让内容先审核再排队发布 | Postiz、Mixpost、Buffer、Meta Business Suite | OAuth/API Key 不进提示词；高风险内容不自动发 |
| 自动化提醒 | 内容表到审核提醒、发布提醒、周报 | n8n、Google Sheets、Notion/Airtable | 流程必须有人审节点，防重复发布 |
| 视觉脚本 | 把帖文变成轮播图、短视频脚本 | Canva、Adobe Express、CapCut | 模板、音乐、图片版权要确认；不伪造客户现场 |
| 安全检查 | 先拦客户隐私、合同报价、夸大承诺 | Meta/LinkedIn/TikTok/YouTube 社区规则 | AI 只提醒，最终发布由负责人确认 |

## 工作机理

```text
产品资料 / FAQ / 展会照片 / 工厂视频 / 目标市场 / 品牌语气
→ 脱敏：删客户名、报价、合同、未公开订单、个人信息
→ 抽主题：产品亮点、应用场景、工厂实力、FAQ、展会、质检
→ 改平台：LinkedIn / Facebook / Instagram / X / TikTok 脚本
→ 查风险：夸大、认证、客户隐私、素材版权、平台规则
→ 人工审核：老板或销售负责人确认事实和语气
→ 排期发布：Postiz / Mixpost / Buffer / Meta Business Suite
→ 周复盘：互动、评论、私信、询盘线索、下周选题
```

## 可复制提示词

```text
你是 B2B 海外社媒内容助理。请根据以下产品资料，为一家跨境贸易公司生成 5 条 LinkedIn 英文帖文。
要求：面向海外采购经理、批发商、品牌商或工程客户；语气专业可信，不使用 best price / top quality / world-leading 这类空泛表达；每条包含标题、正文、CTA、3-5 个 Hashtag；5 条角度不同：产品亮点、应用场景、质量控制、常见问题、工厂能力；不要编造认证、客户名称、价格、交期或测试数据；资料不足请标注“需要人工补充”。
产品资料：{{product_info}}
目标市场：{{market}}
品牌语气：{{brand_tone}}
```

```text
你是 AI Overseas Social Content Assistant。请把下面这条英文社媒初稿改写成 5 个版本：LinkedIn 专业版、Facebook 亲和版、Instagram 配图 caption、X 280 字符以内短观点、TikTok/YouTube Shorts 30 秒口播脚本。
限制：不要添加未提供的数据；不要提客户名、价格、合同、未公开项目；不要使用夸大或绝对化表达；如果原文太像硬广，请改得更像客户教育内容。
原始内容：{{draft}}
目标客户：{{buyer_type}}
```

```text
你是跨境贸易公司的海外社媒内容排期助理。请根据以下信息生成 1 周内容日历。
输出 Markdown 表格：日期｜平台｜内容主题｜内容角度｜所需素材｜文案方向｜CTA｜审核注意事项。
要求：不要每天都发硬广；至少包含 1 条 FAQ、1 条应用场景、1 条信任背书；标注哪些内容需要人工确认事实；不要使用客户隐私、报价、合同、未授权 Logo。
公司行业：{{industry}}
本周重点产品：{{product}}
目标市场：{{market}}
可用素材：{{assets}}
发布平台：{{platforms}}
```

```text
你是海外社媒内容合规检查助手。请检查以下待发布内容是否适合发到 LinkedIn / Facebook / Instagram / X。
请输出：是否建议发布；风险类型（客户隐私、价格合同、夸大宣传、认证声明、医疗安全承诺、侵权素材、平台敏感内容、语气不当）；具体问题句子；修改建议；修改后的安全版本。
待检查内容：{{copy}}
配图/视频说明：{{asset_description}}
```

## 人类主管验收

- 老板看：本周发了什么、是否围绕重点产品、有没有互动/私信/询盘线索。
- 销售看：文案是否能转给客户，是否说清应用场景和常见问题。
- 市场负责人看：平台风格、品牌语气、素材质量、内容节奏是否合适。
- 合规/业务负责人看：客户名、报价、合同、认证、测试数据、平台规则是否安全。
- 每周抽查 5 条 AI 生成稿，记录人工修改比例；高风险内容不得进入自动发布队列。

## 不能自动化的事

- 不自动发布未经审核的内容。
- 不编造客户案例、订单金额、检测报告、认证、交期、产能、合作品牌。
- 不使用客户头像、名片、邮箱、电话、合同、报价单、会议截图、未授权 Logo。
- 不自动回复高风险评论或私信；报价、投诉、售后争议、合作条款交由人工处理。
- 不复制竞品文案、图片、视频和设计，只参考公开内容结构。
- 不替老板决定是否进入某个国家市场、展示某类客户或公开某条产品线。

## 第一周试点

- **Day 1：** 选 1 个重点产品系列，整理 10 个卖点、20 个 FAQ、30 张可公开产品/工厂/展会素材；列出客户名、报价、合同、未公开订单、敏感图片等禁用清单。
- **Day 2：** 生成 10 个内容主题、5 条 LinkedIn 文案、5 条 Facebook/Instagram 改写、3 条短视频脚本。
- **Day 3：** 排出 5 个工作日内容日历：平台、时间、素材、审核人、风险点。
- **Day 4：** 老板或销售负责人确认产品事实、语气和敏感信息；修改可发布版本。
- **Day 5：** 用 Canva / CapCut / Adobe Express 做图文或短视频初稿；用 Postiz / Mixpost / Buffer / Meta Business Suite 排期。
- **Day 6–7：** 记录发布链接、互动、评论、私信、点击和询盘线索；决定第二周继续哪些栏目。

## 相关 Skill / 工具栈

- **Postiz / Postiz Agent**：适合把内容草稿、媒体和多平台排期放进一个社媒工作流；风险是不能无人审核自动发布，OAuth/API Key 必须安全管理。https://github.com/gitroomhq/postiz-app ｜ https://github.com/gitroomhq/postiz-agent
- **Mixpost**：适合多账号内容日历、队列、团队权限和分析；风险是平台 API 授权和数据权限需要维护。https://github.com/inovector/mixpost
- **n8n**：适合把内容表、AI 生成、审核提醒、发布提醒和周报串成低代码流程；风险是必须保留人工审核节点，不能明文保存密码、客户隐私、报价合同。https://github.com/n8n-io/n8n ｜ https://n8n.io/workflows
- **LangChain / LangGraph**：适合有开发能力的团队定制“资料读取→主题生成→多平台改写→风险检查→输出日历”的可控工作流；风险是模型仍会幻觉，需事实校验。https://github.com/langchain-ai/langchain ｜ https://github.com/langchain-ai/langgraph
- **Canva / Adobe Express / CapCut**：适合把脚本变成轮播图和短视频初稿；风险是模板、音乐、图片版权要确认，不伪造客户现场、认证或工厂规模。https://www.canva.com/ ｜ https://www.adobe.com/express/ ｜ https://www.capcut.com/

## 公开资料

- Buffer：Social Media Content Calendar https://buffer.com/library/social-media-calendar/
- Hootsuite：Social Media Content Calendar https://blog.hootsuite.com/social-media-content-calendar/
- LinkedIn Marketing Blog：How to create a social media content calendar https://business.linkedin.com/marketing-solutions/blog/best-practices--content-marketing/2022/how-to-create-a-social-media-content-calendar
- Postiz App https://github.com/gitroomhq/postiz-app
- Postiz Agent https://github.com/gitroomhq/postiz-agent
- Mixpost https://github.com/inovector/mixpost
- n8n https://github.com/n8n-io/n8n
- LangChain https://github.com/langchain-ai/langchain
- LangGraph https://github.com/langchain-ai/langgraph
- Meta Community Standards https://transparency.meta.com/policies/community-standards/
- LinkedIn Professional Community Policies https://www.linkedin.com/legal/professional-community-policies
- TikTok Community Guidelines https://www.tiktok.com/community-guidelines
- YouTube Community Guidelines https://www.youtube.com/howyoutubeworks/policies/community-guidelines/
