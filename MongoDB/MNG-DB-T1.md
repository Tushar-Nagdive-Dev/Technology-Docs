# 📚 Phase 1: MongoDB Foundations — Setup and Basic Understanding

---

# 🛤️ Step 1: What is MongoDB (True Beginner View)

✅ **MongoDB** is a **NoSQL**, **document-oriented** database.  
✅ It stores data as **documents** (`{}`), not rows or tables.

**Imagine**:
- **MySQL** stores data in rows inside tables.
- **MongoDB** stores data in flexible documents inside collections.

---

📖 **Document** = a real-world object (like a person, a product, an order), saved like this:

```json
{
  "_id": "1",
  "name": "John Doe",
  "age": 28,
  "email": "john@example.com"
}
```

> 📌 Every document has a unique `_id` field (auto-generated if you don't provide it).

📖 **Collection** = group of related documents (like a table).

📖 **Database** = group of related collections.

---

✅ **Real Example (Structure):**

| MongoDB Hierarchy | Example Name | Example Description |
|:------------------|:-------------|:--------------------|
| Database | `school` | Database for managing a school system |
| Collection | `students` | All student documents |
| Document | `{name: "Alice", age: 20}` | A single student's data |

---

# 🛤️ Step 2: Why MongoDB?

| Feature | Description |
|:--------|:------------|
| Flexible Schema | You can add, remove, or change fields easily |
| High Performance | Very fast for read/write heavy apps |
| Scales Horizontally | Easy to scale across multiple servers |
| Rich Query Language (MQL) | Can filter, sort, project, aggregate data |
| Cloud-Native | Works great with cloud apps (AWS, GCP, Azure) |

---

# 🛤️ Step 3: Setting up MongoDB

You have two options:

---

## Option 1: Local Installation

✅ Download MongoDB Community Edition (FREE):
👉 [MongoDB Download Center](https://www.mongodb.com/try/download/community)

✅ Install **MongoDB Server** and **MongoDB Compass** (GUI)

After installation:

✅ To **start** MongoDB server (service will auto-start).

✅ Open **MongoDB Compass** to *visually interact* with your databases.

✅ Open **Terminal / Command Prompt** and start the Mongo Shell:
```bash
mongo
```

---

## Option 2: Docker Installation (Optional for later)

If you know Docker, you can quickly run MongoDB:
```bash
docker run --name mongo-mastery -d -p 27017:27017 mongo
```
✅ Then connect via Compass on `localhost:27017`.

---

# 🛤️ Step 4: MongoDB Working Environment

When working with MongoDB you typically use:

| Tool | Purpose |
|:-----|:--------|
| MongoDB Shell | Write queries manually |
| MongoDB Compass | GUI to manage collections, documents easily |
| Programmatic Access (later) | From apps (Node.js, Java, Python) using Drivers |

---

# 🛤️ Step 5: First Commands in MongoDB (True MQL Level)

👉 MongoDB Query Structure is:
```javascript
db.<collection>.action(<query>)
```
where:
- `db` = current database
- `<collection>` = table-like structure
- `action` = operation like `find`, `insertOne`, etc.

---

✅ **1. Create/Select Database**

```javascript
use school;
```
- If the database doesn’t exist, MongoDB will create it when you insert data.

✅ **2. Insert a Document**

```javascript
db.students.insertOne({ name: "Alice", age: 22 });
```

✅ **3. Insert Many Documents**

```javascript
db.students.insertMany([
  { name: "John", age: 25 },
  { name: "Sophia", age: 20 }
]);
```

✅ **4. Read Documents (Select)**

```javascript
db.students.find();  // returns all students
```

✅ **5. Read with Filter (Where)**

```javascript
db.students.find({ age: { $gt: 21 } }); // age greater than 21
```

✅ **6. Update a Document**

```javascript
db.students.updateOne(
  { name: "John" },
  { $set: { age: 26 } }
);
```

✅ **7. Delete a Document**

```javascript
db.students.deleteOne({ name: "Sophia" });
```

---

# 🧠 Understanding Each Command Deeply

| Command | What Happens Behind the Scenes |
|:--------|:-------------------------------|
| `use school;` | Set or create the database `school`. |
| `db.students.insertOne({...})` | Add a document into `students` collection. |
| `db.students.find({})` | Fetch all documents inside `students`. |
| `db.students.updateOne(filter, update)` | Find a document matching filter, apply changes. |
| `db.students.deleteOne(filter)` | Find a document matching filter, delete it. |

---

# 📋 Quick Comparison: MongoDB vs MySQL for Basic Actions

| Operation | MySQL (SQL) | MongoDB (MQL) |
|:----------|:------------|:-------------|
| Create Database | `CREATE DATABASE school;` | `use school;` |
| Insert Row | `INSERT INTO students (name, age) VALUES ('Alice', 22);` | `db.students.insertOne({ name: "Alice", age: 22 });` |
| Select All Rows | `SELECT * FROM students;` | `db.students.find();` |
| Where Clause | `SELECT * FROM students WHERE age > 21;` | `db.students.find({ age: { $gt: 21 } });` |
| Update Row | `UPDATE students SET age = 26 WHERE name = 'John';` | `db.students.updateOne({ name: "John" }, { $set: { age: 26 } });` |
| Delete Row | `DELETE FROM students WHERE name = 'Sophia';` | `db.students.deleteOne({ name: "Sophia" });` |

---

# ✏️ Your Hands-On Exercise for Phase 1

✅ Install MongoDB locally (or via Docker).  
✅ Open MongoDB Compass or Mongo Shell.  
✅ Create a database `company`.  
✅ Create a collection `employees`.  
✅ Insert at least 5 employees with fields:
- `name`
- `age`
- `position`
- `department`

✅ Find all employees.  
✅ Find employees older than 30.  
✅ Update an employee’s department.  
✅ Delete an employee younger than 25.

---

# 🎯 Phase 1 Mini-Checklist

| Skill | Status |
|:------|:------|
| Setup MongoDB locally | ⬜ |
| Create database, collection | ⬜ |
| Insert, Read, Update, Delete documents | ⬜ |
| Understand MongoDB structure (DB → Collection → Document) | ⬜ |
| Understand MQL query structure | ⬜ |

---

# 🚀 Coming Next: Phase 2

If you're ready after exercises, in **Phase 2** we will go to:
- **Schema Design** (Embed vs Reference)
- **Advanced Queries** (Projection, Sorting, Limit)
- **Indexing** (for speed)
- **Aggregation** (like SQL GROUP BY but more powerful!)

---

# ✅ Quick Summary for Phase 1

- MongoDB is **NoSQL**, **Document-based**.
- Data stored as **Documents inside Collections**.
- MongoDB uses **MQL**, not pure JavaScript.
- **CRUD** operations are easy and powerful.
- **Flexible schema** = add fields anytime, no alter tables.

---
