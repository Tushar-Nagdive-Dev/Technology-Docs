# 🚀 **Phase 1 - Lesson 7: Implementing Caching in Spring Boot** 🚀  

## **📌 Lesson Objective**
By the end of this lesson, you will:  
✅ Understand **what caching is** and why it's important  
✅ Learn how to **enable caching in Spring Boot**  
✅ Use **`@Cacheable`, `@CachePut`, and `@CacheEvict`**  
✅ Implement **in-memory caching using ConcurrentHashMap**  
✅ Integrate **Spring Boot with Redis for caching**  

---

## **1️⃣ What is Caching & Why is it Important?**
### 🔹 **Caching** is a technique that **stores frequently accessed data in memory** to improve performance.  

✔ **Faster API responses** 🚀  
✔ **Reduces database load** 📉  
✔ **Improves scalability** 🔥  

### 🔹 **Example Use Case**
Without caching:
1️⃣ **User requests data** → Fetch from **database** → **Slow response** ⏳  
2️⃣ **Next request for the same data** → **Database hit again** 😞  

With caching:
1️⃣ **User requests data** → Fetch from **cache** (fast) → **Quick response** ⚡  
2️⃣ **Next request for the same data** → Data **retrieved instantly** 😍  

---

## **2️⃣ Enabling Spring Boot Caching**
📌 **Step 1: Add Caching Dependency (Optional for Redis)**  
Spring Boot provides built-in caching with **Java’s ConcurrentHashMap** by default.  

📌 **Step 2: Enable Caching in the Main Application Class**  
Modify `DemoApplication.java`:

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cache.annotation.EnableCaching;

@SpringBootApplication
@EnableCaching  // Enables caching
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

✅ **Now caching is enabled!** 🚀  

---

## **3️⃣ Implementing In-Memory Caching Using `@Cacheable`**
📌 Modify `UserService` to cache data.  

```java
package com.example.demo.service;

import com.example.demo.model.User;
import com.example.demo.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cache.annotation.Cacheable;
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

    // CREATE User
    public User createUser(User user) {
        return userRepository.save(user);
    }

    // READ all users (With Caching)
    @Cacheable(value = "users")
    public List<User> getAllUsers() {
        System.out.println("Fetching from database...");
        return userRepository.findAll();
    }

    // READ User by ID (With Caching)
    @Cacheable(value = "users", key = "#id")
    public Optional<User> getUserById(Long id) {
        System.out.println("Fetching user from database...");
        return userRepository.findById(id);
    }
}
```

✅ **How It Works?**
- `@Cacheable(value = "users")` → **Caches results of method calls**.
- First request: **Fetch from database** 🐢  
- Subsequent requests: **Fetch from cache** ⚡  

### **Step 3: Test Caching**
1️⃣ **Run the app** → `mvn spring-boot:run`  
2️⃣ **Call API**:
   ```
   GET http://localhost:8080/api/users
   ```
3️⃣ **Check logs**:
   ```
   Fetching from database...
   ```
4️⃣ **Call API again** (without modifying data) → **No DB query!**  

🎉 **Caching is working!**

---

## **4️⃣ Using `@CachePut` to Update Cache**
📌 Modify `updateUser()` to **update the cache when a user is updated**.

```java
@CachePut(value = "users", key = "#id")
public User updateUser(Long id, User userDetails) {
    User user = userRepository.findById(id).orElseThrow(() -> new RuntimeException("User not found"));
    user.setName(userDetails.getName());
    user.setEmail(userDetails.getEmail());
    return userRepository.save(user);
}
```

✅ **Ensures cache updates when a user is modified.**  

---

## **5️⃣ Using `@CacheEvict` to Remove Data from Cache**
📌 Modify `deleteUser()` to **remove user from cache** after deletion.

```java
@CacheEvict(value = "users", key = "#id")
public void deleteUser(Long id) {
    userRepository.deleteById(id);
}
```

✅ **Ensures cache stays consistent with database.**  

---

## **6️⃣ Integrating Spring Boot with Redis for Caching**
🔹 **Why Redis?**
✔ In-memory **key-value store**  
✔ Faster than database queries  
✔ Used in **production applications**  

📌 **Step 1: Add Redis Dependency in `pom.xml`**
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

📌 **Step 2: Configure Redis in `application.properties`**
```properties
# Redis configuration
spring.cache.type=redis
spring.redis.host=localhost
spring.redis.port=6379
```

📌 **Step 3: Enable Redis Caching in `DemoApplication.java`**
```java
import org.springframework.cache.annotation.EnableCaching;
import org.springframework.context.annotation.Configuration;

@Configuration
@EnableCaching  // Enables Redis Caching
public class CacheConfig {
}
```

📌 **Step 4: Run Redis**
If Redis is not installed, use Docker:
```bash
docker run --name redis-cache -d -p 6379:6379 redis
```

✅ **Now caching is handled by Redis instead of memory!** 🚀  

---

## **7️⃣ Testing Redis Caching**
1️⃣ **Run the app**  
```bash
mvn spring-boot:run
```
2️⃣ **Call API multiple times**:
   ```
   GET http://localhost:8080/api/users
   ```
3️⃣ **Check Redis Cache**
```bash
redis-cli
keys *
```

🎉 **Redis caching is working!**

---

## 🎯 **Lesson 7 - Summary**
✅ **Enabled caching** in Spring Boot using `@EnableCaching`  
✅ Implemented caching using **`@Cacheable`, `@CachePut`, and `@CacheEvict`**  
✅ Used **ConcurrentHashMap (default caching)**  
✅ Integrated **Redis for caching**  

---
