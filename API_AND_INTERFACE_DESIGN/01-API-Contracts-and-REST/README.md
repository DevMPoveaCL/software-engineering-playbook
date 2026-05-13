# 01 — API Contracts and REST

> **Why this matters:** if your contract is vague, teams integrate by guesswork. REST is the baseline style to learn because it forces clarity around resources, semantics, and errors.

---

## 🎯 Problem

Teams often ship endpoints quickly but forget contract quality:

- inconsistent naming (`/getUsers`, `/users/list`, `/user`)  
- success/error responses without predictable shape  
- breaking changes without version strategy

Result: fragile integrations, hidden coupling, and expensive migrations.

---

## 🧠 Mental Model

REST is **resource-oriented communication over HTTP semantics**.

- Resource = business noun (`users`, `orders`, `invoices`)
- HTTP method = action semantics (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`)
- Status code = outcome semantics (`2xx`, `4xx`, `5xx`)
- Representation = contract payload (usually JSON)

If URL says noun, method says action, and response says outcome, the API is readable before opening the code.

---

## 🔄 Flow (Request Lifecycle)

```text
Client
  ├─ 1) Sends HTTP request (method + URL + headers + optional body)
  ├─ 2) Auth and validation run
  ├─ 3) Business rule executes on resource
  └─ 4) Server returns status + contract payload + metadata

Contract guardrails:
- Correlation-Id for tracing
- Idempotency-Key where retries may duplicate writes
- Standard error envelope
```

---

## 🧪 Minimal Example

### Resource design

| Goal | Contract |
|------|----------|
| List users | `GET /v1/users?limit=20&cursor=abc` |
| Read one user | `GET /v1/users/{id}` |
| Create user | `POST /v1/users` |
| Replace user | `PUT /v1/users/{id}` |
| Patch user | `PATCH /v1/users/{id}` |
| Delete user | `DELETE /v1/users/{id}` |

### Response envelope example

```json
{
  "data": {
    "id": "usr_123",
    "name": "Ana",
    "email": "ana@example.com"
  },
  "meta": {
    "requestId": "req_8f2",
    "version": "v1"
  }
}
```

### Error envelope example

```json
{
  "error": {
    "code": "VALIDATION_FAILED",
    "message": "email is invalid",
    "details": [{ "field": "email", "issue": "format" }]
  },
  "meta": {
    "requestId": "req_8f3"
  }
}
```

---

## ✅ Strengths

| Strength | Why It Helps |
|----------|--------------|
| Ubiquity | Every platform/tool understands HTTP/JSON |
| Cache friendliness | GET semantics integrate naturally with HTTP caches/CDN |
| Simple debugging | Easy inspection with browser, curl, logs |
| Stable governance model | OpenAPI + versioning + status semantics are mature |

---

## ⚠️ Weaknesses

| Weakness | Impact |
|----------|--------|
| Overfetch/underfetch risk | Fixed payloads may not match each client need |
| Verb drift in URLs | Teams accidentally create RPC-in-REST |
| Inconsistent semantics | Different teams misuse status codes/error models |

---

## 🚫 Common Mistakes

| Mistake | Why It Breaks | Better Choice |
|---------|----------------|---------------|
| `POST /getUsers` style endpoints | Hides semantics, kills predictability | Resource nouns + HTTP verbs |
| Returning `200` for every outcome | Clients cannot branch correctly | Use 4xx/5xx for errors |
| No idempotency strategy on writes | Retries create duplicates | Support `Idempotency-Key` on sensitive creates |
| Versioning by chaos | Breaking changes hurt integrations | Explicit version policy (`/v1`, compatibility rules) |

---

## 🧭 Use When / Avoid When

| Decision | Guidance |
|----------|----------|
| **Use REST when** | You need broad interoperability, CRUD-oriented resources, clear HTTP semantics, and straightforward governance. |
| **Avoid REST alone when** | Clients need highly customized graphs, strict internal low-latency RPC, or always-on realtime channels. |

---

## 🛡️ Governance Essentials

| Area | Rule of Thumb |
|------|---------------|
| Versioning | Prefer additive changes first; reserve major bumps for true contract breaks |
| Compatibility | Never remove/rename fields silently; deprecate with timeline |
| Idempotency | Mandatory for retryable create/pay/order flows |
| Observability | Correlation IDs, request logs, and error taxonomy are non-negotiable |
| Security | HTTPS, authn/authz boundaries, and least-privilege scopes |

---

## 🔗 Related Topics

- Transport and protocol internals: [Networking](../../NETWORKING/README.md)
- Macro architecture tradeoffs: [System Design](../../ARCHITECTURE_AND_BEST_PRACTICES/03-System-Design/README.md)
- Implementation in Java stack: [Spring Boot API](../../REACT+SPRINGBOOT/02-Spring-Boot-API/README.md)

---

[⬅️ Back to Parent](../README.md) | [➡️ Next: 02-Query-RPC-and-Realtime-APIs](../02-Query-RPC-and-Realtime-APIs/README.md)
