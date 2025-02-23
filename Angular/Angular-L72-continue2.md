### **Lesson 72: Building RESTful APIs with Spring Boot and Integrating with Angular** (Deep Dive Edition)

---

## **3. Building CRUD Operations with Spring Data JPA**

---

### **3.1 Creating REST Endpoints for CRUD Operations**

In this section, we'll dive deep into building **full CRUD (Create, Read, Update, Delete) operations** in Spring Boot using **Spring Data JPA** and then integrate them with **Angular's HttpClient Module**.

---

### **3.1.1 Entity Class: User**
- **Entity Class** represents a table in the database.
- Mapped using **JPA annotations** such as `@Entity`, `@Table`, `@Id`, and `@GeneratedValue`.

**Example: User Entity Class**
```java
package com.example.demo.model;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.Table;
import lombok.Data;

@Entity
@Table(name = "users")
@Data
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String email;
    private String password;
}
```

- **@Entity** – Marks the class as a JPA entity.
- **@Table(name = "users")** – Maps this entity to the `users` table in the database.
- **@Id** – Marks the primary key field.
- **@GeneratedValue** – Configures the primary key generation strategy.
- **@Data (Lombok)** – Generates getters, setters, `equals()`, `hashCode()`, and `toString()` methods.

---

### **3.1.2 Repository Layer: UserRepository**

- Extends **JpaRepository** which provides built-in CRUD operations.
- Spring Data JPA automatically implements this interface at runtime.

**Example: UserRepository Interface**
```java
package com.example.demo.repository;

import com.example.demo.model.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    // Custom query methods can be defined here if needed
}
```

- **JpaRepository<User, Long>** – Manages the `User` entity with `Long` as the primary key type.
- Provides methods like:
  - `findAll()` – Retrieves all records.
  - `findById(Long id)` – Retrieves a record by ID.
  - `save(User user)` – Inserts or updates a record.
  - `deleteById(Long id)` – Deletes a record by ID.

---

### **3.1.3 Service Layer: UserService**

- **Encapsulates business logic**.
- Uses **@Service** annotation to indicate that it's a service layer component.
- **Communicates with the Repository layer** to perform CRUD operations.

**Example: UserService Class**
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

    @Autowired
    private UserRepository userRepository;

    public List<User> getAllUsers() {
        return userRepository.findAll();
    }

    public Optional<User> getUserById(Long id) {
        return userRepository.findById(id);
    }

    public User createUser(User user) {
        return userRepository.save(user);
    }

    public User updateUser(Long id, User userDetails) {
        User user = userRepository.findById(id)
            .orElseThrow(() -> new RuntimeException("User not found with id: " + id));
        user.setName(userDetails.getName());
        user.setEmail(userDetails.getEmail());
        user.setPassword(userDetails.getPassword());
        return userRepository.save(user);
    }

    public void deleteUser(Long id) {
        userRepository.deleteById(id);
    }
}
```

- **@Service** – Registers the class as a Spring service.
- **@Autowired** – Injects the `UserRepository`.
- **Optional<User>** – Handles potential null values.
- **RuntimeException** – Custom exception for not found scenarios.

---

### **3.1.4 Controller Layer: UserController**

- **Exposes RESTful endpoints** to the Angular frontend.
- Uses **@RestController** and **@RequestMapping**.
- Handles HTTP methods: `GET`, `POST`, `PUT`, `DELETE`.

**Example: UserController Class**
```java
package com.example.demo.controller;

import com.example.demo.model.User;
import com.example.demo.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/users")
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }

    @GetMapping("/{id}")
    public ResponseEntity<User> getUserById(@PathVariable Long id) {
        return userService.getUserById(id)
            .map(ResponseEntity::ok)
            .orElse(ResponseEntity.notFound().build());
    }

    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.createUser(user);
    }

    @PutMapping("/{id}")
    public User updateUser(@PathVariable Long id, @RequestBody User userDetails) {
        return userService.updateUser(id, userDetails);
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        userService.deleteUser(id);
        return ResponseEntity.noContent().build();
    }
}
```

- **@RestController** – Defines a RESTful web service controller.
- **@RequestMapping("/api/users")** – Base URI for all endpoints.
- **@GetMapping, @PostMapping, @PutMapping, @DeleteMapping** – Maps HTTP methods.
- **ResponseEntity** – Handles response status and payload.

---

### **3.1.5 CORS Configuration for Angular and Spring Boot Communication**

- **Cross-Origin Resource Sharing (CORS)** is needed because Angular and Spring Boot run on different ports.
- Configure CORS to allow Angular (e.g., http://localhost:4200) to communicate with Spring Boot.

**Example: Global CORS Configuration**
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig {

    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/**").allowedOrigins("http://localhost:4200");
            }
        };
    }
}
```

- **@Configuration** – Registers this as a configuration class.
- **addMapping("/**")** – Applies CORS to all endpoints.
- **allowedOrigins("http://localhost:4200")** – Allows Angular's development server.

---

### **Next Steps:**
1. **4. Integrating Angular with Spring Boot APIs**
   - Using Angular's HttpClient Module
   - Making HTTP Requests (GET, POST, PUT, DELETE)
   - Handling HTTP Errors in Angular
   - Displaying Data with Angular Components and Services

2. **5. Hands-on Exercise: Building a Complete CRUD Feature**
   - Full-Stack Integration of Angular and Spring Boot
   - Real-world example with detailed implementation
