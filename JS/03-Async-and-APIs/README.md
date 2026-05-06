# 03 — Async and APIs

---

## 🎯 What This Folder Covers

This folder teaches you how JavaScript handles **things that take time** — network requests, file reads, timers. Think of it as the language's "while you wait" system: start a task, move on, come back when it's done.

---

## 1. The Event Loop — How JS Actually Works

### The Famous Diagram

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         JavaScript Runtime                              │
│                                                                         │
│  ┌─────────────┐     ┌──────────────┐     ┌─────────────────────────┐  │
│  │   STACK     │     │    HEAP      │     │      WEB APIs /         │  │
│  │ (execution) │     │ (memory)     │     │   Node APIs             │  │
│  │             │     │              │     │                         │  │
│  │ function()  │     │  objects,    │     │ setTimeout, fetch,      │  │
│  │   └─ log()  │     │  closures    │     │ addEventListener,       │  │
│  │             │◄────│              │     │ file system, etc.       │  │
│  └──────┬──────┘     └──────────────┘     └────────────┬────────────┘  │
│         │                                              │                 │
│         │              ┌──────────────────────────────┘                 │
│         │              │                                               │
│         ▼              ▼                                               │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                     CALLBACK QUEUE                              │   │
│  │  ┌────────────┐  ┌────────────┐  ┌────────────┐                 │   │
│  │  │ callback 1 │  │ callback 2 │  │ callback 3 │                 │   │
│  │  └────────────┘  └────────────┘  └────────────┘                 │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│         ▲                                                              │
│         │                    ┌─────────────────────┐                   │
│         └────────────────────┤   EVENT LOOP        │◄── checks if      │
│                              │  (the conductor)    │    stack empty    │
│                              └─────────────────────┘                   │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### Step-by-Step Execution Model

```
TIME ──────────────────────────────────────────────────────────────────────►

CALL STACK:  [ main() ]────────[ syncFn() ]────────[ log(x) ]────── empty
WEB API:     [ setTimeout(fn, 0) ]──[ fetch(url) ]──────────────────────►
QUEUE:       [ fn ]───────────────[ thenCb ]──────────────────────────►

Timeline:
1. main() runs synchronously
2. setTimeout schedules callback (0ms)
3. fetch starts async operation
4. syncFn completes, stack empty
5. Event loop waits for stack to empty
6. callbacks run in order: fn → thenCb
```

**Key insight:** The event loop **never interrupts** running code. It waits for the stack to empty, then picks the next callback.

---

## 2. Callbacks — The Old Pattern

### Real-World Analogy: The Voicemail

```js
// Callback = leaving a voicemail with instructions
// "When you're done, call this number and say the result"

function fetchData(callback) {
  setTimeout(() => {
    callback("Data received! 🎉");
  }, 1000);
}

fetchData(result => console.log(result));
```

### The Callback Hell Problem

```
┌─────────────────────────────────────────────────────────┐
│              CALLBACK HELL (Pyramid of Doom)           │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  getUser(userId, (user) => {                            │
│    getOrders(user.id, (orders) => {                     │
│      getProducts(orders, (products) => {                │
│        getInventory(products, (inventory) => {          │
│          // DEEP NESTING = UNREADABLE = UNMAINTAINABLE │
│        });                                             │
│      });                                               │
│    });                                                 │
│  });                                                   │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

**Why it's a problem:** Nesting makes error handling a nightmare. Which level failed? Which callback didn't run?

---

## 3. Promises — The Modern Ticket System

### The Restaurant Metaphor

```
CUSTOMER (Promise Consumer)
    │
    │  "I'd like a promise of a pizza"
    ▼
┌─────────────────────────────────────┐
│  KITCHEN (Promise)                 │
│                                     │
│  ┌─────────┐     ┌─────────────┐   │
│  │ pending │────►│ fulfilled   │   │  ← Pizza ready!
│  └─────────┘     │ (resolve)   │   │
│        │         └─────────────┘   │
│        │         ┌─────────────┐   │
│        └────────►│ rejected    │   │  ← Kitchen burned it
│                  │ (reject)    │   │
│                  └─────────────┘   │
└─────────────────────────────────────┘
```

### Promise States Visualized

```
┌─────────────────────────────────────────────────────────────┐
│                    PROMISE LIFECYCLE                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│                    ┌──────────┐                             │
│                    │ pending  │  ← Initial state            │
│                    └────┬─────┘                             │
│              ┌──────────┴──────────┐                       │
│              ▼                      ▼                       │
│     ┌──────────────┐       ┌──────────────┐                │
│     │  fulfilled   │       │   rejected   │                │
│     │  (resolved)  │       │   (error)    │                │
│     │ data available│       │  failure     │                │
│     └──────────────┘       └──────────────┘                │
│                                                             │
│  Promise can ONLY transition once from pending              │
│  Once settled (fulfilled/rejected), state is FINAL          │
└─────────────────────────────────────────────────────────────┘
```

### Consuming Promises

```js
// ❌ OLD — Callback nesting
fetch("/api/users", (err, res) => {
  if (err) handleError(err);
  else processUsers(res);
});

// ✅ MODERN — Chained promises
fetch("/api/users")
  .then(response => response.json())
  .then(data => processUsers(data))
  .catch(error => handleError(error))
  .finally(() => stopLoading());
```

### Error Handling — The Catch Pattern

```
┌─────────────────────────────────────────────────────────┐
│              ERROR PROPAGATION IN PROMISES              │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  fetch("/api/users")                                    │
│    .then(parseJSON)  ← If this throws, .catch catches  │
│    .then(process)   ← This is SKIPPED                 │
│    .then(display)    ← This is SKIPPED                 │
│    .catch(err => {   ← ✅ Error handled here!         │
│      showError(err);                                   │
│    })                                                  │
│    .finally(() => {   ← ✅ Always runs                 │
│      hideSpinner();                                     │
│    })                                                  │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

## 4. Async/Await — Promises in Disguise

### The Syntactic Sugar Explained

```js
// PROMISE CHAIN (what async/await compiles to)
fetch("/api/data")
  .then(r => r.json())
  .then(data => console.log(data))
  .catch(err => console.error(err));

// ASYNC/AWAIT (cleaner syntax, same behavior)
async function getData() {
  try {
    const response = await fetch("/api/data");
    const data = await response.json();
    console.log(data);
  } catch (err) {
    console.error(err);
  }
}
```

### Visual: How Await Pauses

```
WITHOUT await:                WITH await:
─────────────────           ─────────────────
fetch(url)                   const response = await fetch(url)
  └─► continues              └─► WAITS HERE until fetch resolves
console.log("done")          console.log("done")

(Before data arrives!)       (After data arrives!)
```

### Parallel vs Sequential

```
SEQUENTIAL (await in loop):
┌─────────────────────────────────────┐
│  Task A ───────► Task B ───────► C  │
│  (waits for A)  (waits for B)       │
│  Total: 3000ms                      │
└─────────────────────────────────────┘

PARALLEL (Promise.all):
┌─────────────────────────────────────┐
│  Task A ─────┬──► C (combined)      │
│  Task B ─────┤    (after both)      │
│  Task C ─────┘                      │
│  Total: 1000ms                      │
└─────────────────────────────────────┘
```

```js
// ❌ SEQUENTIAL — SLOW (each waits for previous)
const users = await fetch("/api/users").then(r => r.json());
const posts = await fetch("/api/posts").then(r => r.json());

// ✅ PARALLEL — FAST (both start together)
const [users, posts] = await Promise.all([
  fetch("/api/users").then(r => r.json()),
  fetch("/api/posts").then(r => r.json())
]);
```

---

## 5. Fetch API — The HTTP Client

### How Fetch Works

```
┌─────────────────────────────────────────────────────────────┐
│                    FETCH REQUEST FLOW                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  fetch(url)  ──► Request ──► Server ──► Response ──► .json() │
│       │                          │                          │
│       │  (Response object)        │  (Parsed data)           │
│       │                           │                         │
│       ▼                           ▼                         │
│  { ok, status,                  { actual data }            │
│    headers,                      (plain JS object)          │
│    json(), ... }                                           │
│                                                             │
│  IMPORTANT: HTTP errors (404, 500) don't throw!             │
│  Need to check response.ok explicitly                        │
└─────────────────────────────────────────────────────────────┘
```

### Complete Fetch Pattern

```js
async function fetchUsers() {
  try {
    // 1. Make request
    const response = await fetch("/api/users", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        "Authorization": `Bearer ${token}`
      },
      body: JSON.stringify({ name: "Alice" })
    });

    // 2. Check for HTTP errors
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    // 3. Parse response
    const data = await response.json();
    return data;

  } catch (err) {
    console.error("Fetch failed:", err);
    throw err; // Re-throw so caller handles
  }
}
```

---

## 6. Common Patterns

### Optional Chaining — Safe Navigation

```js
// ❌ OLD — Manual null checks
const city = user && user.address && user.address.city;

// ✅ MODERN — Optional chaining
const city = user?.address?.city ?? "Unknown";
const fn = obj?.method?.();

// Array access
const item = arr?.[0];
```

### Nullish Coalescing — Default for null/undefined

```js
// ?? returns right side ONLY for null or undefined
const a = null ?? "default";   // "default"
const b = 0 ?? "default";      // 0 (0 is NOT null/undefined)
const c = "" ?? "default";     // "" (empty string is NOT null/undefined)
const d = false ?? "default";  // false (false is NOT null/undefined)
```

### Deep Clone — Safe Object Copying

```js
// ❌ SHALLOW COPY — nested objects share reference
const shallow = { ...original };
shallow.deep.nested = "changed"; // original.deep.nested also changes!

// ✅ DEEP CLONE — completely independent
const deep = JSON.parse(JSON.stringify(original));
const deep2 = structuredClone(original); // Modern alternative
```

---

## 📋 Quick Reference Card

| Concept | Remember |
|---------|----------|
| Event Loop | Stack must empty before callbacks run |
| Promises | 3 states: pending → fulfilled OR rejected |
| async/await | Syntactic sugar over promises |
| Parallel | `Promise.all([p1, p2])` for concurrent tasks |
| Error handling | Always `try/catch` with async/await |
| Fetch | `response.ok` check required for HTTP errors |
| Optional chaining | `?.` for safe property access |

---

## ✅ What TO Do

1. **Prefer `async/await` over `.then().catch()` chains** — More readable, easier to debug
2. **Use `Promise.all()` for parallel requests** — Don't wait sequentially when you can run concurrently
3. **Always check `response.ok` after `fetch`** — HTTP errors don't throw automatically
4. **Use `try/catch` blocks** — Wrap async operations for proper error handling
5. **Use `structuredClone()` for deep objects** — JSON parse/stringify is error-prone with Dates/functions

## ❌ What NOT To Do

1. **Don't nest promises** — Use `await` or proper `.then()` chaining instead of callbacks
2. **Don't forget `response.json()` returns a promise** — You need another `await` or `.then()`
3. **Don't use `==` in async code** — Stick to `===` always
4. **Don't forget `AbortController` for cancellable requests** — Prevents race conditions
5. **Don't mix callbacks and promises** — Pick one pattern per codebase

## 🏢 Workplace Wisdom

> "Async bugs are the hardest to debug because the code that fails isn't the code that shows the error. Always wrap your async code in try/catch and log which operation failed."
>
> "Promise.all() is not magic — if ANY promise rejects, the entire thing rejects. Use Promise.allSettled() when you want partial results."

[⬅️ Previous: 02 DOM-and-Events](../02-DOM-and-Events/README.md) | [⬅️ Back to Parent](../README.md)
