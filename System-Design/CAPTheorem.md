# CAP Theorem (System Design)

## Definition

**CAP Theorem** states that a distributed system can guarantee **only 2 out of these 3 properties at the same time**:

* **C → Consistency**
* **A → Availability**
* **P → Partition Tolerance**

---

## Core Idea

In distributed systems (multiple servers, networks can fail):

> When a **network partition happens**, you must choose:
>
> * **Consistency (correct data)**
>   OR
> * **Availability (always respond)**

You **cannot guarantee both simultaneously**.

---

## Example

```text
Two servers (Node A, Node B)

Network failure occurs (Partition)

Now:
User 1 → hits Node A
User 2 → hits Node B
```

System must decide:

* Show same data? (Consistency)
* Or always respond even if data differs? (Availability)

---

## CAP Properties Explained

### 1. Consistency (C)

* All users see **same latest data**
* Every read returns **most recent write**

**Example:**

```text
Balance updated to ₹500
→ Every user sees ₹500 immediately
```

---

### 2. Availability (A)

* System **always responds**
* Even if data is outdated

**Example:**

```text
Server responds with old balance (₹400)
→ But does not fail
```

---

### 3. Partition Tolerance (P)

* System continues working **even if network fails between nodes**

**Example:**

```text
Node A ↔ Node B connection lost
→ System still operates
```

---

## Visual Flow

```text
           +------------------+
           |  Distributed     |
           |   System         |
           +------------------+
                    ↓
          Network Partition Happens
                    ↓
        Choose ONE:
        -------------------------
        CP → Consistency + Partition
        AP → Availability + Partition
        -------------------------
        (CA not practical in real systems)
```

---

## Types of Systems

### 1. CP (Consistency + Partition Tolerance)

* Prioritize **correct data**
* May **reject requests** (reduce availability)

**Used in:**

* HBase
* MongoDB (in strict modes)

---

### 2. AP (Availability + Partition Tolerance)

* Always respond
* Data may be **temporarily inconsistent**

**Used in:**

* Cassandra
* DynamoDB

---

### 3. CA (Consistency + Availability)

* No partition tolerance (not realistic in distributed systems)

**Used in:**

* Traditional single-node databases
* MySQL (non-distributed setup)

---

## Real-World Use Case

### Banking System → CP

* Must ensure **correct balance**
* Better to fail than show wrong data

---

### Social Media → AP

* Feed can show slightly old data
* But app must **always be available**

---

## Key Points

* Partition tolerance is **mandatory** in distributed systems

* So real choice is:

  > **Consistency vs Availability**

* CP → Safe but may fail

* AP → Fast but eventually consistent

---

## Common Mistake

> ⚠️ Thinking you can have all 3 (C + A + P)

* Not possible when network partition occurs

---

## Architect Insight

* Choose based on **business requirement**

| Use Case        | Choice |
| --------------- | ------ |
| Payments        | CP     |
| Orders          | CP     |
| Social feeds    | AP     |
| Logging/metrics | AP     |

---

## Quick Summary

```text
CAP Theorem:
You can choose only 2:

C + P → Consistent but may reject requests
A + P → Always available but may show stale data
```

---
# Designing Systems Using CAP (Real Architecture Decisions)

## Definition

Designing with **CAP Theorem** means choosing between:

> **Consistency (C)** vs **Availability (A)**
> while always handling **Partition Tolerance (P)** in distributed systems

---

## Core Idea

In real systems:

> **Network failures WILL happen → Partition Tolerance is mandatory**

So the real decision is:

* **CP → Strong correctness**
* **AP → High availability**

---

## Step-by-Step Design Approach

### Step 1: Identify Business Requirement

Ask:

| Question                        | If YES → Choose |
| ------------------------------- | --------------- |
| Is wrong data unacceptable?     | CP              |
| Can system tolerate stale data? | AP              |
| Is uptime critical?             | AP              |
| Is correctness critical?        | CP              |

---

## Example

```text
Payment System:
Wrong balance = ❌ Not acceptable
→ Choose CP

Social Feed:
Slight delay in likes = ✅ Acceptable
→ Choose AP
```

---

## Real-World Architecture Decisions

## 1. CP System Design (Consistency First)

### Use Case

* Payments
* Banking
* Order transactions

### Tools

* MySQL (clustered)
* PostgreSQL
* ZooKeeper

---

### Architecture Flow

```text
Client Request
      ↓
Load Balancer
      ↓
Primary Database (Leader)
      ↓
Replicas (Sync Replication)
      ↓
If partition occurs → Reject requests
```

---

### Key Behavior

* Ensures **strong consistency**
* Uses **leader-based replication**
* May return **errors during failure**

---

## 2. AP System Design (Availability First)

### Use Case

* Social media
* Notifications
* Analytics systems

### Tools

* Cassandra
* DynamoDB

---

### Architecture Flow

```text
Client Request
      ↓
Load Balancer
      ↓
Multiple Nodes (No strict leader)
      ↓
Write to any node
      ↓
Async replication
      ↓
Eventually consistent
```

---

### Key Behavior

* Always responds (**high availability**)
* Data may be **temporarily inconsistent**
* Uses **eventual consistency**

---

## 3. Hybrid Architecture (Most Real Systems)

> Modern systems **mix CP and AP**

---

### Example: E-commerce System

| Component         | CAP Choice |
| ----------------- | ---------- |
| Payments          | CP         |
| Orders            | CP         |
| Product Catalog   | AP         |
| Recommendations   | AP         |
| Logging/Analytics | AP         |

---

### Architecture Flow

```text
User Request
      ↓
API Gateway
      ↓
-------------------------------
| Payment Service  → CP DB    |
| Order Service    → CP DB    |
| Product Service  → AP DB    |
| Recommendation   → AP DB    |
-------------------------------
      ↓
Final Response
```

---

## Design Patterns Used

### 1. Leader-Based Replication (CP)

* One **leader node**
* All writes go through leader
* Ensures consistency

---

### 2. Quorum-Based Systems (Balanced)

* Require majority (quorum) for read/write
* Balance between C and A

Example:

* Cassandra (tunable consistency)

---

### 3. Eventual Consistency (AP)

* Updates propagate asynchronously
* System becomes consistent over time

---

### 4. Retry & Idempotency

* Handle failures gracefully
* Avoid duplicate operations

---

## Visual Flow (Decision Logic)

```text
Is system distributed?
        ↓
      YES
        ↓
Partition will happen
        ↓
-------------------------
| Need strict accuracy? |
-------------------------
     ↓ Yes        ↓ No
     CP           AP
```

---

## Key Points

* You **cannot avoid partition tolerance**
* Choose based on **business criticality**
* Most systems use **hybrid CAP strategy**
* CAP is about **failure handling**, not normal operation

---

## Common Mistakes

> ⚠️ Using CP everywhere → system becomes slow/unavailable
> ⚠️ Using AP for critical data → leads to inconsistency issues

---

## Architect Insight

* Don’t design entire system as CP or AP
* Design **component-wise CAP strategy**

> **Critical services → CP**
> **Scalable services → AP**

---

## Quick Summary

```text
CAP Design Rule:

Payments / Orders → CP
Feeds / Logs / Analytics → AP

Modern systems → Hybrid
```
