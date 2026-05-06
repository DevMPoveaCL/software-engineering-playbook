# 01 — Visual Design Rules

---

## 🎯 What This Folder Covers

This folder teaches the **foundational rules of visual design** — the laws that govern how humans perceive, process, and remember visual information. Think of it as learning the physics of human sight.

---

## 1. Visual Hierarchy — What the Eye Sees First

### The F-Pattern Reading Research

```
┌─────────────────────────────────────────────────────────────────────┐
│                    HOW PEOPLE READ WEBSITES                        │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  Users scan in an F or E pattern:                                 │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────┐       │
│  │ ████████████████  HEADER / LOGO                         │ ← Top    │
│  │ ████████████  (strong horizontal reading)               │   bar   │
│  │ ████████                                           │       │
│  ├─────────────────────────────────────────────────────────┤       │
│  │ First line of content                                  │       │
│  │ ████████                                               │       │
│  │                                                       │       │
│  │ Left-aligned content gets read more often than         │       │
│  │ center-aligned. Users scan LEFT edge first.            │       │
│  │                                                       │       │
│  │ More content  ───► Less read                          │       │
│  │ ████████████████████                                  │       │
│  │ ████████                                              │       │
│  └─────────────────────────────────────────────────────────┘       │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

**Design implication:** Put your most important content **top-left**. The F-pattern means users will scan that area first.

### Z-Pattern for Landing Pages

```
┌─────────────────────────────────────────────────────────────────────┐
│                    Z-PATTERN FOR LANDING PAGES                     │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  Logo/Brand ──────────────── Hero CTA ─────────── Language/Menu    │
│         ╲                                             │             │
│          ╲                                           │             │
│           ╲    Secondary Content                     │             │
│            ╲                                         │             │
│             ╲─────── Bottom CTA ──────────────────  │             │
│                                                                     │
│  Use Z-pattern for simple pages with ONE clear action               │
│  Use F-pattern for content-heavy pages                            │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 2. Color Theory — Practical Application

### The 60-30-10 Rule

```
┌─────────────────────────────────────────────────────────────────────┐
│                    COLOR DISTRIBUTION RULE                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   ┌───────────────────────────────────────────────────────────┐    │
│   │                    YOUR DESIGN SPACE                      │    │
│   │                                                           │    │
│   │  ████████████████████████████████  (60%)                  │    │
│   │           Dominant Color                                    │    │
│   │           (backgrounds, large areas)                      │    │
│   │                                                           │    │
│   │  ████████████████  (30%)                                  │    │
│   │      Secondary Color                                       │    │
│   │      (sections, cards, supporting elements)               │    │
│   │                                                           │    │
│   │  ████████  (10%)                                          │    │
│   │     Accent Color                                           │    │
│   │     (CTAs, alerts, links, key moments)                    │    │
│   │                                                           │    │
│   └───────────────────────────────────────────────────────────┘    │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

**Workplace rule:** The 10% accent is where your eye should go. If everything is colored, nothing stands out.

### Color Contrast — WCAG Compliance

```
┌─────────────────────────────────────────────────────────────────────┐
│                    MINIMUM CONTRAST RATIOS                           │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  WCAG Level    │ Normal Text  │ Large Text (18pt+) │ UI Components │
│  ────────────────────────────────────────────────────────────────    │
│  AA (minimum) │   4.5 : 1    │       3 : 1         │    3 : 1      │
│  AAA (best)   │   7 : 1      │       4.5 : 1       │    4.5 : 1   │
│                                                                     │
│  Large text = 18pt regular OR 14pt bold                            │
│                                                                     │
│  Use WebAIM Contrast Checker: https://webaim.org/resources/contrastchecker/
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

**Real-world analogy:** Contrast is like sound volume. If your text is barely audible (low contrast), people with vision differences or in sunlight literally cannot hear it.

---

## 3. Typography — Readability Rules

### The 100% Rule for Line Height

```
┌─────────────────────────────────────────────────────────────────────┐
│                    LINE HEIGHT REQUIREMENTS                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   Minimum line-height:  1.5 × font-size  (for body text)         │
│   Recommended:           1.6 – 1.8 × font-size                     │
│                                                                     │
│   Too tight (1.2):    Too loose (2.5):                             │
│   ┌──────────────┐    ┌──────────────┐                             │
│   │ Line one     │    │ Line one     │                             │
│   │ Line two     │    │              │                             │
│   │ Line three   │    │ Line two     │                             │
│   └──────────────┘    │              │                             │
│                       │ Line three   │                             │
│                       └──────────────┘                             │
│                       Eye loses track between lines                │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Type Scale — The Modular Scale

```
┌─────────────────────────────────────────────────────────────────────┐
│                    TYPOGRAPHY SCALE (1.25 ratio)                    │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  Scale: 12 → 15 → 19 → 24 → 30 → 38 → 48 → 60                     │
│         │   │    │    │    │    │    │    │                        │
│         S   M    L    XL   XXL  XXX  XXXL                          │
│                                                                     │
│  Your font sizes should follow a ratio, not random numbers.        │
│  Common ratios: 1.25 (Major Third), 1.333 (Perfect Fourth),       │
│                 1.5 (Perfect Fifth)                                │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Character Limits

| Content Type | Characters Per Line (optimal) | Why |
|--------------|-------------------------------|-----|
| Body text | 50-75 characters | Working memory can hold ~7 items at once |
| Headlines | 6-8 words max | Breaks attention if too long |
| Data tables | As needed | Context-dependent |
| UI labels | 3 words max | Mobile requires brevity |

**The 75-character rule:** Each line should contain approximately 75 characters for comfortable reading. Lines that are too wide cause the eye to lose its place when returning to the next line.

---

## 4. Spacing — The Invisible Structure

### The 8-Point Grid

```
┌─────────────────────────────────────────────────────────────────────┐
│                    8-POINT GRID SYSTEM                              │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  All spacing values should be multiples of 8:                      │
│                                                                     │
│  4px  (half-unit, for tight spaces)                                │
│  8px  (base unit)                                                 │
│  16px (2 units)                                                    │
│  24px (3 units)                                                    │
│  32px (4 units)                                                    │
│  48px (6 units)                                                    │
│  64px (8 units)                                                    │
│  96px (12 units)                                                   │
│                                                                     │
│  Why 8? Because it divides evenly into common widths:             │
│  8, 16, 24, 32, 40, 48, 56, 64, 72, 80, 88, 96...                  │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### White Space — The Breathing Room

**Real-world analogy:** White space is like the silence between musical notes. Without silence, music becomes noise. Without white space, design becomes exhaustion.

```
WITH WHITE SPACE:                    WITHOUT WHITE SPACE:
┌─────────────────────┐              ┌─────────────────────┐
│                     │              │ContentContentContent│
│    Content Block   │              │ContentContentContent│
│                     │              │ContentContentContent│
│   ← Generous       │              │                     │
│   ← breathing room │              │ No visual rest     │
│                     │              │ ← Exhausting to read│
└─────────────────────┘              └─────────────────────┘
```

---

## 5. Visual Design Principles

### CRAP Principles (Contrast, Repetition, Alignment, Proximity)

```
┌─────────────────────────────────────────────────────────────────────┐
│                    THE CRAP PRINCIPLES                              │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  1. CONTRAST                                                       │
│     ┌─────────┐      ┌─────────┐                                   │
│     │ small   │ VS   │  LARGE  │   ← Different sizes = contrast    │
│     │ light   │      │  bold   │                                   │
│     └─────────┘      └─────────┘                                   │
│                                                                     │
│  2. REPETITION                                                      │
│     Consistent colors, fonts, spacing across all pages             │
│                                                                     │
│  3. ALIGNMENT                                                       │
│     ┌──────────────────────────────┐                               │
│     │ Left-aligned content        │   ← Invisible grid line        │
│     │ Stays on that edge          │                               │
│     └──────────────────────────────┘                               │
│                                                                     │
│  4. PROXIMITY                                                       │
│     Related items group together; unrelated items separate          │
│                                                                     │
│     ┌────────────┐   ┌────────────┐                               │
│     │ Title      │   │ Unrelated   │   ← Title + image = group    │
│     │ Image      │   │ Element     │   ← Different group          │
│     │ Description│   │             │                               │
│     └────────────┘   └────────────┘                               │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 📋 Quick Reference Card

| Principle | Remember |
|-----------|----------|
| Hierarchy | Eye goes to highest contrast, largest, most isolated element |
| Color 60-30-10 | 60% dominant, 30% secondary, 10% accent |
| Contrast | 4.5:1 minimum for body text (WCAG AA) |
| Typography | 1.5× line-height minimum, 50-75 characters per line |
| Spacing | 8-point grid, generous white space |
| Alignment | One strong alignment line per section |

---

## ✅ What TO Do

1. **Create visual hierarchy through contrast** — Size, weight, color, isolation
2. **Follow the 8-point grid** — Consistent spacing creates professional feel
3. **Limit to 2-3 fonts maximum** — More creates visual chaos
4. **Apply the 60-30-10 color rule** — Deliberate color distribution
5. **Use white space generously** — It helps the eye rest and focus

## ❌ What NOT To Do

1. **Don't use pure black on pure white** — Too high contrast causes eye strain. Use #333 on #FFF
2. **Don't cram content together** — "Breathing room" prevents cognitive overload
3. **Don't use more than 2-3 fonts** — Each font change creates visual "noise"
4. **Don't ignore the F-pattern** — If your important content isn't top-left, users miss it
5. **Don't rely on color alone for information** — 8% of men are colorblind

## 🏢 Workplace Wisdom

> "White space is not empty space. It's the pause that gives meaning to the note. Novice designers fill every pixel. Expert designers know what to leave alone."
>
> "If you have to explain your hierarchy, it doesn't exist. Good hierarchy is self-evident."

[⬅️ Back to Parent](../README.md) | [➡️ Next: 02 Accessibility (WCAG)](../02-Accessibility-WCAG/README.md)
