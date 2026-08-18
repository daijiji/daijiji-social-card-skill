# Screenshot Treatment

When the user supplies an app / web / code / dashboard screenshot, **do not** drop it raw into a `.frame-img`. The aspect ratio mismatch crops UI and the harsh edge looks SaaS-y. Use `.frame-shot` instead — a sibling class in the seed template.

## When to reach for it

- App / Web UI capture — anything with a status bar, tab bar, toolbar, or window chrome.
- Code / terminal screenshots.
- Dashboard / chart screenshots that need to keep every label readable.
- IDE captures where text density matters more than composition.

For **photographic** content (people, scenery, products) — keep using `.frame-img`. The treatments below assume a pixel-perfect UI source where contain-fit is non-negotiable.

## Subject Prep

Before framing, decide what the actual screenshot subject is. If the source capture contains a floating modal/card over an unrelated page, desktop chrome, cropped side text, cursor trails, notification fragments, or leftover background UI, crop to the foreground window/card first and then place that cleaned subject into the stage. Do not beautify the entire raw capture when it carries accidental text or partial UI behind the subject.

Screenshot beautification must not introduce perspective, skew, rotation, or 3D tilt unless the user explicitly asks for a mockup scene. A CleanShot-style treatment is orthographic: straight subject, equal scaling, quiet background, and clear safe padding.

## Anatomy

```
.frame-shot.r-{ratio}
  └─ <img src="…">         (object-fit: contain by default)
```

Optional wrapping:

```
.device-browser
  └─ .frame-shot.r-16x10
       └─ <img>

.device-phone
  └─ .frame-shot.r-3x4
       └─ <img>
```

## Parameters

Pick values before writing the markup. Treat this like the C01 cover decision tree — pick once, don't fiddle mid-build.

### 1. `r-*` ratio (required, matches slot)

| Class      | Use                                                       |
| ---------- | --------------------------------------------------------- |
| `r-16x10`  | Default for app / web shots, looks like a real window     |
| `r-16x9`   | Landscape video / dashboard / wide chart                  |
| `r-4x3`    | Classic desktop window, legacy app                        |
| `r-3x2`    | DSLR-style — only if the source is photographic UI mockup |
| `r-1x1`    | App icon / square widget                                  |
| `r-3x4`    | Mobile portrait shot (pair with `.device-phone`)          |
| `r-21x9`   | Multi-monitor / ultra-wide / WeChat hero                  |

### 2. Corners (style-locked default)

Claude Warm Editorial default: `--r-lg` (12px) on `.frame-shot`. The seed template already applies this. Bump to `--r-xl` (16px) for "cutout" feel on cream paper.

Never go above 16px — anything bigger reads as iOS marketing.

### 3. Shadow

- Default: no shadow. The cream paper stage already provides separation.
- Add `box-shadow: 0 8px 32px rgba(20,20,19,.08)` only on hero shots that float on a cream stage.
- For Dark Product theme screenshots, skip shadow — the navy stage already has depth.

### 4. Background stage

| Token           | Role                                          |
| --------------- | --------------------------------------------- |
| `--surface-card` (default) | Default warm cream stage             |
| `--surface-cream`          | Stronger cream for hero shots         |
| `--navy-soft`              | Dark-mode UI shot on Dark Product     |
| `--canvas`                 | Pure canvas stage (matches page bg)   |

Backgrounds are **never** accent-coloured. If the screenshot needs an accent emphasis, add a `.badge-coral` chip or `.kicker` next to it — don't tint the stage.

### 5. Padding (between shot and stage)

The seed template's `.frame-shot` has `padding: 24px` by default. Override:

- `padding: 0` — image fills the frame. Use when the screenshot itself already has window chrome.
- `padding: 24px` (default) — lets the stage breathe.
- `padding: 56px` — when the shot is busy and needs to feel calm.

### 6. `fit-cover` (override)

Default is `object-fit: contain` — this is the whole point of `.frame-shot`. **Only** add `.fit-contain` (already in seed) when:

- The slot is a hero where exact pixels of the source don't matter (e.g. a code shot used as a background pattern).
- The user explicitly says they want the shot cropped.

## Device chrome

Two wrapper classes ship with the seed. They wrap a `.frame-shot`:

### `.device-browser`

Adds a 36px chrome bar with traffic-light dots. Use for web / desktop app captures.

```html
<div class="device-browser">
  <div class="frame-shot r-16x10">
    <img src="assets/website.png" alt="Linear inbox">
  </div>
</div>
```

### `.device-phone`

Wraps a 3:4 or 16:10 shot in a navy bezel with 28px rounded inner corners. Use for mobile captures.

```html
<div class="device-phone">
  <div class="frame-shot r-3x4">
    <img src="assets/app.png" alt="WeChat detail">
  </div>
</div>
```

Don't stack extra corner rounding on top of `.device-phone` — the bezel already rounds the inner shot.

## Safe-area cropping

When the user delivers a full-screen capture (1290×2796 iOS / 1080×2400 Android / 1920×1080 desktop), do **not** show the status bar, dock, or browser tab strip unless that chrome is the subject. Crop before importing:

1. Trim the iOS / Android status bar (top 47-59 px on retina).
2. Trim the home indicator / nav gesture bar (bottom 34 px on iOS).
3. For desktop: trim everything above the page content unless wrapped in `.device-browser`.

If you can't crop ahead of time, use `object-position: center 6%` to bias the visible region downward — but this is a workaround, not the preferred path.

## Style cheat-sheet

Two recipes that cover 80% of cases.

**Claude Warm Editorial default** — desk-photo warmth:
```
.frame-shot.r-16x10
```
(stage is `--surface-card` cream, padding 24px, no shadow)

**Dark Product hero** — code/IDE on navy:
```
.frame-shot.r-16x10  (inside [data-theme="dark-product"] poster)
```
(stage is `--navy-soft`, padding 24px, no shadow)

**Editorial hero with extra breathing room**:
```
.frame-shot.r-16x10  style="padding:56px"
```

For comparison shots (before / after), use **the same parameters** on both — different treatment between cells reads as inconsistency, not contrast.

## Validator

`validate-social-deck.mjs` doesn't have a screenshot-specific rule (yet). The existing R1 / R2 / R5 still apply: a `.frame-shot` that overflows or pushes the footer is still a fail.

If a deck mixes `.frame-img` and `.frame-shot` on the same poster, that's usually a smell — pick one approach per poster.
