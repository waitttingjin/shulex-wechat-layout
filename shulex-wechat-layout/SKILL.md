---
name: shulex-wechat-layout
description: Use when generating WeChat Official Account HTML for Shulex articles, case studies, or activity recaps — including body layout, highlight cards, dialogue bubbles, quote blocks, and CTA sections. Triggers on requests like "帮我排版", "生成微信HTML", "公众号排版", "wechat_ready.html".
---

# Shulex 公众号 HTML 排版

## 🎯 Target

输出一个可直接粘贴进微信公众号编辑器的 `wechat_ready.html`，视觉风格统一遵循 Shulex 品牌色系，兼容微信内置渲染器。

---

## ✅ Checklist

- [ ] 整体字号 14px，行高 2，主色蓝 `#4c6fff`
- [ ] 小标题有左侧蓝色竖线，字号 17px、加粗
- [ ] 亮点卡片：浅蓝底 `#eef4fd`，挑战/成果双行结构，成果数字加粗
- [ ] 引语块：灰底 `#f5f5f5`，左边竖线，客户姓名+职位
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
font-size: 14px;
line-height: 2;
color: #333;

/* 主色 */
--brand:        #4c6fff;
--brand-light:  #eef4fd;
--brand-dim:    #dde4ef;

/* 背景色体系 */
--bg-brand:   #f5f7ff;   /* 品牌底色块 */
--bg-quote:   #f5f5f5;   /* 引语块 */
--bg-case:    #fff8ec;   /* 案例块 */
--bg-card:    #eef4fd;   /* 亮点卡片 */
```

---

## 🧩 HTML 模块速查

### 小标题

```html
<p style="font-size:17px;font-weight:700;color:#4c6fff;
  border-left:4px solid #4c6fff;padding-left:12px;margin:24px 0 12px;">
  标题文字
</p>
```

### 正文段落

```html
<p style="font-size:14px;line-height:2;color:#333;margin:0 0 16px;">
  段落内容
</p>
```

### 内嵌小标签

```html
<span style="background:#eef4fd;color:#4c6fff;border-radius:4px;
  padding:3px 12px;font-size:12px;font-weight:600;">
  标签文字
</span>
```

### 亮点卡片区（三条挑战→成果）

```html
<div style="background:#eef4fd;padding:20px 24px;margin:24px 0;border-radius:4px;">
  <!-- 挑战01 -->
  <p style="margin:0 0 4px;color:#666;font-size:12px;letter-spacing:1px;">挑战 01</p>
  <p style="margin:0 0 6px;font-size:14px;color:#333;line-height:1.8;">挑战描述（为什么难）</p>
  <p style="margin:0 0 20px;color:#4c6fff;font-size:12px;letter-spacing:1px;">成果</p>
  <p style="margin:0 0 20px;font-size:14px;color:#333;line-height:1.8;">
    解法描述，关键数字：<strong>具体数字</strong>
  </p>
  <!-- 重复 挑战02 / 挑战03 -->
</div>
```

### 引语块

```html
<div style="background:#f5f5f5;border-left:4px solid #4c6fff;
  padding:16px 20px;margin:24px 0;">
  <p style="font-size:14px;line-height:2;color:#333;margin:0 0 8px;font-style:italic;">
    「客户原话」
  </p>
  <p style="font-size:12px;color:#666;margin:0;">— 姓名，职位</p>
</div>
```

### 对话气泡

```html
<!-- 客户消息（右对齐，灰底） -->
<p style="text-align:right;margin:0 0 14px 0;">
  <span style="display:inline-block;background-color:#e0e0e0;color:#333;
    font-size:13px;line-height:1.8;padding:10px 14px;
    max-width:80%;text-align:left;">
    客户消息内容
  </span>
</p>

<!-- AI 回复（左对齐，白底蓝边） -->
<p style="margin:0 0 14px 0;">
  <span style="display:inline-block;background-color:#ffffff;
    border:1px solid #dde4ef;color:#333;font-size:13px;
    line-height:1.8;padding:10px 14px;max-width:85%;text-align:left;">
    AI 回复内容
  </span>
</p>
```

### CTA 区块

```html
<div style="background:#4c6fff;padding:24px;margin:32px 0;border-radius:4px;text-align:center;">
  <p style="color:#fff;font-size:15px;font-weight:700;margin:0 0 8px;">
    主 CTA 文案
  </p>
  <p style="color:rgba(255,255,255,0.85);font-size:13px;margin:0;">
    后台留言「触发词」，1-2 个工作日出结果，不收费
  </p>
</div>
```

### 固定尾部

```html
<p style="font-size:13px;color:#999;line-height:2;margin:32px 0 0;text-align:center;">
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
| 小标题用 `<h2>` 标签 | 改用 `<p>` + inline style，微信不支持 `<h>` 样式继承 |
| 使用 `class` 或外部 CSS | 全部改为 inline style，微信编辑器会剥离 `<style>` 标签 |
| 亮点区块只写解法不写数字 | 每条成果必须有加粗的量化数字 |
| 对话气泡宽度超出屏幕 | 客户消息 max-width:80%，AI 回复 max-width:85% |
| CTA 说「联系我们」 | 改为「留言『诊断』/『了解』」+ 说明响应时效 |
