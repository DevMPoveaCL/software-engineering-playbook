# 🏗️ Clean & Hexagonal Architecture

> **Why this matters:** These architectures protect your business logic from the chaos of the outside world — frameworks change, databases switch, UIs evolve, but your core rules should remain eternal.

---

## 🧠 Mental Model: The Restaurant Metaphor

### Clean Architecture = Kitchen Organization

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   CLEAN ARCHITECTURE IS:                                         │
│   The internal kitchen organization. Who gives orders to whom,  │
│   and how recipes are protected from external influences.       │
│                                                                  │
│   ┌─────────────────────────────────────────────────────────┐     │
│   │                    THE KITCHEN                          │     │
│   │                                                          │     │
│   │  ┌─────────────────────────────────────────────────┐    │     │
│   │  │           CENTER: THE SECRET RECIPE              │    │     │
│   │  │     (Business Rules - What makes us money)       │    │     │
│   │  │                                                  │    │     │
│   │  │   Nobody from outside can change this recipe    │    │     │
│   │  │   just because they brought different ingredients│    │     │
│   │  └─────────────────────────────────────────────────┘    │     │
│   │                    ▲                                       │     │
│   │                    │ (Order flows inward)                 │     │
│   │                    │                                       │     │
│   │  ┌─────────────────────────────────────────────────┐    │     │
│   │  │         USE CASES (Preppers)                    │    │     │
│   │  │   Transform raw input into recipe instructions  │    │     │
│   │  └─────────────────────────────────────────────────┘    │     │
│   │                    ▲                                       │     │
│   │                    │                                       │     │
│   │  ┌─────────────────────────────────────────────────┐    │     │
│   │  │         INTERFACES (Doors/Windows)               │    │     │
│   │  │   What comes in: orders, queries, commands     │    │     │
│   │  └─────────────────────────────────────────────────┘    │     │
│   │                    ▲                                       │     │
│   │                    │                                       │     │
│   │  ┌─────────────────────────────────────────────────┐    │     │
│   │  │    FRAMEWORKS (Ovens, Pans, delivery apps)     │    │     │
│   │  │   External tools that we use but don't depend on│    │     │
│   │  └─────────────────────────────────────────────────┘    │     │
│   │                                                          │     │
│   └─────────────────────────────────────────────────────────┘     │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Hexagonal Architecture = Universal Service Windows

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   HEXAGONAL ARCHITECTURE IS:                                      │
│   The restaurant's universal service windows. No matter how      │
│   food arrives (UberEats, phone, in-person), it enters the       │
│   kitchen through the same port.                                 │
│                                                                  │
│                    ┌─────────────────┐                           │
│                    │                 │                           │
│                    │   MAIN DISH     │                           │
│                    │   (Business     │                           │
│                    │    Logic)       │                           │
│                    │                 │                           │
│                    └────────┬────────┘                           │
│                             │                                    │
│              ┌──────────────┼──────────────┐                     │
│              │              │              │                     │
│         ┌────▼────┐    ┌────▼────┐    ┌────▼────┐                │
│         │ Order   │    │ Payment │    │Delivery │                │
│         │ Port    │    │ Port    │    │ Port    │                │
│         └────┬────┘    └────┬────┘    └────┬────┘                │
│              │              │              │                     │
│         ┌────▼────┐    ┌────▼────┐    ┌────▼────┐                │
│         │ Uber    │    │ Stripe  │    │ PedidosYa│                │
│         │ Adapter │    │ Adapter │    │ Adapter  │                │
│         └─────────┘    └─────────┘    └─────────┘                │
│                                                                  │
│   Change delivery partner? Unplug Uber, plug in PedidosYa.       │
│   The kitchen never knew or cared how food arrived.             │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔄 Clean vs. Hexagonal: Together, Not Opposed

### ASCII: How They Relate

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   CLEAN ARCHITECTURE = "Inside Out"                              │
│   Focus: Protecting the CORE (business rules)                   │
│   Key Question: "How do we shield the chef from distractions?"    │
│                                                                  │
│   HEXAGONAL ARCHITECTURE = "Outside In"                           │
│   Focus: Connecting any EXTERNAL THING to the CORE              │
│   Key Question: "How do we connect anything to our system?"      │
│                                                                  │
│   ┌─────────────────────────────────────────────────────────┐    │
│   │                    YOUR APPLICATION                     │    │
│   │                                                          │    │
│   │   ┌────────────────────────────────────────────────┐    │    │
│   │   │  HEXAGONAL: External Connections (Ports)         │    │    │
│   │   │                                                  │    │    │
│   │   │    [WebAPI]  [MobileAPI]  [ScheduledJobs]       │    │    │
│   │   │         │           │           │               │    │    │
│   │   └─────────┼───────────┼───────────┼───────────────┘    │    │
│   │             │           │           │                    │    │
│   │   ┌─────────▼───────────▼───────────▼───────────────┐    │    │
│   │   │  CLEAN: Use Cases (Application Services)        │    │    │
│   │   │                                                  │    │    │
│   │   │    [CreateOrder]  [ProcessPayment]  [Notify]    │    │    │
│   │   │            │              │             │        │    │    │
│   │   └────────────┼──────────────┼─────────────┼────────┘    │    │
│   │                │              │             │           │    │
│   │   ┌─────────────▼──────────────▼─────────────▼────────┐   │    │
│   │   │  CLEAN: Domain (Business Rules - Sacred)         │   │    │
│   │   │                                                  │   │    │
│   │   │    [Order]  [Payment]  [User]  [Inventory]       │   │    │
│   │   │                                                  │   │    │
│   │   └────────────────────────────────────────────────┘    │    │
│   │                                                          │    │
│   └─────────────────────────────────────────────────────────┘    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🏛️ Clean Architecture Layers

### The Dependency Rule

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   Dependencies flow INWARD only.                                 │
│   Inner layers NEVER know about outer layers.                    │
│                                                                  │
│   ┌─────────────────────────────────────────────────────────┐    │
│   │   LAYER 4: Frameworks & UI                              │    │
│   │   (React, Express, Spring Boot, Django)                 │    │
│   │   - Web controllers                                     │    │
│   │   - UI components                                      │    │
│   │                        │                               │    │
│   │                        ▼ (depends on)                  │    │
│   │   ┌─────────────────────────────────────────────────┐│    │
│   │   │   LAYER 3: Interfaces & Ports                     ││    │
│   │   │   - REST controllers                             ││    │
│   │   │   - Repository interfaces                       ││    │
│   │   │                        │                       ││    │
│   │   │                        ▼ (depends on)          ││    │
│   │   │   ┌─────────────────────────────────────────┐││    │
│   │   │   │   LAYER 2: Use Cases (Application)        │││    │
│   │   │   │   - "CreateOrderUseCase"                 │││    │
│   │   │   │   - "ProcessPaymentUseCase"              │││    │
│   │   │   │                        │               │││    │
│   │   │   │                        ▼ (depends on)  │││    │
│   │   │   │   ┌─────────────────────────────────┐│││    │
│   │   │   │   │   LAYER 1: Domain (Sacred Core)   ││││    │
│   │   │   │   │   - Entities                     │││    │
│   │   │   │   │   - Business Rules              │││    │
│   │   │   │   │   - NO external dependencies    │││    │
│   │   │   │   └─────────────────────────────────┘││    │
│   │   │   └─────────────────────────────────────────┘│    │
│   │   └─────────────────────────────────────────────────┘    │
│   └─────────────────────────────────────────────────────────┘    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## ⚡ Ports and Adapters (Hexagonal)

### Input vs. Output Ports

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   INPUT PORTS (Driving, Inward)                                 │
│   ─────────────────────────────────────                         │
│   Defines WHAT the outside world can ask our system to do        │
│                                                                  │
│   interface CreateOrderPort {                                   │
│     createOrder(orderData: OrderDTO): Order                     │
│   }                                                             │
│                                                                  │
│   Usage: Called by controllers, web handlers, API endpoints      │
│                                                                  │
│   ┌─────────────────────────────────────────────────────────┐   │
│   │                                                          │   │
│   │   [API Controller] ──calls──▶ [CreateOrderPort]          │   │
│   │                                                          │   │
│   └─────────────────────────────────────────────────────────┘   │
│                                                                  │
│   OUTPUT PORTS (Driven, Outward)                                 │
│   ─────────────────────────────────────                         │
│   Defines WHAT our system needs from the outside world          │
│                                                                  │
│   interface OrderRepositoryPort {                               │
│     save(order: Order): void                                    │
│     findById(id: string): Order                                │
│   }                                                             │
│                                                                  │
│   interface NotificationPort {                                   │
│     sendEmail(to: string, template: string): void              │
│   }                                                             │
│                                                                  │
│   Usage: Called by use cases to persist or notify               │
│                                                                  │
│   ┌─────────────────────────────────────────────────────────┐   │
│   │                                                          │   │
│   │   [UseCase] ──calls──▶ [OrderRepositoryPort]              │   │
│   │                        │                                  │   │
│   │              ┌─────────┴─────────┐                       │   │
│   │              │                   │                       │   │
│   │         [Postgres    [MongoDB                           │   │
│   │          Adapter]    Adapter]                           │   │
│   │                                                          │   │
│   └─────────────────────────────────────────────────────────┘   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## ✅ What TO Do

| Do This | Why |
|---------|-----|
| **Keep domain pure** | No dependencies on frameworks = easy testing |
| **Define ports first** | Consumer (use case) defines what it needs |
| **Adapters are plugins** | Swap database without touching business logic |
| **Dependencies point inward** | Outer layers depend on inner, never vice versa |
| **Test business logic without UI** | Test the core directly |

---

## 🚫 What NOT To Do

| Don't Do This | Why Not |
|---------------|---------|
| **Don't mix domain with infrastructure** | `OrderService` should not know about SQL |
| **Don't let frameworks bleed into domain** | JPA annotations in domain = tight coupling |
| **Don't skip the dependency rule** | If inner depends on outer, you lost the battle |
| **Don't over-engineer simple apps** | KISS — if it's a 2-week MVP, use Rails/Django standard |
| **Don't create ports for everything** | Only create when you have multiple adapters |

---

## 📊 When to Use What

| Scenario | Architecture |
|----------|-------------|
| Complex business rules (banking, insurance) | Clean Architecture |
| Many external integrations (payment providers) | Hexagonal Architecture |
| MVP / simple CRUD app | Standard MVC / Rails/Django |
| Both complex rules AND many integrations | Both (most real apps) |

---

[⬅️ Previous: 01 SOLID-Principles](../01-SOLID-Principles/README.md) | [⬅️ Back to Parent](../README.md) | [➡️ Next: 03 System-Design](../03-System-Design/README.md)
