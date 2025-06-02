## 📘 Phase 1 – Lesson 3: RESTful API Design + Swagger Integration + Test Controller

---

### 🎯 Goal:

* Learn best practices for REST API naming and structure
* Create your first working `UserController`
* Integrate **Swagger** to document your APIs
* Test APIs using Swagger UI & Postman

---

## 🧩 Step 1: Follow REST API Design Best Practices

| Method   | Purpose         | Endpoint          |
| -------- | --------------- | ----------------- |
| `GET`    | Fetch data      | `/api/users`      |
| `GET`    | Fetch by ID     | `/api/users/{id}` |
| `POST`   | Create new      | `/api/users`      |
| `PUT`    | Update existing | `/api/users/{id}` |
| `DELETE` | Delete          | `/api/users/{id}` |

> Keep base route as plural nouns (e.g., `/api/users`, not `/api/user`)

---

## ⚙️ Step 2: Add Swagger (OpenAPI) Support

### ✅ Add Dependency (if not already)

📄 `pom.xml`

```xml
<dependency>
  <groupId>org.springdoc</groupId>
  <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
  <version>2.2.0</version>
</dependency>
```

> 🔁 Run `mvn clean install` after adding.

---

### ✅ Swagger UI URL:

```
http://localhost:8080/swagger-ui/index.html
```

---

## 🧪 Step 3: Create a Test User Entity & Controller

📄 `entity/User.java`

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String email;
}
```

📄 `repository/UserRepository.java`

```java
public interface UserRepository extends JpaRepository<User, Long> { }
```

📄 `controller/UserController.java`

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @Autowired
    private UserRepository userRepo;

    @GetMapping
    public List<User> getAllUsers() {
        return userRepo.findAll();
    }

    @PostMapping
    public User createUser(@RequestBody User user) {
        return userRepo.save(user);
    }

    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        return userRepo.findById(id)
            .map(ResponseEntity::ok)
            .orElse(ResponseEntity.notFound().build());
    }

    @PutMapping("/{id}")
    public ResponseEntity<User> updateUser(@PathVariable Long id, @RequestBody User updated) {
        return userRepo.findById(id)
            .map(user -> {
                user.setName(updated.getName());
                user.setEmail(updated.getEmail());
                return ResponseEntity.ok(userRepo.save(user));
            })
            .orElse(ResponseEntity.notFound().build());
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<?> deleteUser(@PathVariable Long id) {
        userRepo.deleteById(id);
        return ResponseEntity.noContent().build();
    }
}
```

---

## 📄 application.properties

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/userdb
spring.datasource.username=postgres
spring.datasource.password=yourPassword
spring.jpa.hibernate.ddl-auto=update
springdoc.swagger-ui.path=/swagger-ui.html
```

---

## ✅ Step 4: Test Using Swagger UI

➡ Open: `http://localhost:8080/swagger-ui/index.html`

✅ You should see:

* GET `/api/users`
* POST `/api/users`
* GET `/api/users/{id}`
* PUT `/api/users/{id}`
* DELETE `/api/users/{id}`

---

## 🧪 Step 5: Test with Postman (Optional)

Test:

* `POST /api/users` with JSON body:

```json
{
  "name": "Tushar",
  "email": "tushar@example.com"
}
```

* `GET /api/users`
* `PUT /api/users/1`
* `DELETE /api/users/1`

---

## 🧠 Summary

| What You Did            | Why It Matters                     |
| ----------------------- | ---------------------------------- |
| Designed REST APIs      | Standardizes backend communication |
| Added Swagger           | Live API documentation & testing   |
| Built UserController    | Complete CRUD endpoint flow        |
| Connected to PostgreSQL | Real persistence layer             |

---

## 🎯 What’s Next?

👉 **Phase 1 – Lesson 4: PostgreSQL Integration + Flyway Migration Scripts + DTOs & Validation**

You’ll:

* Replace `application.properties` with `application.yml`
* Add **Flyway** for DB migrations
* Use **DTOs** to transfer data safely
* Add **validation** annotations to your request payloads
