# 🔌 02 — Interfaces and Polymorphism

> *"Program to an interface, not an implementation." — Gang of Four*

---

## 📌 Why Interfaces Exist

The problem with inheritance: **rigidity**.

When you `extends` a class, you form a tight bond. You can't make a class extend TWO classes (Java doesn't allow multiple inheritance). And if the parent changes, all children break.

**Interfaces** solve this by defining a **contract** — a promise of "if you implement me, you MUST provide these methods." But HOW those methods work is entirely up to you.

```java
// A contract: Anyone who says "I can be driven" must provide a drive() method
public interface Drivable {
    void drive();    // No body — the implementer writes the body
    void brake();    // Another promise
    int getSpeed();  // Promise to return current speed
}

// Now ANY class can implement this contract:
// A Car can be driven
public class Car implements Drivable {
    @Override public void drive() { System.out.println("Car driving"); }
    @Override public void brake() { System.out.println("Car braking"); }
    @Override public int getSpeed() { return 60; }
}

// A Robot can be driven (programmatically)
public class Robot implements Drivable {
    @Override public void drive() { System.out.println("Robot moving"); }
    @Override public void brake() { System.out.println("Robot stopping"); }
    @Override public int getSpeed() { return 10; }
}
```

---

## 🏗️ Interface Anatomy

```
┌─────────────────────────────────────────────────────────────────┐
│  INTERFACE (The Contract)                                       │
│                                                                 │
│  public interface Drivable {                                    │
│      // Abstract methods — no body                             │
│      void drive();                                              │
│      void brake();                                              │
│      int getSpeed();                                            │
│                                                                 │
│      // Java 8+: Default methods — with body                    │
│      default void info() {                                      │
│          System.out.println("Speed: " + getSpeed());           │
│      }                                                          │
│  }                                                              │
└─────────────────────────────────────────────────────────────────┘
                            │
                            │ implements
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│  CONCRETE IMPLEMENTATIONS (The Promise Keepers)                 │
│                                                                 │
│  Car ──────► implements Drivable                                │
│  │  drive(): "Car driving"                                     │
│  │  brake(): "Car braking"                                     │
│  │  getSpeed(): 60                                             │
│                                                                 │
│  Robot ────► implements Drivable                                │
│  │  drive(): "Robot moving"                                    │
│  │  brake(): "Robot stopping"                                  │
│  │  getSpeed(): 10                                             │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📋 Interface Features (Java 8+)

| Feature | What It Does | Example |
|---------|-------------|---------|
| **Abstract methods** | No body — implementing class must provide | `void drive();` |
| **Default methods** | Has body — inheritable by all implementers | `default void info() {...}` |
| **Static methods** | Belong to interface, not instances | `static Drivable create() {...}` |
| **Private methods** | Shared code between default methods (Java 9+) | `private void log() {...}` |

---

## ✅ What TO Do

### DO: Program to the Interface
```java
// Good: Using the interface type, not the implementation
List<String> names = new ArrayList<>();  // List interface, ArrayList implementation
Drivable vehicle = new Car();             // Drivable interface, Car implementation

// This function can now accept ANY Drivable — Car, Robot, Drone, etc.
public void transport(Drivable vehicle) {
    vehicle.drive();
    vehicle.brake();
}
```

### DO: Use Interfaces for Decoupling
```java
// Good: Dependent on abstraction, not concrete class
public class OrderService {
    private final PaymentProcessor paymentProcessor;  // Interface
    
    public OrderService(PaymentProcessor paymentProcessor) {
        this.paymentProcessor = paymentProcessor;
    }
}

// Now you can plug in any payment method:
// PayPal, Stripe, Bitcoin, FuturePaymentMethod
// Without changing OrderService
```

### DO: Single Responsibility for Interfaces
```java
// Good: Focused interface — one responsibility
public interface Serializable { void serialize(OutputStream out); }
public interface Closeable { void close(); }

// Bad: Jack-of-all-trades interface
public interface Everything { void doEverything(); }
```

---

## 🚫 What NOT to Do

### DON'T: Confuse Interfaces with Abstract Classes

| Aspect | Interface | Abstract Class |
|--------|-----------|---------------|
| Inheritance | A class can implement **many** interfaces | A class can extend **only one** abstract class |
| Methods | All abstract by default (before Java 8) | Can have both abstract and concrete |
| Fields | All `public static final` (constants) | Can have instance fields |
| Constructors | No constructors | Can have constructors |
| Use when | Defining a capability/contract | Defining a common base with shared state |

```java
// Interface: "Can do this" — capabilities
public interface Flyable { void fly(); }
public interface Swimmable { void swim(); }

// Abstract class: "Is a type of" — common base
public abstract class Vehicle {
    protected int wheels;
    public abstract void move();
}

// A AmphibiousCar can both fly AND swim
public class AmphibiousCar extends Vehicle implements Flyable, Swimmable {
    @Override public void fly() { /* Jet engines engage */ }
    @Override public void swim() { /* Water jets engage */ }
    @Override public void move() { /* Wheels turn */ }
}
```

### DON'T: Add Unnecessary Methods to Interfaces
```java
// Bad: Forcing ALL implementers to provide unused methods
public interface Printable {
    void print();
    void fax();  // Not all printers can fax!
}

// Good: Split into focused interfaces
public interface Printer { void print(); }
public interface FaxMachine { void fax(); }

// Now a simple printer only implements Printer
```

### DON'T: Use Implementation Inheritance When Composition Works
```java
// Bad: Tight coupling — Honda depends on Engine
public class Honda extends Engine { }  // ❌ IS-A Engine

// Good: Loose coupling — Honda HAS an Engine
public class Honda {
    private Engine engine;  // ✅ HAS-A Engine
    
    public Honda(Engine engine) {
        this.engine = engine;
    }
}
```

---

## 🎯 Why This Matters

### In the Workplace: Testability
Mocking interfaces is trivial. In tests, you inject a `FakePaymentProcessor` instead of Stripe. You test `OrderService` logic without touching the payment network.

### In the Workplace: Flexibility
Requirements change. You won't know every future requirement. With interfaces, you can add `CryptocurrencyPayment` without touching `OrderService`.

---

## 🧠 Mental Model: The Power Outlet

| Power Outlet Analogy | Interface Concept |
|---------------------|------------------|
| The outlet defines a contract (2 or 3 prongs, specific voltage) | Interface defines method signatures |
| Any device that matches the contract works (lamp, toaster, charger) | Any class implementing the interface works |
| You don't care HOW the device works internally | You don't care HOW the implementation works |
| The lamp doesn't extend the outlet | The class doesn't inherit the interface |

---

## 📚 Technical Glossary

- **Interface:** A contract specifying method signatures without implementation.
- **Implementation:** The concrete class that fulfills the interface contract.
- **Coupling:** The degree of interdependence between modules. Low coupling = good.
- **Decoupling:** Writing code that depends on abstractions, not concrete classes.
- **Dependency Injection:** Passing dependencies (like interfaces) into a class, rather than having the class create them.
- **Composition over Inheritance:** Prefer "has-a" relationships over "is-a" for flexibility.

---

[⬅️ Back to Parent](../README.md)
