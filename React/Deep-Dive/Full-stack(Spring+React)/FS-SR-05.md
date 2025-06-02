## 📘 Phase 2 – Lesson 1: JWT Authentication with Spring Boot (Login, Register, Token, Roles)

---

### 🎯 Goal:

Implement complete **authentication flow**:

* Register new users
* Login and return a **JWT token**
* Secure endpoints (only accessible with token)
* Store user roles (ADMIN, USER)

---

## 🧱 Project Layers Involved

```
📦 /auth/
├── controller/
├── service/
├── entity/User.java
├── security/JwtFilter.java
├── config/SecurityConfig.java
├── util/JwtUtil.java
```

---

## 🧩 Step 1: Add Required Dependencies

📄 `pom.xml`

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
  <groupId>io.jsonwebtoken</groupId>
  <artifactId>jjwt-api</artifactId>
  <version>0.11.5</version>
</dependency>
<dependency>
  <groupId>io.jsonwebtoken</groupId>
  <artifactId>jjwt-impl</artifactId>
  <version>0.11.5</version>
  <scope>runtime</scope>
</dependency>
<dependency>
  <groupId>io.jsonwebtoken</groupId>
  <artifactId>jjwt-jackson</artifactId>
  <version>0.11.5</version>
  <scope>runtime</scope>
</dependency>
```

---

## 🧩 Step 2: Create User Entity

📄 `User.java`

```java
@Entity
@Table(name = "users")
public class User {
  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;

  @Column(nullable = false, unique = true)
  private String email;

  private String password;

  @Enumerated(EnumType.STRING)
  private Role role;
}

public enum Role {
  ADMIN, USER
}
```

---

## 🧩 Step 3: Create DTOs

📄 `dto/AuthRequest.java`

```java
public class AuthRequest {
  @NotBlank private String email;
  @NotBlank private String password;
}
```

📄 `dto/AuthResponse.java`

```java
public class AuthResponse {
  private String token;
  private String role;
  public AuthResponse(String token, String role) {
    this.token = token;
    this.role = role;
  }
}
```

---

## 🧩 Step 4: Create JwtUtil.java

📄 `util/JwtUtil.java`

```java
@Component
public class JwtUtil {
  @Value("${jwt.secret}")
  private String secret;

  public String generateToken(String email) {
    return Jwts.builder()
      .setSubject(email)
      .setIssuedAt(new Date())
      .setExpiration(new Date(System.currentTimeMillis() + 86400000)) // 1 day
      .signWith(Keys.hmacShaKeyFor(secret.getBytes()), SignatureAlgorithm.HS256)
      .compact();
  }

  public String extractEmail(String token) {
    return Jwts.parserBuilder()
      .setSigningKey(secret.getBytes())
      .build()
      .parseClaimsJws(token)
      .getBody()
      .getSubject();
  }
}
```

📄 `application.yml`

```yaml
jwt:
  secret: your-very-secret-jwt-key-which-must-be-at-least-256-bit
```

---

## 🧩 Step 5: Create AuthController

```java
@RestController
@RequestMapping("/api/auth")
public class AuthController {

  @Autowired private UserRepository userRepo;
  @Autowired private PasswordEncoder encoder;
  @Autowired private JwtUtil jwtUtil;

  @PostMapping("/register")
  public ResponseEntity<?> register(@RequestBody AuthRequest req) {
    User user = new User();
    user.setEmail(req.getEmail());
    user.setPassword(encoder.encode(req.getPassword()));
    user.setRole(Role.USER);
    return ResponseEntity.ok(userRepo.save(user));
  }

  @PostMapping("/login")
  public ResponseEntity<?> login(@RequestBody AuthRequest req) {
    User user = userRepo.findByEmail(req.getEmail()).orElseThrow();
    if (encoder.matches(req.getPassword(), user.getPassword())) {
      String token = jwtUtil.generateToken(user.getEmail());
      return ResponseEntity.ok(new AuthResponse(token, user.getRole().name()));
    }
    return ResponseEntity.status(HttpStatus.UNAUTHORIZED).build();
  }
}
```

---

## 🧩 Step 6: Add Security Configuration

📄 `SecurityConfig.java`

```java
@Configuration
@EnableMethodSecurity
public class SecurityConfig {

  @Bean
  public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    http.csrf().disable()
      .authorizeHttpRequests()
      .requestMatchers("/api/auth/**").permitAll()
      .anyRequest().authenticated()
      .and()
      .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);
    return http.build();
  }

  @Bean
  public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
  }
}
```

---

✅ You now have:

* `/api/auth/register` → Create user
* `/api/auth/login` → Returns JWT token
* Global password encryption
* Role stored per user

---

## 🧪 Test Authentication Flow

1. ✅ `POST /api/auth/register` → with `email` and `password`
2. ✅ `POST /api/auth/login` → get JWT token
3. ✅ Add token to Postman:

   ```
   Authorization: Bearer <token>
   ```
4. ✅ Secure any endpoint (like `/api/users`) by adding:

   ```java
   @PreAuthorize("hasRole('ADMIN')")
   ```

---

## 🧠 Summary

| Feature          | Result                   |
| ---------------- | ------------------------ |
| User auth        | Register + Login working |
| Password hashing | Securely stored          |
| JWT tokens       | Stateless auth           |
| Roles            | Added per user           |
| Spring Security  | Enabled globally         |

---

## 🎯 What’s Next?

👉 **Phase 2 – Lesson 2: Frontend Integration of JWT Login + Token Handling + Protected Routes**

You’ll:

* Build React login form (Formik + Yup)
* Store token in Redux or Zustand
* Send token with Axios
* Protect frontend routes with `<PrivateRoute />`

