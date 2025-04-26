# 📚 Phase 2: MongoDB Intermediate Level  
*(Schema Design, Query Mastery, Indexing, Aggregation)*

---

# 🧩 Step 1: MongoDB Schema Design (Embed vs Reference)

MongoDB gives you **flexibility**.  
But **how** you design your data decides your app's **performance** and **scalability**.

✅ In SQL, we normalize tables.  
✅ In MongoDB, we **choose between**:
- **Embed documents** inside other documents
- **Reference documents** between collections

---

## 🔥 1.1 Embed Documents (when?)

- When the relationship is **one-to-few**
- When **sub-document** will always be **fetched together** with the main document

✅ **Example:**

A student has one address.

```json
{
  "name": "Alice",
  "age": 22,
  "address": {
    "street": "123 Park Ave",
    "city": "New York",
    "zip": "10001"
  }
}
```

➡️ Address embedded inside Student.

---
  
## 🔥 1.2 Reference Documents (when?)

- When relationship is **one-to-many** OR **many-to-many**
- When sub-entities are **large** or **change independently**
- When you **don't always need** the full related data

✅ **Example:**

A user places multiple orders.

```json
// users collection
{
  "_id": "user1",
  "name": "John Doe"
}

// orders collection
{
  "_id": "order1",
  "userId": "user1",
  "total": 200
}
```

➡️ `orders` collection **references** `users` collection using `userId`.

---

# 🎯 Golden Rule for Schema Design:

| Question | Action |
|:---------|:-------|
| "Do I always need this related data together?" | Embed |
| "Do I often need only the main document?" | Reference |
| "Is the sub-data growing huge?" | Reference |
| "Is performance critical and data small?" | Embed |

✅ Well-designed schemas = **faster queries** + **less server load**.

---

# 🧩 Step 2: MongoDB Query Mastery (Filtering, Projection, Sorting)

---

## 🔍 2.1 Find Queries (Filtering)

✅ **Basic Find** (all documents):
```javascript
db.students.find();
```

✅ **Find with Condition** (WHERE clause):
```javascript
db.students.find({ age: { $gt: 20 } });
```
- `{}` → Query Condition
- `$gt` → Greater than Operator

✅ **Common Comparison Operators:**

| Operator | Meaning |
|:---------|:--------|
| `$eq` | Equal |
| `$ne` | Not Equal |
| `$gt` | Greater Than |
| `$lt` | Less Than |
| `$gte` | Greater Than or Equal |
| `$lte` | Less Than or Equal |
| `$in` | Value in Array |

✅ **Example:**
```javascript
db.students.find({ grade: { $in: ["A", "B"] } });
```
Find students whose grade is A or B.

---

## 🔎 2.2 Projection (Selecting Specific Fields)

✅ **Projection Format**:
```javascript
db.students.find(filter, projection);
```

✅ Example: Only show `name` and `age`
```javascript
db.students.find({}, { name: 1, age: 1, _id: 0 });
```
- `1` → Include field
- `0` → Exclude field

> 🚨 In projection, either you **include** fields OR **exclude** fields, not both together (except `_id`).

---

## 📑 2.3 Sorting

✅ Sort Ascending (small to big):
```javascript
db.students.find().sort({ age: 1 });
```

✅ Sort Descending (big to small):
```javascript
db.students.find().sort({ age: -1 });
```

---
  
# 🧩 Step 3: Indexing (Speed Booster)

## ⚡ 3.1 Why Index?

Without Index:  
- MongoDB scans **every document** → **Slow** on large collections.

With Index:  
- MongoDB **jumps directly** to matching documents → **Fast** queries.

---

## ⚡ 3.2 How to Create an Index

✅ Syntax:
```javascript
db.students.createIndex({ fieldName: 1 });
```
- `1` → Ascending Index
- `-1` → Descending Index

✅ Example: Create Index on `name`
```javascript
db.students.createIndex({ name: 1 });
```

✅ Example: Create Compound Index (`name` + `age`)
```javascript
db.students.createIndex({ name: 1, age: -1 });
```

---

## ⚡ 3.3 Common Mistakes with Indexes

- Too many indexes = **slow inserts** and **large disk size**
- **Always create indexes** based on **query patterns**, not blindly

✅ Rule of Thumb:  
Create Index if:
- You frequently filter/sort by that field
- It's a unique identifier

---

# 🧩 Step 4: Aggregation Framework (The Real Power)

Aggregation = Like **SQL GROUP BY**, **HAVING**, **ORDER BY** — but more powerful 🚀.

✅ **Aggregation Stages:**

| Stage | Purpose |
|:------|:--------|
| `$match` | Filter documents |
| `$group` | Group documents and calculate aggregates |
| `$sort` | Sort results |
| `$project` | Reshape output documents |

---

✅ **Simple Example: Find average age of students**
```javascript
db.students.aggregate([
  { $group: { _id: null, averageAge: { $avg: "$age" } } }
]);
```
- `$group`: groups all documents
- `$avg`: calculates average

---

✅ **Aggregation Example: Group students by grade**
```javascript
db.students.aggregate([
  { $group: { _id: "$grade", totalStudents: { $sum: 1 } } }
]);
```
- `_id: "$grade"`: Group by grade
- `$sum: 1`: Count students

---

# 📋 Phase 2 Mini-Checklist

| Skill | Status |
|:------|:------|
| Understand Embed vs Reference | ⬜ |
| Master Find queries | ⬜ |
| Master Projection and Sorting | ⬜ |
| Understand and Create Indexes | ⬜ |
| Perform Basic Aggregations | ⬜ |

✅ This checklist ensures you're at true **Intermediate Level**!

---

# 🎯 Your Hands-on Exercise for Phase 2

✅ Design a database `ecommerce`.  
✅ Create collections: `products`, `orders`, `customers`.  
✅ Insert 10 products with fields: `name`, `price`, `category`.  
✅ Create orders referencing customers.  
✅ Create an index on `price` field.  
✅ Write aggregation query:
- Find average price per category
- Find how many orders per customer

✅ Test sorting products by `price` descending.

---

### Solution

Below is a comprehensive guide to design an e-commerce database in MongoDB, create the specified collections, insert sample data, create an index, write aggregation queries, and test sorting. The instructions assume you have MongoDB installed locally or running via Docker (as per your previous context) and are using MongoDB Shell or MongoDB Compass. I'll provide Mongo Shell commands for precision, with notes for Compass where applicable. The current date is April 26, 2025, and all commands are based on MongoDB 7.0.x.

---

### Step 1: Design an E-commerce Database

An e-commerce database typically manages products, orders, and customers, with relationships between them. Here’s the design for the `ecommerce` database:

- **Database**: `ecommerce`
- **Collections**:
  - `products`: Stores product details (name, price, category).
  - `orders`: Stores order details, referencing customers and listing products.
  - `customers`: Stores customer details (name, email).
- **Relationships**:
  - `orders` references `customers` via `customerId` (one-to-many).
  - `orders` embeds or references `products` (many-to-many, typically via product IDs in an array).
- **Considerations**:
  - Use ObjectId for unique identifiers (`_id`).
  - Ensure indexes (e.g., on `price`) for query performance.
  - Design for scalability, supporting queries like average price per category and orders per customer.

---

### Step 2: Create Collections: `products`, `orders`, `customers`

1. **Switch to the `ecommerce` Database**:
   - **Mongo Shell**:
     ```javascript
     use ecommerce
     ```
     - Creates the `ecommerce` database implicitly when data is inserted.

   - **MongoDB Compass**:
     - Click “Create Database”, name it `ecommerce`, and proceed.

2. **Create Collections**:
   - In MongoDB, collections are created implicitly when data is inserted, but we can explicitly create them.
   - **Mongo Shell**:
     ```javascript
     db.createCollection("products")
     db.createCollection("orders")
     db.createCollection("customers")
     ```

   - **MongoDB Compass**:
     - In the `ecommerce` database, click “Create Collection” for each: `products`, `orders`, `customers`.

---

### Step 3: Insert 10 Products with Fields: `name`, `price`, `category`

Insert 10 product documents into the `products` collection.

- **Mongo Shell**:
  ```javascript
  db.products.insertMany([
    { name: "Laptop", price: 999.99, category: "Electronics" },
    { name: "Smartphone", price: 699.99, category: "Electronics" },
    { name: "Headphones", price: 149.99, category: "Accessories" },
    { name: "T-Shirt", price: 29.99, category: "Clothing" },
    { name: "Jeans", price: 59.99, category: "Clothing" },
    { name: "Watch", price: 199.99, category: "Accessories" },
    { name: "Tablet", price: 499.99, category: "Electronics" },
    { name: "Sneakers", price: 89.99, category: "Footwear" },
    { name: "Backpack", price: 79.99, category: "Accessories" },
    { name: "Jacket", price: 129.99, category: "Clothing" }
  ])
  ```
  - This inserts 10 products with varied categories and prices.

- **MongoDB Compass**:
  - Navigate to the `products` collection.
  - Click “Add Data” > “Insert Document”.
  - Enter each product in JSON format (e.g., `{ "name": "Laptop", "price": 999.99, "category": "Electronics" }`).
  - Repeat for all 10 products.

---

### Step 4: Create Orders Referencing Customers

First, insert sample customers into the `customers` collection, then create orders referencing them via `customerId`.

1. **Insert Customers**:
   - **Mongo Shell**:
     ```javascript
     db.customers.insertMany([
       { name: "Alice Smith", email: "alice@example.com" },
       { name: "Bob Johnson", email: "bob@example.com" },
       { name: "Carol Lee", email: "carol@example.com" }
     ])
     ```

   - **MongoDB Compass**:
     - In the `customers` collection, insert each customer document (e.g., `{ "name": "Alice Smith", "email": "alice@example.com" }`).

2. **Insert Orders**:
   - Orders reference `customerId` (from `customers._id`) and include an array of `productIds` (from `products._id`).
   - First, retrieve some product IDs for reference (you can skip this step if using Compass and manually note IDs).
   - **Mongo Shell** (example orders):
     ```javascript
     // Sample product IDs (replace with actual _id from your products)
     const productIds = db.products.find().limit(3).toArray().map(p => p._id);
     db.orders.insertMany([
       { 
         customerId: db.customers.findOne({ name: "Alice Smith" })._id, 
         productIds: [productIds[0], productIds[1]], // Laptop, Smartphone
         orderDate: new Date("2025-04-01")
       },
       { 
         customerId: db.customers.findOne({ name: "Bob Johnson" })._id, 
         productIds: [productIds[2]], // Headphones
         orderDate: new Date("2025-04-02")
       },
       { 
         customerId: db.customers.findOne({ name: "Alice Smith" })._id, 
         productIds: [productIds[1], productIds[2]], // Smartphone, Headphones
         orderDate: new Date("2025-04-03")
       },
       { 
         customerId: db.customers.findOne({ name: "Carol Lee" })._id, 
         productIds: [productIds[0]], // Laptop
         orderDate: new Date("2025-04-04")
       }
     ])
     ```
     - This creates four orders, with Alice having two orders, Bob one, and Carol one.

   - **MongoDB Compass**:
     - In the `customers` collection, note the `_id` of each customer.
     - In the `products` collection, note the `_id` of products.
     - In the `orders` collection, insert documents like:
       ```json
       {
         "customerId": ObjectId("..."), // Alice’s _id
         "productIds": [ObjectId("..."), ObjectId("...")], // Laptop, Smartphone
         "orderDate": { "$date": "2025-04-01T00:00:00Z" }
       }
       ```
     - Repeat for each order.

---

### Step 5: Create an Index on the `price` Field

Create an index on the `price` field in the `products` collection to optimize queries involving price.

- **Mongo Shell**:
  ```javascript
  db.products.createIndex({ price: 1 })
  ```
  - `1` indicates an ascending index. Use `-1` for descending if needed.
  - Verify the index:
    ```javascript
    db.products.getIndexes()
    ```
    - Output includes the new index on `price`.

- **MongoDB Compass**:
  - Navigate to the `products` collection.
  - Click the “Indexes” tab.
  - Click “Create Index”, add `price` with order “Ascending (1)”, and confirm.

---

### Step 6: Write Aggregation Queries

#### Aggregation Query 1: Find Average Price per Category

Calculate the average price of products grouped by category.

- **Mongo Shell**:
  ```javascript
  db.products.aggregate([
    {
      $group: {
        _id: "$category",
        averagePrice: { $avg: "$price" }
      }
    },
    {
      $sort: { _id: 1 } // Sort by category alphabetically
    }
  ])
  ```
  - `$group`: Groups by `category` and computes the average `price`.
  - Expected output (approximate, based on inserted data):
    ```json
    { "_id": "Accessories", "averagePrice": 143.32333333333334 }
    { "_id": "Clothing", "averagePrice": 73.32333333333334 }
    { "_id": "Electronics", "averagePrice": 733.3233333333334 }
    { "_id": "Footwear", "averagePrice": 89.99 }
    ```

- **MongoDB Compass**:
  - In the `products` collection, go to the “Aggregations” tab.
  - Add a `$group` stage:
    ```json
    {
      "_id": "$category",
      "averagePrice": { "$avg": "$price" }
    }
    ```
  - Add a `$sort` stage:
    ```json
    { "_id": 1 }
    ```
  - Run the pipeline to view results.

#### Aggregation Query 2: Find How Many Orders per Customer

Count the number of orders per customer, including customer names.

- **Mongo Shell**:
  ```javascript
  db.orders.aggregate([
    {
      $group: {
        _id: "$customerId",
        orderCount: { $sum: 1 }
      }
    },
    {
      $lookup: {
        from: "customers",
        localField: "_id",
        foreignField: "_id",
        as: "customerDetails"
      }
    },
    {
      $unwind: "$customerDetails"
    },
    {
      $project: {
        customerName: "$customerDetails.name",
        orderCount: 1,
        _id: 0
      }
    },
    {
      $sort: { customerName: 1 }
    }
  ])
  ```
  - `$group`: Counts orders per `customerId`.
  - `$lookup`: Joins with `customers` to get customer names.
  - `$unwind`: Flattens the `customerDetails` array.
  - `$project`: Selects `customerName` and `orderCount`.
  - Expected output:
    ```json
    { "customerName": "Alice Smith", "orderCount": 2 }
    { "customerName": "Bob Johnson", "orderCount": 1 }
    { "customerName": "Carol Lee", "orderCount": 1 }
    ```

- **MongoDB Compass**:
  - In the `orders` collection, go to the “Aggregations” tab.
  - Add stages:
    1. `$group`:
       ```json
       {
         "_id": "$customerId",
         "orderCount": { "$sum": 1 }
       }
       ```
    2. `$lookup`:
       ```json
       {
         "from": "customers",
         "localField": "_id",
         "foreignField": "_id",
         "as": "customerDetails"
       }
       ```
    3. `$unwind`:
       ```json
       { "path": "$customerDetails" }
       ```
    4. `$project`:
       ```json
       {
         "customerName": "$customerDetails.name",
         "orderCount": 1,
         "_id": 0
       }
       ```
    5. `$sort`:
       ```json
       { "customerName": 1 }
       ```
  - Run the pipeline to view results.

---

### Step 7: Test Sorting Products by Price Descending

Sort products by `price` in descending order.

- **Mongo Shell**:
  ```javascript
  db.products.find().sort({ price: -1 }).pretty()
  ```
  - `-1` indicates descending order.
  - Expected output (partial):
    ```json
    {
      "_id": ObjectId("..."),
      "name": "Laptop",
      "price": 999.99,
      "category": "Electronics"
    }
    {
      "_id": ObjectId("..."),
      "name": "Smartphone",
      "price": 699.99,
      "category": "Electronics"
    }
    ...
    ```

- **MongoDB Compass**:
  - In the `products` collection, enter the query `{}`.
  - In the “Sort” field, enter `{ "price": -1 }`.
  - Click “Find” to display products sorted by price descending.

---

### Verification and Troubleshooting
- **Verify All Steps**:
  - Check `products`: `db.products.find().count()` should return 10.
  - Check `customers`: `db.customers.find().count()` should return 3.
  - Check `orders`: `db.orders.find().count()` should return 4.
  - Verify the index: `db.products.getIndexes()` should show the `price_1` index.
  - Re-run aggregation queries to ensure correct output.
  - Confirm sorting: The highest-priced product (Laptop, 999.99) should appear first.

- **Common Issues**:
  - **Database not found**: Ensure you’re in the `ecommerce` database (`use ecommerce`).
  - **Invalid ObjectId**: When inserting orders, ensure `customerId` and `productIds` match actual `_id` values from `customers` and `products`.
  - **Aggregation errors**: Check syntax, especially for `$lookup` and `$unwind`, and ensure collection names are correct.
  - **Connection issues**: Verify MongoDB is running (`mongosh` or Docker `docker ps`).

---

### Artifact: MongoDB Script for E-commerce Database Setup and Queries

```javascript
use ecommerce

// Create collections
db.createCollection("products")
db.createCollection("orders")
db.createCollection("customers")

// Insert 10 products
db.products.insertMany([
  { name: "Laptop", price: 999.99, category: "Electronics" },
  { name: "Smartphone", price: 699.99, category: "Electronics" },
  { name: "Headphones", price: 149.99, category: "Accessories" },
  { name: "T-Shirt", price: 29.99, category: "Clothing" },
  { name: "Jeans", price: 59.99, category: "Clothing" },
  { name: "Watch", price: 199.99, category: "Accessories" },
  { name: "Tablet", price: 499.99, category: "Electronics" },
  { name: "Sneakers", price: 89.99, category: "Footwear" },
  { name: "Backpack", price: 79.99, category: "Accessories" },
  { name: "Jacket", price: 129.99, category: "Clothing" }
])

// Insert customers
db.customers.insertMany([
  { name: "Alice Smith", email: "alice@example.com" },
  { name: "Bob Johnson", email: "bob@example.com" },
  { name: "Carol Lee", email: "carol@example.com" }
])

// Insert orders
const productIds = db.products.find().limit(3).toArray().map(p => p._id);
db.orders.insertMany([
  { 
    customerId: db.customers.findOne({ name: "Alice Smith" })._id, 
    productIds: [productIds[0], productIds[1]], 
    orderDate: new Date("2025-04-01")
  },
  { 
    customerId: db.customers.findOne({ name: "Bob Johnson" })._id, 
    productIds: [productIds[2]], 
    orderDate: new Date("2025-04-02")
  },
  { 
    customerId: db.customers.findOne({ name: "Alice Smith" })._id, 
    productIds: [productIds[1], productIds[2]], 
    orderDate: new Date("2025-04-03")
  },
  { 
    customerId: db.customers.findOne({ name: "Carol Lee" })._id, 
    productIds: [productIds[0]], 
    orderDate: new Date("2025-04-04")
  }
])

// Create index on price
db.products.createIndex({ price: 1 })

// Aggregation: Average price per category
db.products.aggregate([
  {
    $group: {
      _id: "$category",
      averagePrice: { $avg: "$price" }
    }
  },
  {
    $sort: { _id: 1 }
  }
])

// Aggregation: Orders per customer
db.orders.aggregate([
  {
    $group: {
      _id: "$customerId",
2193
      orderCount: { $sum: 1 }
    }
  },
  {
    $lookup: {
      from: "customers",
      localField: "_id",
      foreignField: "_id",
      as: "customerDetails"
    }
  },
  {
    $unwind: "$customerDetails"
  },
  {
    $project: {
      customerName: "$customerDetails.name",
      orderCount: 1,
      _id: 0
    }
  },
  {
    $sort: { customerName: 1 }
  }
])

// Sort products by price descending
db.products.find().sort({ price: -1 }).pretty()
```

---

### Summary
You’ve successfully:
1. Designed an `ecommerce` database with `products`, `orders`, and `customers` collections.
2. Inserted 10 products with `name`, `price`, and `category`.
3. Created orders referencing customers via `customerId`.
4. Created an index on the `price` field.
5. Wrote aggregation queries to find the average price per category and orders per customer.
6. Tested sorting products by price in descending order.

The provided script (`ecommerce_setup.js`) can be run in Mongo Shell (`mongosh < ecommerce_setup.js`) to execute all steps. If you need further customization (e.g., additional queries, schema changes), let me know!

### Key Citations
- [MongoDB Official Documentation](https://www.mongodb.com/docs/)
- [MongoDB Aggregation Pipeline](https://www.mongodb.com/docs/manual/core/aggregation-pipeline/)
- [MongoDB Indexing](https://www.mongodb.com/docs/manual/indexes/)

