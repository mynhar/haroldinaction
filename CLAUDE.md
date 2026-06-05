# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Single-file HTML/CSS/JS personal landing page for Harold M. Bonilla López — a Java Enterprise engineer in the financial sector. No build tools, no dependencies beyond two Google Fonts loaded at runtime.

**Entry point:** `index.html` — all CSS, HTML and JavaScript live in this one file.

## Running Locally

Open with VS Code Live Server on port **5501** (configured in `.vscode/settings.json`). No build step, no package manager.

```
# Via VS Code: right-click index.html → "Open with Live Server"
# Direct: open index.html in any browser
```

## Architecture

### Single-file structure

```
index.html
├── <head>
│   ├── Google Fonts: Inter (body) + JetBrains Mono (technical accents)
│   └── <style>  — all CSS in one block (~1000 lines)
├── <body>
│   ├── <nav>           — fixed, shrinks on scroll, active section highlight
│   ├── .mobile-menu    — fullscreen overlay (hidden by default)
│   ├── <header>        — Hero: split layout, bg image right, content left
│   ├── .stats-bar      — 4 metrics bar
│   ├── #about          — 3fr/2fr grid: text + tech logos
│   ├── #services       — 3-col grid (unified border grid pattern)
│   ├── #experience     — 2-col grid (unified border grid pattern)
│   ├── #skills         — 3-col grid (unified border grid pattern)
│   ├── #education      — 2-col grid: 4 edu/cert cards (unified border grid pattern)
│   ├── #contact        — CTA section (solid dark, no gradient)
│   ├── <footer>        — 3-col layout
│   └── <script>        — all JS inline (~60 lines)
```

### CSS design tokens (`:root`)

| Token | Value | Role |
|---|---|---|
| `--primary` | `#0e2b4e` | Anchor navy |
| `--primary-light` | `#1a5cb8` | Royal blue, links |
| `--secondary` | `#0696b8` | Teal-cyan, accents |
| `--accent` | `#e6890d` | Amber, CTA buttons |
| `--dark` | `#060e1c` | Section backgrounds |
| `--radius` | `6px` | Default radius |
| `--radius-lg` | `10px` | Card radius |

### JavaScript (inline `<script>`)

Five independent behaviours, no framework:

1. **Hamburger menu** — `openMenu()` / `closeMenu()`, double-rAF for fade-in transition, locks body scroll
2. **Nav shrink** — adds `.scrolled` class after 60px scroll
3. **Active nav** — scroll listener checks `offsetTop` ranges, sets `.active` on matching `a[data-section]`
4. **Scroll reveal** — `IntersectionObserver` adds `.visible` to `.reveal` elements at 10% threshold
5. **Back to top** — shows button after 400px, smooth scroll on click

### Hero layout

Split design: content occupies left 55% (`width: 55%`, `max-width: 600px`), background image anchored `right center`. Overlay gradient goes from `opacity 0.97` (left) to `0.10` (right) at `100deg`, revealing the illustration on the right half.

Mobile (`≤768px`): reverts to centered layout — `width: 100%`, `text-align: center`, `justify-content: center`, `.hero-glow` hidden.

### Section background rhythm

```
Hero       → dark overlay on image
Stats bar  → --dark  (#060e1c)
About      → --white
Services   → --gray-100
Experience → --dark
Skills     → --white
Education  → --gray-100
CTA        → --gray-900  (solid, no gradient)
Footer     → --dark
```

### Grid pattern (services / experience / skills)

These three sections use a "unified grid" pattern — the grid container has `background: var(--gray-200)` and `gap: 1px`, creating thin divider lines between cards. Individual cards have `background: var(--white)` or dark equivalent. This is intentional; do not add `gap` or individual card borders.

## Images

All images are in `./img/`. Available MVC architecture images:

| File | Notes |
|---|---|
| `10-MVC_Arquitectura.jpeg` | Highest resolution, no embedded text, wide landscape — best for hero |
| `03-MVC_Architecture.png` | Similar to `10`, no embedded text |
| `01-MVC_Architecture.png` | Has "MVC Architecture in Java" text baked in — conflicts with hero overlay |
| `06-MVC_Arquitectura.png` | Has embedded text and Java logo overlay |
| `05-MVC_Arquitectura.jpg` | Small thumbnail, pixelates at full screen |

The hero `.hero-bg` `background-image` path is the only place an image filename appears in `index.html`.

## Design conventions

- **Body text:** Inter (proportional)
- **Technical accents:** JetBrains Mono — used for: nav brand, section labels, stat numbers, skill tags, experience periods, footer headings, copyright
- **Section labels** render as `// LABEL_TEXT` via `::before { content: "//" }`
- **Headings:** `letter-spacing: -0.025em` to `-0.03em` — keep this for the compressed enterprise feel
- **No box-shadows on light sections** — use `border: 1px solid var(--gray-200)` instead
- **Responsive breakpoints:** `1100px` (2-col grids) and `768px` (single col, mobile nav)
