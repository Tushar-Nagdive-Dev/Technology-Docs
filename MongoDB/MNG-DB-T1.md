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

### Solution

Below is a step-by-step guide to accomplish the tasks you outlined, including installing MongoDB, creating a database and collection, performing CRUD operations, and using MongoDB Compass or Mongo Shell. I'll assume you're using a local installation, but I'll also provide Docker instructions for flexibility. The instructions are tailored for a beginner-friendly experience on a typical system (Windows, macOS, or Linux) as of April 26, 2025.

---

### Step 1: Install MongoDB Locally (or via Docker)

#### Option 1: Install MongoDB Locally
1. **Download MongoDB Community Server**:
   - Visit the [MongoDB Download Center](https://www.mongodb.com/try/download/community).
   - Select the latest stable version (e.g., 7.0.x) for your operating system (Windows, macOS, or Linux).
   - Download the installer or archive.

2. **Install MongoDB**:
   - **Windows**:
     - Run the `.msi` installer.
     - Follow the wizard, selecting "Complete" setup and installing MongoDB Compass (optional but recommended for GUI).
     - Ensure the MongoDB service is enabled to run automatically.
   - **macOS**:
     - Use Homebrew: `brew tap mongodb/brew && brew install mongodb-community`.
     - Or, extract the downloaded `.tgz` file and add the `bin/` directory to your PATH.
   - **Linux (Ubuntu example)**:
     - Follow the official guide: Import the MongoDB public GPG key, add the repository, and install with `sudo apt-get install -y mongodb-org`.
     - Start the service: `sudo systemctl start mongod`.

3. **Start MongoDB**:
   - **Windows**: MongoDB runs as a service by default, or start manually via `net start MongoDB`.
   - **macOS/Linux**: Run `mongod` in a terminal or start the service with `sudo systemctl start mongod` (Linux) or `brew services start mongodb-community` (macOS).
   - Verify MongoDB is running by checking `http://localhost:27017` in a browser (should display a message like "It looks like you are trying to access MongoDB over HTTP on the native driver port.").

4. **Install MongoDB Tools** (optional, for Mongo Shell):
   - Download the MongoDB Database Tools from the [MongoDB Download Center](https://www.mongodb.com/try/download/database-tools).
   - Install and add to your PATH for access to `mongo` shell.

#### Option 2: Install MongoDB via Docker
1. **Install Docker**:
   - Download and install [Docker Desktop](https://www.docker.com/products/docker-desktop/) for Windows/macOS or Docker for Linux.
   - Verify installation: `docker --version`.

2. **Pull MongoDB Docker Image**:
   - Run: `docker pull mongo:latest` to get the latest MongoDB image.

3. **Run MongoDB Container**:
   - Start a MongoDB container: `docker run -d -p 27017:27017 --name mongodb mongo:latest`.
   - `-d`: Run in detached mode.
   - `-p 27017:27017`: Map port 27017 (MongoDB default) to the host.
   - Verify the container is running: `docker ps`.

4. **Access MongoDB**:
   - Use `docker exec -it mongodb mongosh` to enter the MongoDB shell.
   - Or connect via MongoDB Compass using `localhost:27017`.

---

### Step 2: Open MongoDB Compass or Mongo Shell

- **MongoDB Compass**:
  - If installed during MongoDB setup, launch MongoDB Compass from your applications menu.
  - Connect to the default URI: `mongodb://localhost:27017`.
  - You’ll see a GUI interface to manage databases and collections.

- **Mongo Shell**:
  - Open a terminal and run `mongosh` (MongoDB Shell, included with recent versions).
  - If using Docker, run `docker exec -it mongodb mongosh`.
  - You should see a prompt like `test>`, indicating you’re connected to the MongoDB server.

For this guide, I’ll provide commands for Mongo Shell, but you can perform equivalent actions in Compass via its GUI.

---

### Step 3: Create a Database `company`

In MongoDB, databases are created implicitly when you use them.

- **Mongo Shell**:
  ```javascript
  use company
  ```
  - This switches to the `company` database. If it doesn’t exist, MongoDB creates it when you insert data.

- **MongoDB Compass**:
  - Click “Create Database” in the Compass interface.
  - Enter `company` as the database name and proceed (you can specify a collection name later).

---

### Step 4: Create a Collection `employees`

Collections are also created implicitly when data is inserted, but you can explicitly create one.

- **Mongo Shell**:
  ```javascript
  db.createCollection("employees")
  ```
  - This creates the `employees` collection in the `company` database.

- **MongoDB Compass**:
  - In the `company` database, click “Create Collection”.
  - Name it `employees` and confirm.

---

### Step 5: Insert at Least 5 Employees

Insert documents with fields: `name`, `age`, `position`, and `department`.

- **Mongo Shell**:
  ```javascript
  db.employees.insertMany([
    { name: "Alice Smith", age: 28, position: "Software Engineer", department: "Engineering" },
    { name: "Bob Johnson", age: 35, position: "Project Manager", department: "Management" },
    { name: "Carol Lee", age: 42, position: "Data Scientist", department: "Analytics" },
    { name: "David Brown", age: 23, position: "Intern", department: "Engineering" },
    { name: "Emma Davis", age: 31, position: "UX Designer", department: "Design" }
  ])
  ```
  - This inserts five employee documents. You’ll see an acknowledgment with inserted IDs.

- **MongoDB Compass**:
  - Navigate to the `employees` collection.
  - Click “Add Data” > “Insert Document”.
  - Enter each document in JSON format (e.g., `{ "name": "Alice Smith", "age": 28, "position": "Software Engineer", "department": "Engineering" }`).
  - Repeat for all five employees.

---

### Step 6: Find All Employees

Retrieve all documents in the `employees` collection.

- **Mongo Shell**:
  ```javascript
  db.employees.find().pretty()
  ```
  - `find()` retrieves all documents, and `pretty()` formats the output for readability.
  - Expected output:
    ```json
    {
      "_id": ObjectId("..."),
      "name": "Alice Smith",
      "age": 28,
      "position": "Software Engineer",
      "department": "Engineering"
    }
    {
      "_id": ObjectId("..."),
      "name": "Bob Johnson",
      "age": 35,
      "position": "Project Manager",
      "department": "Management"
    }
    ...
    ```

- **MongoDB Compass**:
  - In the `employees` collection, the documents are displayed by default.
  - Use the filter field to enter `{}` (empty query) and click “Find” to view all employees.

---

### Step 7: Find Employees Older Than 30

Query for employees with `age` greater than 30.

- **Mongo Shell**:
  ```javascript
  db.employees.find({ age: { $gt: 30 } }).pretty()
  ```
  - `$gt` is the greater-than operator.
  - Expected output:
    ```json
    {
      "_id": ObjectId("..."),
      "name": "Bob Johnson",
      "age": 35,
      "position": "Project Manager",
      "department": "Management"
    }
    {
      "_id": ObjectId("..."),
      "name": "Carol Lee",
      "age": 42,
      "position": "Data Scientist",
      "department": "Analytics"
    }
    {
      "_id": ObjectId("..."),
      "name": "Emma Davis",
      "age": 31,
      "position": "UX Designer",
      "department": "Design"
    }
    ```

- **MongoDB Compass**:
  - In the `employees` collection, enter the filter `{ "age": { "$gt": 30 } }`.
  - Click “Find” to display matching employees.

---

### Step 8: Update an Employee’s Department

Update the department of one employee (e.g., change Alice Smith’s department to “Research”).

- **Mongo Shell**:
  ```javascript
  db.employees.updateOne(
    { name: "Alice Smith" },
    { $set: { department: "Research" } }
  )
  ```
  - `updateOne` updates the first matching document.
  - `$set` modifies the specified field.
  - Verify the update:
    ```javascript
    db.employees.find({ name: "Alice Smith" }).pretty()
    ```
    - Expected output:
      ```json
      {
        "_id": ObjectId("..."),
        "name": "Alice Smith",
        "age": 28,
        "position": "Software Engineer",
        "department": "Research"
      }
      ```

- **MongoDB Compass**:
  - Navigate to the `employees` collection.
  - Find Alice Smith’s document and click the edit icon.
  - Change `"department": "Engineering"` to `"department": "Research"`.
  - Click “Update” to save.

---

### Step 9: Delete an Employee Younger Than 25

Delete one employee with `age` less than 25 (e.g., David Brown, age 23).

- **Mongo Shell**:
  ```javascript
  db.employees.deleteOne({ age: { $lt: 25 } })
  ```
  - `$lt` is the less-than operator.
  - Verify the deletion:
    ```javascript
    db.employees.find().pretty()
    ```
    - David Brown’s document should no longer appear.

- **MongoDB Compass**:
  - In the `employees` collection, enter the filter `{ "age": { "$lt": 25 } }`.
  - Find David Brown’s document, click the delete icon, and confirm deletion.

---

### Verification and Troubleshooting
- **Verify All Steps**:
  - After completing all tasks, run `db.employees.find().pretty()` in Mongo Shell or refresh the `employees` collection in Compass.
  - You should see four remaining employees (after deleting David Brown), with Alice Smith’s department updated to “Research”.

- **Common Issues**:
  - **MongoDB not running**: Ensure the service is active (`sudo systemctl status mongod` on Linux, or check Docker with `docker ps`).
  - **Connection issues**: Verify the connection string (`mongodb://localhost:27017`) and that port 27017 is open.
  - **Syntax errors**: Double-check MongoDB commands for correct syntax, especially operators like `$gt` and `$set`.

---

### Summary
You’ve successfully:
1. Installed MongoDB locally or via Docker.
2. Opened MongoDB Compass or Mongo Shell.
3. Created the `company` database and `employees` collection.
4. Inserted five employee documents.
5. Performed queries to find all employees and those older than 30.
6. Updated an employee’s department.
7. Deleted an employee younger than 25.

All tasks are complete, and you can continue exploring MongoDB with additional queries or operations. If you need further assistance or want to dive deeper into MongoDB features (e.g., aggregation, indexing), let me know!

### Key Citations
- [MongoDB Official Documentation](https://www.mongodb.com/docs/)
- [MongoDB Installation Guide](https://www.mongodb.com/docs/manual/installation/)
- [Docker MongoDB Image](https://hub.docker.com/_/mongo)
