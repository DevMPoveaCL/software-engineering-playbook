# 🌐 HTML - The Structure of the Web

HTML (HyperText Markup Language) **is NOT a programming language**. It's a **markup** language. You can't do math with it; its only purpose is to give structure and semantic meaning to your content.

Think of it as **the foundation and beams of a house**. If the foundation is poorly built (bad semantics), no amount of paint (CSS) will prevent the house from having problems in the long run.

---

## 📊 Objective Table: HTML Analysis

| Aspect | Didactic Explanation |
|--------|----------------------|
| **What is it for?** | Describes and organizes web content (text, images, links, videos) so browsers can understand it. |
| **What are the benefits?** | Helps Google (SEO) understand your page's topic and allows people with disabilities to access it through assistive technology. |
| **When to use it?** | **ALWAYS**. It's the absolute foundation of the internet. Every framework (React, Angular, Vue) ultimately compiles to pure HTML. |
| **When NOT to use it?** | Don't use it to store dynamic data, process logic, or apply visual styles. That's not its responsibility (SRP Principle). |

---

## 📚 Learning Path

| Folder | Topic | What You'll Learn |
|--------|-------|-------------------|
| [01-Semantic-Web](./01-Semantic-Web/README.md) | Semantic Tags | Why `<header>`, `<nav>`, `<main>` matter — accessibility and structure |
| [02-Forms-and-Inputs](./02-Forms-and-Inputs/README.md) | Forms & Validation | How to build forms that work for everyone, everywhere |
| [03-SEO-and-Metadata](./03-SEO-and-Metadata/README.md) | SEO & Metadata | The invisible layer that makes your page findable |

---

## 🧠 Best Practices: Web Semantics

The worst habit of a novice web programmer is suffering from "Divitis" (using the generic `<div>` tag for absolutely everything).

In professional development, **semantics is law**. You must use tags that exactly describe what the content is:
- Use `<header>` for the header.
- Use `<nav>` for the navigation menu.
- Use `<main>` for the unique main content of that page.
- Use `<footer>` for the footer.
- Use `<button>` for actions (like "Submit Form") and `<a>` for links that change pages. Don't mix them!

> **Didactic Tip:** If you build everything with `<div>`, a blind user using a "Screen Reader" will hear meaningless noise. If you use semantic tags, the program will tell them exactly where the menu is and where the important content is. **Writing good HTML is a matter of inclusion and professionalism.**

---

## 📚 Technical Glossary

- **Semantics:** Using the correct element for the correct meaning (e.g., not using an `<h1>` title just to make text bigger, but because it's the main title of the page).
- **SEO (Search Engine Optimization):** The technique of making your page appear first on Google. Good semantic HTML dramatically improves SEO.
- **Tag:** Words enclosed in angle brackets `< >` that tell the browser what the text is.
- **Accessibility (a11y):** Designing pages that can be used by everyone, including people with visual, motor, or cognitive disabilities.

---
### 🔗 Global Navigation
[⬅️ Previous Topic: CSS](../CSS/README.md) | [🏠 Master Index](../README.md) | [➡️ Next Topic: React + SpringBoot](../REACT+SPRINGBOOT/README.md)
<br>
**[⬇️ Dive In: 01-Semantic-Web](./01-Semantic-Web/README.md)**
