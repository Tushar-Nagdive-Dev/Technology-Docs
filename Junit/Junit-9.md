---

# **Phase 3 - Lesson 9: Mocking with Mockito**

---

## **1. What is Mocking?**

**Mocking** is a testing technique used to **simulate the behavior of complex objects** and dependencies. In unit testing:
- **Mocks** are dummy objects that mimic the behavior of real objects.
- They **isolate the class under test** by simulating its dependencies.
- They help **focus on testing the logic** of the class itself, not the behavior of its dependencies.

---

## **2. Why Use Mockito?**

**Mockito** is the most popular mocking framework for Java because:
- It is **easy to use** with a clean and readable API.
- It supports **behavior verification** (e.g., checking method calls).
- It integrates seamlessly with **JUnit 5**.

---

## **3. Key Concepts in Mockito**

### **1. @Mock**
- Creates a mock object.
- Does not call real methods.
- Returns default values (`null`, `0`, `false`) unless specified.

### **2. @InjectMocks**
- Injects mocks into the class under test.
- Automatically injects all mocked dependencies.

### **3. @Spy**
- Creates a partial mock.
- Calls real methods **unless** they are explicitly stubbed.

### **4. when() and thenReturn()**
- **when()**: Specifies the method call to be stubbed.
- **thenReturn()**: Defines the return value for the stubbed method.

### **5. verify()**
- Verifies if a method was called.
- Can check the number of invocations.

---

## **4. Setting Up Mockito with JUnit 5**

### **1. Add Mockito Dependency**

**Maven:**
```xml
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-core</artifactId>
    <version>5.0.0</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-junit-jupiter</artifactId>
    <version>5.0.0</version>
    <scope>test</scope>
</dependency>
```

**Gradle:**
```groovy
testImplementation 'org.mockito:mockito-core:5.0.0'
testImplementation 'org.mockito:mockito-junit-jupiter:5.0.0'
```

### **2. Enable Mockito Annotations**
```java
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.junit.jupiter.MockitoExtension;

@ExtendWith(MockitoExtension.class)
```

- `@ExtendWith(MockitoExtension.class)`: Enables Mockito support in JUnit 5.

---

## **5. Example: Mocking a Service Class**

Let's assume we have the following scenario:
- `UserService`: A class responsible for user-related operations.
- `UserRepository`: A dependency that interacts with the database.

We want to **unit test** `UserService` without calling the actual database, so we'll **mock** `UserRepository`.

---

### **1. User Entity**
```java
public class User {
    private Long id;
    private String name;

    // Constructors, Getters, and Setters
    public User(Long id, String name) {
        this.id = id;
        this.name = name;
    }

    public Long getId() {
        return id;
    }

    public String getName() {
        return name;
    }
}
```

---

### **2. UserRepository Interface**
```java
public interface UserRepository {
    User findById(Long id);
    void save(User user);
}
```

---

### **3. UserService Class**
```java
public class UserService {

    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public User getUserById(Long id) {
        return userRepository.findById(id);
    }

    public void saveUser(User user) {
        userRepository.save(user);
    }
}
```

---

### **4. Unit Test with Mockito**

```java
import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.Mockito.*;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;

@ExtendWith(MockitoExtension.class)
public class UserServiceTest {

    @Mock
    private UserRepository userRepository;

    @InjectMocks
    private UserService userService;

    @BeforeEach
    void setUp() {
        userService = new UserService(userRepository);
    }

    @Test
    void testGetUserById() {
        // Arrange
        User mockUser = new User(1L, "John Doe");
        when(userRepository.findById(1L)).thenReturn(mockUser);

        // Act
        User user = userService.getUserById(1L);

        // Assert
        assertNotNull(user);
        assertEquals("John Doe", user.getName());
    }

    @Test
    void testSaveUser() {
        // Arrange
        User user = new User(2L, "Jane Doe");

        // Act
        userService.saveUser(user);

        // Assert
        verify(userRepository, times(1)).save(user);
    }
}
```

### **Explanation:**
- `@Mock`: Creates a mock instance of `UserRepository`.
- `@InjectMocks`: Injects the mock into `UserService`.
- `when(...).thenReturn(...)`: Stubs the behavior of the `findById()` method.
- `verify(...)`: Verifies that `save()` was called exactly once.
- `times(1)`: Checks the number of method calls.

---

## **6. Using @Spy for Partial Mocks**

### **1. What is @Spy?**
- `@Spy` creates a **partial mock**.
- It **calls real methods** unless they are explicitly stubbed.

### **Example:**
```java
import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.Mockito.*;

import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Spy;
import org.mockito.junit.jupiter.MockitoExtension;

@ExtendWith(MockitoExtension.class)
public class SpyTest {

    @Spy
    private UserService userServiceSpy = new UserService(null);

    @Test
    void testSpyBehavior() {
        // Stub the method to return a custom value
        doReturn("Spy User").when(userServiceSpy).getUserById(1L).getName();

        // Call the real method (this will use the stubbed value)
        String name = userServiceSpy.getUserById(1L).getName();

        // Assert the result
        assertEquals("Spy User", name);
    }
}
```

### **Explanation:**
- `@Spy`: Creates a partial mock of `UserService`.
- `doReturn(...).when(...)`: Stubs the behavior of the `getUserById()` method.
- **Real methods** are called unless explicitly stubbed.

---

## **7. Common Mistakes to Avoid**
- **Incorrect Use of when() with Spy:** Use `doReturn(...).when(...)` with `@Spy`.
- **Unnecessary Mocks:** Avoid mocking classes you don’t own (e.g., utility classes).
- **Overuse of verify():** Use `verify()` only when testing interactions.

---

## **8. Expert Insights and Best Practices**
- **Mock External Dependencies:** Only mock dependencies external to the class under test.
- **Use @InjectMocks Sparingly:** Prefer constructor injection for better test maintainability.
- **Avoid Partial Mocks:** Use `@Spy` only when necessary, as it can lead to fragile tests.

---

## **9. Hands-On Exercise**
1. **Create Additional Mock Tests:**
   - Write tests for update and delete methods in `UserService`.
2. **Experiment with Spy:**
   - Spy on `Calculator` and mock one method while calling others.
3. **Verify Interactions:**
   - Verify method call order using `verifyNoMoreInteractions()`.

---

## **Next Steps: Phase 3 - Lesson 10**
In the next lesson, we'll cover:
- **Integration Testing with Spring Boot**
  - Using `@SpringBootTest` for full integration testing.
  - Testing REST Controllers with `MockMvc`.
  - Mocking service layers with `@MockBean`.
