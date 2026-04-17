# Event-Driven Architecture (EDA)

## Definition

**Event-Driven Architecture** is a design where services communicate by **producing and consuming events**, instead of calling each other directly.

> An **event** = something that already happened (e.g., *OrderPlaced*, *PaymentCompleted*)

---

## Core Idea

> **Producer emits event → Consumers react independently**

* Loose coupling
* Asynchronous communication
* High scalability

---

## Example

```text
User places order

Order Service → emits "OrderCreated" event
        ↓
Inventory Service → deducts stock
        ↓
Payment Service → processes payment
        ↓
Notification Service → sends email
```

---

## Real-World Use Case

### E-commerce Platform

* Order created
* Multiple services react independently:

  * Inventory updates stock
  * Payment processes transaction
  * Email service sends confirmation

👉 No direct service-to-service calls

---

## Key Components

---

### 1. Event Producer

* Service that **creates event**

Example:

* Order Service emits `OrderCreated`

---

### 2. Event Consumer

* Service that **reacts to event**

Example:

* Payment Service listens to `OrderCreated`

---

### 3. Event Broker

* Middleware that **routes events**

---

### Tools

* Apache Kafka
* RabbitMQ

---

### 4. Event

* Message containing:

  * What happened
  * Metadata

Example:

```json
{
  "event": "OrderCreated",
  "orderId": "123",
  "amount": 500
}
```

---

## Visual Flow

```text
Producer (Order Service)
        ↓
Event Broker (Kafka)
        ↓
---------------------------------
| Inventory Service             |
| Payment Service               |
| Notification Service          |
---------------------------------
```

---

## Types of Event Models

---

## 1. Publish-Subscribe (Pub/Sub)

### Definition

* One event → multiple consumers

---

### Flow

```text
OrderCreated Event
        ↓
Multiple Services receive it
```

---

### Use Case

* Notifications
* Logging
* Analytics

---

---

## 2. Event Streaming

### Definition

* Continuous stream of events

---

### Example

* User activity tracking
* Real-time analytics

---

---

## 3. Event Sourcing

### Definition

* Store **all events instead of current state**

---

### Example

```text
Account Balance:
+100 → -50 → +200
```

Instead of storing final value, store all events

---

## Benefits

* Complete history
* Easy debugging

---

## Visual Comparison

```text
Traditional:
Service → Direct API → Service

Event-Driven:
Service → Event → Broker → Multiple Services
```

---

## Key Benefits

* Loose coupling (services independent)
* High scalability
* Easy to add new services
* Supports async processing

---

## Challenges

### 1. Eventual Consistency

* Data may not update instantly

---

### 2. Debugging Complexity

* Hard to track event flow

---

### 3. Duplicate Events

* Same event may be processed multiple times

---

### 4. Ordering Issues

* Events may arrive out of order

---

## Design Patterns Used

### 1. Saga Pattern

* Manage distributed transactions

---

### 2. Idempotency

* Same event → safe to process multiple times

---

### 3. Dead Letter Queue (DLQ)

* Store failed events

---

### 4. Retry Mechanism

* Retry failed processing

---

## Visual Flow (Advanced)

```text
Producer
   ↓
Event Broker (Kafka)
   ↓
Consumer 1 → Success
Consumer 2 → Fail → Retry → DLQ
Consumer 3 → Success
```

---

## Key Points

* No direct communication between services
* Events drive system behavior
* Highly scalable and flexible
* Works well with microservices

---

## When NOT to Use

* Simple systems
* Strong immediate consistency required

---

## Architect Insight

* Use EDA when:

  * System is large and distributed
  * High scalability needed

> Rule:
> **Design events carefully — they are your contracts**

---

## Quick Summary

```text
EDA = Communication via events

Flow:
Producer → Broker → Consumers

Tools:
- Kafka
- RabbitMQ

Benefits:
- Scalable
- Decoupled
- Async
```

---
