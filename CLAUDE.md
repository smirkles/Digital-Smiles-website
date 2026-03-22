# Digital Smiles — Design System Rules

## Overview
Single-page static website for an AI automation consulting business. No build system, no frameworks — just a single `index.html` file with inline CSS and vanilla HTML.

## Project Structure
```
/
└── index.html    # Entire site: HTML + inline <style> block + no JS
```

## 1. Token Definitions (CSS Custom Properties)

All design tokens are defined in `:root` within the `<style>` block of `index.html`.

### Colors

| Token | Value | Usage |
|---|---|---|
| `--bg` | `#FDF6EE` | Page background (warm cream) |
| `--bg-white` | `#FFFFFF` | Card/component backgrounds |
| `--bg-dark` | `#1A1A2E` | Dark sections (CTA) |
| `--bg-dark-card` | `#242440` | Cards on dark backgrounds |
| `--text` | `#1A1A2E` | Primary text |
| `--text-muted` | `#6B6B80` | Secondary/body text |
| `--text-light` | `#F0EDE8` | Text on dark backgrounds |
| `--accent-yellow` | `#FFD23F` | Primary accent, CTAs, buttons |
| `--accent-yellow-light` | `#FFF3CD` | Yellow tinted backgrounds |
| `--accent-green` | `#6BCB77` | Success, "popular" badges |
| `--accent-green-light` | `#E8F8EA` | Green tinted backgrounds |
| `--accent-purple` | `#9B8FE8` | Section labels, annotations |
| `--accent-purple-light` | `#EEEAFF` | Purple tinted backgrounds |
| `--accent-orange` | `#FF8C42` | Price highlights, accents |
| `--accent-orange-light` | `#FFF0E5` | Orange tinted backgrounds |
| `--accent-pink` | `#FF6B8A` | Spare accent (defined, lightly used) |

### Borders & Shadows

| Token | Value |
|---|---|
| `--border` | `rgba(0,0,0,0.06)` |
| `--border-light` | `rgba(255,255,255,0.1)` |
| `--shadow-sm` | `0 2px 8px rgba(0,0,0,0.04)` |
| `--shadow-md` | `0 8px 30px rgba(0,0,0,0.06)` |
| `--shadow-lg` | `0 16px 50px rgba(0,0,0,0.08)` |

### Radii

| Token | Value |
|---|---|
| `--radius` | `16px` |
| `--radius-lg` | `24px` |

Pill-shaped elements (buttons, badges) use `border-radius: 100px`.

### Typography

| Token | Value | Usage |
|---|---|---|
| `--font-main` | `'Plus Jakarta Sans', sans-serif` | All body/heading text |
| `--font-hand` | `'Caveat', cursive` | Handwritten annotations, section labels |

Fonts loaded from Google Fonts with `preconnect`.

## 2. Typography Scale

| Element | Size | Weight | Notes |
|---|---|---|---|
| Hero H1 | `clamp(2.6rem, 5.5vw, 4rem)` | 800 | `letter-spacing: -0.03em`, `line-height: 1.1` |
| Section Title | `clamp(1.8rem, 3.5vw, 2.5rem)` | 800 | `letter-spacing: -0.02em`, `line-height: 1.15` |
| Section Label | `1.15rem` | 600 | Uses `--font-hand`, purple on purple-light bg |
| Body / Paragraph | `1.1rem` (hero), `0.9rem` (cards) | 400–500 | `line-height: 1.6`, color `--text-muted` |
| Small text | `0.82rem`–`0.85rem` | 500–600 | Labels, nav links, footer |

## 3. Component Patterns

### Buttons
- **Primary (`.btn-primary`, `.cta-btn`)**: Yellow bg (`--accent-yellow`), dark text, `border-radius: 100px`, `font-weight: 700`. Hover: lift up with enhanced shadow.
- **Secondary (`.btn-secondary`)**: White bg, subtle border, `border-radius: 100px`, `font-weight: 600`.
- **Nav CTA (`.nav-cta`)**: Dark bg, white text, pill shape.

### Cards
- White bg (`--bg-white`), `1.5px solid var(--border)`, `border-radius: var(--radius-lg)` (24px).
- Hover effect: `translateY(-4px to -6px)` + shadow elevation.
- Padding: typically `28px–32px` horizontal, `24px–26px` vertical.

### Section Layout
Each section follows this pattern:
```html
<section id="name">
  <div class="container section-center">
    <div class="section-label">Label Text</div>
    <div class="section-title">Title Text</div>
    <div class="section-subtitle">Subtitle text.</div>
    <!-- Grid content -->
  </div>
</section>
```

### Grids
- 3-column layout for packages, steps, testimonials: `grid-template-columns: repeat(3, 1fr)`.
- 4-column for stats: `repeat(4, 1fr)`.
- Responsive: collapse to `1fr` below `900px`.

### Annotations (Handwritten style)
- Font: `var(--font-hand)` (Caveat)
- Typically purple or yellow colored
- Positioned with rotation (`transform: rotate(±5deg)`)
- Used sparingly for personality

## 4. Layout System

- **Container**: `.container` — `max-width: 1120px`, centered, `padding: 0 24px`.
- **Section spacing**: `padding: 80px 0` default.
- **No CSS framework** — custom CSS only.

## 5. Icon System

- No icon library — uses **emoji** for icons (⚡, 🔧, 🧠, ✓, ★).
- Inline SVGs for arrow icons in buttons.
- Social links in footer use text abbreviations ("in", "Up").

## 6. Responsive Breakpoints

| Breakpoint | Changes |
|---|---|
| `≤ 900px` | Grids → single column, nav links hidden, annotations hidden, CTA section narrower |
| `≤ 600px` | Hero h1 → `2rem`, section padding → `56px 0`, tighter card spacing |

## 7. Animation

- `fadeUp` keyframe: `opacity: 0, translateY(20px)` → `opacity: 1, translateY(0)`.
- Applied to hero elements with staggered delays (0s, 0.1s, 0.2s, 0.3s).
- Hover transitions: `0.3s`–`0.4s` ease for transforms and shadows.

## 8. Color Usage Convention

Each of the 3 package/step/testimonial cards uses a **different accent color** via `:nth-child()`:
1. Yellow/Orange (1st)
2. Green (2nd)
3. Purple (3rd)

This tricolor pattern is a consistent visual motif throughout the site.

## 9. Dark Section Pattern

The CTA section uses `--bg-dark` with:
- Decorative radial gradients (yellow + purple) via `::before`/`::after` pseudo-elements.
- Light text (`--text-light`), muted text at `#9090A8`.
- Section label gets semi-transparent purple bg.

## Rules for Figma-to-Code Integration

1. **No build step** — all code goes directly into `index.html`.
2. **Use CSS custom properties** — never hardcode colors or spacing; use the tokens defined in `:root`.
3. **Follow the card pattern** — white bg, subtle border, large radius, hover lift.
4. **Maintain the tricolor accent pattern** — yellow/orange, green, purple for repeated items.
5. **Use `--font-hand` (Caveat)** only for annotations and section labels.
6. **Keep pill-shaped buttons** — `border-radius: 100px` for all interactive elements.
7. **Respect the container** — all content within `.container` at `max-width: 1120px`.
8. **Add responsive rules** — ensure new sections gracefully collapse at `900px` and `600px`.
9. **No external JS dependencies** — keep the site static and lightweight.
