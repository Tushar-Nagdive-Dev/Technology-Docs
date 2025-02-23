### **Lesson 72: Building RESTful APIs with Spring Boot and Integrating with Angular** (Detailed Version)

---

## **1. Introduction to RESTful APIs with Spring Boot**

### **1.1 What are RESTful APIs?**
- **REST (Representational State Transfer)**:
  - An architectural style for building **scalable and maintainable web services**.
  - Communicates over **HTTP** and is **stateless**.
  - Uses **CRUD operations** mapped to **HTTP methods**:
    - **GET** – Retrieve data
    - **POST** – Create new data
    - **PUT** – Update existing data
    - **DELETE** – Delete data
- **JSON** is the most commonly used data format for request and response payloads.

---

### **1.2 Why Use Spring Boot for RESTful APIs?**
- **Spring Boot** is ideal for building RESTful APIs because:
  - **Easy Setup**: Rapid setup with **Spring Initializr**.
  - **Scalable Architecture**: Perfect for **microservices architecture**.
  - **Auto Configuration**: Auto-configures dependencies.
  - **Built-in Security**: With **Spring Security** integration.
  - **Integration with JPA/Hibernate** for **database access**.

---

### **1.3 Key Annotations and Concepts in Spring Boot REST APIs**
- **@RestController**:
  - Marks a class as a RESTful web service controller.
  - Combines `@Controller` and `@ResponseBody`.

- **@RequestMapping**:
  - Maps HTTP requests to handler methods.
  - Can be used at the class level or method level.

- **HTTP Method Annotations**:
  - **@GetMapping** – Maps HTTP GET requests.
  - **@PostMapping** – Maps HTTP POST requests.
  - **@PutMapping** – Maps HTTP PUT requests.
  - **@DeleteMapping** – Maps HTTP DELETE requests.

- **Request Handling Annotations**:
  - **@RequestBody** – Maps the request body to a method parameter.
  - **@PathVariable** – Maps URI path variables to method parameters.
  - **@RequestParam** – Extracts query parameters.
  - **@RequestHeader** – Extracts HTTP headers.

- **Response Annotations**:
  - **@ResponseStatus** – Customizes the HTTP status code.
  - **ResponseEntity** – Customizes the entire HTTP response.

- **Exception Handling**:
  - **@ExceptionHandler** – Handles exceptions thrown by controller methods.
  - **@ControllerAdvice** – Centralized exception handling across the application.

---

## **2. Setting up Controllers, Services, and Repositories in Spring Boot**

### **2.1 Creating REST Controllers with @RestController**
- **@RestController**:
  - Combines `@Controller` and `@ResponseBody`.
  - Returns data in **JSON format** using **Jackson** library.
  - Suitable for creating **RESTful web services**.

---

**Example: Simple UserController**
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping
    public String getAllUsers() {
        return "List of all users";
    }

    @GetMapping("/{id}")
    public String getUserById(@PathVariable Long id) {
        return "User with ID: " + id;
    }
}
```

- **Explanation**:
  - **@RestController** makes this class a RESTful controller.
  - **@RequestMapping("/api/users")** defines the base URI for all endpoints.
  - **@GetMapping** is used to map HTTP GET requests.
  - **@PathVariable** extracts the dynamic part of the URI.

---

### **2.2 Service Layer for Business Logic with @Service**
- **@Service** is used to:
  - **Encapsulate business logic**.
  - **Decouple business logic** from controller methods.
  - **Promote modularity** and **reusability**.

---

**Example: UserService**
```java
@Service
public class UserService {

    public String getAllUsers() {
        return "List of all users from Service";
    }

    public String getUserById(Long id) {
        return "User with ID: " + id + " from Service";
    }
}
```

- **Explanation**:
  - **@Service** makes this class a Spring bean containing business logic.
  - Methods contain business rules or calculations.

---

### **2.3 Data Access with Spring Data JPA and @Repository**
- **Spring Data JPA**:
  - Provides **CRUD operations** out-of-the-box.
  - Uses **JPA (Java Persistence API)** with **Hibernate** as the default ORM.
  - Works with relational databases like **MySQL** and **PostgreSQL**.

- **@Repository**:
  - Marks a class as a Data Access Layer (DAO).
  - **Spring Data JPA** generates the implementation for the repository interface.

---

**Example: UserRepository**
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
}
```

- **Explanation**:
  - **JpaRepository<User, Long>** provides CRUD operations for `User` entity.
  - `Long` is the type of the primary key.
  - Spring Data JPA automatically implements this interface.

---

### **2.4 Integration with MySQL/PostgreSQL using Spring Data JPA**
- **Dependency for Spring Data JPA**:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

- **Dependency for MySQL Driver**:
```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```

- **Dependency for PostgreSQL Driver**:
```xml
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
</dependency>
```

---

### **MySQL Configuration in application.properties**:
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/angular_springboot
spring.datasource.username=root
spring.datasource.password=yourpassword
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
```

---

### **PostgreSQL Configuration**:
```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/angular_springboot
spring.datasource.username=postgres
spring.datasource.password=yourpassword
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
```

---

## **3. Building CRUD Operations with Spring Data JPA**

### **3.1 Creating REST Endpoints for CRUD Operations**
- **GET** – Retrieve data
- **POST** – Create new data
- **PUT** – Update existing data
- **DELETE** – Delete data

**Example: UserController with CRUD Operations**
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }

    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.createUser(user);
    }

    @PutMapping("/{id}")
    public User updateUser(@PathVariable Long id, @RequestBody User user) {
        return userService.updateUser(id, user);
    }

    @DeleteMapping("/{id}")
    public void deleteUser(@PathVariable Long id) {
        userService.deleteUser(id);
    }
}
```

- **Explanation**:
  - **@Autowired** injects the UserService.
  - **@RequestBody** maps the request body to `User` object.
  - **@PathVariable** extracts `id` from the URI.

---

### **3.2 Request Validation and Exception Handling**
- Using **@Valid** for request validation.
- Creating **Custom Exception Handlers** with `@ControllerAdvice`.

---

## **Next Steps:**
- **Integrating Angular with Spring Boot APIs**
- **Hands-on Exercise: Building a Complete CRUD Feature**
