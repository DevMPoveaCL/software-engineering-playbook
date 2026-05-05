# 02 — Flexbox and Grid

[⬅️ Back to Parent](../README.md)

---

## 🎯 What This Folder Covers

This folder teaches the **two modern CSS layout systems** — Flexbox for one-dimensional alignment, Grid for two-dimensional layouts. Master these and you'll never need float again.

---

## 1. Flexbox — The Alignment Power Tool

### When to Use Flexbox

```
┌─────────────────────────────────────────────────────────────────────┐
│                    FLEXBOX USE CASES                                │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  USE FLEXBOX FOR:                                                  │
│  • Aligning items in ONE direction (row OR column)                 │
│  • Distributing space between items                                │
│  • Centering content (both axes)                                  │
│  • Navigation menus                                                │
│  • Card layouts that wrap                                          │
│  • Fill remaining space with one item                             │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │ NAVBAR: [Logo]     [Home] [About] [Contact]      [Login] → │  │
│  │            flex-grow: 1        flex-grow: 0                   │  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │ CARD WRAP:  [Card] [Card] [Card] [Card] [Card]              │  │
│  │              flex-wrap: wrap (wraps to next line)            │  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │ CENTERED:                                                    │  │
│  │                    ┌──────────┐                              │  │
│  │   ┌───────────────►│  Center  │◄──────────────┐             │  │
│  │   │  justify-content: center                      │             │  │
│  │   │  align-items: center                          │             │  │
│  │   └───────────────►│  Box     │◄──────────────┘             │  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### The Two Axes

```
┌─────────────────────────────────────────────────────────────────────┐
│                    FLEXBOX AXES                                   │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  MAIN AXIS (direction of flex-direction)                          │
│                                                                     │
│  row (default)                 column                              │
│  ←━━━━━━━━━━━━━━━━━━━━━━━━━►    ↑                                  │
│  Main Axis                      Main Axis                           │
│                                  ↓                                  │
│       ↑ Cross Axis                 ←━━━━━━━━━━━━━━━━━━━━━━━━━►     │
│       │                             Cross Axis                       │
│                                                                     │
│  flex-direction: row        → items flow LEFT TO RIGHT           │
│  flex-direction: row-reverse→ items flow RIGHT TO LEFT            │
│  flex-direction: column     → items flow TOP TO BOTTOM             │
│  flex-direction: column-reverse → BOTTOM TO TOP                    │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Container Properties

| Property | Values | What It Does |
|----------|--------|-------------|
| `display` | `flex` | Activates flexbox |
| `flex-direction` | `row` `column` `row-reverse` `column-reverse` | Sets main axis direction |
| `justify-content` | `flex-start` `center` `flex-end` `space-between` `space-around` `space-evenly` | Aligns items along main axis |
| `align-items` | `stretch` `flex-start` `center` `flex-end` `baseline` | Aligns items along cross axis |
| `flex-wrap` | `nowrap` `wrap` `wrap-reverse` | Allows items to wrap |
| `gap` | `8px` `1rem` `50%` | Space between items |

### Justify-Content Visual Guide

```
┌─────────────────────────────────────────────────────────────────────┐
│              justify-content (MAIN AXIS alignment)               │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  flex-start          center           flex-end                     │
│  [Item][Item][Item]   [Item][Item][Item]   [Item][Item][Item]     │
│                                                                     │
│  space-between       space-around     space-evenly                │
│  [    Item    ][Item][    Item    ] [ Item ][Item][ Item ]        │
│                                 [ Item ][Item][ Item ]            │
│                                   ↑  equal space around           │
│                              even space both sides                 │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Align-Items Visual Guide

```
┌─────────────────────────────────────────────────────────────────────┐
│              align-items (CROSS AXIS alignment)                    │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  stretch (default)     flex-start         center                 │
│  ┌───────────────┐      ┌───────────────┐  ┌───────────────┐     │
│  │               │      │               │  │               │     │
│  │   [Item]      │      │   [Item]      │  │               │     │
│  │   [Item]      │      │   [Item]      │  │   [Item]      │     │
│  │               │      │               │  │   [Item]      │     │
│  │   (fills)     │      │               │  │               │     │
│  └───────────────┘      └───────────────┘  └───────────────┘     │
│                                                                     │
│  baseline (text alignment)                                         │
│  ┌───────────────┐                                                │
│  │  [Small text] │ ← All text baselines align                     │
│  │  [MEDIUM]     │                                                │
│  │  [large text] │                                                │
│  └───────────────┘                                                │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 2. Flex Item Properties

### Grow, Shrink, Basis

```
┌─────────────────────────────────────────────────────────────────────┐
│                    FLEX GROW / SHRINK / BASIS                      │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  flex-basis: initial size before flex-grow/shrink                 │
│  flex-grow: how much to GROW to fill available space             │
│  flex-shrink: how much to SHRINK when space is limited            │
│                                                                     │
│  Example: 3 items in a 300px container                            │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │ [   100px   ][   100px   ][   100px   ]  Total: 300px        │  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                                                     │
│  flex-grow: 1, 0, 0 (first grows, others stay fixed)              │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │ [100px+grows][   100px   ][   100px   ]  First: 200px        │  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                                                     │
│  flex-grow: 1, 1, 1 (all grow equally)                            │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │ [   100px   ][   100px   ][   100px   ]  Each: 100px         │  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                                                     │
│  flex-shrink: 0, 1, 1 (first won't shrink, others will)           │
│  (when container shrinks below 300px)                            │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │ [   100px   ][    80px   ][    80px   ]  Total: 260px       │  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Shorthand Pattern

```css
/* flex: grow shrink basis */
.item { flex: 1 0 200px; }

/* Common patterns */
.grow { flex: 1; }        /* flex: 1 1 0% — equal growth, shrink if needed */
.fixed { flex: 0 0 200px; } /* No growth, no shrink, fixed 200px */
.stretch { flex: 1 0 100%; } /* Grow to fill space */
```

---

## 3. CSS Grid — Two-Dimensional Layouts

### When to Use Grid

```
┌─────────────────────────────────────────────────────────────────────┐
│                    GRID USE CASES                                  │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  USE GRID FOR:                                                    │
│  • Layouts with BOTH rows AND columns                             │
│  • Two-dimensional data (table-like)                             │
│  • Card grids with consistent rows                                │
│  • Page-level layouts (header/sidebar/main/footer)               │
│  • Gallery layouts                                                │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐               │  │
│  │ │ Cell 1 │ │ Cell 2 │ │ Cell 3 │ │ Cell 4 │  ← 4 columns    │  │
│  │ └────────┘ └────────┘ └────────┘ └────────┘               │  │
│  │ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐               │  │
│  │ │ Cell 5 │ │ Cell 6 │ │ Cell 7 │ │ Cell 8 │  ← 2 rows      │  │
│  │ └────────┘ └────────┘ └────────┘ └────────┘               │  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                                                     │
│  KEY DIFFERENCE:                                                  │
│  Flexbox = 1 dimension (row OR column)                           │
│  Grid = 2 dimensions (rows AND columns simultaneously)            │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Grid Template Syntax

```css
/* 3 equal columns */
.grid { grid-template-columns: 1fr 1fr 1fr; }

/* 2 columns: sidebar 250px, main fills rest */
.grid { grid-template-columns: 250px 1fr; }

/* 3 columns: 1fr 2fr 1fr (asymmetric) */
.grid { grid-template-columns: 1fr 2fr 1fr; }

/* Repeat 4 columns */
.grid { grid-template-columns: repeat(4, 1fr); }

/* Auto-fit: responsive columns */
.grid { grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); }

/* Rows: explicit row sizing */
.grid {
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: auto 1fr auto; /* Header, main, footer */
}
```

### Grid Gap

```css
/* Gap between rows and columns */
.grid { gap: 20px; }

/* Different row/column gaps */
.grid { row-gap: 20px; column-gap: 10px; }

/* Shorthand: row-gap column-gap */
.grid { gap: 20px 10px; }
```

---

## 4. Grid Areas — Page Layouts Made Easy

### Named Areas Syntax

```css
/* Define layout with named areas */
.page {
  display: grid;
  grid-template-areas:
    "header  header  header"
    "sidebar content content"
    "footer  footer  footer";
  grid-template-rows: auto 1fr auto;
  grid-template-columns: 200px 1fr;
  min-height: 100vh;
}

.header { grid-area: header; }
.sidebar { grid-area: sidebar; }
.content { grid-area: content; }
.footer { grid-area: footer; }
```

### Visual Result

```
┌─────────────────────────────────────────────────────────────────────┐
│                    GRID AREAS VISUALIZATION                        │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  "header  header  header"                                          │
│  ┌─────────┬─────────┬─────────┐                                  │
│  │         │         │         │  ← header spans all 3 columns     │
│  └─────────┴─────────┴─────────┘                                  │
│                                                                     │
│  "sidebar content content"                                         │
│  ┌─────────┬─────────┬─────────┐                                  │
│  │         │         │         │                                   │
│  │ sidebar │ content │ content │  ← sidebar 1 column, content 2   │
│  │         │         │         │                                   │
│  └─────────┴─────────┴─────────┘                                  │
│                                                                     │
│  "footer  footer  footer"                                           │
│  ┌─────────┬─────────┬─────────┐                                  │
│  │         │         │         │  ← footer spans all 3 columns      │
│  └─────────┴─────────┴─────────┘                                  │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 5. Flexbox vs Grid — When to Use What

```
┌─────────────────────────────────────────────────────────────────────┐
│                    FLEXBOX VS GRID DECISION                        │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│                          ┌─────────────┐                           │
│                          │ Need to     │                           │
│                          │ decide?    │                           │
│                          └──────┬──────┘                           │
│                                 │                                  │
│              ┌──────────────────┴──────────────────┐             │
│              ▼                                      ▼             │
│     ┌────────────────┐                   ┌────────────────┐       │
│     │ 1 DIMENSIONAL   │                   │ 2 DIMENSIONAL   │       │
│     │ (row OR column) │                   │ (rows AND cols)│       │
│     └────────┬───────┘                   └────────┬───────┘       │
│              │                                      │               │
│              ▼                                      ▼               │
│     ┌────────────────┐                   ┌────────────────┐       │
│     │ Navigation     │                   │ Page Layout    │       │
│     │ Card wrapping  │                   │ Gallery grid  │       │
│     │ Center content│                   │ Dashboard     │       │
│     │ Form fields    │                   │ Table data    │       │
│     └────────────────┘                   └────────────────┘       │
│                                                                     │
│  REAL-WORLD RULE:                                                  │
│  If you're debating flex vs grid:                                 │
│  • Can items wrap into new rows? Use flexbox                      │
│  • Do you need precise control over rows AND columns? Use grid    │
│                                                                     │
│  CAN YOU USE BOTH? YES!                                            │
│  Grid for page layout, flexbox for component internals            │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 📋 Quick Reference Card

| Property | Flexbox | Grid |
|----------|---------|------|
| `display` | `flex` | `grid` |
| Direction | `flex-direction` | N/A (items flow auto) |
| Main axis | `justify-content` | N/A |
| Cross axis | `align-items` | `align-items` (if single row) |
| Gap | `gap` | `gap` |
| Template | N/A | `grid-template-columns/rows` |
| Areas | N/A | `grid-template-areas` |

---

## ✅ What TO Do

1. **Use flexbox for component-level layouts** — Navbars, card groups, form fields
2. **Use grid for page-level layouts** — Header/sidebar/main/footer
3. **Use `gap` instead of margins** — Cleaner, only affects space between items
4. **Use `flex-wrap: wrap` for responsive cards** — Flex handles the wrap
5. **Use `minmax()` with auto-fit for responsive grids** — No media queries needed

## ❌ What NOT To Do

1. **Don't use float for layouts** — Flexbox/Grid replaced it completely
2. **Don't use table for layout** — Semantic tables only for data
3. **Don't set `flex-basis` to `auto` when you want fixed sizes** — Remember `flex: 0 0 200px` for fixed
4. **Don't use flexbox when you need row AND column control** — Use grid
5. **Don't forget `align-items` defaults to stretch** — Items will stretch to match tallest sibling

## 🏢 Workplace Wisdom

> "If you find yourself using `float` in 2024, stop. Flexbox handles anything float did, cleaner. Grid handles anything float couldn't."
>
> "Flexbox is for alignment (making things line up nicely). Grid is for layout (defining the structure of space). Most components need flexbox. Most pages need grid."
