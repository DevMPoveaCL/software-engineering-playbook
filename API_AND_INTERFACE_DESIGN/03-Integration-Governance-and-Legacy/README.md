# 03 — Integration, Governance, and Legacy

> **Why this matters:** the hardest API problems are rarely “how to return JSON.” They are integration reliability, backward compatibility, and long-term contract governance.

---

## 🧭 Quick Path

1. Pick integration style for the boundary (event callback, enterprise contract, etc.).
2. Define reliability semantics first (retry, idempotency, deduplication, timeout).
3. Define evolution rules (versioning, compatibility, deprecation timeline).
4. Add observability and security controls before rollout.

---

## 1) Webhooks

### Problem
System A needs to notify System B asynchronously when events happen, without B polling constantly.

### Mental Model
Webhook = **event callback contract** over HTTP.

Producer sends event to consumer URL when event occurs.

### Flow

```text
Event occurs on Producer
  └─ Producer signs payload + sends POST callback
      └─ Consumer verifies signature + processes event
          └─ Consumer returns 2xx; otherwise Producer retries
```

### Minimal Example

```json
{
  "event": "invoice.paid",
  "id": "evt_9001",
  "occurredAt": "2026-05-12T20:15:00Z",
  "data": { "invoiceId": "inv_123", "amount": 149.00 }
}
```

### Strengths
- Asynchronous and scalable integration pattern
- Decouples producer timing from consumer polling cadence

### Weaknesses
- Delivery is often at-least-once (duplicates are possible)
- Consumer endpoint reliability becomes critical

### Common Mistakes
- No signature verification
- No idempotency key / dedup by event ID
- Retry policy without dead-letter handling

### Use / Avoid
- **Use when:** event notifications to external partners or internal bounded contexts.
- **Avoid when:** strict immediate response data is required in same user flow.

---

## 2) SOAP

### Problem
Some enterprise ecosystems require strict formal contracts, advanced WS-* standards, and long-lived legacy interoperability.

### Mental Model
SOAP = **XML message protocol with WSDL-defined contract**.

### Flow

```text
Client reads WSDL
  └─ Generates contract-aware client
      └─ Sends SOAP envelope
          └─ Service validates and returns SOAP response/fault
```

### Minimal Example (envelope skeleton)

```xml
<soap:Envelope>
  <soap:Header>...</soap:Header>
  <soap:Body>
    <GetInvoiceRequest>
      <InvoiceId>inv_123</InvoiceId>
    </GetInvoiceRequest>
  </soap:Body>
</soap:Envelope>
```

### Strengths
- Formal contract governance (WSDL/XSD)
- Mature support in legacy enterprise environments

### Weaknesses
- Verbose payloads and tooling overhead
- Slower onboarding for modern web teams

### Common Mistakes
- Assuming SOAP is “bad by default” instead of evaluating constraints
- Mixing SOAP and REST semantics in a single contract surface without clear boundary

### Use / Avoid
- **Use when:** enterprise/regulated partners require formal SOAP contracts.
- **Avoid when:** modern public web API simplicity and developer experience are top priority.

---

## 🛡️ Contract Governance Rules

| Area | Rule | Why |
|------|------|-----|
| Versioning | Prefer additive non-breaking changes; major versions only for unavoidable breaks | Protect existing consumers |
| Compatibility | Never silently remove/rename contract fields | Reduces integration outages |
| Error semantics | Publish stable error codes and remediation hints | Faster client-side recovery |
| Idempotency | Required for retried writes/events | Prevents duplicates and double charges |
| Deprecation | Announce timeline + migration path + sunset date | Predictable transitions |

---

## 🔍 Observability Checklist

- [ ] Correlation/request IDs in every interaction
- [ ] Structured logs with contract version and consumer identity
- [ ] Delivery metrics (success %, retries, DLQ counts)
- [ ] Alerting for error spikes by endpoint/event type
- [ ] Trace links across producer/consumer boundaries

Without this, debugging distributed contract failures becomes guesswork.

---

## 🔐 Security Checklist

- [ ] TLS everywhere (in transit encryption)
- [ ] Message signing/HMAC for webhooks
- [ ] AuthN/AuthZ scopes per consumer
- [ ] Replay protection (timestamp/nonce windows)
- [ ] Secret rotation and key expiration policy

Security is part of the contract, not an implementation detail.

---

## ⚖️ Selection Guidance by Scenario

| Scenario | Better Default | Reason |
|----------|----------------|--------|
| Partner needs payment/completion notifications | Webhooks | Async callback model fits event-driven boundaries |
| Regulated enterprise integration with contract tooling | SOAP | Formal WSDL/XSD and compliance-friendly patterns |
| Internal domain events with strong retries/idempotency | Webhooks (or event bus + callback edge) | Better decoupling than synchronous waits |

---

## 🔗 Related Topics

- Distributed tradeoffs and resilience patterns: [System Design](../../ARCHITECTURE_AND_BEST_PRACTICES/03-System-Design/README.md)
- HTTP transport and security basics: [Networking](../../NETWORKING/README.md)

---

[⬅️ Back to Parent](../README.md) | [⬅️ Previous: 02-Query-RPC-and-Realtime-APIs](../02-Query-RPC-and-Realtime-APIs/README.md)
