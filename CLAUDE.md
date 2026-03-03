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

**Sections (in order):** Nav → Mobile Menu Overlay → Hero → Features → Pricing (includes Youth Coaching card) → Gallery (+ Lightbox) → Team → Contact → Footer

**Tailwind config** is inlined in a `<script>` block in `<head>`, extending the theme with `font-heading` (Plus Jakarta Sans).

**JavaScript (~130 lines at bottom of `<body>`) handles:**
- Sticky nav: transparent over hero (IntersectionObserver), white/blur otherwise — also toggles hamburger button color (`text-white` ↔ `text-stone-700`)
- Active nav highlighting: IntersectionObserver with `rootMargin: '-40% 0px -60% 0px'`
- Mobile menu toggle (open/close)
- Gallery lightbox with keyboard (Escape/arrow keys) and touch swipe support
- Scroll-triggered fade-up animations via `getBoundingClientRect` (not IntersectionObserver) on `scroll` + `load` events

**Scroll animations:** Elements get class `fade-up` in HTML; JS adds `visible` when they enter viewport. CSS transitions handle the actual animation.

## Key Details

- **Color palette:** `slate-800`/`slate-900` (primary/navy), `red-700`/`red-800` (accent/brick red), `stone-50` (light bg), `stone-900` (dark sections)
- **Font:** Plus Jakarta Sans (Google Fonts) used via `font-heading` Tailwind class for headings; system sans-serif for body
- **Booking link:** `https://book.runswiftapp.com/facilities/indie-sportsplex` (external, opens in new tab) — used in multiple CTAs
- **Social media links:** Instagram (`https://www.instagram.com/indiesportsplex/`) and Facebook (`https://www.facebook.com/indiesportsplex`) — real URLs in Contact section and Footer
- **Images:** `images/` directory — `hero-bg.jpg`, `gallery-1..5.jpg`, `logo.png`, `mayur.jpg`, `rohan.jpg`
- **Gallery lightbox:** `data-index` attributes on `.gallery-img` elements map to the `imgSrcs` array built from those same elements

## Hero Section

- Large logo (`h-32 sm:h-36 lg:h-44`) displayed at top of hero content above the badge and heading
- Background: solid `bg-stone-900` base + `<img>` at `opacity-35` (no gradient overlay)
- Two CTAs: "Book a Lane" (red-700) and "Memberships" (white outline)

## Mobile Menu

- The `<div id="mobile-menu">` lives **outside** `<nav>`, directly after `</nav>`, at `z-40`
- The `<nav>` is `z-50`, so it always renders above the overlay — hamburger/X button is never blocked
- Overlay is solid `bg-stone-900` (no backdrop-blur — blur caused white-out on light page sections)
- Hamburger button color is synced with nav state in `heroObserver`: `text-white` over hero, `text-stone-700` when scrolled

## Git Conventions

- **Commit messages:** Always succinct, single-line only — no body, no bullet points.
