# HTML 生成规范

选定风格后，读取 `references/design-md/<slug>.md`，按本规范把用户内容生成为**单文件自包含 HTML**。

## 硬性规则

1. **单文件**：所有 CSS 内联在一个 `<style>`，零本地外部文件依赖。保存为一个 `.html`，双击即可在浏览器打开、可直接分享。
2. **字体**：查 DESIGN.md 的「Note on Font Substitutes / Font Substitutes」部分，引入它推荐的免费替代（通常是 Inter / Geist / IBM Plex Mono / Space Grotesk / JetBrains Mono 等）的 Google Fonts `<link>`，并写**完整 fallback 字体栈**保证离线降级。不要硬塞专有字体名（Linear Display、SoDoSans 等），否则会回退失败。
3. **颜色**：直接用 DESIGN.md frontmatter `colors:` 里的真实 hex，**不要自己编近似色**。
4. **尺度**：字号 / 字重 / 行高 / 字距用 `typography:` token；圆角用 `rounded:`；间距用 `spacing:` scale。
5. **组件形态**：按钮 / 卡片 / 输入 / 导航等按 `components:` 定义渲染，包括 hover/pressed/focused 等状态。
6. **Do's & Don'ts 必读必守**：这是与「通用漂亮页面」的核心区别。逐条对照，违反任一条 Don't 的元素必须移除——哪怕它「看起来更美」。
7. **布局骨架来自词库，不套模板**：每个 section 的 grid/flex 结构从 `references/layout-patterns.md` 选一个布局家族，相邻不重样、一页 ≥3–4 种家族。视觉 token（颜色/字号/圆角）仍取自 DESIGN.md——`layout-patterns` 只管**骨架**，`design-md` 只管**皮肤**，两者正交。

## 字体引入示例

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono&display=swap" rel="stylesheet">
<style>
  body { font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif; }
  code, .mono { font-family: 'JetBrains Mono', ui-monospace, 'SF Mono', Menlo, monospace; }
</style>
```

## 布局编排（先想骨架，再写 CSS）

页面的骨架结构决定它「像不像模板」。**永远不要让所有 section 都用同一种卡片网格**——这是「布局单一」的头号来源。生成前先读 `references/layout-patterns.md`，为每个 section 挑一个**结构不同**的布局家族，组合成骨架。

编排流程：
1. **数内容块**：把内容拆成 4–8 个 section。
2. **选 Hero 开场**：按调性从 7 种 Hero 变体里挑（工具/数据 → Split / Showcase；宣言/品牌 → Manifesto / Centered；媒体/汽车 → Full-Bleed）。**不要默认居中三件套**。
3. **逐 section 选不同家族**：从 Bento / Zigzag 分栏 / 满版宣言 / 时间线 / 指标带 / 编辑正文 / 横向滚动 / Logo 墙 / 手风琴 / 规格表 / 等高卡片里挑，**相邻不重样**。
4. **反重复校验**：见 `layout-patterns.md` 的「反重复纪律」——一页 ≥3–4 种家族、同一家族 ≤1 次、Zigzag 连续 ≤2 次、Bento cell 数 = 内容数、feature 区不默认 3 列等高。

按内容类型的**推荐组合**（只是起点，按实际内容增减，且要让相邻区块换成不同家族）：

- **产品落地页**：Split/Showcase Hero → Bento 特性 → 指标带 → Zigzag 深入 → 满版 CTA → Footer
- **文章 / 周报 / 长文**：编辑正文 Hero → 编辑式正文（单列 / 双栏）→ 满版引言断点 → 继续正文 → 作者署名
- **简历 / 作品集**：Manifesto intro → Zigzag 代表作 → 横向滚动更多作品 → 时间线经历 → 联系区
- **数据 / 看板类**：指标开场 Hero → 指标带 → Bento 数据卡 → 规格表分组 → Footer

结构服务于内容，不要为了凑布局家族而硬造无关区块。

## 明暗与氛围

DESIGN.md 定义的是 light 就用 light，是 dark 就用 dark——**不要擅自加主题切换开关**。暗色系的留白靠 surface 层级（canvas → surface-1 → surface-2）而非空隙；亮色系靠真实留白。氛围（摄影驱动 / 产品截图当主角 / 渐变 / 单色禁欲 等）要忠实还原该风格的「标志特征」。

## 响应式

按 DESIGN.md 的「Responsive Behavior」部分加媒体查询：断点、卡片网格列数坍缩、导航转汉堡菜单、大标题在移动端缩放。触控目标 ≥40–44px。

## 生成后自检清单

逐条过一遍再交付：

- [ ] 是否单文件、双击即可在浏览器打开
- [ ] 所有颜色是否来自 DESIGN.md（没有自创近似色）
- [ ] 是否违反该风格的任一条 Don't
- [ ] 是否呈现了该风格的「标志特征」（索引中的标志特征列）
- [ ] 字体是否用了推荐的免费替代 + fallback 栈
- [ ] 响应式断点是否符合 DESIGN.md
- [ ] 圆角 / 间距 / 字距是否用了 token 而非随意值
- [ ] **布局多样性（机械检查）**：列出全页用到的布局家族，确认 ≥3 种、相邻不重样、同一家族未重复（导航 / CTA / Footer 除外）
- [ ] **Hero 非默认居中**：Hero 是按调性选的变体，而非无脑居中三件套
- [ ] **反重复纪律**：Zigzag 连续 ≤2、Bento 无空格且 cell 数 = 内容数、feature 区不是默认 3 列等高卡片

## 输出方式

把生成的 HTML 写成一个 `.html` 文件，保存到用户指定位置（未指定则存到当前工作目录，文件名与内容相关），告知用户绝对路径，并提示可直接用浏览器打开预览。若用户只想看代码不想要文件，也可直接在回复中给出完整 HTML。
