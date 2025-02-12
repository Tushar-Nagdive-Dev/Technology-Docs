# 🚀 **Phase 1 - Lesson 4: Building a Complete REST API with CRUD Operations** 🚀  

## **📌 Lesson Objective**
By the end of this lesson, you will:  
✅ Understand **what CRUD operations are** and why they are important  
✅ Learn how to **build a REST API using Spring Boot**  
✅ Implement **CRUD operations (Create, Read, Update, Delete)**  
✅ Use **Spring Data JPA with an H2 Database (In-memory DB)**  
✅ Test your API using **Postman** or a Web Browser  

---

## **1️⃣ What is CRUD?**
**CRUD** stands for:  
🔹 **C** - Create (Add a new resource)  
🔹 **R** - Read (Retrieve a resource)  
🔹 **U** - Update (Modify an existing resource)  
🔹 **D** - Delete (Remove a resource)  

### **🔥 Real-World Example**
Imagine a **User Management System** where you can:
✔ Add new users (**Create**)  
✔ Get user details (**Read**)  
✔ Update user information (**Update**)  
✔ Delete a user (**Delete**)  

📌 **We'll implement a CRUD API for managing users using Spring Boot.**  

---

## **2️⃣ Setting Up the Spring Boot Project**
We'll create a **Spring Boot application** with the following dependencies:  
✔ **Spring Web** → To build REST APIs  
✔ **Spring Data JPA** → To interact with the database  
✔ **H2 Database** → An in-memory database for testing  

### **Step 1: Create a New Spring Boot Project**
1️⃣ Open **[Spring Initializr](https://start.spring.io/)**  
2️⃣ Select:
   - **Project**: Maven
   - **Spring Boot Version**: Latest stable
   - **Dependencies**:
     - ✅ Spring Web
     - ✅ Spring Data JPA
     - ✅ H2 Database
3️⃣ Click **Generate Project**, download and extract the ZIP file.  

---

## **3️⃣ Setting Up the User Entity (Model Layer)**
💡 **Entities represent database tables in Java.**  

📌 Create a new package **`com.example.demo.model`** and add a `User` entity:

```java
package com.example.demo.model;

import jakarta.persistence.*;

@Entity  // Marks this class as a database entity
@Table(name = "users")  // Maps to "users" table in DB
public class User {

    @Id  // Primary Key
    @GeneratedValue(strategy = GenerationType.IDENTITY)  // Auto-incremented ID
    private Long id;

    private String name;
    private String email;

    public User() {}  // Default constructor required by JPA

    public User(String name, String email) {
        this.name = name;
        this.email = email;
    }

    // Getters and Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}
```

✅ **What’s happening?**
- `@Entity` → Marks the class as a **database entity**  
- `@Table(name = "users")` → Maps to a **database table named `users`**  
- `@Id` → Defines the **primary key**  
- `@GeneratedValue(strategy = GenerationType.IDENTITY)` → Enables **auto-increment for ID**  

---

## **4️⃣ Creating the Repository Layer (Data Access)**
📌 Create a new package **`com.example.demo.repository`** and add the `UserRepository` interface:

```java
package com.example.demo.repository;

import com.example.demo.model.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
}
```

✅ **What’s happening?**
- `JpaRepository<User, Long>` → Provides **CRUD methods** for `User`  
- `@Repository` → Marks this interface as a **database repository**  

---

## **5️⃣ Creating the Service Layer (Business Logic)**
📌 Create a new package **`com.example.demo.service`** and add `UserService`:

```java
package com.example.demo.service;

import com.example.demo.model.User;
import com.example.demo.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Optional;

@Service
public class UserService {

    private final UserRepository userRepository;

    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    // CREATE a new user
    public User createUser(User user) {
        return userRepository.save(user);
    }

    // READ all users
    public List<User> getAllUsers() {
        return userRepository.findAll();
    }

    // READ a single user by ID
    public Optional<User> getUserById(Long id) {
        return userRepository.findById(id);
    }

    // UPDATE user details
    public User updateUser(Long id, User userDetails) {
        User user = userRepository.findById(id).orElseThrow(() -> new RuntimeException("User not found"));
        user.setName(userDetails.getName());
        user.setEmail(userDetails.getEmail());
        return userRepository.save(user);
    }

    // DELETE a user
    public void deleteUser(Long id) {
        userRepository.deleteById(id);
    }
}
```

✅ **What’s happening?**
- **`createUser()`** → Saves a new user to the database  
- **`getAllUsers()`** → Retrieves all users  
- **`getUserById()`** → Retrieves a specific user  
- **`updateUser()`** → Modifies user data  
- **`deleteUser()`** → Removes a user from the database  

---

## **6️⃣ Creating the Controller Layer (REST API)**
📌 Create a new package **`com.example.demo.controller`** and add `UserController`:

```java
package com.example.demo.controller;

import com.example.demo.model.User;
import com.example.demo.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;
import java.util.Optional;

@RestController
@RequestMapping("/api/users")
public class UserController {

    private final UserService userService;

    @Autowired
    public UserController(UserService userService) {
        this.userService = userService;
    }

    // CREATE a new user
    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.createUser(user);
    }

    // READ all users
    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }

    // READ user by ID
    @GetMapping("/{id}")
    public Optional<User> getUserById(@PathVariable Long id) {
        return userService.getUserById(id);
    }

    // UPDATE user details
    @PutMapping("/{id}")
    public User updateUser(@PathVariable Long id, @RequestBody User userDetails) {
        return userService.updateUser(id, userDetails);
    }

    // DELETE user
    @DeleteMapping("/{id}")
    public void deleteUser(@PathVariable Long id) {
        userService.deleteUser(id);
    }
}
```

✅ **What’s happening?**
| HTTP Method | Endpoint | Action |
|-------------|----------|--------|
| `POST` | `/api/users` | Create a new user |
| `GET` | `/api/users` | Retrieve all users |
| `GET` | `/api/users/{id}` | Retrieve a single user |
| `PUT` | `/api/users/{id}` | Update user details |
| `DELETE` | `/api/users/{id}` | Delete a user |

---

## **7️⃣ Running & Testing the API**
### **Step 1: Configure the H2 Database**
📌 **Add the following to `application.properties`:**
```properties
# Set H2 Database as the data source
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.h2.console.enabled=true
```

### **Step 2: Run the application**
```bash
mvn spring-boot:run
```

### **Step 3: Test the API using Postman**
- `POST` → **Add a user**
- `GET` → **Retrieve users**
- `PUT` → **Update user**
- `DELETE` → **Delete user**

---

## 🎯 **Lesson 4 - Summary**
✅ Built a **complete CRUD API** using Spring Boot.  
✅ Used **Spring Data JPA** with an **H2 in-memory database**.  
✅ Implemented **Service, Repository, and Controller layers**.  

---
