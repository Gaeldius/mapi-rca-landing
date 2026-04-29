# DESIGN.md — MAPi × RCA Landing Page

Strict design system. The palette is intentionally narrow. Discipline > variety.

## Color tokens — the only colors allowed

```
--c-blue:    #006DEC   /* MAPi primary blue. THE ONLY blue allowed on the site. */
--c-orange:  #FF934F   /* MAPi secondary, accent, CTA highlight, "OFFERT", italic poetry */
--c-ink:     #28323E   /* All body text. Also used as the dark neutral for banner / KPI / footer / dark cards. */
--c-bg:      #EBF6FF   /* MAPi pale blue. Soft sections (positioning, audit secondary, FAQ, offer). */
--c-paper:   #FFFFFF   /* White, for clean sections. */
--c-line:    #DCE8F3   /* Subtle borders. */
--c-rca:     #4F42ED   /* RCA's purple. ONLY allowed in the RCA logo and the RCA-specific accent in the partnership card. Do not bleed elsewhere. */
```

Variants are obtained **by opacity only**, never by introducing a new hue:

```
--c-blue-15:  rgba(0,109,236,.15)
--c-blue-10:  rgba(0,109,236,.10)
--c-blue-06:  rgba(0,109,236,.06)
--c-orange-20: rgba(255,147,79,.20)
--c-orange-12: rgba(255,147,79,.12)
--c-ink-80:   rgba(40,50,62,.80)
--c-ink-65:   rgba(40,50,62,.65)
--c-ink-45:   rgba(40,50,62,.45)
--c-ink-15:   rgba(40,50,62,.15)
```

### Forbidden — do not introduce

- ❌ Any other shade of blue (no Infogreffe navy #053368, no Tailwind blue-600, etc.)
- ❌ Purples, magentas, teals, greens (except success state #17B67A in form submit confirmation)
- ❌ Solid black (#000) or pure white-on-pure-white panels — always `--c-paper` and `--c-ink`
- ❌ Gradients between non-palette colors

### Allowed gradient compositions

- `linear-gradient(135deg, var(--c-blue), var(--c-orange))` — used as glow under cards (filter: blur, opacity 0.18-0.20)
- Dark hero overlay: `linear-gradient(92deg, rgba(10,20,35,.92) → rgba(10,20,35,.35))`
- Radial ambient glows in dark sections: `rgba(0,109,236,.42)`, `rgba(255,147,79,.22)`

## Typography — Museo Sans, locally hosted

The official MAPi typeface is **Museo Sans**. The font files live in `assets/fonts/`. Loaded weights:

```
Museo Sans 300  (Light)
Museo Sans 500  (Medium) — body default
Museo Sans 500 Italic
Museo Sans 700  (Bold) — emphasis, h3-h4, button text
Museo Sans 700 Italic
Museo Sans 900  (Black) — h1, h2, big numbers
```

### Hierarchy

| Element | Weight | Size | Treatment |
|---|---|---|---|
| H1 hero | 900 | clamp(2.5rem, 5.8vw, 4.7rem) | letter-spacing -0.035em, line-height 1.06 |
| H2 section | 900 | clamp(2rem, 4vw, 3.3rem) | letter-spacing -0.025em |
| H3 card title | 700 | 1.4-1.6rem | letter-spacing -0.02em |
| Body | 500 | 17px / 1.05rem | line-height 1.65 |
| Eyebrow | 700 | 0.78rem | uppercase, letter-spacing 0.16em |
| Trust value | 900 | 1.7rem | letter-spacing -0.02em |
| Italic accent | 500 italic | inherit | colored `--c-blue` (light bg) or `--c-orange` (dark bg) |

### Special italic treatment

A short italic phrase (`<em>` or `.italic`) is sprinkled in headlines to soften the typographic block and add poetry. Example:
- "deux équipes humaines, *une plateforme qui veille*"
- "qui *navigue* vers"
- "qu'on entend"

Rule: **one italic per headline maximum**. Color: `--c-blue` on light bg, `--c-orange` on dark bg.

## Logos — non-negotiable

**Always use the official SVGs.**

```
assets/logos/logo-mapi.svg   — MAPi logotype with orange dot above the i
assets/logos/logo-rca.svg    — RCA wordmark with purple X mark
```

- Never recreate the logos in HTML/CSS text. Always `<img src="assets/logos/...">`.
- Standard heights: nav 36px (MAPi), 32px (RCA). In partnership pods: 44px / 36px. Footer: 40px / 30px.
- In footer (dark), apply `filter: brightness(0) invert(1);` to the RCA logo only — RCA's wordmark is dark-text-on-white. MAPi keeps its colors.
- The MAPi orange dot (the pastille above the `i`) is the brand's signature element. Echo it visually if needed (eyebrow `::before`, step icon `::after`), but never replace the logo.

## Spacing scale

```
--r-sm: 10px
--r-md: 18px
--r-lg: 28px
--r-xl: 44px
```

Section vertical padding: 100-140px desktop, 60-80px mobile. Containers max-width 1240px, side padding 28px → 20px on mobile.

## Elevation / shadows

```
--shadow-xs:    0 1px 2px rgba(40,50,62,.05)
--shadow-sm:    0 4px 16px rgba(40,50,62,.07)
--shadow-md:    0 16px 44px rgba(40,50,62,.10)
--shadow-lg:    0 32px 80px rgba(40,50,62,.14)
--shadow-blue:  0 20px 50px rgba(0,109,236,.28)
--shadow-warm:  0 20px 50px rgba(255,147,79,.35)
```

Cards: `--shadow-md` at rest, `--shadow-lg` on hover (with translateY(-4px)).
Hero card audit: tilted -1.2deg, straightens to 0 on hover.

## Components

### Buttons

**Primary CTA — single, canonical version, used everywhere identically.**

```html
<a class="btn btn-blue">
  Obtenir mon audit offert <span class="ct-strike">(1 299 €)</span>
  <svg>...arrow...</svg>
</a>
```

- Background: `--c-blue` solid
- Text: white
- Strikethrough on price using `.ct-strike` (line-through, 2px, opacity 0.7, weight 500)
- Border-radius: 999px (pill)
- Padding: 16px 26px
- Hover: translateY(-2px), opacity 0.9 (no color change)
- Shadow: `--shadow-blue`

**Secondary / outline (light bg)**

```html
<a class="btn btn-outline">La solution hybride</a>
```

- Transparent background, blue border + blue text
- Hover: blue fill, white text

**Outline on dark/image bg (hero only)** — see `.hero .btn-outline` override:
- White border (55% opacity), white text, glassmorphism `rgba(255,255,255,.06)` + blur(6px)
- Hover: white fill, blue text

### Cards

**Stat card (light bg sections)**

- Background: `--c-bg` on `--c-paper` section, `--c-paper` on `--c-bg` section
- Padding: 38-40px
- Border: 1.5px solid `--c-line`
- Radius: `--r-lg`
- Hover: translateY(-4-6px), `--shadow-md`, sometimes border-color → `--c-blue` for tech accent

**Pos-card (positioning trio)**

- 3 cards in `repeat(3, 1fr)`, gap 22px, stack to 1fr below 1060px
- Variants: `.human` (orange role), `.commercial` (orange role), `.tech` (blue role)
- Each starts with a tiny role label: 2px bar + uppercase text in role color
- H3 1.42rem, 12px below
- Body paragraph 0.98rem
- Bullet list with check-svg in role color

### Form

- Labels above inputs (always)
- Inputs: 14px padding, 1.5px border `--c-line`, radius 12px, background `--c-bg`
- Focus: border `--c-blue`, background white, ring `0 0 0 4px var(--c-blue-10)`
- Submit button = canonical CTA (full width)

### Banner (top marquee)

Currently removed from page. If re-added: `--c-ink` background, white text, marquee animation 38-42s linear infinite, `<b>` for orange highlights, `◆` separators in orange.

## Motion — strict, purposeful

### Entrance reveals

```
[data-animate]
  opacity: 0 → 1
  translateY(28px → 0)
  duration: 0.8s cubic-bezier(.2,.7,.2,1)
  triggered via IntersectionObserver, threshold 0.12, rootMargin -40px bottom
```

Stagger via `data-delay="1|2|3|4"` → 100/200/300/400ms.

### Hover

- Buttons: translateY(-2px), 200ms cubic-bezier(.2,.7,.2,1)
- Cards: translateY(-4 to -6px), 300ms ease, shadow upgrade

### Rotating hero amount

```js
amounts = ['40 000 €', '77 000 €', '142 000 €', '250 000 €', '418 000 €', '680 000 €', '1 247 000 €']
TICK = 2600ms, FADE = 450ms
```

Out: opacity 0, translateY(-0.28em), blur(6px).
In: same but reversed.
Respects `prefers-reduced-motion`.

### Forbidden motion

- ❌ Bouncing, spinning permanently, parallax scroll, scroll-jacking, Lottie, complex 3D
- ❌ Hover effects > 300ms
- ❌ Auto-playing video/audio

## Hero specifics — image background

The hero uses `assets/images/hero-bottle.jpg` (a message-in-a-bottle photo through a porthole). This is a brand-strategic visual — do not replace without explicit instruction.

### Overlay rules

```
desktop: linear-gradient(92deg, 
  rgba(10,20,35,.92) 0%, 
  rgba(10,20,35,.78) 32%, 
  rgba(10,20,35,.48) 58%, 
  rgba(10,20,35,.35) 78%, 
  rgba(10,20,35,.55) 100%)

mobile: linear-gradient(180deg, 
  rgba(10,20,35,.85) 0%, 
  rgba(10,20,35,.72) 50%, 
  rgba(10,20,35,.78) 100%)
```

Plus an orange ambient glow at top-right (mix-blend-mode: screen) to echo the bottle's amber label.

### Hero typography in dark context

- H1: white + `text-shadow: 0 2px 24px rgba(0,0,0,.35)`
- `.italic`: `--c-orange` (NOT blue, because blue is hard to read on this image)
- `.hl` highlight underline: orange at opacity 0.65
- `.rotating-amount`: `color: inherit` (white)
- `.hero-lede`: `rgba(255,255,255,.92)` + subtle text-shadow
- Trust values: white + text-shadow

## Section transitions

A white SVG wave (`<svg class="hero-wave">`) sits at the bottom of the hero, transitioning into the white positioning section below. Don't remove unless replacing with another organic transition.

## Accessibility

- All decorative SVGs marked `aria-hidden="true"`.
- All form inputs have explicit `<label>` elements.
- Color contrast: ink #28323E on white = 12.6:1 ✓ AAA. White on `--c-blue` = 4.7:1 ✓ AA.
- `prefers-reduced-motion` disables the rotating hero amount.
- `aria-live="polite"` on the rotating amount span.

## File structure

```
/
├── index.html              # single-page landing
├── PRODUCT.md              # this skill's product context
├── DESIGN.md               # this file
├── README.md
├── assets/
│   ├── fonts/              # Museo Sans woff/woff2 (300, 500, 500i, 700, 700i, 900)
│   ├── logos/
│   │   ├── logo-mapi.svg   # official MAPi
│   │   └── logo-rca.svg    # official RCA
│   └── images/
│       └── hero-bottle.jpg # hero background
└── .claude/skills/
    └── impeccable/         # design skill (gitignored or not, depending on team policy)
```

Single-file HTML deliberately — no framework, no build step. Edits are direct, deploys are instant via GitHub Pages.
