# 📚 MongoDB Mastery Program (Zero to Expert)

## 🏁 Phase 1: Introduction to MongoDB (Foundations)

### 🎯 Goals
- Understand what MongoDB is
- Know why/where MongoDB is used
- Set up MongoDB
- Learn basic commands (Create, Read, Update, Delete = CRUD)

---

## 1. What is MongoDB?

| Aspect | Description |
|:------|:------------|
| Type | NoSQL Database |
| Data Storage | Stores data as **documents** in **collections** (like JSON format) |
| Famous For | Flexibility, scalability, handling large volumes of unstructured data |
| Popular Uses | Big Data, Real-time Analytics, IoT, Content Management, Social Apps |

🧠 **Insight**: MongoDB is **document-based** unlike SQL which is **table-based**.

✅ **Example**:  
SQL Table:
```text
+----+--------+-----+
| ID | Name   | Age |
+----+--------+-----+
| 1  | John   | 25  |
| 2  | Alice  | 30  |
+----+--------+-----+
```
MongoDB Document:
```json
{ "_id": 1, "name": "John", "age": 25 }
{ "_id": 2, "name": "Alice", "age": 30 }
```

---

## 2. Why MongoDB?

- **Flexible Schema** — No need to define columns before inserting data
- **High Performance** — Very fast read/write
- **Easy Scalability** — Good for cloud-native, distributed apps
- **Rich Queries** — Powerful query language similar to SQL

🔔 **Common Mistake to Avoid**:  
Thinking "MongoDB means no schema at all" — Not true. **Schemas are flexible**, but you should design them wisely (we’ll cover this deeply later).

---

## 3. Setting Up MongoDB

### 🛠️ Install MongoDB on Local
- Go to [MongoDB Download Center](https://www.mongodb.com/try/download/community)
- Choose your OS (Windows, Mac, Linux)
- Install MongoDB Community Edition
- Also install **MongoDB Compass** (GUI tool)

💡 **MongoDB Compass** helps you *visually manage your data* without writing commands!

**OR**

🛠️ Install via **Docker** (if you know Docker):
```bash
docker run --name mongo-mastery -d -p 27017:27017 mongo
```

---

## 4. MongoDB Basics: CRUD Operations

👉 All work happens inside **Collections** inside a **Database**.

### ➡️ Create (Insert Documents)
```javascript
db.users.insertOne({ name: "John", age: 25 });
db.users.insertMany([
  { name: "Alice", age: 30 },
  { name: "Bob", age: 22 }
]);
```

### ➡️ Read (Find Documents)
```javascript
db.users.find(); // Find all
db.users.find({ name: "Alice" }); // Find where name = Alice
```

### ➡️ Update
```javascript
db.users.updateOne({ name: "John" }, { $set: { age: 26 } });
db.users.updateMany({}, { $set: { isActive: true } });
```

### ➡️ Delete
```javascript
db.users.deleteOne({ name: "Bob" });
db.users.deleteMany({ age: { $lt: 25 } });
```

✅ These basic operations will **build your hands-on confidence**.

---

# ✨ Exercises for Phase 1

**Exercise 1**:  
- Install MongoDB
- Open Mongo Shell or Compass
- Create a database named `school`
- Create a collection `students`
- Insert at least 5 students with fields `name`, `age`, `grade`
- Perform find, update, and delete operations.

---

# 📘 Phase 1 Mini-Summary

| Key Point | You Should Know |
|:----------|:---------------|
| MongoDB stores **documents** in **collections** | ✅ |
| Documents are similar to **JSON** | ✅ |
| You can perform CRUD using `insertOne`, `find`, `updateOne`, `deleteOne` | ✅ |
| Setup MongoDB locally or via Docker | ✅ |

---

# 🛤️ What's Next (Phase 2 Preview)

If you're comfortable with this (and tell me to proceed), we'll move to:

✅ MongoDB Schema Design Principles  
✅ Advanced Queries and Operators  
✅ Aggregations  
✅ Indexes  
✅ Replication, Sharding (Scalability)  
✅ Real-world Use Cases (Blog App, Analytics Dashboard)  
✅ Optimization and Best Practices  
✅ Interview Questions  
✅ Advanced Architectures (MongoDB with Node.js, Spring Boot, Microservices)  
✅ Certification Practice (if you want!)

---
