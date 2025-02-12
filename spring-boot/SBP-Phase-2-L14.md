# 🚀 **Phase 2 - Lesson 14: Spring Boot + GraphQL (Efficient API Queries & Fetching)**  

## **📌 Lesson Objective**  
By the end of this lesson, you will:  
✅ Understand what **GraphQL** is and why it's better than REST in some cases  
✅ Learn how to **integrate GraphQL with Spring Boot**  
✅ Implement **GraphQL queries & mutations**  
✅ Optimize data fetching with **GraphQL resolvers**  
✅ Test GraphQL APIs using **GraphiQL (GraphQL UI)**  

---

## **1️⃣ What is GraphQL & Why Use It?**  

📌 **GraphQL** is a query language for APIs that allows **clients to request exactly the data they need**.  

### **🔥 GraphQL vs REST API**
| Feature | REST API | GraphQL |
|---------|---------|---------|
| **Data Fetching** | Fetches **fixed data** | Fetches **only required fields** |
| **Multiple Resources** | Requires multiple API calls | Fetches **everything in one request** |
| **Over-fetching** | Returns unnecessary fields | Returns **only needed data** |
| **Under-fetching** | Requires extra API calls | Fetches **nested data in one request** |

✅ **Use GraphQL when:**  
✔ **Client needs flexibility** (e.g., Mobile Apps, UI-heavy apps)  
✔ **Reducing multiple API calls** (e.g., fetching user + orders in one request)  

---

## **2️⃣ Setting Up GraphQL in Spring Boot**  

📌 **Step 1: Add GraphQL Dependencies in `pom.xml`**  
```xml
<dependency>
    <groupId>com.graphql-java-kickstart</groupId>
    <artifactId>graphql-spring-boot-starter</artifactId>
    <version>15.0.0</version>
</dependency>

<dependency>
    <groupId>com.graphql-java-kickstart</groupId>
    <artifactId>graphiql-spring-boot-starter</artifactId>
    <version>11.1.0</version>
</dependency>
```

📌 **Step 2: Enable GraphQL in `application.properties`**  
```properties
graphql.servlet.enabled=true
graphql.servlet.corsEnabled=true
graphql.playground.enabled=true
```

📌 **Step 3: Define a `User` Model in `com.example.demo.model`**  
```java
package com.example.demo.model;

import jakarta.persistence.*;

@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String email;

    public User() {}

    public User(String name, String email) {
        this.name = name;
        this.email = email;
    }

    public Long getId() { return id; }
    public String getName() { return name; }
    public String getEmail() { return email; }
}
```

📌 **Step 4: Create `UserRepository.java`**  
```java
package com.example.demo.repository;

import com.example.demo.model.User;
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {
}
```

✅ **Now we have a User entity and repository ready for GraphQL!**  

---

## **3️⃣ Defining a GraphQL Schema**  
📌 **Step 5: Create `schema.graphqls` in `src/main/resources/graphql`**  

```graphql
type User {
    id: ID
    name: String
    email: String
}

type Query {
    getUsers: [User]
    getUserById(id: ID!): User
}

type Mutation {
    createUser(name: String!, email: String!): User
    updateUser(id: ID!, name: String, email: String): User
    deleteUser(id: ID!): String
}
```

✅ **What’s happening?**
- `type User` → Defines **User fields**.
- `type Query` → Defines **fetch operations**.
- `type Mutation` → Defines **modification operations**.

---

## **4️⃣ Implementing GraphQL Queries & Mutations**  

📌 **Step 6: Create `UserGraphQL.java` (GraphQL Resolver)**  
```java
package com.example.demo.graphql;

import com.example.demo.model.User;
import com.example.demo.repository.UserRepository;
import graphql.kickstart.tools.GraphQLMutationResolver;
import graphql.kickstart.tools.GraphQLQueryResolver;
import org.springframework.stereotype.Component;

import java.util.List;
import java.util.Optional;

@Component
public class UserGraphQL implements GraphQLQueryResolver, GraphQLMutationResolver {

    private final UserRepository userRepository;

    public UserGraphQL(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    // Query: Fetch all users
    public List<User> getUsers() {
        return userRepository.findAll();
    }

    // Query: Fetch user by ID
    public Optional<User> getUserById(Long id) {
        return userRepository.findById(id);
    }

    // Mutation: Create a new user
    public User createUser(String name, String email) {
        User user = new User(name, email);
        return userRepository.save(user);
    }

    // Mutation: Update user
    public User updateUser(Long id, String name, String email) {
        User user = userRepository.findById(id).orElseThrow(() -> new RuntimeException("User not found"));
        if (name != null) user.setName(name);
        if (email != null) user.setEmail(email);
        return userRepository.save(user);
    }

    // Mutation: Delete user
    public String deleteUser(Long id) {
        userRepository.deleteById(id);
        return "User deleted successfully!";
    }
}
```

✅ **Now GraphQL queries and mutations are implemented!** 🚀  

---

## **5️⃣ Testing GraphQL API using GraphiQL**  

📌 **Step 7: Start the Spring Boot Application**  
```bash
mvn spring-boot:run
```

📌 **Step 8: Open GraphiQL UI**  
Go to **`http://localhost:8080/graphiql`** (GraphQL Playground).  

---

### 🔍 **Query: Fetch All Users**  
```graphql
{
  getUsers {
    id
    name
    email
  }
}
```
✔ **Response:**
```json
{
  "data": {
    "getUsers": [
      { "id": 1, "name": "Alice", "email": "alice@example.com" },
      { "id": 2, "name": "Bob", "email": "bob@example.com" }
    ]
  }
}
```

---

### 🔍 **Query: Fetch User by ID**  
```graphql
{
  getUserById(id: 1) {
    name
    email
  }
}
```
✔ **Response:**  
```json
{
  "data": {
    "getUserById": {
      "name": "Alice",
      "email": "alice@example.com"
    }
  }
}
```

---

### 🔍 **Mutation: Create a New User**  
```graphql
mutation {
  createUser(name: "Charlie", email: "charlie@example.com") {
    id
    name
    email
  }
}
```
✔ **Response:**  
```json
{
  "data": {
    "createUser": {
      "id": 3,
      "name": "Charlie",
      "email": "charlie@example.com"
    }
  }
}
```

---

### 🔍 **Mutation: Update User**  
```graphql
mutation {
  updateUser(id: 3, name: "Charlie Brown", email: "charlieb@example.com") {
    id
    name
    email
  }
}
```

---

### 🔍 **Mutation: Delete User**  
```graphql
mutation {
  deleteUser(id: 3)
}
```

🎉 **GraphQL API is now fully functional!** 🚀  

---

## 🎯 **Lesson 14 - Summary**  
✅ Integrated **GraphQL in Spring Boot**  
✅ Created **GraphQL schema for Users**  
✅ Implemented **Queries & Mutations**  
✅ Tested API with **GraphiQL Playground**  

---
