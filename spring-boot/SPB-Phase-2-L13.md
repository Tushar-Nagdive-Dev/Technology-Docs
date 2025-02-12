# 🚀 **Phase 2 - Lesson 13: Calling External APIs from Spring Boot (RESTTemplate & WebClient)**  

## **📌 Lesson Objective**  
By the end of this lesson, you will:  
✅ Understand how to **call external REST APIs** from Spring Boot  
✅ Learn **RESTTemplate** (Synchronous API Calls)  
✅ Use **WebClient** (Asynchronous & Reactive API Calls)  
✅ Handle **GET, POST, PUT, DELETE requests**  
✅ Implement **error handling & timeout settings**  

---

## **1️⃣ Why Call External APIs from Spring Boot?**  

📌 **Real-World Use Cases:**  
✔ **Fetching live currency exchange rates** 💱  
✔ **Getting weather updates from an API** 🌦  
✔ **Consuming payment gateway APIs (e.g., PayPal, Stripe)** 💳  
✔ **Calling internal microservices in a distributed system** 🔄  

Spring Boot provides **two main ways** to call external APIs:  
- **`RESTTemplate`** (Old, Synchronous) ✅  
- **`WebClient`** (New, Asynchronous, Reactive) ⚡  

---

## **2️⃣ Calling External APIs with `RESTTemplate` (Synchronous Approach)**  

📌 **Step 1: Add `RESTTemplate` Bean in `AppConfig.java`**  
Create a new package **`com.example.demo.config`** and add:

```java
package com.example.demo.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class AppConfig {

    @Bean
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}
```

📌 **Step 2: Create `ApiService.java` to Call an External API**  
Create a new package **`com.example.demo.service`** and add:

```java
package com.example.demo.service;

import org.springframework.stereotype.Service;
import org.springframework.web.client.RestTemplate;
import org.springframework.http.ResponseEntity;

@Service
public class ApiService {

    private final RestTemplate restTemplate;

    public ApiService(RestTemplate restTemplate) {
        this.restTemplate = restTemplate;
    }

    public String getExternalApiData() {
        String apiUrl = "https://api.publicapis.org/entries"; // External API
        ResponseEntity<String> response = restTemplate.getForEntity(apiUrl, String.class);
        return response.getBody();
    }
}
```

📌 **Step 3: Create a Controller to Expose API**  
Create a new package **`com.example.demo.controller`** and add:

```java
package com.example.demo.controller;

import com.example.demo.service.ApiService;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api/external")
public class ApiController {

    private final ApiService apiService;

    public ApiController(ApiService apiService) {
        this.apiService = apiService;
    }

    @GetMapping("/fetch")
    public String fetchExternalData() {
        return apiService.getExternalApiData();
    }
}
```

📌 **Step 4: Run the Application & Test in Browser/Postman**  
```bash
mvn spring-boot:run
```
```bash
curl -X GET "http://localhost:8080/api/external/fetch"
```
🎉 **Your Spring Boot application is now consuming an external API!** 🚀  

---

## **3️⃣ Calling External APIs with `WebClient` (Asynchronous Approach)**  
**Why use WebClient?**  
✅ Supports **Non-blocking Asynchronous Calls**  
✅ Handles **Streaming Responses** (e.g., Large JSON)  
✅ Recommended for **Reactive Programming & Microservices**  

📌 **Step 1: Add WebFlux Dependency in `pom.xml`**  
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>
```

📌 **Step 2: Configure `WebClient` Bean in `AppConfig.java`**  
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.reactive.function.client.WebClient;

@Configuration
public class AppConfig {

    @Bean
    public WebClient.Builder webClientBuilder() {
        return WebClient.builder();
    }
}
```

📌 **Step 3: Create `WebClientService.java` for API Calls**  
```java
package com.example.demo.service;

import org.springframework.stereotype.Service;
import org.springframework.web.reactive.function.client.WebClient;
import reactor.core.publisher.Mono;

@Service
public class WebClientService {

    private final WebClient.Builder webClientBuilder;

    public WebClientService(WebClient.Builder webClientBuilder) {
        this.webClientBuilder = webClientBuilder;
    }

    public Mono<String> fetchExternalData() {
        return webClientBuilder.build()
                .get()
                .uri("https://api.publicapis.org/entries")
                .retrieve()
                .bodyToMono(String.class);
    }
}
```

📌 **Step 4: Create `WebClientController.java`**  
```java
package com.example.demo.controller;

import com.example.demo.service.WebClientService;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import reactor.core.publisher.Mono;

@RestController
@RequestMapping("/api/webclient")
public class WebClientController {

    private final WebClientService webClientService;

    public WebClientController(WebClientService webClientService) {
        this.webClientService = webClientService;
    }

    @GetMapping("/fetch")
    public Mono<String> fetchExternalData() {
        return webClientService.fetchExternalData();
    }
}
```

📌 **Step 5: Run & Test WebClient API**  
```bash
mvn spring-boot:run
```
```bash
curl -X GET "http://localhost:8080/api/webclient/fetch"
```

🎉 **Now you're making non-blocking API calls using WebClient!** 🚀  

---

## **4️⃣ Handling API Errors & Timeouts**
**Problem:** External APIs might fail due to **server errors, timeouts, or bad requests**.  

✅ **Solution:** Add **error handling & timeout settings**.  

📌 **Modify `WebClientService.java` to Handle Errors & Timeouts**  
```java
public Mono<String> fetchExternalData() {
    return webClientBuilder.build()
            .get()
            .uri("https://api.publicapis.org/entries")
            .retrieve()
            .onStatus(status -> status.value() == 404, response -> Mono.error(new RuntimeException("Not Found!")))
            .onStatus(status -> status.value() == 500, response -> Mono.error(new RuntimeException("Server Error!")))
            .bodyToMono(String.class)
            .timeout(Duration.ofSeconds(5)) // Timeout after 5 sec
            .doOnError(error -> System.out.println("Error: " + error.getMessage()));
}
```

✅ **Now WebClient will:**
- **Handle 404 & 500 Errors** gracefully  
- **Timeout after 5 seconds** to prevent long waits  
- **Log errors** for debugging  

---

## 🎯 **Lesson 13 - Summary**  
✅ Used **RESTTemplate** for **synchronous API calls**  
✅ Used **WebClient** for **asynchronous non-blocking calls**  
✅ Implemented **GET requests to fetch external API data**  
✅ Added **error handling & timeout settings** for API failures  
