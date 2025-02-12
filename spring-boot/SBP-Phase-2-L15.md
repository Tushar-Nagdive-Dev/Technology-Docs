# 🚀 **Phase 2 - Lesson 15: Building Microservices with Spring Boot (Eureka, Feign, API Gateway)**  

## **📌 Lesson Objective**  
By the end of this lesson, you will:  
✅ Understand **what microservices are and why they are used**  
✅ Set up **Eureka Service Discovery** for microservices communication  
✅ Implement **API Gateway for centralized routing**  
✅ Use **Feign Client for service-to-service communication**  
✅ Learn **how microservices scale and work in distributed systems**  

---

## **1️⃣ What are Microservices & Why Use Them?**  
📌 **Microservices Architecture** is an approach where applications are built as **a collection of small, independent services** instead of a single monolithic system.  

### **🔥 Monolith vs. Microservices**
| Feature | Monolithic Architecture | Microservices Architecture |
|---------|-------------------------|----------------------------|
| **Scalability** | Hard to scale | Scales independently |
| **Deployment** | Full app redeployment required | Each service is deployed separately |
| **Communication** | Internal function calls | REST APIs, gRPC, or Messaging |
| **Tech Stack** | Single technology stack | Different stacks per service |
| **Fault Tolerance** | Failure can crash the entire app | Failure is isolated to a service |

✅ **Use Microservices when:**  
✔ Building **scalable** systems (e.g., Netflix, Amazon, Uber)  
✔ Different teams manage **different services**  
✔ Need **independent deployment**  

---

## **2️⃣ Setting Up Eureka Server (Service Discovery)**  
📌 **Step 1: Create a New Spring Boot Project for Eureka Server**  
1️⃣ **Go to [Spring Initializr](https://start.spring.io/)**  
2️⃣ Select:
   - **Project:** Maven  
   - **Spring Boot Version:** Latest  
   - **Dependencies:** ✅ Eureka Server  
3️⃣ **Generate the Project & Extract the ZIP**  

📌 **Step 2: Enable Eureka Server in `EurekaServerApplication.java`**  
```java
package com.example.eurekaserver;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
@EnableEurekaServer // Enables Eureka Server
public class EurekaServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class, args);
    }
}
```

📌 **Step 3: Configure `application.yml` for Eureka Server**  
```yaml
server:
  port: 8761

eureka:
  instance:
    hostname: localhost
  client:
    register-with-eureka: false
    fetch-registry: false
```

📌 **Step 4: Run the Eureka Server**
```bash
mvn spring-boot:run
```
📌 **Step 5: Open Eureka Dashboard**  
Go to **`http://localhost:8761/`** 🎉  

✅ **Eureka Server is now running!**  

---

## **3️⃣ Registering Microservices with Eureka**  
📌 **Step 1: Create a New Microservice (User Service)**  
1️⃣ **Go to [Spring Initializr](https://start.spring.io/)**  
2️⃣ Select:
   - **Dependencies:** ✅ Eureka Client, ✅ Spring Web, ✅ Spring Boot Actuator  

📌 **Step 2: Enable Eureka Client in `UserServiceApplication.java`**  
```java
package com.example.userservice;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient // Enables service registration
public class UserServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}
```

📌 **Step 3: Configure `application.yml` for Eureka Client**  
```yaml
server:
  port: 8081

spring:
  application:
    name: user-service

eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/
```

📌 **Step 4: Run the User Service**
```bash
mvn spring-boot:run
```
📌 **Step 5: Check Eureka Dashboard**  
Go to **`http://localhost:8761/`** and see **`user-service` registered!** 🎉  

✅ **User Service is now registered with Eureka!**  

---

## **4️⃣ Implementing API Gateway for Centralized Routing**  
📌 **Step 1: Create a New Spring Boot Project for API Gateway**  
1️⃣ **Go to [Spring Initializr](https://start.spring.io/)**  
2️⃣ Select:
   - **Dependencies:** ✅ Eureka Client, ✅ API Gateway  

📌 **Step 2: Enable API Gateway in `ApiGatewayApplication.java`**  
```java
package com.example.apigateway;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.gateway.route.RouteLocator;
import org.springframework.cloud.gateway.route.builder.RouteLocatorBuilder;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
@EnableDiscoveryClient
public class ApiGatewayApplication {
    public static void main(String[] args) {
        SpringApplication.run(ApiGatewayApplication.class, args);
    }

    @Bean
    public RouteLocator routes(RouteLocatorBuilder builder) {
        return builder.routes()
                .route("user-service", r -> r.path("/users/**")
                        .uri("lb://user-service"))
                .build();
    }
}
```

📌 **Step 3: Configure `application.yml` for API Gateway**  
```yaml
server:
  port: 8080

spring:
  application:
    name: api-gateway

eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/
```

📌 **Step 4: Run API Gateway**
```bash
mvn spring-boot:run
```
📌 **Step 5: Test Gateway Routing**  
```bash
curl -X GET "http://localhost:8080/users/"
```
🎉 **Now all requests go through API Gateway!**  

✅ **API Gateway is now handling all service requests!**  

---

## **5️⃣ Using Feign Client for Inter-Service Communication**  
📌 **Step 1: Add Feign Dependency in `UserService`**  
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

📌 **Step 2: Enable Feign Client in `UserServiceApplication.java`**  
```java
@SpringBootApplication
@EnableFeignClients // Enables Feign for service-to-service calls
public class UserServiceApplication { ... }
```

📌 **Step 3: Create `OrderClient.java` to Call Order Service**  
```java
@FeignClient(name = "order-service") // Call order-service
public interface OrderClient {

    @GetMapping("/orders/{userId}")
    List<Order> getUserOrders(@PathVariable Long userId);
}
```

📌 **Step 4: Inject Feign Client in User Service**  
```java
@Service
public class UserService {
    
    private final OrderClient orderClient;

    @Autowired
    public UserService(OrderClient orderClient) {
        this.orderClient = orderClient;
    }

    public List<Order> getOrdersForUser(Long userId) {
        return orderClient.getUserOrders(userId);
    }
}
```

✅ **Now User Service can fetch orders from Order Service via Feign Client!**  

---

## 🎯 **Lesson 15 - Summary**  
✅ Set up **Eureka Server for service discovery**  
✅ Registered **User Service as a microservice**  
✅ Implemented **API Gateway for centralized routing**  
✅ Used **Feign Client for service-to-service calls**  

---
