# Digital Smiles — Design System Rules

## Overview
Single-page static website for **Digital Smiles**, a Marketing / Revenue Systems Engineering consultancy. No build system, no frameworks — a single `index.html` file with an inline `<style>` block and one small inline vanilla `<script>` (a scroll-spy that drives the Process timeline).

## Project Structure
```
/
├── index.html          # Entire site: HTML + inline <style> + one small inline <script>
├── assets/
│   ├── fonts/          # Recoleta Alt (heading font), self-hosted .woff2/.otf
│   └── img/            # Logo + profile photo
├── figma/              # Figma capture export (not part of the live site)
└── process-variants/   # Standalone design prototypes for the Process section (not shipped)
```

## 1. Token Definitions (CSS Custom Properties)

All design tokens are defined in `:root` within the `<style>` block of `index.html`.

### Colors

| Token | Value | Usage |
|---|---|---|
| `--bg` | `#FDF6EE` | Page background (warm cream) |
| `--bg-white` | `#FFFFFF` | Card/component backgrounds |
| `--bg-dark` | `#1A1A2E` | Dark sections (CTA) |
| `--bg-dark-card` | `#242440` | Cards on dark backgrounds *(defined, currently unused)* |
| `--text` | `#1A1A2E` | Primary text |
| `--text-muted` | `#6B6B80` | Secondary/body text |
| `--text-light` | `#F0EDE8` | Text on dark backgrounds |
| `--accent-yellow` | `#FFD23F` | Primary accent, CTAs, buttons |
| `--accent-yellow-light` | `#FFF3CD` | Yellow tinted backgrounds |
| `--accent-green` | `#6BCB77` | Success, hero dot, phase 2 |
| `--accent-green-light` | `#E8F8EA` | Green tinted backgrounds, support note |
| `--accent-purple` | `#9B8FE8` | Section labels, annotations, phase 3 |
| `--accent-purple-light` | `#EEEAFF` | Purple tinted backgrounds |
| `--accent-orange` | `#FF8C42` | Accents, phase 1 eyebrow/labels |
| `--accent-orange-light` | `#FFF0E5` | Orange tinted backgrounds |
| `--accent-pink` | `#FF6B8A` | Spare accent — base token *(currently unused)* |
| `--accent-pink-light` | `#FFE3EA` | Pink tint — 5th About tag |

### Borders & Shadows

| Token | Value |
|---|---|
| `--border` | `rgba(0,0,0,0.06)` |
| `--border-light` | `rgba(255,255,255,0.1)` *(defined, currently unused)* |
| `--shadow-sm` | `0 2px 8px rgba(0,0,0,0.04)` |
| `--shadow-md` | `0 8px 30px rgba(0,0,0,0.06)` |
| `--shadow-lg` | `0 16px 50px rgba(0,0,0,0.08)` |

### Radii

| Token | Value |
|---|---|
| `--radius` | `16px` |
| `--radius-lg` | `24px` |

Pill-shaped elements (buttons, badges, tool pills) use `border-radius: 100px`.

> **Unused tokens:** `--accent-pink`, `--bg-dark-card`, and `--border-light` are defined in `:root` but have no `var()` references — kept as palette/utility spares. Remove them if you want a strictly-used token set.

### Typography

| Token | Value | Usage |
|---|---|---|
| `--font-main` | `'Plus Jakarta Sans', sans-serif` | Body text, nav, buttons, labels — everything that isn't a heading or annotation |
| `--font-hand` | `'Caveat', cursive` | Handwritten annotations, section labels, card eyebrows |
| _(no token)_ | `'Recoleta Alt', serif` | **Headings only** — applied directly to `h1, h2, h3` at `font-weight: 700` |

**Font loading:**
- `Plus Jakarta Sans` + `Caveat` — Google Fonts, with `preconnect`.
- `Recoleta Alt` — **self-hosted** via `@font-face` from `assets/fonts/` (`Recoleta Alt Bold.woff2` / `.otf`, weight 700, `font-display: swap`). It has no CSS custom property — headings get it through the `h1, h2, h3` rule, not a `var()`.

## 2. Typography Scale

Base: `body` is `1.125rem`, `line-height: 1.6`, `letter-spacing: 0.005em`.

| Element | Size | Weight | Notes |
|---|---|---|---|
| Hero H1 | `clamp(2.6rem, 5.5vw, 5rem)` | 800 | `Recoleta Alt`, `letter-spacing: -0.03em`, `line-height: 1.1` |
| Section Title | `clamp(1.8rem, 3.5vw, 2.5rem)` | 800 | `Recoleta Alt`, `letter-spacing: -0.02em`, `line-height: 1.15` |
| Section Label | `1.15rem` | 600 | Uses `--font-hand`, purple on purple-light bg |
| Card eyebrow / annotation | `1.1rem`–`1.45rem` | 400–600 | Uses `--font-hand` |
| Body / Paragraph | `1.1rem` (hero), `~0.9–1rem` (cards) | 400–500 | `line-height: 1.6`, color `--text-muted` |
| Small / meta text | `0.72rem`–`0.85rem` | 500–700 | Labels, nav links, footer, `.vt-mlabel` (uppercase) |

All headings inherit `Recoleta Alt` from the global `h1, h2, h3` rule. Only the **700** weight of Recoleta Alt is self-hosted, so the `800` set on hero/section titles has no matching face — browsers synthesize (faux-bold) it or fall back to 700. If true 800 headings are wanted, add an `@font-face` for the heavier weight.

## 3. Component Patterns

### Buttons
- **Primary (`.btn-primary`, `.cta-btn`)**: Yellow bg (`--accent-yellow`), dark text, `border-radius: 100px`, `font-weight: 700`. Hover: lift up with enhanced shadow.
- **Secondary (`.btn-secondary`)**: White bg, subtle border, `border-radius: 100px`, `font-weight: 600`.
- **Nav CTA (`.nav-cta`)**: Dark bg, white text, pill shape.

### Cards
- White bg (`--bg-white`), `1.5px solid var(--border)`, `border-radius: var(--radius-lg)` (24px), `--shadow-md`.
- Padding varies by component (≈24–44px) — e.g. `.vt-inner` 42×44, `.proof-card` 28×24, `.about-inner` 52×56.
- Some cards hover-lift (`.proof-card`: `translateY(-4px)` + shadow); the timeline/about/faq cards don't.

### Section Layout
Most sections follow this pattern:
```html
<section id="name">
  <div class="container section-center">
    <div class="section-label">Label Text</div>
    <h2 class="section-title">Title Text</h2>
    <div class="section-subtitle">Subtitle text.</div>
    <!-- content -->
  </div>
</section>
```
The Process section splits the centered header and the (left-aligned) timeline into **two** `.container` blocks inside one `<section>`.

### Process Timeline (`.vt-*`)
The Services/Process section is a vertical scroll-timeline, **not** a row of cards:
- **Layout**: `.vt-wrap` is a `300px 1fr` grid — a sticky left rail (`.vt-rail`, `position: sticky; top: 130px`) beside a column of tall stages (`.vt-stage`, `min-height` 68/88/80vh for first/middle/last).
- **Rail**: numbered nodes (`.vt-num`) joined by a vertical **yellow → green → purple** gradient connector (`.vt-rail::before`). Each node + label is a click-to-jump `.vt-tab` — an `<a>`, **not** `<nav>` (the global `nav { position: fixed }` rule would hijack a `<nav>` and break the grid).
- **Active state**: the inline scroll-spy adds `.active` / `.done` to tabs as you scroll (`IntersectionObserver`). The active node fills its phase colour (1 yellow · 2 green · 3 purple); the matching `.vt-lphase` recolours.
- **Cards**: each stage holds a `.vt-inner` card → `.vt-eyebrow` (hand font), `.vt-h`, `.vt-desc`, then a `.vt-meta` row of **Goal / Deliverable** (`.vt-mlabel` uppercase + `.vt-mval`). `.vt-support` is the optional green note.
- **Mobile (`≤900px`)**: the rail is hidden; stages stack as self-contained cards, each showing its own `.vt-cardnum` colour badge.
- `.vt-outro` is the closing line beneath the timeline.

### FAQ Accordion (`.faq-*`)
- Native `<details class="faq-item">` / `<summary>` — **no JS**. `.faq-list` caps width at 780px.
- Default marker hidden; a `+` / `–` toggle is drawn via `summary::after` (flips on `[open]`). Open items gain `--shadow-md`.

### Toolkit (`.toolkit-*`)
- `.toolkit-groups` is a 2-column grid of `.toolkit-group` blocks — a category `<h4>` over a `.tools-pills` row of `.tool-text` pills.
- The 5th group spans full width (`:nth-child(5) { grid-column: 1 / -1 }`); the grid collapses to 1 column at `900px`.
- `.toolkit-note` is a hand-font closing line.

### Annotations (Handwritten style)
- Font: `var(--font-hand)` (Caveat); typically purple, yellow, or a phase accent.
- Positioned with rotation (`transform: rotate(±5deg)`); used sparingly for personality (`.hero-annotation`, `.about-photo-annotation`, `.cta-annotation`).

### Badges & Pills
- **Hero badge (`.hero-badge`)**: White pill, subtle border + `--shadow-sm`, holds a green circular dot (`.hero-badge-dot`) with a `✓`. Used once, above the hero H1.
- **Tool pills (`.tool-text`)**: White rounded-`100px` pills with border; hover turns the border yellow. Grouped under category headings in the Toolkit.
- **About tags (`.about-tag`)**: Tinted pills cycling the accent palette via `:nth-child()` — yellow → green → purple → orange → pink (5 tags) — each with a matching translucent border.

### Stats Ribbon (`.stats` / `.stats-inner`)
- White card (`--radius-lg`, `--shadow-md`) holding a 4-column grid (`.stats-grid`) of `.stat` items.
- Vertical hairline dividers between stats via `.stat:not(:last-child)::after` (hidden below 900px).
- `.stat-number` is `2rem`/800; `.stat-label` is small muted text.

### About Block (`.about-inner`)
- Two-column flex card: circular photo column (`.about-photo`, 148px, yellow ring via `border` + double `box-shadow`) beside text content.
- Collapses to a centered single column below `900px` (rule lives inline in the About section, not the shared responsive block).

### Footer (`footer` / `.footer-grid`)
- 4-column grid: `1.5fr 1fr 1fr 1.5fr` (brand · links · connect · subscribe).
- **Subscribe form (`.footer-email-row`)**: pill email input + yellow pill button; input border turns yellow on focus.
- Collapses to 2 columns at `900px`, 1 column at `600px`.

## 4. Layout System

- **Container**: `.container` — `max-width: 1300px`, centered, `padding: 0 24px`. The shared `.logo img` rule sizes both logos (`height: 70px`).
- **Section spacing**: `padding: 80px 0` default.
- **No CSS framework** — custom CSS only.

## 5. Icon System

- No icon library — uses **emoji** for icons: ✓ (hero badge dot), ★ (testimonial stars), 👋 (About annotation), 🇦🇺 (Melbourne About tag).
- Inline SVGs for arrow icons in buttons (`.btn-primary`, `.cta-btn`).
- The FAQ `+`/`–` toggle and the Toolkit `→` are CSS-generated (`::after` / text content), not emoji.
- Social links in footer use text abbreviations ("in", "Up").

## 6. Responsive Breakpoints

| Breakpoint | Changes |
|---|---|
| `≤ 900px` | Process rail hidden, stages stack as cards with number badges; Toolkit & Testimonials grids → 1 column; stats → 2 columns; footer → 2 columns; nav links hidden; hero annotation hidden; About stacks; CTA narrower |
| `≤ 600px` | Hero h1 → `2rem`; section padding → `56px 0`; tighter `.stats-inner` padding; footer → 1 column |

## 7. Animation

- `fadeUp` keyframe: `opacity: 0, translateY(20px)` → `opacity: 1, translateY(0)`. Applied to hero elements with staggered delays (0s, 0.1s, 0.2s, 0.3s).
- Process timeline: rail nodes/labels transition colour (`~0.35s`) as the active phase changes; the active node also scales up.
- Hover transitions: `0.3s`–`0.4s` ease for transforms and shadows.

## 8. Color Usage Convention

Repeated items rotate through the accent palette via `:nth-child()`. The core motif is **yellow → green → purple**:
- **Process phases** (`.vt-tab` nodes, eyebrows, meta labels): 1 yellow · 2 green · 3 purple.
- **About tags** (`.about-tag`): yellow → green → purple → orange → pink (5).
- **Testimonial avatars** (`.proof-card .proof-avatar`): orange · purple · green.

Yellow / green / purple is the consistent visual motif throughout the site.

## 9. Dark Section Pattern

The CTA section uses `--bg-dark` with:
- Decorative radial gradients (yellow + purple) via `::before`/`::after` pseudo-elements.
- Light text (`--text-light`), muted text at `#9090A8`.
- Section label gets semi-transparent purple bg.

## Rules for Figma-to-Code Integration

1. **No build step** — all code goes directly into `index.html`.
2. **Use CSS custom properties** — never hardcode colors or spacing; use the tokens defined in `:root`.
3. **Follow the card pattern** — white bg, subtle border, large radius, shadow.
4. **Maintain the tricolor accent pattern** — yellow → green → purple for repeated items (see §8).
5. **Use `--font-hand` (Caveat)** only for annotations, section labels, and card eyebrows.
6. **Keep pill-shaped buttons** — `border-radius: 100px` for buttons, badges, and tool pills.
7. **Respect the container** — all content within `.container` at `max-width: 1300px`.
8. **Add responsive rules** — ensure new sections gracefully collapse at `900px` and `600px`.
9. **Keep JavaScript inline and dependency-free** — the only script is the inline vanilla scroll-spy powering the Process timeline. No external JS libraries/frameworks; add any new behaviour as inline vanilla JS (and avoid `<nav>` for non-navigation elements, since `nav {}` is globally styled).

## Git Workflow

- **`production`** — the main branch (renamed from `main`). This is the live/release branch.
- **`staging`** — integration branch. All feature work is merged here first.

**Branch workflow:**
1. Create new branches from `staging`
2. Merge feature branches → `staging`
3. When ready to release, merge `staging` → `production`
