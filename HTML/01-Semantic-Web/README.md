# 🌐 01 — Semantic Web

> *"Use the right tag for the right job."*

---

## 📌 What Is Semantic HTML?

**Semantic HTML** means using HTML elements that *describe their own content* — not just visually, but *meaningfully*. It's the difference between a book written in all caps vs. one with proper headings, paragraphs, and chapters.

A `<div>` says: *"I'm a generic box."*
A `<nav>` says: *"I'm a navigation section."*

Machines (screen readers, search engines, browsers) understand the second one.

---

## 🏗️ The Document Structure Anatomy

Think of an HTML document as a **newspaper**:

```
┌─────────────────────────────────────────────────────────────┐
│  HEADER (The masthead with logo and publication name)     │
├─────────────────────────────────────────────────────────────┤
│  NAV (The table of contents / sections)                   │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  MAIN (The lead article — unique to THIS page)             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  ARTICLE #1                                          │   │
│  │  ┌───────────────────────────────────────────────┐  │   │
│  │  │  SECTION (A story within the article)         │  │   │
│  │  └───────────────────────────────────────────────┘  │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  ASIDE (Related ads, sidebars — tangential content)         │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│  FOOTER (Copyright, contact, legal links)                  │
└─────────────────────────────────────────────────────────────┘
```

---

## 📋 Semantic Elements Reference

| Tag | Real-World Analogy | Purpose |
|-----|-------------------|---------|
| `<header>` | Newspaper masthead | Page or section header (logo, title, intro) |
| `<nav>` | Table of contents | Navigation links |
| `<main>` | The front-page lead story | **Unique** primary content of the page |
| `<article>` | A standalone magazine article | Self-contained content (blog post, news item) |
| `<section>` | A chapter in a book | Thematic grouping of content |
| `<aside>` | Sidebar advertisement | Tangentially related content |
| `<footer>` | Newspaper footer | Footer info (copyright, contacts, links) |
| `<address>` | Contact card | Contact information for the author |

---

## ✅ What TO Do

### DO: Use Semantic Tags Everywhere
```html
<!-- Good: The tag describes its content -->
<body>
  <header>
    <nav>
      <ul>
        <li><a href="/">Home</a></li>
        <li><a href="/about">About</a></li>
      </ul>
    </nav>
  </header>

  <main>
    <article>
      <h1>How to Brew Coffee</h1>
      <section>
        <h2>Choosing Beans</h2>
        <p>Fresh beans make all the difference...</p>
      </section>
    </article>
  </main>

  <footer>
    <address>Contact: cafe@example.com</address>
  </footer>
</body>
```

### DO: One `<main>` Per Page
The `<main>` element should appear **only once** per page. It's the snowflake — unique and irreplaceable. If you need to wrap repeated content, use `<section>` or `<article>`.

---

## 🚫 What NOT to Do

### DON'T: Suffer from "Divitis"
```html
<!-- Bad: Everything is a div. What does it all mean? -->
<div class="header">
  <div class="nav">
    <div class="menu">...</div>
  </div>
</div>
```

### DON'T: Mix `<button>` and `<a>`
```html
<!-- Bad: <a> is for navigation. <button> is for actions. -->
<a href="/submit" class="btn">Submit Form</a>  <!-- Opens URL -->
<button onclick="location.href='/submit'">Go to Submit</button>  <!-- Triggers JS -->
```

### DON'T: Use Heading Tags for Styling
```html
<!-- Bad: h1 is for the main title, not to make text big -->
<h1 style="font-size: 14px;">Small Text</h1>

<!-- Good: Use CSS for styling, h1 for structure -->
<h1>The Main Title of This Page</h1>
```

---

## 🎯 Why This Matters

### In the Workplace: Accessibility (a11y)
Blind users rely on **screen readers** to navigate your page. When a screen reader hits a `<div>`, it says nothing meaningful. When it hits `<nav>`, it announces: *"Navigation region."*

A user with visual impairment can jump straight to `<main>` and skip the repetitive header/nav/footer. That's **inclusion**.

### In the Workplace: SEO
Google's crawlers are like screen readers. Semantic HTML helps Google understand:
- What is the **main topic** of your page (`<article>`)
- Where is the **navigation** (`<nav>`)
- What is **tangential** content (`<aside>`)

Sites with proper semantics rank higher. It's not magic — it's machine-readable structure.

---

## 🧠 Mental Model: The Building

| Physical Building | HTML Structure |
|-------------------|----------------|
| The whole building has one address | `<html>` is the root |
| The lobby, floors, rooms divide space | `<body>`, `<main>`, `<section>` |
| Each room has a purpose | Each tag has semantic meaning |
| Fire exits labeled clearly | `<nav>`, `<footer>` tell users where to go |

---

## 📚 Technical Glossary

- **Divitis:** The anti-pattern of overusing `<div>` for everything instead of semantic elements.
- **Screen Reader:** Software (like NVDA, VoiceOver) that reads page content aloud for blind users.
- **DOM (Document Object Model):** The tree structure that represents your HTML in the browser's memory.
- **Landmark Roles:** ARIA roles (`role="navigation"`) that duplicate what semantic tags already provide — use the tags instead.

---

[⬅️ Back to Parent](../README.md) | [➡️ Next: 02-Forms-and-Inputs](../02-Forms-and-Inputs/README.md)
