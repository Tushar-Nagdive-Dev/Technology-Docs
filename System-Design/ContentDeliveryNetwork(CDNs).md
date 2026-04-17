# Content Delivery Networks (CDN)

## Definition

A **CDN (Content Delivery Network)** is a system of **distributed servers located across different regions** that deliver content to users from the **nearest server (edge location)**.

---

## Core Idea

> Serve content from **closest location to user → faster response, lower latency**

Instead of hitting your main server every time, users get data from a nearby CDN server.

---

## Example

```text
Without CDN:
User (India) → Server (USA) → Slow response

With CDN:
User (India) → Nearest CDN (Mumbai) → Fast response
```

---

## Real-World Use Case

### E-commerce / Web Apps

* Images (product photos)
* Videos
* CSS / JS files

**With CDN:**

* Static content served from nearby edge servers
* Reduces load on main backend
* Improves page load speed globally

---

## How CDN Works

## Step-by-Step Flow

```text
1. User requests content (image/video)
2. CDN checks nearest edge server
3. If cached → return immediately (fast)
4. If not cached → fetch from origin server
5. Store in CDN → return to user
```

---

## Visual Flow

```text
User (India)
      ↓
Nearest CDN Edge (Mumbai)
      ↓          ↓
   Cache Hit    Cache Miss
      ↓           ↓
   Response    Origin Server (USA)
                    ↓
               Store in CDN
                    ↓
                 Response
```

---

## Types of Content

### 1. Static Content (Best for CDN)

* Images
* Videos
* CSS / JS
* HTML

---

### 2. Dynamic Content (Advanced CDN)

* APIs
* Personalized data (limited caching)

---

## CDN Technologies (with simple meaning)

* Cloudflare
  → Global CDN + security (very popular)

* Akamai
  → Enterprise CDN

* Amazon CloudFront
  → CDN integrated with AWS

* Fastly
  → Real-time CDN with low latency

---

## Key Concepts

### Edge Servers

* Servers placed close to users

---

### Origin Server

* Your main backend server

---

### Cache Hit / Miss

* Hit → served from CDN
* Miss → fetched from origin

---

### TTL (Time To Live)

* Defines how long CDN stores content

---

## Key Points

* Reduces **latency (faster response)**
* Reduces **backend load**
* Improves **global performance**
* Highly effective for **static content**

---

## When NOT to Use

* Highly dynamic or sensitive data
* Real-time transactional systems

---

## Common Problems

### 1. Cache Invalidation

> Hard to update content quickly across all edge servers

---

### 2. Stale Content

* Old data served due to long TTL

---

### 3. Cold Start

* First request → slow (no cache)

---

## Architect Insight

* Combine **CDN + Backend Cache (Redis)**
* Use CDN for:

  * Static assets
  * Public APIs

> Rule:
> **CDN = Edge caching (global)**
> **Redis = Application caching (internal)**

---

## Quick Summary

```text
CDN = Global cache near users

Benefits:
- Faster response
- Reduced latency
- Lower backend load

Flow:
User → CDN → (Hit/Miss) → Origin
```

---
# CDN vs Cache vs Reverse Proxy (Important Comparison)

## Definition

* **CDN** → Global network that delivers content from **nearest location (edge servers)**
* **Cache** → Temporary storage to **speed up data access** (usually inside system)
* **Reverse Proxy** → Server that sits in front of backend and **handles requests on behalf of servers**

---

## Core Idea

> All improve performance, but they operate at **different layers**

* CDN → **Global layer (internet level)**
* Cache → **Application/data layer**
* Reverse Proxy → **Gateway layer (entry point)**

---

## Example

```text id="9m8q0v"
User requests image

→ CDN (nearest server)
→ Reverse Proxy (routes request)
→ Cache (Redis)
→ Backend Server
→ Database
```

---

## Detailed Comparison

| Feature       | CDN                           | Cache                | Reverse Proxy         |
| ------------- | ----------------------------- | -------------------- | --------------------- |
| Purpose       | Deliver content globally      | Speed up data access | Route/manage requests |
| Location      | Edge (near user)              | App layer / memory   | Front of backend      |
| Data Type     | Static (mostly)               | Any (DB/API results) | Requests/responses    |
| Scope         | Global                        | Internal             | Internal              |
| Example Tools | Cloudflare, Amazon CloudFront | Redis, Memcached     | Nginx, HAProxy        |

---

## Visual Flow

```text id="qk6x6l"
User
 ↓
CDN (Edge Cache)
 ↓
Reverse Proxy (Nginx)
 ↓
Application Cache (Redis)
 ↓
Backend Service
 ↓
Database
```

---

## When to Use What

### Use CDN when:

* Global users
* Static content (images, videos, JS)

---

### Use Cache when:

* Repeated DB/API calls
* Read-heavy systems

---

### Use Reverse Proxy when:

* Load balancing
* Security (SSL, rate limiting)
* Routing requests

---

## Key Points

* CDN reduces **network latency**
* Cache reduces **database load**
* Reverse proxy improves **request handling & security**

---

## Architect Insight

> These are not alternatives — they are **used together**

---

---

# How Netflix / YouTube Use CDN (Real Architecture)

## Core Idea

> Deliver video content from **servers closest to users** to ensure smooth streaming

---

## Netflix Architecture

### Technology

* Open Connect

---

## How It Works

### Step-by-Step Flow

```text id="q3z91s"
User clicks Play
      ↓
DNS routes to nearest Open Connect server
      ↓
Video served from local ISP CDN
      ↓
If not available → fetch from Netflix origin
```

---

## Visual Flow

```text id="m6qg5c"
User
 ↓
ISP Network
 ↓
Netflix Open Connect (Local CDN)
 ↓         ↓
Hit        Miss
 ↓          ↓
Video      Netflix Origin Server
```

---

## Key Strategy

* Netflix places CDN servers **inside ISPs**
* Reduces internet traffic + latency
* Ensures high-quality streaming

---

## YouTube Architecture

### Technology

* Google Global Cache

---

## How It Works

### Step-by-Step Flow

```text id="5qu6xj"
User requests video
      ↓
Google routes to nearest cache server
      ↓
Video streamed from edge location
      ↓
If not cached → fetched from central data center
```

---

## Visual Flow

```text id="x4w2nl"
User
 ↓
Google Edge Server
 ↓         ↓
Hit        Miss
 ↓          ↓
Stream     Central Storage
```

---

## Key Strategy

* Distributed global edge servers
* Adaptive bitrate streaming
* Heavy caching of popular videos

---

## Real-World Techniques Used

### 1. Edge Caching

* Store popular videos near users

---

### 2. Adaptive Streaming

* Adjust video quality based on internet speed

---

### 3. Pre-Caching Popular Content

* Trending videos cached in advance

---

### 4. Load Balancing

* Distribute traffic across servers

---

## Key Points

* CDN is **core to video streaming platforms**
* Reduces buffering
* Improves user experience globally

---

## Architect Insight

* For large-scale systems:

  * Combine:

    * CDN (global delivery)
    * Cache (internal optimization)
    * Reverse proxy (routing/security)

> Real systems are **layered, not single-solution**

---

## Quick Summary

```text id="z7m0kw"
CDN → Global delivery (edge)
Cache → Fast data access (internal)
Reverse Proxy → Traffic control

Netflix/YouTube:
→ Use CDN near users
→ Cache popular content
→ Ensure smooth streaming
```
