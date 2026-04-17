# SAGA Pattern (Distributed Transactions)

## Definition

The **Saga Pattern** is a way to manage **transactions across multiple microservices** without using a single global transaction.

> Break one big transaction into **multiple smaller local transactions**, each with a **compensation (undo) step**

---

## Core Idea

> If one step fails → **undo all previous successful steps**

This replaces traditional ACID transactions in distributed systems.

---

## Example

### Order Creation Flow

```text id="2fxn3a"
1. Create Order
2. Deduct Inventory
3. Process Payment
```

---

### Failure Scenario

```text id="p7f9z1"
Payment fails ❌
→ Undo Inventory (add stock back)
→ Cancel Order
```

---

## Real-World Use Case

### E-commerce System

* Order Service
* Inventory Service
* Payment Service

Each service:

* Performs its own transaction
* Has a **compensation action**

---

## Types of Saga

---

## 1. Choreography-Based Saga

### Definition

* Services communicate via **events**
* No central controller

---

### Flow

```text id="h7z2yx"
Order Service → Event: Order Created
        ↓
Inventory Service → Deduct stock → Event
        ↓
Payment Service → Process payment
```

---

### Failure Handling

```text id="o3w4jb"
Payment fails
→ Emit failure event
→ Inventory restores stock
→ Order cancels
```

---

### Tools

* Apache Kafka

---

### Pros

* Decoupled
* Scalable

### Cons

* Hard to track flow
* Debugging complex

---

---

## 2. Orchestration-Based Saga

### Definition

* A central **orchestrator service** controls the flow

---

### Flow

```text id="l6z9op"
Orchestrator
   ↓
Order Service
   ↓
Inventory Service
   ↓
Payment Service
```

---

### Failure Handling

```text id="y4q8vd"
Payment fails
→ Orchestrator triggers:
   - Undo Inventory
   - Cancel Order
```

---

### Tools

* Camunda
* Temporal

---

### Pros

* Easy to manage
* Clear flow

### Cons

* Central dependency
* Less flexible

---

## Visual Comparison

```text id="2s8kdn"
Choreography:
Service → Event → Service → Event

Orchestration:
Orchestrator → Controls all services
```

---

## Key Components

### 1. Local Transactions

* Each service updates its own DB

---

### 2. Events / Commands

* Communication between services

---

### 3. Compensation Actions

* Undo operations

---

## Key Points

* No global transaction
* Uses **eventual consistency**
* Handles **distributed failures gracefully**
* Essential for **microservices architecture**

---

## When to Use

* Multiple services involved in one business flow
* No distributed transaction support
* Need scalability

---

## When NOT to Use

* Simple monolithic systems
* Strong consistency required instantly

---

## Common Mistakes

> ⚠️ Forgetting compensation logic
> ⚠️ Not handling partial failures
> ⚠️ Poor event design

---

## Architect Insight

* Prefer:

  * **Choreography** → for simple, scalable flows
  * **Orchestration** → for complex business logic

> Rule:
> **Every step must have an undo step**

---

## Quick Summary

```text id="5l0zv2"
Saga = Distributed transaction pattern

Types:
- Choreography (event-driven)
- Orchestration (central control)

Key:
- Local transactions
- Compensation (undo)
```
