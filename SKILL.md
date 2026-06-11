---
name: shulex-wechat-layout
description: Use when generating WeChat Official Account HTML for Shulex articles, case studies, or activity recaps — including body layout, highlight cards, dialogue bubbles, quote blocks, and CTA sections. Triggers on requests like "帮我排版", "生成微信HTML", "公众号排版", "wechat_ready.html".
---

# Shulex 公众号 HTML 排版

## 🎯 Target

输出一个可直接粘贴进微信公众号编辑器的 `wechat_ready.html`，视觉风格统一遵循 Shulex 品牌色系，兼容微信内置渲染器。

---

## ✅ Checklist

- [ ] 整体字号 15px，行高 1.9，段间距 24px，主色蓝 `#4c6fff`
- [ ] 小标题：左侧竖线 5px、品牌蓝，文字 18px 加粗、深色 `#1a1a1a`，非品牌蓝
- [ ] 关键词/链接：品牌蓝色字 + 「」包裹 + 下划线，不用背景色块
- [ ] 亮点卡片：浅蓝底 `#eef4fd`，挑战/成果双行结构，成果数字加粗，**容器必须用 `<span style="display:block">` 而非 `<div>`**
- [ ] 引语块：无背景，左侧竖线，斜体，客户姓名+职位
- [ ] 对话气泡：客户右对齐灰底，AI 左对齐白底蓝边
- [ ] CTA 区块：蓝底 `#4c6fff` 白字，触发词明确
- [ ] 文件输出路径：`MyContentFactory/Assets/YYYY-MM-DD_客户名/wechat_ready.html`

---

## 📥 Input Contract

- `article_md` (Required)：文章正文，Markdown 或纯文本
- `client_name` (Required)：客户名称，用于文件命名和内容植入
- `highlights` (Optional)：3条挑战→成果数据，默认从正文提取
- `quote` (Optional)：客户引语原话 + 说话人姓名职位
- `dialogue` (Optional)：对话示例，客户消息+AI回复对，用于展示 AI 效果
- `cta_keyword` (Optional)：CTA 触发词，默认「诊断」
- `output_date` (Optional)：输出日期，默认今日，格式 YYYY-MM-DD

---

## 🔁 Workflow

### Step 1：解析内容

从输入中提取：
1. 文章各 Section（标题、正文段落）
2. 亮点数据（挑战01/02/03 + 对应成果数字）
3. 客户引语（原话 + 姓名职位）
4. 对话示例（可选）

### Step 2：生成 HTML

按以下模块顺序拼装：

```
[导语段]
[Section 1–N 正文]
[亮点卡片区]（如有 highlights）
[对话气泡区]（如有 dialogue）
[客户引语块]（如有 quote）
[CTA 区块]
[固定尾部]
```

### Step 3：验证 & 输出

- 用 Checklist 逐项核查
- 如有缺失模块，补全后重新生成
- 将文件写入 `~/ag/MyContentFactory/Assets/<output_date>_<client_name>/wechat_ready.html`

---

## ⚙️ Side Effects

- 创建目录 `~/ag/MyContentFactory/Assets/YYYY-MM-DD_客户名/`
- 写入 `wechat_ready.html`（覆盖同名文件前需确认）

---

## 🚨 Failure Receipt

- 如正文为空：提示「请提供文章正文」，不生成空文件
- 如亮点数据不足 3 条：提示「亮点数据不完整，将以占位符填充，请核对后替换」
- 如输出目录不存在：自动创建，并在响应中告知路径

---

## 🎨 设计系统（Design Tokens）

```css
/* 基础 */
font-size: 15px;
line-height: 1.9;
color: #333;
paragraph-margin: 0 0 24px;

/* 主色 */
--brand:        #4c6fff;
--brand-light:  #eef4fd;
--brand-dim:    #dde4ef;

/* 文字 */
--text-primary:   #1a1a1a;   /* 标题深色 */
--text-body:      #333;      /* 正文 */
--text-secondary: #666;      /* 次要信息 */
--text-muted:     #999;      /* 注释 */
```

---

## 🧩 HTML 模块速查

### 小标题

> 竖线粗（5px）、品牌蓝；文字深色（`#1a1a1a`）不用品牌蓝，字号 18px 加粗。

```html
<p style="font-size:18px;font-weight:700;color:#1a1a1a;
  border-left:5px solid #4c6fff;padding-left:12px;
  margin:32px 0 16px;line-height:1.5;">
  小标题文字
</p>
```

---

### 正文段落

> 段间距 24px，呼吸感强，不要压缩。

```html
<p style="font-size:15px;line-height:1.9;color:#333;margin:0 0 24px;">
  段落内容
</p>
```

---

### 关键词 / 内链高亮

> 用品牌蓝色 + 「」包裹 + 下划线，不用背景色块。适合强调产品名、数据、行动项。

```html
<!-- 关键词高亮 -->
<span style="color:#4c6fff;font-weight:600;">「关键词」</span>

<!-- 可点击链接 -->
<a href="#" style="color:#4c6fff;text-decoration:underline;font-weight:500;">
  「链接文字」
</a>

<!-- 强调数字（加粗不变色） -->
<strong style="color:#1a1a1a;">具体数字</strong>
```

---

### 亮点卡片区（三条挑战→成果）

> ⚠️ **必须用 `<span style="display:block">` 而非 `<div>`**
> 微信编辑器从浏览器粘贴时会剥离 `div` 的 `background-color`，但会保留 `span` 的 `background-color`。

```html
<span style="display:block;background-color:#eef4fd;padding:24px 28px;margin:32px 0;border-radius:6px;">

  <!-- 挑战01 -->
  <p style="margin:0 0 4px;color:#4c6fff;font-size:12px;
    letter-spacing:1.5px;font-weight:600;">挑战 01</p>
  <p style="margin:0 0 8px;font-size:15px;color:#333;line-height:1.8;">
    挑战描述（为什么难）
  </p>
  <p style="margin:0 0 4px;color:#666;font-size:12px;letter-spacing:1.5px;">成果</p>
  <p style="margin:0 0 28px;font-size:15px;color:#333;line-height:1.8;">
    解法描述，关键数字：<strong style="color:#1a1a1a;">具体数字</strong>
  </p>

  <!-- 重复 挑战02 / 挑战03，最后一条去掉底部 margin-bottom:28px -->

</span>
```

---

### 引语块

> 无背景色，左侧竖线，文字斜体，整体轻盈。

```html
<div style="border-left:4px solid #4c6fff;padding:4px 0 4px 20px;margin:32px 0;">
  <p style="font-size:15px;line-height:1.9;color:#444;margin:0 0 10px;font-style:italic;">
    「客户原话」
  </p>
  <p style="font-size:13px;color:#999;margin:0;">— 姓名，职位</p>
</div>
```

---

### 对话气泡

```html
<!-- 客户消息（右对齐，灰底，圆角） -->
<p style="text-align:right;margin:0 0 14px 0;">
  <span style="display:inline-block;background-color:#ebebeb;color:#333;
    font-size:14px;line-height:1.8;padding:10px 16px;
    border-radius:18px 4px 18px 18px;max-width:80%;text-align:left;">
    客户消息内容
  </span>
</p>

<!-- AI 回复（左对齐，白底蓝边，圆角） -->
<p style="margin:0 0 14px 0;">
  <span style="display:inline-block;background-color:#ffffff;
    border:1px solid #dde4ef;color:#333;font-size:14px;
    line-height:1.8;padding:10px 16px;
    border-radius:4px 18px 18px 18px;max-width:85%;text-align:left;">
    AI 回复内容
  </span>
</p>
```

---

### 分割线

> 用于章节之间，比色块更轻。

```html
<hr style="border:none;border-top:1px solid #ebebeb;margin:32px 0;">
```

---

### CTA 区块

> ⚠️ 同样必须用 `<span style="display:block">` 保留蓝底背景

```html
<span style="display:block;background-color:#4c6fff;padding:28px 24px;margin:40px 0;
  border-radius:8px;text-align:center;">
  <p style="color:#fff;font-size:16px;font-weight:700;margin:0 0 10px;line-height:1.6;">
    主 CTA 文案
  </p>
  <p style="color:rgba(255,255,255,0.85);font-size:14px;margin:0;line-height:1.7;">
    后台留言「触发词」，1-2 个工作日出结果，不收费
  </p>
</span>
```

---

### 固定尾部

```html
<p style="font-size:13px;color:#999;line-height:2;margin:40px 0 0;text-align:center;">
  以上，既然看到这里了，如果觉得不错，随手点个赞、在看、转发三连吧，如果想第一时间收到推送，也可以给我个星标⭐～
</p>
```

---

## 📁 文件命名规范

```
MyContentFactory/
  Assets/
    YYYY-MM-DD_客户名/
      wechat_ready.html          ← 本 skill 输出目标
      one-pager-poster.html      ← 海报（见 shulex-case-production）
      客户名_案例海报_3x.png
```

---

## 🚨 常见雷区

| 雷区 | 正确做法 |
|------|---------|
| 小标题文字用品牌蓝 `#4c6fff` | 文字用深色 `#1a1a1a`，只有竖线和标签用品牌蓝 |
| 关键词用背景色块包裹 | 改用品牌蓝色字 + 「」包裹 + 下划线（更轻盈） |
| 段间距太小（<16px） | 段落 `margin-bottom` 统一 24px，保持呼吸感 |
| 小标题用 `<h2>` 标签 | 改用 `<p>` + inline style，微信不支持 `<h>` 样式继承 |
| 使用 `class` 或外部 CSS | 全部改为 inline style，微信编辑器会剥离 `<style>` 标签 |
| 亮点区块只写解法不写数字 | 每条成果必须有加粗的量化数字 |
| 对话气泡没有圆角 | 用 `border-radius` 区分客户（右上直角）和 AI（左上直角） |
| CTA 说「联系我们」 | 改为「留言『诊断』/『了解』」+ 说明响应时效 |
| **背景色容器用 `<div>`** | **必须改用 `<span style="display:block">`**：微信编辑器粘贴时会剥离 `div` 的 `background-color`，但保留 `span` 的 `background-color`。所有有背景色的卡片、CTA 块一律用 `span` |
| **背景容器用 `<table bgcolor>`** | 同上，`bgcolor` 属性也不可靠，唯一可靠方案是 `<span style="display:block;background-color:...">` |
