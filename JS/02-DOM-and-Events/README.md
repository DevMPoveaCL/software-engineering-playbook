# 02 — DOM and Events

[⬅️ Back to Parent](../README.md)

---

## 🎯 What This Folder Covers

This folder teaches you how JavaScript **speaks to HTML** — selecting elements, changing them, and responding when users interact. Think of it as the nervous system connecting your page's brain (JS) to its body (HTML).

---

## 1. The DOM — The Living Tree

### What is the DOM?

The **Document Object Model (DOM)** is your HTML transformed into an in-memory tree that JavaScript can manipulate.

```
Real World: A family tree               DOM: An HTML tree

     ┌──────────┐                          <html>
     │  Grand   │                         /     \
     │  Parent  │                    ┌──head──┐  body
     └────┬─────┘                   title     meta   ┌──div#app
    ┌─────┴─────┐                              ┌────┴─────┐
┌───┤     ┌────┴────┐                     ┌──┴──┐  ┌────┴────┐
│Parent│   │Parent  │                    header nav section
│  A   │   │  B     │                       └──┬──┘    └───┬───┘
└───┬──┘   └────┬───┘                          │         │
┌───┴───┐ ┌─────┴────┐                       ┌──┴───┐  ┌──┴───┐
│ Child │ │ Child 2 │                     ┌──┤Child│  │Child│
│  A1   │ │   B1    │                     │  └─────┘  └─────┘
└───────┘ └─────────┘                     └─h1, p, button
```

**Analogy:** The DOM is like an **interactive blueprint**. When you modify the blueprint, the building (the web page) updates in real-time.

---

## 2. Selecting Elements — Finding Your Way

### Method Comparison Table

| Method | Returns | Collection Type | Use Case |
|--------|---------|----------------|----------|
| `getElementById(id)` | Element or `null` | Single | Fastest. Use for unique elements. |
| `getElementsByClassName(name)` | HTMLCollection | Live | Re-query on DOM change. Use in simple cases. |
| `getElementsByTagName(tag)` | HTMLCollection | Live | Rarely used directly. |
| `querySelector(selector)` | Element or `null` | Single | **General-purpose.** CSS selectors anywhere. |
| `querySelectorAll(selector)` | NodeList | Static | **All matches.** Iterate for multiple. |

### Live vs Static Collections

```
LIVE Collection (getElementsByClassName)
┌─────────────────────────────────────┐
│  Query runs EVERY time you access   │
│  [ .item ] → [ .item ] → [ .item ]  │
│   (DOM changed? → New result!)     │
└─────────────────────────────────────┘

STATIC NodeList (querySelectorAll)
┌─────────────────────────────────────┐
│  Query runs ONCE at call time       │
│  Snapshot taken, never changes     │
│   (Even if DOM changes later!)     │
└─────────────────────────────────────┘
```

**Workplace rule:** `querySelectorAll` is safer because it won't surprise you when the DOM changes underneath.

### Modern Selection Patterns

```js
// ID (fastest)
const header = document.getElementById("header");

// Class (live HTMLCollection)
const buttons = document.getElementsByClassName("btn")[0]; // Need index!

// ✅ MODERN — CSS Selector (any complex query)
const el = document.querySelector("#app .card[data-active='true']");
const allCards = document.querySelectorAll(".card");

// Nested selection
const navLinks = document.querySelector("nav").querySelectorAll("a");
```

---

## 3. Modifying Elements — Changing the Page

### The Inner Text vs Inner HTML Trap

```
┌─────────────────────────────────────────────────────────┐
│  textContent (SAFE)           innerHTML (DANGEROUS)    │
├─────────────────────────────────────────────────────────┤
│  Treats content as PLAIN TEXT   Parses content as HTML   │
│  XSS-proof                     Vulnerable to XSS        │
│  Faster                        Slower                   │
│                                                         │
│  el.textContent = "<script>";  el.innerHTML = "<script>";│
│  // Renders literally         // Executes! ⚠️          │
└─────────────────────────────────────────────────────────┘
```

**Workplace rule:** Never put user input into `innerHTML`. Use `textContent` or sanitize with DOMPurify.

### Modifying Classes — The classList API

```js
const btn = document.querySelector(".btn");

// Single operations
btn.classList.add("active");
btn.classList.remove("hidden");
btn.classList.toggle("visible");
btn.classList.contains("active"); // boolean

// ❌ OLD WAY — Overwrites all classes
btn.className = "btn active";

// ✅ MODERN WAY — Preserves existing classes
btn.className += " active";
```

### Styles — When to Use Them

```js
// ❌ WRONG — Inline styles everywhere
element.style.color = "blue";
element.style.backgroundColor = "#f0f0f0";

// ✅ CORRECT — CSS classes, not inline styles
.element { color: blue; }
.element.is-active { color: red; }

// JS just toggles the class
element.classList.add("is-active");
```

**Why:** Inline styles make debugging harder, can't be overridden by CSS specificity, and mix concerns.

---

## 4. Creating & Inserting Elements

### The DOM Surgery Diagram

```
BEFORE: <parent>                    AFTER: <parent>
        ┌───────┐                         ┌───────┐
        │ child │ ─────────────────────►  │ child │
        └───────┘                         └───────┘
                                              │
                                              ▼
                                        ┌───────┐
                                        │ new   │  ← appended
                                        │ child │
                                        └───────┘

INSERTION METHODS:
┌──────────────────┬──────────────────────────────────────────┐
│ appendChild()    │ Add to END of parent's children          │
│ prepend()        │ Add to START of parent's children        │
│ insertBefore()   │ Insert before a reference node           │
│ insertAdjacentHTML() │ "beforebegin" | "afterbegin" |       │
│                   │ "beforeend" | "afterend"               │
└──────────────────┴──────────────────────────────────────────┘
```

### Creating Elements — Step by Step

```js
// Step 1: Create the element
const card = document.createElement("div");
card.textContent = "New Card";
card.className = "card";

// Step 2: Insert into DOM
const container = document.querySelector(".container");
container.appendChild(card);

// ❌ WRONG — Creating HTML strings (harder to debug)
container.innerHTML += "<div class='card'>New Card</div>";

// ✅ CORRECT — DOM API (explicit, debuggable)
const card = document.createElement("div");
card.textContent = "New Card";
container.appendChild(card);
```

---

## 5. Event Handling — Making Pages Interactive

### The Event Listener Pattern

```
USER ACTION (click, keypress, scroll)
        │
        ▼
┌───────────────────────────────────┐
│  1. Browser detects event         │
│  2. Creates event object          │
│  3. Checks capture/bubble phase  │
│  4. Fires handlers on matching    │
│     elements                      │
└───────────────────────────────────┘
```

### The Listener Registration Model

```js
// ✅ CORRECT — addEventListener (can have multiple!)
element.addEventListener("click", handler);
element.addEventListener("click", anotherHandler);

// ❌ WRONG — onClick property (overwrites!)
element.onclick = handler1;
element.onclick = handler2; // handler1 is GONE
```

### Event Delegation — The Parent Listens

```
WITHOUT Delegation (N handlers):
┌─────────────────────────────────────┐
│  .item #1 → click handler          │
│  .item #2 → click handler          │
│  .item #3 → click handler          │ ← 100 items = 100 handlers
│  ...                                │
└─────────────────────────────────────┘

WITH Delegation (1 handler):
┌─────────────────────────────────────┐
│  .list → click handler             │
│    └─► checks e.target.matches     │
│    └─► "was an .item clicked?"     │
│        YES → handle                │
└─────────────────────────────────────┘
```

```js
// ❌ ANTI-PATTERN — One handler per item
document.querySelectorAll(".item").forEach(item => {
  item.addEventListener("click", () => console.log("Clicked"));
});

// ✅ PATTERN — Event delegation
document.querySelector(".list").addEventListener("click", (e) => {
  if (e.target.matches(".item")) {
    console.log("Item clicked:", e.target.textContent);
  }
});
```

**Why it matters:** Delegation uses **one** event listener instead of hundreds. Better performance, handles dynamically added elements.

---

## 6. Common Events Reference

| Event | When to Use | Workplace Metaphor |
|-------|-------------|-------------------|
| `click` | Buttons, links, interactive elements | "The user pressed the button" |
| `submit` | Forms (with preventDefault) | "The user finished filling out the form" |
| `change` | Selects, checkboxes, radio buttons | "The user made a selection" |
| `input` | Text fields (every keystroke) | "The user is typing" |
| `keydown` / `keyup` | Keyboard shortcuts | "The user pressed a key" |
| `mouseenter` / `mouseleave` | Hover effects | "The user's cursor entered/exited" |
| `focus` / `blur` | Form fields | "The user is typing in this field" |
| `DOMContentLoaded` | When HTML is parsed | "The page skeleton is ready" |

---

## 7. DOM Traversal — Navigation

```
┌─────────────────────────────────────────────────────┐
│                    ELEMENT NAVIGATION               │
├─────────────────────────────────────────────────────┤
│                                                     │
│  parentNode          ← Go to parent                 │
│  children            ← HTMLCollection of children   │
│  firstElementChild   ← First child element          │
│  lastElementChild    ← Last child element           │
│  nextElementSibling  ← Next on same level          │
│  previousElementSibling ← Previous on same level   │
│                                                     │
│  ┌─────────┐                                        │
│  │ Parent  │                                        │
│  └───┬─────┘                                        │
│  ┌────┴────┐                                        │
│  │  elem   │←── you are here                        │
│  └────┬────┘                                        │
│  ┌────┴────┐                                        │
│  │ Child   │                                        │
│  └─────────┘                                        │
└─────────────────────────────────────────────────────┘
```

---

## 📋 Quick Reference Card

| Concept | Remember |
|---------|----------|
| Selection | `querySelector` for one, `querySelectorAll` for all |
| Content | `textContent` safe, `innerHTML` dangerous |
| Classes | `classList.add/remove/toggle`, never overwrite `className` |
| Styles | CSS classes > inline JS styles |
| Insertion | `appendChild`, `prepend`, `insertAdjacentHTML` |
| Events | `addEventListener`, never overwrite `onclick` |
| Delegation | Listen on parent, check `e.target.matches()` |

---

## ✅ What TO Do

1. **Use `querySelector` and `querySelectorAll`** — CSS selectors are versatile and readable
2. **Use `textContent` for user-facing text** — Prevents XSS vulnerabilities
3. **Use `classList` for class manipulation** — Clean API, preserves existing classes
4. **Use event delegation** — One handler > many handlers
5. **Use `addEventListener`** — Allows multiple handlers, proper separation

## ❌ What NOT To Do

1. **Don't use `innerHTML` with user input** — XSS risk. Use `textContent` or sanitizers
2. **Don't use `onclick =` properties** — Overwrites previous handlers
3. **Don't select by index in live collections** — Use `querySelectorAll` for stable NodeList
4. **Don't forget `event.preventDefault()` on forms** — Page will reload/submit
5. **Don't manipulate styles inline** — Mixes concerns; use CSS classes instead

## 🏢 Workplace Wisdom

> "Event delegation isn't just a performance trick — it's how scalable DOM code works. One listener handles 1000 items, not 1000 listeners fighting over memory."
>
> "Your page should work with keyboard navigation alone. If you only tested with the mouse, you tested nothing."
