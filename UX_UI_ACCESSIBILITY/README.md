# 🎨 UX, UI and Web Accessibility

In this section we'll group the rules and regulations that ensure our interfaces not only look good, but are inclusive and legally sound.

---

## 📊 Objective Table: UX/UI/Accessibility Analysis

| Aspect | Didactic Explanation |
|--------|----------------------|
| **What is it?** | The discipline of designing interfaces that are visually effective, cognitively accessible, and legally compliant (WCAG 2.2, ADA). |
| **Benefits** | Increases user satisfaction, expands audience reach (1.3 billion people have disabilities), reduces legal risk, improves SEO. |
| **When to use it?** | Every interface, landing page, dashboard, or interactive product. Design decisions should always consider accessibility from day one. |
| **When NOT to use it?** | Never skip accessibility. Even "simple" landing pages need proper contrast, keyboard navigation, and alt text. |

---

## 🗂️ Learning Path

| Folder | Topic | What You'll Learn |
|--------|-------|-------------------|
| `01-Visual-Design-Rules` | Visual Hierarchy, Color Theory, Typography, Spacing, CRAP Principles | The foundational laws of visual design — how humans perceive, process, and remember information |
| `02-Accessibility-WCAG` | WCAG Principles (POUR), Level A/AA/AAA Criteria, Testing | The legal and ethical requirements for building inclusive digital experiences |
| `03-Cognitive-Doc-Design` | Progressive Disclosure, Chunking, Recognition Over Recall, Signposting | How to design documentation that people can actually use, understand, and remember |

---

## ⚖️ Executive Summary: Legality and Accessibility in Landing Pages

**Objective:** Implement a high-impact visual experience (vault, animations, and infinite loops) without violating international accessibility standards (WCAG 2.2, ADA, European Accessibility Directive).

### 1. User Control Rule (WCAG 2.2.2 Criterion)
- **What to do:** Any animation or video in a loop lasting more than 5 seconds **must** be pausable or stoppable by the user.
- **Why by law:** It prevents people with attention deficits (ADHD), cognitive difficulties, or low vision from getting so distracted they can't consume the main content (like your menu and mission).
- **Teaching Solution:** Add a clearly visible "Pause/Play" button that's accessible by navigating only with the `Tab` key on your keyboard.

### 2. Visual Safety (Flashing Threshold Criterion)
- **What to do:** Don't include visual elements that flash more than 3 times per second.
- **Why by law:** Rapid flashing can trigger **photosensitive epilepsy**. It's one of the strictest accessibility regulations, and non-compliance can result in serious lawsuits.
- **Teaching Solution:** Make smooth animations, use slow transitions (for example, when opening a digital vault) and avoid intense white flashes or strobes.

### 3. Autoplay and Sound (Autoplay Policy)
- **What to do:** Any background video must be muted (`muted`) by default.
- **Why by law/browsers:** Modern browsers (Chrome, Safari) block by code any video with autoplay sound. This protects privacy and avoids annoying startled reactions when users enter a website.
- **Teaching Solution:** When placing your `<video>` tag in HTML, make sure to include the attributes: `autoplay muted loop playsinline`.

### 4. Respecting System Preferences (Prefers-Reduced-Motion)
- **What to do:** Your website should "listen" to whether the user has enabled "Reduce motion" in their operating system's general settings (Windows, Mac, or mobile).
- **Why by law:** Forcing a person with vestibular disorders (prone to nausea or vertigo) to watch an aggressive or large-scale animation is considered a direct access barrier.
- **Teaching Solution:** Use a CSS *Media Query* (`@media (prefers-reduced-motion: reduce)`) to display an elegant static image instead of a video if the system detects this preference is enabled.

### 5. Contrast and Legibility (WCAG 1.4.3 Criterion)
- **What to do:** Text and interactive menus must have a contrast ratio of at least `4.5:1` against the background or video.
- **Why by law:** A video with many colors or movement can camouflage letters, making them illegible for people with reduced vision, or even for regular people using their devices in sunlight.
- **Teaching Solution:** Place a semi-transparent dark filter (*overlay*) over the background video. That way, the white menu letters will always stand out regardless of what's showing behind.

---

> **Practical Conclusion:**
> A spectacular design with aggressive animations (like a vault opening on scroll) can be legally sound **if the main movement is initiated by the user** (scroll) and if we give them the tools to stop the background loop. Good UI code always considers the health and comfort of its end user.

---
### 🔗 Global Navigation
[⬅️ Previous Topic: SQL](../SQL/README.md) | [🏠 Master Index](../README.md)
<br>
**[⬇️ Dive In: 01-Visual-Design-Rules](./01-Visual-Design-Rules/README.md)**
