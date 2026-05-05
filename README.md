# 📚 Learning Documentation — Master Index

> **Your deeply connected knowledge base.** This repository is a living space dedicated to collecting, organizing, and sharing my learning journey across technologies.

---

## 🎯 How to Navigate This Repository

Each topic below contains **3 sub-levels of deep-dive knowledge**. The structure is designed to take you from fundamentals to advanced integration.

```
ROOT (Master Index)
│
├── 🏛️ ARCHITECTURE_AND_BEST_PRACTICES
│   ├── 01-SOLID-Principles
│   ├── 02-Clean-Hexagonal
│   └── 03-System-Design
│
├── ☕ JAVA
│   ├── 01-OOP-Pillars
│   ├── 02-Interfaces-and-Polymorphism
│   └── 03-Project-Structure
│
├── 💛 JS
│   ├── 01-Core-Syntax-and-Types
│   ├── 02-DOM-and-Events
│   └── 03-Async-and-APIs
│
├── 🎨 CSS
│   ├── 01-Box-Model-and-Flow
│   ├── 02-Flexbox-and-Grid
│   └── 03-Responsive-Architecture
│
├── 🌐 HTML
│   ├── 01-Semantic-Web
│   ├── 02-Forms-and-Inputs
│   └── 03-SEO-and-Metadata
│
├── ⚛️ REACT+SPRINGBOOT
│   ├── 01-React-Core
│   ├── 02-Spring-Boot-API
│   └── 03-FullStack-Integration
│
├── 📱 IONIC+ANGULAR+FIREBASE+CAPACITOR
│   ├── 01-Angular-Architecture
│   ├── 02-Ionic-UI
│   └── 03-Firebase-Capacitor
│
├── 🐙 GIT
│   ├── 01-Git-Fundamentals
│   ├── 02-GitHub-Collaboration
│   └── 03-CI-CD-Workflows
│
├── 🐳 DOCKER
│   ├── 01-Images-and-Containers
│   ├── 02-Volumes-and-Storage
│   └── 03-Docker-Compose
│
├── 🌎 NETWORKING
│   ├── 01-Web-Protocols
│   ├── 02-Dev-Ports-and-Envs
│   └── 03-Security-and-JWT
│
├── 🐘 SQL
│   ├── 01-Relational-Design
│   ├── 02-Queries-and-Joins
│   └── 03-Indexes-and-Performance
│
└── 🎨 UX_UI_ACCESSIBILITY
    ├── 01-Visual-Design-Rules
    ├── 02-Accessibility-WCAG
    └── 03-Cognitive-Doc-Design
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

| # | Topic | Description | Subfolders |
|---|-------|-------------|------------|
| 01 | 🏛️ **[ARCHITECTURE_AND_BEST_PRACTICES](./ARCHITECTURE_AND_BEST_PRACTICES)** | SOLID, Clean Architecture, Hexagonal Architecture, System Design | 3 sublevels |
| 02 | ☕ **[JAVA](./JAVA)** | OOP Pillars, Interfaces and Polymorphism, Project Structure | 3 sublevels |
| 03 | 💛 **[JS](./JS)** | Core Syntax and Types, DOM and Events, Async and APIs | 3 sublevels |
| 04 | 🎨 **[CSS](./CSS)** | Box Model and Flow, Flexbox and Grid, Responsive Architecture | 3 sublevels |
| 05 | 🌐 **[HTML](./HTML)** | Semantic Web, Forms and Inputs, SEO and Metadata | 3 sublevels |
| 06 | ⚛️ **[REACT+SPRINGBOOT](./REACT+SPRINGBOOT)** | React Core, Spring Boot API, FullStack Integration | 3 sublevels |
| 07 | 📱 **[IONIC+ANGULAR+FIREBASE+CAPACITOR](./IONIC+ANGULAR+FIREBASE+CAPACITOR)** | Angular Architecture, Ionic UI, Firebase and Capacitor | 3 sublevels |
| 08 | 🐙 **[GIT](./GIT)** | Git Fundamentals, GitHub Collaboration, CI/CD Workflows | 3 sublevels |
| 09 | 🐳 **[DOCKER](./DOCKER)** | Images and Containers, Volumes and Storage, Docker Compose | 3 sublevels |
| 10 | 🌎 **[NETWORKING](./NETWORKING)** | Web Protocols, Dev Ports and Envs, Security and JWT | 3 sublevels |
| 11 | 🐘 **[SQL](./SQL)** | Relational Design, Queries and Joins, Indexes and Performance | 3 sublevels |
| 12 | 🎨 **[UX_UI_ACCESSIBILITY](./UX_UI_ACCESSIBILITY)** | Visual Design Rules, Accessibility WCAG, Cognitive Doc Design | 3 sublevels |

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