# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Single-page static website for Indie Sportsplex, a cricket practice facility in South Austin, TX. No build step, no package manager, no framework — just `index.html` with TailwindCSS via CDN.

**To preview:** Open `index.html` in a browser directly, or use any static file server:
```
python3 -m http.server 8000
```

**Deployed to:** GitHub Pages at `www.indiesportsplex.com` (configured via `CNAME`).

## Architecture

Everything lives in `index.html` — HTML, inline `<style>`, inline `<script>`, and Tailwind utility classes. There are no separate CSS or JS files.

**Sections (in order):** Nav → Hero → Features → Youth Coaching → Pricing → Gallery (+ Lightbox) → Team → Contact → Footer

**Tailwind config** is inlined in a `<script>` block in `<head>`, extending the theme with `font-heading` (Plus Jakarta Sans).

**JavaScript (~120 lines at bottom of `<body>`) handles:**
- Sticky nav: transparent over hero (IntersectionObserver), white/blur otherwise
- Active nav highlighting: IntersectionObserver with `rootMargin: '-40% 0px -60% 0px'`
- Mobile menu toggle
- Gallery lightbox with keyboard (Escape/arrow keys) and touch swipe support
- Scroll-triggered fade-up animations via `getBoundingClientRect` (not IntersectionObserver) on `scroll` + `load` events

**Scroll animations:** Elements get class `fade-up` in HTML; JS adds `visible` when they enter viewport. CSS transitions handle the actual animation.

## Key Details

- **Color palette:** `green-800` (primary), `amber-500` (accent), `stone-50` (light bg), `stone-900` (dark sections)
- **Font:** Plus Jakarta Sans (Google Fonts) used via `font-heading` Tailwind class for headings; system sans-serif for body
- **Booking link:** `https://book.runswiftapp.com/facilities/indie-sportsplex` (external, opens in new tab) — used in multiple CTAs
- **Social media links:** Currently `href="#"` placeholders in Contact section and Footer — awaiting real URLs
- **Images:** `images/` directory — `hero-bg.jpg`, `gallery-1..5.jpg`, `logo.png`, `mayur.jpg`, `rohan.jpg`
- **Gallery lightbox:** `data-index` attributes on `.gallery-img` elements map to the `imgSrcs` array built from those same elements

## Git Conventions

- **Commit messages:** Always succinct, single-line only — no body, no bullet points.
