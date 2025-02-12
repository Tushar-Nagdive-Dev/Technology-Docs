# 🚀 **Phase 1 - Lesson 2: Understanding Spring Boot Project Structure & Components** 🚀  

## **📌 Lesson Objective**
By the end of this lesson, you will:  
✅ Understand the **default Spring Boot project structure**  
✅ Learn about **important files and folders** in a Spring Boot project  
✅ Explore the **Spring Boot application entry point**  
✅ Understand **Spring Boot Annotations** and how they work  
✅ Learn about **application properties (application.properties & application.yml)**  

---

## **1️⃣ Spring Boot Project Structure Overview**
When you create a **Spring Boot** project (using **Spring Initializr** or manually), you will get the following directory structure:

```
📂 my-spring-boot-app/
├── 📂 src/
│   ├── 📂 main/
│   │   ├── 📂 java/com/example/demo/
│   │   │   ├── 📄 DemoApplication.java  <-- Entry point
│   │   │   ├── 📂 controller/  <-- Controllers (REST APIs)
│   │   │   ├── 📂 service/  <-- Business logic
│   │   │   ├── 📂 repository/  <-- Database interactions
│   │   │   ├── 📂 model/  <-- Data models (Entities)
│   │   │   ├── 📂 config/  <-- Configuration files
│   │   ├── 📂 resources/
│   │   │   ├── 📄 application.properties  <-- Configuration file
│   │   │   ├── 📂 static/  <-- Static resources (HTML, CSS, JS)
│   │   │   ├── 📂 templates/  <-- HTML templates (Thymeleaf, Freemarker)
│   │   │   ├── 📂 public/  <-- Public assets
│   ├── 📂 test/  <-- Unit & Integration Tests
├── 📄 pom.xml  <-- Maven configuration file
```

💡 **Understanding the Key Components**:
- `src/main/java/` → Contains **Java source code**.
- `src/main/resources/` → Stores **configuration, static files, and templates**.
- `application.properties` or `application.yml` → **Configuration settings**.
- `pom.xml` → **Maven dependencies**.

---

## **2️⃣ Spring Boot Application Entry Point**
The **main class** (`DemoApplication.java`) is the entry point of every Spring Boot application.

### **🔥 What does it do?**
1️⃣ It **boots up the Spring Boot application**.  
2️⃣ It **auto-configures** the application (based on dependencies).  
3️⃣ It **starts the embedded web server** (Tomcat, Jetty, Undertow).  

### **📝 Example:**
```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication  // Auto-configures Spring Boot
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);  // Starts Spring Boot
    }
}
```

### **💡 Breaking it down**
| Annotation | Purpose |
|------------|---------|
| `@SpringBootApplication` | Enables **auto-configuration, component scanning, and Spring Boot setup** |
| `SpringApplication.run()` | Starts the **Spring Boot application and embedded Tomcat** |

---

## **3️⃣ Understanding Spring Boot Annotations**
Spring Boot uses **annotations** to reduce configuration.

### **Commonly Used Spring Boot Annotations**
| Annotation | Description |
|------------|-------------|
| `@SpringBootApplication` | Main entry point of Spring Boot |
| `@RestController` | Defines a **REST API controller** |
| `@GetMapping("/users")` | Maps an HTTP `GET` request to a method |
| `@PostMapping("/users")` | Maps an HTTP `POST` request to a method |
| `@Service` | Defines a **Service Layer** for business logic |
| `@Repository` | Defines a **Repository Layer** for database interactions |
| `@Component` | Generic component that Spring manages |
| `@Autowired` | Injects **dependencies automatically** |

👉 These annotations make Spring Boot applications **clean, modular, and easy to develop**.

---

## **4️⃣ application.properties & application.yml**
Spring Boot uses **configuration files** to manage settings.

### **1️⃣ `application.properties` (Key-Value Format)**
Used to configure **database, server, security, logging**, etc.

Example:
```properties
# Server Configuration
server.port=8081

# Database Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=1234
spring.jpa.hibernate.ddl-auto=update

# Logging
logging.level.org.springframework=INFO
```

### **2️⃣ `application.yml` (YAML Format)**
Same as `application.properties`, but in a **structured** format.

Example:
```yaml
server:
  port: 8081

spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mydb
    username: root
    password: 1234
  jpa:
    hibernate:
      ddl-auto: update

logging:
  level:
    org.springframework: INFO
```

💡 **Which one to use?**
- `application.properties` → **Simpler & easier for small configurations**.
- `application.yml` → **Better for complex configurations with multiple levels**.

---

## **5️⃣ Spring Boot Controllers (REST API Basics)**
A **controller** handles HTTP requests and sends responses.

### **Example: Creating a REST API**
Create a new **Controller** inside `src/main/java/com/example/demo/controller/`:

```java
package com.example.demo.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController  // Marks this class as a REST API Controller
@RequestMapping("/api")  // Base URL for APIs
public class HelloController {
    
    @GetMapping("/hello")
    public String sayHello() {
        return "Hello, Spring Boot!";
    }
}
```

### **Run the application and test:**
1️⃣ **Start your Spring Boot application** (`mvn spring-boot:run`).  
2️⃣ **Open browser or Postman** and visit:  
   ```
   http://localhost:8080/api/hello
   ```
3️⃣ You should see:
   ```
   Hello, Spring Boot!
   ```

---

## 🎯 **Lesson 2 - Summary**
✅ **Spring Boot project structure** follows a modular approach.  
✅ **`@SpringBootApplication`** is the main entry point.  
✅ **Annotations like `@RestController`, `@GetMapping`, `@Autowired`** make development easier.  
✅ **`application.properties` vs `application.yml`** for configuration.  
✅ **Created a basic REST API using Spring Boot**.  

---

## **💻 Hands-on Exercise: Modify & Run Spring Boot API**
🎯 **Task**:  
1️⃣ Modify the existing REST API:
   - Add a new endpoint `/api/greet` that returns **"Hello, [your name]!"**  
   - Change the server port to `9090` using `application.properties`.  
2️⃣ Run the application and test the API.

---
