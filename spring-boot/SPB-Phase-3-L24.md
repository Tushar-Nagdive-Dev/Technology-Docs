# 🚀 **Phase 3 - Lesson 24: Optimizing Spring Boot Performance (Profiling, Caching, Load Testing)**  

## **📌 Lesson Objective**  
By the end of this lesson, you will:  
✅ Understand **Spring Boot Performance Optimization Techniques**  
✅ Use **Profiling to analyze application performance**  
✅ Implement **Caching with Redis & In-Memory Cache**  
✅ Perform **Load Testing using JMeter**  
✅ Optimize **Database Performance (JPA, Hibernate Tuning)**  

---

# ⚡ **Part 1: Profiling & Analyzing Performance in Spring Boot**  

## **1️⃣ Why Profiling is Important?**  
📌 **Profiling** helps identify performance bottlenecks, such as **slow queries, memory leaks, and inefficient code execution**.  

### **🔥 Common Performance Issues in Spring Boot**
✔ **Slow Database Queries**  
✔ **High CPU or Memory Usage**  
✔ **Unoptimized API Calls**  
✔ **Blocking I/O Operations**  

✅ **Use Profiling when:**  
- You need to **identify and fix performance issues**.  
- You want to **optimize memory usage and CPU consumption**.  

---

## **2️⃣ Enabling Spring Boot Actuator for Performance Monitoring**  
📌 **Step 1: Add Actuator Dependency in `pom.xml`**  
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

📌 **Step 2: Enable Actuator Endpoints in `application.properties`**  
```properties
management.endpoints.web.exposure.include=health,info,metrics
management.endpoint.metrics.enabled=true
```

📌 **Step 3: View Performance Metrics**  
Run Spring Boot app and access:  
```bash
curl -X GET "http://localhost:8080/actuator/metrics"
```
✔ **Now you can monitor CPU, Memory, and HTTP request performance!** 🚀  

---

## **3️⃣ Using Java Profilers (VisualVM & YourKit)**  
📌 **Step 1: Install VisualVM**  
Download **[VisualVM](https://visualvm.github.io/)** and connect it to your running Spring Boot application.

📌 **Step 2: Run Your Application with Profiling**  
```bash
mvn spring-boot:run
```
📌 **Step 3: Attach VisualVM to the Process**  
✔ Now you can **monitor CPU, memory usage, and threads in real-time**! 🎯  

---

# 🔥 **Part 2: Improving Performance with Caching (Redis & In-Memory Cache)**  

## **4️⃣ Why Use Caching?**  
📌 **Caching** stores frequently accessed data in memory, **reducing database queries & improving API response times**.  

✅ **Use Caching when:**  
- You need **fast access to frequently used data**.  
- You want to **reduce database load & improve API speed**.  

---

## **5️⃣ Implementing In-Memory Caching with Spring Boot**  
📌 **Step 1: Enable Caching in Spring Boot**  
```java
package com.example.demo.config;

import org.springframework.cache.annotation.EnableCaching;
import org.springframework.context.annotation.Configuration;

@Configuration
@EnableCaching
public class CacheConfig {
}
```

📌 **Step 2: Add Caching to a Service Method**  
```java
package com.example.demo.service;

import org.springframework.cache.annotation.Cacheable;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    @Cacheable("users")
    public String getUserData(String userId) {
        // Simulate slow database call
        try { Thread.sleep(3000); } catch (InterruptedException e) { }
        return "User Data for " + userId;
    }
}
```

📌 **Step 3: Test the Caching**  
```bash
curl -X GET "http://localhost:8080/api/users/1"
```
✔ First call takes **3 seconds**, but subsequent calls return instantly! 🚀  

---

## **6️⃣ Implementing Redis Caching in Spring Boot**  
📌 **Step 1: Add Redis Dependency in `pom.xml`**  
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

📌 **Step 2: Configure Redis in `application.properties`**  
```properties
spring.redis.host=localhost
spring.redis.port=6379
```

📌 **Step 3: Enable Redis Caching in `CacheConfig.java`**  
```java
package com.example.demo.config;

import org.springframework.cache.CacheManager;
import org.springframework.cache.annotation.EnableCaching;
import org.springframework.cache.concurrent.ConcurrentMapCacheManager;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.cache.RedisCacheManager;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;

@Configuration
@EnableCaching
public class CacheConfig {

    @Bean
    public CacheManager cacheManager(RedisConnectionFactory redisConnectionFactory) {
        return RedisCacheManager.create(redisConnectionFactory);
    }

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory);
        return template;
    }
}
```

📌 **Step 4: Test Redis Caching**  
```bash
curl -X GET "http://localhost:8080/api/users/1"
```
✔ **Data is now cached in Redis for faster access!** 🔥  

---

# 🚀 **Part 3: Load Testing Spring Boot APIs with JMeter**  

## **7️⃣ What is Load Testing & Why is It Important?**  
📌 **Load Testing** simulates **multiple users accessing APIs** to measure performance under high traffic.  

✅ **Use Load Testing when:**  
- You need to **check API response times under heavy traffic**.  
- You want to **identify scalability limits & bottlenecks**.  

---

## **8️⃣ Performing Load Testing Using JMeter**  
📌 **Step 1: Download Apache JMeter**  
- Install **[Apache JMeter](https://jmeter.apache.org/download_jmeter.cgi)**  

📌 **Step 2: Create a Load Test Plan in JMeter**  
1️⃣ Open **JMeter**  
2️⃣ Create **Thread Group** (100 users, Ramp-up Time: 10s)  
3️⃣ Add **HTTP Request Sampler** for:  
   - **GET** `http://localhost:8080/api/users`  
4️⃣ Add **Listeners** → **View Results in Table**  
5️⃣ Start the Test & Analyze Performance  

✔ **Now you can measure API performance under load!** 🚀  

---

# ⚡ **Part 4: Optimizing Database Performance (JPA, Hibernate Tuning)**  

## **9️⃣ Optimizing Spring Data JPA for Faster Queries**  
📌 **Step 1: Enable Query Logging in `application.properties`**  
```properties
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```

📌 **Step 2: Use Pagination & Limit Query Results**  
```java
@Query("SELECT u FROM User u WHERE u.status = :status")
Page<User> findByStatus(@Param("status") String status, Pageable pageable);
```

📌 **Step 3: Use Indexing in Database**  
```sql
CREATE INDEX idx_user_status ON users(status);
```

📌 **Step 4: Optimize Hibernate Fetching**  
```java
@Entity
public class Order {
    @ManyToOne(fetch = FetchType.LAZY) // Use LAZY loading to improve performance
    private User user;
}
```
✔ **Queries are now optimized for better performance!** 🚀  

---

## 🎯 **Lesson 24 - Summary**  
✅ Used **Spring Boot Actuator & Profiling for performance analysis**  
✅ Implemented **Caching with Redis & In-Memory Cache**  
✅ Performed **Load Testing using JMeter**  
✅ Optimized **Database Performance with JPA & Hibernate Tuning**  

---
