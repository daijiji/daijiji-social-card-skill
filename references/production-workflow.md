# Production Workflow

## Recommended Folder Shape

Create a task folder under the current workspace:

```text
social-card-<slug>/
  index.html
  render.cjs
  assets/
  output/
```

Use descriptive slugs:

- `social-card-claude-update`
- `social-card-hiking-outfit`
- `wechat-ai-card-skill-cover`

## HTML/CSS Rendering Pattern

Build one HTML file containing all frames:

```html
<main class="sheet">
  <section class="poster xhs" id="xhs-01">...</section>
  <section class="poster xhs" id="xhs-02">...</section>
  <section class="poster wide" id="wechat-21x9">...</section>
  <section class="poster square" id="wechat-1x1">...</section>
  <div class="pair-preview" id="wechat-pair-preview">
    <div class="pair-row">
      <div class="preview-wide">...</div>
      <div class="preview-square">...</div>
    </div>
  </div>
</main>
```

Each frame must have stable dimensions:

```css
.poster.xhs    { width: 1080px; height: 1440px; }
.poster.square { width: 1080px; height: 1080px; }
.poster.wide   { width: 2100px; height:  900px; }
```

Use `box-sizing:border-box`, fixed safe margins, and `overflow:hidden`.

For WeChat covers:

- Compose `wechat-21x9` and `wechat-1x1` as separate source frames.
- Also include `pair-preview` in the same HTML so the pair can be inspected together.
- Export the two real deliverables separately.
- Export the pair preview only when helpful for review.
- The `1:1` cover should use a simplified title derived from the long title, not a crop or text squeeze from the `21:9` frame.

## Theme Switching

Set the theme on `<html data-theme="…">`:

```html
<html lang="zh-CN" data-theme="claude-canvas">
```

Available themes:

- `claude-canvas` (default) — warm cream + coral + navy
- `coral-callout` — coral full-bleed for single-page emphasis
- `dark-product` — deep navy for code/product mockup pages
- `forest-warm` — cream + forest green for outdoor/nature
- `midnight-claude` — deepest dark for cinematic/game key art

Switch theme per poster by setting `data-theme` on the `<section class="poster">` itself — the CSS variables cascade.

## Rendering

Use Playwright or an equivalent browser screenshot workflow:

1. Open `index.html`.
2. Wait for fonts and images (Google Fonts + Lucide).
3. Screenshot each frame node by id.
4. Save to `output/`.
5. Verify dimensions.

Example render logic:

```js
const targets = [
  ["#xhs-01", "xhs-01-cover.png"],
  ["#xhs-02", "xhs-02-point.png"],
  ["#wechat-21x9", "wechat-21x9-cover.png"],
  ["#wechat-1x1", "wechat-1x1-cover.png"],
  ["#wechat-pair-preview", "wechat-cover-pair-preview.png"],
];
```

If a local dev server is needed for assets or font loading, start it and tell the user the URL. If `file://` rendering works, no server is required.

## Verification Commands

Useful checks on Windows (PowerShell):

```powershell
Get-ChildItem output\*.png | ForEach-Object {
  $img = [System.Drawing.Image]::FromFile($_.FullName)
  "$($_.Name)  $($img.Width)x$($img.Height)"
  $img.Dispose()
}
```

Or with ImageMagick:

```bash
magick identify output/*.png
```

For visual inspection in Codex:

- Use image viewing tools for local PNGs.
- Show final PNGs inline with absolute paths.

## Screenshot Treatment

Programmatic framing is preferred for user-provided screenshots:

- Create a clean target-ratio frame using `.frame-shot`.
- Add plain cream, soft grey, or navy background. Do not add page-wide grid/dot backgrounds unless the user explicitly asks for a technical blueprint look.
- If the capture contains a floating window/card over unrelated UI, crop to the foreground subject before placing it.
- Place screenshot inside with safe padding (`.inset-sub` or `.inset-bal`).
- Preserve readable text.
- Do not redraw the screenshot unless the user asked for redesign.
- Do not add perspective, skew, rotation, or mockup tilt unless the user explicitly asks for a scene mockup.

For Claude Warm Editorial:

- Small radius (`--r-lg` = 12px) or subtle shadow is allowed, but avoid SaaS marketing-card styling.
- Use `.device-browser` for web/desktop captures, `.device-phone` for mobile captures.

## Generated Images

When generating missing visuals:

- Generate only the raw visual asset.
- Keep text out of generated images.
- Match the page role and theme.
- Save generated assets into `assets/` and place them in the HTML.
- Generate only the pages that need it, usually 1-2 images for a set.

## Accessibility And Readability

- Use strong contrast for all text.
- Do not place long text over busy photos.
- If text must sit over photo, use a solid cream/navy block or a high-contrast strip, not a blur blob.
- Avoid negative letter spacing in Chinese body text.
- Use line-height that lets Chinese breathe: roughly 1.06-1.18 for big titles, 1.55-1.65 for body.

## Common Fixes

- Cover feels empty: enlarge title, enlarge image, or add a functional bottom strip.
- Screenshot too small: reduce side text, give screenshot 55%-70% of the canvas.
- Lower area empty: read `portrait-fill.md`; merge pages, add a full-height ledger, use a larger evidence image, add a marginal quote column, or switch to an atmospheric thesis page.
- Style feels generic: add issue metadata, better type hierarchy, a stronger evidence image, richer paper-wash/grain atmosphere, or more intentional content dividers.
- Text overflows: shorten copy before shrinking type.
