# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Atlas Leads is the agency website for an HVAC lead generation and CRM service. The entire site is a single self-contained static file with no build system, no package manager, and no external dependencies beyond Google Fonts loaded at runtime via CDN.

## Repository Structure

```
index.html   — the entire website (HTML + CSS + JS, all inline)
README.md    — one-line description
```

There is no build step, no `package.json`, no bundler, and no test suite. To "run" the site, open `index.html` directly in a browser or serve it with any static file server, e.g.:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

## Architecture & Conventions

### Single-file layout

All code lives in `index.html` in this order:
1. `<head>` — Google Fonts import, then a single `<style>` block containing all CSS
2. `<body>` — semantic HTML sections (nav, hero, problem, services, how-it-works, CTA, footer)
3. A single `<script>` block at the bottom — scroll-reveal via `IntersectionObserver`, and staggered card animations

### Design system (CSS custom properties)

Colours and tokens are defined in `:root`:

| Variable | Value | Usage |
|---|---|---|
| `--black` | `#0a0a0a` | page background |
| `--dark` | `#111214` | alternating section background |
| `--card` | `#16191e` | card surfaces |
| `--border` | `#1f2430` | all borders / dividers |
| `--orange` | `#f36c21` | primary accent / CTA colour |
| `--orange-glow` | `rgba(243,108,33,0.18)` | icon backgrounds, glows |
| `--white` | `#f0ede8` | body text |
| `--muted` | `#7a8299` | secondary / label text |
| `--accent` | `#1a8fff` | blue accent (used sparingly in hero gradient) |

### Typography

Three Google Fonts families, loaded via a single CDN `<link>`:
- **Bebas Neue** — all headings (`h1`, `h2`, `h3`, section stats)
- **DM Sans** — body text and buttons
- **DM Mono** — labels, tags, monospace badges

### Scroll reveal

Elements with class `.reveal` start invisible (`opacity: 0; transform: translateY(30px)`) and gain class `.visible` when they enter the viewport via an `IntersectionObserver` (threshold 0.1, rootMargin `-60px` at bottom). The observer disconnects each element after it fires once (`observer.unobserve`).

### Sections & anchors

| Section | ID | Notes |
|---|---|---|
| Navigation | *(fixed)* | hides `ul` on `max-width: 900px` |
| Hero | *(none)* | stat strip with three metrics |
| Problem | `#problem` | two-column grid, collapses to 1-col on mobile |
| Services | `#services` | 3-column grid using `1.5px` gap as border trick |
| How It Works | `#how` | numbered steps with vertical line via `::before` pseudo |
| CTA / Contact | `#contact` | centred, radial gradient background |
| Footer | *(none)* | flex row, collapses to column on mobile |

### External links

All CTA buttons and the nav "Book a Call" link to the same Calendly URL:
`https://calendly.com/drikusbisschoff/al-agency-discovery-call`

## Making Changes

- Edit `index.html` directly — there is no compilation step.
- CSS changes go inside the single `<style>` block in `<head>`; keep new rules consistent with existing CSS custom-property usage.
- JavaScript changes go inside the single `<script>` block before `</body>`.
- Responsive breakpoint is `max-width: 900px`; all mobile overrides live in the `@media` block at the bottom of the `<style>` section.
- The noise overlay is a fixed SVG `data:` URL on `body::before` (z-index 999, pointer-events none) — do not remove it; it is intentional texture.
