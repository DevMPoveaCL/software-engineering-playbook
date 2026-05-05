# 🚀 03 - FullStack Integration

> **The Symphony.** Combining React's beauty with Spring Boot's power to build complete, production-ready applications.

## 🎼 Two Worlds, One App

```
┌─────────────────────────────────────────────────────────────────────┐
│                          FULLSTACK APP                              │
├───────────────────────────────┬─────────────────────────────────────┤
│         REACT (Frontend)      │         SPRING BOOT (Backend)        │
│                               │                                     │
│   ┌─────────────────────────┐ │   ┌──────────────────────────────┐   │
│   │   Components           │ │   │   Controllers                │   │
│   │   useState/useEffect   │ │   │   @GetMapping, @PostMapping  │   │
│   │   useContext           │ │   │                              │   │
│   └───────────┬────────────┘ │   └───────────┬──────────────────┘   │
│               │              │              │                       │
│               │ HTTP (JSON)  │              │                       │
│               ▼              │              ▼                       │
│   ┌─────────────────────────┐ │   ┌──────────────────────────────┐   │
│   │   API Service Layer     │ │   │   Service Layer               │   │
│   │   axios/fetch           │ │   │   @Service + Business Logic   │   │
│   └─────────────────────────┘ │   └───────────┬──────────────────┘   │
│                               │              │                       │
│                               │              ▼                       │
│                               │   ┌──────────────────────────────┐   │
│                               │   │   Repository Layer           │   │
│                               │   │   JPA + Database             │   │
│                               │   └──────────────────────────────┘   │
└───────────────────────────────┴─────────────────────────────────────┘
```

## 🔌 Connecting React to Spring Boot

### Step 1: The API Service (React Side)

```typescript
// src/services/pizzaService.ts
const API_URL = 'http://localhost:8080/api';

export const pizzaService = {
  getAll: async (): Promise<Pizza[]> => {
    const response = await fetch(`${API_URL}/pizzas`);
    if (!response.ok) throw new Error('Failed to fetch pizzas');
    return response.json();
  },

  create: async (pizza: CreatePizzaDTO): Promise<Pizza> => {
    const response = await fetch(`${API_URL}/pizzas`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(pizza),
    });
    if (!response.ok) throw new Error('Failed to create pizza');
    return response.json();
  },
};
```

### Step 2: CORS Configuration (Spring Boot Side)

```java
@Configuration
public class CorsConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/api/**")
                .allowedOrigins("http://localhost:3000")
                .allowedMethods("GET", "POST", "PUT", "DELETE")
                .allowCredentials(true);
    }
}
```

---

## 🔐 JWT Integration Flow

```
┌─────────┐  1. Login Form  ┌─────────────┐  2. Validate  ┌─────────┐
│  User   │ ────────────────►│  Spring    │ ─────────────►│   DB    │
│(React)  │ ◄─────────────── │  Boot API  │ ◄──────────── │ (Users) │
└─────────┘  3. JWT Token     └─────────────┘  4. Success   └─────────┘
     │
     │ 5. Request + Authorization: Bearer <token>
     ▼
┌─────────────┐
│  /api/pizzas │
│  /api/orders │
└─────────────┘
```

### React: Attaching Token to Requests

```typescript
// Axios interceptor approach
api.interceptors.request.use((config) => {
  const token = localStorage.getItem('token'); // Consider httpOnly cookies!
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});
```

---

## 📁 FullStack Project Structure

```
project-root/
├── frontend/                    # React App
│   ├── src/
│   │   ├── components/         # Dumb components
│   │   ├── pages/              # Route pages
│   │   ├── services/            # API calls
│   │   ├── context/             # State management
│   │   └── types/               # TypeScript interfaces
│   └── package.json
│
├── backend/                    # Spring Boot App
│   ├── src/main/java/
│   │   ├── controller/
│   │   ├── service/
│   │   ├── repository/
│   │   └── model/
│   └── pom.xml
│
└── docker-compose.yml           # Run both with one command
```

---

## ✅ What to Do / What NOT to Do

| What to do ✅ | What NOT to do ❌ | Why |
|--------------|-------------------|-----|
| Environment variables for API URLs | Hardcode URLs | Dev/prod separation |
| Centralized API service layer | Scattering fetch calls everywhere | DRY, maintainability |
| Unified error handling | Ignoring HTTP error codes | Debugging nightmares |
| CORS in backend only | Proxy workarounds in frontend | Security and clarity |
| Wait for mount before API calls | Calling in render phase | Avoid N+1 renders |
| Loading/error states | Silent failures | UX and debuggability |

---

## ⚠️ Common Integration Pitfalls

### 1. N+1 Queries
```java
// BAD: Lazy loading causes N+1
@GetMapping("/pizzas")
public List<Pizza> getPizzas() {
    return repo.findAll(); // Triggers query for each pizza's ingredients
}

// GOOD: Eager fetch or @EntityGraph
@EntityGraph(attributePaths = {"ingredients"})
List<Pizza> findAllWithIngredients();
```

### 2. Not Validating DTOs on Both Sides
```typescript
// React: Client validation (UX only)
if (!pizza.name || pizza.name.length < 2) {
    setError('Name must be at least 2 characters');
}

// Spring: Server validation (SECURITY is mandatory)
@PostMapping("/pizzas")
public Pizza create(@Valid @RequestBody PizzaDTO dto) {
    // Validation happens automatically
}
```

### 3. Unbounded List Requests
```typescript
// BAD
const pizzas = await fetch('/api/pizzas').then(r => r.json());

// GOOD
const pizzas = await fetch('/api/pizzas?page=0&size=20').then(r => r.json());
```

---

## 🎯 FullStack Security Checklist

- [ ] CORS configured for specific origins only
- [ ] JWT stored in httpOnly cookies (not localStorage)
- [ ] CSRF protection enabled for state-changing operations
- [ ] Input validation on backend (never trust client)
- [ ] HTTPS in production
- [ ] Rate limiting on auth endpoints
- [ ] Sensitive data excluded from API responses

---

## 🔗 Related Topics

- **[⬅️ Back to Parent](../README.md)**
- **[⬅️ Previous: Spring Boot API](../02-Spring-Boot-API/README.md)**
- **[➡️ React Core](../01-React-Core/README.md)**

---

*Principles applied: DRY (API layer), KISS (single responsibility per layer), security in depth*
