# Resiliency (System Design)

## Definition

**Resiliency** is the ability of a system to **handle failures and continue operating without crashing**.

---

## Core Idea

> Failures are inevitable → system should **recover or continue gracefully**

Instead of avoiding failure, resilient systems are designed to **survive failure**.

---

## Example

```text
User → Service A → Service B → Database

If Service B fails:
→ System retries OR uses fallback
→ User still gets response (maybe partial)
```

---

## Real-World Use Case

### E-commerce Platform

* User places order
* Payment service fails temporarily

**Resilient behavior:**

* Retry payment
* Or queue request for later
* Or show “try again” instead of crashing

---

## Key Techniques (Building Resiliency)

---

## 1. Retry Mechanism

### Definition

Retry failed requests automatically

```text
Request → Fail → Retry → Success
```

### Example

* Network timeout → retry after few milliseconds

---

## 2. Circuit Breaker

### Definition

Stop calling a failing service to **prevent cascading failure**

---

### Flow

```text
Service failing repeatedly
        ↓
Circuit opens (stop requests)
        ↓
After time → try again
```

---

### Tools

* Resilience4j
* Hystrix

---

## 3. Fallback

### Definition

Provide **alternative response when system fails**

---

### Example

```text
Product Service down
→ Show cached data or default message
```

---

## 4. Timeout

### Definition

Stop waiting after a certain time

```text
Request → wait 2 sec → timeout → fallback
```

---

## 5. Bulkhead Pattern

### Definition

Isolate components so failure in one does not affect others

---

### Example

```text
Separate thread pools:
- Payment service
- Recommendation service
```

---

## 6. Rate Limiting

### Definition

Limit number of requests to protect system

---

### Example

```text
Max 100 requests/sec
```

---

## 7. Redundancy

### Definition

Multiple instances of services

```text
Service A → Instance 1, 2, 3
```

---

## Visual Flow

```text
Client
  ↓
API Gateway
  ↓
Service A
  ↓
------------------------
| Retry / Timeout       |
| Circuit Breaker       |
| Fallback              |
------------------------
  ↓
Service B (may fail)
```

---

## Key Points

* Failures are **expected** in distributed systems
* Resiliency ensures **system stays functional**
* Improves **availability and reliability**

---

## When NOT to Overuse

* Too many retries → system overload
* Too many fallbacks → wrong user experience

---

## Common Mistakes

> ⚠️ No timeout → system hangs
> ⚠️ Infinite retries → cascading failure
> ⚠️ No circuit breaker → system collapse

---

## Architect Insight

* Combine techniques:

  * Retry + Timeout
  * Circuit Breaker + Fallback

> Rule:
> **Fail fast, recover smartly**

---

## Quick Summary

```text
Resiliency = System survives failure

Techniques:
- Retry
- Circuit Breaker
- Timeout
- Fallback
- Bulkhead
- Rate Limiting
```

---
# Designing Resilient Microservices (Real Architecture)

## Definition

Designing resilient microservices means building services that:

> **continue working (fully or partially) even when some components fail**

---

## Core Idea

> Expect failures → isolate them → recover gracefully → protect the system

---

## Example

```text id="y6d7n2"
User → Order Service → Payment Service → DB

If Payment fails:
→ Retry OR fallback
→ Order system does not crash
```

---

## Real-World Use Case

### E-commerce System

* User places order
* Payment service is slow/unavailable

**Resilient design:**

* Retry payment
* Queue request (process later)
* Show “pending” instead of failure

---

## Architecture Design

```text id="m7p6xf"
Client
  ↓
API Gateway
  ↓
----------------------------------------
| Order Service      → Redis (Cache)    |
| Payment Service    → DB               |
| Inventory Service  → DB               |
----------------------------------------
  ↓
Message Queue (Kafka)
  ↓
Async Processing
```

---

## Key Resiliency Patterns

---

## 1. Retry + Timeout

### How it works

```text id="4l0q4m"
Request → Timeout (2s) → Retry (3 times) → Fail
```

### Purpose

* Handle temporary failures
* Avoid long waits

---

## 2. Circuit Breaker

### How it works

```text id="2p7jhh"
Failures increase
      ↓
Circuit opens (stop calls)
      ↓
System uses fallback
```

### Tools

* Resilience4j
* Hystrix

---

## 3. Fallback Mechanism

### Example

```text id="km9fsy"
Recommendation Service fails
→ Show default recommendations
```

---

## 4. Bulkhead Isolation

### How it works

```text id="jlwm03"
Separate resources:
- Payment threads
- Recommendation threads
```

### Benefit

* One service failure doesn’t affect others

---

## 5. Asynchronous Communication

### Tools

* Apache Kafka

### Flow

```text id="nyxv4c"
Order placed
   ↓
Kafka Queue
   ↓
Payment processed later
```

---

## 6. Idempotency

### Definition

Same request → same result (no duplicates)

---

### Example

```text id="vv7m0y"
Payment API called twice
→ Only one payment processed
```

---

## 7. Health Checks & Auto Recovery

### Example

```text id="0t6q9r"
Service fails → Restart automatically
```

---

## 8. Distributed Caching

### Tools

* Redis

### Purpose

* Reduce dependency on DB
* Provide fallback data

---

## Visual Flow (Resilient System)

```text id="3g7yfe"
Client
  ↓
API Gateway
  ↓
Service A
  ↓
--------------------------------
| Retry / Timeout              |
| Circuit Breaker              |
| Fallback                     |
--------------------------------
  ↓
Service B (may fail)
  ↓
Message Queue (Kafka)
  ↓
Async Recovery
```

---

## Key Points

* Use **retry + timeout together**
* Always include **fallback**
* Prefer **async processing** for reliability
* Isolate failures using **bulkheads**

---

## Architect Insight

> Never trust external services → always protect your system

---

# High Availability vs Resiliency vs Fault Tolerance

## Definition

### High Availability (HA)

System is **always up and accessible**

---

### Resiliency

System **recovers from failures and continues working**

---

### Fault Tolerance

System continues working **without any interruption even during failure**

---

## Core Idea

```text id="6g9w4n"
Availability → System is reachable
Resiliency → System recovers
Fault Tolerance → System never stops
```

---

## Example

### High Availability

```text id="x8j2k3"
One server fails → traffic goes to another server
```

---

### Resiliency

```text id="7d4f2s"
Service fails → retry / fallback → recover
```

---

### Fault Tolerance

```text id="8h1k9q"
System continues seamlessly even if component fails
(no visible impact)
```

---

## Visual Comparison

```text id="3m8k2p"
Failure Occurs
      ↓
-----------------------------------------
HA  → Switch to another server
Res → Recover after failure
FT  → No impact at all
-----------------------------------------
```

---

## Key Differences

| Feature    | High Availability | Resiliency           | Fault Tolerance |
| ---------- | ----------------- | -------------------- | --------------- |
| Goal       | Stay available    | Recover from failure | No interruption |
| Downtime   | Minimal           | Short recovery time  | Zero downtime   |
| Complexity | Medium            | Medium               | High            |
| Cost       | Moderate          | Moderate             | High            |

---

## Real-World Mapping

| System Type  | Approach             |
| ------------ | -------------------- |
| Banking      | Fault Tolerance + HA |
| E-commerce   | HA + Resiliency      |
| Social Media | Resiliency           |

---

## Key Points

* HA = redundancy
* Resiliency = recovery
* Fault tolerance = no failure visible

---

## Common Mistake

> ⚠️ Treating all three as same
> → They solve different problems

---

## Architect Insight

* Most systems use:

  * **HA + Resiliency**
* Fault tolerance is used only where:

  * Failure is unacceptable (e.g., critical systems)

---

## Quick Summary

```text id="5v1r0k"
HA → System always available
Resiliency → Recovers from failure
Fault Tolerance → No interruption at all
```

---
