---
name: daijiji-social-card-skill
description: Generate Daijiji-style social card image sets and WeChat official account cover pairs from articles, scripts, screenshots, product notes, subtitles, or photos. Use when the user asks for 小红书图文, Rednote/Xiaohongshu images, social cards, carousel images, 3:4 covers, 微信公众号封面, WeChat 21:9 + 1:1 covers, Claude warm-editorial cards, or magazine-warm social images.
---

# Daijiji Social Card Skill

基于 Anthropic Claude 产品视觉语言定制的社交卡片生成技能。产出小红书 3:4 图组、微信公众号 21:9 + 1:1 封面配对、文章封面、平台缩略图。

本技能从 `guizang-social-card-skill` 演化而来，但**不沿用**原技能的 Editorial 杂志风 / Swiss 国际主义风二分法。它围绕 Claude 的暖编辑视觉系统重新设计——奶油画布 + 珊瑚强调 + 深海军产品面，serif 标题 + 人文 sans 正文，文学性而非工程感。

## What To Produce

适用场景：

- 小红书 / Rednote 图组：封面 + 内容页，3:4 比例。
- 微信公众号封面配对：一张 `21:9` 主封面 + 一张 `1:1` 方封面，在同一 HTML 内组合预览。
- 截图密集的产品帖、文章封面、教程轮播、户外/生活方式笔记、AI/产品更新解读。
- 需要 Claude 暖编辑气质的社交图——比 SaaS 冷静，比杂志克制，比 Swiss 暖。

不适用：

- 完整的横向 PPT 演示文稿。用 PPT 技能。
- 长视频生成。用视频技能。
- 纯图片编辑，无版式或文章抽取需求。

### 小红书品类能力圈

11 个常见小红书品类分三档。详见 `references/category-cookbook.md`。

**强端到端**（文案、结构、图故事都在能力内）：

- 旅行、职场、推荐（指定子类型后）。

**强文案结构，图需用户供给或图库取**：

- 游戏、影视、美食（食谱方向）、彩妆（教程方向）、健身、家居、穿搭（精选/随笔方向）。

**能力外——诚实回退而非硬上**：

- 美食菜品大片摆盘、穿搭日常 OOTD 全身、情感梦核/氛围装饰风、Y2K/千禧辣妹/哥特萝莉/kawaii 装饰风、纯摄影展示帖。

第三档请求进来时，先说清楚做不到什么，不要静默改造成一个偏离用户意图的版式。

## Core Principle

表达优先。目标不是把文字塞进海报，而是把素材转成清晰的视觉论证。

每页先想清楚：

- 一眼能让读者 get 到什么？
- 哪张截图、照片、图示能支撑这个观点？
- 哪些字必须大，哪些可以退成 caption 或 metadata？
- 哪些内容属于正文，不该上图，可以删掉？

## Required References

按需读：

- `references/platform-specs.md` — 精确比例、输出尺寸、命名规范。
- `references/style-system.md` — Claude 暖编辑视觉规则、身份测试、反模式。
- `references/theme-presets.md` — Claude 主题色板（4 个 light + 1 个 dark）。
- `references/layout-recipes.md` — 卡片/封面/微信页结构配方（C01-C14）。
- `references/components.md` — 共享组件规范：字体栈、字号尺度、最小可读尺寸、中文标题长度带、卡片互斥规则、图片容器比例类、间距 token、Lucide 图标规则。
- `references/background-systems.md` — 奶油纸纹 + 珊瑚光晕 + 深海军氛围层。
- `references/portrait-fill.md` — 3:4 适配，避免竖向欠填。
- `references/content-planning.md` — 封面钩子、分页、文案压缩。
- `references/production-workflow.md` — HTML/CSS 渲染与图片处理。
- `references/image-overlay.md` — 文字压在照片上时的照片资质、局部 tint、人脸/主体避让。
- `references/screenshot-treatment.md` — 应用/网页/代码/仪表盘截图处理，`.frame-shot` vs `.frame-img`，圆角/阴影/底色/内边距，`.device-browser` / `.device-phone` 外壳。
- `references/title-shortener.md` — 微信 21:9+1:1 封面配对、跨平台复用时从长标题派生短标题。
- `references/category-cookbook.md` — 把小红书品类名路由到对应配方并确认能力圈。
- `references/qa-checklist.md` — 交付前 QA。

## Workflow

### 1. Intake

只收集会改变产出的缺失信息：

- 目标平台与比例。
- 源文本、字幕、文章或标题。
- **小红书品类**——用户提到 11 个常见类型之一时，按 `references/category-cookbook.md` 路由并确认在能力圈内。能力外的请求，**设计前**先告知用户，不要静默改造。
- 用户供给的图片/截图及各自出现位置。**新闻/教程/数据/评测类内容，主动追问截图或照片**——它们是证据层。没有真实素材的海报容易读成填充物。
- **用户只给文字（完全没图）时，问一次：**

  ```
  这篇我需要 1-2 张图。三种走法：
  A. 你自己有照片 / 截图，传给我（推荐——最不"AI 感"）
  B. 我去 Pexels / Unsplash / Flickr 帮你找
  C. 用 AI 生成
  ```

  一句话推荐 A——自己的照片是让海报不像 AI 生成的关键。用户选什么都接受（包括"都行你看着办"），然后继续。**不要再追问，不要跨多轮反复推 A。** 这是一次性问题。
- 偏好风格（如指定）：Claude 暖编辑（默认）、深海军产品面、珊瑚 callout 强调等。
- 硬约束：标题文字、1:1 封面无图、必须含硬件照、截图保持可读等。

用户已给足上下文时，按合理假设推进。

内容涉及当前产品发布、政策、价格、声明或新闻时，用浏览核实不稳定事实，并在最终回复中引用来源。

### 2. Extract The Story

设计前先把素材转成页面计划。

小红书：

- 第 1 页是封面钩子。
- 第 2-N 页每页只承载一个观点。
- 多数帖用 5-9 页。低位空白时压缩或合并页面。
- 细节放正文，图承载钩子、对比、清单、锐利结论。

微信：

- 永远产出配对系统：`21:9` 主封面 + `1:1` 方封面。
- 两个封面建在同一 HTML 文件里，加一个组合预览段，方便一起检查视觉关系。
- `21:9` 保留完整或近完整标题、副标题、一个强视觉关系。
- `1:1` 用从长标题派生的简化短标题：大字居中、默认无图、无拥挤副标题。

### 3. Choose Theme

Claude 暖编辑系统**只有一个视觉模式**，但有 5 个主题色板。一个图组用一个主题，不混。

**Claude Warm Editorial** 带来：

- Cormorant Garamond / 思源宋体 serif 标题 + Inter / 思源黑体 sans 正文。
- 暖奶油画布 + 深暖墨字 + 珊瑚强调 + 深海军产品面。
- 纸纹 + 珊瑚光晕氛围层（不是平铺奶油色）。
- 杂志栏、引文、大图井、产品 mockup 卡——文学 + 产品双气质。
- 适合：想让页面读起来慢、被斟酌过、手工排过的感觉。

5 个主题（详见 `references/theme-presets.md`）：

1. **Claude Canvas**（默认）—— 奶油画布 + 珊瑚 + 深海军。最 Claude。
2. **Coral Callout** —— 珊瑚满铺强调版，封面/CTA 气质。
3. **Dark Product** —— 深海军为主、奶油为辅。产品 mockup、代码、技术内容。
4. **Forest Warm** —— 暖奶油 + 森林墨绿强调。户外、可持续、自然笔记。
5. **Midnight Claude** —— 唯一深色主题。游戏 key art、夜景摄影、电影感封面。

不要在同一图组里混主题，除非用户明确要分章节系统。

### 4. Plan Pages

写一个简洁的内部计划：

```text
Page 01 / cover / hook / image source / layout intent
Page 02 / point / key copy / visual evidence / layout intent
...
```

用户要审批时，渲染前先展示这个计划。否则内部用，直接推进。

用 `references/layout-recipes.md` 选页面结构。避免每页都是重复的"标题+卡片"。

3:4 图先查 `references/portrait-fill.md` 再写代码。短表或账本必须扩展成完整竖向构图——加引文列、图证据、边注、更大的行、背景 hero 区。

### 4.5. Copy The Seed Template

不要从零写 HTML。基于 Step 3 选的主题复制种子模板：

- 所有主题共用 `assets/template-claude-card.html`，复制到任务文件夹为 `index.html`。
- 种子已接好：字体加载、主题 token、三种海报尺寸（`.poster.xhs` / `.poster.square` / `.poster.wide`）、配对预览框、纸纹/氛围层、所有 layout recipes 引用到的类名。

在 `<html>` 元素上设主题：

```html
<html data-theme="claude-canvas | coral-callout | dark-product | forest-warm | midnight-claude">
```

把 `<!-- POSTERS_HERE -->` 后的单个占位海报替换成每页一个 `<section class="poster ...">` 块，每块承载一个 Layout Recipe（C01-C14）的 HTML 骨架。

### 5. Build And Render

默认实现模式：

- 在当前工作区建任务文件夹，如 `social-card-<slug>/`。
- 源图放 `assets/`。
- 从 Step 4.5 复制种子模板开始，不要从空白文件开始。优先只改 `<!-- POSTERS_HERE -->` 区域逐页替换。任务需要自定义布局 CSS 时，在复制后的文件里加一个命名清晰的任务作用域块，保留语义默认重置（`figure { margin:0; }`，无浏览器默认间距意外）。
- 用 Playwright 或浏览器截图工具导出每个 `.poster` 或 `.cover` 节点。
- 渲染图存 `output/`。
- 校验尺寸并检查渲染出的 PNG。
- 保留 `node validate-social-deck.mjs <task-dir>` 用于自动核查。它检查溢出（R1）、页脚碰撞（R2）、serif 标题字重（R3）、最小字号（R4）、4 横带密度（R5）、`.h-xl` 行数上限（R6）、浏览器默认 figure margin 漂移（R7）。任何 FAIL 退出码 1——自动核查请求时交付前修复。WARN 是建议，但要读。

不要在图里放可见指令、键盘快捷键或使用说明。

Claude 暖编辑用分层背景系统。优先：纸纹 + 珊瑚光晕径向渐变 + 可选 WebGL 墨流 canvas。读 `references/background-systems.md`；不要靠平铺奶油色，不要加全页网格/点阵背景。

### 6. Image And Screenshot Handling

用户给截图时：

- 除非用户要求重设计，保留截图内容。
- 优先程序化装框：目标比例画布、安全内边距、干净背景、可读截图。
- 不要拉伸截图。
- 截图清晰度重要时，放大截图区，缩小附近文字。

#### Text-On-Image Composition

海报把文字压在照片上时（满铺封面、大图井、生成图叠字），遵循 `references/image-overlay.md`：

- **先选图，必要时才 tint。** 覆盖 ≥60% 画布的照片必须先过 quiet-zone 和亮度测试。先无遮罩构图；缩略图检查不过时，只在标题区加局部、与图片色调匹配的 tint。不要默认全画布渐隐。
- **主体映射是强制的。** 放标题前，用 Read 工具读图，用自然语言描述主体脸/焦点特征在哪，把主体映射记成 HTML 注释放在 hero 块旁边。文字只放在记录过的安全区。
- **裁剪纪律——每张照片 inline 设 `object-position`。** 模板默认（`center 50%`）是兜底，不是推荐。每张 `<img>` 基于主体位置决定并 inline 写出：如 `style="object-position:center 62%"` 给中段主体，`center 30%` 给天空重的风景，`center 70%` 给前景装备。见 `references/components.md` 表格和 `image-overlay.md` 人脸照片细节。跳过这步会在高比例（`r-3x4`、`r-21x9`）上静默裁掉主体。
- **缩略图测试。** 把渲染出的 PNG 缩到 360px 宽，确认标题仍可读。标题和照片打架时，移标题、换照片、或加局部与图片色调匹配的 tint；照片看着死气沉沉时，tint 太重或照片本身不适合压字。

深色封面（如游戏 key art 上的杂志感）和带 hero 照片的封面都要做这些检查。跳过是已知失败模式（见 `style-system.md` 反模式 D）。

用户没图时：

- 这条分支只在 Step 1 "三选一" 落到 B（网络取图）或 C（AI 生成）时跑。不要静默滑进 B 或 C——用户选过。
- C（AI 生成）：只在真正增值处用生成图，通常 1-2 页。生成匹配页面视觉角色的图，不是通用装饰。生成图不要内嵌标题、页码、logo 或假 UI 标签，除非概念明确需要。
- B（网络取图）：见下面 Web-Sourced Images 段。

#### Web-Sourced Images（用户没图时的兜底）

用户没截图/照片且生成图不适合页面角色时（如氛围照、户外/生活方式背景、游戏封面、真实产品照），从网络取，不要让页面发空。

策略：**先取，后告知，让用户决定署名。** 不要按猜测的许可证预过滤来源——用户是最终合成的权利人，决定什么可接受。

推荐来源，按优先级。**下面五个都是免费图库，无强制授权费**；不从付费图库（视觉中国/Getty/站酷海洛等）取。

1. **Unsplash** — `https://unsplash.com/s/photos/<keyword>`。户外/生活方式/氛围背景强。英文关键词最佳。许可证宽松但逐案核实。
2. **Pexels** — `https://www.pexels.com/search/<keyword>/` 或 `https://www.pexels.com/zh-cn/search/<keyword>/`。**原生支持中文关键词**——补 Unsplash 在国内场景的缺口（中文街景/国风物件/本地地名）。中文/中国特定主题时优先用。Pexels License 下免费。
3. **Flickr CC 授权池** — `https://www.flickr.com/search/?text=<keyword>&license=2%2C3%2C4%2C5%2C6%2C9`。许可证过滤限 Creative Commons。补"纪实真实感"缺口：街拍、人在场景中、真实室内、非摆拍场景。用户选署名时保留 CC 署名。
4. **Wallhaven** — `https://wallhaven.cc/search?q=<keyword>`。游戏/动漫/壁纸主题强。内容用户上传，版权未核实。
5. **直接网络搜索** — 需要特定主体时（产品渲染图、游戏剧照、历史照片）。用 WebFetch / WebSearch 找候选 URL。

取图方式：

- 用 WebFetch 或 `curl` 下载到任务文件夹的 `assets/`。
- 按用途命名，不按 hash：`assets/hero-mountain.jpg`、`assets/ui-pulse-card.png`。
- 在 `assets/SOURCES.md` 记录来源 URL（每文件一行：`hero-mountain.jpg ← <url>`）。即使用户最终选不署名也要记——保留出处给人类作者。

取图后，**定稿前**把出处告知用户：

```
我从 <site> 取了这些图：
- assets/hero-mountain.jpg — <url>
- assets/ui-pulse-card.png — <url>

⚠️ 版权未经核实。请你判断是否可用。
是否需要在图文中标注来源？
- 要：我把 "Photo · <site> · @<author>" 加到对应页脚 / 角标。
- 不要：原样使用,不加注释。
```

用户选"标注"——加小 `mono` caption（`.t-meta` 18-20px 角落）。不要挤进布局焦点区。

用户选"不标注"——静默推进。出处仍活在 `assets/SOURCES.md` 供用户自查。

图只是合成中一个元素时（如九宫格里的一张），用户可合理跳过署名。不要强加破坏布局的署名标签。

### 7. Deliver

**先给用户看，按需校验。** 每次渲染后自动跑校验器太慢，耽误用户看结果。默认流程：

1. 渲染完成后，立即把渲染图内联展示给用户（绝对路径）+ 一句话总结做了什么。
2. 问一个问题：**"先你自己看，还是我先自动核查一遍？"**
3. 用户说"我自己看"/"先给我"/"no need"——停在这，让用户检查，回应用户提出的任何问题。
4. 用户说"你查吧"/"auto-check"/"yes"——才跑 `node validate-social-deck.mjs <task-dir>`，修 FAIL，重新渲染后交付。提一下密度/上限 WARN。

不要在给用户看之前静默跑校验器——每遍几分钟，用户通常更快发现问题。

最终回复（用户已审或要求自动核查后）应含：

- 输出文件夹路径。
- 渲染图内联展示，绝对路径，有用时。
- 关于尺寸和校验的短注（或"未校验，待你审"）。
- 任何网络取图：来源 URL + 站点 + 用户做的署名决定。
- 任何未解决风险，如源图分辨率低。

## Non-Negotiables

- 永远不编辑原 guizang-social-card-skill 或任何上游被复制的技能。
- 不要造随机装饰 SVG 椭圆、blob、雨滴、贴纸、无意义圆圈。
- 不要用嵌套卡片或通用 SaaS 卡片布局作为默认。
- 不要让文字溢出、贴边、或撞页脚带。`.foot` 用 `margin-top: auto` 钉在 flex 列里，不要用 `position: absolute` 盖在增长内容上。
- 不要让文字小到手机上看不清。
- **不要在 serif 标题上 inline 写 `font-size` + `font-weight`。** 用类型化类（`.h-display` / `.h-xl` / `.h-md` / `.num-mega`）。Claude 暖编辑的标题是 serif weight 400-500 + 负字距，不是 700-900 粗体。粗大 serif 标题读成"通用 landing-page"，不是 Claude。
- 不要交付只有平铺奶油色背景、mono 标签满页、无氛围层的暖编辑海报。跑 `references/style-system.md` 的 Editorial Identity Test——光一个 serif 标题不构成 Claude 暖编辑。
- 不要造假数据、发布细节、百分比。
- 不要裁掉脸、关键 UI 文字、硬件/产品细节，除非用户明确接受。
- 不要把 21:9 封面盲目裁成 1:1。每个比例分别构图。
- **3:4 卡必须吃满画布。** 内容（文字+图+数据）必须覆盖 ≥75% 画布高度。任何 >15% 画布高度的纯空白带都需要"留白理由"：(a) hero image 自带呼吸、(b) 单句宣言式 hero statement、(c) 段落顶/底 leading & trailing whitespace（前后总和 ≤15%）。**禁止用 `<div style="flex: 1"></div>` 上下夹击把内容塞到中段**——杂志页留白逻辑不适用于社交卡。每条 recipe 的最小密度见 `references/layout-recipes.md` 的「Minimum density」段。渲染后必须跑 `qa-checklist.md` 的 4 横带密度检查。
- **不要用冷灰或纯白做画布。** 奶油色是品牌差异点。`#ffffff` 读成"又一个 AI 工具"。
- **不要用冷蓝或饱和青做强调色。** 珊瑚是品牌电压。
- **不要把珊瑚用得到处都是。** 珊瑚在单个元素上稀缺，只在满铺珊瑚 callout 卡上慷慨。
- **不要用 Inter 做标题。** serif 字符是品牌声音。
- **不要连续两个 band 用同一种表面模式。** 节奏交替：奶油 → 奶油卡 → 深海军 mockup → 奶油 → 珊瑚 callout → 深海军页脚。
