# 2026-06-17 visual-suite watchdog fix｜AI 展会线索整理员

## 触发原因
03:00 主任务产物为 content PASS / visual partial：只生成并入库 2 张 PNG，缺少 card-03-priority.png、card-04-mechanism-prompt.png、card-05-dashboard-stack.png，final_message.md 也只有 2 行 MEDIA。

## 补救动作
- 读取工作区 visual_prompt.md、role-map-copy.md、short-platform-copy.md、chief_review.md。
- 通过 `hermes -p visual-agent chat --toolsets file,terminal,image_gen,vision` 调用 visual-agent profile 的 image_gen 原生路线补齐 3 张缺图。
- 禁止 HTML / Canvas / PIL / SVG / 程序化截图 / 本地拉伸裁切；PIL 仅用于尺寸校验。
- 将 5 张 PNG 复制到 GitHub repo：`visual/ai-trade-show-lead-organizer/`。
- 更新 `reports/daily-role-map/20260617.md` 为 visual PASS。

## 文件清单
- `visual/ai-trade-show-lead-organizer/card-01-cover.png`｜941×1672｜9:16 等价｜首页封面。
- `visual/ai-trade-show-lead-organizer/card-02-inputs.png`｜941×1672｜9:16 等价｜线索入口。
- `visual/ai-trade-show-lead-organizer/card-03-priority.png`｜941×1672｜9:16 等价｜客户分层。
- `visual/ai-trade-show-lead-organizer/card-04-mechanism-prompt.png`｜941×1672｜9:16 等价｜工作机理 + 提示词。
- `visual/ai-trade-show-lead-organizer/card-05-dashboard-stack.png`｜941×1672｜9:16 等价｜老板看板 + 工具栈 + 边界。

## 总编复核
- 首页结构：PASS。card-01 主标题写清“今天介绍的数字员工是：AI 展会线索整理员”，并包含「岗位职责」「核心技能」「业务作用」。
- 文案通俗度：PASS。上图文案包含名片、扫码表、聊天截图、客户表、报价、样品、销售负责人、50 条线索等跨境业务对象和具体动作。
- 视觉质量：PASS。5 张图主标题可读，底部署名“郝朋友的AI进化论”存在，无明显错字、乱码或遮挡。
- 安全：PASS。未出现真实客户、联系方式、报价、合同、付款条款或未公开信息。

## 结论
watchdog 补救完成：短文案 PASS；5 图 9:16 等价 PASS；首页结构 PASS；GitHub 本期文件已准备 commit/push。外部正式发布仍需郝敬确认。
