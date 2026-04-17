# HDFS Architecture (Hadoop Distributed File System)

## Definition

**HDFS (Hadoop Distributed File System)** is a distributed storage system designed to store **large files across multiple machines** with **high fault tolerance**.

---

## Core Idea

> Split large files into **blocks** and store them across multiple servers with **replication**

* Handles big data
* Ensures data is not lost even if nodes fail

---

## Example

```text
File (1 GB)
→ Split into blocks (128 MB each)
→ Stored across multiple machines
→ Each block replicated (e.g., 3 copies)
```

---

## Real-World Use Case

### Big Data Processing (e.g., log analysis)

* Store massive logs
* Process using Hadoop/Spark
* Fault-tolerant storage

---

## Key Components

---

## 1. NameNode (Master)

## Definition

* Manages **metadata** (not actual data)

---

### Responsibilities

* Knows where data blocks are stored
* Maintains file system structure
* Handles client requests

---

### Example

```text
File: logs.txt
→ Block1 → DataNode1
→ Block2 → DataNode2
```

---

---

## 2. DataNode (Worker)

## Definition

* Stores **actual data blocks**

---

### Responsibilities

* Read/write data
* Send heartbeat to NameNode
* Replicate data

---

---

## 3. Secondary NameNode

## Definition

* Assists NameNode (not a backup)

---

### Responsibility

* Periodically merges metadata (checkpointing)

---

---

## How HDFS Works

---

## Write Flow

```text id="2e2h8g"
Client
  ↓
NameNode (metadata)
  ↓
DataNodes (store blocks)
  ↓
Replication (copies created)
```

---

### Steps

1. Client asks NameNode where to store data
2. NameNode returns DataNode locations
3. Client writes data directly to DataNodes
4. Data is replicated

---

---

## Read Flow

```text id="n3r9i7"
Client
  ↓
NameNode (get block locations)
  ↓
DataNodes
  ↓
Read data
```

---

### Steps

1. Client asks NameNode for block locations
2. Reads from nearest DataNode

---

---

## Visual Flow

```text id="u7f3g1"
           NameNode (Master)
                 ↓
---------------------------------
| DataNode1 | DataNode2 | DataNode3 |
---------------------------------
        (Blocks stored here)
```

---

## Key Concepts

---

### 1. Block Storage

* Files split into large blocks (default ~128MB)

---

### 2. Replication

* Default: 3 copies
* Ensures fault tolerance

---

### 3. Rack Awareness

* Data stored across different racks
* Protects against rack failure

---

## Key Points

* Designed for **large-scale data**
* Optimized for **high throughput, not low latency**
* Fault-tolerant via replication
* Master-slave architecture

---

## Limitations

* NameNode is a **single point of failure** (improved in newer versions)
* Not suitable for small files
* High latency for random reads

---

## Common Mistakes

> ⚠️ Using HDFS for real-time systems
> ⚠️ Storing too many small files

---

## Architect Insight

* Use HDFS when:

  * Handling **huge data (TBs/PBs)**
  * Batch processing (Hadoop, Spark)

* Avoid when:

  * Low latency required
  * Small file storage

---

## Quick Summary

```text
HDFS:
- Master (NameNode) → metadata
- Workers (DataNodes) → actual data
- Files split into blocks
- Replicated for fault tolerance
```
