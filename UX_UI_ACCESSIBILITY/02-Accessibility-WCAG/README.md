# 02 — Accessibility (WCAG)

---

## 🎯 What This Folder Covers

This folder teaches **web accessibility** — the legal and ethical requirements for building interfaces usable by people with disabilities. Think of it as the building code for inclusive digital experiences.

---

## 1. Why Accessibility Matters

### The Statistics

```
┌─────────────────────────────────────────────────────────────────────┐
│                    WHO NEEDS ACCESSIBILITY?                        │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ~15% of world population (1.3 billion people) have a disability  │
│                                                                     │
│  ┌───────────────────────────────────────────────────────────────┐ │
│  │  Type              │  Affected           │  Consideration    │ │
│  │  ───────────────────────────────────────────────────────────  │ │
│  │  Visual            │  253 million        │  Screen readers,  │ │
│  │                    │  (low vision,       │  color blindness  │ │
│  │                    │   blindness)        │                   │ │
│  │  Motor             │  750 million        │  Keyboard only,   │ │
│  │                    │                     │  limited mobility │ │
│  │  Cognitive         │  ~400 million        │  Clear content,   │ │
│  │                    │  (learning,         │  predictable nav  │ │
│  │                    │   ADHD, dyslexia)    │                   │ │
│  │  Auditory          │  430 million         │  Captions,        │ │
│  │                    │                     │  transcripts      │ │
│  └───────────────────────────────────────────────────────────────┘ │
│                                                                     │
│  Additionally: Situational disabilities                            │
│  • New parent holding a baby (one hand)                            │
│  • Broken arm / arm in a cast                                      │
│  • Direct sunlight making screen unreadable                       │
│  • Noisy environment (can't hear audio)                            │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### The Legal Landscape

| Regulation | Region | Key Requirement |
|------------|--------|----------------|
| **WCAG 2.2** | International (ISO standard) | Global benchmark |
| **ADA** | USA | Website = Place of public accommodation |
| **Section 508** | USA (federal) | Federal websites and tech |
| **European Accessibility Act** | EU (2025 deadline) | Digital services in EU |
| **AODA** | Canada (Ontario) | Private sector requirements |

**Workplace warning:** Accessibility lawsuits increased 300% from 2018-2023. Domino's, Netflix, and Target have all faced major lawsuits. Ignorance is not a legal defense.

---

## 2. WCAG 2.2 Success Criteria — The Four Principles

### POUR: The Accessibility Framework

```
┌─────────────────────────────────────────────────────────────────────┐
│                    WCAG 4 PRINCIPLES (POUR)                        │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ┌─────────────────┐                                               │
│  │  PERCEIVABLE    │  Information must be presentable to users    │
│  │                 │  in ways they can perceive                   │
│  │  • Alt text     │  (text alternatives, captions, contrast)    │
│  │  • Captions     │                                              │
│  │  • Resizable    │                                              │
│  └────────┬────────┘                                               │
│           │                                                       │
│  ┌────────┴────────┐                                               │
│  │  OPERABLE       │  User interface must be operable             │
│  │                 │                                              │
│  │  • Keyboard     │  (keyboard accessible, no seizure risk,     │
│  │  • No seizure   │   user control)                              │
│  │  • User control │                                              │
│  └────────┬────────┘                                               │
│           │                                                       │
│  ┌────────┴────────┐                                               │
│  │  UNDERSTANDABLE │  Information and UI must be understandable   │
│  │                 │                                              │
│  │  • Readable    │  (readable, predictable, input assistance)   │
│  │  • Predictable │                                              │
│  │  • Error help  │                                              │
│  └────────┬────────┘                                               │
│           │                                                       │
│  ┌────────┴────────┐                                               │
│  │  ROBUST         │  Content must be robust enough to be         │
│  │                 │  reliably interpreted by various UAs        │
│  │  • Standards    │  (valid HTML, ARIA when needed)             │
│  │  • Compatible   │                                              │
│  └─────────────────┘                                               │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 3. Level A — Minimum Requirements

### 1.1.1 Non-Text Content — Alt Text

```html
<!-- ❌ WRONG — No alt text -->
<img src="chart.png">

<!-- ✅ CORRECT — Descriptive alt -->
<img src="chart.png" alt="Bar chart showing Q3 sales: Product A
     generated $50k, Product B generated $75k, Product C generated $30k">

<!-- ✅ DECORATIVE — Empty alt (ignored by screen readers) -->
<img src="decorative-divider.png" alt="">

<!-- ✅ ARIA — For complex images, consider long description -->
<img src="complex-chart.png"
     alt="Sales trend chart"
     aria-describedby="chart-description">
<p id="chart-description">Detailed breakdown: Product A started
at $40k in January, peaked at $60k in March...</p>
```

### 2.1.1 Keyboard — All Functionality

```
┌─────────────────────────────────────────────────────────────────────┐
│                    KEYBOARD ACCESSIBILITY                           │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  Every interactive element MUST be reachable via keyboard:        │
│                                                                     │
│  TAB key ──────► Move forward through focusable elements          │
│  SHIFT+TAB ────► Move backward                                    │
│  ENTER ────────► Activate links, buttons, controls                │
│  SPACE ────────► Activate buttons, toggle checkboxes             │
│  ARROW keys ───► Navigate within radio groups, menus, sliders     │
│  ESC ──────────► Close dialogs, cancel actions                    │
│                                                                     │
│  EXPECTED BEHAVIOR:                                                │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │ ✓ Focus indicator visible (never use outline: none)        │  │
│  │ ✓ Logical focus order (top-to-bottom, left-to-right)      │  │
│  │ ✓ No keyboard traps (no endless tab loops)                 │  │
│  │ ✓ All actions available via keyboard                       │  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### 4.1.2 Name, Role, Value — Form Labels

```html
<!-- ❌ WRONG — No labels, placeholder as substitute -->
<input type="email" placeholder="your@email.com">

<!-- ✅ CORRECT — Proper labels -->
<label for="email">Email address</label>
<input type="email" id="email" name="email">

<!-- ✅ CORRECT — Visible label, aria-label for icon buttons -->
<button aria-label="Close dialog">
  <svg>❌</svg>
</button>
```

---

## 4. Level AA — Standard Compliance (Most Sites)

### 1.4.3 Contrast (Minimum) — 4.5:1

```css
/* ❌ WRONG — Fails WCAG AA (2.4:1 contrast ratio) */
.high-contrast-fail {
  color: #777777;  /* Light gray on white */
  background: #ffffff;
}

/* ✅ CORRECT — Passes WCAG AA (4.5:1 minimum) */
.acceptable-contrast {
  color: #767676;  /* Darker gray on white */
  background: #ffffff;
}

/* WCAG AAA for large text (7:1) */
.aaa-contrast {
  color: #595959;
  background: #ffffff;
}
```

**Quick test tool:** https://webaim.org/resources/contrastchecker/

### 2.4.6 Headings and Labels

```
┌─────────────────────────────────────────────────────────────────────┐
│                    HEADING HIERARCHY RULES                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  Structure requirement:                                              │
│  • One <h1> per page (the main title)                              │
│  • Headings must be nested logically (no Skipping levels)          │
│  • h1 → h2 → h3 (not h1 → h3 → h1)                               │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │ <h1>Page Title</h1>                                         │  │
│  │                                                             │  │
│  │   <h2>Section One</h2>  ✓                                   │  │
│  │     <h3>Subsection A</h3>  ✓                              │  │
│  │     <h3>Subsection B</h3>  ✓                              │  │
│  │                                                             │  │
│  │   <h2>Section Two</h2>  ✓                                  │  │
│  │                                                             │  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                                                     │
│  SKIP: h1 → h3 (without h2) = WRONG                               │
│  Screen reader users navigate by headings — skip levels confuse    │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### 3.2.2 On Input — No Unexpected Context Changes

```html
<!-- ❌ WRONG — Selection changes context -->
<select onchange="form.submit()"> <!-- Unexpected form submit -->

<!-- ✅ CORRECT — User-initiated with clear button -->
<select id="country">
  <option>Select country</option>
</select>
<button type="submit">Apply filter</button>

<!-- ✅ CORRECT — Autocomplete with consent -->
<input type="email" autocomplete="email">
```

---

## 5. Level AAA — Enhanced Requirements

### 2.2.2 Pause, Stop, Hide — Auto-Playing Media

```css
/* CSS Solution for animation control */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

### Real-World: Video Player Implementation

```html
<!-- ✅ WCAG AA — User controls with pause -->
<video controls poster="poster.jpg">
  <source src="video.mp4" type="video/mp4">
  <track kind="captions" src="captions.vtt" srclang="en" label="English">
  <!-- Captions for deaf/hard-of-hearing -->
</video>

<!-- Key attributes for accessibility: -->
<!-- • controls — shows play/pause/volume (mandatory) -->
<!-- • autoplay muted loop playsinline — video best practices -->
<!-- • <track> — captions/subtitles for video -->
<!-- • poster — fallback image -->
```

---

## 6. Accessibility Testing Checklist

### Manual Testing Sequence

```
┌─────────────────────────────────────────────────────────────────────┐
│                    MINIMUM ACCESSIBILITY TEST                       │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  1. KEYBOARD ONLY TEST                                               │
│     □ Can you navigate the entire page using only Tab/Shift+Tab?    │
│     □ Can you activate all buttons with Enter?                      │
│     □ Is the focus indicator clearly visible?                      │
│     □ Can you close dialogs with ESC?                              │
│                                                                     │
│  2. SCREEN READER TEST                                              │
│     □ Use NVDA (Windows) or VoiceOver (Mac)                         │
│     □ Is all content announced?                                    │
│     □ Do images have alt text?                                     │
│     □ Are form fields labeled?                                     │
│     □ Does reading order make sense?                               │
│                                                                     │
│  3. VISUAL TESTING                                                  │
│     □ Zoom to 200% — does content still work?                      │
│     □ Windows High Contrast mode — visible?                        │
│     □ Color contrast checker — passes 4.5:1?                       │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Automated Tools

| Tool | Purpose | How |
|------|---------|-----|
| **axe DevTools** | Browser extension | Find WCAG violations |
| **WAVE** | WebAIM tool | Visual feedback |
| **Lighthouse** | Chrome DevTools | Quick audit |
| **NVDA** | Screen reader | Windows accessibility |

---

## 📋 Quick Reference Card

| Criterion | Remember |
|-----------|----------|
| Alt text | Describe content, not "image of" |
| Keyboard | Every action must be keyboard-accessible |
| Contrast | 4.5:1 minimum (body), 3:1 (large text/UI) |
| Labels | Every input needs a visible or aria label |
| Headings | One h1, nested hierarchy, no skips |
| Focus | Never use `outline: none` without replacement |

---

## ✅ What TO Do

1. **Test with keyboard only** — Turn off your mouse for an hour
2. **Add alt text to ALL images** — Descriptive, not "image of X"
3. **Ensure 4.5:1 contrast ratio** — Use WebAIM contrast checker
4. **Use semantic HTML** — `<button>`, `<nav>`, `<main>`, `<article>`
5. **Provide skip links** — "Skip to main content" for keyboard users

## ❌ What NOT To Do

1. **Don't use `outline: none`** — Unless providing equally visible focus indicator
2. **Don't use placeholders as labels** — They disappear when typing, fail contrast
3. **Don't use color alone to convey meaning** — Icons/text required
4. **Don't auto-play media** — User must control playback
5. **Don't use accessibility tags as decoration** — `role="presentation"` should only be used when truly decorative

## 🏢 Workplace Wisdom

> "Accessibility is not a feature. It's a quality of the product baseline. Like security, it's far cheaper to build it in from the start than to retrofit it later."
>
> "If you have to choose ONE thing to fix: Make your page keyboard-navigable. That's the single highest-impact accessibility improvement."

[⬅️ Previous: 01 Visual Design Rules](../01-Visual-Design-Rules/README.md) | [⬅️ Back to Parent](../README.md) | [➡️ Next: 03 Cognitive Doc Design](../03-Cognitive-Doc-Design/README.md)
