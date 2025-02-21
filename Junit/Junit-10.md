---

# **Phase 3 - Lesson 10: Integration Testing with Spring Boot**

---

## **1. What is Integration Testing?**

**Integration Testing** is a testing level where:
- Individual units are **combined and tested as a group**.
- It ensures that modules work together as expected.
- It **verifies interactions** between components like controllers, services, repositories, and external systems.

---

## **2. Why Use Integration Testing in Spring Boot?**

- **End-to-End Verification:** Ensures the entire flow works from controller to database.
- **Confidence in Changes:** Confirms that changes in one layer don't break other layers.
- **Realistic Environment Testing:** Simulates real-world scenarios by using an application context similar to production.

---

## **3. Key Annotations in Spring Boot Testing**

### **1. @SpringBootTest**
- Loads the **full application context**.
- Ideal for **end-to-end integration testing**.
- Slower than unit tests due to the startup time.

### **2. @WebMvcTest**
- Loads only the **web layer** (controllers, filters, etc.).
- Excludes service and repository beans.
- Used for **controller testing**.

### **3. @MockBean**
- Mocks a Spring bean within the application context.
- Replaces the real bean with a **Mockito mock**.
- Useful for isolating the layer being tested.

### **4. @AutoConfigureMockMvc**
- Configures `MockMvc` for testing **Spring MVC controllers**.
- Allows testing REST APIs without starting a web server.

---

## **4. Setting Up Spring Boot Integration Tests**

### **1. Add Required Dependencies**

**Maven:**
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

**Gradle:**
```groovy
testImplementation 'org.springframework.boot:spring-boot-starter-test'
```

This includes:
- JUnit 5
- Spring Test
- Mockito
- AssertJ (for fluent assertions)

---

## **5. Example: Testing a REST Controller**

Let's test the following flow:
1. `UserController`: REST API for managing users.
2. `UserService`: Business logic for user operations.
3. `UserRepository`: Data access layer for user entities.

---

### **1. User Entity**

```java
package com.example.demo.entity;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;

@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String email;

    // Constructors, Getters, and Setters
    public User() {
    }

    public User(Long id, String name, String email) {
        this.id = id;
        this.name = name;
        this.email = email;
    }

    public Long getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public String getEmail() {
        return email;
    }
}
```

---

### **2. UserRepository Interface**

```java
package com.example.demo.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import com.example.demo.entity.User;

public interface UserRepository extends JpaRepository<User, Long> {
}
```

---

### **3. UserService Class**

```java
package com.example.demo.service;

import com.example.demo.entity.User;
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
}
```

---

### **4. UserController Class**

```java
package com.example.demo.controller;

import com.example.demo.entity.User;
import com.example.demo.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;
import java.util.Optional;

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
        Optional<User> user = userService.getUserById(id);
        return user.map(ResponseEntity::ok)
                   .orElseGet(() -> ResponseEntity.notFound().build());
    }

    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.createUser(user);
    }
}
```

---

## **6. Integration Test with @SpringBootTest**

### **1. What is @SpringBootTest?**
- Loads the **full application context**.
- Starts an **embedded web server** for end-to-end testing.
- Tests **controller, service, and repository** layers together.

### **2. Example: Testing UserController**

```java
package com.example.demo;

import com.example.demo.entity.User;
import com.example.demo.repository.UserRepository;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTest;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import static org.assertj.core.api.Assertions.assertThat;

@ExtendWith(SpringExtension.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class UserControllerIntegrationTest {

    @Autowired
    private TestRestTemplate restTemplate;

    @Autowired
    private UserRepository userRepository;

    @Test
    void testCreateUser() {
        User user = new User(null, "John Doe", "john@example.com");

        ResponseEntity<User> response = restTemplate.postForEntity("/api/users", user, User.class);

        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
        assertThat(response.getBody()).isNotNull();
        assertThat(response.getBody().getId()).isNotNull();
    }

    @Test
    void testGetAllUsers() {
        User user1 = new User(null, "Alice", "alice@example.com");
        User user2 = new User(null, "Bob", "bob@example.com");
        userRepository.save(user1);
        userRepository.save(user2);

        ResponseEntity<User[]> response = restTemplate.getForEntity("/api/users", User[].class);

        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
        assertThat(response.getBody()).isNotEmpty();
        assertThat(response.getBody().length).isEqualTo(2);
    }
}
```

### **Explanation:**
- `@SpringBootTest`: Loads the entire application context.
- `webEnvironment = RANDOM_PORT`: Starts an embedded server on a random port.
- `TestRestTemplate`: Used to **call REST endpoints**.
- `assertThat(...)`: Asserts the status code, response body, and list size.

---

## **7. Running the Integration Test**

- In **IntelliJ** or **Eclipse**: Right-click the `UserControllerIntegrationTest` class → **Run**.
- Using **Maven**:
```bash
mvn test
```
- Using **Gradle**:
```bash
./gradlew test
```

---

## **8. Common Mistakes to Avoid**
- **Wrong Web Environment:** Use `RANDOM_PORT` or `DEFINED_PORT` for embedded server tests.
- **Database State:** Clean up the database after tests using `@AfterEach` or an in-memory database.
- **Misconfigured Dependencies:** Ensure all required beans are loaded in the context.

---

## **9. Hands-On Exercise**
1. **Add More Integration Tests:**
   - Test the `getUserById()` endpoint.
2. **Experiment with MockMvc:**
   - Use `MockMvc` to test controllers without starting the server.
3. **Test Negative Scenarios:**
   - Test for `404 Not Found` when user does not exist.

---

## **Next Steps: Phase 3 - Lesson 11**
In the next lesson, we'll cover:
- **Test-Driven Development (TDD)**
  - Writing tests before implementation.
  - Red-Green-Refactor cycle.
  - Practical examples of TDD in action.
