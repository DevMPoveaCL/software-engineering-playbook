# 01 — Core Syntax and Types

[⬅️ Back to Parent](../README.md)

---

## 🎯 What This Folder Covers

This folder teaches you the **DNA of JavaScript**: how the language stores, manipulates, and reasons about data. Think of it as learning vocabulary and grammar before writing a novel.

---

## 1. Variable Declarations — The Three Musketeers

### Real-World Analogy: The Three Contract Types

Imagine you're a **project manager** hiring contractors:
| Contractor | Behavior | Contract Terms |
|------------|----------|----------------|
| `var` | Old-school, unreliable — shows up anywhere, hoists himself to the top of the floor | Function-scoped, redeclarable, hoisted as `undefined` |
| `let` | Modern professional — block-scoped, can't be in two places at once | Block-scoped, reassignable, TDZ protection |
| `const` | Most trustworthy — signs a binding contract, cannot be reassigned | Block-scoped, **immutable binding**, TDZ protection |

### The Temporal Dead Zone (TDZ) — Why It Matters

```js
// ❌ WHAT NOT TO DO — Trying to access 'let' before declaration
console.log(counter); // ReferenceError: Cannot access 'counter' before initialization
let counter = 10;

// ✅ CORRECT — Declaration happens BEFORE access
let counter = 10;
console.log(counter); // 10
```

**Why in the workplace:** `var` hoisting is like a contractor who walks into the meeting room before the meeting is officially called to order. Unpredictable. Use `const` by default; `let` only when reassignment is genuinely needed.

---

## 2. Data Types — The Seven Primitives

### The Periodic Table of JS Types

```
┌─────────────────────────────────────────────────────────┐
│                    JavaScript Data Types                 │
├──────────────────────────┬──────────────────────────────┤
│     PRIMITIVES (7)       │   NON-PRIMATIVES (Reference) │
├──────────────────────────┼──────────────────────────────┤
│ string  "hello"          │                             │
│ number  42, 3.14, NaN     │   Object   { key: "value"} │
│ bigint  9007199254740991n │   Array    [1, 2, 3]        │
│ boolean true / false      │   Function function(){}    │
│ undefined (declared, no  │   Date, RegExp, Map, Set    │
│         value yet)       │                             │
│ null     (empty on purpose│                             │
│ symbol   Symbol("id")     │                             │
└──────────────────────────┴──────────────────────────────┘
```

### The `typeof` Trap Table

```js
typeof "hello"     // "string"  ✅
typeof 42          // "number"  ✅
typeof true        // "boolean" ✅
typeof undefined   // "undefined" ✅
typeof null        // "object"  ⚠️  (historical JS bug — null is NOT an object)
typeof {}          // "object"  ✅
typeof []          // "object"  ⚠️  (arrays are objects in JS)
Array.isArray([])  // true      ✅  — Use this to check arrays
```

**Workplace rule:** Never trust `typeof` for arrays or null. Write `Array.isArray()` or `value === null`.

---

## 3. String Methods — Your Text Toolkit

### Searching & Matching

| Method | Returns | Real-World Analogy |
|--------|---------|-------------------|
| `includes(substr)` | `boolean` | "Does this document contain the word 'invoice'?" |
| `startsWith(str)` | `boolean` | "Does this URL start with 'https'?" |
| `endsWith(str)` | `boolean` | "Does this filename end with '.pdf'?" |
| `indexOf(str)` | `number` (or `-1`) | "Find the first occurrence of 'error' at position 42" |
| `lastIndexOf(str)` | `number` | "Find the last occurrence of 'error'" |
| `match(regex)` | `array` or `null` | "Extract all email addresses from this text" |
| `search(regex)` | `number` (or `-1`) | "Find where the zip code pattern starts" |

### Transformation Pipeline

```
Input: "  ALICE Cooper  "

    ┌─────────────────────────────────┐
    │  trim()                         │
    │  "  ALICE Cooper  " → "ALICE Cooper"
    └─────────────────────────────────┘
              ↓
    ┌─────────────────────────────────┐
    │  toLowerCase()                  │
    │  "ALICE Cooper" → "alice cooper" │
    └─────────────────────────────────┘
              ↓
    ┌─────────────────────────────────┐
    │  toUpperCase()                  │
    │  "alice cooper" → "ALICE COOPER" │
    └─────────────────────────────────┘
              ↓
    ┌─────────────────────────────────┐
    │  repeat(count)                  │
    │  "ha" → "hahaha" (x3)           │
    └─────────────────────────────────┘
```

### Template Literals — Multiline strings without the pain

```js
// ❌ OLD WAY — Messy concatenation with \n
const bio = "Name: " + name + ", Age: " + (age + 1) + "\nLocation: " + city;

// ✅ MODERN WAY — Readable template literal
const bio = `
  Name: ${name}
  Age: ${age + 1}
  Location: ${city}
`;
```

---

## 4. Array Methods — The Modern Toolkit

### Two Categories: Mutating vs Non-Mutating

**Think of it like this:**

- **Mutating methods** = rearranging furniture in the **same room**
- **Non-mutating methods** = taking furniture to a **new room** (leaving original intact)

```
┌──────────────────────────────────────────────────────────────────┐
│                    MUTATING (In-Place)                           │
├──────────────┬───────────────────────────────────────────────────┤
│  push()      │  Add to END        → [1, 2, 3] push(4) → [1,2,3,4]│
│  pop()       │  Remove from END   → [1,2,3] pop() → removes 3    │
│  unshift()   │  Add to START     → [1,2] unshift(0) → [0,1,2]   │
│  shift()     │  Remove from START→ [1,2,3] shift() → removes 1 │
│  splice()    │  Cut/Replace      → surgical removal             │
│  sort()      │  In-place reorder → [3,1,2].sort() → [1,2,3]    │
│  reverse()   │  Flip order       → [1,2,3].reverse() → [3,2,1] │
└──────────────┴───────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────┐
│                 NON-MUTATING (Returns New)                       │
├──────────────┬───────────────────────────────────────────────────┤
│  concat()    │  Merge arrays   → [1].concat([2]) → [1,2]         │
│  slice()     │  Copy portion   → [1,2,3].slice(1,2) → [2]       │
│  map()       │  Transform each → [1,2,3].map(n => n*2) → [2,4,6]│
│  filter()    │  Keep matching  → [1,2,3,4].filter(n => n%2===0) │
│  find()      │  First match    → [1,2,3].find(n => n>1) → 2    │
│  reduce()    │  Accumulate      → [1,2,3].reduce((a,n) => a+n)  │
└──────────────┴───────────────────────────────────────────────────┘
```

### The Power of Chaining — Assembly Line Metaphor

```js
// Think of it like an assembly line:
// Raw materials (data) → Process 1 → Process 2 → Final Product

[1, 2, 3, 4, 5]
  .filter(n => n % 2 === 0)  // Step 1: Keep only even [2, 4]
  .map(n => n * 10)          // Step 2: Double each [20, 40]
  .reduce((acc, n) => acc + n, 0); // Step 3: Sum = 60
```

### `forEach` vs `map` — The Crucial Difference

```js
// ❌ WRONG — forEach returns undefined, can't chain
[1, 2, 3].forEach(n => console.log(n * 2)); // undefined

// ✅ CORRECT — map returns a new array, chainable
[1, 2, 3].map(n => n * 2); // [2, 4, 6]
```

**Workplace rule:** If you need to transform data, use `map`. If you just need side effects (logging, DOM updates), use `forEach`.

---

## 5. Functions — The Building Blocks

### Declaration vs Expression vs Arrow — Three Styles

```
┌─────────────────────────────────────────────────────────────────┐
│  FUNCTION DECLARATION          FUNCTION EXPRESSION   ARROW FN  │
├─────────────────────────────────────────────────────────────────┤
│  function greet(name) {       const greet = function(n){   const greet = (n) => `
│    return `Hello, ${n}`         return `Hello, ${n}`         `Hello, ${n}`
│  }                             }                             `; }
├─────────────────────────────────────────────────────────────────┤
│  ✅ Hoisted                   ❌ Not hoisted             ❌ Not hoisted  │
│  ✅ Has 'arguments'          ✅ Named variable          ❌ No 'this'    │
│  ✅ Has own 'this'           ✅ Has own 'this'          ✅ Concise       │
│                                 (anonymous)                 ✅ No binding  │
└─────────────────────────────────────────────────────────────────┘
```

### The `this` Problem — Why Arrow Functions Matter

```js
// ❌ PROBLEM — Regular function loses 'this' context
const counter = {
  count: 0,
  increment: function() {
    setTimeout(function() {
      this.count++; // ❌ 'this' is NOT counter here (it's the timeout)
    }, 1000);
  }
};
// ✅ SOLUTION — Arrow function preserves 'this'
const counter = {
  count: 0,
  increment: function() {
    setTimeout(() => {
      this.count++; // ✅ 'this' IS counter
    }, 1000);
  }
};
```

**Workplace rule:** Use arrow functions for callbacks and inline functions. Use regular `function` for methods that need their own `this` context.

---

## 6. Control Flow — Decision Making

### The Decision Tree

```
                    ┌─────────────────┐
                    │  START / entry   │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │  Condition?     │
                    │  (if/switch)    │
                    └───┬──────┬──────┘
                        │ TRUE │ FALSE
                        ▼      ▼
               ┌──────────┐   ┌──────────┐
               │  Block A │   │  Block B  │
               │(executed)│   │(skipped) │
               └──────────┘   └──────────┘
                        │      │
                        ▼      ▼
                   ┌──────────────┐
                   │  END / exit  │
                   └──────────────┘
```

### Loop Selection Guide

| Loop Type | Use When | Workplace Metaphor |
|-----------|----------|-------------------|
| `for` | You know the exact iteration count | "Count the chairs in each of the 5 rows" |
| `for...of` | Iterating over an array/list | "Hand out one flyer to each person in line" |
| `for...in` | Iterating over object keys | "Check each light switch in the building" |
| `while` | Condition-based, unknown iteration | "Keep boiling water until it reaches 100°C" |
| `do...while` | Must execute at least once | "Ask for the password at least once" |

---

## 7. Objects — Structured Data Containers

### Creation Patterns

```js
// Literal — Most common, readable
const alice = { name: "Alice", age: 30 };

// Constructor — Classical OOP style
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// Class — Modern OOP (syntactic sugar over prototype)
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}
```

### Destructuring — Unpacking Like a Suitcase

```js
const user = { name: "Alice", age: 30, city: "NYC" };

// ❌ OLD WAY — Repetitive
const name = user.name;
const age = user.age;

// ✅ MODERN WAY — Destructuring
const { name, age } = user;
const { name: userName, city: hometown = "Unknown" } = user;
```

**Workplace rule:** Destructuring reduces typos and makes refactoring easier. If you see `user.name` three times, destructure it once.

---

## 📋 Quick Reference Card

| Concept | Remember |
|---------|----------|
| Variables | `const` default, `let` for reassignment, never `var` |
| Types | 7 primitives + objects; use `Array.isArray()` for arrays |
| Strings | Immutable; methods return new strings |
| Arrays | Prefer non-mutating (`map`, `filter`, `reduce`) |
| Functions | Arrow for callbacks, regular for methods |
| Control | `switch` for known values, `if` for complex conditions |
| Objects | Destructuring over dot notation repetition |

---

## ✅ What TO Do

1. **Default to `const`** — It signals intent and prevents accidental reassignment
2. **Prefer non-mutating array methods** — They make code predictable and debuggable
3. **Use destructuring** — Clean, readable, less error-prone
4. **Template literals over concatenation** — Handles multiline, embedding expressions cleanly
5. **Use `for...of` for arrays** — More readable than classic `for` loops

## ❌ What NOT To Do

1. **Don't use `var`** — Function scope causes unpredictable behavior
2. **Don't use `==` (loose equality)** — Type coercion leads to hidden bugs: `"" == false` is `true`
3. **Don't mutate original arrays** — `push()` on the original breaks immutability; use spread instead
4. **Don't use `typeof` for arrays/null** — It's a known JS bug; use `Array.isArray()` or `=== null`
5. **Don't forget the `new` for Date/RegExp** — `Date.now()` vs `new Date()`

## 🏢 Workplace Wisdom

> "A developer who understands `const`, `let`, and immutable patterns writes code that other developers can reason about without headaches."
>
> "If your array code is full of `.push()` and `.pop()`, you're writing Java like it's 2010. Embrace `map`, `filter`, and `reduce` — they make debugging trivial because the original data is always preserved."
