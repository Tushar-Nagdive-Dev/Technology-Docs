# 🚀 **Phase 3 - Lesson 23: Implementing API Rate Limiting & Throttling in Spring Boot**  

## **📌 Lesson Objective**  
By the end of this lesson, you will:  
✅ Understand **Rate Limiting & Throttling** and why they are essential  
✅ Implement **Basic Rate Limiting with Spring Boot**  
✅ Use **Bucket4j for Token Bucket Rate Limiting**  
✅ Apply **Spring Cloud Gateway Rate Limiting for Microservices**  
✅ Secure APIs against **DDoS & abuse with IP-based throttling**  

---

# 📌 **1️⃣ What is Rate Limiting & Why is It Important?**  
📌 **Rate Limiting** controls the number of requests a user, IP, or system can make within a specific time frame.  

### **🔥 Why Use Rate Limiting?**
✔ **Prevents API Abuse** – Stops excessive requests from a single user or bot.  
✔ **Enhances Security** – Protects against **DDoS attacks** and brute force login attempts.  
✔ **Optimizes Performance** – Ensures fair resource allocation across users.  

✅ **Use Rate Limiting when:**  
- You offer **public APIs with a free tier**.  
- You need to **prevent excessive API calls from single users**.  
- You want to **limit API access per user/IP**.  

---

# 🛡 **Part 1: Basic Rate Limiting in Spring Boot**  

## **2️⃣ Implementing Simple Rate Limiting with Interceptors**  
📌 **Step 1: Create `RateLimitInterceptor.java`**  
```java
package com.example.demo.security;

import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;

import java.util.concurrent.ConcurrentHashMap;
import java.util.concurrent.atomic.AtomicInteger;

@Component
public class RateLimitInterceptor implements HandlerInterceptor {

    private final ConcurrentHashMap<String, AtomicInteger> requestCounts = new ConcurrentHashMap<>();
    private final int MAX_REQUESTS_PER_MINUTE = 5; // Limit to 5 requests per minute

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        String ipAddress = request.getRemoteAddr();
        requestCounts.putIfAbsent(ipAddress, new AtomicInteger(0));

        if (requestCounts.get(ipAddress).incrementAndGet() > MAX_REQUESTS_PER_MINUTE) {
            response.setStatus(HttpServletResponse.SC_TOO_MANY_REQUESTS);
            return false;
        }

        return true;
    }
}
```

📌 **Step 2: Register Interceptor in `WebConfig.java`**  
```java
package com.example.demo.config;

import com.example.demo.security.RateLimitInterceptor;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {

    private final RateLimitInterceptor rateLimitInterceptor;

    public WebConfig(RateLimitInterceptor rateLimitInterceptor) {
        this.rateLimitInterceptor = rateLimitInterceptor;
    }

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(rateLimitInterceptor);
    }
}
```

📌 **Step 3: Test Rate Limiting API**  
```bash
curl -X GET "http://localhost:8080/api/resource"
```
✔ If requests exceed **5 per minute**, you get `429 Too Many Requests`.  

🎉 **Basic rate limiting is now implemented!** 🔐  

---

# 🛠 **Part 2: Advanced Rate Limiting Using Bucket4j**  

## **3️⃣ Using Bucket4j for Token Bucket Rate Limiting**  
📌 **Step 1: Add Bucket4j Dependency in `pom.xml`**  
```xml
<dependency>
    <groupId>com.github.vladimir-bukhtoyarov</groupId>
    <artifactId>bucket4j-core</artifactId>
    <version>7.6.0</version>
</dependency>
```

📌 **Step 2: Implement Rate Limiting with Bucket4j (`RateLimitService.java`)**  
```java
package com.example.demo.service;

import io.github.bucket4j.*;
import org.springframework.stereotype.Service;

import java.time.Duration;
import java.util.Map;
import java.util.concurrent.ConcurrentHashMap;

@Service
public class RateLimitService {

    private final Map<String, Bucket> cache = new ConcurrentHashMap<>();

    public Bucket resolveBucket(String ip) {
        return cache.computeIfAbsent(ip, k -> createNewBucket());
    }

    private Bucket createNewBucket() {
        return Bucket4j.builder()
                .addLimit(Bandwidth.classic(10, Refill.greedy(10, Duration.ofMinutes(1)))) // 10 requests per minute
                .build();
    }

    public boolean tryConsume(String ip) {
        return resolveBucket(ip).tryConsume(1);
    }
}
```

📌 **Step 3: Implement Rate-Limited Controller (`RateLimitController.java`)**  
```java
package com.example.demo.controller;

import com.example.demo.service.RateLimitService;
import jakarta.servlet.http.HttpServletRequest;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api/ratelimited")
public class RateLimitController {

    private final RateLimitService rateLimitService;

    public RateLimitController(RateLimitService rateLimitService) {
        this.rateLimitService = rateLimitService;
    }

    @GetMapping
    public String getLimitedResource(HttpServletRequest request) {
        String ip = request.getRemoteAddr();

        if (!rateLimitService.tryConsume(ip)) {
            return "429 Too Many Requests - Rate limit exceeded!";
        }

        return "Success - Request allowed!";
    }
}
```

📌 **Step 4: Test Advanced Rate Limiting**  
```bash
curl -X GET "http://localhost:8080/api/ratelimited"
```
✔ **10 requests allowed per minute**, after that `429 Too Many Requests` appears.  

🎉 **Advanced Rate Limiting with Bucket4j is now implemented!** 🔐  

---

# 🌐 **Part 3: Rate Limiting in Microservices using Spring Cloud Gateway**  

## **4️⃣ Implementing API Rate Limiting with Spring Cloud Gateway**  
📌 **Step 1: Add Spring Cloud Gateway Dependency**  
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
```

📌 **Step 2: Configure Rate Limiting in `application.yml`**  
```yaml
spring:
  cloud:
    gateway:
      routes:
        - id: user-service
          uri: lb://user-service
          predicates:
            - Path=/users/**
          filters:
            - name: RequestRateLimiter
              args:
                redis-rate-limiter.replenishRate: 5  # 5 requests per second
                redis-rate-limiter.burstCapacity: 10
```

📌 **Step 3: Enable Redis for Distributed Rate Limiting**  
```bash
docker run -d --name redis -p 6379:6379 redis
```

📌 **Step 4: Test Rate Limiting at API Gateway**  
```bash
curl -X GET "http://localhost:8080/users"
```
✔ **Only 5 requests per second allowed!**  

🎉 **Spring Cloud Gateway now applies rate limiting for microservices!** 🚀  

---

# 🔒 **Part 4: IP-Based Throttling for Security**  

## **5️⃣ Blocking Requests from Specific IPs**  
📌 **Step 1: Add IP Blocking Logic in `SecurityFilter.java`**  
```java
package com.example.demo.security;

import jakarta.servlet.*;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.stereotype.Component;

import java.io.IOException;
import java.util.Set;

@Component
public class SecurityFilter implements Filter {

    private final Set<String> blockedIps = Set.of("192.168.1.100");

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {
        HttpServletRequest httpRequest = (HttpServletRequest) request;
        String ip = httpRequest.getRemoteAddr();

        if (blockedIps.contains(ip)) {
            ((HttpServletResponse) response).setStatus(HttpServletResponse.SC_FORBIDDEN);
            return;
        }

        chain.doFilter(request, response);
    }
}
```

🎉 **Now malicious IPs are blocked from accessing APIs!** 🔐  

---

## 🎯 **Lesson 23 - Summary**  
✅ Implemented **basic rate limiting using Spring Interceptors**  
✅ Used **Bucket4j for token bucket rate limiting**  
✅ Applied **Spring Cloud Gateway rate limiting for microservices**  
✅ Secured APIs with **IP-based blocking**  

---
