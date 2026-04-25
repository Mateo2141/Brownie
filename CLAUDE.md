# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Single-file landing page for **Guati** — a Colombian artisanal alfajor brand (galleta de yuca + arequipe de guatila). Everything lives in `index.html`: all CSS, HTML, and JS are embedded. No build step, no package manager, no backend.

To preview: open `index.html` in any browser. Fully responsive (mobile, tablet, desktop).

## Libraries

**Only Google Fonts** (no external JS libraries):
- `Dancing Script` weight 700 → `--font-script` (brand name, display titles)
- `Montserrat` weights 400/500/600/700 → `--font-body` (everything else)

## Color Palette (CSS custom properties)

```
--verde-oscuro:  #2B4A1F   ← navbar, footer, primary buttons
--verde-medio:   #3D6B2C   ← accents, card borders, hover states
--beige-fondo:   #C8A96E   ← kraft paper background sections
--beige-suave:   #F0E0C0   ← alternate section backgrounds
--beige-claro:   #FAF5E9   ← light section backgrounds
--marron-tierra: #6B3F1A   ← secondary text, descriptors
--blanco:        #FFFFFF
--negro-suave:   #1A1A1A   ← footer background
```

## Architecture

### HTML Structure
```
<head>   Google Fonts + all CSS
<body>
  <svg> (hidden)         — SVG symbol definitions for all icons
  <nav>                  — Fixed navbar
  <div class="nav__mobile"> — Mobile menu overlay
  <section #hero>        — 100vh hero, kraft-bg
  <section #producto>    — Product card + ingredient cards
  <section #historia>    — kraft-bg, verde-oscuro, glassmorphism
  <section #porque>      — Expandable reason cards (5 items)
  <section #mascota>     — CSS mascot + state buttons
  <section #numeros>     — Animated counters + CTA
  <footer>               — 3-column grid + social icons
  <div #chatbot-container> — Fixed floating chatbot placeholder
  <script>               — All vanilla JS
```

### CSS Organization (single `<style>` block, numbered sections)
1. Reset & variables
2. Typography utilities
3. Layout utilities (`.container`, `.section-pad`, `[data-animate]`)
4. Kraft texture overlay (`.kraft-bg` via `::before` with SVG feTurbulence)
5. Navbar + mobile menu
6. Hero
7. Producto section
8. Historia section
9. Por qué Guati section
10. Mascota section
11. Números section
12. Chatbot
13. Footer
14–15. Responsive (tablet 1024px, mobile 768px)

### SVG Icons
All icons are defined as `<symbol>` elements in a hidden `<svg>` block at the top of `<body>`. Referenced with `<use href="#icon-name">`. Available symbols: `icon-leaf`, `icon-check`, `icon-ban`, `icon-globe`, `icon-plant`, `icon-recycle`, `icon-flag`, `icon-bulb`, `icon-chart`, `icon-star`, `icon-heart`, `icon-instagram`, `icon-tiktok`, `icon-facebook`, `icon-whatsapp`, `icon-chevron-down`, `icon-yuca`, `icon-guatila`, `icon-panela`.

## Key Patterns

### Scroll animations
All animated elements use `[data-animate]` attribute. CSS sets `opacity:0; transform:translateY(32px)`. JS `IntersectionObserver` adds `.is-visible` class (triggers transition). Stagger via `style="transition-delay:0.Xs"` inline.

### Expandable reason cards
Hybrid approach: CSS `:hover` expands on pointer devices; JS click-toggle (accordion) on touch. Both use `max-height` transition on `.reason-card__body`. JS detects via `matchMedia('(hover: hover)')`.

### Counter animation
`IntersectionObserver` on `#countersBlock` fires once. `requestAnimationFrame` loop with quadratic ease-out over 2000ms. Target from `data-target`, suffix from `data-suffix`.

### Mascot CSS
Built entirely in CSS + inline SVG (no images). Three animation states toggled by JS class on `#mascotMain`: default float (`mascot-float` keyframe), `.mascot--bailando` (rotate + bounce), `.mascot--saludando` (float + right arm wave). Force reflow with `void el.offsetWidth` before class swap to restart animation cleanly.

### Kraft texture
`.kraft-bg` sections get a `::before` pseudo-element with an inline SVG `feTurbulence` `data:image/svg+xml` background, `opacity:0.07`, `pointer-events:none`.

### Navbar scroll
`window.scroll` listener throttled via `requestAnimationFrame` flag. Adds `.nav--scrolled` class at `scrollY > 80` (applies `backdrop-filter:blur(10px)` + shadow).

## Assets

- `assets/images/alfajor-guatila.png` — product hero image (used in Producto section)
- `assets/images/brownie.png` — unused
- `assets/videos/reveal.mp4` — unused (was part of old "Antojo Criollo" flow)

## Responsive Breakpoints

- `> 1024px` — full desktop layout
- `≤ 1024px` — tablet: `historia` single column, nav link spacing reduced
- `≤ 768px` — mobile: hamburger menu, all grids single column, 200px mascot
- `≤ 480px` — extra-small: attr-grid single column, chips column
