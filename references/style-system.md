# Style System

本技能从 Anthropic Claude 产品视觉语言提取静态社交图原则。不依赖 Claude.com 实际页面模板。

## Shared Rules

- 内容形状决定版式。不要先选漂亮版式再编内容去填。
- 强层级：标题、钩子、证据、caption、metadata。
- 真图作证据或氛围，不作装饰。
- 避免可见杂物：无随机 SVG 圆、椭圆滴、blob、bokeh、装饰贴纸、假图示装饰、装饰渐变。
- 同一图组所有页通过网格、字体、色板、复现 metadata 保持视觉关联。
- 每页有清晰焦点。

## Single Mode: Claude Warm Editorial

本技能**只有一个视觉模式**——Claude 暖编辑。不沿用原 guizang skill 的 Editorial/Swiss 二分法。Claude 的视觉语言是单一的：暖奶油 + serif 标题 + 人文 sans 正文 + 珊瑚强调 + 深海军产品面。它既不是纯杂志（太冷），也不是纯 Swiss（太工程），是中间的暖编辑气质。

5 个主题色板（见 `theme-presets.md`）是这个模式下的色板变体，不是不同模式。Claude Canvas 是默认；Coral Callout / Dark Product / Forest Warm / Midnight Claude 是场景变体。

### Visual Anchors

- **Cormorant Garamond / 思源宋体 serif 标题**——weight 400-500，负字距（`-.01em` 到 `-.04em`）。文学性，不喊叫。
- **Inter / 思源黑体 sans 正文**——weight 400 段落，weight 500 标签和强调短语。人文比例，非几何。
- **JetBrains Mono 代码**——所有代码块、终端、技术标签。
- **暖奶油画布**（`#faf9f5`）——品牌差异点。不是冷灰白，不是纯白。
- **深暖墨字**（`#141413`）——略偏暖的近黑，不是纯黑。
- **珊瑚强调**（`#cc785c`）——Anthropic 签名色。CTA、链接、callout 卡。稀缺用。
- **深海军产品面**（`#181715`）——代码编辑器 mockup、模型对比卡、页脚。产品 chrome 在这展示。
- **分层氛围背景**——纸纹 + 珊瑚光晕径向 + 可选 WebGL 墨流。不是平铺奶油色。
- **杂志栏、引文、大图井、产品 mockup 卡**——文学 + 产品双气质。
- **Anthropic spike-mark**（4 辐射星号）——品牌字标前缀和内容标记。可选 inline SVG。
- **圆角层级**：8px 按钮/输入，12px 内容/产品卡，16px hero 容器，pill 徽章。

### Typography Stance — "the larger, the lighter"

这条规则对 Claude 暖编辑**不可协商**。Claude 的 Copernicus serif 标题永远 weight 400，配负字距。粗大 serif 标题（700-900）读成"通用 landing-page"，不是 Claude。

- 大标题（≥72px on 1080×1440）：weight 400-500，负字距 `-.02em` 到 `-.04em`。
- 中标题（40-72px）：weight 500，负字距 `-.01em` 到 `-.02em`。
- 小标题/标签（<40px）：weight 500，正常或正字距。
- 正文：weight 400。
- Caption/meta：weight 500，mono，正字距或 `+.04em`。

**反模式**：90px h1 weight 800 + 负字距 = 通用 infographic banner，不是 Claude。永远用类型化类（`.h-display` / `.h-xl` / `.h-md`），不要 inline 写 `font-size` + `font-weight`。

### Layout Patterns

- **封面**：大标题顶或左，大图/照片证据占 35-55%，底部 issue strip 或清单。
- **特写页**：一张大图 + 窄栏压缩解释。
- **清单页**：编辑头 + 编号项 + 一张支持图裁切或无图标图示。
- **引文/结论页**：大引文 + 小来源/上下文行。
- **对比页**：两栏张力，简单标签，一个视觉锚。
- **产品 mockup 页**：深海军卡承载代码/终端/模型对比，奶油栏做说明。
- **callout 页**：满铺珊瑚卡，奶油字，宣言式 CTA。

### Background Rule

平铺奶油色不够。用 `background-systems.md` 的分层背景：奶油底 + 纸纹 + 珊瑚光晕径向 + 可选 WebGL 墨流 canvas。背景造氛围同时保持文字可读。

**不要**加全页网格/点阵背景——那是 Swiss，不是 Claude。

**不要**用平铺 `#faf9f5` 什么都没有——那是 SaaS landing page，不是暖编辑。

### Surface Rhythm

Claude 的页面节奏靠表面模式交替：

1. **奶油画布**（`--canvas`）——默认正文底。
2. **奶油卡**（`--surface-card`）——特写卡、内容卡。
3. **深海军产品面**（`--navy`）——代码编辑器 mockup、模型展示卡、页脚。
4. **珊瑚 callout**（`--coral` 满铺）——CTA 瞬间、宣言页。

**不要连续两个 band 用同一种表面模式。** 节奏交替：奶油 → 奶油卡 → 深海军 mockup → 奶油 → 珊瑚 callout → 深海军页脚。

## Image Rules

- 照片：有意裁剪。脸、手、产品、主体留在安全区。
- 产品截图：保留文字；细节重要时用 `object-fit:contain`。
- 硬件照：让物体成为第一视口信号。照片要大到可检视。
- 生成图：只生成图内容，不生成带文字的最终海报。
- 不要让生成图内嵌页面 chrome、标题、边框、caption 或假 UI，除非概念明确需要。

## Bad Smells

看到这些就改：

- 一页看起来像通用模板，文章贴进去。
- 多页大块下方空白。
- 不解释任何事的装饰形状。
- 截图小到看不清。
- 页间 margin 不一致。
- 深照片上蓝字对比差。
- 1:1 封面是 21:9 封面的拥挤裁切。
- 粗大 serif 标题（weight 700+）——读成 landing page，不是 Claude。
- 平铺奶油背景无氛围层——读成 SaaS，不是暖编辑。
- 冷蓝/青强调色——读成 OpenAI/Google，不是 Anthropic。
- 珊瑚用得到处都是——稀缺是品牌电压，泛滥是杂乱。

## Style Identity Test

一个海报编译通过远早于它的风格身份正确。交付前每页跑这个测试。

### Claude Warm Editorial Identity Test

一个海报是 Claude 暖编辑**仅当以下全部成立**：

1. **背景有至少一层氛围**——纸纹、珊瑚光晕径向、冻结 WebGL canvas、或墨流场（见 `background-systems.md`）。全页纯 `#faf9f5` 什么都没有不是 Claude。
2. **大标题用 serif 字族**——Cormorant Garamond / 思源宋体 / Noto Serif SC。Inter 做大标题 = 失败。
3. **大标题 weight ≤500**——`.h-display` / `.h-xl` / `.h-md` 的计算 `font-weight` 是 400 或 500。700+ = 失败。
4. **画布是暖奶油**（light 主题）或**深海军**（dark 主题）——不是纯白、不是冷灰、不是米黄叠米黄。
5. **强调色是珊瑚**（或 Forest Warm 的墨绿）——不是冷蓝、不是饱和青、不是紫色。
6. **海报含至少一个**：大图井、serif 引文带 em-italic 英文、左/右边注栏、有真实杂志行层级的账本、深海军产品 mockup 卡、满铺珊瑚 callout。

如果一个海报有 serif 标题、mono 标签满页、结构化 pill 网格、平铺奶油背景——它是 **Swiss-with-a-serif**，不是 Claude 暖编辑。要么加氛围 + 杂志结构，要么诚实地把整组换成别的风格。

## Anti-Patterns From Real Demos

这些都无错渲染、单独看合理，但和干净参考一比就垮。

### Anti-pattern A: 粗大 serif 标题

```html
<!-- 错:inline 700-800 字重在巨大标题上 -->
<h1 style="font-size: 92px; font-weight: 700">如果只能留 5 个,我留这些。</h1>

<!-- 对:类型化类,自动 400-500 字重 + 负字距 -->
<h1 class="h-xl">如果只能<br>留 <em>5 个</em>,<br>我留这些。</h1>
```

"the larger, the lighter" 是硬规则。90px h1 weight 700+ 瞬间把设计从 Claude 暖编辑降级成通用 landing-page editorial。永远用 `.h-display` / `.h-xl` / `.h-md` / `.num-mega`。不要用 inline `font-size` + `font-weight` 绕过它们。

### Anti-pattern B: 暖编辑无氛围

一页有下面全部，即使你意图 Claude 暖编辑，也读成 Swiss-in-disguise：

- 单一平铺奶油色，无纸纹、无珊瑚光晕、无 WebGL。
- Mono 标签（`JetBrains Mono uppercase`）在每个 kicker、foot、pill、行标签上。
- 一个 serif 标题孤零零飘着，无大图、无引文、无边注栏。

用**之一**修：

- 按 `background-systems.md` 加纸纹 + 珊瑚光晕背景。
- 把 mono 标签换成 serif / italic 微文案。
- 引入大图或 serif 引文带栏结构。
- 加深海军产品 mockup 卡做反差。

### Anti-pattern C: 页脚重叠（结尾随笔撞 `.foot`）

`.foot` 或 `.issue-strip` 用 `position: absolute; bottom: ...` 定位时，上方内容增长超过预期高度会穿过页脚。用这些安全模式之一：

```css
/* 模式 A — flex 列。Foot 是最后一个子元素,被 margin-top: auto 推下。 */
.poster .pad { display: flex; flex-direction: column; height: 100%; }
.poster .foot { margin-top: auto; }

/* 模式 B — grid 固定页脚行。 */
.poster .pad { display: grid; grid-template-rows: auto 1fr auto; height: 100%; }
```

种子模板已用模式 A。保留它。某 recipe 真需要绝对定位 foot 时，在内容容器上预留 `padding-bottom: <foot-height + 24px>`。

### Anti-pattern D: 满铺照片标题压主体

海报 100% 缩放看戏剧化，缩略图尺寸不可读。两个常见子失败：

- Hero 照片覆盖整张海报，标题直接坐在背后碰巧的像素上。图片自身亮度跨画布变化，标题在某些带可读、某些带消失。
- 遮罩层在但只在一个带（底部），而主体脸被 `object-position: center top` 推到顶部。大标题落在脸上。

两个都过 HTML lint；都过缩略图可读测试才垮。

按 `image-overlay.md` 修：

1. **先资质照片。** 需要低细节 quiet zone 和受控亮度。不过就换成 framed-photo recipe，不要硬压字。
2. **放标题前映射主体。** 读图，用自然语言描述脸/焦点特征在哪，把主体映射记成 HTML 注释放在 hero 块旁边。文字只放记录过的安全区。
3. **`object-position` 匹配主体位置**——见 `image-overlay.md` 表格。人脸/hero 镜头永远不要留默认。
4. **缩略图测试**——渲染 PNG，缩到 360px 宽，看标题。和照片打架时，移标题、换照片、或加局部与图片色调匹配的 tint；照片看着死气沉沉时，tint 太重或照片本身不适合压字。

这个反模式几乎过所有其他检查，因为 HTML 有效、图加载、标题渲染。唯一抓法是看读者实际会看到的尺寸的渲染输出。

### Anti-pattern E: 冷蓝/青强调（Claude 品牌叛逃）

```html
<!-- 错:冷蓝强调,读成 OpenAI/Google -->
<button style="background: #002FA7">Try now</button>
<a style="color: #315d93">Learn more</a>

<!-- 对:珊瑚强调,Anthropic 签名 -->
<button class="btn-coral">Try now</button>
<a class="link-coral">Learn more</a>
```

Claude 的品牌电压是珊瑚（`#cc785c`），不是冷蓝。即使用户说"加个蓝色按钮"，也要先解释 Claude 用珊瑚——冷蓝会把整张海报读成"又一个 AI 工具"。如果用户坚持要冷蓝，那是用户的选择，但默认永远是珊瑚。

### Anti-pattern F: 珊瑚泛滥

```html
<!-- 错:珊瑚用得到处都是 -->
<div style="background: #cc785c">标题</div>
<div style="background: #cc785c">副标题</div>
<div style="background: #cc785c">按钮</div>
<div style="background: #cc785c">标签</div>

<!-- 对:珊瑚稀缺,只在 CTA 和 callout 卡上 -->
<h1 class="h-display">标题</h1>  <!-- 墨字 -->
<p class="body">副标题</p>       <!-- body 色 -->
<button class="btn-coral">按钮</button>  <!-- 珊瑚 -->
<span class="badge-cream">标签</span>    <!-- 奶油卡 -->
```

珊瑚在单个元素上稀缺（一个按钮、一条链接、一个标签），只在满铺 callout 卡上慷慨（`callout-card-coral`）。泛滥的珊瑚读成杂乱，不是品牌电压。
