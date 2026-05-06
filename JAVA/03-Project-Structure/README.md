# 📁 03 — Project Structure

> *"Java doesn't allow loose files. If you have a Sum.java on your desktop, you're doing it wrong."*

---

## 📌 The Package System

Java has **one mandatory rule**: All code lives inside a **package**. Packages correspond to folder hierarchies, and the folder hierarchy must match the package name.

This isn't arbitrary — it's the foundation of:
- **Namespacing:** No conflicts between `com.company.User` and `com.client.User`
- **Encapsulation:** Package-private access (`default` modifier)
- **Autoloading:** The JDK knows where to find your classes

---

## 🏗️ Standard Maven/Gradle Project Structure

```
project/
│
├── pom.xml                  # Maven: build configuration
│   (or build.gradle         # Gradle: build configuration)
│
└── src/
    ├── main/
    │   ├── java/
    │   │   └── com/
    │   │       └── mycompany/
    │   │           └── myproject/
    │   │               │
    │   │               ├── Main.java              ← Entry point
    │   │               │
    │   │               ├── models/               ← Data entities
    │   │               │   ├── User.java
    │   │               │   ├── Product.java
    │   │               │   └── Order.java
    │   │               │
    │   │               ├── repositories/          ← Data access
    │   │               │   ├── UserRepository.java
    │   │               │   └── ProductRepository.java
    │   │               │
    │   │               ├── services/             ← Business logic
    │   │               │   ├── UserService.java
    │   │               │   └── OrderService.java
    │   │               │
    │   │               ├── controllers/          ← API endpoints
    │   │               │   └── UserController.java
    │   │               │
    │   │               └── utils/                 ← Helpers
    │   │                   └── DateUtils.java
    │   │
    │   └── resources/
    │       ├── application.yml          ← Config
    │       └── static/                  ← Frontend assets
    │
    └── test/
        └── java/
            └── com/
                └── mycompany/
                    └── myproject/
                        ├── services/
                        │   └── UserServiceTest.java
                        └── controllers/
                            └── UserControllerTest.java
```

---

## 📋 Layer Responsibilities

| Layer | Responsibility | What It Does | What It Does NOT |
|-------|---------------|---------------|------------------|
| **models/** | Data entities | Represent data (User, Order, Product) | No business logic, no I/O |
| **repositories/** | Data access | Fetch from / save to database | No business decisions |
| **services/** | Business logic | Validations, calculations, orchestration | No direct HTTP/SQL handling |
| **controllers/** | API layer | Handle HTTP requests/responses | No complex calculations |
| **utils/** | Helpers | Pure functions, conversions | No side effects |

---

## ✅ What TO Do

### DO: Follow the Naming Convention
```java
// Package declaration MUST match folder structure
package com.mycompany.myproject.services;

// This file lives at: src/main/java/com/mycompany/myproject/services/UserService.java
```

### DO: Use Lowercase Package Names
```java
// Good: lowercase, dot-separated
package com.mycompany.myproject.models;
package com.mycompany.myproject.repositories;
package netflix.recommendation.algorithms;

// Bad: mixed case (violates convention)
package com.MyCompany.MyProject.Models;
```

### DO: One Class Per File
```java
// User.java
public class User { }

// UserService.java (different class, different file)
public class UserService { }
```

### DO: Apply SOLID Principles to Package Structure

| Principle | What It Means for Structure |
|-----------|----------------------------|
| **S**ingle Responsibility | Each class has one job (UserService handles users, OrderService handles orders) |
| **O**pen/Closed | Add new features by adding classes, not modifying existing ones |
| **L**iskov Substitution | All service classes implementing their interface identically |
| **I**nterface Segregation | Small, focused interfaces (Serializable, Closeable) |
| **D**ependency Inversion | High-level modules don't depend on low-level modules |

---

## 🚫 What NOT to Do

### DON'T: Use Default Package
```java
// Bad: No package — causes classpath issues, no namespace
public class User { }

// Good: Proper package
package com.mycompany.myproject.models;
public class User { }
```

### DON'T: Put Everything in One Package
```java
// Bad: One giant package for 50 classes
package com.mycompany.myproject;  // 😱 Everything here

// User.java, Order.java, UserService.java, OrderService.java,
// Database.java, API.java, Utils.java, Config.java... total chaos
```

### DON'T: Mix Layers in Same Package
```java
// Bad: models package containing business logic
package com.mycompany.myproject.models;
public class User {
    public void sendEmail() {  // ❌ This is a service concern
        // email sending logic here
    }
}
```

### DON'T: Create Classes Longer Than 300 Lines
```java
// Bad: 2000-line class that does everything
public class MegaService { ... }

// Good: Split by responsibility
public class UserService { }      // User logic only
public class OrderService { }      // Order logic only
public class PaymentService { }   // Payment logic only
```

---

## 🎯 Why This Matters

### In the Workplace: Scalability
A 10-person team with a flat package structure constantly collides. "Who touched User.java?" With proper structure, you know exactly where to look and who owns what.

### In the Workplace: Onboarding
New developers can navigate a clean structure immediately. "Need to fix order logic?" → `services/OrderService.java`. No hunting through 50 files.

### In the Workplace: Testing
Clean layers mean tests target the right level. You test `UserService` by mocking `UserRepository`. You don't need a real database for service-layer tests.

---

## 🧠 Mental Model: The Hospital

| Hospital Department | Java Package |
|-------------------|--------------|
| Reception (intake) | `controllers/` — First point of contact |
| Triage nurses | `services/` — Business decisions |
| Medical records | `repositories/` — Data retrieval |
| Patient charts | `models/` — Data representation |
| Janitorial supplies | `utils/` — Helpers, not core logic |

A surgeon doesn't go find their own scalpel from a shared closet. Each department knows its domain and interacts with others through well-defined protocols.

---

## 📚 Technical Glossary

- **Package:** A namespace that organizes Java classes into logical groups, mapped to folder structure.
- **Maven/Gradle:** Build tools that manage dependencies, compilation, and packaging.
- **Layered Architecture:** Separating code by responsibility (controllers → services → repositories).
- **SRP (Single Responsibility Principle):** Each class should have one reason to change.
- **Classpath:** The set of paths where Java looks for compiled classes at runtime.

---

[⬅️ Previous: 02 Interfaces-and-Polymorphism](../02-Interfaces-and-Polymorphism/README.md) | [⬅️ Back to Parent](../README.md)
