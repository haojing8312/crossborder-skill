# 20260710 visual suite watchdog fix｜AI 产品认证到期提醒员

## 事故/缺口

04:10 watchdog 检查发现主任务工作区已有 5 张 9:16 图片和短文案，但以下验收件缺失：

- `review/chief_review.md` 未生成。
- `final_message.md` 未生成，导致 Feishu delivery 缺少 `配套文案：` 与 `MEDIA:/...`。
- GitHub repo 缺少 `reports/daily-role-map/20260710.md`。
- GitHub repo 缺少 `visual/ai-product-certification-expiry-reminder/` 5 张 PNG。

## 修复动作

- 补写 `review/chief_review.md`，明确文案通俗度、去 AI 味、首页结构、5 图 9:16、署名、错字/遮挡复核均 PASS。
- 补写 `final_message.md`，包含短文案正文和 5 行 `MEDIA:/...`。
- 复制 5 张视觉图到 GitHub repo：`visual/ai-product-certification-expiry-reminder/`。
- 补写本报告与 `reports/daily-role-map/20260710.md`。

## 校验结果

- `card-01-homepage.png`：1080×1920，9:16；系列首页 / 今日岗位封面 PASS；含岗位职责、核心技能、业务作用。
- `card-02-painpoints.png`：1080×1920，9:16；痛点场景 PASS。
- `card-03-before-after.png`：1080×1920，9:16；旧方法 / 数字员工对比 PASS。
- `card-04-workflow-prompt.png`：1080×1920，9:16；工作流 / 提示词 PASS。
- `card-05-toolstack-pilot.png`：1080×1920，9:16；工具栈 / 第一周试点 PASS。

## 状态

watchdog fix PASS。本期文件准备 commit/push。
