## 📘 Phase 1 – Lesson 4: PostgreSQL Integration + Flyway + DTOs + Validation

---

### 🎯 Goal:

* Migrate DB schema using **Flyway**
* Secure your API using **DTOs**
* Add **field-level validation** with annotations
* Clean up your config with `application.yml`

---

## 🧩 Step 1: Convert `application.properties` → `application.yml`

📄 `application.yml`

```yaml
server:
  port: 8080

spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/userdb
    username: postgres
    password: yourPassword
  jpa:
    hibernate:
      ddl-auto: validate
    show-sql: true
    properties:
      hibernate:
        format_sql: true
  flyway:
    enabled: true
    locations: classpath:db/migration
```

✅ Now the app won’t auto-generate schema — **Flyway will handle it.**

---

## 🧩 Step 2: Add Flyway Migration

📁 Folder:

```
src/main/resources/db/migration/
```

📄 `V1__create_users_table.sql`

```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(100) UNIQUE NOT NULL
);
```

✅ On app start, Flyway will auto-run this script and version it.

---

## 🧩 Step 3: Use DTO Pattern (Data Transfer Object)

### 🔍 Why?

> Prevent exposing the entire Entity (like passwords, IDs) and enforce control over request/response data.

📄 `dto/UserRequestDTO.java`

```java
public class UserRequestDTO {
    @NotBlank
    private String name;

    @Email
    @NotBlank
    private String email;

    // getters & setters
}
```

📄 `dto/UserResponseDTO.java`

```java
public class UserResponseDTO {
    private Long id;
    private String name;
    private String email;

    // constructor
    public UserResponseDTO(User user) {
        this.id = user.getId();
        this.name = user.getName();
        this.email = user.getEmail();
    }

    // getters
}
```

---

## 🧩 Step 4: Update Controller to Use DTO

📄 `UserController.java`

```java
@PostMapping
public ResponseEntity<UserResponseDTO> createUser(@Valid @RequestBody UserRequestDTO dto) {
    User user = new User();
    user.setName(dto.getName());
    user.setEmail(dto.getEmail());
    return ResponseEntity.ok(new UserResponseDTO(userRepo.save(user)));
}
```

✅ Do the same for PUT/update. Return `UserResponseDTO`, not `User`.

---

## 🛑 Step 5: Add Global Validation Handler (optional)

📄 `GlobalExceptionHandler.java`

```java
@ControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<Map<String, String>> handleValidation(MethodArgumentNotValidException ex) {
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getFieldErrors().forEach(err ->
            errors.put(err.getField(), err.getDefaultMessage()));
        return ResponseEntity.badRequest().body(errors);
    }
}
```

✅ Now invalid requests return clean error JSON.

---

## 🧪 Test Cases:

✅ POST `/api/users` with:

```json
{
  "name": "",
  "email": "not-an-email"
}
```

Should return:

```json
{
  "name": "must not be blank",
  "email": "must be a well-formed email address"
}
```

---

## ✅ Summary

| Task             | Outcome                  |
| ---------------- | ------------------------ |
| Switched to YAML | Cleaner config           |
| Used Flyway      | Controlled DB migrations |
| Introduced DTOs  | Safe API boundaries      |
| Added validation | Prevents invalid data    |

---

## 🎯 What’s Next?

👉 **Phase 2 – Lesson 1: JWT Authentication with Spring Security (Login, Register, Token, Roles)**

We’ll:

* Add Spring Security
* Implement JWT token-based login & register
* Store user passwords securely
* Protect API routes with roles
