# 🧠 MongoDB Mastery Roadmap (Zero to Expert)

I'll break it into **Phases** — like a professional course — each phase **building on the previous one**.

---

# 📚 Phase 0: Quick Overview (Before We Start)

| Item | MongoDB | MySQL |
|:-----|:--------|:------|
| Type | NoSQL (Document-Based) | SQL (Relational Tables) |
| Data Structure | **Documents** inside **Collections** | **Rows** inside **Tables** |
| Schema | Flexible (Dynamic Schema) | Strict (Pre-defined Schema) |
| Query Language | **MQL (MongoDB Query Language)** | SQL |
| Storage Format | BSON (Binary JSON) | Tables |
| Best Use Cases | Big Data, Real-time Apps, IoT, CMS, Analytics | Structured Data, Banking, ERP, Traditional Apps |

---

# 🧱 Phase 1: Foundation - MongoDB Concepts, Setup, and Basics

## 1.1 What is MongoDB?
- **MongoDB** is a database that stores **data as documents**, not tables.
- Each document is **like a JSON object** `{}` but stored internally as **BSON** (Binary JSON).

✅ Example Document:
```json
{
  "_id": "1",
  "name": "John",
  "age": 25
}
```
- Documents are grouped into **Collections** (like tables).

---

## 1.2 Install MongoDB

### Option 1: Install Locally
- Download from [MongoDB Official Site](https://www.mongodb.com/try/download/community)
- Install MongoDB Server + MongoDB Compass (GUI)

### Option 2: Install via Docker
```bash
docker run --name mongo-mastery -d -p 27017:27017 mongo
```

---

## 1.3 MongoDB Database, Collection, and Document

| Level | Meaning | Example |
|:------|:--------|:--------|
| Database | Group of collections | `school` |
| Collection | Group of documents | `students` |
| Document | Single record (JSON) | `{ name: "John" }` |

✅ Create a Database
```javascript
use school;
```

✅ Insert into Collection
```javascript
db.students.insertOne({ name: "John", age: 25 });
```

✅ Find from Collection
```javascript
db.students.find();
```

✅ MongoDB vs MySQL at this stage:
| Operation | MySQL | MongoDB |
|:----------|:------|:--------|
| Create DB | `CREATE DATABASE school;` | `use school;` |
| Insert Row | `INSERT INTO students (name, age) VALUES ('John', 25);` | `db.students.insertOne({ name: "John", age: 25 });` |
| Select Row | `SELECT * FROM students;` | `db.students.find();` |

---

# 🎯 Phase 1.5: Basic CRUD Operations (with full Query-Level Understanding)

## Create

✅ Insert One
```javascript
db.students.insertOne({ name: "John", age: 25 });
```

✅ Insert Many
```javascript
db.students.insertMany([
  { name: "Alice", age: 30 },
  { name: "Bob", age: 22 }
]);
```

## Read

✅ Find All
```javascript
db.students.find();
```

✅ Find with Filter
```javascript
db.students.find({ age: { $gt: 20 } });
```

## Update

✅ Update One
```javascript
db.students.updateOne({ name: "John" }, { $set: { age: 26 } });
```

✅ Update Many
```javascript
db.students.updateMany({}, { $set: { isActive: true } });
```

## Delete

✅ Delete One
```javascript
db.students.deleteOne({ name: "Bob" });
```

✅ Delete Many
```javascript
db.students.deleteMany({ age: { $lt: 25 } });
```

---

# 🧠 Deep Note: MongoDB Query Language (MQL)

👉 MongoDB commands **are not JavaScript** —  
They are **pure queries** with the following format:

**Find Query Format**
```javascript
db.<collection>.find(
  <filter conditions>,
  <projection fields>
)
```
- **filter** → WHERE Clause
- **projection** → SELECT Columns

✅ Example
```javascript
db.students.find({ age: { $gt: 18 } }, { name: 1, age: 1, _id: 0 });
```
Means: find students older than 18, show name and age, hide `_id`.

---

# 📚 Phase 2: Intermediate MongoDB - Schema Design, Indexing, Aggregation

## 2.1 Schema Design: Embed vs Reference

✅ Embed (when tightly related)
```json
{
  "name": "John",
  "address": {
    "street": "123 Main St",
    "city": "NY"
  }
}
```

✅ Reference (when loosely related)
```json
{
  "name": "Alice",
  "addressId": "addr123"
}
```
(Separate collection for addresses.)

---

## 2.2 Indexing

✅ Create Index
```javascript
db.students.createIndex({ name: 1 });
```
- `1` → Ascending Order
- `-1` → Descending Order

✅ Why Index?
- Without index → MongoDB scans all documents (slow)
- With index → MongoDB jumps directly (fast)

---

## 2.3 Aggregation Framework (like SQL GROUP BY)

✅ Example: Average Age
```javascript
db.students.aggregate([
  { $group: { _id: null, avgAge: { $avg: "$age" } } }
]);
```

✅ Important Stages:
| Stage | Purpose |
|:------|:--------|
| `$match` | Filter documents (like WHERE) |
| `$group` | Group documents |
| `$sort` | Sort documents |
| `$project` | Reshape documents |

---

# 📚 Phase 3: Advanced MongoDB Topics

## 3.1 Lookup (MongoDB Joins)

✅ Lookup Example
```javascript
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customerId",
      foreignField: "_id",
      as: "customerDetails"
    }
  }
]);
```

---

## 3.2 Transactions (Like SQL Transactions)

✅ Example
```javascript
const session = db.getMongo().startSession();
session.startTransaction();
try {
  db.accounts.updateOne({ name: "Alice" }, { $inc: { balance: -100 } }, { session });
  db.accounts.updateOne({ name: "Bob" }, { $inc: { balance: 100 } }, { session });
  session.commitTransaction();
} catch (e) {
  session.abortTransaction();
}
session.endSession();
```

---

## 3.3 Replication and Sharding

✅ Replication
- Keep multiple copies (for failover and backups).

✅ Sharding
- Split data across servers (for massive scaling).

✅ Example Sharding Key:
```javascript
sh.shardCollection("school.students", { studentId: "hashed" });
```

---

# 📚 Phase 4: Mastery Projects and Best Practices

- Build a mini CMS with MongoDB
- Build an E-commerce app backend
- Design an analytics dashboard
- Learn MongoDB Performance Optimization
- Understand Security: Authentication, Authorization, SSL

---

# 📋 Full Roadmap Summary

| Phase | Topics |
|:------|:------|
| Phase 0 | Quick overview: MongoDB vs MySQL |
| Phase 1 | Setup, Basic CRUD, Understanding MQL |
| Phase 1.5 | CRUD Deep Dive, MQL queries |
| Phase 2 | Schema Design, Indexes, Aggregations |
| Phase 3 | Advanced Topics: Lookup, Transactions, Replication |
| Phase 4 | Projects, Real-world apps, Optimization, Security |

---

# 📖 How We'll Progress

✅ One phase at a time  
✅ Step-by-step real examples  
✅ Practical Exercises  
✅ Quizzes and Revision at each stage  
✅ Building Real Applications  
✅ Then Final Mastery Certification Style Tests 🚀
