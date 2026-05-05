# 🎯 System Design

> **Why this matters:** System design is the discipline of building software that scales. A brilliant algorithm that crashes under load is worthless. This teaches you to think at scale from day one.

---

## 🧠 Mental Model: The Restaurant Chain

### From Single Restaurant to Chain

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   SINGLE RESTAURANT (Monolithic App)                            │
│   ───────────────────────────────────                           │
│                                                                  │
│   ┌─────────────────────────────────────────────────────────┐   │
│   │                    ONE KITCHEN                            │   │
│   │                                                          │   │
│   │   [Orders] ──▶ [Cooking] ──▶ [Serving] ──▶ [Customer]    │   │
│   │                                                          │   │
│   │   Problems:                                              │   │
│   │   - One chef does everything                            │   │
│   │   - If one station breaks, restaurant closes             │   │
│   │   - Can't handle 10x customers                          │   │
│   │                                                          │   │
│   └─────────────────────────────────────────────────────────┘   │
│                                                                  │
│   RESTAURANT CHAIN (Microservices)                              │
│   ──────────────────────────────                                │
│                                                                  │
│   ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │
│   │  ORDERING   │  │  KITCHEN    │  │  DELIVERY   │             │
│   │   SERVICE   │  │  SERVICE    │  │  SERVICE    │             │
│   │             │  │             │  │             │             │
│   │  - Menu     │  │  - Cooking  │  │  - Drivers  │             │
│   │  - Pricing  │  │  - Quality  │  │  - Tracking │             │
│   │  - Cart     │  │             │  │             │             │
│   └──────┬──────┘  └──────┬──────┘  └──────┬──────┘             │
│          │                │                │                     │
│          └────────────────┴────────────────┘                     │
│                      MESSAGE QUEUE                               │
│              (Orders flow asynchronously)                       │
│                                                                  │
│   Benefits:                                                     │
│   - Each service scales independently                          │
│   - Kitchen breaks ≠ Ordering stops                            │
│   - Can handle 1000x orders per second                         │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📐 Scale Concepts

### The Scale Ladder

| Level | Description | Example |
|-------|-------------|---------|
| **1** | Single server | All on one machine |
| **10** | Vertical scaling | More RAM, CPU, disk |
| **100** | Horizontal scaling | Multiple servers |
| **1K** | Database partitioning | Sharding begins |
| **10K** | Caching everywhere | Redis, CDN |
| **100K** | Async processing | Message queues |
| **1M** | Specialized services | Microservices |

### Vertical vs. Horizontal Scaling

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   VERTICAL SCALING (Scale Up)                                    │
│   "Bigger pan to cook more"                                      │
│                                                                  │
│              ┌─────────────┐                                     │
│              │  SUPER      │                                     │
│              │  PAN        │                                     │
│              │             │                                     │
│              └─────────────┘                                     │
│                                                                  │
│   ✅ Simpler                                                   │
│   ✅ No code changes                                           │
│   ❌ Hardware limits                                           │
│   ❌ Single point of failure                                   │
│                                                                  │
│   HORIZONTAL SCALING (Scale Out)                                │
│   "More pans working together"                                  │
│                                                                  │
│       ┌─────────┐  ┌─────────┐  ┌─────────┐                     │
│       │  PAN 1  │  │  PAN 2  │  │  PAN 3  │                     │
│       └────┬────┘  └────┬────┘  └────┬────┘                     │
│            └────────────┴─────────────┘                          │
│                      │                                           │
│                      ▼                                           │
│               ┌─────────────┐                                    │
│               │  LOAD       │                                    │
│               │  BALANCER   │                                    │
│               └─────────────┘                                    │
│                                                                  │
│   ✅ Unlimited scale                                           │
│   ✅ Redundant (one breaks, others keep cooking)               │
│   ❌ More complex                                              │
│   ❌ Requires stateless design                                 │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🗄️ Database Scaling Patterns

### Read Replicas

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   ┌──────────┐                                                  │
│   │  WRITE   │                                                  │
│   │  Primary │                                                  │
│   └────┬─────┘                                                  │
│        │ (replicate)                                             │
│   ┌────┴─────┐  ┌──────────┐  ┌──────────┐                     │
│   │  READ    │  │  READ    │  │  READ    │                     │
│   │  Replica │  │  Replica │  │  Replica │                     │
│   │    1     │  │    2     │  │    3     │                     │
│   └──────────┘  └──────────┘  └──────────┘                     │
│                                                                  │
│   Application:                                                  │
│   - Writes → Primary                                            │
│   - Reads → Any replica (for scale)                            │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Sharding

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   SHARDING: Split data by key                                    │
│                                                                  │
│   ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐│
│   │  SHARD 1        │  │  SHARD 2        │  │  SHARD 3        ││
│   │  Users A-M      │  │  Users N-S      │  │  Users T-Z      ││
│   │                 │  │                 │  │                 ││
│   │  - Alice        │  │  - Nancy         │  │  - Tom         ││
│   │  - Bob          │  │  - Oscar         │  │  - Zara        ││
│   │  - ...          │  │  - ...           │  │  - ...         ││
│   └─────────────────┘  └─────────────────┘  └─────────────────┘│
│                                                                  │
│   Shard Key: First letter of last name                         │
│   ────────────────────────────────────────                      │
│   "Find Alice?" → Shard 1 → Done!                              │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔄 Caching Strategy

### The Cache Ladder

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   REQUEST → CDN (static assets) → Browser Cache → App Cache    │
│                                              ↓                  │
│                                        Redis/Memcached          │
│                                              ↓                  │
│                                        Database                 │
│                                                                  │
│   SPEED:  CDN (ms) > Browser (ms) > Redis (μs) > DB (ms)      │
│   COST:   Free        > Free        > RAM $$$  > Disk $        │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Cache Patterns

| Pattern | Description | Use When |
|---------|-------------|----------|
| **Cache-Aside** | App checks cache, falls back to DB | Read-heavy, data changes occasionally |
| **Write-Through** | Write to cache AND DB simultaneously | Reads must always be consistent |
| **Write-Behind** | Write to cache, DB async | Writes must be fast, eventual consistency OK |
| **Refresh-Ahead** | Proactively refresh expiring cache | Hot data that must always be available |

---

## 📝 API Design Patterns

### RESTful API Design

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   COLLECTION PATTERN                                             │
│                                                                  │
│   GET    /users              ← List all users (paginated)      │
│   POST   /users              ← Create user                      │
│   GET    /users/{id}         ← Get single user                 │
│   PUT    /users/{id}         ← Replace user                     │
│   PATCH  /users/{id}         ← Update user (partial)           │
│   DELETE /users/{id}        ← Delete user                      │
│                                                                  │
│   NESTED RESOURCES                                              │
│                                                                  │
│   GET    /users/{id}/orders  ← All orders for user            │
│   POST   /users/{id}/orders  ← Create order for user          │
│   GET    /users/{id}/orders/{orderId} ← Specific order        │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### GraphQL vs REST

| Aspect | REST | GraphQL |
|--------|------|---------|
| **Data fetching** | Multiple endpoints | Single query |
| **Over-fetching** | Returns all fields | Ask for what you need |
| **Under-fetching** | N+1 problem possible | Can request all in one |
| **Caching** | HTTP caching natural | Custom caching |
| **Learning curve** | Simpler | Steeper |

---

## 🔀 Message Queues

### Why Queues?

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   SYNCHRONOUS (What we DON'T want)                              │
│   ──────────────────────────────                                 │
│                                                                  │
│   User places order                                              │
│         │                                                        │
│         ▼                                                        │
│   ┌─────────────────────────────────────────────────────────┐   │
│   │  Kitchen must complete ALL steps before response sent  │   │
│   │                                                          │   │
│   │  1. Cook food (10 min)                                   │   │
│   │  2. Plate (1 min)                                        │   │
│   │  3. Deliver (30 min)                                     │   │
│   │                                                          │   │
│   │  User waits 41 MINUTES for "Order Placed" confirmation  │   │
│   └─────────────────────────────────────────────────────────┘   │
│                                                                  │
│   ASYNC (What we WANT)                                          │
│   ───────────────────                                           │
│                                                                  │
│   User places order                                              │
│         │                                                        │
│         ▼                                                        │
│   ┌─────────────┐     ┌─────────────┐     ┌─────────────┐      │
│   │  ORDER      │     │   KITCHEN   │     │ DELIVERY    │      │
│   │  SERVICE    │ ──▶ │   QUEUE     │ ──▶ │   QUEUE     │      │
│   │             │     │             │     │             │      │
│   └─────────────┘     └──────┬──────┘     └──────┬─────┘      │
│        │                      │                    │             │
│        │                      ▼                    ▼             │
│        │  "Order #123 placed!  │       "Food ready!"            │
│        │   We're preparing..." │       "Driver assigned!"       │
│        │                       │                              │
│        │  User gets IMMEDIATE response                        │
│        │  Actual processing happens in background             │
│        │                                                      │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🛡️ Designing for Failure

### The Circuit Breaker Pattern

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   NORMAL STATE                                                  │
│   ────────────                                                  │
│   ┌────────┐     ┌────────┐     ┌────────┐                   │
│   │ Call   │ ──▶ │ Service│ ──▶ │  OK    │                   │
│   │        │     │        │     │        │                   │
│   └────────┘     └────────┘     └────────┘                   │
│                                                                  │
│   FAILURE DETECTED (Breaker trips)                              │
│   ───────────────────────────────────                           │
│   ┌────────┐     ┌────────┐     ┌────────┐                   │
│   │ Call   │ ──▶ │ OPEN   │ ──▶ │FAILFAST│                   │
│   │        │     │        │     │(cached) │                   │
│   └────────┘     └────────┘     └────────┘                   │
│                      │                                           │
│              After timeout,                                     │
│              try again (HALF-OPEN)                              │
│                                                                  │
│   RECOVERY (Service restored)                                    │
│   ───────────────────────────                                    │
│   ┌────────┐     ┌────────┐     ┌────────┐                   │
│   │ Call   │ ──▶ │ CLOSED │ ──▶ │  OK    │                   │
│   │        │     │(normal)│     │        │                   │
│   └────────┘     └────────┘     └────────┘                   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## ✅ What TO Do

| Do This | Why |
|---------|-----|
| **Design for failure** | Networks break, servers crash, disks fail |
| **Make services stateless** | Enables horizontal scaling |
| **Cache aggressively** | Reduces database load dramatically |
| **Use async where possible** | Better user experience |
| **Design APIs for evolution** | Versioning, backwards compatibility |

---

## 🚫 What NOT To Do

| Don't Do This | Why Not |
|---------------|---------|
| **Don't optimize prematurely** | Build for correctness first |
| **Don't ignore monitoring** | You can't fix what you can't see |
| **Don't couple services tightly** | Distributed monolith is worse than monolith |
| **Don't forget about data consistency** | CAP theorem: choose wisely |
| **Don't skip the load balancer** | One server = single point of failure |

---

[⬅️ Back to Parent](../README.md)
