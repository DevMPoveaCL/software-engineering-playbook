# 🧱 SOLID Principles

> **Why this matters:** SOLID principles are the "traffic rules" of object-oriented design. Follow them, and your code scales gracefully. Ignore them, and even simple changes become archaeological expeditions.

---

## 🧠 Mental Model: The Restaurant Kitchen

Imagine a restaurant kitchen without organization:

```
❌ CHAOS KITCHEN:
The chef also does the dishes, orders supplies, greets customers,
takes out trash, and manages finances.
When the chef is sick, the whole restaurant stops.

vs.

✅ ORGANIZED KITCHEN:
- Butcher → cuts meat (only meat)
- Sauce chef → makes sauces (only sauces)  
- Dishwasher → cleans dishes (only dishes)
- Manager → handles money (only money)

When the sauce chef is sick, the butcher keeps cutting meat.
The restaurant continues. Error isolated.
```

**SOLID principles** are the rules that enforce this organization in your code.

---

## 1️⃣ S — Single Responsibility Principle (SRP)

### The Rule

> A class should have **one and only one** reason to change.

### Analogy

The **butcher** should only cut meat. Not also handle:
- Customer complaints
- Inventory ordering
- Cleaning the floor

If you need to change "how we handle complaints," you touch the Manager class, NOT the Butcher class.

### ASCII: SRP Visualization

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   ❌ WRONG: God Class                                           │
│   ┌─────────────────────────────────────────────────────────┐   │
│   │                class UserManager                         │   │
│   │                                                          │   │
│   │   validateUser()     ← If validation logic changes       │   │
│   │   sendEmail()        ← If email sending changes         │   │
│   │   generateReport()   ← If reporting changes             │   │
│   │   calculateSalary()  ← If payroll changes              │   │
│   │   updateProfile()    ← If profile format changes        │   │
│   │                                                          │   │
│   │   Change ANYTHING → Touch this class → Test everything   │   │
│   └─────────────────────────────────────────────────────────┘   │
│                                                                  │
│   ✅ RIGHT: Separated Concerns                                  │
│   ┌──────────────┐  ┌──────────────┐  ┌──────────────┐       │
│   │class         │  │class         │  │class         │       │
│   │UserValidator │  │EmailService  │  │ReportGenerator│       │
│   │              │  │              │  │              │       │
│   │validate()    │  │send()        │  │generate()    │       │
│   └──────────────┘  └──────────────┘  └──────────────┘       │
│          │                 │                  │                   │
│          ▼                 ▼                  ▼                   │
│   ┌──────────────┐  ┌──────────────┐  ┌──────────────┐       │
│   │class         │  │class         │  │class         │       │
│   │PayrollCalculator│ │ProfileService│ │UserManager   │       │
│   │              │  │              │  │ (composes all)│       │
│   │calculate()   │  │update()     │  │              │       │
│   └──────────────┘  └──────────────┘  └──────────────┘       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 2️⃣ O — Open/Closed Principle (OCP)

### The Rule

> Code should be **open for extension** but **closed for modification**.

### Analogy

Your restaurant has a successful recipe. Instead of changing the original recipe every time you want to try something new, you:
1. Keep the original recipe intact
2. Create a **new variant** that extends it

### ASCII: OCP Visualization

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   ❌ WRONG: Modify existing code                                │
│                                                                  │
│   class PaymentProcessor {                                       │
│     processCard() { ... }                                        │
│     processCash() { ... }                                       │
│                                                                  │
│     // Adding PayPal? Gotta modify THIS class                   │
│     processPayPal() { ... }  ← Changes existing tested code    │
│   }                                                             │
│                                                                  │
│   ✅ RIGHT: Extend without modifying                           │
│                                                                  │
│              ┌──────────────────┐                               │
│              │ <<abstract>>      │                               │
│              │ PaymentMethod     │                               │
│              │ + process()       │                               │
│              └────────┬─────────┘                               │
│           ┌───────────┴───────────┐                              │
│           │                       │                              │
│   ┌───────▼───────┐      ┌───────▼───────┐                      │
│   │ CardPayment   │      │ CashPayment   │                      │
│   │ + process()   │      │ + process()   │                      │
│   └───────────────┘      └───────────────┘                      │
│                                                                  │
│   // Adding PayPal? CREATE NEW CLASS, don't touch existing     │
│   ┌─────────────────┐                                            │
│   │ PayPalPayment  │                                            │
│   │ + process()    │                                            │
│   └─────────────────┘                                            │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 3️⃣ L — Liskov Substitution Principle (LSP)

### The Rule

> Objects of a **superclass** should be replaceable with objects of its **subclasses** without breaking the application.

### Analogy

If you have a recipe template that says "calculateArea()", a Square and a Circle both inherit from Shape — both should be able to calculate their area correctly. You shouldn't be able to pass a Triangle that can't calculate area correctly.

### ASCII: LSP Violation

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   ❌ VIOLATION: Subclass that can't do what parent promises     │
│                                                                  │
│   class Bird {                                                   │
│     fly() { "Flying!" }                                          │
│   }                                                             │
│                                                                  │
│   class Penguin extends Bird {                                   │
│     fly() { throw new Error("I can't fly!"); }  ← LSP VIOLATION │
│   }                                                             │
│                                                                  │
│   function makeBirdFly(bird: Bird) {                            │
│     bird.fly();  // Works with Bird, breaks with Penguin       │
│   }                                                             │
│                                                                  │
│   ✅ CORRECT: Model reality accurately                         │
│                                                                  │
│   class Bird {                                                   │
│     eat() { }  // All birds can eat                             │
│   }                                                             │
│                                                                  │
│   class FlyingBird extends Bird {                               │
│     fly() { }                                                    │
│   }                                                             │
│                                                                  │
│   class Penguin extends Bird {                                  │
│     // Penguins don't fly - that's fine!                        │
│   }                                                             │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 4️⃣ I — Interface Segregation Principle (ISP)

### The Rule

> It's better to have **many small, specific interfaces** than one large, general-purpose interface.

### Analogy

Don't force every restaurant worker to implement skills they don't need:

❌ `interface RestaurantWorker` with methods: `cutMeat()`, `cookFood()`, `serveTable()`, `cleanFloor()`
→ A cashier is forced to have empty implementations for `cutMeat()` and `cleanFloor()`

✅ Split into: `MeatCutter`, `Cook`, `Server`, `Cleaner`

### ASCII: ISP Visualization

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   ❌ WRONG: Fat interface                                        │
│                                                                  │
│   interface Worker {                                             │
│     cutMeat()                                                    │
│     cookFood()                                                   │
│     serveTable()                                                 │
│     cleanFloor()                                                 │
│   }                                                             │
│                                                                  │
│   class Cashier implements Worker {                             │
│     cutMeat() {}    // Empty! Waste                              │
│     cookFood() {}  // Empty! Waste                              │
│     serveTable() { /* actually used */ }                        │
│     cleanFloor() {}  // Empty! Waste                            │
│   }                                                             │
│                                                                  │
│   ✅ RIGHT: Segregated interfaces                               │
│                                                                  │
│   interface MeatCutter { cutMeat() }                            │
│   interface Cook { cookFood() }                                │
│   interface Server { serveTable() }                            │
│   interface Cleaner { cleanFloor() }                           │
│                                                                  │
│   class Cashier implements Server {                             │
│     serveTable() { /* only what I need */ }                     │
│   }                                                             │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 5️⃣ D — Dependency Inversion Principle (DIP)

### The Rule

> High-level modules should **not depend on low-level modules**. Both should depend on **abstractions**.

### Analogy

Your light bulb shouldn't be **permanently soldered** to your house's electrical system:

❌ Direct dependency: `Bulb → HouseWiring` (can't change bulb without changing wiring)

✅ Abstraction: `Bulb → StandardSocket ← HouseWiring` (change either without affecting the other)

### ASCII: DIP Visualization

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   ❌ WRONG: Direct dependency                                    │
│                                                                  │
│   ┌──────────────────┐      ┌──────────────────┐               │
│   │   HighLevel      │      │    LowLevel       │               │
│   │   SalesService   │─────▶│    MySQLDatabase │               │
│   │   (Business     │      │   (Concrete)      │               │
│   │    Logic)       │      │                   │               │
│   └──────────────────┘      └──────────────────┘               │
│         │                                                       │
│         │ If MySQL fails and you switch to MongoDB:             │
│         │ → Rewrite SalesService (BAD!)                        │
│         │                                                       │
│   ✅ RIGHT: Depend on abstractions                              │
│                                                                  │
│   ┌──────────────────┐      ┌──────────────────┐               │
│   │   HighLevel      │      │    <<interface>> │               │
│   │   SalesService   │─────▶│   DatabasePort    │               │
│   │   (Business     │      │                   │               │
│   │    Logic)       │      └─────────┬─────────┘               │
│   └──────────────────┘                │                         │
│                      ┌─────────────────┼─────────────────┐      │
│                      ▼                 ▼                 ▼      │
│               ┌──────────┐     ┌──────────┐     ┌──────────┐  │
│               │  MySQL   │     │ MongoDB  │     │ Postgres │  │
│               │ Adapter  │     │ Adapter  │     │ Adapter  │  │
│               └──────────┘     └──────────┘     └──────────┘  │
│                                                                  │
│         Change database? Just swap the adapter. No business     │
│         logic changes needed.                                    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📊 SOLID Summary Table

| Principle | Rule | Analogy |
|-----------|------|---------|
| **S** Single Responsibility | One reason to change | Butcher cuts meat only |
| **O** Open/Closed | Extend, don't modify | Recipe variants |
| **L** Liskov Substitution | Subclasses replace parents | All birds can eat |
| **I** Interface Segregation | Small interfaces | Cashier only serves |
| **D** Dependency Inversion | Depend on abstractions | Bulb ↔ socket |

---

## ✅ What TO Do

| Do This | Why |
|---------|-----|
| **Ask "what changes this class?"** | If multiple answers, it has multiple responsibilities |
| **Prefer composition over inheritance** | Composition is more flexible |
| **Use dependency injection** | Makes DIP achievable |
| **Write interfaces for consumers** | Consumer defines what it needs |
| **Refactor toward SRP when it hurts** | Don't over-engineer upfront |

---

## 🚫 What NOT To Do

| Don't Do This | Why Not |
|---------------|---------|
| **Don't create God Classes** | Every change risks breaking everything |
| **Don't modify tested code unnecessarily** | Risk of regressions |
| **Don't force classes to implement unused methods** | Violates ISP |
| **Don't hardcode dependencies** | Makes swapping hard |
| **Don't apply SOLID prematurely** | Start simple, refactor when needed |

---

[⬅️ Back to Parent](../README.md) | [➡️ Next: 02 Clean-Hexagonal](../02-Clean-Hexagonal/README.md)
