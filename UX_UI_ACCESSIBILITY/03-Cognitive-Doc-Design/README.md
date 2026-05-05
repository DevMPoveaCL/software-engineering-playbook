# 03 — Cognitive Doc Design

[⬅️ Back to Parent](../README.md)

---

## 🎯 What This Folder Covers

This folder teaches you how to design **documentation that people can actually use** — not just read, but understand, remember, and apply. Think of it as the science of making complex information digestible.

---

## 1. Progressive Disclosure — One Layer at a Time

### The Iceberg Principle

```
┌─────────────────────────────────────────────────────────────────────┐
│                    PROGRESSIVE DISCLOSURE                            │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│     ┌─────────────────┐                                             │
│     │   WHAT USERS    │  ← Visible: Just enough to orient           │
│     │   SEE FIRST     │      and decide to proceed                   │
│     └────────┬────────┘                                             │
│              │                                                      │
│              ▼                                                      │
│     ┌─────────────────┐                                             │
│     │  "Learn More"   │  ← Click/tap to expand                      │
│     │  "Show Details" │      Second layer revealed                  │
│     │  "Advanced ▼"  │                                             │
│     └────────┬────────┘                                             │
│              │                                                      │
│              ▼                                                      │
│     ┌─────────────────┐                                             │
│     │   FULL DOCS     │  ← Third layer: Complete reference          │
│     │   (Deep dive)   │      For those who need everything          │
│     └─────────────────┘                                             │
│                                                                     │
│  PRINCIPLE: Don't dump everything at once                          │
│  Show surface → Invitation to explore → Complete detail           │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Implementation Patterns

```html
<!-- Pattern 1: Accordion -->
<details>
  <summary>What is WCAG compliance?</summary>
  <p>Web Content Accessibility Guidelines are the international
     standard for web accessibility...</p>
</details>

<!-- Pattern 2: Tabbed interface for related content -->
<ul role="tablist">
  <button role="tab" aria-selected="true">Overview</button>
  <button role="tab">Technical Details</button>
  <button role="tab">Examples</button>
</ul>

<!-- Pattern 3: Progressive reveal on scroll -->
<div class="on-scroll-reveal">
  <!-- Content appears as user scrolls -->
</div>
```

**Workplace rule:** If a new employee can't understand your docs in 5 minutes, you've already lost them. Progressive disclosure respects their time and current knowledge level.

---

## 2. Chunking — The Working Memory Solution

### Miller's Law: 7±2 Items

```
┌─────────────────────────────────────────────────────────────────────┐
│                    WORKING MEMORY LIMITATIONS                       │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  Working memory holds approximately 7 items (±2)                   │
│                                                                     │
│  TOO MANY ITEMS (15 items, 3 groups of 5):                        │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │ Apples Oranges Bananas Grapes Mangoes │                    │  │
│  │ Carrots Celery Lettuce Spinach Kale     │                    │  │
│  │ Beef Pork Chicken Lamb Fish             │                    │  │
│  └─────────────────────────────────────────────────────────────┘  │
│  Harder to remember: 3 groups is fine, but items within groups    │
│  also compete for slots                                           │
│                                                                     │
│  CHUNKED (5 items, 3 meaningful groups):                          │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │ FRUITS:     Apples, Oranges, Grapes                        │  │
│  │ VEGGIES:    Carrots, Celery, Spinach                        │  │
│  │ MEATS:      Beef, Chicken, Fish                            │  │
│  └─────────────────────────────────────────────────────────────┘  │
│  EASIER: 3 meaningful groups × 3 items = clear mental model      │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Document Organization by Chunk Size

| Chunk Type | Size | When to Use |
|------------|------|-------------|
| Bullet point | 1 sentence | Quick facts, single ideas |
| Section | 3-7 points | Related items, features |
| Page | 1 main concept | Deep explanations |
| Document | 1 complete topic | Technical references |

### The "Scan-Ability" Test

**Real-world analogy:** Users read documentation like drivers read road signs — they glance, extract key information, and move on. Documentation must work at a glance.

```
┌─────────────────────────────────────────────────────────────────────┐
│                    BEFORE: Dense Wall of Text                      │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  To configure the authentication system you will need to first     │
│  navigate to the admin panel by clicking the settings icon in      │
│  the top right corner. Then select the security tab from the      │
│  navigation menu. On this page you can configure your OAuth2      │
│  settings including the client ID and secret which you will       │
│  need to obtain from your identity provider.                       │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│                    AFTER: Scannable Chunks                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ## Configure Authentication                                       │
│                                                                     │
│  **Step 1:** Navigate to Admin Panel                               │
│  Settings icon (top-right) → Security tab                         │
│                                                                     │
│  **Step 2:** Get OAuth2 Credentials                                │
│  Obtain from your identity provider:                               │
│  • Client ID                                                       │
│  • Client Secret                                                   │
│                                                                     │
│  **Step 3:** Enter Credentials                                      │
│  Paste values into the OAuth2 settings form                        │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 3. Recognition Over Recall — Reduce Mental Load

### The Principle

```
┌─────────────────────────────────────────────────────────────────────┐
│                    RECOGNITION VS RECALL                           │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  RECALL (hard — requires searching memory)                        │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │  "What was that keyboard shortcut for undo?"               │  │
│  │  "How do I spell that CSS property again?"                  │  │
│  │  "Was it flex-direction or flex-direction?"                  │  │
│  └─────────────────────────────────────────────────────────────┘  │
│  User must retrieve info from memory                              │
│                                                                     │
│  RECOGNITION (easy — option is visible)                          │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │  [Undo] [Redo] [Cut] [Copy] [Paste]  ← Options visible     │  │
│  │  flex-direction: row;                ← Recognize correct   │  │
│  │  auto-fill | auto-fit | ...            ← Select from list   │  │
│  └─────────────────────────────────────────────────────────────┘  │
│  User just recognizes the correct option                          │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Design Patterns That Enable Recognition

```html
<!-- Pattern 1: Code completion / autocomplete -->
<!-- User doesn't need to remember exact syntax -->
<input list="css-properties" placeholder="Type a property...">
<datalist id="css-properties">
  <option value="display">
  <option value="flex-direction">
  <option value="justify-content">
  <option value="align-items">
</datalist>

<!-- Pattern 2: Side-by-side before/after examples -->
<div class="example-grid">
  <pre><code>/* BAD */
display: flex;
justify-content: center;</code></pre>
  <pre><code>/* GOOD */
display: flex;
justify-content: center;
align-items: center;</code></pre>
</div>

<!-- Pattern 3: Quick reference cards (always visible) -->
<aside class="quick-ref">
  <h3>Quick Reference</h3>
  <table>
    <tr><td>Flex center:</td><td>justify + align = center</td></tr>
    <tr><td>Gap syntax:</td><td>gap: row col;</td></tr>
  </table>
</aside>
```

---

## 4. Signposting — Orientation Cues

### Navigation Breadcrumbs

```
┌─────────────────────────────────────────────────────────────────────┐
│                    SIGNPOSTING ELEMENTS                            │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  BREADCRUMBS (where am I in the structure)                        │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │ Docs > Authentication > OAuth2 > Configuration            │  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                                                     │
│  IN-PAGE NAVIGATION (what's in this page)                        │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │ Contents: [Overview] [Setup] [Troubleshooting] [API Ref]   │  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                                                     │
│  PREV/NEXT LINKS (where to go next)                              │
│  ┌───────────────────┐          ┌───────────────────┐           │
│  │ ← Previous:       │          │  Next:             │           │
│  │    Basic Setup    │          │  Troubleshooting  →│           │
│  └───────────────────┘          └───────────────────┘           │
│                                                                     │
│  SECTION HEADERS (what this section is about)                     │
│  ## Configuring OAuth2 Authentication ───────────────────────     │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Visual Hierarchy Cues

```markdown
# Page Title (H1) — One per page

## Major Section (H2) — 2-4 per page

### Subsection (H3) — As needed

**Bold for key terms** — Don't bury the lede

> Blockquotes for important notes that users might skip
```

---

## 5. Documentation Landing Pages

### The "Hub and Spoke" Model

```
┌─────────────────────────────────────────────────────────────────────┐
│                    LANDING PAGE STRUCTURE                          │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│                        ┌─────────────┐                             │
│                        │  LANDING    │                             │
│                        │   PAGE      │                             │
│                        │  (Hub)      │                             │
│                        └──────┬──────┘                             │
│                               │                                    │
│         ┌─────────────────────┼─────────────────────┐             │
│         │                     │                     │             │
│         ▼                     ▼                     ▼             │
│  ┌─────────────┐      ┌─────────────┐      ┌─────────────┐    │
│  │  Getting    │      │   API       │      │  Examples   │    │
│  │  Started    │ ───► │   Reference │ ───► │  & Tutorials│    │
│  │  (Spoke)    │      │   (Spoke)   │      │   (Spoke)   │    │
│  └─────────────┘      └─────────────┘      └─────────────┘    │
│         │                     │                     │             │
│         ▼                     ▼                     ▼             │
│  ┌─────────────┐      ┌─────────────┐      ┌─────────────┐    │
│  │ Installation│      │  Endpoints  │      │  Demo App   │    │
│  │ Basic Auth  │      │  Parameters  │      │  Code Sample│    │
│  └─────────────┘      └─────────────┘      └─────────────┘    │
│                                                                     │
│  Each spoke links to:                                              │
│  • Other related spokes                                          │
│  • The hub (for orientation)                                     │
│  • Relevant reference docs                                       │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Landing Page Template

```markdown
# [Product Name] Documentation

[One sentence explaining what this product does]

## Quick Links
[Get Started] [API Reference] [Tutorials] [Examples]

## Getting Started
[Step-by-step guide — 5 minutes to first result]

## Popular Topics
[Grid of linked topic cards with icons and 1-line descriptions]

## Troubleshooting
[Link to common issues and solutions]

---
[Previous: Other Documentation] [Next: Related Topic]
```

---

## 📋 Quick Reference Card

| Principle | Remember |
|-----------|----------|
| Progressive disclosure | Show surface first, reveal on demand |
| Chunking | 3-7 items per group, meaningful categorization |
| Recognition over recall | Make options visible, don't make users remember |
| Signposting | Breadcrumbs, in-page nav, prev/next links |
| Scanability | Bold, headers, lists, white space |

---

## ✅ What TO Do

1. **Use accordion/details elements** — Hide complexity until needed
2. **Break long content into sections** — 3-7 bullet points max per list
3. **Add table of contents** — For docs longer than 3 sections
4. **Show code before and after** — Recognition over typing from memory
5. **Include "Quick Reference" boxes** — Always-visible reminders

## ❌ What NOT To Do

1. **Don't dump everything on one page** — Cognitive overload causes users to give up
2. **Don't use walls of text** — If you can't scan it in 5 seconds, rewrite it
3. **Don't skip headings** — They are navigation landmarks for screen readers
4. **Don't use jargon without definition** — The first mention should define unfamiliar terms
5. **Don't forget prev/next navigation** — Users should never be stuck wondering "what next?"

## 🏢 Workplace Wisdom

> "The best documentation doesn't tell users everything. It tells them exactly what they need to know right now, and makes the rest discoverable."
>
> "If you have to explain your documentation structure, the structure is wrong. Users should never feel lost."
