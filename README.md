# design-to-html

把任意内容做成设计精致、**忠于真实品牌设计语言**的单文件 HTML 网页。

内置 **74 个从真实网站提取的完整设计系统**（DESIGN.md），每个都包含真实的颜色 / 字体 / 间距 / 圆角 / 组件 token 与 Do's & Don'ts 规则。你不用凭空「设计」——先匹配一个真实品牌风格，再严格按它的规范生成。

> 基于 [Agent Skill](https://docs.claude.com/en/docs/agents-and-tools/agent-skills) 规范（`SKILL.md` 入口 + `references/` 资源）构建，可被任意支持该规范的主流 agent 加载使用，不绑定特定平台。

## 为什么这样设计

让 AI「生成一个漂亮的 HTML 页面」很容易，但结果往往是泛泛的、没有品牌识别度的通用页面。这个 skill 的价值在于输出**有真实设计语言支撑**：颜色、字距、圆角、氛围都来自一个具体品牌的设计系统，且严守它的禁忌。所以质量命脉是「忠于选中风格」，不是「看起来好看就行」。

## 工作流（4 步）

1. **理解内容** —— 判断类型（落地页 / 长文 / 简历 / 数据看板…）、调性、目标受众与明暗偏好。
2. **匹配候选风格** —— 沿「内容类型 / 调性氛围 / 明暗偏好 / 行业线索」四个维度，从 `references/style-index.md` 语义匹配出 1–3 个候选。
3. **选定风格** —— 唯一命中直接用；多个都说得通用 AskUserQuestion 让用户选。
4. **生成 HTML** —— 只读选中的那个 DESIGN.md，按 `html-generation-guide.md` 规范生成**单文件自包含 HTML**。

核心原则：**先匹配，后生成 · 忠于 token，不自创 · Do's & Don'ts 是硬约束 · 按需加载。**

## 内置的 74 个设计系统

| | | | | |
|---|---|---|---|---|
| Airtable | Airbnb | Apple | Binance | BMW |
| BMW M | Bugatti | Cal | Claude | Clay |
| ClickHouse | Cohere | Coinbase | Composio | Cursor |
| Dell (1996) | ElevenLabs | Expo | Ferrari | Figma |
| Framer | HashiCorp | HP | IBM | Intercom |
| Kraken | Lamborghini | Linear | Lovable | Mastercard |
| Meta | MiniMax | Mintlify | Miro | Mistral AI |
| MongoDB | Nike | Nintendo (2001) | Notion | NVIDIA |
| Ollama | OpenCode AI | Pinterest | PlayStation | PostHog |
| Raycast | Renault | Replicate | Resend | Revolut |
| Runway ML | Sanity | Sentry | Shopify | Slack |
| SpaceX | Spotify | Starbucks | Stripe | Supabase |
| Superhuman | Tesla | The Verge | Together AI | Uber |
| Vercel | Vodafone | Voltagent | Warp | Webflow |
| Wired | Wise | xAI | Zapier | |

## 文件结构

```
design-to-html/
├── SKILL.md                      # 工作流 + 关键原则
├── references/
│   ├── style-index.md            # 74 个风格索引（匹配用）
│   ├── html-generation-guide.md  # HTML 生成规范与自检清单
│   └── design-md/                # 74 个 DESIGN.md 原文（只读选中的那个）
└── LICENSE
```

## 安装与使用

本 skill 遵循 Agent Skill 规范，任何支持该规范的主流 agent（Claude Code、Cursor、Codex 等）均可加载。把本目录放入对应工具的 skills 目录后重启会话即可识别，然后直接用自然语言描述需求，例如：

- 「把这份周报做成一个 HTML 页面，要 Linear 那种感觉」
- 「帮我做一个产品落地页」
- 「把这篇文章排版成精美的单文件网页」

Skill 会自动匹配最贴切的品牌风格（命中多个时让你选），并严格按该风格的 token 与规则生成可双击打开的 HTML。

## 许可证

[MIT](./LICENSE)

## 商标与免责声明

本仓库的代码（`SKILL.md`、生成规范、索引）以 MIT 许可证开源。但 `references/design-md/` 下的 74 个设计系统提取自各真实品牌的公开网站，描述的是这些品牌的视觉设计语言——相关品牌名称、Logo、配色、字体搭配等商标与品牌资产归各自品牌所有，本仓库仅作设计参考与学习用途。

使用这些风格生成的 HTML 如用于商业或公开场景，请勿直接冒充或误导为相关官方品牌，并遵守各品牌的使用规范。MIT 许可证覆盖仓库中的代码与文档，但不覆盖第三方商标权。
