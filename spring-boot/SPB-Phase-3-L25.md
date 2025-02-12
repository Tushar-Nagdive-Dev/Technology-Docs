# 🚀 **Phase 3 - Lesson 25: Implementing Circuit Breakers & Fault Tolerance in Spring Boot (Resilience4j & Hystrix)**  

## **📌 Lesson Objective**  
By the end of this lesson, you will:  
✅ Understand **what Circuit Breakers are and why they are needed**  
✅ Implement **Resilience4j for fault tolerance**  
✅ Use **Hystrix for handling service failures**  
✅ Apply **Retry & Rate Limiting for improved resilience**  
✅ Secure microservices from **cascading failures**  

---

# ⚡ **Part 1: What is a Circuit Breaker & Why Use It?**  

📌 **A Circuit Breaker** is a design pattern that **prevents system failures from cascading** by temporarily stopping requests to a failing service.  

### **🔥 Why Use Circuit Breakers?**
✔ **Prevents Overloading** – Stops failing services from consuming system resources.  
✔ **Faster Failures** – Avoids waiting for timeouts by returning fallback responses.  
✔ **Auto-Recovery** – Automatically retries when the service is back online.  

✅ **Use Circuit Breakers when:**  
- You have **microservices communicating via REST APIs**.  
- You want to **prevent failures from spreading across the system**.  

---

# 🔌 **Part 2: Implementing Resilience4j for Circuit Breaking**  

## **1️⃣ Adding Resilience4j to Spring Boot**  
📌 **Step 1: Add Dependencies in `pom.xml`**  
```xml
<dependency>
    <groupId>io.github.resilience4j</groupId>
    <artifactId>resilience4j-spring-boot2</artifactId>
    <version>1.7.1</version>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

📌 **Step 2: Enable Circuit Breaker in `application.properties`**  
```properties
resilience4j.circuitbreaker.instances.myService.failureRateThreshold=50
resilience4j.circuitbreaker.instances.myService.slowCallRateThreshold=60
resilience4j.circuitbreaker.instances.myService.waitDurationInOpenState=5000
```
✔ **Failure rate above 50% will trigger the circuit breaker**  

---

## **2️⃣ Using Circuit Breaker in a REST API**  
📌 **Step 3: Implement Circuit Breaker in `MyService.java`**  
```java
package com.example.demo.service;

import io.github.resilience4j.circuitbreaker.annotation.CircuitBreaker;
import org.springframework.stereotype.Service;
import org.springframework.web.client.RestTemplate;

@Service
public class MyService {

    private final RestTemplate restTemplate = new RestTemplate();

    @CircuitBreaker(name = "myService", fallbackMethod = "fallbackResponse")
    public String fetchData() {
        return restTemplate.getForObject("http://unstable-service/api/data", String.class);
    }

    public String fallbackResponse(Exception e) {
        return "Fallback response: Service is down!";
    }
}
```

📌 **Step 4: Create API Controller to Call the Service**  
```java
package com.example.demo.controller;

import com.example.demo.service.MyService;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api")
public class MyController {

    private final MyService myService;

    public MyController(MyService myService) {
        this.myService = myService;
    }

    @GetMapping("/fetch")
    public String fetchData() {
        return myService.fetchData();
    }
}
```

📌 **Step 5: Test Circuit Breaker**  
```bash
curl -X GET "http://localhost:8080/api/fetch"
```
✔ If the **unstable service is down**, you get:  
```
Fallback response: Service is down!
```

🎉 **Circuit Breaker is now protecting your service from failures!** 🔐  

---

# 🔄 **Part 3: Adding Retry & Rate Limiting with Resilience4j**  

## **3️⃣ Implementing Retry Mechanism**  
📌 **Step 1: Add Retry Configuration in `application.properties`**  
```properties
resilience4j.retry.instances.myService.maxAttempts=3
resilience4j.retry.instances.myService.waitDuration=2000
```

📌 **Step 2: Use Retry in `MyService.java`**  
```java
import io.github.resilience4j.retry.annotation.Retry;

@Service
public class MyService {

    @Retry(name = "myService", fallbackMethod = "fallbackResponse")
    public String fetchData() {
        return restTemplate.getForObject("http://unstable-service/api/data", String.class);
    }
}
```
✔ **Now, the service will retry 3 times before failing!**  

---

## **4️⃣ Implementing Rate Limiting**  
📌 **Step 1: Configure Rate Limiting in `application.properties`**  
```properties
resilience4j.ratelimiter.instances.myService.limitForPeriod=5
resilience4j.ratelimiter.instances.myService.limitRefreshPeriod=60s
```

📌 **Step 2: Use Rate Limiter in `MyService.java`**  
```java
import io.github.resilience4j.ratelimiter.annotation.RateLimiter;

@Service
public class MyService {

    @RateLimiter(name = "myService", fallbackMethod = "rateLimitFallback")
    public String fetchData() {
        return restTemplate.getForObject("http://unstable-service/api/data", String.class);
    }

    public String rateLimitFallback(Exception e) {
        return "Too many requests! Please try again later.";
    }
}
```
✔ **Now, the API allows only 5 requests per minute!**  

---

# ⚙ **Part 4: Implementing Hystrix for Circuit Breaking (Netflix OSS)**  

## **5️⃣ Why Use Hystrix?**  
📌 **Hystrix** is a fault tolerance library from Netflix used to **handle failures in microservices**.  

✅ **Use Hystrix when:**  
- You want **legacy circuit breaking support** in Spring Boot.  
- You need **fallback mechanisms for unstable APIs**.  

---

## **6️⃣ Adding Hystrix to Spring Boot**  
📌 **Step 1: Add Hystrix Dependency in `pom.xml`**  
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
</dependency>
```

📌 **Step 2: Enable Hystrix in `MyService.java`**  
```java
package com.example.demo.service;

import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import org.springframework.stereotype.Service;
import org.springframework.web.client.RestTemplate;

@Service
public class MyService {

    private final RestTemplate restTemplate = new RestTemplate();

    @HystrixCommand(fallbackMethod = "fallbackResponse")
    public String fetchData() {
        return restTemplate.getForObject("http://unstable-service/api/data", String.class);
    }

    public String fallbackResponse() {
        return "Fallback: Service is currently unavailable.";
    }
}
```

📌 **Step 3: Enable Hystrix Dashboard for Monitoring**  
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
</dependency>
```

📌 **Step 4: Access Hystrix Dashboard**  
```bash
http://localhost:8080/hystrix
```
✔ **Now you can monitor and manage service failures!** 🚀  

---

# 🎯 **Lesson 25 - Summary**  
✅ Implemented **Circuit Breakers with Resilience4j**  
✅ Used **Retry & Rate Limiting to improve API resilience**  
✅ Integrated **Hystrix for Netflix-style fault tolerance**  
✅ Secured microservices from **cascading failures**  

---
