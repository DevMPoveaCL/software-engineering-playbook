# 🏛️ 01 — The OOP Pillars

> *"Abstraction hides the engine. Encapsulation locks the hood. Inheritance builds the family tree. Polymorphism speaks one language, many dialects."*

---

## 📌 The Four Pillars, Explained

Object-Oriented Programming has four fundamental principles that separate it from procedural code. Ignore any one of them, and your code becomes a tangled mess.

Think of them as the **four load-bearing walls** of a building. Remove one, and the structure weakens.

---

## 🏗️ Pillar 1: Abstraction

**The principle:** Hide complexity behind a simple interface. Users interact with the facade, not the machinery.

### Real-World Analogy: The ATM

When you use an ATM, you:
1. Insert your card
2. Enter your PIN
3. Select "Withdraw $100"
4. Take the cash

You **don't** interact with the vault mechanism, the cash dispenser belt, or the database of balances. The ATM abstracts all of that away behind a simple screen and buttons.

```java
// Good: Simple, focused interface
public interface ATM {
    void insertCard(Card card);
    void enterPin(int pin);
    void withdraw(double amount);
    double checkBalance();
}

// You don't need to know HOW the ATM counts bills or queries the bank
// The interface hides all of that
```

### What TO Do
```java
// Good: Simple public interface, complex private implementation
public class BankAccount {
    private List<Transaction> transactions;  // private (hidden)
    private double balance;
    
    // Public API — simple and focused
    public void deposit(double amount) {
        validateAmount(amount);
        balance += amount;
        logTransaction(amount, DEPOSIT);
    }
    
    public void withdraw(double amount) {
        validateAmount(amount);
        balance -= amount;
        logTransaction(amount, WITHDRAW);
    }
    
    public double getBalance() { return balance; }
}
```

### What NOT to Do
```java
// Bad: Leaking internals, forcing users to understand complexity
public class BankAccount {
    public double balance;  // Exposed! Anyone can set it directly
    public List<Transaction> transactions;  // Users can modify!
    // Now every user has to understand how transactions work
}
```

---

## 🏗️ Pillar 2: Encapsulation

**The principle:** Bundle data (state) and methods (behavior) together. Protect the data from external interference.

### Real-World Analogy: The Bank Account

Your bank account balance is `private`. You can't walk up to the bank's memory and change your balance from `$100` to `$1,000,000`. You must go through the **methods** (deposit, withdraw, transfer).

This protects against:
- Invalid values (negative balance)
- Race conditions (two simultaneous withdrawals)
- Audit failures (every transaction logged)

```java
// Good: Encapsulated — balance is protected
public class BankAccount {
    private double balance;  // private = protected
    
    public void deposit(double amount) {
        if (amount <= 0) throw new IllegalArgumentException("Must be positive");
        balance += amount;
    }
}

// Usage
account.deposit(100);  // ✅ Goes through the proper method
// account.balance = -999999;  // ❌ Can't do this — private
```

### What TO Do
```java
// Good: Private fields, public methods (getters/setters only where needed)
public class User {
    private String name;
    private String email;
    
    public String getName() { return name; }  // Read-only access
    public void setEmail(String email) {
        if (!email.contains("@")) throw new IllegalArgumentException("Invalid email");
        this.email = email;
    }
}
```

### What NOT to Do
```java
// Bad: No encapsulation — data exposed
public class User {
    public String name;  // Anyone can modify directly
    public String email;
}

// Now anywhere in your code:
// user.name = null;  // Breaks everything
// user.email = "not-an-email";  // Invalid data enters the system
```

---

## 🏗️ Pillar 3: Inheritance

**The principle:** Create a new class (child) by reusing and extending an existing class (parent). "Is-a" relationship.

### Real-World Analogy: The Animal Kingdom

```
                    ANIMAL (Abstract Parent)
                         │
          ┌───────────────┼───────────────┐
          │               │               │
        CAT             DOG            BIRD
     (meow(),       (bark(),         (fly(),
      scratch())     fetch())          chirp())
```

A `Cat` **IS AN** `Animal`. A `Dog` **IS AN** `Animal`. They inherit:
- The ability to breathe (`animal.breathe()`)
- The ability to eat (`animal.eat()`)

But they provide their own implementations:
- `Cat.sound()` → "Meow"
- `Dog.sound()` → "Woof"

### Implementation
```java
// Parent class
public abstract class Animal {
    protected String name;  // protected = accessible to children
    
    public void eat() {
        System.out.println(name + " is eating...");
    }
    
    public abstract String sound();  // Abstract = must override
}

// Child classes
public class Cat extends Animal {
    @Override
    public String sound() {
        return "Meow!";
    }
}

public class Dog extends Animal {
    @Override
    public String sound() {
        return "Woof!";
    }
}

// Usage
Animal cat = new Cat();
cat.name = "Whiskers";
cat.eat();        // "Whiskers is eating..."
cat.sound();      // "Meow!"
```

### What NOT to Do (Excessive Inheritance)
```java
// Bad: Deep hierarchical inheritance — the "God class" problem
public class Vehicle {}
public class MotorVehicle extends Vehicle {}
public class Car extends MotorVehicle {}
public class SportsCar extends Car {}
public class Ferrari extends SportsCar {}

// Every change in Vehicle ripples down. Hard to test, hard to change.
// Composition over Inheritance is the preferred pattern.
```

---

## 🏗️ Pillar 4: Polymorphism

**The principle:** "Many forms." The same message (method call) produces different behaviors depending on the object type.

### Real-World Analogy: The Universal Remote

Your TV remote has a "Power" button. But **what** it powers on depends on the device:
- Point at TV → turns on the TV
- Point at AC → turns on the air conditioner

Same button (interface). Different behavior (implementation).

### Types of Polymorphism

**1. Compile-time (Overloading)**
Same method name, different parameters:
```java
public class Calculator {
    int add(int a, int b) { return a + b; }
    double add(double a, double b) { return a + b; }  // Same name, different type
    int add(int a, int b, int c) { return a + b + c; } // Same name, more params
}
```

**2. Runtime (Overriding)**
Same method signature, different behavior based on object type:
```java
public abstract class Shape {
    abstract double area();  // Same method signature
}

public class Circle extends Shape {
    double radius;
    @Override double area() { return Math.PI * radius * radius; }
}

public class Square extends Shape {
    double side;
    @Override double area() { return side * side; }
}

// Usage
Shape s1 = new Circle(2);
Shape s2 = new Square(4);

s1.area();  // 12.57 (circle calculation)
s2.area();  // 16 (square calculation)
```

---

## 🎯 Why This Matters

### In the Workplace: Maintainability
Code that uses the four pillars is **self-documenting**. When someone sees `cat.sound()`, they know exactly what it does. The class structure reveals the domain model.

### In the Workplace: Testability
Encapsulated classes with clean interfaces are easy to mock in tests. You test the interface, not the internals.

### In the Workplace: Reusability
Inheritance and polymorphism mean you're not writing the same code twice. One bug fix in `Animal` propagates to all subclasses — but only for inherited behavior, not overridden methods.

---

## 🧠 Mental Model: The Car Factory

| Car Factory Concept | OOP Pillar |
|--------------------|------------|
| Blueprints (Audi A4, A6, A8 share a platform) | Inheritance (shared parent class) |
| Driver only knows steering wheel + pedals | Abstraction (simple interface) |
| Engine is under the hood, not exposed to driver | Encapsulation (hidden internals) |
| Same gas pedal → different acceleration per model | Polymorphism (same call, different result) |

---

## 📚 Technical Glossary

- **Abstraction:** Hiding implementation details behind a simple public interface.
- **Encapsulation:** Bundling data and methods, restricting direct access to state.
- **Inheritance:** A child class reusing and extending a parent's behavior ("is-a" relationship).
- **Polymorphism:** Same interface, different behavior. "Many forms."
- **Overloading:** Same method name, different parameters (compile-time).
- **Overriding:** Same method signature, different behavior (runtime).
- **Composition:** Building complex objects from simpler ones ("has-a" relationship). Preferred over deep inheritance.

---

[⬅️ Back to Parent](../README.md) | [➡️ Next: 02 Interfaces-and-Polymorphism](../02-Interfaces-and-Polymorphism/README.md)
