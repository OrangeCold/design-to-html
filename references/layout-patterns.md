# 布局模式词库（骨架库）

这是 design-to-html 的**布局骨架目录**。`style-index.md` 和 `design-md/*` 决定页面「长什么脸」（颜色/字体/圆角/token），本文件决定页面「骨架怎么搭」。两者**正交**。

## 本文件管什么 / 不管什么

- ✅ **管**：每个区块用哪种 grid / flex 结构拼起来、Hero 用哪种开场、section 之间如何错开节奏、内容多到放不下时换什么容器。
- ❌ **不管**：颜色、字号、字重、圆角、间距刻度、明暗氛围——这些**一律取自选中的 `design-md/<slug>.md` 的 token**。下面所有骨架代码里出现的具体视觉值都只是占位，生成时必须替换成该品牌的真实 token。

> 一句话：`design-md` 是**皮肤**，本文件是**骨架**。换皮不换骨，换骨不换皮。

## 怎么用：布局编排三步

生成 HTML 前**先想骨架，再写 CSS**。这是和「套模板」的核心区别。

1. **数内容块**：把用户内容拆成若干 section（通常 4–8 个），每个 section 想清楚它要传递什么。
2. **每块选一个不同布局家族**：从下面的 Hero 变体 + Section 家族里，给每个 section 挑一个**结构不同**的骨架。相邻 section 结构不能雷同。
3. **检查多样性**：用文末「反重复纪律」和生成指南的自检清单核对——一页是否用了足够多种布局家族、有没有连续重复。

骨架代码里的具体值（颜色 `c-accent`、字号 `fs-display`、圆角 `r-card`、间距 `s-…`）都是**语义占位符**，生成时换成选中 DESIGN.md 的真实 token。

---

## 一、Hero 变体（开场怎么打）

Hero 是第一印象，**不要默认居中三件套**。按内容调性从下面挑一种。

### 1. 不对称分屏 Split（默认首选）
左文右图（或反向），大量留白。绝大多数 SaaS / 工具 / 落地页的最稳开场。

```
┌─────────────────────────────────────┐
│  标题（左对齐）    │                 │
│  副标              │     [视觉/图]   │
│  [主CTA][次CTA]    │                 │
└─────────────────────────────────────┘
```
```html
<section class="hero hero-split">
  <div class="hero-copy">…标题/副标/CTA…</div>
  <div class="hero-asset">…产品图 or 视觉…</div>
</section>
```
```css
.hero-split {
  display: grid;
  grid-template-columns: 1.05fr 0.95fr;   /* 偶数行可翻成 0.95fr 1.05fr 制造差异 */
  align-items: center;
  gap: var(--s-lg);
}
@media (max-width: 768px) { .hero-split { grid-template-columns: 1fr; } }
```

### 2. 居中聚焦 Centered（克制使用）
标题 + 副标 + CTA 全居中。**只在内容是宣言/发布/品牌主张时用**，不要当万能默认。

```
┌─────────────────────────────────────┐
│            副标 eyebrow（可选）       │
│            大 标 题                   │
│         副标说明（≤20词）             │
│        [主CTA]  [次CTA]               │
└─────────────────────────────────────┘
```
```css
.hero-centered { text-align: center; display: flex; flex-direction: column; align-items: center; gap: var(--s-md); }
```

### 3. 产品截图当主角 Showcase
左文窄 + 右大截图（带阴影/边框，模拟真实 UI）。Linear / Notion / Raycast 类工具页。

```
┌──────────┬──────────────────────────┐
│ 标题      │                          │
│ 副标      │   ┌──────────────────┐  │
│ CTA       │   │  产品截图(大)     │  │
│           │   └──────────────────┘  │
└──────────┴──────────────────────────┘
```
```css
.hero-showcase { display: grid; grid-template-columns: minmax(0, 30rem) 1fr; align-items: center; gap: var(--s-xl); }
.hero-showcase img { width: 100%; border-radius: var(--r-card); box-shadow: 0 30px 60px -20px rgba(0,0,0,.35); }
```

### 4. 编辑宣言 Manifesto（巨字无图）
一句巨型标题，几乎没有别的，近乎海报。奢华 / 编辑 / 品牌主张页。

```
┌─────────────────────────────────────┐
│                                     │
│   一 句  占  满  整  屏  的  话      │
│   （副标极短，可无图）                │
│                                     │
└─────────────────────────────────────┘
```
```css
.hero-manifesto h1 { font-size: clamp(2.5rem, 7vw, 6rem); line-height: 1.02; max-width: 16ch; }
```

### 5. 满版摄影压字 Full-Bleed
全屏背景图 + 叠加文字 + 半透明遮罩。Apple / Airbnb / Spotify / 汽车类。

```
┌─────────────────────────────────────┐
│ ▓▓▓▓▓▓▓ 背景满版图 ▓▓▓▓▓▓▓          │
│ ▓                          ▓        │
│ ▓   叠加的标题 + CTA       ▓        │
│ ▓▓▓▓▓▓▓ 遮罩层 ▓▓▓▓▓▓▓▓▓            │
└─────────────────────────────────────┘
```
```html
<section class="hero-fullbleed">
  <img class="hero-bg" src="…" alt="">
  <div class="hero-overlay">…标题/CTA…</div>
</section>
```
```css
.hero-fullbleed { position: relative; min-height: 100svh; display: grid; place-items: center; }
.hero-bg { position: absolute; inset: 0; width: 100%; height: 100%; object-fit: cover; }
.hero-overlay { position: relative; z-index: 1; }              /* 配合背景遮罩保证文字对比度 */
```

### 6. 指标开场 Metric-Lead
用一个大数字 + 一句话开场，适合数据 / 金融 / 成果型落地页。

```
┌─────────────────────────────────────┐
│            99.99%                    │
│        一句话说明这个数字             │
│            [CTA]                     │
└─────────────────────────────────────┘
```
```css
.hero-metric .big-num { font-size: clamp(3rem, 10vw, 8rem); line-height: 1; }
```

### 7. 粘性滚动 Sticky-Pinned
Hero 用 `position: sticky` 钉在顶部，下方 section 滚上来覆盖。制造层次，纯 CSS 可做。

```css
.hero-sticky { position: sticky; top: 0; z-index: 0; min-height: 90svh; }
.section-after { position: relative; z-index: 1; background: var(--surface); }  /* 必须有不透明背景才能盖住 */
```

---

## 二、Section 布局家族（内容区骨架）

**这是「布局单一」最容易翻车的地方**——别让每个 section 都是「3 张等高卡片」。下面 11 种家族，按内容挑，相邻不重样。

### 1. Bento 不对称网格（强推，替代等高卡片默认）
混合大小的格子，主次分明。Apple / 现代 SaaS 默认。

```
┌──────────────┬───────┐
│              │       │
│   大格(主)    │  中格 │
│              ├───────┤
│              │  小格 │
├──────┬───────┴───────┤
│ 小格 │     宽格       │
└──────┴───────────────┘
```
```css
.bento {
  display: grid;
  grid-template-columns: repeat(4, 1fr);          /* 按内容数量定列数 */
  grid-auto-rows: minmax(8rem, auto);
  gap: var(--s-md);
}
.bento .cell-lg { grid-column: span 2; grid-row: span 2; }
.bento .cell-wide { grid-column: span 2; }
@media (max-width: 768px) { .bento { grid-template-columns: 1fr; } .bento .cell-lg, .bento .cell-wide { grid-column: auto; grid-row: auto; } }
```
> **cell 数 = 内容数**。有 5 块内容就搭 5 个 cell（如 2+3 或 大格+4小格），**不要留空格**，也别硬塞第 6 个凑数。

### 2. Zigzag 左右分栏（图文交替）
图左文右、下一行文左图右，错落叙事。**连续最多 2 次，第 3 次必须换家族**。

```
┌───────────┬───────────┐
│   [图]     │   文字     │
├───────────┼───────────┤
│   文字     │   [图]     │
└───────────┴───────────┘
```
```css
.zigzag > .row { display: grid; grid-template-columns: 1fr 1fr; gap: var(--s-lg); align-items: center; }
.zigzag > .row[data-reverse] .col-media { order: 2; }   /* 奇数行正序，偶数行翻转 */
@media (max-width: 768px) { .zigzag > .row { grid-template-columns: 1fr; } .zigzag > .row[data-reverse] .col-media { order: 0; } }
```

### 3. 满版宣言带 Full-Bleed Statement
一句大话 / 大引言占满整个 section，可反色或换底，制造节奏断点。

```
┌═════════════════════════════════════┐   ← 可反色 / 换 surface
║   一 句  占 满 的  宣 言 / 引 言     ║
╚═════════════════════════════════════╝
```
```css
.statement { padding: var(--s-3xl) var(--s-lg); text-align: center; }
.statement blockquote { font-size: clamp(1.5rem, 4vw, 3rem); line-height: 1.2; max-width: 20ch; margin: 0 auto; }
```

### 4. 纵向时间线 / 步骤 Timeline
历程、发展、流程、How it works。

```
  ●──── 标题
  │      说明
  ●──── 标题
  │      说明
  ●──── 标题
         说明
```
```css
.timeline { display: grid; gap: var(--s-lg); }
.timeline .step { display: grid; grid-template-columns: auto 1fr; gap: var(--s-md); }
.timeline .step::before { content: ""; width: 12px; height: 12px; border-radius: 50%; background: var(--c-accent); margin-top: .4em; }
```

### 5. 指标陈列带 Metric Strip
一行几个大数字 + 标签，成果/数据陈列。

```
┌────────┬────────┬────────┬────────┐
│ 99.9%  │  2.1M  │  120+  │ 4.8★   │
│  可用性 │  用户  │  国家  │ 评分   │
└────────┴────────┴────────┴────────┘
```
```css
.metrics { display: grid; grid-template-columns: repeat(4, 1fr); gap: var(--s-md); text-align: center; }
@media (max-width: 768px) { .metrics { grid-template-columns: repeat(2, 1fr); } }
.metrics .num { font-size: clamp(2rem, 5vw, 3.5rem); line-height: 1; }
```

### 6. 编辑式正文 Editorial
文章/周报/长文的主体。单列限宽（≤65ch）或双栏（`column-count: 2`），忠实该品牌的排版节奏与行高。

```css
.editorial { max-width: 65ch; margin-inline: auto; }
.editorial.long { column-count: 2; column-gap: var(--s-xl); }   /* 长文双栏，移动端单栏 */
@media (max-width: 768px) { .editorial.long { column-count: 1; } }
```

### 7. 横向滚动卡片 Horizontal Scroll
作品集 / 图集 / 证言 / 横向陈列。用原生 `overflow-x` + scroll-snap，无需 JS。

```
┌─────┐ ┌─────┐ ┌─────┐ ┌ →
│卡片1│ │卡片2│ │卡片3│ │
└─────┘ └─────┘ └─────┘
```
```css
.scroller { display: flex; gap: var(--s-md); overflow-x: auto; scroll-snap-type: x mandatory; padding-bottom: var(--s-md); }
.scroller > * { flex: 0 0 18rem; scroll-snap-align: start; }
```

### 8. Logo Wall（社会证明）
合作方/客户 logo 网格。logo 灰度统一，**只放 logo，不加行业标签**。

```css
.logo-wall { display: grid; grid-template-columns: repeat(auto-fit, minmax(9rem, 1fr)); gap: var(--s-md); place-items: center; }
.logo-wall img { height: 2rem; width: auto; filter: grayscale(1); opacity: .7; }
```

### 9. 手风琴 FAQ
原生 `<details>/<summary>`，零 JS。

```html
<details class="faq"><summary>问题</summary><div>答案</div></details>
```
```css
.faq { border-block: 1px solid var(--c-border); }
.faq summary { cursor: pointer; padding: var(--s-md) 0; list-style: none; }
.faq summary::-webkit-details-marker { display: none; }
.faq[open] summary { color: var(--c-accent); }
```

### 10. 规格表 Spec Table（分组而非逐行 hairline）
硬件/产品规格、对比表。**不要给每一行都加 `border-bottom`**——分组块或卡片化。

```
┌─ 材料 ────────────┐  ┌─ 性能 ────────────┐
│ 项 值              │  │ 项 值              │
│ 项 值              │  │ 项 值              │
└────────────────────┘  └────────────────────┘
```
```css
.specs { display: grid; grid-template-columns: repeat(2, 1fr); gap: var(--s-lg); }
.specs .group { padding: var(--s-md); border-radius: var(--r-card); background: var(--surface-2); }
.specs .group h4 { color: var(--c-accent); }
@media (max-width: 768px) { .specs { grid-template-columns: 1fr; } }
```

### 11. 等高卡片网格 Equal Cards（克制使用）
2–4 列等高卡片。**这是最容易雷同的默认布局，除非内容真的对等，否则优先用 Bento**。真要用，建议 **2 列**而非 3 列，降低模板感。

```css
.cards { display: grid; grid-template-columns: repeat(2, 1fr); gap: var(--s-md); }   /* 默认 2 列；4 项以上才考虑 3 列 */
@media (max-width: 768px) { .cards { grid-template-columns: 1fr; } }
```

---

## 三、导航 / CTA / Footer 变体（简略）

**导航**：① 顶部常规 `nav` ② 透明叠加在 Hero 上（绝对定位）③ 极简无背景粘顶。**桌面端必须单行**，放不下转汉堡菜单。高度 ≤ 80px。

**CTA**：① 满版带底色 banner（推荐，节奏断点）② 内联在文案末尾 ③ 卡片式聚拢。一页 CTA 意图不重复（"联系我们"/"开始合作"/"立即咨询"算同一意图，只留一个措辞）。

**Footer**：① 多列链接网格（常见）② 极简单行版权 ③ 大字品牌名 + 少量链接。暗色页用更深 surface。

---

## 四、反重复纪律（编排硬规则）

骨架选好后，按这些规则校验。**违反即视为模板化**。

1. **一页至少 3–4 种不同布局家族**。8 个 section 的页面，布局家族 ≥ 4 种。
2. **同一种布局家族一页最多用 1 次**（导航 / CTA / Footer 除外）。第二个 feature 区不能再是 Bento 之后的 Bento。
3. **相邻 section 结构不雷同**。Bento 下面别接等高卡片网格，满版宣言下面别接满版宣言。
4. **Zigzag 左右分栏连续 ≤ 2 次**，第 3 个图文分栏必须换成别的家族。
5. **3 列等高卡片不作为 feature 区的默认选择**——优先 Bento 或 2 列。
6. **Bento 的 cell 数 = 内容数**，中间和末尾不留空格。
7. **Hero 不总是居中**。按调性从 7 种 Hero 变体里挑：工具/数据 → Split 或 Showcase；宣言/品牌 → Manifesto 或 Centered；媒体/汽车 → Full-Bleed。
8. **节奏要有呼吸**：密（Bento/指标带）与疏（满版宣言/编辑正文）交替，别让整页都是密集网格。

---

## 五、移动端通用坍缩

- 所有多列 grid 在 `< 768px` 坍缩为**单列**（`grid-template-columns: 1fr`），横向滚动卡和 logo wall 可保留多列。
- Sticky / Full-bleed hero 在移动端取消钉住或降低 `min-height`（地址栏会跳动，用 `svh`/`dvh` 而非 `vh`）。
- 巨字 Hero 在移动端用 `clamp()` 缩放，不要让标题溢出视口。
- 触控目标 ≥ 44px。

---

骨架选好后，回到 `html-generation-guide.md`，把每个 section 套上选中的布局家族，再用选中 DESIGN.md 的 token 填充视觉值，即可生成。
