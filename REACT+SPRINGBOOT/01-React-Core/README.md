# ⚛️ 01 - React Core

> **Your first building block.** Like Lego, React is made of reusable pieces called Components.

## 🎯 What Is React Core?

React Core is the fundamental library—nothing more than a **UI rendering engine**. It doesn't have routing, HTTP calls, or complex state management out of the box. You compose the UI by building **Components**.

## 🧱 Component Anatomy (SRP + DRY in action)

```
┌─────────────────────────────────────────────┐
│           React Component                   │
├─────────────────────────────────────────────┤
│  Props (input)  →  [Logic + JSX]  →  UI    │
│  State (memory) →                         │
└─────────────────────────────────────────────┘
```

### Analogy: The Pizza Chef
- **Component** = A pizza recipe (template)
- **Props** = The ingredients passed in (you can't change what's given)
- **State** = Your internal notes (what you decide to remember)
- **JSX** = The final pizza (what the customer sees)

## ⚡ useState — Component Memory

```jsx
const [pizzasOrdered, setPizzasOrdered] = useState(0);
```

| What to do ✅ | What NOT to do ❌ | Why |
|--------------|-------------------|-----|
| Initialize with a proper default value | Leave state undefined | Avoids unpredictable behavior |
| Use setters to modify state | Mutate state directly (`pizzasOrdered++`) | React needs immutable updates |
| Keep state minimal | Store computed values in state | Derived data should be derived, not stored |

### Real-World Analogy
State is like a **light switch**. It remembers `on` or `off`. Flip it → the room repaints instantly.

---

## 🎬 useEffect — Side Effects

Side effects run **after** render—like a waiter putting up the "Open" sign after the restaurant starts.

```jsx
useEffect(() => {
  fetchPizzas(); // API call after render
}, []); // Empty = run once on mount
```

| What to do ✅ | What NOT to do ❌ | Why |
|--------------|-------------------|-----|
| Declare dependencies in array | Forget deps that are used inside | Stale closures |
| Return cleanup function | Forget cleanup for subscriptions | Memory leaks |
| Keep effects focused | Put unrelated logic in one effect | Separation of concerns |

---

## 🔗 Props — Inter-Component Communication

```
┌──────────────┐     envelope (props)      ┌──────────────┐
│   Parent     │ ──────────────────────────► │   Child      │
│   (Boss)     │ ◄───────────────────────── │  (Employee) │
└──────────────┘      state updates       └──────────────┘
```

| What to do ✅ | What NOT to do ❌ | Why |
|--------------|-------------------|-----|
| Props flow down (parent → child) | Mutate props in child | Props are read-only (like a will) |
| Destructure props | Access via `arguments[0]` | Readability |
| Pass only what child needs | Prop drilling (passing through 5 levels) | Use Context instead |

---

## 🎯 useContext — Shared State Without Prop Drilling

```jsx
// Without Context: App → Header → Nav → UserAvatar (4 levels of passing)
// With Context:    App → [Context] → UserAvatar (direct)
```

| What to do ✅ | What NOT to do ❌ | Why |
|--------------|-------------------|-----|
| Create context per domain (Auth, Theme) | One giant context for everything | Hard to maintain |
| Provide at top, consume below | Consume above provider | Violates React tree flow |
| Split by update frequency | Store everything in one context | Unnecessary re-renders |

---

## 📋 useRef — DOM Access & Stable Values

```jsx
const inputRef = useRef(null);
inputRef.current.focus(); // Direct DOM access
```

| What to do ✅ | What NOT to do ❌ | Why |
|--------------|-------------------|-----|
| Access DOM elements | Overusing for state | useState exists for a reason |
| Store mutable values that persist | Using ref for derived values | Refs don't trigger re-renders |
| Cleanup in useEffect | Forgetting to cleanup | Prevents memory leaks |

---

## 🧠 Quick Reference: Component Decision Tree

```
Is it UI-only (no state needed)?
  → Plain Function Component

Does it need internal memory?
  → useState

Does it need to "talk" to outside (API, timer, subscription)?
  → useEffect

Does it need to access DOM directly?
  → useRef

Is data needed by many components at different levels?
  → useContext
```

---

## 🏠 Project Structure (Clean Architecture for React)

```
src/
├── components/          # Presentational (dumb) components
│   ├── Button/
│   │   ├── Button.tsx
│   │   └── Button.test.tsx
│   └── Card/
├── hooks/               # Custom hooks (business logic)
│   ├── useAuth.ts
│   └── usePizzas.ts
├── context/             # State providers
│   └── AuthContext.tsx
├── types/               # TypeScript interfaces
└── utils/               # Pure functions
```

---

## 🔗 Related Topics

- **[⬅️ Back to Parent](../README.md)**
- **[➡️ Next: Spring Boot API](../02-Spring-Boot-API/README.md)**

---

*Principles applied: SRP (Single Responsibility), DRY, KISS*
