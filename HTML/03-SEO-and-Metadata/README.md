# 🔍 03 — SEO and Metadata

> *"If your page exists but Google can't find it, does it really exist?"*

---

## 📌 What Is SEO Metadata?

Every webpage has two audiences: **humans** and **machines**.

Humans see the content. Machines (Google crawlers, social media scrapers, screen readers) read the **metadata** — the invisible instructions about what the page is, who made it, and how to display it.

Metadata lives in `<head>`, above the visible content. It never shows on the page — but it's always working behind the scenes.

---

## 🏗️ Where Metadata Lives

```
┌─────────────────────────────────────────────────────────────┐
│  <html lang="en">                                           │
│    <head>                                                   │
│         │                                                   │
│         ├── <title>My Page Title</title>         ← Browser  │
│         │                                       tab + SEO   │
│         ├── <meta charset="UTF-8">            ← Encoding   │
│         │                                                   │
│         ├── <meta name="description"           ← SEO desc  │
│         │     content="Page summary for Google">           │
│         │                                                   │
│         └── <link rel="canonical">             ← SEO dedup  │
│                                                             │
│    <body>  ← VISIBLE CONTENT starts here                   │
│    ...                                                      │
└─────────────────────────────────────────────────────────────┘
```

---

## 📋 Metadata Elements Reference

### Document-Level Metadata

| Element | Purpose |
|---------|---------|
| `<title>` | Browser tab title + Google search title (max 60 chars) |
| `<meta charset>` | Character encoding (always UTF-8) |
| `<meta name="viewport">` | Responsive behavior for mobile |
| `<link rel="icon">` | Browser tab favicon |

### SEO Metadata

| Element | Purpose |
|---------|---------|
| `<meta name="description">` | Google search snippet (150-160 chars) |
| `<meta name="author">` | Author name |
| `<meta name="keywords">` | ~~Historically misused, mostly ignored by Google~~ |
| `<link rel="canonical">` | Prevents duplicate content penalties |

### Open Graph (Social Media)

| Element | Purpose |
|---------|---------|
| `<meta property="og:title">` | Title when shared on Facebook/LinkedIn |
| `<meta property="og:description">` | Description in social post |
| `<meta property="og:image">` | Thumbnail image (1200×630px) |
| `<meta property="og:url">` | Canonical URL for the content |

### Twitter (X)

| Element | Purpose |
|---------|---------|
| `<meta name="twitter:card">` | Card type: `summary`, `summary_large_image` |
| `<meta name="twitter:title">` | Title for Twitter preview |
| `<meta name="twitter:description">` | Description for Twitter preview |
| `<meta name="twitter:image">` | Image for Twitter preview |

---

## ✅ What TO Do

### DO: Write a Compelling `<title>`
```html
<!-- Good: Clear, concise, contains keyword -->
<title>How to Brew Coffee: A Barista's Guide | CoffeeSite</title>

<!-- Bad: Default, unreadable, no keyword -->
<title>Home</title>
```

### DO: Write a Descriptive `<meta description>`
```html
<!-- Good: Actionable, contains keyword, 150-160 chars -->
<meta name="description" content="Learn how to brew the perfect cup of coffee at home. 
Includes pour-over, French press, and espresso techniques for beginners.">
```

### DO: Use Open Graph for Social Sharing
```html
<meta property="og:title" content="How to Brew Coffee">
<meta property="og:description" content="Master the art of coffee brewing at home.">
<meta property="og:image" content="https://example.com/coffee-share.jpg">
<meta property="og:url" content="https://example.com/brewing-guide">
```

### DO: Set Viewport for Mobile
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

---

## 🚫 What NOT to Do

### DON'T: Ignore Duplicate Content with Canonical
```html
<!-- Bad: Google sees /?page=1 and /?page=2 as separate pages -->
<!-- Good: Tell Google the canonical version -->
<link rel="canonical" href="https://example.com/page">
```

### DON'T: Use the Same Title on Every Page
```html
<!-- Bad: Every page says the same thing — Google penalizes -->
<title>Home</title>
<title>Home</title>
<title>Home</title>
```

### DON'T: Omit the viewport meta tag
```html
<!-- Without this, mobile browsers render at desktop width -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

### DON'T: Use `keywords` Meta Tag for SEO
```html
<!-- Google ignores this. It was abused by spammers. -->
<meta name="keywords" content="coffee, espresso, brew">
```

---

## 🎯 Why This Matters

### In the Workplace: Search Rankings
Google reads your metadata to understand what your page is about. A missing `<title>` means Google invents one (often just URL text). A missing `og:image` means your social shares have no thumbnail — **dramatically lower click-through rates**.

### In the Workplace: Social Sharing
When someone shares a link on LinkedIn or Twitter, the platform scrapes your Open Graph metadata. If you don't have it, you get a broken, ugly preview — or no preview at all. That affects brand perception and click rates.

---

## 🧠 Mental Model: The Library Card

| Library Card | Web Metadata |
|--------------|--------------|
| Card title | `<title>` |
| Brief description | `<meta description>` |
| Author name | `<meta name="author">` |
| Subject tags | Semantic HTML + Open Graph |
| Cover image | `og:image` |

When you search a library catalog, the card tells you whether the book is worth reading. Metadata does the same for web pages.

---

## 📚 Technical Glossary

- **SEO (Search Engine Optimization):** The practice of making your content findable by search engines.
- **Open Graph:** A protocol that controls how URLs are displayed when shared on social media.
- **Canonical URL:** The "official" version of a page, used to prevent duplicate content issues.
- **Crawler/Bot:** Automated software that reads pages and indexes them for search engines.
- **Viewport:** The visible area of a webpage. Mobile browsers simulate a desktop width without the viewport meta tag.

---

[⬅️ Back to Parent](../README.md)
