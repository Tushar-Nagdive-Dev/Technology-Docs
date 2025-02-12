# 🚀 **Phase 1 - Lesson 6: Implementing Logging & Actuator for Monitoring in Spring Boot** 🚀  

## **📌 Lesson Objective**
By the end of this lesson, you will:  
✅ Understand the **importance of logging in Spring Boot**  
✅ Learn how to use **SLF4J & Logback for structured logging**  
✅ Implement **different logging levels** (`INFO`, `DEBUG`, `ERROR`)  
✅ Enable **Spring Boot Actuator** for monitoring  
✅ Explore **useful Actuator endpoints**  

---

## **1️⃣ What is Logging & Why is it Important?**
### 🔹 **Logging helps to:**
✔ Debug issues quickly 🛠  
✔ Track API requests & responses 📊  
✔ Monitor application behavior in production 🚀  
✔ Store logs in files for future reference 📂  

Spring Boot **uses SLF4J + Logback** for logging by default.

---

## **2️⃣ Using SLF4J (Simple Logging Facade for Java)**
📌 Modify `UserController` to include logging:

```java
package com.example.demo.controller;

import com.example.demo.model.User;
import com.example.demo.service.UserService;
import jakarta.validation.Valid;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.*;

import java.util.List;
import java.util.Optional;
import java.util.stream.Collectors;

@RestController
@RequestMapping("/api/users")
public class UserController {

    private static final Logger logger = LoggerFactory.getLogger(UserController.class);

    private final UserService userService;

    @Autowired
    public UserController(UserService userService) {
        this.userService = userService;
    }

    // CREATE User with logging
    @PostMapping
    public ResponseEntity<?> createUser(@Valid @RequestBody User user, BindingResult result) {
        if (result.hasErrors()) {
            List<String> errors = result.getFieldErrors()
                .stream()
                .map(error -> error.getField() + ": " + error.getDefaultMessage())
                .collect(Collectors.toList());

            logger.warn("Validation errors: {}", errors); // Logging validation errors
            return ResponseEntity.badRequest().body(errors);
        }

        logger.info("Creating a new user: {}", user.getEmail());
        return ResponseEntity.ok(userService.createUser(user));
    }

    // READ all users
    @GetMapping
    public List<User> getAllUsers() {
        logger.info("Fetching all users");
        return userService.getAllUsers();
    }

    // READ User by ID
    @GetMapping("/{id}")
    public ResponseEntity<?> getUserById(@PathVariable Long id) {
        logger.info("Fetching user with ID: {}", id);
        Optional<User> user = userService.getUserById(id);

        if (user.isPresent()) {
            return ResponseEntity.ok(user);
        } else {
            logger.error("User with ID {} not found", id);
            return ResponseEntity.notFound().build();
        }
    }
}
```

✅ **Logging Levels Used:**
- `logger.info()` → Informational messages  
- `logger.warn()` → Warnings (e.g., validation errors)  
- `logger.error()` → Errors (e.g., user not found)  

---

## **3️⃣ Setting Logging Levels in `application.properties`**
📌 Add logging configurations in `src/main/resources/application.properties`:

```properties
# Set logging levels
logging.level.root=INFO
logging.level.org.springframework=DEBUG
logging.level.com.example.demo=TRACE  # Custom logs
```

✅ **Explanation:**
- `INFO` → Default level (logs important events)  
- `DEBUG` → Useful for debugging (logs more details)  
- `TRACE` → Logs everything (useful for deep debugging)  

---

## **4️⃣ Writing Logs to a File**
📌 Modify `application.properties` to store logs in a file:

```properties
# Log file configuration
logging.file.name=logs/app.log
logging.pattern.file=%d{yyyy-MM-dd HH:mm:ss} - %msg%n
logging.level.com.example.demo=DEBUG
```

✅ **This will create a file `logs/app.log` where logs will be stored.**  

---

## **5️⃣ Enabling Spring Boot Actuator for Monitoring**
**Spring Boot Actuator** provides **production-ready features** for monitoring.

📌 **Step 1: Add Actuator Dependency**
Modify `pom.xml` and add:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

📌 **Step 2: Enable Actuator Endpoints in `application.properties`**
```properties
# Enable all Actuator endpoints
management.endpoints.web.exposure.include=*
```

📌 **Step 3: Restart the Application**
```bash
mvn spring-boot:run
```

📌 **Step 4: Access Actuator Endpoints**
| **Endpoint** | **Description** | **URL** |
|-------------|----------------|---------|
| `/actuator` | Lists all available endpoints | `http://localhost:8080/actuator` |
| `/actuator/health` | Checks the health of the application | `http://localhost:8080/actuator/health` |
| `/actuator/info` | Displays custom application info | `http://localhost:8080/actuator/info` |
| `/actuator/loggers` | Shows logging levels | `http://localhost:8080/actuator/loggers` |

📌 **Step 5: Customize Actuator Info**
Modify `application.properties`:

```properties
management.info.env.enabled=true
info.app.name=Spring Boot CRUD API
info.app.version=1.0.0
```

**Response of `/actuator/info`:**
```json
{
  "app": {
    "name": "Spring Boot CRUD API",
    "version": "1.0.0"
  }
}
```

✅ **Now we have monitoring enabled with Actuator!** 🎉

---

## 🎯 **Lesson 6 - Summary**
✅ Implemented **SLF4J logging** (`INFO`, `DEBUG`, `ERROR`)  
✅ Configured **log levels** in `application.properties`  
✅ Stored logs in a **file (`logs/app.log`)**  
✅ Enabled **Spring Boot Actuator for monitoring**  
✅ Tested Actuator **health, info, and loggers endpoints**  

---
