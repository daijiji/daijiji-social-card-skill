# Theme Presets

一个图组用一个主题。不要跨页混色板，除非用户明确要分章节系统。

所有主题色值源自 `DESIGN-claude.md` 的 Anthropic Claude 产品视觉系统。色板分两层：**基础 token**（画布/墨/正文/muted/hairline）+ **强调 token**（珊瑚主色 + 深海军产品面 + 辅助 accent）。

## Claude Warm Editorial Palettes

5 个主题，4 个 light + 1 个 dark。所有 light 主题共享同一套基础 token（Claude 的奶油/墨/正文是品牌恒量），差异在强调色和氛围层。dark 主题独立。

### Claude Canvas（默认）

最 Claude 的主题。奶油画布 + 深暖墨 + 珊瑚强调 + 深海军产品面。用于多数内容——AI 思考、产品解读、职场随笔、文化评论、中性编辑帖。

```css
:root,
[data-theme="claude-canvas"] {
  /* 基础 token — Claude 品牌恒量 */
  --canvas:           #faf9f5;  /* 暖奶油画布,品牌差异点 */
  --surface-soft:     #f5f0e8;  /* 段分隔,极柔带 */
  --surface-card:     #efe9de;  /* 内容卡,比画布深一档 */
  --surface-cream:    #e8e0d2;  /* 最强奶油,选中态/强调带 */
  --ink:              #141413;  /* 暖深墨,所有标题+主文 */
  --body:             #3d3d3a;  /* 默认正文 */
  --body-strong:      #252523;  /* 强调段,引导文 */
  --muted:            #6c6a64;  /* 副标题,面包屑 */
  --muted-soft:       #8e8b82;  /* caption,版权 */
  --hairline:         #e6dfd8;  /* 1px 边线,奶油面 */
  --hairline-soft:    #ebe6df;  /* 同带内极淡分隔 */

  /* 强调 token */
  --coral:            #cc785c;  /* 珊瑚主色,Anthropic 签名 */
  --coral-active:     #a9583e;  /* 按压态 */
  --coral-soft:       #e6dfd8;  /* 禁用态 */
  --coral-on:         #ffffff;  /* 珊瑚上的字 */
  --navy:             #181715;  /* 深海军产品面 */
  --navy-elevated:    #252320;  /* 深海军内嵌卡 */
  --navy-soft:        #1f1e1b;  /* 代码块底 */
  --navy-on:          #faf9f5;  /* 深海军上的字,奶油调白 */
  --navy-on-soft:     #a09d96;  /* 深海军上次要字 */
  --accent-teal:      #5db8a6;  /* 辅助 teal,极少用 */
  --accent-amber:     #e8a55a;  /* 辅助 amber,极少用 */

  /* RGB 变体,供 rgba() 用 */
  --ink-rgb:          20,20,19;
  --canvas-rgb:       250,249,245;
  --coral-rgb:        204,120,92;
  --navy-rgb:         24,23,21;
}
```

### Coral Callout

珊瑚满铺强调版。封面、CTA 气质、宣言式海报、需要电压的瞬间。**不要整组图都用这个主题**——它是 Claude Canvas 的强调变体，用于单张封面或 callout 页，混在 Claude Canvas 组里。

```css
[data-theme="coral-callout"] {
  /* 基础 token 翻转:珊瑚做画布 */
  --canvas:           #cc785c;  /* 珊瑚满铺 */
  --surface-soft:     #c46e52;  /* 略深珊瑚 */
  --surface-card:     #b86347;  /* 更深,卡背景 */
  --surface-cream:    #a9583e;  /* 最深,强调带 */
  --ink:              #faf9f5;  /* 奶油色字 */
  --body:             #f5ede4;  /* 奶油偏暖 */
  --body-strong:      #ffffff;
  --muted:            #e6dfd8;
  --muted-soft:       #d8cfc2;
  --hairline:         rgba(250,249,245,.22);
  --hairline-soft:    rgba(250,249,245,.12);

  /* 强调 token:奶油做次表面 */
  --coral:            #faf9f5;  /* 奶油做按钮/卡 */
  --coral-active:     #efe9de;
  --coral-soft:       #e8e0d2;
  --coral-on:         #141413;  /* 奶油上是墨字 */
  --navy:             #181715;  /* 深海军仍可用 */
  --navy-elevated:    #252320;
  --navy-soft:        #1f1e1b;
  --navy-on:          #faf9f5;
  --navy-on-soft:     #a09d96;

  --ink-rgb:          250,249,245;
  --canvas-rgb:       204,120,92;
  --coral-rgb:        250,249,245;
  --navy-rgb:         24,23,21;
}
```

Coral Callout 必须覆盖氛围层——奶油纸纹在珊瑚上不对：

```css
[data-theme="coral-callout"] .grain {
  opacity: .18;
  mix-blend-mode: overlay;
  background-image: radial-gradient(rgba(255,255,255,.12) 1px, transparent 1px);
}
[data-theme="coral-callout"] .paper-wash {
  background:
    radial-gradient(80% 50% at 28% 16%, rgba(255,255,255,.10), transparent 64%),
    radial-gradient(70% 60% at 80% 86%, rgba(24,23,21,.18), transparent 72%);
}
[data-theme="coral-callout"] .frame-img {
  background: #b86347;
  box-shadow: 0 0 0 1px rgba(250,249,245,.18);
}
```

### Dark Product

深海军为主、奶油为辅。代码编辑器 mockup、模型对比卡、终端输出、技术内容、开发者页。Claude 在产品 chrome 上用深海军——这是那个节奏。

```css
[data-theme="dark-product"] {
  /* 基础 token 翻转:深海军做画布 */
  --canvas:           #181715;  /* 深海军画布 */
  --surface-soft:     #1f1e1b;  /* 略亮深海军 */
  --surface-card:     #252320;  /* 内嵌卡 */
  --surface-cream:    #2a2825;  /* 最亮深海军,强调带 */
  --ink:              #faf9f5;  /* 奶油色主字 */
  --body:             #d8d4cc;  /* 奶油偏暖正文 */
  --body-strong:      #ffffff;
  --muted:            #a09d96;  /* 深海军上次要 */
  --muted-soft:       #6c6a64;
  --hairline:         rgba(250,249,245,.14);
  --hairline-soft:    rgba(250,249,245,.08);

  /* 强调 token:珊瑚仍做 CTA,奶油做次表面 */
  --coral:            #cc785c;
  --coral-active:     #a9583e;
  --coral-soft:       #3a3530;
  --coral-on:         #ffffff;
  --navy:             #faf9f5;  /* 奶油做"navy"角色,反差 */
  --navy-elevated:    #efe9de;
  --navy-soft:        #e8e0d2;
  --navy-on:          #141413;
  --navy-on-soft:     #6c6a64;
  --accent-teal:      #5db8a6;
  --accent-amber:     #e8a55a;

  --ink-rgb:          250,249,245;
  --canvas-rgb:       24,23,21;
  --coral-rgb:        204,120,92;
  --navy-rgb:         250,249,245;
}
```

Dark Product 氛围层覆盖：

```css
[data-theme="dark-product"] .grain {
  opacity: .22;
  mix-blend-mode: screen;
  background-image: radial-gradient(rgba(250,249,245,.10) 1px, transparent 1px);
}
[data-theme="dark-product"] .paper-wash {
  background:
    radial-gradient(80% 50% at 28% 16%, rgba(204,120,92,.10), transparent 64%),
    radial-gradient(70% 60% at 80% 86%, rgba(0,0,0,.30), transparent 72%),
    linear-gradient(180deg, rgba(250,249,245,.02), rgba(0,0,0,.20));
}
[data-theme="dark-product"] .frame-img {
  background: #252320;
  box-shadow: 0 0 0 1px rgba(250,249,245,.10);
}
```

### Forest Warm

暖奶油 + 森林墨绿强调。户外、徒步、可持续、自然笔记、接地气生活方式。Claude Canvas 的强调色变体——把珊瑚换成森林墨绿，氛围层加一点苔藓感。

```css
[data-theme="forest-warm"] {
  /* 基础 token 同 Claude Canvas */
  --canvas:           #faf9f5;
  --surface-soft:     #f5f0e8;
  --surface-card:     #efe9de;
  --surface-cream:    #e8e0d2;
  --ink:              #141413;
  --body:             #3d3d3a;
  --body-strong:      #252523;
  --muted:            #6c6a64;
  --muted-soft:       #8e8b82;
  --hairline:         #e6dfd8;
  --hairline-soft:    #ebe6df;

  /* 强调 token:珊瑚 → 森林墨绿 */
  --coral:            #2e6b4f;  /* 森林墨绿做主强调 */
  --coral-active:     #245540;
  --coral-soft:       #d4dfd2;
  --coral-on:         #ffffff;
  --navy:             #181715;
  --navy-elevated:    #252320;
  --navy-soft:        #1f1e1b;
  --navy-on:          #faf9f5;
  --navy-on-soft:     #a09d96;
  --accent-teal:      #5db8a6;
  --accent-amber:     #e8a55a;

  --ink-rgb:          20,20,19;
  --canvas-rgb:       250,249,245;
  --coral-rgb:        46,107,79;
  --navy-rgb:         24,23,21;
}
```

Forest Warm 氛围层加苔藓径向：

```css
[data-theme="forest-warm"] .paper-wash {
  background:
    radial-gradient(80% 50% at 28% 16%, rgba(46,107,79,.08), transparent 64%),
    radial-gradient(70% 60% at 80% 86%, rgba(24,23,21,.06), transparent 72%);
}
```

### Midnight Claude

**唯一**官方深色 Editorial 主题。游戏 key art、夜景摄影、电影感封面、深调文化内容——源图本身已深、奶油背景会削弱的内容。不要即兴造第二个深色主题；Midnight Claude 不合身就换主题（深色暖编辑不是通用开关）。

```css
[data-theme="midnight-claude"] {
  /* 基础 token:深海军画布 + 奶油字 */
  --canvas:           #181715;
  --surface-soft:     #1f1e1b;
  --surface-card:     #252320;
  --surface-cream:    #2a2825;
  --ink:              #faf9f5;
  --body:             #d8d4cc;
  --body-strong:      #ffffff;
  --muted:            #a09d96;
  --muted-soft:       #6c6a64;
  --hairline:         rgba(250,249,245,.14);
  --hairline-soft:    rgba(250,249,245,.08);

  /* 强调 token:珊瑚 + amber 双暖强调(深色场景下 amber 更显) */
  --coral:            #cc785c;
  --coral-active:     #a9583e;
  --coral-soft:       #3a3530;
  --coral-on:         #ffffff;
  --navy:             #faf9f5;
  --navy-elevated:    #efe9de;
  --navy-soft:        #e8e0d2;
  --navy-on:          #141413;
  --navy-on-soft:     #6c6a64;
  --accent-teal:      #5db8a6;
  --accent-amber:     #e8a55a;  /* 深色下 amber 升级为主辅强调 */

  --ink-rgb:          250,249,245;
  --canvas-rgb:       24,23,21;
  --coral-rgb:        204,120,92;
  --navy-rgb:         250,249,245;
}
```

Midnight Claude 必须覆盖氛围层——亮纸纹数学不对：

```css
[data-theme="midnight-claude"] .grain {
  opacity: .26;
  mix-blend-mode: screen;
  background-image: radial-gradient(rgba(250,249,245,.10) 1px, transparent 1px);
}
[data-theme="midnight-claude"] .paper-wash {
  background:
    radial-gradient(80% 50% at 28% 16%, rgba(204,120,92,.12), transparent 64%),
    radial-gradient(70% 60% at 80% 86%, rgba(0,0,0,.30), transparent 72%),
    linear-gradient(180deg, rgba(250,249,245,.02), rgba(0,0,0,.32));
}
[data-theme="midnight-claude"] .frame-img {
  background: #1f1e1b;
  box-shadow: 0 0 0 1px rgba(250,249,245,.10);
}
```

种子 `template-claude-card.html` 内置这些覆盖——切 `data-theme` 自动应用。

## 主题使用规则

- **一个图组一个主题。** 不要跨页混色板。
- **Claude Canvas 是默认。** 没有明确偏好时用它。
- **Coral Callout 是单页强调变体，不是整组主题。** 一组 Claude Canvas 图里插一张 Coral Callout 封面或 callout 页是合理的；整组用 Coral Callout 会过载。
- **Dark Product 用于技术/产品内容。** 代码、模型对比、终端、开发者叙事。
- **Forest Warm 用于户外/自然。** 把珊瑚换成墨绿，氛围加苔藓。
- **Midnight Claude 是唯一深色 Editorial。** 源图已深时用。不要即兴造第二个深色。
- **`--coral` 在 light 主题上稀缺，在 Coral Callout 上慷慨。** 单个按钮、单条链接、单个标签用珊瑚；满铺 callout 卡才用珊瑚做面。
- **`--navy` 是产品面，不是装饰。** 代码块、mockup 卡、模型对比表、页脚用深海军。不要在奶油画布上随便铺深海军色块。
- **不要用冷灰或纯白做画布。** `#ffffff` 读成"又一个 AI 工具"。奶油是品牌。
- **不要用冷蓝或饱和青做强调。** 珊瑚是品牌电压。teal/amber 是辅助，极少用。
- **light 主题不要变成米黄叠米黄。** 维持真实对比——墨字在奶油上、深海军卡在奶油上、珊瑚做锐利强调。
- **Midnight Claude 不要堆不透明卡或填色块。** 深色暖编辑靠照片满铺 + 暖珊瑚/amber 强调做层级，不靠背景块。
