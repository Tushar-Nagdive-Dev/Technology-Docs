# 📚 Deep Dive: Aggregation Framework in MongoDB

---

# 🧠 What is Aggregation?

**Aggregation** is a way of **processing documents** and **returning transformed results**.

It’s like saying:

🔹 "Hey MongoDB, I don't just want raw documents.  
🔹 I want to filter them, group them, calculate on them, and reshape them into new forms!"

👉 In SQL, we use `GROUP BY`, `HAVING`, `SUM()`, `AVG()` —  
👉 In MongoDB, **Aggregation Pipeline** is even more powerful.

---

# 🚀 What is an Aggregation Pipeline?

An **Aggregation Pipeline** is a **series of stages**.  
Each stage **takes documents**, **transforms them**, and **passes them to the next stage**.

✅ Think of it like **water flowing through pipes**:  
Data → Stage 1 → Stage 2 → Stage 3 → Final Output.

Each stage is an operation like filtering, grouping, sorting, reshaping, etc.

---

# 📦 Common Aggregation Stages (Important!)

| Stage | Purpose | Example |
|:------|:--------|:--------|
| `$match` | Filters documents (like WHERE) | `{ age: { $gt: 20 } }` |
| `$group` | Groups documents and calculates | `{ _id: "$city", total: { $sum: 1 } }` |
| `$project` | Reshapes documents | `{ name: 1, age: 1 }` |
| `$sort` | Sorts documents | `{ age: -1 }` |
| `$limit` | Limits number of documents | `5` |
| `$lookup` | Joins documents across collections | (Advanced) |

---

# 🎯 Important Aggregation Operators

✅ `$sum` — Calculate sum  
✅ `$avg` — Calculate average  
✅ `$max` — Find maximum  
✅ `$min` — Find minimum  
✅ `$push` — Create array of values  
✅ `$first`, `$last` — First/last document in a group

---

# 🔥 Real-World Aggregation Examples

---

## Example 1: Find the Average Age of Students

```javascript
db.students.aggregate([
  { $group: { _id: null, avgAge: { $avg: "$age" } } }
]);
```

🧠 Here:
- `$group`: Group all documents together
- `avgAge: { $avg: "$age" }`: Calculate average of age

---

## Example 2: Find How Many Students in Each Grade

```javascript
db.students.aggregate([
  { $group: { _id: "$grade", studentCount: { $sum: 1 } } }
]);
```

🧠 Here:
- `_id: "$grade"`: Group by grade field
- `studentCount`: Count documents per grade

---

## Example 3: Sort Students by Age

```javascript
db.students.aggregate([
  { $sort: { age: -1 } }
]);
```

🧠 Here:
- Sort students descending by age.

---

## Example 4: Show Only Name and Age

```javascript
db.students.aggregate([
  { $project: { _id: 0, name: 1, age: 1 } }
]);
```

🧠 Here:
- Only show name and age fields.
- Hide `_id`.

---

# 🎯 Golden Professional Advice about Aggregation

✅ Always use `$match` early to **reduce documents** at the start (faster processing).  
✅ Always `$project` only required fields (save memory).  
✅ `$group` results carefully — think what you want to calculate.  
✅ Aggregation Pipelines can be **very long** — 10, 20, 30 stages even!

Aggregation is the **real muscle** of MongoDB for Analytics, Reporting, and Data Transformations.

---

# ✏️ Easy Way to Remember Aggregation Pipeline

> **Maya's Trick:**  
> "First Match, Then Group, Then Project, Then Sort!"

- First `$match` (filter early)
- Then `$group` (summarize)
- Then `$project` (reshape)
- Then `$sort` (order output)

✅ This order is not mandatory but very **common**.

---

# 📋 Phase 2 - Practice Tasks (Hands-On)

### ✅ Practice Set 1: Basic CRUD and Query

1. Create Database: `ecommerce`
2. Create Collections:
   - `products`
   - `customers`
   - `orders`

3. Insert 10 `products` with fields:
   - `name`
   - `price`
   - `category`

4. Insert 5 `customers` with fields:
   - `name`
   - `email`
   - `address`

5. Insert `orders` where:
   - each order links a customer
   - has list of purchased products
   - total price

---

### ✅ Practice Set 2: Aggregation and Indexing

1. Find all products where price > 5000.

2. Project only name and price for all products.

3. Sort all products by price descending.

4. Create an index on `price`.

5. Create an aggregation pipeline:
   - Find **average product price** per `category`.

6. Create an aggregation:
   - Find **total number of orders** per customer.

7. Create an aggregation:
   - Find **total sales amount** (`sum of totalPrice`) per customer.

---

# 🧠 Bonus Challenging Task for Fun 🎯

8. Create a query:
   - Find **the customer who spent the most** in total.

*(Hint: Group orders by customer, sum totalPrice, then sort descending.)*

---

# 📜 Conclusion of Phase 2

✅ You now know:
- MongoDB **Schema Design** principles  
- How to write powerful **Find queries**  
- **Sorting**, **Projection**, and **Filtering**  
- **Create and use Indexes** properly  
- **Design Aggregation Pipelines** like a pro  

---

# 🚀 Ready for Phase 3?

In Phase 3, we will go **Advanced MongoDB**:
- `$lookup` (MongoDB Joins)
- `$unwind` (Array flattening)
- Schema Validation (strictness)
- Transactions (ACID)
- Replication and Sharding for Big Systems

---

### Solution

Below is a comprehensive guide to complete the tasks in **Practice Set 2: Aggregation and Indexing** for the `ecommerce` database created in your previous interactions. The tasks involve querying products, creating an index, and writing aggregation pipelines. I’ll assume you’re working with the `ecommerce` database containing the `products`, `orders`, and `customers` collections, as set up previously. Since the previous data had prices much lower than 5000 (e.g., max 999.99), I’ll address the price query contextually and provide a note about it. All commands are for MongoDB Shell (`mongosh`), with MongoDB Compass alternatives where applicable. If you prefer Node.js, I can adapt the script, but I’ll use `mongosh` to align with your recent preference for avoiding Node.js runtime errors.

---

### Contextual Note on Price > 5000
The sample products inserted previously (e.g., Laptop: 999.99, Smartphone: 699.99) have prices far below 5000. A query for `price > 5000` would return no results with the current data. To make the task meaningful, I’ll:
- Assume you meant a lower threshold (e.g., `price > 500`) to match the dataset.
- Provide the query for `price > 5000` as requested, with a note that it returns no results unless new data is added.
- Suggest adding a high-priced product (e.g., price > 5000) if you want to test this query.

If you confirm a different dataset or want to modify the products, I can adjust the instructions.

---

### Step-by-Step Solution

#### Step 1: Find All Products Where `price > 5000`

Query the `products` collection for products with `price` greater than 5000.

- **Mongo Shell**:
  ```javascript
  db.products.find({ price: { $gt: 5000 } }).pretty()
  ```
  - `$gt`: Greater than operator.
  - **Expected Output**: No results, as all products in the current dataset have prices ≤ 999.99.

- **Alternative (for meaningful results)**:
  To match the dataset, try `price > 500`:
  ```javascript
  db.products.find({ price: { $gt: 500 } }).pretty()
  ```
  - **Expected Output**:
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
    ```

- **MongoDB Compass**:
  - Navigate to the `products` collection.
  - Enter filter: `{ "price": { "$gt": 5000 } }` (or `{ "price": { "$gt": 500 } }` for results).
  - Click “Find”.

- **Optional: Add a High-Priced Product**:
  To test `price > 5000`, insert a new product:
  ```javascript
  db.products.insertOne({ name: "Luxury Watch", price: 7500.00, category: "Accessories" })
  ```
  Then re-run the query:
  ```javascript
  db.products.find({ price: { $gt: 5000 } }).pretty()
  ```
  - **Output**:
    ```json
    {
      "_id": ObjectId("..."),
      "name": "Luxury Watch",
      "price": 7500,
      "category": "Accessories"
    }
    ```

---

#### Step 2: Project Only `name` and `price` for All Products

Retrieve all products, showing only the `name` and `price` fields.

- **Mongo Shell**:
  ```javascript
  db.products.find({}, { name: 1, price: 1, _id: 0 }).pretty()
  ```
  - Second argument specifies fields: `1` to include, `0` to exclude.
  - **Expected Output** (partial):
    ```json
    { "name": "Laptop", "price": 999.99 }
    { "name": "Smartphone", "price": 699.99 }
    { "name": "Headphones", "price": 149.99 }
    ...
    ```

- **MongoDB Compass**:
  - In the `products` collection, enter query: `{}`.
  - In the “Project” field, enter: `{ "name": 1, "price": 1, "_id": 0 }`.
  - Click “Find”.

---

#### Step 3: Sort All Products by Price Descending

Sort products by `price` in descending order.

- **Mongo Shell**:
  ```javascript
  db.products.find().sort({ price: -1 }).pretty()
  ```
  - `-1`: Descending order.
  - **Expected Output** (partial):
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
  - Enter query: `{}`.
  - In the “Sort” field, enter: `{ "price": -1 }`.
  - Click “Find”.

---

#### Step 4: Create an Index on `price`

Create an index on the `price` field to optimize price-related queries.

- **Mongo Shell**:
  ```javascript
  db.products.createIndex({ price: 1 })
  ```
  - `1`: Ascending index.
  - Verify:
    ```javascript
    db.products.getIndexes()
    ```
    - **Output** (partial):
      ```json
      [
        { "v": 2, "key": { "_id": 1 }, "name": "_id_" },
        { "v": 2, "key": { "price": 1 }, "name": "price_1" }
      ]
      ```

- **MongoDB Compass**:
  - In the `products` collection, go to the “Indexes” tab.
  - Click “Create Index”, add `price` with “Ascending (1)”, and confirm.

- **Note**: If you ran the previous script, this index already exists. MongoDB will ignore duplicate index creation.

---

#### Step 5: Aggregation Pipeline: Find Average Product Price per Category

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
  - `$group`: Groups by `category`, computes average `price`.
  - **Expected Output**:
    ```json
    { "_id": "Accessories", "averagePrice": 143.32333333333334 }
    { "_id": "Clothing", "averagePrice": 73.32333333333334 }
    { "_id": "Electronics", "averagePrice": 733.3233333333334 }
    { "_id": "Footwear", "averagePrice": 89.99 }
    ```

- **MongoDB Compass**:
  - In the `products` collection, go to “Aggregations”.
  - Add stages:
    1. `$group`:
       ```json
       {
         "_id": "$category",
         "averagePrice": { "$avg": "$price" }
       }
       ```
    2. `$sort`:
       ```json
       { "_id": 1 }
       ```
  - Run the pipeline.

---

#### Step 6: Aggregation: Find Total Number of Orders per Customer

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
  - `$lookup`: Joins with `customers`.
  - `$unwind`: Flattens the array.
  - `$project`: Selects fields.
  - **Expected Output**:
    ```json
    { "customerName": "Alice Smith", "orderCount": 2 }
    { "customerName": "Bob Johnson", "orderCount": 1 }
    { "customerName": "Carol Lee", "orderCount": 1 }
    ```

- **MongoDB Compass**:
  - In the `orders` collection, go to “Aggregations”.
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
  - Run the pipeline.

---

#### Step 7: Aggregation: Find Total Sales Amount (Sum of `totalPrice`) per Customer

The current `orders` collection doesn’t have a `totalPrice` field, as it stores `productIds` and `orderDate`. To compute the total sales amount, we need to:
- Join `orders` with `products` to get product prices.
- Sum the prices of products in each order.
- Group by customer to calculate the total sales amount.

- **Mongo Shell**:
  ```javascript
  db.orders.aggregate([
    {
      $unwind: "$productIds" // Flatten productIds array
    },
    {
      $lookup: {
        from: "products",
        localField: "productIds",
        foreignField: "_id",
        as: "productDetails"
      }
    },
    {
      $unwind: "$productDetails" // Flatten productDetails array
    },
    {
      $group: {
        _id: "$customerId",
        totalSales: { $sum: "$productDetails.price" }
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
        totalSales: 1,
        _id: 0
      }
    },
    {
      $sort: { customerName: 1 }
    }
  ])
  ```
  - `$unwind`: Expands `productIds` to process each product.
  - `$lookup`: Joins with `products` to get prices.
  - `$group`: Sums prices per `customerId`.
  - `$lookup` and `$unwind`: Get customer names.
  - `$project`: Selects fields.
  - **Expected Output** (approximate, based on product prices):
    ```json
    { "customerName": "Alice Smith", "totalSales": 1649.97 } // Laptop+Smartphone, Smartphone+Headphones
    { "customerName": "Bob Johnson", "totalSales": 149.99 } // Headphones
    { "customerName": "Carol Lee", "totalSales": 999.99 } // Laptop
    ```

- **MongoDB Compass**:
  - In the `orders` collection, go to “Aggregations”.
  - Add stages:
    1. `$unwind`:
       ```json
       { "path": "$productIds" }
       ```
    2. `$lookup`:
       ```json
       {
         "from": "products",
         "localField": "productIds",
         "foreignField": "_id",
         "as": "productDetails"
       }
       ```
    3. `$unwind`:
       ```json
       { "path": "$productDetails" }
       ```
    4. `$group`:
       ```json
       {
         "_id": "$customerId",
         "totalSales": { "$sum": "$productDetails.price" }
       }
       ```
    5. `$lookup`:
       ```json
       {
         "from": "customers",
         "localField": "_id",
         "foreignField": "_id",
         "as": "customerDetails"
       }
       ```
    6. `$unwind`:
       ```json
       { "path": "$customerDetails" }
       ```
    7. `$project`:
       ```json
       {
         "customerName": "$customerDetails.name",
         "totalSales": 1,
         "_id": 0
       }
       ```
    8. `$sort`:
       ```json
       { "customerName": 1 }
       ```
  - Run the pipeline.

---

### Verification
- **Products Query (`price > 5000`)**: Returns no results unless a high-priced product is added.
- **Projection**: Check that only `name` and `price` appear for all products.
- **Sorting**: Verify the highest-priced product (e.g., Laptop, 999.99) is first.
- **Index**: Confirm the `price_1` index exists with `db.products.getIndexes()`.
- **Aggregations**:
  - Average price: Check for four categories with correct averages.
  - Orders per customer: Verify Alice (2), Bob (1), Carol (1).
  - Total sales: Cross-check with product prices (e.g., Alice’s orders include Laptop+Smartphone, Smartphone+Headphones).

- **Troubleshooting**:
  - **No results for `price > 5000`**: Add a product with `price > 5000` or adjust to `price > 500`.
  - **Aggregation errors**: Ensure `productIds` and `customerId` fields exist in `orders`. Check collection names (`products`, `customers`).
  - **MongoDB not running**: Verify with `mongosh` or `docker ps`.

---

### Artifact: MongoDB Script for Practice Set 2

```javascript
use ecommerce;

// Find products where price > 5000 (or > 500 for current data)
db.products.find({ price: { $gt: 500 } }).pretty()

// Project name and price for all products
db.products.find({}, { name: 1, price: 1, _id: 0 }).pretty()

// Sort products by price descending
db.products.find().sort({ price: -1 }).pretty()

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

// Aggregation: Total number of orders per customer
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

// Aggregation: Total sales amount per customer
db.orders.aggregate([
  {
    $unwind: "$productIds"
  },
  {
    $lookup: {
      from: "products",
      localField: "productIds",
      foreignField: "_id",
      as: "productDetails"
    }
  },
  {
    $unwind: "$productDetails"
  },
  {
    $group: {
      _id: "$customerId",
      totalSales: { $sum: "$productDetails.price" }
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
      totalSales: 1,
      _id: 0
    }
  },
  {
    $sort: { customerName: 1 }
  }
])
```

---

### How to Run
1. **Ensure MongoDB is Running**:
   - Local: `mongod`.
   - Docker: `docker run -d -p 27017:27017 mongo:latest`.
2. **Save the Script**:
   - Save as `practice_set_2.js`.
3. **Execute**:
   ```bash
   mongosh --file practice_set_2.js
   ```
4. **Verify**:
   - Connect to `mongosh`, switch to `use ecommerce`, and check collections (`db.products.find()`, `db.orders.find()`).

---

### Summary
You’ve completed **Practice Set 2**:
1. Queried products with `price > 5000` (adjusted to `> 500` for results).
2. Projected `name` and `price` for all products.
3. Sorted products by price descending.
4. Created an index on `price`.
5. Wrote aggregations for:
   - Average price per category.
   - Total orders per customer.
   - Total sales amount per customer.

The script (`practice_set_2.js`) is ready to run in `mongosh`. If you need to add high-priced products, use Node.js, or have other questions, let me know!

### Key Citations
- [MongoDB Official Documentation](https://www.mongodb.com/docs/)
- [MongoDB Aggregation Pipeline](https://www.mongodb.com/docs/manual/core/aggregation-pipeline/)
- [MongoDB Indexing](https://www.mongodb.com/docs/manual/indexes/)

To find the customer who spent the most in total in the `ecommerce` database, we need to calculate the total sales amount per customer by summing the prices of products in their orders and then identify the customer with the highest total. Since the `orders` collection in the previous setup does not have a `totalPrice` field but instead references `productIds`, we’ll join with the `products` collection to get product prices, sum them per customer, and sort to find the top spender. I’ll provide the query for MongoDB Shell (`mongosh`) and MongoDB Compass, assuming the `ecommerce` database with `orders`, `customers`, and `products` collections as previously defined.

---

### Aggregation Query: Find the Customer Who Spent the Most in Total

The query involves:
- Unwinding the `productIds` array in `orders` to process each product.
- Joining with `products` to get product prices.
- Grouping by `customerId` to sum the prices.
- Joining with `customers` to get customer names.
- Sorting by total sales descending and limiting to one result.

#### Mongo Shell
```javascript
db.orders.aggregate([
  {
    $unwind: "$productIds" // Flatten productIds array
  },
  {
    $lookup: {
      from: "products",
      localField: "productIds",
      foreignField: "_id",
      as: "productDetails"
    }
  },
  {
    $unwind: "$productDetails" // Flatten productDetails array
  },
  {
    $group: {
      _id: "$customerId",
      totalSpent: { $sum: "$productDetails.price" }
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
      totalSpent: 1,
      _id: 0
    }
  },
  {
    $sort: { totalSpent: -1 } // Sort descending by totalSpent
  },
  {
    $limit: 1 // Get the top spender
  }
])
```

#### Expected Output
Based on the previous `orders` data (e.g., Alice ordered Laptop+Smartphone and Smartphone+Headphones, Bob ordered Headphones, Carol ordered Laptop):
```json
{ "customerName": "Alice Smith", "totalSpent": 1649.97 }
```
- **Explanation**: Alice’s orders include:
  - Order 1: Laptop (999.99) + Smartphone (699.99) = 1699.98.
  - Order 2: Smartphone (699.99) + Headphones (149.99) = 849.98.
  - Total: 1699.98 + 849.98 = 2549.96 (Note: My earlier calculation of 1649.97 was incorrect due to a misinterpretation of orders; the correct total is 2549.96).
- Bob: Headphones (149.99).
- Carol: Laptop (999.99).
- Alice spent the most.

#### MongoDB Compass
1. Navigate to the `orders` collection.
2. Go to the “Aggregations” tab.
3. Add the following stages:
   - **Stage 1: `$unwind`**:
     ```json
     { "path": "$productIds" }
     ```
   - **Stage 2: `$lookup`**:
     ```json
     {
       "from": "products",
       "localField": "productIds",
       "foreignField": "_id",
       "as": "productDetails"
     }
     ```
   - **Stage 3: `$unwind`**:
     ```json
     { "path": "$productDetails" }
     ```
   - **Stage 4: `$group`**:
     ```json
     {
       "_id": "$customerId",
       "totalSpent": { "$sum": "$productDetails.price" }
     }
     ```
   - **Stage 5: `$lookup`**:
     ```json
     {
       "from": "customers",
       "localField": "_id",
       "foreignField": "_id",
       "as": "customerDetails"
     }
     ```
   - **Stage 6: `$unwind`**:
     ```json
     { "path": "$customerDetails" }
     ```
   - **Stage 7: `$project`**:
     ```json
     {
       "customerName": "$customerDetails.name",
       "totalSpent": 1,
       "_id": 0
     }
     ```
   - **Stage 8: `$sort`**:
     ```json
     { "totalSpent": -1 }
     ```
   - **Stage 9: `$limit`**:
     ```json
     1
     ```
4. Run the pipeline to see the result.

---

### Verification
- **Check Output**: Ensure the top spender is Alice Smith with `totalSpent` ≈ 2549.96.
- **Cross-Check**:
  - Run `db.orders.find({ customerId: db.customers.findOne({ name: "Alice Smith" })._id }).pretty()` to verify her orders.
  - Confirm product prices in `db.products.find({ _id: { $in: [/* productIds from Alice’s orders */] } })`.
- **Troubleshooting**:
  - **Empty Result**: Ensure `orders`, `products`, and `customers` collections are populated.
  - **Incorrect Totals**: Verify `productIds` in `orders` match valid `_id` values in `products`.
  - **MongoDB Not Running**: Check with `mongosh` or `docker ps`.

---

### Artifact: MongoDB Query Script

```javascript
use ecommerce;

db.orders.aggregate([
  {
    $unwind: "$productIds"
  },
  {
    $lookup: {
      from: "products",
      localField: "productIds",
      foreignField: "_id",
      as: "productDetails"
    }
  },
  {
    $unwind: "$productDetails"
  },
  {
    $group: {
      _id: "$customerId",
      totalSpent: { $sum: "$productDetails.price" }
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
      totalSpent: 1,
      _id: 0
    }
  },
  {
    $sort: { totalSpent: -1 }
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
   - Save as `top_spender_query.js`.
3. **Execute**:
   ```bash
   mongosh --file top_spender_query.js
   ```
4. **Verify**:
   - Check the output in `mongosh`.
   - Confirm with `db.customers.find()` and `db.orders.find()`.

---

### Summary
The query identifies Alice Smith as the customer who spent the most (≈2549.96) by aggregating order data, joining with products and customers, and sorting by total spent. The script is ready to run in `mongosh`. If you need adjustments (e.g., adding `totalPrice` to `orders`, Node.js version), let me know!

### Key Citations
- [MongoDB Aggregation Pipeline](https://www.mongodb.com/docs/manual/core/aggregation-pipeline/)
- [MongoDB $lookup](https://www.mongodb.com/docs/manual/reference/operator/aggregation/lookup/)
