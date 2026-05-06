# 03 — Responsive Architecture

[⬅️ Previous: 02-Flexbox-and-Grid](../02-Flexbox-and-Grid/README.md) | [⬅️ Back to Parent](../README.md)

---

## 🎯 What This Folder Covers

This folder teaches how to build **layouts that adapt** to any screen size — from mobile phones to 4K displays. Think of it as designing a building that rearranges its rooms based on available space.

---

## 1. The Viewport — The Foundation

### Understanding Viewport Units

```
┌─────────────────────────────────────────────────────────────────────┐
│                    VIEWPORT UNITS                                  │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  DEVICE: ┌──────────────────────────────────────────┐             │
│          │                viewport                    │             │
│          │  ┌────────────────────────────────────┐  │             │
│          │  │           vw = 100%                  │  │             │
│          │  │  ┌───────────────────────────────┐  │  │             │
│          │  │  │        vh = 100%              │  │  │             │
│          │  │  │  ┌─────────────────────────┐  │  │  │             │
│          │  │  │  │    vmin / vmax         │  │  │  │             │
│          │  │  │  │    (smaller/larger)    │  │  │  │             │
│          │  │  │  └─────────────────────────┘  │  │  │             │
│          │  │  └───────────────────────────────┘  │  │             │
│          │  └────────────────────────────────────┘  │             │
│          └──────────────────────────────────────────┘             │
│                                                                     │
│  vw = 1/100th of viewport width   (100vw = full width)            │
│  vh = 1/100th of viewport height  (100vh = full height)          │
│  vmin = smaller of vw/vh                                          │
│  vmax = larger of vw/vh                                           │
│                                                                     │
│  MOBILE WARNING:                                                   │
│  Mobile browsers have a "viewport" that differs from screen size  │
│  because of the address bar! Use dvh instead of vh for mobile:   │
│  • 100vh = viewport including address bar (may show blank space)   │
│  • 100dvh = dynamic viewport (adapts to show/hide bar)           │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 2. Media Queries — The Breakpoints

### Breakpoint Strategy

```
┌─────────────────────────────────────────────────────────────────────┐
│                    COMMON BREAKPOINTS                              │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  STRATEGY: Mobile-first (base styles for small, add up)           │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │  Base (mobile):      320px - 479px                         │  │
│  │  ┌─────────────────────────────────────────────────────────┐│  │
│  │  │  Base styles — single column, stacked layout         ││  │
│  │  │  font-size: 16px (prevents iOS zoom on focus)        ││  │
│  │  └─────────────────────────────────────────────────────────┘│  │
│  └─────────────────────────────────────────────────────────────┘  │
│                              ▲                                     │
│                              │ min-width: 480px                   │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │  Tablet:             480px - 767px                        │  │
│  │  ┌─────────────────────────────────────────────────────────┐│  │
│  │  │  2 columns, larger fonts, expanded navigation         ││  │
│  │  └─────────────────────────────────────────────────────────┘│  │
│  └─────────────────────────────────────────────────────────────┘  │
│                              ▲                                     │
│                              │ min-width: 768px                   │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │  Desktop:            768px - 1023px                        │  │
│  │  ┌─────────────────────────────────────────────────────────┐│  │
│  │  │  3 columns, sidebars appear, larger spacing           ││  │
│  │  └─────────────────────────────────────────────────────────┘│  │
│  └─────────────────────────────────────────────────────────────┘  │
│                              ▲                                     │
│                              │ min-width: 1024px                  │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │  Large Desktop:     1024px - 1279px                        │  │
│  │  ┌─────────────────────────────────────────────────────────┐│  │
│  │  │  Full layout, all features visible, max-width: 1400px││  │
│  │  └─────────────────────────────────────────────────────────┘│  │
│  └─────────────────────────────────────────────────────────────┘  │
│                              ▲                                     │
│                              │ min-width: 1280px                  │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │  4K / Extra Large:   1280px+                               │  │
│  │  ┌─────────────────────────────────────────────────────────┐│  │
│  │  │  Constrained content width, reduced line lengths      ││  │
│  │  └─────────────────────────────────────────────────────────┘│  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Media Query Syntax

```css
/* Mobile-first: base styles apply to all, min-width adds on */

/* Base (mobile) */
.container { padding: 16px; }

/* Tablet and up */
@media (min-width: 768px) {
  .container { padding: 24px; }
}

/* Desktop and up */
@media (min-width: 1024px) {
  .container { padding: 32px; max-width: 1200px; }
}

/* Print styles */
@media print {
  .no-print { display: none; }
}

/* Dark mode */
@media (prefers-color-scheme: dark) {
  :root { --bg: #1a1a1a; }
}

/* Reduced motion */
@media (prefers-reduced-motion: reduce) {
  * { animation: none; transition: none; }
}
```

---

## 3. Fluid Typography — Scalable Text

### The Problem with Fixed Font Sizes

```
┌─────────────────────────────────────────────────────────────────────┐
│                    FIXED VS FLUID TYPE                              │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  FIXED (static)          FLUID (scalable)                          │
│  ┌──────────────────┐   ┌──────────────────┐                       │
│  │ Desktop: 16px   │   │ viewport range   │                       │
│  │ Tablet:  16px  │   │ min: 14px → max: 18px│                      │
│  │ Mobile:   16px  │   │ scales with vw  │                       │
│  │                  │   │                  │                       │
│  │ Mobile text may │   │ Comfortable at   │                       │
│  │ be TOO LARGE    │   │ all sizes        │                       │
│  │ for small screen│   │                  │                       │
│  └──────────────────┘   └──────────────────┘                       │
│                                                                     │
│  CALC() FOR FLUID TYPE:                                            │
│  font-size: clamp(min, preferred, max);                            │
│  font-size: clamp(14px, 2.5vw + 0.5rem, 18px);                    │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Clamp Pattern

```css
/* Fluid typography with clamp */
/* Formula: clamp(min, calc(base + scaling), max) */
h1 {
  font-size: clamp(1.5rem, 3vw + 1rem, 3rem);
  /* Min: 24px, Preferred: 3vw + 16px, Max: 48px */
}

h2 {
  font-size: clamp(1.25rem, 2vw + 0.75rem, 2rem);
  /* Min: 20px, Preferred: 2vw + 12px, Max: 32px */
}

p {
  font-size: clamp(0.875rem, 1vw + 0.5rem, 1.125rem);
  /* Min: 14px, Preferred: 1vw + 8px, Max: 18px */
}
```

---

## 4. Responsive Images — srcset and sizes

### Image Sizing Strategy

```
┌─────────────────────────────────────────────────────────────────────┐
│                    RESPONSIVE IMAGE STRATEGY                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  PROBLEM:                                                          │
│  Loading a 4000px hero image on a 320px mobile = massive waste     │
│  • Mobile downloads 4000px image (2MB+)                           │
│  • Shows it scaled down to 320px                                   │
│  • User waits 10 seconds for no reason                             │
│                                                                     │
│  SOLUTION: Multiple image sizes via srcset                        │
│                                                                     │
│  <img src="hero-800.jpg"        ← Default (for no srcset support)│
│       srcset="hero-400.jpg 400w,    ← 400px wide (mobile)          │
│                hero-800.jpg 800w,    ← 800px wide (tablet)        │
│                hero-1200.jpg 1200w,  ← 1200px wide (desktop)      │
│                hero-1600.jpg 1600w"  ← 1600px wide (retina)        │
│       sizes="(max-width: 480px) 100vw,      ← Mobile: 100vw        │
│              (max-width: 1024px) 80vw,     ← Tablet: 80vw         │
│              1200px">                      ← Desktop: 1200px      │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Modern Image Formats

```html
<!-- WebP with JPEG fallback -->
<picture>
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="Description">
</picture>

<!-- AVIF (better compression) with fallback chain -->
<picture>
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="Description">
</picture>
```

---

## 5. Container Queries — The Next Evolution

### The Problem with Media Queries

```
┌─────────────────────────────────────────────────────────────────────┐
│                    MEDIA QUERY VS CONTAINER QUERY                 │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  MEDIA QUERY (viewport-based):                                    │
│  @media (min-width: 768px) { .card { flex-direction: row; } }     │
│  • Checks the BROWSER WIDTH                                       │
│  • Card doesn't know if it's in sidebar or main content          │
│  • Same component behaves same everywhere                        │
│                                                                     │
│  CONTAINER QUERY (element-based):                                 │
│  @container (min-width: 400px) { .card { flex-direction: row; } }│
│  • Checks the CONTAINER'S WIDTH                                   │
│  • Card adapts to its placement                                   │
│  • Sidebar cards stack, main-area cards row                      │
│                                                                     │
│  ┌────────────────────────────────────────────────────────────┐  │
│  │ @media BROWSER is 1024px wide                              │  │
│  │  ┌────────────────┐  ┌────────────────┐                   │  │
│  │  │ Sidebar (300px)│  │   Main (724px) │                   │  │
│  │  │ ┌────────────┐ │  │ ┌────────────┐ │                   │  │
│  │  │ │    CARD    │ │  │ │    CARD    │ │                   │  │
│  │  │ │   stacks   │ │  │ │    rows    │ │                   │  │
│  │  │ └────────────┘ │  │ └────────────┘ │                   │  │
│  │  └────────────────┘  │  └────────────────┘                   │  │
│  │                      │  CARD SHOULD KNOW                    │  │
│  │                      │  ITS CONTAINER SIZE                   │  │
│  └────────────────────────────────────────────────────────────┘  │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Container Query Syntax

```css
/* Define a containment context */
.card-wrapper {
  container-type: inline-size;
  container-name: card;
}

/* Query the container */
@container card (min-width: 400px) {
  .card {
    flex-direction: row;
  }
}

@container card (max-width: 399px) {
  .card {
    flex-direction: column;
  }
}
```

---

## 6. Responsive Architecture Patterns

### The Fluid Grid

```css
/* Fluid grid that adapts without breakpoints */
.grid {
  display: grid;
  /* Creates as many columns as fit, min 250px each */
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1rem;
}

/* Result:
   1280px+ → 4 columns
   1024px → 3 columns
   768px → 2-3 columns
   480px → 1-2 columns
   320px → 1 column
*/
```

### Flexbox-Based Component Patterns

```css
/* Responsive navbar */
.navbar {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.navbar-brand { flex: 1 0 200px; }  /* Grows to fill space */
.navbar-links { display: flex; gap: 1rem; } /* Shrinks as needed */

/* Responsive card */
.card {
  display: flex;
  flex-direction: column;
}

@media (min-width: 480px) {
  .card { flex-direction: row; }
}
```

### Mobile-First Component Architecture

```css
/* 1. Base (mobile) — stacked, simple */
.component { display: flex; flex-direction: column; }

/* 2. Tablet — 2 columns possible */
@media (min-width: 480px) {
  .component { flex-direction: row; }
}

/* 3. Desktop — sidebars appear */
@media (min-width: 768px) {
  .component-aside { display: block; }
}

/* 4. Large — constrained max-width */
@media (min-width: 1024px) {
  .component { max-width: 1200px; margin: 0 auto; }
}
```

---

## 📋 Quick Reference Card

| Concept | Remember |
|---------|----------|
| Viewport units | `vw`, `vh`, `dvh`, `vmin`, `vmax` |
| Breakpoints | Mobile-first: add complexity with `min-width` |
| Fluid type | `clamp(min, preferred, max)` |
| Responsive images | `srcset` with `sizes` |
| Container queries | Query element size, not viewport (modern browsers) |

---

## ✅ What TO Do

1. **Start mobile-first** — Base styles for small, add complexity with `min-width`
2. **Use relative units** — `rem` for typography, `em` for spacing, `%` for containers
3. **Use `clamp()` for fluid typography** — No breakpoint needed for text scaling
4. **Use responsive images** — `srcset` serves the right size for each device
5. **Constrain content width** — Lines over 75 characters are hard to read

## ❌ What NOT To Do

1. **Don't set `width: 100vw` on mobile** — Address bar causes horizontal scroll; use `100dvw`
2. **Don't use fixed breakpoints for everything** — Use fluid grids with `auto-fit` and `minmax()`
3. **Don't load 4K images on mobile** — Use `srcset` or serve WebP/AVIF
4. **Don't forget `text-size-adjust`** — Prevents iOS text inflation on orientation change
5. **Don't use `max-width: 100%` on images then set a fixed width** — Conflicts happen; use `img { max-width: 100%; height: auto; }`

## 🏢 Workplace Wisdom

> "Responsive design isn't a feature. It's the baseline. If your site doesn't work on mobile, it doesn't work for the majority of users."
>
> "Container queries are the future. Once browsers fully support them, responsive components will finally be truly portable — the same component can adapt whether it's in a sidebar or main area."
