# ACID Compliance (System Design / Databases)

## Definition

**ACID** is a set of 4 properties that ensure **reliable and consistent database transactions**.

* **A** → Atomicity
* **C** → Consistency
* **I** → Isolation
* **D** → Durability

---

## Core Idea

When multiple operations happen in a transaction, ACID ensures:

> **Either everything succeeds OR nothing changes — and data always stays correct**

---

## Example

### Bank Transfer

Transfer ₹100 from Account A → Account B

```text
Step 1: Deduct ₹100 from A
Step 2: Add ₹100 to B
```

If Step 2 fails → Step 1 must be **reversed**

---

## Real-World Use Case

### Banking System

* Money transfer must be **accurate and safe**
* No partial updates allowed
* Even if system crashes, data must remain correct

Used in:

* Banking systems
* Payment systems
* Order processing systems

---

## ACID Properties Explained

### 1. Atomicity (All or Nothing)

* Transaction is treated as a **single unit**
* If one step fails → **rollback everything**

**Example:**

```text
Deduct money ✔
Add money ❌
→ Entire transaction cancelled
```

---

### 2. Consistency (Valid Data Only)

* Database always stays in a **valid state**
* Rules (constraints) must not break

**Example:**

* Balance cannot go negative
* Foreign key must exist

---

### 3. Isolation (No Interference)

* Transactions run **independently**
* One transaction should not see **partial data** of another

**Example:**

* User A transfer is not visible to User B until completed

---

### 4. Durability (Permanent Save)

* Once committed → data is **never lost**
* Even if system crashes

**Example:**

* After payment success → data stored safely on disk

---

## Visual Flow

```text
Start Transaction
        ↓
 Perform Operations
 (Multiple Steps)
        ↓
 All Successful?
    ↓         ↓
  Yes         No
   ↓           ↓
Commit      Rollback
   ↓           ↓
Data Saved   Undo Changes
```

---

## Key Points

* Ensures **data reliability and correctness**
* Used in **RDBMS systems** (MySQL, PostgreSQL, Oracle)
* Critical for **financial and transactional systems**
* Prevents **data corruption and inconsistency**

---

## When NOT to Use (Relaxed ACID)

* In **high-scale distributed systems** (like NoSQL)
* When performance > strict consistency

Example:

* Social media feeds
* Logging systems

---

## Common Mistake

> ⚠️ Assuming all databases fully support ACID

* Some NoSQL systems follow **BASE** (eventual consistency) instead

---

## Quick Comparison

| Property    | Meaning           |
| ----------- | ----------------- |
| Atomicity   | All or nothing    |
| Consistency | Valid data only   |
| Isolation   | No interference   |
| Durability  | Data is permanent |

---

## Architect Insight

* Use ACID when:

  * **Money / transactions involved**
  * **Data correctness is critical**

* Avoid strict ACID when:

  * You need **high scalability and performance**

> Trade-off:
> **Consistency vs Scalability**

# Isolation Levels (Read Uncommitted → Serializable)

## Definition

**Isolation Levels** define **how much one transaction can see data from another transaction while both are running**.

---

## Core Idea

When multiple transactions run at the same time:

> Control **data visibility** to avoid conflicts and inconsistencies

Higher isolation → More safety, less performance
Lower isolation → Better performance, more risk

---

## Example

Two users accessing same account:

```text
User A → Updating balance
User B → Reading balance
```

Isolation level decides:

* Can B see A’s incomplete update?
* Or only final committed data?

---

## Types of Isolation Levels

### 1. Read Uncommitted (Lowest)

* Can read **uncommitted (dirty) data**

**Problem:** Dirty Read

```text
A updates balance → not committed
B reads it → WRONG value
```

---

### 2. Read Committed

* Can only read **committed data**

**Prevents:** Dirty Read
**Still possible:** Non-repeatable read

```text
B reads balance = 100
A updates to 200 and commits
B reads again = 200 (changed)
```

---

### 3. Repeatable Read

* Same query returns **same result within a transaction**

**Prevents:** Dirty Read + Non-repeatable Read
**Still possible:** Phantom Read

```text
B reads rows (count = 10)
A inserts new row
B reads again → count = 11 (new row appeared)
```

---

### 4. Serializable (Highest)

* Transactions behave like **executed one-by-one**

**Prevents:** All issues

* Dirty Read
* Non-repeatable Read
* Phantom Read

**Trade-off:** Slowest but safest

---

## Visual Flow

```text
Isolation Level ↑
-------------------------
Serializable     → Full safety (slow)
Repeatable Read  → Medium-high safety
Read Committed   → Basic safety
Read Uncommitted → No safety (fast)
-------------------------
Performance ↑ (reverse direction)
```

---

## Key Problems (Important)

* **Dirty Read** → Reading uncommitted data
* **Non-repeatable Read** → Same query gives different results
* **Phantom Read** → New rows appear during transaction

---

## Real-World Use Case

* Banking → **Serializable / Repeatable Read**
* E-commerce orders → **Read Committed**
* Analytics/logging → **Read Uncommitted (rarely used)**

---

## Key Points

* Higher isolation = **more locking, less concurrency**
* Lower isolation = **better performance, more risk**
* Most systems use **Read Committed (balanced)**

---

## Architect Insight

* Choose isolation based on **business criticality**
* Don’t always use highest level → it impacts scalability

---

# ACID vs BASE (Very Important)

## Definition

### ACID

Ensures **strong consistency and reliability**

* Atomicity, Consistency, Isolation, Durability

### BASE

Used in distributed systems for scalability:

* **Basically Available**
* **Soft state**
* **Eventual consistency**

---

## Core Idea

> ACID → **Consistency first**
> BASE → **Availability & Scalability first**

---

## Example

### ACID (Banking)

```text
Transfer ₹100
→ Must be 100% correct immediately
```

### BASE (Social Media)

```text
Like a post
→ Count may update after few seconds (acceptable)
```

---

## Real-World Use Case

### ACID Systems

* Banking
* Payment gateways
* Order transactions

### BASE Systems

* Social media feeds
* Notifications
* Analytics systems

---

## Visual Flow

```text
ACID System:
User Action → Immediate Consistent Data

BASE System:
User Action → Temporary inconsistency → Eventually consistent
```

---

## Key Differences

| Feature      | ACID                   | BASE                         |
| ------------ | ---------------------- | ---------------------------- |
| Consistency  | Strong                 | Eventual                     |
| Availability | Medium                 | High                         |
| Performance  | Lower                  | Higher                       |
| Use Case     | Transactions (banking) | Large-scale distributed apps |

---

## Tools / Systems

### ACID Databases

* MySQL
* PostgreSQL
* Oracle Database

### BASE / NoSQL Systems

* MongoDB
* Cassandra
* DynamoDB

---

## Key Points

* ACID = **Correctness first**
* BASE = **Scale first**
* BASE accepts **temporary inconsistency**
* Modern systems often use **hybrid approach**

---

## Architect Insight

* Use ACID for:

  * Payments
  * Orders
  * Financial data

* Use BASE for:

  * High-scale apps
  * Real-time feeds
  * Distributed systems

> Trade-off:
> **Consistency vs Availability (CAP Theorem)**
