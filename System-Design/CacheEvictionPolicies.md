# Cache Eviction Policies (System Design)

## Definition

**Eviction Policy** defines **which data to remove from cache when it becomes full**.

---

## Core Idea

Cache has **limited memory**, so:

> When new data comes → some old data must be removed

The policy decides **what to remove**.

---

## Example

```text
Cache Size = 3

Stored:
[A, B, C]

New Data = D
→ One of A/B/C must be removed
```

---

## Real-World Use Case

* E-commerce → product cache
* Social media → feed cache
* APIs → response caching

---

## Types of Eviction Policies

---

## 1. LRU (Least Recently Used) ✅ Most Common

### Definition

Remove the data that was **not used recently**

---

### Core Idea

> Recently used data is more important

---

### Example

```text
Access Order:
A → B → C → A

Cache = [A, B, C]

New Data = D
→ Remove B (least recently used)
```

---

### Use Case

* Web apps
* User sessions
* API responses

---

### Key Point

* Best general-purpose policy

---

---

## 2. LFU (Least Frequently Used)

### Definition

Remove data that is used **least number of times**

---

### Core Idea

> Frequently accessed data should stay longer

---

### Example

```text
A (used 10 times)
B (used 2 times)
C (used 1 time)

New Data = D
→ Remove C
```

---

### Use Case

* Recommendation systems
* Analytics

---

### Key Point

* Good for stable access patterns

---

---

## 3. FIFO (First In First Out)

### Definition

Remove the **oldest inserted data**

---

### Core Idea

> First added → first removed

---

### Example

```text
Cache = [A, B, C]

New Data = D
→ Remove A
```

---

### Use Case

* Simple systems
* Queue-like behavior

---

### Key Point

* Easy but not efficient

---

---

## 4. TTL-Based Eviction

### Definition

Remove data after **fixed time (Time To Live)**

---

### Core Idea

> Data expires automatically

---

### Example

```text
Data cached at 10:00
TTL = 5 min
→ Removed at 10:05
```

---

### Use Case

* Sessions
* Temporary data
* API caching

---

### Key Point

* Controls **data freshness**

---

---

## 5. Random Eviction

### Definition

Remove **random data**

---

### Core Idea

> Simple but unpredictable

---

### Use Case

* Rarely used
* When simplicity matters

---

---

## Visual Comparison

```text
Policy   → Removes
-------------------------
LRU      → Least recently used
LFU      → Least frequently used
FIFO     → Oldest data
TTL      → Expired data
Random   → Any random data
```

---

## Key Points

* LRU is **most commonly used**
* LFU is useful for **highly repeated access patterns**
* TTL helps avoid **stale data**
* Choice depends on **access pattern**

---

## Common Mistakes

> ⚠️ Using wrong policy → poor cache performance
> ⚠️ Ignoring TTL → stale data issues

---

## Architect Insight

* Default choice → **LRU + TTL combination**
* For high-scale systems:

  * Combine **LRU (space) + TTL (freshness)**

> Rule:
> **Keep hot data, remove cold data**

---

## Quick Summary

```text
Eviction Policy = What to remove when cache is full

Best default:
- LRU + TTL

Choose based on:
- Access frequency
- Data freshness
```

---

## Tools Support

* Redis → Supports LRU, LFU, TTL
* Memcached → Supports LRU

---
