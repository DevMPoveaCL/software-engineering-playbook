# ☕ 02 - Spring Boot API

> **The Brain.** While React is the face, Spring Boot is the reasoning engine that processes requests and talks to the database.

## 🏗️ What Is Spring Boot?

Spring Boot is a **Java framework** that makes building REST APIs effortless. It handles HTTP requests, security, database connections, and business logic—all with minimal configuration.

## 🔄 Request Flow (Full Stack Architecture)

```
┌─────────────┐      HTTP/JWT       ┌──────────────────┐      SQL       ┌─────────────────┐
│   React     │ ◄──────────────────► │  Spring Boot     │ ◄────────────► │  PostgreSQL/    │
│  Frontend   │     JSON REST       │  Backend API     │                │  MySQL          │
│  (SPA)      │                     │  (Java)          │                │  (Relational)   │
└─────────────┘                     └──────────────────┘                └─────────────────┘
```

## 📦 Spring Boot Architecture

```
┌────────────────────────────────────────────────────────┐
│                    Controller Layer                     │
│            (@RestController, @RequestMapping)           │
├────────────────────────────────────────────────────────┤
│                    Service Layer                       │
│            (@Service, Business Logic)                  │
├────────────────────────────────────────────────────────┤
│                    Repository Layer                   │
│            (@Repository, JPA, JDBC)                    │
├────────────────────────────────────────────────────────┤
│                    Entity / Model                     │
│            (@Entity, Database Mapping)                 │
└────────────────────────────────────────────────────────┘
```

### Analogy: The Restaurant
- **Controller** = The waiter who takes orders (HTTP requests)
- **Service** = The chef who prepares the food (business logic)
- **Repository** = The pantry where ingredients are stored (database access)
- **Entity** = The recipe card describing each dish (database schema)

---

## 🎯 API Endpoint Conventions (Implementation Scope)

| What to do ✅ | What NOT to do ❌ | Why |
|--------------|-------------------|-----|
| Use proper HTTP methods (GET, POST, PUT, DELETE) | Use GET for everything | Semantics matter |
| Return proper status codes (200, 201, 400, 404, 500) | Always return 200 | Clients need to know outcome |
| Use plural nouns for endpoints (`/pizzas`) | `/getPizza`, `/pizzaList` | REST conventions |
| Version your API (`/v1/pizzas`) | Break clients when changing | Backward compatibility |

### HTTP Methods Map

| Method | Action | Example |
|--------|--------|---------|
| `GET` | Retrieve resources | `GET /pizzas` |
| `POST` | Create new resource | `POST /pizzas` |
| `PUT` | Update entire resource | `PUT /pizzas/1` |
| `DELETE` | Remove resource | `DELETE /pizzas/1` |

> 📌 **Boundary:** This chapter focuses on implementing endpoints in Spring Boot. For API contract theory and style selection (REST vs GraphQL/gRPC/WebSocket/Webhooks/WebRTC/SOAP), use [`../../API_AND_INTERFACE_DESIGN/README.md`](../../API_AND_INTERFACE_DESIGN/README.md).

---

## 🔐 JWT Authentication Flow

```
┌─────────┐                    ┌─────────────┐                    ┌─────────┐
│ Client  │ ── 1. Login ────► │ Spring Boot │ ── 2. Validate ──► │ Database│
│(React)  │ ◄── 3. JWT Token ─ │   Backend   │ ◄─ 4. Success ──── │  (Users)│
└─────────┘                    └─────────────┘                    └─────────┘
      │
      │ 5. Request + JWT
      ▼
┌─────────────┐
│ Protected   │
│ Resource    │
└─────────────┘
```

| What to do ✅ | What NOT to do ❌ | Why |
|--------------|-------------------|-----|
| Store JWT in httpOnly cookie | LocalStorage (XSS vulnerable) | XSS attacks can steal tokens |
| Validate on every request | Trust client-side checks | Server is the source of truth |
| Expire tokens reasonably | Never expire | Compromised tokens live forever |
| Refresh tokens rotation | Reuse refresh tokens | Replay attack prevention |

---

## 🗃️ JPA / Repository Pattern

```java
@Repository
public interface PizzaRepository extends JpaRepository<Pizza, Long> {
    List<Pizza> findByNameContaining(String name);
}
```

| What to do ✅ | What NOT to do ❌ | Why |
|--------------|-------------------|-----|
| Use derived query methods | Write native SQL for simple queries | Readability, portability |
| Use @Transactional for writes | Forget transaction boundaries | Data inconsistency |
| Lazy/Eager fetch consciously | Load all relationships always | N+1 queries kill performance |
| Pagination for lists | Return unlimited results | Memory exhaustion |

---

## 💉 Dependency Injection (SOLID in Action)

```java
@Service
public class PizzaService {
    private final PizzaRepository repo;
    
    // Spring injects the dependency automatically
    public PizzaService(PizzaRepository repo) {
        this.repo = repo;
    }
}
```

| What to do ✅ | What NOT to do ❌ | Why |
|--------------|-------------------|-----|
| Constructor injection | @Autowired on fields | Testability, immutability |
| Depend on abstractions (interfaces) | Concrete classes | Swappable implementations |
| Single Responsibility per service | God Services doing everything | Cohesion |

---

## ⚠️ Common Mistakes

### 1. Entity = DTO Exposure
```java
// BAD: Entity has lazy collections exposed
@Entity
public class Pizza {
    @ManyToMany(fetch = FetchType.LAZY)
    List<Ingredient> ingredients; // Expensive to load!
}

// GOOD: DTO pattern
@Data
public class PizzaDTO {
    Long id;
    String name;
    List<String> ingredientNames; // Only what client needs
}
```

### 2. Not Validating Input
```java
// BAD
@PostMapping("/pizzas")
public Pizza createPizza(Pizza pizza) {
    repo.save(pizza); // What if pizza.name is null?
}

// GOOD
@PostMapping("/pizzas")
public Pizza createPizza(@Valid @RequestBody PizzaDTO dto) {
    // Validation annotation handles it
}
```

---

## 🔗 Related Topics

[⬅️ Previous: 01-React-Core](../01-React-Core/README.md) | [⬅️ Back to Parent](../README.md) | [➡️ Next: 03-FullStack-Integration](../03-FullStack-Integration/README.md)

---

*Principles applied: SOLID, Clean Architecture, Repository Pattern*
