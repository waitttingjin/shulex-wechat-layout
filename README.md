# 📰 Shulex WeChat Layout

**Shulex 公众号 HTML 排版 Skill** — 面向 AI Agent 的微信公众号排版技能包，输入文章正文即可生成可直接粘贴进微信编辑器的 `wechat_ready.html`，视觉风格完全遵循 Shulex 品牌规范。

---

## 预览

### 亮点卡片（挑战 → 成果）

```
┌─────────────────────────────────┐
│  挑战 01                         │
│  物流查询工单量大，人工处理耗时    │
│                                  │
│  成果                            │
│  60 秒内完成响应，人工介入率从    │
│  接近 100% 降至不足 20%          │
└─────────────────────────────────┘
```

> 浅蓝底色 `#eef4fd`，成果数字加粗，主色蓝 `#4c6fff` 标注标签。

---

### 对话气泡

```
                  [客户] 我的包裹什么时候到？  ▶
◀ [AI] 您好！已查询到您的订单，预计明日 18:00 前送达……
```

> 客户消息右对齐灰底，AI 回复左对齐白底蓝边，直观展示 AI 客服效果。

---

### 引语块

```
┌──│ 「引入 Shulex AI 客服员工之后，我们的物流工单
   │ 基本不需要人工了，客服可以专注处理复杂问题。」
   │ — 王总，运营负责人
```

---

## 文件说明

| 文件 | 说明 |
|------|------|
| `SKILL.md` | Agent Skill 核心指令文件，包含设计系统、HTML 模块、Workflow、Checklist |

---

## 核心色板

```css
--brand:        #4c6fff;   /* 主色蓝 */
--brand-light:  #eef4fd;   /* 亮点卡片底色 */
--brand-dim:    #dde4ef;   /* 气泡边框 */
--bg-quote:     #f5f5f5;   /* 引语块底色 */
--bg-case:      #fff8ec;   /* 案例块底色 */
```

---

## 支持的 HTML 模块

| 模块 | 说明 |
|------|------|
| 小标题 | 左侧蓝色竖线，17px 加粗 |
| 正文段落 | 14px，行高 2 |
| 亮点卡片 | 三条挑战→成果，数字加粗 |
| 引语块 | 客户原话 + 姓名职位 |
| 对话气泡 | 客户右灰 / AI 左白蓝边 |
| 内嵌标签 | 浅蓝底圆角标签 |
| CTA 区块 | 蓝底白字，触发词明确 |
| 固定尾部 | 点赞/在看/转发/星标引导语 |

---

## 使用方式

在对话中调用此 Skill：

```
使用 shulex-wechat-layout，帮我排版这篇文章：[文章正文]
客户名：遨森，亮点数据：AI回复率11.61%、物流查询60秒响应、人工介入率降至20%
```

Agent 会自动生成并写入：

```
~/ag/MyContentFactory/Assets/YYYY-MM-DD_遨森/wechat_ready.html
```

---

## 输出规范

```
MyContentFactory/
  Assets/
    YYYY-MM-DD_客户名/
      wechat_ready.html     ← 本 Skill 输出目标
```

---

## 相关 Skill

| Skill | 说明 |
|-------|------|
| [shulex-wechat-writer](https://github.com/waitttingjin/shulex-wechat-writer) | 公众号文章写作规范（选题 / 模板 / 质检） |
| [shulex-poster-designer](https://github.com/waitttingjin/shulex-poster-designer) | 案例海报生成（1080×607px 横版） |
| [shulex-case-production](https://github.com/waitttingjin/shulex-case-production) | 客户案例全流程生产 Pipeline |
