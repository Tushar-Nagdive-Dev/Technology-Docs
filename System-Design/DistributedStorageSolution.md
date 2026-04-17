# Distributed Storage Solutions (System Design)

## Definition

**Distributed Storage** is a system where data is stored across **multiple machines (nodes)** instead of a single server.

> Data is **distributed, replicated, and managed across a cluster**

---

## Core Idea

> Store data across many servers to achieve **scalability, reliability, and fault tolerance**

---

## Example

```text
Single Server:
All data → One machine → Risk of failure ❌

Distributed Storage:
Data → Split across Server1, Server2, Server3
→ System continues even if one fails ✅
```

---

## Real-World Use Case

### Cloud Storage (Google Drive / AWS S3)

* Files are stored across multiple servers
* If one server fails → data still available

---

## Key Techniques

---

## 1. Data Partitioning (Sharding)

### Definition

Split data into smaller parts and store on different nodes

---

### Example

```text
User Data:
A–F → Server 1
G–M → Server 2
N–Z → Server 3
```

---

### Benefit

* Improves scalability
* Distributes load

---

---

## 2. Replication

### Definition

Store **copies of data on multiple nodes**

---

### Example

```text
Data → Server 1 (Primary)
       ↓
   Server 2 (Replica)
       ↓
   Server 3 (Replica)
```

---

### Benefit

* Fault tolerance
* High availability

---

---

## 3. Consistency Models

### Strong Consistency

* All users see same data immediately

### Eventual Consistency

* Data becomes consistent over time

---

## Visual Flow

```text
Client
  ↓
Distributed Storage Cluster
  ↓
--------------------------
| Node1 | Node2 | Node3  |
--------------------------
```

---

## Types of Distributed Storage

---

## 1. Object Storage

### Definition

Stores data as **objects (files + metadata)**

---

### Tools

* Amazon S3
* Google Cloud Storage

---

### Use Case

* Images
* Videos
* Backups

---

---

## 2. Distributed File Systems

### Definition

Stores files across multiple machines like a single file system

---

### Tools

* HDFS
* GlusterFS

---

### Use Case

* Big data processing

---

---

## 3. Distributed Databases

### Definition

Database spread across multiple nodes

---

### Tools

* Cassandra
* MongoDB

---

### Use Case

* High-scale applications

---

---

## Visual Comparison

```text
Object Storage → Files (S3)
File System    → File hierarchy (HDFS)
Database       → Structured data (Cassandra)
```

---

## Key Points

* Enables **horizontal scaling**
* Provides **fault tolerance**
* Supports **high availability**
* Uses **partitioning + replication**

---

## Challenges

### 1. Data Consistency

* Hard to maintain across nodes

---

### 2. Network Latency

* Data spread across machines

---

### 3. Complex Management

* Requires coordination between nodes

---

## Common Mistakes

> ⚠️ Ignoring replication → data loss risk
> ⚠️ Overusing strong consistency → performance issues

---

## Architect Insight

* Choose based on use case:

| Use Case            | Storage Type   |
| ------------------- | -------------- |
| Media files         | Object Storage |
| Big data processing | Distributed FS |
| Real-time apps      | Distributed DB |

---

## Quick Summary

```text
Distributed Storage = Data across multiple servers

Techniques:
- Sharding
- Replication

Types:
- Object (S3)
- File system (HDFS)
- Database (Cassandra)
```

---
Great! Let's dive into **PageRank** - the algorithm that helped Google dominate web search.

## The Core Problem

In the early web (1990s), search engines had a problem:
- They could find pages with matching keywords easily
- But **how do you rank them?** Which page is most important/trustworthy?

**Bad approach**: Just count keyword frequency
- Result: Spam! People stuffed keywords everywhere to rank higher

## PageRank's Big Idea

**Treat the web like an academic citation network.**

In academia:
- Important papers get cited by many other papers
- A citation from a famous researcher counts more than from an unknown one

On the web:
- **Links are like citations** - they're "votes" for a page
- A link from a popular site (like Wikipedia) counts more than from a random blog

## How PageRank Works

### The Basic Formula

Each page gets a "PageRank score" based on:

1. **How many pages link to it** (quantity of votes)
2. **The importance of those linking pages** (quality of votes)

**Simple version:**
```
PageRank(Page A) = Sum of (PageRank of each linking page / number of links on that page)
```

### Visual Example

```
Page A ← Page B (has 10 outgoing links)
Page A ← Page C (has 2 outgoing links)
Page A ← Page D (has 100 outgoing links)
```

- Link from Page C counts **most** (it's selective, only 2 links)
- Link from Page D counts **least** (it links to everything)
- Page A's score = (B's score/10) + (C's score/2) + (D's score/100)

### Concrete Example

Imagine 4 websites:

```
[Wikipedia] ──links to──> [Your Blog]
     ↑                          ↓
     |                     links to
     |                          ↓
[Random Site] <──links to── [Friend's Blog]
```

**Initial scores** (everyone starts equal): 1.0

**After iteration 1:**
- Your Blog gets a boost (Wikipedia linked to it!)
- Wikipedia's score increases (others link to it)
- Random Site stays low (no one links to it)

**After iteration 2:**
- Scores stabilize based on the link structure

**Result:**
- Wikipedia: High score (many quality incoming links)
- Your Blog: Medium-high (Wikipedia's vote is powerful)
- Friend's Blog: Medium (your blog linked to it)
- Random Site: Low (isolated)

## The Random Surfer Model

PageRank imagines a person randomly clicking links:

1. Start on a random page
2. Keep clicking random links
3. Occasionally (15% of the time) jump to a completely random page

**The PageRank score = probability that the random surfer lands on that page**

Pages that are well-connected and linked from important pages will be visited more often.

## The Mathematical Process

PageRank is calculated **iteratively**:

```
Initial state:
All pages start with score: 1/N (where N = total pages)

Iteration 1:
Each page distributes its score equally among pages it links to

Iteration 2:
Recalculate based on new incoming scores

Repeat...
Until scores converge (stop changing much)
```

## Real Formula

```
PR(A) = (1-d) + d × Σ(PR(Ti) / C(Ti))
```

Where:
- **PR(A)** = PageRank of page A
- **d** = damping factor (usually 0.85) - the 15% random jump probability
- **Ti** = pages that link to A
- **C(Ti)** = number of outgoing links from page Ti
- **Σ** = sum over all pages linking to A

## Why It Revolutionized Search

**Before PageRank:**
- Search results easily manipulated by keyword stuffing
- Hard to distinguish authoritative sources

**After PageRank:**
- Links became "votes of confidence"
- Harder to game (you'd need many quality sites to link to you)
- Academic papers, government sites, major news naturally ranked higher

## Example Scenario

Search: "climate change"

**Without PageRank:**
1. Random blog with "climate change" 100 times
2. Someone's personal website
3. NASA's climate page (only mentions it 5 times)

**With PageRank:**
1. NASA's climate page (high authority, many quality links)
2. Scientific journals (cited by universities)
3. Wikipedia (linked by everyone)
4. Random blog (low authority, few links)

## Modern Reality

Google doesn't use pure PageRank anymore. Modern ranking uses:
- PageRank (link authority)
- Content quality signals
- User behavior (click-through rates)
- Freshness
- Mobile-friendliness
- Page speed
- 200+ other factors

But PageRank was the **foundational insight** that made Google different.

## Simple Analogy

Think of PageRank like **reputation in a small town**:

- **High PageRank** = The mayor (everyone knows them, people listen)
- **Medium PageRank** = Local business owner (known in their circle)
- **Low PageRank** = New person in town (few connections)

When the mayor recommends someone, that's a powerful endorsement. When a newcomer recommends someone, it matters less.

**The web is the same** - recommendations (links) from important pages carry more weight!
