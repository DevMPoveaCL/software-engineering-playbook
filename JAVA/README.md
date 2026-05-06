# â˜• Java â€” Fundamentals and Object-Oriented Programming

Java is a strict, strongly typed, and 100% object-oriented language. Its core philosophy is *"Write Once, Run Anywhere"* â€” thanks to the Java Virtual Machine (JVM).

This section covers the key concepts to master Java, moving from simple scripts to a true architectural mindset.

---

## ðŸ“Š Objective Table: Java Analysis

| Aspect | Didactic Explanation |
|--------|----------------------|
| **What is it?** | A strict, strongly typed, 100% object-oriented language that runs on a Virtual Machine (JVM). |
| **Benefits** | "Write Once, Run Anywhere" â€” the same bytecode runs on any device with a JVM. Robust, type-safe, and enterprise-ready. |
| **When to use it?** | Enterprise applications, Android development, large-scale systems, or any project requiring strict type safety and maintainability. |
| **When NOT to use it?** | Simple scripts, rapid prototyping, or when startup time and memory usage are critical constraints. |

---

## ðŸ“š Learning Path

| Folder | Topic | What You'll Learn |
|--------|-------|-------------------|
| [01-OOP-Pillars](./01-OOP-Pillars/README.md) | The Four Pillars | Abstraction, Encapsulation, Inheritance, Polymorphism |
| [02-Interfaces-and-Polymorphism](./02-Interfaces-and-Polymorphism/README.md) | Interfaces | Contracts, polymorphism, and why composition beats inheritance |
| [03-Project-Structure](./03-Project-Structure/README.md) | Package Structure | Maven/Gradle layouts, layer responsibilities, and SOLID in practice |

---

## ðŸ§  OOP: Object-Oriented Programming Made Easy

OOP is a way of programming where we mimic the real world using "blueprints" and "physical things".

### 1. Classes vs. Objects
- **Class:** The blueprint or mold. Example: *The architectural plan of a house.*
- **Object:** The physical thing created from that mold. Example: *Your real house with a blue door and 3 windows.*

```java
// The Class (The Mold)
public class Cat {
    String name; // Attribute (characteristic)
    
    void meow() { // Method (action)
        System.out.println("Meow!");
    }
}

// The Object (The real creation)
Cat myCat = new Cat();
myCat.name = "Whiskers";
myCat.meow();
```

### 2. The 4 Pillars of OOP

1. **Abstraction:** Hide complex details and show only what matters.
   - *Analogy:* When driving a car, you only need to know how to use the steering wheel and accelerator. You don't need to understand fuel injection.
2. **Encapsulation:** Protect data (variables) inside a class so no one from outside can break them. That's why we make variables `private` and use `getters` and `setters`.
   - *Analogy:* Your bank account is encapsulated. You can't change your "balance" variable to a million directly. You must use the method (ATM) `deposit()`.
3. **Inheritance:** Create a new "Child" class based on a "Parent" to reuse code (DRY Principle).
   - *Analogy:* A `Cat` and a `Dog` inherit from `Animal`. Both breathe and eat, so we don't write that code twice.
4. **Polymorphism:** "Many forms". An object can behave differently depending on the situation.
   - *Analogy:* You have a universal remote (Interface). The "Power" button does something different if you point it at the TV (turns on the screen) vs. the AC (blows air). They share the same contract ("power on") but have different implementations.

---

## ðŸš¨ Best Practices and Structure in Java

Unlike Python or JS (where you can have loose files), **Java hates mess**.

### 1. Never Use Loose Files
If you have a `Sum.java` file loose on your desktop, you're using it wrong. Java is structured in **Packages**, which correspond to actual folders.

*Correct structure of a real project:*
```text
src/
 â””â”€â”€ main/
       â””â”€â”€ java/
            â””â”€â”€ com/
                 â””â”€â”€ mycompany/
                      â””â”€â”€ myproject/
                           â”œâ”€â”€ models/ (Your classes like Cat or Dog)
                           â”œâ”€â”€ services/ (Business logic)
                           â””â”€â”€ Main.java (Entry point)
```

### 2. Use Interfaces to Decouple Code
In professional Java, we rarely use deep direct inheritance (extends) because it couples us too tightly to the parent class. We prefer **Implementing Interfaces** (Composition over Inheritance).

- *Bad:* `public class Car extends FourWheeledMotorVehicle {...}`
- *Good:* `public class Car implements Drivable, Loadable {...}`

### 3. Access Modifiers (Golden Rule)
- **`public`**: Everyone sees it. Use it for methods (actions you want others to use).
- **`private`**: Only the same class sees it. Use it **ALWAYS** for attributes (variables). No one should directly touch your object's properties.
- **`protected`**: Child classes (inheritance) and classes in the same package see it.

> **Final Tip:** In Java, less is more. If a class exceeds 300 lines of code, it's probably violating the Single Responsibility Principle (SRP) from SOLID. Split it up.

---

## ðŸ“š Technical Glossary

- **JVM (Java Virtual Machine):** The engine that executes Java bytecode. It provides the "write once, run anywhere" capability.
- **Package:** A namespace that organizes classes into logical groups, mapped to folder structure.
- **Wrapper Class:** Classes like `Integer`, `Double` that wrap primitive types to make them behave like objects.
- **Interface:** A contract that defines what methods a class must implement, without specifying how.
- **SOLID:** Five principles (SRP, OCP, LSP, ISP, DIP) for writing maintainable, scalable OOP code.

---
### 🔗 Global Navigation
[⬅️ Previous Topic: Architecture](../ARCHITECTURE_AND_BEST_PRACTICES/README.md) | [🏠 Master Index](../README.md) | [➡️ Next Topic: JavaScript](../JS/README.md)
