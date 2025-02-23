---

## **Lesson 72: Building RESTful APIs with Spring Boot and Integrating with Angular**

---

## **What You Will Learn:**
1. **Introduction to RESTful APIs with Spring Boot**
   - What are RESTful APIs?
   - Why Use Spring Boot for RESTful APIs?
   - Key Annotations and Concepts in Spring Boot REST APIs

2. **Setting up Controllers, Services, and Repositories in Spring Boot**
   - Creating REST Controllers with `@RestController`
   - Service Layer for Business Logic with `@Service`
   - Data Access with Spring Data JPA and `@Repository`
   - Integration with MySQL/PostgreSQL using Spring Data JPA

3. **Building CRUD Operations with Spring Data JPA**
   - Creating REST Endpoints for CRUD Operations
   - `GET`, `POST`, `PUT`, `DELETE` Mappings in Spring Boot
   - Request Validation and Exception Handling
   - Response Structure and Standardized Error Responses

4. **Integrating Angular with Spring Boot APIs**
   - Using Angular's HttpClient Module for API Integration
   - Making HTTP Requests (GET, POST, PUT, DELETE)
   - Handling HTTP Errors in Angular
   - Displaying Data with Angular Components and Services

5. **Hands-on Exercise: Building a Complete CRUD Feature**
   - Spring Boot Backend with CRUD Endpoints
   - Angular Frontend with CRUD UI and Integration
   - Full-Stack CRUD Application with Angular + Spring Boot

6. **Expert Insights and Best Practices**
7. **Common Mistakes to Avoid**
8. **Recap and Next Steps**

---

## **1. Introduction to RESTful APIs with Spring Boot**

### **1.1 What are RESTful APIs?**
- **RESTful APIs** (Representational State Transfer):
  - Architectural style for designing networked applications.
  - Based on **HTTP methods**: `GET`, `POST`, `PUT`, `DELETE`.
  - **Stateless** communication between client and server.
  - Data is typically exchanged in **JSON** or **XML** format.

---

### **1.2 Why Use Spring Boot for RESTful APIs?**
- **Spring Boot** is a powerful framework for building RESTful APIs because:
  - **Rapid Development**: Quick setup with Spring Initializr.
  - **Easy Integration**: Built-in support for JPA, Hibernate, Security.
  - **Scalable Architecture**: Microservices architecture with Spring Cloud.
  - **High Performance**: Optimized for enterprise-level applications.
  - **Security**: Integration with Spring Security for authentication and authorization.

---

### **1.3 Key Annotations and Concepts in Spring Boot REST APIs**
- **@RestController** – Defines a REST controller.
- **@RequestMapping** – Maps HTTP requests to handler methods.
- **@GetMapping, @PostMapping, @PutMapping, @DeleteMapping** – Shortcuts for HTTP methods.
- **@RequestBody** – Binds the body of the HTTP request to a method parameter.
- **@PathVariable** – Extracts values from the URI.
- **@RequestParam** – Extracts query parameters from the URI.
- **@ResponseStatus** – Customizes the HTTP status code of the response.
- **@ExceptionHandler** – Handles exceptions in a controller.

---

## **2. Setting up Controllers, Services, and Repositories in Spring Boot**

### **2.1 Creating REST Controllers with @RestController**
- **@RestController** is a combination of `@Controller` and `@ResponseBody`.
- Used to create RESTful web services.
- Returns data directly in **JSON** format using **Jackson**.

**Example: Simple REST Controller**
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

---

### **2.2 Service Layer for Business Logic with @Service**
- **@Service** annotation is used for business logic.
- **Decouples business logic** from the controller.
- **Promotes modularity and reusability**.

**Example: User Service**
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

---

### **2.3 Data Access with Spring Data JPA and @Repository**
- **@Repository** annotation is used for the Data Access Layer.
- **Spring Data JPA** provides default implementations for CRUD operations.
- Works with **Hibernate** as the ORM (Object-Relational Mapping).

**Example: User Repository**
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
}
```

---

### **2.4 Integration with MySQL/PostgreSQL using Spring Data JPA**
- **MySQL Configuration in application.properties**:
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/angular_springboot
spring.datasource.username=root
spring.datasource.password=yourpassword
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
```

- **PostgreSQL Configuration**:
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

**Example: User Controller with CRUD Operations**
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

---

### **3.2 Request Validation and Exception Handling**
- Using **@Valid** for request validation.
- Creating **Custom Exception Handlers** with `@ControllerAdvice`.

---

### **3.3 Response Structure and Standardized Error Responses**
- Returning consistent **JSON responses**.
- Custom **ResponseEntity** for HTTP status codes.

---

## **4. Integrating Angular with Spring Boot APIs**

### **4.1 Using Angular's HttpClient Module for API Integration**
- Making HTTP Requests (GET, POST, PUT, DELETE).
- Configuring **HttpClientModule** in `app.module.ts`.
- Handling HTTP Errors with **HttpErrorResponse**.

---

### **4.2 Displaying Data with Angular Components and Services**
- Creating Angular **Services** for API communication.
- Using **Observable pattern** for data flow.
- Displaying data with **ngFor** and **ngIf** in components.

---

## **5. Hands-on Exercise: Building a Complete CRUD Feature**
- **Spring Boot Backend**:
  - Setting up **CRUD endpoints** for User management.
- **Angular Frontend**:
  - Creating **Angular Service** for User APIs.
  - Creating **Angular Components** for CRUD operations.
- **Full-Stack Integration**:
  - Displaying data in Angular UI.
  - Implementing **Add, Edit, Delete** functionality.

---

## **Next Steps:**
- **Lesson 73: JWT Authentication and Authorization**
  - Implementing JWT Security with Spring Boot
  - Integrating JWT Authentication with Angular
  - Role-Based Authorization and Secure API Calls
