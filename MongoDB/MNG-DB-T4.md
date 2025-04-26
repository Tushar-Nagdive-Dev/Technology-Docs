# 📚 Deep Dive into MongoDB Aggregation Framework (Multi-Stage Pipelines)

---

# 🧩 Step 1: Understanding "Stages" in Aggregation

✅ Aggregation **Pipeline** =  
- A **series of stages**.
- Each stage **takes input documents**, **processes**, and **outputs** to the next stage.

🔵 *Important: Output of Stage 1 becomes Input for Stage 2*  
🔵 *Stages are executed in the order you write them.*

Think of it like this:

```plaintext
Input → Stage 1 ($match) → Stage 2 ($group) → Stage 3 ($sort) → Output
```

---

# 🧠 Step 2: Commonly Used Aggregation Stages

| Stage | Purpose |
|:------|:--------|
| `$match` | Filter documents (like SQL `WHERE`) |
| `$group` | Group documents (like SQL `GROUP BY`) |
| `$project` | Select specific fields, reshape documents |
| `$sort` | Sort documents |
| `$limit` | Limit number of documents |
| `$lookup` | Perform joins across collections |
| `$unwind` | Break array fields into separate documents |

---

# 🎯 Golden Strategy for Aggregations:

✅ Always start with `$match` first to **reduce the number of documents early**.  
✅ Then use `$group` to **group or summarize** your data.  
✅ Then `$project` to **reshape** final output.  
✅ Then `$sort` to **order** final results.  

---

# 🚀 Step 3: Multi-Stage Aggregation Examples

---

## 🛠 Example 1: Students Analysis  
*Goal: Find average age of students who are above 20 years, and show only name and age sorted descending.*

### 🧠 SQL Equivalent:
```sql
SELECT name, age FROM students WHERE age > 20 ORDER BY age DESC;
```

### ✅ MongoDB Aggregation:
```javascript
db.students.aggregate([
  { $match: { age: { $gt: 20 } } },            // Stage 1: Filter students with age > 20
  { $project: { name: 1, age: 1, _id: 0 } },    // Stage 2: Only show name and age
  { $sort: { age: -1 } }                        // Stage 3: Sort descending by age
]);
```

| Stage | Purpose |
|:------|:--------|
| `$match` | Filter students aged > 20 |
| `$project` | Keep only `name` and `age` |
| `$sort` | Sort by age descending |

---

## 🛠 Example 2: E-commerce Orders Analysis  
*Goal: Find total spending per customer, sort by highest spender first.*

### 🧠 SQL Equivalent:
```sql
SELECT customerId, SUM(totalPrice) FROM orders GROUP BY customerId ORDER BY SUM(totalPrice) DESC;
```

### ✅ MongoDB Aggregation:
```javascript
db.orders.aggregate([
  { $group: { _id: "$customerId", totalSpent: { $sum: "$totalPrice" } } },   // Stage 1: Group by customerId
  { $sort: { totalSpent: -1 } }                                               // Stage 2: Sort descending by spending
]);
```

| Stage | Purpose |
|:------|:--------|
| `$group` | Sum totalPrice per customer |
| `$sort` | Order customers by total spent descending |

---

## 🛠 Example 3: Products Category Analysis  
*Goal: Find number of products in each category, only for categories with more than 5 products.*

### ✅ MongoDB Aggregation:
```javascript
db.products.aggregate([
  { $group: { _id: "$category", totalProducts: { $sum: 1 } } }, // Stage 1: Group by category
  { $match: { totalProducts: { $gt: 5 } } },                    // Stage 2: Only categories with >5 products
  { $sort: { totalProducts: -1 } }                              // Stage 3: Sort descending by count
]);
```

| Stage | Purpose |
|:------|:--------|
| `$group` | Count products per category |
| `$match` | Filter only categories having more than 5 products |
| `$sort` | Sort descending by number of products |

---

## 🛠 Example 4: Using `$lookup` (MongoDB JOIN)

✅ **Problem:**  
Suppose you have `orders` and `customers`.  
You want to fetch orders **with customer details attached**.

### ✅ MongoDB Aggregation:
```javascript
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",         // join with customers collection
      localField: "customerId",   // field from orders
      foreignField: "_id",        // field from customers
      as: "customerDetails"       // output array field
    }
  }
]);
```

🧠 `$lookup` = MongoDB JOIN operation.

- It matches `orders.customerId` with `customers._id`.
- Attaches matched customer details as `customerDetails` array in the result.

✅ This is **highly powerful** — allows multi-collection queries like relational DBs.

---

# 🧠 Step 4: More Advanced Operators (To Be Used Later)

| Operator | Purpose |
|:---------|:--------|
| `$unwind` | Deconstructs array fields (makes one document per array element) |
| `$addFields` | Add new fields dynamically |
| `$facet` | Multiple parallel pipelines |
| `$bucket` | Group data into ranges (like 0-10, 10-20) |

---

# ✨ Quick Professional Summary: Multi-Stage Aggregation Flow

✅ Always reduce documents early (`$match`)  
✅ Group if needed (`$group`)  
✅ Reshape (`$project`)  
✅ Sort (`$sort`)  
✅ Join if needed (`$lookup`)  
✅ Flatten arrays if needed (`$unwind`)  
✅ Final output with correct fields

---

# 📝 Practice Exercise (Deep Aggregation Challenge)

✅ Create database `onlineSchool`.  
✅ Create collections: `courses`, `students`, `enrollments`.

- `courses` (fields: courseName, courseFee)
- `students` (fields: name, age)
- `enrollments` (fields: studentId, courseId, date)

✅ Now perform aggregations:
1. Find **average course fee** per category.
2. Find **total enrollments** per course.
3. Find **students with total fees paid** (join `enrollments` + `courses`).
4. Find **student who enrolled in the most courses**.

---

### Solution

Below is a comprehensive guide to create the `onlineSchool` database in MongoDB, set up the specified collections (`courses`, `students`, `enrollments`), insert sample data, and perform the requested aggregation queries. I’ll provide MongoDB Shell (`mongosh`) commands, with MongoDB Compass alternatives where applicable. The instructions assume MongoDB is running locally or via Docker, as per your previous setup, and are tailored for MongoDB 7.0.x as of April 26, 2025. Since the `courses` collection doesn’t include a `category` field but the first aggregation requires it, I’ll add a `category` field to `courses` and explain the assumption.

---

### Step 1: Create Database `onlineSchool`

Switch to the `onlineSchool` database, which is created implicitly when data is inserted.

- **Mongo Shell**:
  ```javascript
  use onlineSchool
  ```

- **MongoDB Compass**:
  - Click “Create Database”.
  - Enter `onlineSchool` as the database name and proceed.

---

### Step 2: Create Collections: `courses`, `students`, `enrollments`

Create the collections with the specified fields:
- `courses`: `courseName`, `courseFee`, `category` (added for aggregation).
- `students`: `name`, `age`.
- `enrollments`: `studentId`, `courseId`, `date`.

- **Mongo Shell**:
  ```javascript
  db.createCollection("courses")
  db.createCollection("students")
  db.createCollection("enrollments")
  ```

- **MongoDB Compass**:
  - In the `onlineSchool` database, click “Create Collection”.
  - Create `courses`, `students`, and `enrollments`.

---

### Step 3: Insert Sample Data

To perform meaningful aggregations, I’ll insert sample data into each collection.

#### Insert Courses
Include a `category` field to support the “average course fee per category” aggregation.

- **Mongo Shell**:
  ```javascript
  db.courses.insertMany([
    { courseName: "Introduction to Python", courseFee: 500.00, category: "Programming" },
    { courseName: "Data Science Basics", courseFee: 800.00, category: "Data Science" },
    { courseName: "Web Development", courseFee: 600.00, category: "Programming" },
    { courseName: "Machine Learning", courseFee: 1000.00, category: "Data Science" },
    { courseName: "Graphic Design", courseFee: 400.00, category: "Design" }
  ])
  ```

- **MongoDB Compass**:
  - In the `courses` collection, click “Add Data” > “Insert Document”.
  - Insert each course, e.g.:
    ```json
    {
      "courseName": "Introduction to Python",
      "courseFee": 500.00,
      "category": "Programming"
    }
    ```
  - Repeat for all courses.

#### Insert Students
- **Mongo Shell**:
  ```javascript
  db.students.insertMany([
    { name: "Alice Smith", age: 20 },
    { name: "Bob Johnson", age: 22 },
    { name: "Carol Lee", age: 19 },
    { name: "David Brown", age: 21 }
  ])
  ```

- **MongoDB Compass**:
  - In the `students` collection, insert each student, e.g.:
    ```json
    {
      "name": "Alice Smith",
      "age": 20
    }
    ```

#### Insert Enrollments
Reference `studentId` and `courseId` using `_id` from `students` and `courses`.

- **Mongo Shell**:
  ```javascript
  db.enrollments.insertMany([
    { 
      studentId: db.students.findOne({ name: "Alice Smith" })._id, 
      courseId: db.courses.findOne({ courseName: "Introduction to Python" })._id, 
      date: new Date("2025-01-01") 
    },
    { 
      studentId: db.students.findOne({ name: "Alice Smith" })._id, 
      courseId: db.courses.findOne({ courseName: "Web Development" })._id, 
      date: new Date("2025-01-02") 
    },
    { 
      studentId: db.students.findOne({ name: "Alice Smith" })._id, 
      courseId: db.courses.findOne({ courseName: "Machine Learning" })._id, 
      date: new Date("2025-01-03") 
    },
    { 
      studentId: db.students.findOne({ name: "Bob Johnson" })._id, 
      courseId: db.courses.findOne({ courseName: "Data Science Basics" })._id, 
      date: new Date("2025-01-04") 
    },
    { 
      studentId: db.students.findOne({ name: "Carol Lee" })._id, 
      courseId: db.courses.findOne({ courseName: "Graphic Design" })._id, 
      date: new Date("2025-01-05") 
    },
    { 
      studentId: db.students.findOne({ name: "David Brown" })._id, 
      courseId: db.courses.findOne({ courseName: "Introduction to Python" })._id, 
      date: new Date("2025-01-06") 
    }
  ])
  ```

- **MongoDB Compass**:
  - In the `students` and `courses` collections, note the `_id` values.
  - In the `enrollments` collection, insert documents, e.g.:
    ```json
    {
      "studentId": ObjectId("..."), // Alice’s _id
      "courseId": ObjectId("..."), // Python course _id
      "date": { "$date": "2025-01-01T00:00:00Z" }
    }
    ```
  - Repeat for all enrollments.

---

### Step 4: Perform Aggregations

#### Aggregation 1: Find Average Course Fee per Category
Calculate the average `courseFee` for each `category` in the `courses` collection.

- **Mongo Shell**:
  ```javascript
  db.courses.aggregate([
    {
      $group: {
        _id: "$category",
        averageFee: { $avg: "$courseFee" }
      }
    },
    {
      $sort: { _id: 1 } // Sort alphabetically by category
    }
  ])
  ```
  - **Expected Output**:
    ```json
    { "_id": "Data Science", "averageFee": 900 }
    { "_id": "Design", "averageFee": 400 }
    { "_id": "Programming", "averageFee": 550 }
    ```

- **MongoDB Compass**:
  - In the `courses` collection, go to “Aggregations”.
  - Add stages:
    1. `$group`:
       ```json
       {
         "_id": "$category",
         "averageFee": { "$avg": "$courseFee" }
       }
       ```
    2. `$sort`:
       ```json
       { "_id": 1 }
       ```
  - Run the pipeline.

---

#### Aggregation 2: Find Total Enrollments per Course
Count the number of enrollments for each course, including course names.

- **Mongo Shell**:
  ```javascript
  db.enrollments.aggregate([
    {
      $group: {
        _id: "$courseId",
        enrollmentCount: { $sum: 1 }
      }
    },
    {
      $lookup: {
        from: "courses",
        localField: "_id",
        foreignField: "_id",
        as: "courseDetails"
      }
    },
    {
      $unwind: "$courseDetails"
    },
    {
      $project: {
        courseName: "$courseDetails.courseName",
        enrollmentCount: 1,
        _id: 0
      }
    },
    {
      $sort: { courseName: 1 }
    }
  ])
  ```
  - **Expected Output**:
    ```json
    { "courseName": "Data Science Basics", "enrollmentCount": 1 }
    { "courseName": "Graphic Design", "enrollmentCount": 1 }
    { "courseName": "Introduction to Python", "enrollmentCount": 2 }
    { "courseName": "Machine Learning", "enrollmentCount": 1 }
    { "courseName": "Web Development", "enrollmentCount": 1 }
    ```

- **MongoDB Compass**:
  - In the `enrollments` collection, go to “Aggregations”.
  - Add stages:
    1. `$group`:
       ```json
       {
         "_id": "$courseId",
         "enrollmentCount": { "$sum": 1 }
       }
       ```
    2. `$lookup`:
       ```json
       {
         "from": "courses",
         "localField": "_id",
         "foreignField": "_id",
         "as": "courseDetails"
       }
       ```
    3. `$unwind`:
       ```json
       { "path": "$courseDetails" }
       ```
    4. `$project`:
       ```json
       {
         "courseName": "$courseDetails.courseName",
         "enrollmentCount": 1,
         "_id": 0
       }
       ```
    5. `$sort`:
       ```json
       { "courseName": 1 }
       ```
  - Run the pipeline.

---

#### Aggregation 3: Find Students with Total Fees Paid
Calculate the total fees paid by each student by joining `enrollments` with `courses`.

- **Mongo Shell**:
  ```javascript
  db.enrollments.aggregate([
    {
      $lookup: {
        from: "courses",
        localField: "courseId",
        foreignField: "_id",
        as: "courseDetails"
      }
    },
    {
      $unwind: "$courseDetails"
    },
    {
      $group: {
        _id: "$studentId",
        totalFees: { $sum: "$courseDetails.courseFee" }
      }
    },
    {
      $lookup: {
        from: "students",
        localField: "_id",
        foreignField: "_id",
        as: "studentDetails"
      }
    },
    {
      $unwind: "$studentDetails"
    },
    {
      $project: {
        studentName: "$studentDetails.name",
        totalFees: 1,
        _id: 0
      }
    },
    {
      $sort: { studentName: 1 }
    }
  ])
  ```
  - **Expected Output**:
    ```json
    { "studentName": "Alice Smith", "totalFees": 2100 } // Python (500) + Web (600) + ML (1000)
    { "studentName": "Bob Johnson", "totalFees": 800 } // Data Science (800)
    { "studentName": "Carol Lee", "totalFees": 400 } // Graphic Design (400)
    { "studentName": "David Brown", "totalFees": 500 } // Python (500)
    ```

- **MongoDB Compass**:
  - In the `enrollments` collection, go to “Aggregations”.
  - Add stages:
    1. `$lookup`:
       ```json
       {
         "from": "courses",
         "localField": "courseId",
         "foreignField": "_id",
         "as": "courseDetails"
       }
       ```
    2. `$unwind`:
       ```json
       { "path": "$courseDetails" }
       ```
    3. `$group`:
       ```json
       {
         "_id": "$studentId",
         "totalFees": { "$sum": "$courseDetails.courseFee" }
       }
       ```
    4. `$lookup`:
       ```json
       {
         "from": "students",
         "localField": "_id",
         "foreignField": "_id",
         "as": "studentDetails"
       }
       ```
    5. `$unwind`:
       ```json
       { "path": "$studentDetails" }
       ```
    6. `$project`:
       ```json
       {
         "studentName": "$studentDetails.name",
         "totalFees": 1,
         "_id": 0
       }
       ```
    7. `$sort`:
       ```json
       { "studentName": 1 }
       ```
  - Run the pipeline.

---

#### Aggregation 4: Find Student Who Enrolled in the Most Courses
Identify the student with the highest number of enrollments.

- **Mongo Shell**:
  ```javascript
  db.enrollments.aggregate([
    {
      $group: {
        _id: "$studentId",
        enrollmentCount: { $sum: 1 }
      }
    },
    {
      $lookup: {
        from: "students",
        localField: "_id",
        foreignField: "_id",
        as: "studentDetails"
      }
    },
    {
      $unwind: "$studentDetails"
    },
    {
      $project: {
        studentName: "$studentDetails.name",
        enrollmentCount: 1,
        _id: 0
      }
    },
    {
      $sort: { enrollmentCount: -1 } // Sort descending by enrollmentCount
    },
    {
      $limit: 1 // Get the top student
    }
  ])
  ```
  - **Expected Output**:
    ```json
    { "studentName": "Alice Smith", "enrollmentCount": 3 }
    ```
    - Alice enrolled in Python, Web Development, and Machine Learning.

- **MongoDB Compass**:
  - In the `enrollments` collection, go to “Aggregations”.
  - Add stages:
    1. `$group`:
       ```json
       {
         "_id": "$studentId",
         "enrollmentCount": { "$sum": 1 }
       }
       ```
    2. `$lookup`:
       ```json
       {
         "from": "students",
         "localField": "_id",
         "foreignField": "_id",
         "as": "studentDetails"
       }
       ```
    3. `$unwind`:
       ```json
       { "path": "$studentDetails" }
       ```
    4. `$project`:
       ```json
       {
         "studentName": "$studentDetails.name",
         "enrollmentCount": 1,
         "_id": 0
       }
       ```
    5. `$sort`:
       ```json
       { "enrollmentCount": -1 }
       ```
    6. `$limit`:
       ```json
       1
       ```
  - Run the pipeline.

---

### Verification
- **Collections**:
  - `db.courses.find().count()`: Should return 5.
  - `db.students.find().count()`: Should return 4.
  - `db.enrollments.find().count()`: Should return 6.
- **Aggregations**:
  - **Average Fee**: Verify three categories (Data Science: 900, Design: 400, Programming: 550).
  - **Enrollments per Course**: Check Python has 2 enrollments, others have 1.
  - **Total Fees**: Confirm Alice’s total (2100), Bob’s (800), Carol’s (400), David’s (500).
  - **Most Enrollments**: Ensure Alice has 3 enrollments.
- **Troubleshooting**:
  - **Empty Results**: Verify data in `courses`, `students`, `enrollments`. Check `studentId` and `courseId` match `_id` values.
  - **Aggregation Errors**: Ensure collection names and field names are correct.
  - **MongoDB Not Running**: Check with `mongosh` or `docker ps`.

---

### Artifact: MongoDB Script for `onlineSchool` Setup and Aggregations

```javascript
use onlineSchool;

// Create collections
db.createCollection("courses")
db.createCollection("students")
db.createCollection("enrollments")

// Insert courses
db.courses.insertMany([
  { courseName: "Introduction to Python", courseFee: 500.00, category: "Programming" },
  { courseName: "Data Science Basics", courseFee: 800.00, category: "Data Science" },
  { courseName: "Web Development", courseFee: 600.00, category: "Programming" },
  { courseName: "Machine Learning", courseFee: 1000.00, category: "Data Science" },
  { courseName: "Graphic Design", courseFee: 400.00, category: "Design" }
])

// Insert students
db.students.insertMany([
  { name: "Alice Smith", age: 20 },
  { name: "Bob Johnson", age: 22 },
  { name: "Carol Lee", age: 19 },
  { name: "David Brown", age: 21 }
])

// Insert enrollments
db.enrollments.insertMany([
  { 
    studentId: db.students.findOne({ name: "Alice Smith" })._id, 
    courseId: db.courses.findOne({ courseName: "Introduction to Python" })._id, 
    date: new Date("2025-01-01") 
  },
  { 
    studentId: db.students.findOne({ name: "Alice Smith" })._id, 
    courseId: db.courses.findOne({ courseName: "Web Development" })._id, 
    date: new Date("2025-01-02") 
  },
  { 
    studentId: db.students.findOne({ name: "Alice Smith" })._id, 
    courseId: db.courses.findOne({ courseName: "Machine Learning" })._id, 
    date: new Date("2025-01-03") 
  },
  { 
    studentId: db.students.findOne({ name: "Bob Johnson" })._id, 
    courseId: db.courses.findOne({ courseName: "Data Science Basics" })._id, 
    date: new Date("2025-01-04") 
  },
  { 
    studentId: db.students.findOne({ name: "Carol Lee" })._id, 
    courseId: db.courses.findOne({ courseName: "Graphic Design" })._id, 
    date: new Date("2025-01-05") 
  },
  { 
    studentId: db.students.findOne({ name: "David Brown" })._id, 
    courseId: db.courses.findOne({ courseName: "Introduction to Python" })._id, 
    date: new Date("2025-01-06") 
  }
])

// Aggregation 1: Average course fee per category
db.courses.aggregate([
  {
    $group: {
      _id: "$category",
      averageFee: { $avg: "$courseFee" }
    }
  },
  {
    $sort: { _id: 1 }
  }
])

// Aggregation 2: Total enrollments per course
db.enrollments.aggregate([
  {
    $group: {
      _id: "$courseId",
      enrollmentCount: { $sum: 1 }
    }
  },
  {
    $lookup: {
      from: "courses",
      localField: "_id",
      foreignField: "_id",
      as: "courseDetails"
    }
  },
  {
    $unwind: "$courseDetails"
  },
  {
    $project: {
      courseName: "$courseDetails.courseName",
      enrollmentCount: 1,
      _id: 0
    }
  },
  {
    $sort: { courseName: 1 }
  }
])

// Aggregation 3: Total fees paid per student
db.enrollments.aggregate([
  {
    $lookup: {
      from: "courses",
      localField: "courseId",
      foreignField: "_id",
      as: "courseDetails"
    }
  },
  {
    $unwind: "$courseDetails"
  },
  {
    $group: {
      _id: "$studentId",
      totalFees: { $sum: "$courseDetails.courseFee" }
    }
  },
  {
    $lookup: {
      from: "students",
      localField: "_id",
      foreignField: "_id",
      as: "studentDetails"
    }
  },
  {
    $unwind: "$studentDetails"
  },
  {
    $project: {
      studentName: "$studentDetails.name",
      totalFees: 1,
      _id: 0
    }
  },
  {
    $sort: { studentName: 1 }
  }
])

// Aggregation 4: Student with most enrollments
db.enrollments.aggregate([
  {
    $group: {
      _id: "$studentId",
      enrollmentCount: { $sum: 1 }
    }
  },
  {
    $lookup: {
      from: "students",
      localField: "_id",
      foreignField: "_id",
      as: "studentDetails"
    }
  },
  {
    $unwind: "$studentDetails"
  },
  {
    $project: {
      studentName: "$studentDetails.name",
      enrollmentCount: 1,
      _id: 0
    }
  },
  {
    $sort: { enrollmentCount: -1 }
  },
  {
    $limit: 1
  }
])
```

---

### How to Run
1. **Ensure MongoDB is Running**:
   - Local: `mongod`.
   - Docker: `docker run -d -p 27017:27017 mongo:latest`.
2. **Save the Script**:
   - Save as `online_school_setup.js`.
3. **Execute**:
   ```bash
   mongosh --file online_school_setup.js
   ```
4. **Verify**:
   - Connect to `mongosh`, use `onlineSchool`, and check:
     ```javascript
     db.courses.find().pretty()
     db.students.find().pretty()
     db.enrollments.find().pretty()
     ```

---

### Summary
You’ve successfully:
1. Created the `onlineSchool` database with `courses`, `students`, and `enrollments` collections.
2. Inserted sample data with `category` in `courses` for aggregation.
3. Performed aggregations:
   - Average course fee per category (e.g., Data Science: 900).
   - Total enrollments per course (e.g., Python: 2).
   - Total fees paid per student (e.g., Alice: 2100).
   - Student with most enrollments (Alice: 3 courses).

The script (`online_school_setup.js`) is ready to run in `mongosh`. If you need Node.js support, additional queries, or modifications (e.g., different data), let me know!

### Key Citations
- [MongoDB Official Documentation](https://www.mongodb.com/docs/)
- [MongoDB Aggregation Pipeline](https://www.mongodb.com/docs/manual/core/aggregation-pipeline/)
- [MongoDB $lookup](https://www.mongodb.com/docs/manual/reference/operator/aggregation/lookup/)
