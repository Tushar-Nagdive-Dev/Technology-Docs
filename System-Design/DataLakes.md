# Data Lakes (System Design)

## Definition

A **Data Lake** is a centralized system that stores **large volumes of raw data** (structured, semi-structured, and unstructured) in its **original format**, without applying a schema upfront.

---

## Core Idea

> **Store first → Process later (Schema-on-read)**

* Store all data as-is
* Decide how to use it later
* Supports analytics, ML, and big data use cases

---

## Example

```text
Data Sources:
- Logs (JSON)
- User clicks
- Images/videos
- IoT sensors

→ Stored directly in Data Lake

Later:
→ Processed using Spark / SQL tools
```

---

## Real-World Use Case

### E-commerce Platform

* Stores:

  * User activity
  * Orders
  * Product images
* Used for:

  * Recommendations
  * Fraud detection
  * Customer insights

---

## Tools & Technologies (with simple meaning)

### 1. Storage Layer (Where data is stored)

* Amazon S3
  → Stores files like a **huge cloud folder (infinite storage)**

* Azure Data Lake Storage
  → Same as S3 but in Azure

* Google Cloud Storage
  → Same concept in Google Cloud

---

### 2. Data Ingestion (How data comes in)

* Apache Kafka
  → Streams real-time data (like live user clicks)

* AWS Kinesis
  → AWS version of Kafka

* Batch Jobs / APIs
  → Upload data in chunks (daily logs, files)

---

### 3. Processing Layer (Make data useful)

* Apache Spark
  → Processes large data very fast

* Apache Hive
  → Run SQL queries on big data

* Databricks
  → Managed platform for Spark (easy + powerful)

---

### 4. Query & Visualization (Use the data)

* Amazon Athena
  → Run SQL directly on S3 without servers

* Google BigQuery
  → Fast querying + analytics

* Tableau / Power BI
  → Create dashboards and reports

---

## Visual Flow

```text
Data Sources
(Apps, Logs, IoT)
        ↓
Ingestion
(Kafka / APIs)
        ↓
Storage (Data Lake)
(S3 / GCS / ADLS)
        ↓
Processing
(Spark / Databricks)
        ↓
Query
(Athena / BigQuery)
        ↓
Dashboard / ML
(Tableau / Power BI)
```

---

## Key Points

* Stores **raw data (no schema)**
* Supports **all formats**
* Highly **scalable & low cost**
* Used for **analytics & machine learning**
* Separates **storage, compute, query**

---

## When NOT to Use

* Need **fast structured queries** → Use Data Warehouse
* Need **strict schema & clean data upfront**

---

## Common Mistake

> ⚠️ No structure = **Data Swamp**

Fix:

* Add metadata
* Organize data
* Apply access control

---

## Quick Comparison

| Feature   | Data Lake     | Data Warehouse |
| --------- | ------------- | -------------- |
| Data Type | Raw           | Structured     |
| Schema    | On read       | On write       |
| Use Case  | ML, analytics | BI reporting   |

---

## Architect Insight

* **Amazon S3 is the backbone** of modern data lakes
* Principle:

  > **Separate storage, processing, and querying**
