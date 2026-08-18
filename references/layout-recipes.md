# Layout Recipes

Claude 暖编辑静态社交图配方。不是从 Claude.com 复制的页面模板——是从其视觉语言提取的版式结构。

## Claude Warm Editorial Recipes

这些结构（账本/边注/引文/图井/产品 mockup/竖向 pipeline）适用于**任何**想要 Claude 暖编辑气质的话题——AI、产品、户外、职场、文化、游戏都欢迎。模式是视觉姿态，不是内容过滤器。

竖向规则：

- 每个 3:4 页应有意占据竖向画布。内容只生成一张薄表时，切到 C08/C09/C10 或加大引文、证据图、边注栏、全高账本。

**内容密度规则（硬）**：1080×1440 卡上，内容必须覆盖 ≥75% 画布高度。任何 >15% 画布高度（>216px）的纯空白带需要说明理由——hero 图呼吸、单句宣言、或顶/底 leading & trailing（合计 ≤15%）。**不要**用 `<div style="flex: 1"></div>` 把内容推到竖向中心——社交卡逐张刷，欠填读成"PowerPoint 漏排"。每条 recipe 下面带 `Minimum density:` 行，标 3:4 画布最小内容集。文案达不到地板时，**缩画布（切 1:1）或换 recipe**——永远不要发欠填的。

---

### C01 Cover: Claude Issue Cover

小红书第 1 页、竖向社交卡、文章卡封面最佳。

结构：

- 顶 issue row：品类、日期、期号、或账号标签。小 mono kicker。
- 大 serif/Songti 标题，通常 2-4 行。weight 400-500，负字距。
- 一张大图或照片裁切占 35-55% 页面。`.frame-img` 带 `.r-3x4` 或 `.r-4x3`。
- 底 issue strip：3-5 短点用 em-dash 分隔，hairline 上方。

风格：

- 奶油画布背景，深暖墨标题。
- 照片可在大矩形井内 bleed。
- 珊瑚做一个竖向 rule、页码、或小标签——稀缺。
- 纸纹 + 珊瑚光晕氛围层（见 `background-systems.md`）。

**Minimum density (3:4)**：标题（2-4 行）+ 大图（≥35% 画布）+ 底 issue strip（3-5 点）。三者缺一即欠填。

---

### C02 Field Note Photo

户外、物件、硬件、真实世界观察最佳。

结构：

- 大纪实照（`.frame-img` `.r-3x4` 或 `.r-4x3`，占 ≥50% 画布）。
- 窄 caption 列或底 caption 带。
- 一句短 takeaway 用大 serif 类型。

照片是证据时用，不是装饰。

**Minimum density (3:4)**：大图（≥50%）+ takeaway（1 句大字）+ caption（2-3 行 mono）。图 <50% 时切 C01 或 C06。

---

### C03 Editorial Essay Split

用一个观点带细微差别解释最佳。

结构：

- 左：大标题或引文（`.col-7-5` 或 `.col-8-4`）。
- 右：2-3 短段或编号片段。
- 栏间细 hairline rule。

段落保持短。变密时拆页。

**Minimum density (3:4)**：标题 + 3 短段 或 标题 + 2 段 + 编号底列表。标题单独是 C04，不是 C03。只有标题 + 1 段时，切 C04 引文或折边注栏进 C11。

---

### C04 Pull Quote / Thesis

核心句或结论最佳。

结构：

- 大引文跨页（`.pullquote`，64px serif italic）。
- 小来源/上下文行（`.meta`，18px mono）。
- 可选小注或 issue 标记（`.kicker`）。

在密页之间造节奏时用。

**Minimum density (3:4)**：这是**唯一**允许 ≤60% 画布内容的 recipe（hero 声明 = 刻意留白）。但必须加 (a) 来源/上下文行 18-20px mono 距底 ≤15%，(b) 日期戳或章号 kicker 在顶，(c) hairline rule 在来源行上。没有这三个"锚点"，留白读成缺内容。不能供至少一个锚点时不要用 C04。

---

### C05 Checklist / Buying Guide

小红书实用内容最佳。

结构：

- 头标题（`.h-xl`）。
- 4-6 行，每行编号、项、后果。
- 可选小照片裁切或材质色板。

避免通用圆角卡；用行、rule、栏、issue 标签。

**Minimum density (3:4)**：标题 + 4-6 编号行。每行含编号（mono）+ 项（serif 中标题）+ 后果（sans 正文）。少于 4 行时切 C07 或加图证据。

---

### C06 Evidence Wall

多截图、参考、或小图最佳。

结构：

- 2×2 或 3 栏图网格（`.grid-3` 或 2×2）。
- 每图短 caption（`.img-cap`）。
- 一个更大头条锚定解读（`.h-md`）。

仅在所供图在最终尺寸可读时用。

**Minimum density (3:4)**：头条 + 4-6 图 + 每图 caption。图 <4 张时切 C02 单大图。

---

### C07 Closing Note

末页最佳。

结构：

- 大 serif takeaway 或引文。
- 小"下一步"或 CTA 行。
- 可选珊瑚 `.btn-coral` 或 `.link-coral`（每海报最多一个）。
- 底 issue strip 或签名。

**Minimum density (3:4)**：takeaway（大字 1-2 行）+ 下步行（mono 或 link）+ 底 strip。三者缺一即欠填。

---

### C08 Product Mockup Dark

代码编辑器、终端、模型对比、agent 流程图、开发者产品 chrome 最佳。

结构：

- 深海军卡（`.card-navy`）承载产品 mockup——代码块、终端输出、模型对比表、agent 流程图。
- 奶油栏做说明——标题 + 1-2 短段解释产品 chrome。
- 可选珊瑚 `.btn-coral` 或 `.link-coral` 做"Learn more"。

**Claude 特有**：这是 Claude 展示产品的地方。深海军卡是产品 chrome 的家，不是装饰色块。代码截图优先放深海军卡内，不要放奶油卡。

**Minimum density (3:4)**：深海军 mockup 卡（≥40% 画布）+ 奶油说明栏（标题 + 1-2 段）+ 可选 CTA。mockup <40% 时切 C03 或加更多说明。

---

### C09 Coral Callout

宣言、CTA、强观点、需要电压的瞬间最佳。

结构：

- 满铺珊瑚卡（`.card-coral`）占 ≥60% 画布。
- 大 serif 标题（`.h-xl` 或 `.h-display`），奶油字（`--coral-on`）。
- 可选奶油 `.btn-cream` 做 CTA（在珊瑚上用奶油按钮，不要再用珊瑚按钮）。
- 小 mono 来源或上下文行。

**整组图不要都用 C09**——它是 Claude Canvas 组里的强调页，不是整组主题。一组里插 1-2 张 C09 callout 是合理的；全用 C09 过载。

**Minimum density (3:4)**：珊瑚卡（≥60%）+ 大标题（2-3 行）+ 可选 CTA + 来源行。珊瑚卡 <60% 时切 C07 或 C04。

---

### C10 Comparison Table

两个或多个产品/方案/观点对比最佳。

结构：

- 头标题（`.h-xl`）。
- 对比表或栏——左列项名，右列值/描述。hairline 分隔行。
- 可选一个珊瑚高亮单元（`.card-coral` 单格）标"推荐"或"最佳"。
- 底 takeaway 行。

**Minimum density (3:4)**：标题 + 4-6 对比行 + takeaway。行 <4 时切 C05 清单或加列。

---

### C11 Marginalia Essay

带边注的随笔——主栏正文 + 边栏小注最佳。

结构：

- 左主栏（`.col-8-4` 8 份）：标题 + 2-3 段正文。
- 右边注栏（4 份）：5-7 行小注，每行 mono kicker + sans 短句。
- 栏间 hairline。

**Minimum density (3:4)**：标题 + 3 段（每段 3-4 句）+ 5-7 边注行。段 <3 或边注 <5 时切 C03。

---

### C12 Vertical Pipeline

步骤流程、教程、递进逻辑最佳。

结构：

- 头标题（`.h-xl`）。
- 5 步竖向 pipeline——每步：编号（mono 大字）+ 步骤标题（serif 中标题）+ 步骤描述（sans 正文）。
- 步骤间珊瑚竖向 rule 或 hairline。
- 底 issue strip。

**Minimum density (3:4)**：标题 + 5 步。每步含编号 + 标题 + 描述。步 <5 时切 C05 清单。6 步需缩标题到 1 行。

---

### C13 Stat Hero / KPI

单个大数字、关键指标、震撼统计最佳。

结构：

- 顶 kicker（mono，品类/来源）。
- 巨大数字（`.num-mega`，200px sans weight 200）。
- 数字下 serif 解释（`.h-md`，1-2 行）。
- 底 context 行（mono，来源/日期）。
- 可选珊瑚 accent 做数字单位或小标。

**Minimum density (3:4)**：kicker + 大数字 + 解释 + context。四者缺一即欠填。数字不大（<144px）时切 C04 引文。

---

### C14 WeChat Cover Pair

微信 21:9 + 1:1 封面配对专用。

结构（21:9）：

- 左：完整或近完整标题（`.h-xl`，1 行 ≤16 字）+ 副标题（`.h-sub`）。
- 右：一个强视觉关系——大图、产品 mockup、或珊瑚 callout 块。
- 底 issue row。

结构（1:1）：

- 从长标题派生短标题（4-10 中文字，见 `references/title-shortener.md`）。
- 大字居中（`.h-display`，2 行 ≤6 字/行）。
- 默认无图。
- 无拥挤副标题。
- 强对比 + 呼吸空间。

**配对预览**：两个封面建在同一 HTML，加 `.pair-preview` 段并排展示，方便检查视觉关系。

**Minimum density**：

- 21:9：标题 + 副标题 + 视觉关系 + issue row。四者缺一即空。
- 1:1：短标题（大字居中）+ 可选小 issue 标。标题 <4 字时加 issue 标，否则空。

---

## Recipe Selection Guide

按内容意图选 recipe，不按话题查表：

| 内容意图 | 推荐 recipe |
| --- | --- |
| 封面钩子 | C01 |
| 单张纪实照 + takeaway | C02 |
| 解释一个观点带细微差别 | C03 或 C11 |
| 核心句/宣言/引文 | C04 |
| 实用清单/购买指南 | C05 |
| 多截图/多图证据 | C06 |
| 末页/下一步/CTA | C07 |
| 代码/产品 mockup/技术 | C08 |
| 强观点/CTA/电压瞬间 | C09 |
| 对比/选型 | C10 |
| 随笔 + 边注 | C11 |
| 步骤/教程/流程 | C12 |
| 大数字/震撼统计 | C13 |
| 微信封面配对 | C14 |

## Recipe Mixing Rules

- **一个图组用 3-7 个不同 recipe。** 全用同一 recipe 读成模板。
- **封面永远 C01 或 C14**（微信）。不要用 C04/C09 做封面——它们是内页节奏。
- **C09 Coral Callout 每组最多 2 张。** 珊瑚是稀缺电压。
- **C08 Product Mockup 在技术内容里可重复**——代码/产品页可连续用，因为深海军是产品 chrome 的家。
- **C04 Pull Quote 在密页间造节奏**——不要连续两张 C04。
- **C13 Stat Hero 每组最多 1 张。** 大数字是震撼瞬间，不是常态。

## Anti-Pattern: Recipe Misuse

- **C01 封面无图**——C01 需要大图占 35-55%。无图时切 C04 或 C13。
- **C09 用珊瑚做整组主题**——C09 是单页强调，不是整组。整组珊瑚过载。
- **C08 把代码放奶油卡**——代码属于深海军卡。奶油卡放代码读成 SaaS 营销页。
- **C14 1:1 是 21:9 的拥挤裁切**——每个比例分别构图。1:1 用派生的短标题，不是长标题缩字号。
- **C12 少于 5 步**——pipeline 需要递进感。4 步以下切 C05 清单。
