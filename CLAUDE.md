# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static marketing website for **DurianBoy Jeff**, a durian business with two stalls in Perak, Malaysia. Live at [durianboyjeff.com](https://durianboyjeff.com/). Built with Vite + Tailwind CSS v3, no JavaScript framework.

## Commands

```bash
npm run dev        # Start dev server (http://localhost:5173)
npm run build      # Build to dist/
npm run preview    # Preview the built dist/ locally
```

Deployment is automatic: push to `main` triggers the GitHub Actions workflow (`.github/workflows/deploy.yml`) that builds and deploys to GitHub Pages.

## Architecture

The entire site is a **single HTML page** (`index.html`) with inline JavaScript at the bottom. There is no component system or routing.

- `index.html` — all markup, structured data (JSON-LD), SEO meta tags, and inline JS for mobile nav + smooth scroll
- `style.css` — Tailwind directives + three reusable component classes: `.container-custom`, `.btn` / `.btn-alt`, `.tag`
- `main.js` — Vite entry point; only imports `style.css`
- `tailwind.config.js` — custom design tokens; always use these instead of raw Tailwind colors
- `assets/` — all images (logo, hero, about, variety photos, review screenshots, QR codes)

## Design Tokens

All brand colors are defined in `tailwind.config.js` under `theme.extend.colors`:

| Token | Use |
|---|---|
| `green` | Primary brand, CTA buttons, accents |
| `lime` | Tag/badge backgrounds |
| `gold` | Secondary CTA (`btn-alt`) |
| `cream` | Page background, section fills |
| `ink` | Primary text |
| `muted` | Secondary/body text |

Custom animations: `animate-fade-in-up` (hero entrance), `animate-scroll-x` (reviews rail at 80s loop).

## Sections

`index.html` contains these sections in order, each with a matching `id` for anchor nav:
`#home` → `#about` → `#varieties` → `#why` → `#delivery` → `#reviews` → `#guarantee` → `#contact`

The reviews carousel (`#reviews`) uses a CSS-only infinite scroll — two identical sets of 6 images inside `.track` so the loop is seamless. To add a review, add the image to both sets.

## Pickup & Delivery Section (`#delivery`)

Contains two location cards (Taiping and Sitiawan) in a `grid grid-cols-1 md:grid-cols-2` layout. Each card uses `flex-row` with a pin icon on the left and text on the right.

**Important layout note:** The heading/subtitle live in a `max-w-3xl` centered container, but the cards grid sits **outside** that container as a sibling `div` directly inside the `<section>`. This is intentional — keeping the grid inside `max-w-3xl` would constrain card width and cause address text to wrap to 3 lines.

Each location also has its own `LocalBusiness` JSON-LD block in `<head>`. When adding or editing locations, update both the card HTML and the corresponding schema block.
