# 📚 Learning Documentation — Master Index

> **Your deeply connected knowledge base.** This repository is a living space dedicated to collecting, organizing, and sharing my learning journey across technologies.

---

## 🎯 How to Navigate This Repository

Each topic below contains **3 sub-levels of deep-dive knowledge**. The structure is designed to take you from fundamentals to advanced integration.

```
ROOT (Master Index)
│
├── Architecture & Best Practices
│   ├── 01 - SOLID & Clean Code Principles
│   ├── 02 - Design Patterns
│   └── 03 - Hexagonal Architecture
│
├── UX, UI & Accessibility
│   ├── 01 - Design Fundamentals
│   ├── 02 - WCAG & ADA Standards
│   └── 03 - Inclusive Design
│
├── ⚛️ React + SpringBoot (FullStack)
│   ├── 01 - React Core (Hooks, State, Components)
│   ├── 02 - Spring Boot API (REST, JWT, JPA)
│   └── 03 - FullStack Integration
│
└── 📱 Ionic + Angular + Firebase + Capacitor (Mobile)
    ├── 01 - Angular Architecture (MVC, DI, Guards)
    ├── 02 - Ionic UI (Platform-Adaptive Components)
    └── 03 - Firebase + Capacitor (BaaS + Native Wrapper)
```

---

## 🧠 Development Principles and Clean Code

To make the code we write professional, easy to maintain, and readable by others, we apply certain principles:

### 1. KISS (Keep It Simple, Stupid)
- **What is it?** Code should be as simple as possible.
- **Why?** Simple code is easier to read, understand, and fix when it breaks.
- **Example**: 
  - ❌ *Bad:* 10 complex functions to calculate a discount.
  - ✅ *Good:* One clear function: `return price - (price * discount);`.

### 2. DRY (Don't Repeat Yourself)
- **What is it?** Avoid writing the same logic in different places.
- **Why?** Changes need to be made in only one location.
- **Example**: A shared `calculateVAT(price)` function instead of `price * 0.21` everywhere.

### 3. YAGNI (You Aren't Gonna Need It)
- **What is it?** Don't program features "just in case."
- **Why?** You're investing time in complexity you'll never use.
- **Example**: Building a blog → don't add multi-language and payment systems yet.

### 4. Clean Code
- **What is it?** Write code for humans, not machines.
- **Why?** Facilitates teamwork and future improvements.
- **Example**: 
  - ❌ *Bad:* `let x = 10;` (What is x?)
  - ✅ *Good:* `let numberOfDays = 10;` (Self-explanatory)

### 5. SOLID (5 Principles for Robust Design)

| Principle | What It Means | Simple Analogy |
|-----------|--------------|----------------|
| **S** - Single Responsibility | One class, one job | A chef only cooks, doesn't serve |
| **O** - Open/Closed | Open for extension, closed for modification | Add new recipes without changing the chef |
| **L** - Liskov Substitution | Subtypes must be substitutable | A Duck that inherits from Bird must fly |
| **I** - Interface Segregation | Many specific interfaces > one general | Don't force Receptionist to writeCode() |
| **D** - Dependency Inversion | Depend on abstractions, not concretions | Car depends on "Engine", not "ToyotaEngine" |

---

## 🗂️ Repository Structure

### 🎯 Core Topics

| Topic | Description | Deep Dive Levels |
|-------|-------------|------------------|
| 🏛️ **[Architecture and Best Practices](./ARCHITECTURE_AND_BEST_PRACTICES)** | SOLID, DRY, KISS, TDD, Clean Architecture and Hexagonal Architecture | 3 subfolders |
| 🎨 **[UX, UI and Accessibility](./UX_UI_ACCESSIBILITY)** | Beautiful, legal, and inclusive interfaces (WCAG, ADA standards) | Multiple guides |
| ☕ **[JAVA](./JAVA)** | Object-oriented and basic projects | Variables, loops, OOP |
| 💛 **[JavaScript](./JS)** | Array methods, DOM manipulation, fundamentals | Core concepts |
| ⚛️ **[React + SpringBoot](./REACT+SPRINGBOOT)** | FullStack applications | **3 sublevels** |
| 🐳 **[Docker](./DOCKER)** | Command sheets for containers | Packaging apps |
| 🌐 **[Networking](./NETWORKING)** | Network concepts, URLs and HTTP status codes | OSI model |
| 🐙 **[Git](./GIT)** | Visual commands and best practices | Commits, branches |
| 📱 **[Ionic + Angular](./IONIC+ANGULAR+FIREBASE+CAPACITOR)** | Mobile apps with web technologies | **3 sublevels** |
| 🎨 **[HTML & CSS](./HTML)** | Web structure and styles | Flexbox, Grid |
| 🐘 **[SQL](./SQL)** | Relational databases | Queries, joins |

---

## 🔑 Key Architecture Diagrams

### FullStack Request Flow
```
┌─────────────┐      HTTP/JWT       ┌──────────────────┐      SQL       ┌─────────────────┐
│   React     │ ◄──────────────────► │  Spring Boot     │ ◄────────────► │  PostgreSQL/    │
│  Frontend   │     JSON REST       │  Backend API     │                │  MySQL          │
│  (SPA)      │                     │  (Java)          │                │  (Relational)   │
└─────────────┘                     └──────────────────┘                └─────────────────┘
```

### Angular Module Architecture
```
┌─────────────────────────────────────────────────────────────────┐
│                        AppModule                                 │
├────────────────┬────────────────────────┬───────────────────────┤
│    Core        │        Shared         │      Feature          │
│   Module       │        Module         │      Modules          │
│                │                        │                       │
│  - Auth Guard  │  - Reusable Components │  - Dashboard          │
│  - Singleton   │  - Shared Pipes        │  - Products            │
│    Services    │  - Common Directives   │  - Orders              │
└────────────────┴────────────────────────┴───────────────────────┘
```

---

## 🚀 How to Learn From Here

1. **Clone the repository**: `git clone` to your computer
2. **Pick a topic**: Start with your technology of interest
3. **Read the sub-levels**: Each topic has 3 folders for progressive depth
4. **Apply the principles**: Notice how SOLID, DRY, KISS appear everywhere

---

> 📌 **Final Note:** "Learning is not a destination, it's a journey." This repository is a constantly evolving tool, designed to document that continuous learning journey. If you find errors, open an Issue or PR!

---

*Created with 💻 and focused on Clean Code by [Marco Povea](https://github.com/DevMPoveaCL).*
