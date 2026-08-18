# Background Systems

Claude Warm Editorial preserves the Anthropic product surface feeling: warm cream paper, soft coral glow, deep navy product moments. Static Rednote/WeChat images should feel like a page from a Claude release note or the Anthropic blog — not a flat beige card with a faint grid.

Do not reduce this mode to a flat `#faf9f5` page with a faint grid. Do not add visible grid, dot-matrix, or drafting-paper patterns to the background.

## Layer Model

Use 3-4 layers:

1. Paper base from `theme-presets.md` (the `--canvas` color).
2. Procedural paper grain (`.grain`).
3. Coral glow + ink wash (`.paper-wash`).
4. Content layer.

Recommended CSS order:

```html
<section class="poster xhs">
  <div class="grain"></div>
  <div class="paper-wash"></div>
  <div class="content">...</div>
</section>
```

The seed template `assets/template-claude-card.html` already ships these two layers with theme-aware variants for each of the 5 themes (Claude Canvas / Coral Callout / Dark Product / Forest Warm / Midnight Claude). You do not need to write the CSS yourself — just include both `<div>` layers inside every `.poster`.

## When To Show Stronger Atmosphere

Use stronger visible atmosphere (let `.paper-wash` opacity read more) for:

- Cover.
- Chapter/divider.
- Pull quote.
- Sparse thesis page.
- Closing page.

Use subtle atmosphere for:

- Screenshot pages.
- Dense ledgers.
- Checklists.
- Product evidence pages.

If screenshots are the main evidence, the background should support them rather than compete with them.

## Theme-Specific Atmosphere

### Claude Canvas (default)

- `.grain`: multiply-blend, 3px dot pattern at 0.32 opacity.
- `.paper-wash`: coral radial top-left + ink radial bottom-right + vertical ink gradient.

### Coral Callout

- `.grain`: overlay-blend, white specks at 0.18 opacity.
- `.paper-wash`: white radial top-left + navy radial bottom-right.
- The whole canvas is coral — atmosphere is about adding depth, not adding more coral.

### Dark Product

- `.grain`: screen-blend, warm specks at 0.22 opacity.
- `.paper-wash`: coral glow top-left + shadow bottom-right + vertical shadow gradient.
- Use this for code-heavy or product-mockup pages where the navy surface is the "product."

### Forest Warm

- `.grain`: same as Claude Canvas.
- `.paper-wash`: moss radial top-left + ink radial bottom-right.
- The only theme where the accent shifts from coral to forest green.

### Midnight Claude

- `.grain`: screen-blend, warm specks at 0.26 opacity.
- `.paper-wash`: coral glow top-left + heavy shadow bottom-right + vertical shadow gradient.
- The deepest dark theme — use for game key art, night scenes, cinematic moments.

## 2D Fallback

If the CSS layers fail to render (rare), create a 2D canvas with:

- Large radial coral wash at low opacity.
- Soft paper noise.
- Theme color at very low opacity.

The output should still feel like a Claude surface, not a plain fill.

## Do Not

- Do not use bright gradients.
- Do not use page-wide grid, dot-matrix, graph-paper, or drafting-paper backgrounds.
- Do not use decorative blobs or circles with no relationship to the layout.
- Do not place strong background marks behind body text.
- Do not let the atmosphere obscure screenshots or small captions.
- Do not animate the final image sequence unless the task is video.
- Do not stack a second WebGL/canvas layer — the Claude Warm Editorial system is intentionally CSS-only for static export stability.
