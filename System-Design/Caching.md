# Caching (System Design)

## Definition

**Caching** is a technique of storing **frequently accessed data in a fast storage layer** so future requests can be served **quickly without hitting the main database**.

---

## Core Idea

> **Avoid repeated expensive operations by storing results temporarily**

* Reduce database load
* Improve response time
* Increase system performance

---

## Example

```text
Without Cache:
User → Request → Database → Response (slow)

With Cache:
User → Request → Cache → Response (fast)
                 ↓ (if miss)
               Database
```

---

## Real-World Use Case

### E-commerce Website

* Product details (name, price, images) are frequently accessed

**With caching:**

* First request → fetched from DB and stored in cache
* Next requests → served from cache (very fast)

**Result:**

* Faster page load
* Reduced DB queries
* Better scalability

---

## Types of Caching (Important)

### 1. Application Cache

* Stored inside application memory
* Fast but limited

### 2. Distributed Cache

* Shared across multiple servers
* Scalable and commonly used

### 3. CDN Cache

* Stores static content near users (images, videos)

---

## Technologies (with simple meaning)

### 1. In-Memory Cache

* Redis
  → Super fast, stores data in RAM (most popular)

* Memcached
  → Simple key-value cache, lightweight

---

### 2. Distributed Cache / Cloud

* Amazon ElastiCache
  → Managed Redis/Memcached on AWS

* Hazelcast
  → Distributed cache + computation

---

### 3. CDN (Static Content Cache)

* Cloudflare
  → Caches content globally

* Akamai
  → Enterprise-level CDN

---

## Visual Flow

```text
User Request
      ↓
   Cache Layer
   (Redis)
   ↓       ↓
 Hit       Miss
 ↓          ↓
Response   Database
             ↓
        Store in Cache
             ↓
          Response
```

---

## Key Concepts

### Cache Hit

* Data found in cache → fast response

### Cache Miss

* Data not in cache → fetch from DB

### TTL (Time To Live)

* Cache expires after certain time

---

## Key Points

* Improves **performance & latency**
* Reduces **database load**
* Works best for **read-heavy systems**
* Data is **temporary (not source of truth)**

---

## When NOT to Use

* Highly dynamic data (changes frequently)
* Strong consistency required (banking transactions)

---

## Common Mistakes

> ⚠️ Not invalidating cache → stale data issues
> ⚠️ Caching everything → memory waste

---

## Architect Insight

* Use cache for:

  * Frequently accessed data
  * Read-heavy workloads

* Follow rule:

  > **Cache = optimization layer, not primary storage**

---

## Quick Summary

```text
Cache = Fast storage for repeated data

Benefits:
- Faster response
- Less DB load

Tools:
- Redis (most popular)
- Memcached
- CDN (Cloudflare)
```

---
# Cache Strategies (Write-through, Write-back, Cache-aside)

## Definition

**Cache strategies** define **how data flows between Cache and Database** during read/write operations.

---

## Core Idea

> Decide **when to update cache vs database** to balance **performance vs consistency**

---

## 1. Cache-Aside (Lazy Loading) ✅ Most Common

### How it works

* Application checks cache first
* If **miss → fetch from DB → update cache**

### Flow

```text
Read:
User → Cache → (Miss) → DB → Cache → Response

Write:
User → DB → (Invalidate cache)
```

---

### Example

```java
String data = redis.get(key);
if (data == null) {
    data = db.get(key);
    redis.set(key, data);
}
return data;
```

---

### Real-World Use Case

* Product details
* User profiles
* Read-heavy systems

---

### Pros

* Simple and flexible
* Cache only what is needed

### Cons

* First request is slow (cache miss)
* Risk of stale data

---

## 2. Write-Through

### How it works

* Write goes to **cache AND database together**

### Flow

```text
Write:
User → Cache → DB

Read:
User → Cache → Response
```

---

### Example

```java
redis.set(key, value);
db.save(key, value);
```

---

### Real-World Use Case

* Systems where **consistency is important**
* Session data, user preferences

---

### Pros

* Cache always up-to-date
* No stale reads

### Cons

* Slower writes
* Unnecessary cache writes

---

## 3. Write-Back (Write-Behind)

### How it works

* Write goes to **cache first**
* DB updated **later asynchronously**

### Flow

```text
Write:
User → Cache → (Async) → DB

Read:
User → Cache → Response
```

---

### Example

```text
User updates value → stored in Redis
→ Background worker updates DB later
```

---

### Real-World Use Case

* High-performance systems
* Logging, analytics

---

### Pros

* Very fast writes
* Reduced DB load

### Cons

* Risk of data loss (if cache fails)
* Complex to manage

---

## Visual Comparison

```text
Cache-Aside:
App ↔ Cache ↔ DB

Write-Through:
App → Cache → DB

Write-Back:
App → Cache → (Async) → DB
```

---

## Key Points

* **Cache-aside → most widely used**
* **Write-through → strong consistency**
* **Write-back → high performance**

---

## Architect Insight

* Default choice → **Cache-aside**
* Use write-through if **data must always be fresh**
* Use write-back for **high write throughput systems**

---

---

# Designing Caching in Microservices (Real Architecture)

## Definition

Using cache in microservices means placing a **fast data layer between services and databases** to improve performance and scalability.

---

## Core Idea

> Each service should **own its cache** and reduce direct database dependency

---

## Example

```text id="6g6lke"
User Service → frequently accessed user data
Product Service → product details
Order Service → transaction data (less caching)
```

---

## Architecture Design

### 1. Basic Microservices with Cache

```text
Client
  ↓
API Gateway
  ↓
----------------------------
| User Service   → Redis   |
| Product Service→ Redis   |
| Order Service  → DB only |
----------------------------
```

---

### 2. Read Flow

```text
Client → Service → Cache
                  ↓       ↓
                Hit      Miss
                 ↓         ↓
             Response     DB
                            ↓
                        Update Cache
```

---

### 3. Write Flow (Cache-Aside)

```text
Client → Service → DB
                  ↓
            Invalidate Cache
```

---

## Tools Used

* Redis → Most common
* Memcached
* Amazon ElastiCache

---

## Design Patterns

### 1. Per-Service Cache (Recommended)

* Each service has its own cache
* Avoid shared cache across services

---

### 2. TTL-Based Expiry

* Cache auto-expires after time

---

### 3. Cache Invalidation

* On update → remove cache

---

### 4. Read-Through Optimization

* Cache layer automatically fetches data

---

## Real-World Example

### E-commerce System

```text
Product Service:
- Cache product details (high read)

User Service:
- Cache user profiles

Order Service:
- Minimal caching (critical data)
```

---

## Key Challenges

### 1. Cache Invalidation

> Hardest problem in caching

* When to update/delete cache?

---

### 2. Stale Data

* Data may be outdated

---

### 3. Cache Consistency

* Multiple services → synchronization issues

---

## Key Points

* Use cache for **read-heavy services**
* Avoid caching **critical transactional data**
* Prefer **Redis for distributed caching**
* Use **TTL + invalidation strategy**

---

## Architect Insight

* Don’t overuse caching
* Cache only:

  * Frequently accessed data
  * Expensive queries

> Rule:
> **If DB can handle it → don’t cache unnecessarily**

---

## Quick Summary

```text
Cache Strategy:
- Cache-aside → default
- Write-through → consistency
- Write-back → performance

Microservices:
- Per-service cache
- Redis is standard
- Use TTL + invalidation
```

---
# Caching – Important Concepts Explained

---

## 1. Horizontal Scaled Servers

## Definition

**Horizontal scaling** means adding **more servers (instances)** instead of increasing power of one server.

---

## Core Idea

> Distribute load across multiple servers to improve scalability

---

## Example

```text
Without Scaling:
User → Single Server → Overloaded

With Horizontal Scaling:
User → Load Balancer → Server1 / Server2 / Server3
```

---

## Real-World Use Case

* High-traffic apps (e-commerce, social media)
* Each server may have its own cache → improves performance

---

## Key Point

* Requires **distributed caching (like Redis cluster)** to share data across servers

---

## 2. Client Hash Request to a Given Server

## Definition

A technique where **client request is mapped to a specific server using hashing**.

---

## Core Idea

> Same request → same server → better cache utilization

---

## Example

```text
UserID = 123
Hash(123) → Server 2

User always routed to Server 2
```

---

## Real-World Use Case

* Session-based applications
* Reduces cache misses

---

## Visual Flow

```text
User Request
     ↓
 Hash Function
     ↓
Specific Server
     ↓
 Local Cache Hit
```

---

## Key Point

* Improves **cache locality**
* Used in **consistent hashing**

---

## 3. Appropriate for Applications with More Reads than Writes

## Definition

Caching works best when **read operations are much higher than write operations**.

---

## Core Idea

> Cache avoids repeated reads, but frequent writes make cache invalidation costly

---

## Example

```text
Product Page:
Reads → 10,000 times
Writes → 10 times

→ Perfect for caching
```

---

## Real-World Use Case

* Product catalogs
* News websites
* Social media feeds

---

## Key Point

* Write-heavy systems → caching less effective

---

## 4. Expiration Policy (TTL)

## Definition

Defines **how long data stays in cache before it expires**

---

## Core Idea

> Balance between **fresh data vs performance**

---

## Example

```text
TTL = 5 minutes

Data cached at 10:00
Expires at 10:05
```

---

## Real-World Use Case

* Stock prices → short TTL
* Product details → longer TTL

---

## Trade-Off

```text
Long TTL  → Faster but stale data
Short TTL → Fresh but more DB load
```

---

## Key Point

* Choosing TTL is **critical for system design**

---

## 5. Hotspots Problem (Celebrity Problem)

## Definition

When **one piece of data is requested by many users at the same time**, creating a hotspot.

---

## Core Idea

> One key gets extremely high traffic → overloads cache/server

---

## Example

```text
Trending Product / Viral Post

Millions of users → same cache key
→ Server overloaded
```

---

## Real-World Use Case

* Viral tweets/posts
* Flash sales
* Live events

---

## Visual Flow

```text
Many Users
    ↓↓↓↓↓
   Same Key
    ↓
 Cache Server Overload
```

---

## Solutions

* Replicate cache
* Use CDN
* Load balancing
* Rate limiting

---

## 6. Cold Start Problem (Cache Warm-Up)

## Definition

When cache is **empty initially**, causing all requests to hit the database.

---

## Core Idea

> First traffic spike → DB overload (cache not ready)

---

## Example

```text
System Restart

Cache = Empty
All users → DB
→ High load / possible failure
```

---

## Real-World Use Case

* Application restart
* New deployment

---

## Visual Flow

```text
Cache Empty
    ↓
All Requests → DB
    ↓
Cache gradually fills
```

---

## Solutions

* Preload cache (warm-up scripts)
* Lazy loading (cache-aside)
* Background jobs to fill cache
* Gradual traffic ramp-up

---

## Key Point

> Proper cache warm-up strategy prevents system overload

---

## Final Summary

```text
Horizontal Scaling → More servers for load
Hashing → Same user → same server
Best Use → Read-heavy systems
TTL → Controls freshness vs performance
Hotspot → Too many requests for one key
Cold Start → Empty cache overloads DB
```

---

## Architect Insight

* Caching is not just performance → it’s **distributed system design problem**
* Must handle:

  * Load distribution
  * Consistency
  * Failure scenarios

---

# Cache Stampede & Thundering Herd (Critical Problems)

## Definition

* **Cache Stampede**: Many requests hit the system when a cache entry **expires or is missing**, causing a surge to the database.
* **Thundering Herd**: A large number of clients wake up or retry **at the same time**, overwhelming the system.

---

## Core Idea

> When cache fails (miss/expiry), **all requests fall back to DB simultaneously → overload**

---

## Example

```text
Cache TTL expires for "Product:123"

1000 users request same data
        ↓
Cache Miss (all)
        ↓
All hit Database
        ↓
DB overload / latency spike
```

---

## Real-World Use Case

* Flash sales
* Viral content (trending product/post)
* High-traffic APIs

---

## Visual Flow

```text
Users (many)
   ↓↓↓↓↓
Cache (expired)
   ↓
DB (overloaded)
```

---

## Solutions (Very Important)

### 1. Cache Locking (Mutex)

* First request fetches from DB
* Others wait

```text
1 request → DB
Others → wait → get cached result
```

---

### 2. Randomized TTL (Jitter)

* Avoid same expiry time

```text
TTL = 5 min ± random
```

---

### 3. Early Refresh (Refresh Ahead)

* Refresh cache **before expiry**

---

### 4. Request Coalescing

* Combine multiple requests into one

---

### 5. Rate Limiting / Circuit Breaker

* Protect DB from overload

---

## Key Points

* Happens during **cache miss or expiry spikes**
* Can crash database if not handled
* Must design **defensive caching strategy**

---

## Architect Insight

> Always assume cache can fail → design fallback carefully

---

# Redis Internal Working (Deep Understanding)

## Definition

Redis is an **in-memory data store** that provides **ultra-fast read/write operations**.

---

## Core Idea

> Store data in **RAM instead of disk** → extremely fast access

---

## How Redis Works (High-Level)

### 1. In-Memory Storage

* Data stored in RAM
* Key-value format

```text
"user:1" → {name: "Tushar"}
```

---

### 2. Single-Threaded Event Loop

* Uses **single thread** for commands
* Avoids locking → fast and simple

---

### 3. Event Loop Flow

```text
Client Request
      ↓
 Event Loop (Queue)
      ↓
 Execute Command
      ↓
 Return Response
```

---

### 4. Persistence (Optional)

* Data can be saved to disk:

#### RDB (Snapshot)

* Periodic snapshots

#### AOF (Append Only File)

* Logs every write

---

### 5. Data Structures

Redis supports:

* Strings
* Lists
* Sets
* Hashes
* Sorted Sets

---

### 6. Replication

```text
Primary (Master)
      ↓
 Replicas (Slaves)
```

* Read scaling
* Fault tolerance

---

### 7. Pub/Sub & Streams

* Messaging support
* Real-time systems

---

## Visual Flow

```text
Client
  ↓
Redis (RAM)
  ↓
(Optional Persistence)
  ↓
Disk (RDB / AOF)
```

---

## Key Points

* Extremely fast (in-memory)
* Single-threaded but highly efficient
* Supports persistence + replication
* Widely used for caching

---

## Architect Insight

* Redis is not just cache → it’s a **data platform**
* Use for:

  * Caching
  * Rate limiting
  * Session store
  * Pub/Sub

---

# How Redis Implements LRU / LFU (Deep Dive)

## Core Idea

Redis does **approximate LRU/LFU**, not perfect → to keep performance high

---

## 1. LRU in Redis

### Definition

Removes **least recently used keys**

---

### How Redis Actually Does It

> Uses **sampling instead of tracking every access**

---

### Process

```text
1. Randomly pick few keys (sample)
2. Check last access time
3. Remove least recently used among them
```

---

### Why Not Exact LRU?

* Exact LRU requires heavy tracking → slow
* Redis optimizes for **speed**

---

## 2. LFU in Redis

### Definition

Removes **least frequently used keys**

---

### How Redis Implements LFU

Each key stores:

* Access frequency counter
* Decay over time

---

### Process

```text
1. Increment counter on access
2. Counters decay over time (aging)
3. Sample keys
4. Remove least frequently used
```

---

### Key Feature

* Prevents old popular keys from staying forever

---

## Visual Flow

```text
Cache Full
   ↓
Random Sampling
   ↓
Check LRU / LFU
   ↓
Evict Selected Key
```

---

## Key Points

* Redis uses **approximate algorithms (fast)**
* Sampling-based eviction
* LFU includes **decay mechanism**
* Trade-off: accuracy vs performance

---

## Supported Policies in Redis

* `allkeys-lru`
* `volatile-lru`
* `allkeys-lfu`
* `volatile-lfu`
* `noeviction`

---

## Common Mistake

> ⚠️ Assuming Redis LRU = perfect LRU
> → It is **approximate but efficient**

---

## Architect Insight

* For most systems:

  * Use **LRU + TTL**
* For heavy repeated access:

  * Use **LFU**

> Redis prioritizes **performance over perfect accuracy**

---

## Final Summary

```text
Cache Stampede → Many requests hit DB together
Solution → Locking, TTL jitter, refresh

Redis:
- In-memory
- Single-threaded
- Very fast

Eviction:
- LRU/LFU (approximate)
- Sampling-based
```

---
