# QA Checklist

Run this before final delivery.

## Dimensions

- Rednote images are `1080 x 1440`.
- WeChat 21:9 cover is `2100 x 900`.
- WeChat 1:1 cover is `1080 x 1080`.
- WeChat pair preview exists in the same HTML when WeChat covers are requested.
- File names are stable and ordered.

## Text

- No text overflows or touches the edge.
- No content collides with `.issue-strip`. If body copy or a closing line is overlapping the page footer, the footer is probably `position: absolute` while content above grew. Switch to flex `margin-top: auto` (see Anti-pattern C in `style-system.md`).
- Cover title is readable on a phone at thumbnail size.
- Body text is not smaller than it needs to be.
- Chinese line breaks are intentional.
- Important product names, English terms, and code snippets are spelled correctly.
- Captions match the source text; no invented facts.

## Layout

- Each page has one clear focal point.
- Pages do not all use the same structure.
- Lower empty space is either intentional or fixed.
- Images and screenshots align to the grid.
- Screenshot pages give the screenshot enough area.
- 1:1 WeChat cover uses a simplified short title and is composed separately, not blindly cropped.

### 4-band Density Check (3:4 only — run after render)

Open the rendered PNG. Mentally divide it into 4 horizontal bands of 360px each (0-25%, 25-50%, 50-75%, 75-100%). For each band, classify as:

- **Filled** — contains text, image, data, or rule.
- **Justified empty** — empty for a stated reason: hero-image breathing (C01), single-sentence statement (C04/C13), leading/trailing margin (top 8% / bottom 8%).
- **Under-filled** — empty with no reason. **Any single under-filled band > 15% canvas height (>216px) is a fail.**

A poster passes when:

1. Total Filled + Justified empty ≥ 100%.
2. Filled bands cover ≥ 75% canvas height (≥ 1080px of 1440px).
3. No two adjacent bands are both "justified empty" — that creates a >25% void mid-poster.

If failing: don't shrink the canvas, don't add decorative blobs. Either expand copy (more ledger items, longer paragraphs, supporting evidence row, marginalia column) or switch recipe (C07 → C04 for genuinely-short content; C03 → C11 to add a marginal column; thin ledger → C08 Tall Ledger with bigger rows).

## Style

- Package uses one visual system: Claude Warm Editorial.
- Identity test passes for every poster (see `style-system.md` "Identity Test"). In particular: every display title ≥72px uses a typed class with weight ≤500; serif display family on the title; at least one atmosphere layer beyond a flat fill (paper grain / coral glow / paper-wash); at least one of large photo well, serif pull quote, marginalia column, or true ledger structure.
- Coral scarcity: each poster has at most one `.card-coral` and one `.btn-coral`. Coral is the accent, not the primary surface.
- Surface rhythm: no two adjacent bands use the same surface mode (cream → navy → cream, not cream → cream → cream).
- No random SVG blobs, ovals, drops, circles, stickers, or decorative gradients.
- No nested cards.
- No excessive rounded corners (max `--r-xl` = 16px on cards; `--r-pill` only on badges/buttons).

## Images

- Supplied images are not distorted.
- Faces, hardware, product UI, and key text are not accidentally cropped.
- Screenshots remain readable.
- Generated images do not contain unwanted text, logos, page numbers, or poster borders.
- Photo crops feel intentional.
- For any image fetched from the web (Pexels / Unsplash / Flickr CC / Wallhaven / direct search): the source URL is recorded in `assets/SOURCES.md`, and the user has been asked whether to add an in-image attribution caption. The user's answer is honored. Flickr CC attribution preserves the author name when the user opts in.

## Text-On-Image (when applicable)

Run these only for posters where text touches a photo (full-bleed cover, large image well, generated overlay). See `references/image-overlay.md`.

- Image area ≥60% of canvas → the photo passes the quiet-zone + light tests; no-mask composition was tried first; any tint is localized, image-toned, and only used if the thumbnail check fails.
- Subject map is documented as an HTML comment next to the hero block (face / focal feature location + safe zones).
- No display title (≥72 px) overlaps a face, hand, or key product feature.
- `object-position` was chosen from the subject-location table, not left at default for face shots.
- Thumbnail test: downscale the rendered PNG to 360 px wide and confirm the title is still legible without strain.

## Final Response

- Include the output path.
- Show the main cover inline when useful.
- Mention verification performed.
- Mention any limitations, such as low-resolution source assets.
