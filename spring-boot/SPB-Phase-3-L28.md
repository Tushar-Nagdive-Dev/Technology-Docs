# 🚀 **Phase 3 - Lesson 28: Enterprise-Level Spring Boot Architecture & Best Practices**  

## **📌 Lesson Objective**  
By the end of this lesson, you will:  
✅ Learn **Enterprise-Level Spring Boot Architecture**  
✅ Implement **Best Practices for Scalable Microservices**  
✅ Use **Hexagonal & Clean Architecture for Maintainability**  
✅ Apply **Security, Logging, and Observability Standards**  
✅ Optimize **Performance, Resilience, and Cloud Deployment**  

---

# 🏗 **Part 1: Enterprise-Level Spring Boot Architecture**  

## **1️⃣ Why Enterprise-Level Architecture Matters?**  
📌 In **large-scale applications**, maintaining code structure, security, scalability, and observability is crucial.  

✅ **Best Architecture Principles:**  
✔ **Scalability** – Easily handles high traffic & requests.  
✔ **Modularity** – Clean separation of concerns.  
✔ **Observability** – Logs, traces, and metrics for debugging.  
✔ **Security** – Implements OAuth2, JWT, and RBAC.  
✔ **Performance** – Optimized queries, caching, and async processing.  

### **🔥 Common Enterprise Architectures**
1️⃣ **Layered Architecture (MVC)** – Best for small/medium projects.  
2️⃣ **Hexagonal Architecture (Ports & Adapters)** – Recommended for large-scale microservices.  
3️⃣ **Event-Driven Architecture (Kafka, RabbitMQ)** – Best for real-time systems.  
4️⃣ **Reactive Architecture (WebFlux, RxJava)** – Best for high-performance applications.  

---

# 🏛 **Part 2: Implementing Hexagonal Architecture in Spring Boot**  

## **2️⃣ What is Hexagonal Architecture?**  
📌 **Hexagonal (Ports & Adapters) Architecture** separates the core business logic from external dependencies (DB, APIs).  

### **🔥 Why Use Hexagonal Architecture?**
✔ **Decouples Business Logic from Infrastructure**  
✔ **Easier Testing & Maintainability**  
✔ **Supports Multiple Interfaces (Web, CLI, Message Queue)**  

### **🔥 Hexagonal Architecture Layers**
| **Layer**           | **Description** |
|---------------------|---------------|
| **Application (Core Business Logic)** | Handles domain rules & logic. |
| **Ports (Interfaces)** | Defines API contracts (e.g., Repositories, Services). |
| **Adapters (Implementation Details)** | Connects with DB, APIs, Queues. |

---

## **3️⃣ Implementing Hexagonal Architecture in Spring Boot**  

📌 **Step 1: Define Package Structure**
```plaintext
com.example.app
 ├── application (Core Business Logic)
 │   ├── service (Domain Services)
 │   ├── port (Interfaces)
 ├── adapters (Infrastructure)
 │   ├── persistence (Database Implementation)
 │   ├── web (REST API Implementation)
 │   ├── messaging (Kafka, RabbitMQ)
```

📌 **Step 2: Create Domain Model (`User.java`)**  
```java
package com.example.app.application.model;

public class User {
    private Long id;
    private String name;
    private String email;
    // Getters & Setters
}
```

📌 **Step 3: Define a Port (Interface) (`UserRepositoryPort.java`)**  
```java
package com.example.app.application.port;

import com.example.app.application.model.User;
import java.util.List;

public interface UserRepositoryPort {
    User save(User user);
    List<User> findAll();
}
```

📌 **Step 4: Implement Adapter for Database (`UserRepositoryAdapter.java`)**  
```java
package com.example.app.adapters.persistence;

import com.example.app.application.port.UserRepositoryPort;
import com.example.app.application.model.User;
import org.springframework.stereotype.Repository;

import java.util.ArrayList;
import java.util.List;

@Repository
public class UserRepositoryAdapter implements UserRepositoryPort {
    private final List<User> users = new ArrayList<>();

    @Override
    public User save(User user) {
        users.add(user);
        return user;
    }

    @Override
    public List<User> findAll() {
        return users;
    }
}
```

📌 **Step 5: Create Application Service (`UserService.java`)**  
```java
package com.example.app.application.service;

import com.example.app.application.port.UserRepositoryPort;
import com.example.app.application.model.User;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class UserService {
    private final UserRepositoryPort userRepository;

    public UserService(UserRepositoryPort userRepository) {
        this.userRepository = userRepository;
    }

    public User createUser(User user) {
        return userRepository.save(user);
    }

    public List<User> getUsers() {
        return userRepository.findAll();
    }
}
```

📌 **Step 6: Create REST Adapter (`UserController.java`)**  
```java
package com.example.app.adapters.web;

import com.example.app.application.service.UserService;
import com.example.app.application.model.User;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/users")
public class UserController {
    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }

    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.createUser(user);
    }

    @GetMapping
    public List<User> getUsers() {
        return userService.getUsers();
    }
}
```

📌 **Step 7: Test the API**  
```bash
curl -X POST "http://localhost:8080/users" -H "Content-Type: application/json" -d '{"name":"Alice", "email":"alice@example.com"}'
curl -X GET "http://localhost:8080/users"
```
✔ **Your app is now built using Hexagonal Architecture!** 🚀  

---

# 🔐 **Part 3: Security & Observability Best Practices**  

## **4️⃣ Implementing Security Best Practices**  
📌 **Use OAuth2, JWT, Keycloak for Authentication**  
```properties
spring.security.oauth2.client.registration.keycloak.client-id=spring-app
spring.security.oauth2.client.provider.keycloak.issuer-uri=http://localhost:8081/realms/master
```

📌 **Use RBAC (Role-Based Access Control)**  
```java
@PreAuthorize("hasRole('ADMIN')")
public void deleteUser(Long id) { ... }
```

📌 **Enable Secure Headers & CSRF Protection**  
```java
http.csrf().disable()
    .headers().frameOptions().disable();
```

---

## **5️⃣ Logging, Tracing & Metrics with OpenTelemetry**  
📌 **Enable OpenTelemetry Distributed Tracing**  
```properties
otel.traces.exporter=jaeger
otel.exporter.jaeger.endpoint=http://localhost:14250
```

📌 **Enable Centralized Logging with ELK (Elasticsearch, Logstash, Kibana)**  
```bash
docker-compose up -d elk
```
✔ **All logs are now indexed in Kibana!** 🚀  

---

# 🚀 **Part 4: Performance & Scalability Best Practices**  

## **6️⃣ Optimizing Performance**  
📌 **Use Caching (Redis, Ehcache)**  
```java
@Cacheable("users")
public List<User> getUsers() { ... }
```

📌 **Enable Connection Pooling for Database**  
```properties
spring.datasource.hikari.maximum-pool-size=10
```

📌 **Use Async Processing for Heavy Tasks**  
```java
@Async
public CompletableFuture<String> processData() { ... }
```

---

## **7️⃣ Scaling Microservices in the Cloud**  
📌 **Deploy on Kubernetes (AWS EKS, Google GKE)**  
```bash
kubectl apply -f deployment.yaml
```

📌 **Use API Gateway for Load Balancing**  
```yaml
spring.cloud.gateway.routes:
  - id: user-service
    uri: lb://user-service
    predicates: Path=/users/**
```

📌 **Enable Horizontal Auto-Scaling**  
```bash
kubectl autoscale deployment user-service --cpu-percent=50 --min=2 --max=5
```
✔ **Your microservices now scale automatically!** 🚀  

---

## 🎯 **Lesson 28 - Summary**  
✅ Implemented **Hexagonal Architecture in Spring Boot**  
✅ Applied **Security & Observability Best Practices**  
✅ Optimized **Performance & Caching**  
✅ Deployed **Microservices on Kubernetes with Auto-Scaling**  

---
