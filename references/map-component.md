# Map Component (.map-block)

Use when the content has **spatial relationships**: a travel route, store locations, walking tour, neighborhood overview, store-vs-store comparison by district, before/after relocation. Single coordinates ("我在北京") don't need a map.

The seed template does not ship `.map-block` classes by default — add the CSS below to your task's `index.html` when you need a map. The same markup works in any of the 5 themes; only the tone tokens differ.

## Three rendering modes

Pick mode **before** writing the markup. They are not stackable.

Default selection:

- Travel guides, walking routes, hiking, store locations, or anything the reader may use for orientation → **Mode T** Mapbox Static.
- Same need, but no Mapbox token → **Mode O** OSM static tile composite.
- Conceptual relationship map, fictional route, or purely editorial "places are connected like this" graphic → **Mode S** schematic SVG.

### Mode T · Mapbox Static Image (default for real routes)

Real raster tiles, desaturated and tinted to match the deck. Use when the content is travel / hiking / neighborhood wayfinding and you need to show actual terrain or streets. Requires `MAPBOX_ACCESS_TOKEN` in the build env.

```
https://api.mapbox.com/styles/v1/mapbox/light-v11/static/
  pin-s-1+555555(116.404,39.915),
  pin-l-2+1936b3(116.421,39.918)
  /116.412,39.916,14,0/1200x675@2x
  ?access_token=YOUR_TOKEN
```

```html
<div class="map-block r-16x10 tone-paper">
  <img src="https://api.mapbox.com/styles/v1/mapbox/light-v11/static/.../1200x675@2x?access_token=..." alt="Lhasa walking route">

  <!-- pins overlay (calculate % from mapbox bounds) -->
  <div class="map-pin" style="left:34%; top:52%;">...</div>
  <div class="map-pin accent" style="left:64%; top:42%;">...</div>

  <div class="map-legend">LHASA · MAPBOX LIGHT</div>
</div>
```

Use `tone-paper` (multiply-blend, saturation 36%) on Claude Canvas / Forest Warm. Use `tone-ink` (brightness 62%, saturation 18%) on Dark Product / Midnight Claude. Don't show the raw Mapbox colour-saturated style — it fights the deck.

**Style allowlist** for the URL — keep it monochrome:
- `mapbox/light-v11`
- `mapbox/dark-v11`
- `mapbox/streets-v12` (only with `tone-paper`, never raw)

Never use `outdoors-v12` or `satellite-v9` — too colourful, will clash with all themes.

### Mode O · OpenStreetMap tile composite (fallback for real routes)

Free, no token. Use this when a real map is needed but Mapbox is unavailable. Render server-side via `staticmaps` npm package or similar, then place the raster as the `.map-block > img` child. Quality is lower than Mapbox, so keep labels sparse and let HTML pins carry the useful text.

### Mode S · Schematic SVG (conceptual / illustrative only)

Hand-drawn-style vector map drawn into a `viewBox="0 0 100 100"` SVG. Best for Claude Warm Editorial decks where the map should feel like an illustrated guide, not a satellite capture. No external service.

```html
<div class="map-block r-16x10">
  <svg viewBox="0 0 100 100" preserveAspectRatio="none">
    <!-- coastline / district boundary -->
    <path class="map-coast" d="M 6 18 Q 32 22 58 16 T 96 28"/>
    <!-- main road -->
    <path class="map-road" d="M 8 78 Q 42 62 78 70 T 96 56"/>
    <!-- secondary road -->
    <path class="map-road" d="M 24 14 L 38 62 L 52 86"/>
    <!-- water / park polygon -->
    <path class="map-water" d="M 62 8 L 92 12 L 96 36 L 70 30 Z"/>
  </svg>

  <!-- pins overlay: % coords relative to .map-block -->
  <div class="map-pin" style="left:32%; top:48%;">
    <div class="dot"></div><div class="line"></div>
    <div class="card"><div class="name">大昭寺</div><span class="meta">DAY 1</span></div>
  </div>
  <div class="map-pin accent" style="left:62%; top:38%;">
    <div class="dot"></div><div class="line"></div>
    <div class="card"><div class="name">布达拉宫</div><span class="meta">DAY 2 · 18:40 SUNSET</span></div>
  </div>

  <div class="map-legend">LHASA · 拉萨城关 · 1:8K</div>
</div>
```

The SVG paths are stylized, not GPS-accurate. **Don't** try to trace OpenStreetMap by hand — this map is a graphic device, not a wayfinding tool. Sketch the major axes that frame the pins.

Layer order in the SVG (back to front):
1. `.map-water` polygons (rivers, lakes, parks)
2. `.map-coast` (boundary)
3. `.map-grid` (optional reference grid lines)
4. `.map-road` (transport lines)

Pins sit *outside* the SVG, as HTML siblings — they get the `.frame-shot` treatment for free.

## CSS to add when using maps

```css
.map-block {
  position: relative;
  overflow: hidden;
  border-radius: var(--r-lg);
  background: var(--surface-card);
}
.map-block.tone-paper > img {
  mix-blend-mode: multiply;
  filter: saturate(36%);
}
.map-block.tone-ink > img {
  filter: brightness(62%) saturate(18%);
}
.map-block svg {
  position: absolute; inset: 0;
  width: 100%; height: 100%;
}
.map-coast { fill: none; stroke: var(--ink); stroke-width: 0.4; opacity: .5; }
.map-road   { fill: none; stroke: var(--ink); stroke-width: 0.6; opacity: .7; }
.map-water  { fill: var(--coral); opacity: .12; }
.map-grid   { fill: none; stroke: var(--hairline); stroke-width: 0.2; }

.map-pin {
  position: absolute;
  transform: translate(-50%, -100%);
  display: flex; flex-direction: column; align-items: center;
  z-index: 2;
}
.map-pin .dot {
  width: 12px; height: 12px; border-radius: 50%;
  background: var(--ink); border: 2px solid var(--canvas);
}
.map-pin.accent .dot { background: var(--coral); }
.map-pin .line { width: 1px; height: 28px; background: var(--ink); opacity: .4; }
.map-pin .card {
  background: var(--canvas);
  border: 1px solid var(--hairline);
  border-radius: var(--r-md);
  padding: 8px 12px;
  font-family: var(--sans-en), var(--sans-zh), sans-serif;
  white-space: nowrap;
  box-shadow: 0 4px 12px rgba(20,20,19,.08);
}
.map-pin .card .name { font-size: 16px; font-weight: 500; color: var(--ink); }
.map-pin .card .meta {
  font-family: var(--mono); font-size: 12px;
  color: var(--muted); letter-spacing: .04em; text-transform: uppercase;
}
.map-pin.left .card { transform: translateX(calc(-100% - 16px)); }
.map-pin.left { flex-direction: column; align-items: flex-end; }

.map-legend {
  position: absolute; bottom: 12px; left: 12px;
  font-family: var(--mono); font-size: 12px;
  color: var(--muted); letter-spacing: .04em; text-transform: uppercase;
  background: rgba(var(--canvas-rgb),.8);
  padding: 4px 8px; border-radius: var(--r-sm);
  z-index: 2;
}
```

## Hard rules

- **No pin labels on the SVG or raster tile.** Names live in `.map-pin .card`, never as SVG `<text>` or baked into the map image. Keeps font rendering consistent and avoids scaling issues.
- **Max 6 pins per board**. More than that, the cards collide. Use a relations / index card to list secondary points.
- **One accent pin maximum**. Multiple accents kill the visual hierarchy. The accent pin uses `--coral` (or `--coral` which is forest green in Forest Warm theme).
- **Pins must not overlap cards**. Test in browser first — if two cards are within 80 px on the long axis, alternate `.left` placement.
- **No live JavaScript map**. Social cards are static PNGs, MapLibre is wasted weight.

## Pin placement playbook

The `.map-pin` element uses `left:X%; top:Y%;` to anchor on its centre. The card grows to the right by default; add `.left` to flip:

```html
<div class="map-pin left" style="left:78%; top:30%;">
  <div class="dot"></div><div class="line"></div>
  <div class="card"><div class="name">车公庄</div><span class="meta">START</span></div>
</div>
```

Rule of thumb:
- Pin sits in left 60% of canvas → card opens right (default).
- Pin sits in right 60% of canvas → card opens left (`.left`).
- Two pins close to each other → alternate sides so cards don't stack.

## Recipe routing

Map block fits into existing recipes; it doesn't get its own.

| Recipe          | How the map fits                                                |
| --------------- | --------------------------------------------------------------- |
| C03 split       | Top half = `.map-block r-16x9`; bottom half = explanatory text. |
| C11 marginalia  | Main column = `.map-block r-3x4`; aside lists relation cards.   |
| C14 pipeline    | One step contains a small map (r-16x10) showing that step's location. |
| C10 two signals | One signal cell holds a map; the other holds the data table.    |
| C01 image hero  | Replace the hero photo with `.map-block r-16x10`.                |

Don't invent a "map-only" page — the map always pairs with text that says **why** the spatial relationship matters.

## Common mistakes

- **Pasting Google Maps screenshots** — wrong tone, has UI chrome, will look out of place. Use Mapbox Static, OSM static tiles, or a schematic when the map is conceptual.
- **Tracing a real map into SVG** — schematic should *abstract*, not replicate. If a viewer can guess the city from your SVG alone, you over-traced.
- **Tiny pin cards** — `.card .name` is 16 px minimum. Cut the name if it overflows; never shrink.
- **Accent on every pin** — defeats the highlight system. Pick one most-important point.
