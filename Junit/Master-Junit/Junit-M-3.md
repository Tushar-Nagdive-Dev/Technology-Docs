---

# **Phase 3: Integration Testing with TestContainers**

---

## **Objective**

In this phase, we will:
- Write **Integration Tests** for:
  - **User Service**
  - **Product Service**
  - **Order Service**
- Use **TestContainers** with **PostgreSQL** to:
  - Spin up a real PostgreSQL container for each test.
  - Ensure consistent and isolated test environments.
- Mock external dependencies using **@MockBean**.
- Achieve **end-to-end integration** without affecting the local database.

---

## **1. Why Use TestContainers for Integration Testing?**

- **Realistic Testing Environment:** Uses a real PostgreSQL database instead of in-memory databases like H2.
- **Isolation and Repeatability:** Containers are isolated, ensuring tests do not interfere with each other.
- **CI/CD Friendly:** Containers can be spun up in CI pipelines for consistent test environments.

---

## **2. Test Strategy and Scope**

### **1. What to Test:**
- **Repository Layer:**
  - Custom queries
  - CRUD operations
- **Service Layer:**
  - Interaction with repositories
  - Business logic
  - End-to-end flow

### **2. What Not to Test:**
- **Controller Layer:** Covered in **End-to-End Tests**.
- **Framework Behavior:** Spring Boot and JPA functionalities are already tested by their maintainers.

---

## **3. Setting Up TestContainers**

### **1. Add TestContainers Dependencies**

**Maven:**
```xml
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>testcontainers</artifactId>
    <version>1.19.0</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>postgresql</artifactId>
    <version>1.19.0</version>
    <scope>test</scope>
</dependency>
```

**Gradle:**
```groovy
testImplementation 'org.testcontainers:testcontainers:1.19.0'
testImplementation 'org.testcontainers:postgresql:1.19.0'
```

### **2. Docker Requirement**
- **TestContainers** requires **Docker** to be installed and running.
- Ensure Docker is up and accessible by running:
```bash
docker --version
```

---

## **4. Integration Test for User Service**

### **1. User Entity (Recap)**

```java
package com.example.user.entity;

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
    public User() {}

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

### **2. UserRepository (Recap)**

```java
package com.example.user.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import com.example.user.entity.User;

public interface UserRepository extends JpaRepository<User, Long> {
}
```

---

### **3. UserService (Recap)**

```java
package com.example.user.service;

import com.example.user.entity.User;
import com.example.user.repository.UserRepository;
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

### **4. Integration Test with TestContainers**

```java
package com.example.user.service;

import static org.assertj.core.api.Assertions.assertThat;

import com.example.user.entity.User;
import com.example.user.repository.UserRepository;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTest;
import org.springframework.test.context.DynamicPropertyRegistry;
import org.springframework.test.context.DynamicPropertySource;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import org.testcontainers.containers.PostgreSQLContainer;
import org.testcontainers.junit.jupiter.Container;
import org.testcontainers.junit.jupiter.Testcontainers;

import java.util.Optional;

@ExtendWith(SpringExtension.class)
@DataJpaTest
@Testcontainers
public class UserServiceIntegrationTest {

    @Container
    static PostgreSQLContainer<?> postgresContainer = new PostgreSQLContainer<>("postgres:latest")
            .withDatabaseName("test_db")
            .withUsername("test_user")
            .withPassword("test_pass");

    @DynamicPropertySource
    static void overrideProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgresContainer::getJdbcUrl);
        registry.add("spring.datasource.username", postgresContainer::getUsername);
        registry.add("spring.datasource.password", postgresContainer::getPassword);
    }

    @Autowired
    private UserRepository userRepository;

    @Test
    void testCreateUser() {
        User user = new User(null, "Alice", "alice@example.com");
        User savedUser = userRepository.save(user);

        assertThat(savedUser).isNotNull();
        assertThat(savedUser.getId()).isNotNull();
        assertThat(savedUser.getName()).isEqualTo("Alice");
    }

    @Test
    void testFindUserById() {
        User user = new User(null, "Bob", "bob@example.com");
        User savedUser = userRepository.save(user);

        Optional<User> foundUser = userRepository.findById(savedUser.getId());

        assertThat(foundUser).isPresent();
        assertThat(foundUser.get().getName()).isEqualTo("Bob");
    }

    @Test
    void testFindAllUsers() {
        userRepository.save(new User(null, "Charlie", "charlie@example.com"));
        userRepository.save(new User(null, "Dave", "dave@example.com"));

        var users = userRepository.findAll();

        assertThat(users).isNotEmpty();
        assertThat(users.size()).isEqualTo(2);
    }
}
```

---

### **Explanation:**
- `@Container`: Declares the PostgreSQL container.
- `@Testcontainers`: Initializes and manages the container lifecycle.
- `@DynamicPropertySource`: Overrides Spring DataSource properties.
- `@DataJpaTest`: Loads only the JPA components (Repositories and Entities).
- **Tests Covered:**
  - `createUser()`
  - `findUserById()`
  - `findAllUsers()`

### **Running the Tests:**
- In **IntelliJ** or **Eclipse**: Right-click the `UserServiceIntegrationTest` class → **Run**.
- Using **Maven**:
```bash
mvn test
```
- Using **Gradle**:
```bash
./gradlew test
```

---

### **Expected Output:**
- TestContainers spins up a **PostgreSQL container**.
- The tests run **against the real database**.
- The container is **automatically started and stopped**.

---

## **5. Integration Tests for Product Service and Order Service**

- **Repeat the above pattern** for `ProductService` and `OrderService`.
- Use **TestContainers** with PostgreSQL for consistent environments.
- Test **end-to-end flow** for CRUD operations.

---

## **Next Steps: Phase 4 - End-to-End Testing with Selenium**

In the next phase, we'll:
- Automate E2E tests for:
  - User registration and login.
  - Product addition and listing.
  - Order creation and tracking.
- Use **Selenium WebDriver** for browser automation.
