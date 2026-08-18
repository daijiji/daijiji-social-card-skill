# Components

Claude 暖编辑种子模板的共享组件规范。需要查类名、字号、跨 recipe 复现的硬规则时读这个。每 recipe 细节在 `layout-recipes.md`；每主题色 token 在 `theme-presets.md`。

## Font Stacks

`template-claude-card.html` 用混合字体方案——英文 serif + 中文 serif + 英文 sans + 中文 sans + mono：

- `--serif-en`: Cormorant Garamond, EB Garamond, Garamond, "Times New Roman", serif — 英文 serif 标题（Copernicus / Tiempos Headline 的开源替代）。
- `--serif-zh`: "Noto Serif SC", "Songti SC", STSong, serif — 中文 serif 标题。
- `--sans-en`: Inter, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif — 英文 sans 正文（StyreneB 的开源替代）。
- `--sans-zh`: "Noto Sans SC", "PingFang SC", "Microsoft YaHei", sans-serif — 中文 sans 正文。
- `--mono`: "JetBrains Mono", ui-monospace, "Cascadia Code", monospace — 代码、标签、metadata。

**字体配对规则**：

- 大标题（≥40px）：中文用 `--serif-zh`，英文用 `--serif-en`。CSS 用 `font-family: var(--serif-en), var(--serif-zh), serif`——英文优先 serif-en，中文回退 serif-zh。
- 正文（14-28px）：中文用 `--sans-zh`，英文用 `--sans-en`。CSS 用 `font-family: var(--sans-en), var(--sans-zh), sans-serif`。
- 代码/标签：`--mono`，无中文回退（代码标签通常英文）。
- 引文 italic：英文用 `--serif-en` italic，中文不 italic（中文无 italic 概念，用字号/颜色区分）。

**永远不要**：

- 用 Inter 做大标题——serif 字符是品牌声音。
- 用 serif 做正文段落——Claude 正文是 sans，serif 只在标题和引文。
- 把中文标题设成 italic——中文无 italic，会变形。

## Type Scale + Weight Mapping

遵循 **"the larger, the lighter"**。大字永远比小字轻。1080×1440 板上，考虑手机缩放后最小可读尺寸 22-28px。

Claude 暖编辑字号尺度（3:4 默认）：

| 角色 | 类 | 尺寸 | 字重 | 字距 | 字族 |
| --- | --- | --- | --- | --- | --- |
| Display | `.h-display` | 124px | 400 | -.04em | serif-en/zh |
| Section 标题 | `.h-xl` | 88px | 500 | -.03em | serif-en/zh |
| 中标题 | `.h-md` | 56px | 500 | -.02em | serif-en/zh |
| 副标题 | `.h-sub` | 36px | 400 it | -.01em | serif-en (英文 italic) |
| 引文 | `.pullquote` | 64px | 400 it | -.02em | serif-en/zh |
| Lead | `.lead` | 28px | 400 | 0 | sans-en/zh |
| 正文 | `.body` | 24px | 400 | 0 | sans-en/zh |
| Kicker | `.kicker` | 21px | 500 | +.04em | mono |
| Meta | `.meta` | 18px | 500 | +.04em | mono |
| Label | `.label` | 18px | 500 | +.04em | mono |
| Caption | `.img-cap` | 18px | 500 | 0 | mono |

> **字重纪律**：Claude 的 Copernicus serif 永远 weight 400。Cormorant Garamond 替代时用 weight 500（Cormorant 在 500 时视觉重量接近 Copernicus 400）。**永远不要** weight 700+ 在 serif 标题上——读成通用 landing-page，不是 Claude。

模板在 `.poster.square` 和 `.poster.wide` 内自动缩小 display 尺寸。通常不要覆盖。单个标题无法再短时才覆盖。

### Chinese Title Length Bands

中文字符视觉密度比拉丁字母高。定尺寸前先选带：

| 标题形状 | `.h-display` | `.h-xl` |
| --- | --- | --- |
| 1 行, ≤6 中文字 | 132px（默认） | 96px（默认） |
| 1 行, 7-10 字 | 108px | 88px |
| 2 行, 每行 ≤8 字 | 96px | 80px |
| 2 行, 任一行 9-12 字 | 84px | 72px |
| 3 行（罕见） | 72px | 64px |

标题仍放不下时，**先短文案**。永远不要靠把正文缩到最小可读尺寸以下解决溢出。

### `.h-xl` — 每板硬上限（已校验）

种子模板内置这些上限。超过即违反竖向预算：

| 板 | 默认 `.h-xl` | 最多行 | 每行最多字 | 超过会怎样 |
| --- | --- | --- | --- | --- |
| `.poster.xhs` (1080×1440) | 88px | 2 | 9 | 3 行标题把账本/栏/marginalia 推过 1440 |
| `.poster.square` (1080×1080) | 78px | 2 | 8 | 底部卡或 metadata 带裁切 |
| `.poster.wide` (2100×900) | 96px | 1 | 16 | 换 2 行挤右边栏 |

**3 行中文标题在 `.poster.xhs` 上**：从 C03/C10/C12（数据重）切到 C01/C05（封面型）让标题主导。不要把 `.h-xl` 缩到 72px 以下——小+重读成 Web1.0。

### `.h-display` — 每板硬上限

| 板 | 默认 `.h-display` | 最多行 | 每行最多字 |
| --- | --- | --- | --- |
| `.poster.xhs` (1080×1440) | 124px | 2 | 7 |
| `.poster.square` (1080×1080) | 108px | 2 | 6 |
| `.poster.wide` (2100×900) | 132px | 1 | 12 |

## Minimum Readable Sizes (mobile-safe)

1080×1440 PNG 通常在手机 360-420 逻辑像素宽看。低于所列尺寸不可读：

| 角色 | 最小 | 注 |
| --- | --- | --- |
| 正文/段落 | 24px | 暖编辑 |
| Lead | 28px | "1.5x 正文"守门 |
| Caption/kicker | 18px | 不要低于 16px |
| Label/meta 带 | 18px | 仅 mono |
| 网格内单元标题 | 22px | 矩阵/简卡 |
| 数字标注 | 20px | stat-card .lbl, 账本 .sub |

文案放不下时，砍文案。不要缩字号。

## Card Fills — 互斥

Claude 暖编辑有 5 个卡类；**同一节点上永远不要组合**：

- `.card-cream` — 奶油卡背景（`--surface-card`），墨字。内容卡主力。
- `.card-navy` — 深海军背景（`--navy`），奶油字。产品 mockup、代码窗、模型对比。
- `.card-coral` — 珊瑚满铺（`--coral`），白字。callout 卡，每海报最多一张。
- `.card-outlined` — 透明 + 1px hairline 边，墨字。需要卡但不要重量时。
- `.card-cream-strong` — 最强奶油（`--surface-cream`），墨字。选中态、强调带。

多卡网格必须每单元用**同一个**卡类，除允许单个珊瑚高亮当一项要突出时。在同一网格混 `.card-cream` 和 `.card-outlined` 看起来像草率模板。

**珊瑚卡纪律**：每海报最多一张 `.card-coral`。珊瑚是稀缺电压，不是网格填色。多张珊瑚卡读成杂乱。

## Image Containers

`.frame-img` 系统。永远选标准比例类。永远不要写 `aspect-ratio: 2592/1798` 这种临时比例。

| 类 | 比例 | 用 |
| --- | --- | --- |
| `.r-3x4` | 3:4 | 竖向封面和田野笔记照默认 |
| `.r-1x1` | 1:1 | 方肖像、产品物、平衡网格 |
| `.r-4x3` | 4:3 | 经典编辑照、满铺顶区 |
| `.r-3x2` | 3:2 | 杂志内联图 |
| `.r-16x9` | 16:9 | 风景照、信息图 |
| `.r-16x10` | 16:10 | 左文+右图分栏默认 |
| `.r-21x9` | 21:9 | 微信 21:9 hero 图 |

默认 `object-fit: cover`，`object-position: center 50%`。UI 截图、密文字、代码、信息图用 `.fit-contain`。不要裁脸或产品关键特征。

**主体感知裁剪——每张照片 inline 设 `object-position`。** 模板默认是兜底，不是推荐。每张 `<img>` 前看源图问：主体在哪？然后 inline 设 `style="object-position:center N%"`：

| 源图主体位置 | inline 值 |
| --- | --- |
| 主体近顶（天空重风景,脸在顶） | `center 25-35%` |
| 主体居中（默认） | `center 50%`（省略,是默认） |
| 主体中段（徒步者,手,中框） | `center 55-65%` |
| 主体低（前景装备,账本,框底） | `center 70-80%` |

默认 `center 50%` 会在高比例（`r-3x4`、`r-21x9`）上静默裁掉主体。每张交付照片都要有刻意 `object-position`——即使结论是"50% 这里行"。渲染检查遍抓这个：主体超过 1/3 在可见裁切外，交付前修。

**Caption 类名（不要搞错）**：

- `.img-cap` — 18px mono，用在 `.frame-img` 下的 `<figcaption>`。写 `.cap` 回退到浏览器默认 16px，触发 R4。

没有共享 `.cap` 类。草稿里看到 `.cap`，渲染前修。

## Screenshot Containers (.frame-shot)

UI 截图/网页捕获/代码照用 `.frame-shot`。默认 `object-fit: contain` 保持源像素纯净——和 `.frame-img` 相反。见 `references/screenshot-treatment.md` 全参数矩阵。

Claude 暖编辑默认（不要二次猜这些）：

| 角 | 阴影 | 默认底 | 默认内边距 |
| --- | --- | --- | --- |
| `corners-md` (12px) | `shadow-soft` | `bg-surface-card` | `inset-sub` |

两个设备外壳（`.device-browser`、`.device-phone`）提供纯 CSS 浏览器 chrome / 手机边框——无 SVG、无图依赖。每个 chrome 包一个 `.frame-shot`。

**Claude 特有**：代码截图优先用 `.card-navy` 包 `.frame-shot`——深海军是 Claude 展示产品 chrome 的地方。不要把代码截图放在奶油卡里，那读成 SaaS 营销页，不是 Claude 产品页。

## Spacing Tokens

Claude 暖编辑用 4px 基础的 2x 尺度。坚持这些 token——任意 `px` margin 跨海报漂移快。

| Token | 值 | 典型用 |
| --- | --- | --- |
| `--sp-3` | 8px | 紧凑 chip 间距,内联 meta |
| `--sp-4` | 12px | 卡内间距,密列表行 |
| `--sp-5` | 16px | 正文块底 margin,lead 到段 |
| `--sp-6` | 24px | 卡 padding（紧凑）,网格间距（紧） |
| `--sp-7` | 32px | 默认网格间距,卡 padding（默认） |
| `--sp-8` | 40px | 卡 padding（默认）,段间距（紧凑） |
| `--sp-9` | 48px | 段间距（默认）,callout 卡内边距 |
| `--sp-10` | 64px | 竖向内容块主断,CTA band 内边距 |
| `--sp-11` | 80px | 海报外边距（方/宽） |
| `--sp-12` | 96px | 海报外边距（竖）,段间距（大） |
| `--sp-13` | 160px | 宽海报水平边距 |

## Border Radius Scale

Claude 圆角层级——层次化，不是一刀切：

| Token | 值 | 用 |
| --- | --- | --- |
| `--r-xs` | 4px | 徽章 accent,小下拉 |
| `--r-sm` | 6px | 小内联按钮,下拉项 |
| `--r-md` | 8px | 标准 CTA 按钮,文本输入,分类 tab |
| `--r-lg` | 12px | 内容卡（特写/定价/代码窗/模型对比） |
| `--r-xl` | 16px | Hero 容器,大 marquee 组件 |
| `--r-pill` | 9999px | 徽章 pill,"NEW" 标 |
| `--r-full` | 9999px | 头像替代,图标按钮 |

## Icons

默认图标库 **Lucide**。Claude 暖编辑偶尔用图标；限制每海报 1-2 个。

规则：

- **永远不要 emoji。** Emoji 破坏暖编辑气质。
- 用角状 Lucide 图标（`arrow-right`、`check`、`triangle-alert`、`dot`、`plus`、`equal`、`slash`）。避免枕状圆角图标（`smile`、`heart-filled`）——它们和编辑几何打架。
- 图标 stroke weight：1.5（Lucide 默认）。不要加粗。
- 尺寸：56px 账本行指示，32px 内联正文，24px chip。永远不要低于 20px。
- 颜色：`var(--coral)` 高亮，`var(--muted)` 中性，`var(--ink)` 主。永远不要渐变。

加图标：

```html
<i data-lucide="arrow-right" width="32" height="32"></i>
```

模板加载时跑 `lucide.createIcons()`。加载后注入图标时手动再跑一次。

## Anthropic Spike-mark

Claude 的 4 辐射星号品牌标记。作为字标前缀和内容标记出现。inline SVG：

```html
<svg class="spike" width="24" height="24" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true">
  <path d="M12 2 L13.5 10.5 L22 12 L13.5 13.5 L12 22 L10.5 13.5 L2 12 L10.5 10.5 Z"/>
</svg>
```

规则：

- 颜色跟 `currentColor`——在奶油上是 `--ink`，在深海军上是 `--navy-on`，在珊瑚上是 `--coral-on`。
- 尺寸：16px 内联，24px 字标前缀，32px 大标记。
- **永远不要**在字标内反转为白底深色——保持单色。
- 不要装饰性使用（不要旋转、不要渐变、不要多个堆叠）。

## Issue Label and Corner Metadata

社交卡没有 PPT 的 chrome/foot 双行 metadata。用单个安静元素代替。

Claude 暖编辑 issue 元素：

- `.issue-row` — 顶："Vol. 01 · 2026.05" 中间小珊瑚点分段。
- `.issue-strip` — 底带：3-5 短标签用 em-dash 分隔，hairline 上方。
- `.kicker` — 顶或内联小 mono 标签，大写 + 宽字距。

每海报一个 issue 元素。永远不要同一海报顶 issue strip + 底 issue strip——读成装饰。

## Buttons and Links

Claude 暖编辑的按钮和链接遵循 Claude 产品视觉：

- `.btn-coral` — 珊瑚 CTA。背景 `--coral`，字 `--coral-on`，padding 12px × 20px，高 40px，圆角 `--r-md` (8px)。按压态 `.btn-coral-active` 深 `--coral-active`。
- `.btn-cream` — 奶油按钮 + hairline 边。背景 `--canvas`，字 `--ink`，1px hairline 边。
- `.btn-navy` — 深海军按钮（在深海军卡上用）。背景 `--navy-elevated`，字 `--navy-on`。
- `.link-coral` — 内联正文链接，`--coral` 色。按压下划线。珊瑚内联链接是系统最独特的小细节之一。

**珊瑚按钮纪律**：每海报最多一个 `.btn-coral`。多个珊瑚按钮读成 CTA 泛滥。次按钮用 `.btn-cream`。

## Layout Primitives

| 类 | 间距 | 用 |
| --- | --- | --- |
| `.stack` | flex 列 | 默认竖向流 |
| `.row` | flex 行 | 水平流 |
| `.gap-1`...`-5` | 12-64px | 紧到松间距 |
| `.gap-6`...`-10` | 24-64px | Carbon 尺度 |
| `.col-7-5` | 7:5 分 | 引文 + 图 |
| `.col-8-4` | 8:4 分 | 文重 + 图 |
| `.col-6-6` | 6:6 分 | hero 标题 + 图/mockup |
| `.grid-3` | 3 列 | 三栏网格 |
| `.grid-4` | 4 列 | 四栏网格 |

## Hard Rules (Shared)

这些规则守护视觉身份。违反几乎总是错。

1. **一个图组一个主题。** 永远不混色板。
2. **大标题用 serif，正文用 sans。** 这条 split 不可破。
3. **大标题 weight ≤500 + 负字距。** 永远不 700+。
4. **画布是暖奶油**（light）或**深海军**（dark）。永远不纯白、不冷灰。
5. **强调色是珊瑚**（或 Forest Warm 墨绿）。永远不冷蓝、不饱和青。
6. **珊瑚稀缺。** 单元素用珊瑚克制，只在满铺 callout 卡上慷慨。每海报最多一张 `.card-coral`、一个 `.btn-coral`。
7. **不要连续两个 band 同表面模式。** 节奏交替：奶油 → 奶油卡 → 深海军 → 奶油 → 珊瑚 → 深海军。
8. **永远不要 emoji。** 用 Lucide 或克制字体。
9. **不要造假数据、假百分比、`Lorem` 文本。**
10. **不要在 serif 标题上 inline 写 `font-size` + `font-weight`。** 用类型化类。
11. **不要平铺奶油背景无氛围层。** 纸纹 + 珊瑚光晕是暖编辑的必需。
12. **每个 `.poster` 有稳定导出尺寸。** 永远不要在海报内容用 `vw` / `vh`。
13. **每张图包在 `.frame-img` 里带标准比例类。**
14. **多卡网格用一个卡类。** 最多一个珊瑚高亮。
15. **守字号尺度和最小尺寸。** 砍文案不要缩字号。
