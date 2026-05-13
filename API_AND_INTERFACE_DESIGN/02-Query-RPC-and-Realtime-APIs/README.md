# 02 — Query, RPC, and Realtime APIs

> **Why this matters:** REST is a strong baseline, but not every problem is CRUD. This module teaches when GraphQL, gRPC, WebSocket, or WebRTC is the right tool—and when it is not.

---

## 🧭 Quick Decision Table

| Need | Usually Best First Candidate | Why |
|------|------------------------------|-----|
| Client-specific data shapes | **GraphQL** | Consumer asks for exactly what it needs |
| Fast internal service calls | **gRPC** | Strong typed contracts and efficient binary transport |
| Bidirectional app-server realtime | **WebSocket** | Persistent full-duplex channel |
| Browser peer-to-peer low-latency media/data | **WebRTC** | Designed for direct realtime peer communication |

---

## 1) GraphQL

### Problem

REST endpoints can overfetch or underfetch when different clients need different field combinations.

### Mental Model

GraphQL is a **typed query language over a graph schema**. Clients request shape; server resolves fields.

### Flow

```text
Client sends query/mutation
  └─ GraphQL gateway validates schema
      └─ Resolvers fetch from services/data sources
          └─ Gateway returns exactly requested shape
```

### Minimal Example

```graphql
query UserCard {
  user(id: "usr_123") {
    name
    orders(limit: 2) { id total }
  }
}
```

### Strengths

- Precision payloads per client
- Strong schema introspection
- Powerful for multi-client products

### Weaknesses

- Resolver complexity and N+1 traps
- Caching and cost control need explicit governance

### Common Mistakes

- No query depth/complexity limits
- No DataLoader-like batching for N+1
- Exposing internal topology as schema

### Use / Avoid

- **Use when:** many clients need different views over related entities.
- **Avoid when:** you only need simple CRUD and want minimal operational overhead.

---

## 2) gRPC

### Problem

Internal service-to-service communication needs low latency, strict contracts, and codegen across teams.

### Mental Model

gRPC is **remote procedure calls over typed Protobuf contracts**.

### Flow

```text
Client SDK (generated from .proto)
  └─ HTTP/2 transport
      └─ Service method execution
          └─ Typed response or typed error status
```

### Minimal Example

```proto
service UserService {
  rpc GetUser (GetUserRequest) returns (GetUserResponse);
}

message GetUserRequest { string id = 1; }
message GetUserResponse { string id = 1; string name = 2; }
```

### Strengths

- Fast binary serialization
- Contract-first, strongly typed
- Native streaming support

### Weaknesses

- Harder browser/native web direct usage
- Debugging less human-readable than JSON

### Common Mistakes

- Breaking field numbering in `.proto`
- Mixing public edge concerns into internal RPC contracts
- Weak observability/metadata propagation

### Use / Avoid

- **Use when:** internal microservices require strict contracts and performance.
- **Avoid when:** public API ecosystem expects plain HTTP/JSON interoperability.

---

## 3) WebSocket

### Problem

Polling every few seconds for changing data is wasteful and slow for live UX.

### Mental Model

WebSocket is a **persistent bidirectional tunnel** between client and server.

### Flow

```text
Client connects (HTTP upgrade)
  └─ Auth handshake accepted
      └─ Both sides send/receive events over one socket
          └─ Heartbeats + reconnect strategy keep channel healthy
```

### Minimal Example

```json
{ "event": "price.updated", "symbol": "AAPL", "value": 201.44 }
```

### Strengths

- Real-time push without polling
- Bidirectional communication for interactive apps

### Weaknesses

- Stateful connection management complexity
- Requires explicit backpressure and reconnect rules

### Common Mistakes

- No heartbeat/timeout policy
- Missing auth token rotation on long-lived sessions
- No event schema versioning

### Use / Avoid

- **Use when:** chat, live dashboards, collaborative tools, multiplayer interactions.
- **Avoid when:** simple request/response suffices and infra should stay stateless.

---

## 4) WebRTC

### Problem

Realtime media (audio/video) through a central server can add high latency and cost.

### Mental Model

WebRTC enables **peer-to-peer realtime media/data**, with signaling + ICE to establish paths.

### Flow

```text
Peer A and Peer B exchange SDP/ICE via signaling server
  └─ Connectivity checks (STUN/TURN)
      └─ Direct or relayed media/data channel established
```

### Minimal Example (conceptual)

| Step | Action |
|------|--------|
| 1 | Create offer on Peer A |
| 2 | Send offer to Peer B via signaling API |
| 3 | Peer B answers and both exchange ICE candidates |
| 4 | Media/data channel starts |

### Strengths

- Very low-latency peer communication
- Built for browser-native media workflows

### Weaknesses

- NAT/firewall traversal complexity
- Operational dependency on TURN for hard networks

### Common Mistakes

- Assuming direct P2P always succeeds
- Ignoring fallback capacity planning for TURN
- Treating signaling as an afterthought

### Use / Avoid

- **Use when:** video/audio calls, realtime peer collaboration, data channels with low latency goals.
- **Avoid when:** deterministic request/response APIs are all you need.

---

## ⚖️ Comparison Snapshot

| Dimension | GraphQL | gRPC | WebSocket | WebRTC |
|-----------|---------|------|-----------|--------|
| Primary mode | Query/mutation | RPC/stream | Event channel | Peer media/data |
| Typical coupling | Schema to gateway | Service contract | Event protocol | Signaling + ICE contracts |
| Browser fit | High | Medium (via gateway/transcoding) | High | High |
| Operational complexity | Medium | Medium | Medium-High | High |

---

## 🔗 Related Topics

- HTTP/TLS foundations: [Networking](../../NETWORKING/README.md)
- Service tradeoffs at system scale: [System Design](../../ARCHITECTURE_AND_BEST_PRACTICES/03-System-Design/README.md)

---

[⬅️ Back to Parent](../README.md) | [⬅️ Previous: 01-API-Contracts-and-REST](../01-API-Contracts-and-REST/README.md) | [➡️ Next: 03-Integration-Governance-and-Legacy](../03-Integration-Governance-and-Legacy/README.md)
