---

# **Phase 2: Unit Testing with JUnit and Mockito**

---

## **Objective**

In this phase, we will:
- Write **Unit Tests** for the **Service** and **Repository** layers of:
  - **User Service**
  - **Product Service**
  - **Order Service**
- Use **Mockito** to mock dependencies and isolate the unit under test.
- Achieve **high code coverage** using **JaCoCo**.

---

## **1. Unit Testing Strategy**

### **1. What to Test:**
- **Service Layer:**
  - Business logic
  - Input validation
  - Interaction with the repository layer
- **Repository Layer:**
  - Custom query methods (if any)
  - Basic CRUD operations (optional, as Spring Data JPA is already tested)

### **2. What Not to Test:**
- **Framework Integration:** Do not test Spring Boot framework features.
- **Database Operations:** These are covered in **Integration Tests**.

---

## **2. Dependencies and Setup**

### **1. Add JUnit 5 and Mockito Dependencies**

**Maven:**
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
    <exclusions>
        <exclusion>
            <groupId>org.junit.vintage</groupId>
            <artifactId>junit-vintage-engine</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-core</artifactId>
    <version>5.0.0</version>
    <scope>test</scope>
</dependency>
```

**Gradle:**
```groovy
testImplementation 'org.springframework.boot:spring-boot-starter-test'
testImplementation 'org.mockito:mockito-core:5.0.0'
```

---

## **3. Unit Tests for User Service**

### **1. User Entity**

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

### **2. UserRepository Interface**

```java
package com.example.user.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import com.example.user.entity.User;

public interface UserRepository extends JpaRepository<User, Long> {
}
```

---

### **3. UserService Class**

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

### **4. Unit Tests for UserService**

```java
package com.example.user.service;

import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.Mockito.*;

import com.example.user.entity.User;
import com.example.user.repository.UserRepository;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;

import java.util.List;
import java.util.Optional;

@ExtendWith(MockitoExtension.class)
public class UserServiceTest {

    @Mock
    private UserRepository userRepository;

    @InjectMocks
    private UserService userService;

    private User user;

    @BeforeEach
    void setUp() {
        user = new User(1L, "John Doe", "john@example.com");
    }

    @Test
    void testGetAllUsers() {
        when(userRepository.findAll()).thenReturn(List.of(user));

        List<User> users = userService.getAllUsers();

        assertNotNull(users);
        assertEquals(1, users.size());
        assertEquals("John Doe", users.get(0).getName());
        verify(userRepository, times(1)).findAll();
    }

    @Test
    void testGetUserById_UserExists() {
        when(userRepository.findById(1L)).thenReturn(Optional.of(user));

        Optional<User> foundUser = userService.getUserById(1L);

        assertTrue(foundUser.isPresent());
        assertEquals("John Doe", foundUser.get().getName());
        verify(userRepository, times(1)).findById(1L);
    }

    @Test
    void testGetUserById_UserNotFound() {
        when(userRepository.findById(999L)).thenReturn(Optional.empty());

        Optional<User> foundUser = userService.getUserById(999L);

        assertFalse(foundUser.isPresent());
        verify(userRepository, times(1)).findById(999L);
    }

    @Test
    void testCreateUser() {
        when(userRepository.save(user)).thenReturn(user);

        User savedUser = userService.createUser(user);

        assertNotNull(savedUser);
        assertEquals("John Doe", savedUser.getName());
        verify(userRepository, times(1)).save(user);
    }
}
```

---

### **Explanation:**
- `@Mock`: Creates a mock instance of `UserRepository`.
- `@InjectMocks`: Injects the mock into `UserService`.
- `when(...).thenReturn(...)`: Stubs the repository method calls.
- `verify(...)`: Verifies the interaction with the mock.
- **Tests Covered:**
  - `getAllUsers()`
  - `getUserById()` for existing and non-existing users.
  - `createUser()`

### **Running the Tests:**
- In **IntelliJ** or **Eclipse**: Right-click the `UserServiceTest` class → **Run**.
- Using **Maven**:
```bash
mvn test
```
- Using **Gradle**:
```bash
./gradlew test
```

---

## **4. Unit Tests for Product Service and Order Service**

- **Repeat the above pattern** for `ProductService` and `OrderService`.
- Use **Mockito** to mock their repositories and dependencies.
- Test all CRUD operations and business logic.

---

## **5. Measuring Code Coverage with JaCoCo**

### **1. Add JaCoCo Plugin**

**Maven:**
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.jacoco</groupId>
            <artifactId>jacoco-maven-plugin</artifactId>
            <version>0.8.8</version>
            <executions>
                <execution>
                    <goals>
                        <goal>prepare-agent</goal>
                        <goal>report</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

### **2. Running Code Coverage Report**
```bash
mvn test
mvn jacoco:report
```

### **3. Viewing the Report**
- The report is generated at:
```
target/site/jacoco/index.html
```
- Open `index.html` in a browser to view detailed coverage.

---

## **Next Steps: Phase 3 - Integration Testing with TestContainers**

In the next phase, we'll:
- Write **Integration Tests** for User, Product, and Order services.
- Use **TestContainers** with **PostgreSQL**.
- Mock external dependencies using **@MockBean**.
