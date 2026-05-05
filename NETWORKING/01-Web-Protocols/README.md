# 🌐 Web Protocols

> **Why this matters:** HTTP is the language of the web. Every web request, every API call, every page load — it all speaks HTTP. Master this, and you understand the foundation everything else builds on.

---

## 🧠 Mental Model: The Restaurant Order System

Imagine you're at a restaurant:

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   YOU (Client)              WAITER              KITCHEN          │
│      │                       │                    │              │
│      │  "I'd like the        │                    │              │
│      │   steak, medium-rare"│                    │              │
│      │ ────────────────────▶ │                    │              │
│      │                       │                    │              │
│      │                       │ "Order: steak MR"   │              │
│      │                       │ ─────────────────▶ │              │
│      │                       │                    │              │
│      │                       │                    │ (cooking...) │
│      │                       │                    │              │
│      │                       │   "Here's your     │              │
│      │    "Here you go!"      │    steak!"         │              │
│      │ ◀──────────────────── │ ◀──────────────────│              │
│      │                       │                    │              │
└─────────────────────────────────────────────────────────────────┘
```

**HTTP works the same way:**

- **You (Client)** = Browser, app, or code making a request
- **Waiter (HTTP)** = The protocol that carries your request and the response
- **Kitchen (Server)** = Processes your request and sends back food (data)

---

## 📋 HTTP Methods: The Request "Verbs"

| Method | Purpose | Real-World Analogy |
|--------|---------|-------------------|
| `GET` | Retrieve data | Asking to see the menu |
| `POST` | Create new resource | Ordering a new dish |
| `PUT` | Replace entire resource | Swapping your current order for a new one |
| `PATCH` | Partially update | "Can you add fries to my order?" |
| `DELETE` | Remove resource | "Cancel my order" |

### ASCII: GET Request

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   BROWSER                         SERVER                         │
│      │                               │                           │
│      │  GET /api/users/123           │                           │
│      │ ────────────────────────────▶│                           │
│      │                               │                           │
│      │                               │ (finds user 123)          │
│      │                               │                           │
│      │  200 OK { "name": "Ana" }     │                           │
│      │ ◀────────────────────────────│                           │
│      │                               │                           │
└─────────────────────────────────────────────────────────────────┘
```

### ASCII: POST Request

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   FRONTEND                        API                            │
│      │                              │                            │
│      │  POST /api/orders            │                            │
│      │  Content-Type: application/json                            │
│      │  { "product": "laptop",      │                            │
│      │    "quantity": 1 }           │                            │
│      │ ────────────────────────────▶│                            │
│      │                              │                            │
│      │                              │ (creates order)             │
│      │                              │                            │
│      │  201 Created                 │                            │
│      │  Location: /api/orders/456   │                            │
│      │ ◀────────────────────────────│                            │
│      │                              │                            │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔢 HTTP Status Codes: The Restaurant Responses

### The Code Categories

| Range | Category | What It Means |
|-------|----------|---------------|
| `1xx` | Informational | "I'm thinking..." (Server is processing) |
| `2xx` | Success | "Done! Here's what you asked for" |
| `3xx` | Redirection | "Go somewhere else to find this" |
| `4xx` | Client Error | "YOU messed up" (Your request is wrong) |
| `5xx` | Server Error | "I (server) messed up" (Server failed) |

### The Most Common Codes

| Code | Meaning | When You See It |
|------|---------|-----------------|
| `200 OK` | Success | Standard successful response |
| `201 Created` | Created | New resource successfully created |
| `204 No Content` | Success, empty | Successful DELETE or action with no response |
| `301 Moved Permanently` | Redirect | Resource has a new permanent URL |
| `304 Not Modified` | Cache hit | Browser can use cached version |
| `400 Bad Request` | Client error | Malformed request body |
| `401 Unauthorized` | Client error | Not logged in (no valid token) |
| `403 Forbidden` | Client error | Logged in but no permission |
| `404 Not Found` | Client error | Resource doesn't exist |
| `409 Conflict` | Client error | State conflict (e.g., duplicate) |
| `422 Unprocessable Entity` | Client error | Validation failed |
| `429 Too Many Requests` | Client error | Rate limit exceeded |
| `500 Internal Server Error` | Server error | Unhandled crash on server |
| `502 Bad Gateway` | Server error | Upstream service failed |
| `503 Service Unavailable` | Server error | Server is down |

---

## 🏗️ REST: The API Design Standard

### What is REST?

REST (Representational State Transfer) is a **set of conventions** for designing APIs that are predictable and easy to use.

### RESTful URL Design

| Action | ❌ Non-RESTful | ✅ RESTful |
|--------|---------------|-----------|
| Get all users | `GET /getUsers` | `GET /users` |
| Get user 123 | `GET /getUser?id=123` | `GET /users/123` |
| Create user | `POST /createUser` | `POST /users` |
| Update user | `POST /updateUser?id=123` | `PUT /users/123` |
| Delete user | `POST /deleteUser?id=123` | `DELETE /users/123` |

### ASCII: REST Resource Model

```
┌─────────────────────────────────────────────────────────────────┐
│                    RESTful API Structure                         │
│                                                                  │
│              ┌────────────────────────────────┐                │
│              │           /users                │                │
│              │    CRUD: GET, POST               │                │
│              └───────────┬──────────────────────┘                │
│                          │                                       │
│                          │                                       │
│              ┌───────────▼──────────────────────┐                │
│              │      /users/{id}                 │                │
│              │  CRUD: GET, PUT, PATCH, DELETE   │                │
│              └──────────────────────────────────┘                │
│                                                                  │
│              ┌────────────────────────────────┐                │
│              │        /orders                 │                │
│              │    CRUD: GET, POST               │                │
│              └───────────┬──────────────────────┘                │
│                          │                                       │
│              ┌───────────▼──────────────────────┐                │
│              │      /orders/{id}                │                │
│              │  CRUD: GET, PUT, PATCH, DELETE   │                │
│              └──────────────────────────────────┘                │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📦 Request/Response Anatomy

### HTTP Request Parts

```http
GET /api/users/123 HTTP/1.1
Host: api.example.com
Accept: application/json
Authorization: Bearer eyJhbG...
Content-Type: application/json
```

| Part | Description |
|------|-------------|
| Request Line | Method + Path + HTTP Version |
| Headers | Metadata about the request |
| Body | Data (for POST/PUT/PATCH) |

### HTTP Response Parts

```http
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: max-age=3600

{
  "id": 123,
  "name": "Ana García",
  "email": "ana@example.com"
}
```

| Part | Description |
|------|-------------|
| Status Line | HTTP Version + Status Code + Message |
| Headers | Metadata about the response |
| Body | The actual data |

---

## ⚡ HTTP/2 and HTTP/3: Modern Improvements

| Feature | HTTP/1.1 | HTTP/2 | HTTP/3 |
|---------|----------|--------|--------|
| **Multiplexing** | No (one request at a time) | Yes | Yes |
| **Header Compression** | No | Yes (HPACK) | Yes (QPACK) |
| **Server Push** | No | Yes | Deprecated |
| **Connection** | Text-based | Binary | UDP-based |
| **Latency** | Higher | Lower | Lowest |

---

## ✅ What TO Do

| Do This | Why |
|---------|-----|
| **Use proper HTTP methods** | GET for reads, POST for creates, etc. |
| **Return appropriate status codes** | 201 for created, 404 for not found, etc. |
| **Use plural nouns for resources** | `/users` not `/user` |
| **Version your APIs** | `/v1/users`, `/v2/users` for backwards compat |
| **Secure with HTTPS** | Always encrypt in transit |

---

## 🚫 What NOT To Do

| Don't Do This | Why Not |
|---------------|---------|
| **Don't use GET for state changes** | GET should be idempotent (safe) |
| **Don't return 200 for errors** | Use 4xx/5xx appropriately |
| **Don't expose internal errors** | Return generic 500 to clients |
| **Don't use verbs in URLs** | `/getUsers` is not RESTful |
| **Don't forget to handle timeouts** | Network is unreliable |

---

[⬅️ Back to Parent](../README.md)
