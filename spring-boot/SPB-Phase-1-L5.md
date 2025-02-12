# 🚀 **Phase 1 - Lesson 5: Implementing Validation & Exception Handling in Spring Boot** 🚀  

## **📌 Lesson Objective**
By the end of this lesson, you will:  
✅ Learn how to **validate request data** using **Spring Boot validation**  
✅ Implement **custom validation messages**  
✅ Understand **exception handling in REST APIs**  
✅ Implement **global exception handling** using `@ControllerAdvice`  
✅ Handle **custom exceptions with meaningful error responses**  

---

## **1️⃣ Why Validation is Important?**
Data validation ensures:  
✔️ **Correct & expected input values**  
✔️ **Prevention of invalid or incomplete data in the database**  
✔️ **Security against malicious inputs (e.g., SQL Injection, XSS attacks)**  

For example, when adding a **user**, we want to:
- **Ensure the name is not empty**
- **Ensure the email is in a valid format**
- **Ensure the password has at least 6 characters**

Spring Boot provides **built-in validation** using the **Jakarta Validation API (`jakarta.validation`)**.

---

## **2️⃣ Adding Validation to the User Entity**
📌 Modify the `User` model and **add validation annotations**:

```java
package com.example.demo.model;

import jakarta.persistence.*;
import jakarta.validation.constraints.Email;
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.Size;

@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @NotBlank(message = "Name is required") // Validation: Name cannot be empty
    private String name;

    @NotBlank(message = "Email is required")
    @Email(message = "Invalid email format") // Validation: Must be a valid email
    private String email;

    @NotBlank(message = "Password is required")
    @Size(min = 6, message = "Password must be at least 6 characters long") // Validation: Minimum 6 characters
    private String password;

    public User() {}

    public User(String name, String email, String password) {
        this.name = name;
        this.email = email;
        this.password = password;
    }

    // Getters and Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }

    public String getPassword() { return password; }
    public void setPassword(String password) { this.password = password; }
}
```

✅ **Validation Annotations Used:**
- `@NotBlank(message = "message")` → Ensures the field is **not empty or null**  
- `@Email(message = "message")` → Ensures a **valid email format**  
- `@Size(min = 6, message = "message")` → Ensures **minimum length**  

---

## **3️⃣ Applying Validation in the Controller**
📌 Modify `UserController` to validate input using `@Valid`:

```java
package com.example.demo.controller;

import com.example.demo.model.User;
import com.example.demo.service.UserService;
import jakarta.validation.Valid;
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

    private final UserService userService;

    @Autowired
    public UserController(UserService userService) {
        this.userService = userService;
    }

    // CREATE User with Validation
    @PostMapping
    public ResponseEntity<?> createUser(@Valid @RequestBody User user, BindingResult result) {
        if (result.hasErrors()) {
            // Collect validation errors
            List<String> errors = result.getFieldErrors()
                .stream()
                .map(error -> error.getField() + ": " + error.getDefaultMessage())
                .collect(Collectors.toList());

            return ResponseEntity.badRequest().body(errors);
        }
        return ResponseEntity.ok(userService.createUser(user));
    }

    // READ all users
    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }

    // READ User by ID
    @GetMapping("/{id}")
    public ResponseEntity<?> getUserById(@PathVariable Long id) {
        Optional<User> user = userService.getUserById(id);
        return user.map(ResponseEntity::ok).orElseGet(() -> ResponseEntity.notFound().build());
    }
}
```

✅ **What’s happening?**
- **`@Valid`** → Triggers validation based on the model's constraints.
- **`BindingResult`** → Captures validation errors.
- **If validation fails**, a **list of errors is returned** instead of proceeding.

---

## **4️⃣ Custom Exception Handling in Spring Boot**
### **Why Handle Exceptions?**
- REST APIs should return **meaningful error messages** instead of Java stack traces.
- We should handle **404 (Not Found), 400 (Bad Request), and other HTTP status codes** properly.

---

### **5️⃣ Global Exception Handling using `@ControllerAdvice`**
📌 Create a new package **`com.example.demo.exception`** and add `GlobalExceptionHandler`:

```java
package com.example.demo.exception;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.validation.FieldError;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseStatus;
import org.springframework.web.bind.annotation.RestControllerAdvice;

import java.util.HashMap;
import java.util.Map;

@RestControllerAdvice // Global exception handler
public class GlobalExceptionHandler {

    // Handling validation errors
    @ExceptionHandler(MethodArgumentNotValidException.class)
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    public Map<String, String> handleValidationExceptions(MethodArgumentNotValidException ex) {
        Map<String, String> errors = new HashMap<>();
        for (FieldError error : ex.getBindingResult().getFieldErrors()) {
            errors.put(error.getField(), error.getDefaultMessage());
        }
        return errors;
    }

    // Handling generic exceptions
    @ExceptionHandler(Exception.class)
    public ResponseEntity<?> handleGenericException(Exception ex) {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(ex.getMessage());
    }
}
```

✅ **What’s happening?**
- `@RestControllerAdvice` → A **global exception handler** for all controllers.
- `@ExceptionHandler(MethodArgumentNotValidException.class)` → **Handles validation errors**.
- `@ExceptionHandler(Exception.class)` → **Handles generic exceptions**.
- **Validation errors** are returned in a structured JSON format.

---

## **6️⃣ Testing Validation & Exception Handling**
### **Step 1: Run the Application**
```bash
mvn spring-boot:run
```

### **Step 2: Test API in Postman**
1️⃣ **Create User with Invalid Data**  
   - **POST `http://localhost:8080/api/users`**
   - Send this JSON:
   ```json
   {
     "name": "",
     "email": "invalidemail",
     "password": "123"
   }
   ```
   - **Response:**
   ```json
   {
     "name": "Name is required",
     "email": "Invalid email format",
     "password": "Password must be at least 6 characters long"
   }
   ```

2️⃣ **Try Fetching a User that Doesn’t Exist**
   - **GET `http://localhost:8080/api/users/999`**
   - **Response:** `404 Not Found`

3️⃣ **Try Sending Invalid JSON**
   - **POST `http://localhost:8080/api/users`**
   - Send this JSON:
   ```json
   {
     "name": "John"
   }
   ```
   - **Response:**
   ```json
   {
     "email": "Email is required",
     "password": "Password is required"
   }
   ```

🎉 **Validation and Exception Handling are now implemented!**

---

## 🎯 **Lesson 5 - Summary**
✅ Used **Spring Boot validation** with `@NotBlank`, `@Email`, `@Size`  
✅ Implemented **global exception handling** with `@ControllerAdvice`  
✅ Handled **custom validation errors**  
✅ Tested **error responses using Postman**  

---
