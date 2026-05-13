# 🔌 API and Interface Design

APIs are not “just endpoints.” They are **contracts between systems**: agreements about data shape, timing, errors, ownership, and change over time.

This topic teaches how to choose and design API styles with engineering judgment, so you can avoid cargo-cult choices like “always REST” or “just use GraphQL everywhere.”

---

## 📊 Objective Table: API Interface Design

| Aspect | Didactic Explanation |
|--------|----------------------|
| **What is it for?** | Designing clear contracts so different systems (frontend, backend, services, partners) can communicate safely and evolve without chaos. |
| **What are the benefits?** | Lower coupling, clearer ownership, predictable errors, safer versioning, and easier observability when incidents happen. |
| **When to use it?** | Every time two components communicate across a boundary: browser↔API, service↔service, platform↔third-party, or realtime channels. |
| **When NOT to over-engineer it?** | Tiny prototypes or single-process scripts. Start simple, then increase contract rigor when integration risk appears. |

---

## 🧠 Mental Model: Contracts, Not Code

Think of an API like a legal contract:

- **Parties**: who talks to whom (client/server, producer/consumer, peer/peer)
- **Language**: schema and protocol (JSON/OpenAPI, GraphQL schema, Protobuf, XML/WSDL)
- **Timing**: sync request/response vs async events vs long-lived streams
- **Breach handling**: error semantics, retries, idempotency, timeouts
- **Amendments**: versioning, compatibility, deprecation windows

If these pieces are undefined, integration breaks even if your code compiles.

---

## 🧭 API Style Matrix (Quick Selection)

| Style | Interaction Model | Contract Shape | Best Fit | Watch Out |
|-------|-------------------|----------------|----------|-----------|
| **REST** | Sync request/response | Resource + HTTP semantics + OpenAPI | CRUD-heavy business APIs, public APIs | Verb-like URLs, wrong status codes, breaking changes |
| **GraphQL** | Sync query/mutation (optional subscriptions) | Typed graph schema | Multiple clients with different data needs | N+1 queries, over-flexibility without governance |
| **gRPC** | Sync RPC + streaming | Protobuf service contract | Internal service-to-service with strict performance | Browser compatibility and debugging complexity |
| **WebSocket** | Full-duplex persistent channel | Message protocol you define | Live dashboards, chat, collaborative apps | Backpressure, reconnect strategy, auth rotation |
| **WebRTC** | Peer-to-peer realtime media/data | SDP + ICE + signaling contract | Low-latency audio/video/data between clients | NAT traversal, signaling complexity |
| **Webhooks** | Async callbacks (event push) | Event payload + signature contract | Cross-system notifications | Retry/idempotency failures, unverified signatures |
| **SOAP** | Structured message exchange | WSDL + XML schema | Legacy enterprise integrations with strict contracts | Verbose payloads, tooling overhead |

---

## 🧱 Boundary Guide (Avoid Duplication)

| Topic | Owns This | API Topic Owns This |
|-------|-----------|---------------------|
| **Networking** | TCP, TLS, DNS, HTTP transport fundamentals | API contract design and style selection built on top of HTTP/networking |
| **System Design** | Scale, distributed tradeoffs, CAP, reliability architecture | Interface-level contract choices and compatibility rules |
| **React + Spring Boot** | Implementation details (controllers, DTOs, fetch/client usage) | Why a contract style is chosen and how to govern it |
| **JavaScript Async/APIs** | Consuming APIs from the client side | Designing producer-side contracts and multi-system evolution |

Use this topic for **interface strategy**. Link out for transport internals, platform code, and macro-architecture.

---

## 📚 Learning Path

| Module | Focus | What You Will Learn |
|--------|-------|---------------------|
| [01-API-Contracts-and-REST](./01-API-Contracts-and-REST/README.md) | Baseline contract thinking | Resource modeling, HTTP semantics, idempotency, versioning, OpenAPI, and REST mistakes |
| [02-Query-RPC-and-Realtime-APIs](./02-Query-RPC-and-Realtime-APIs/README.md) | Specialized interaction styles | GraphQL, gRPC, WebSocket, and WebRTC selection and tradeoffs |
| [03-Integration-Governance-and-Legacy](./03-Integration-Governance-and-Legacy/README.md) | Async integration and lifecycle governance | Webhooks, SOAP, compatibility, observability, security, and deprecation |

> **Start here if you are new:** Begin with [01-API-Contracts-and-REST](./01-API-Contracts-and-REST/README.md). The other styles make more sense once the contract baseline is solid.

---

## 📚 Technical Glossary

- **API Contract:** Shared agreement about requests, responses, errors, and change rules.
- **Consumer-Driven Contract:** A test/spec approach where consumers define expectations providers must keep.
- **Idempotency:** Repeating the same request produces the same effect (critical for retries).
- **Schema Evolution:** Changing contracts without breaking existing consumers.
- **Backward Compatibility:** Old clients keep working after a new release.

---

### 🔗 Global Navigation
[⬅️ Previous Topic: Networking](../NETWORKING/README.md) | [🏠 Master Index](../README.md) | [➡️ Next Topic: SQL](../SQL/README.md)
<br>
**[⬇️ Dive In: 01-API-Contracts-and-REST](./01-API-Contracts-and-REST/README.md)**
