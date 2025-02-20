# **Phase 2 - Step 9: Implementing JWT Authentication**  

We will now enhance the **User Service** by adding **JWT Authentication** to secure our APIs. This involves:  
1. **User Registration**: Allowing users to register.  
2. **User Login**: Authenticating users and issuing JWT tokens.  
3. **Token Validation**: Securing endpoints to allow access only to authenticated users.

---

## **9.1. Understanding JWT Authentication**  
- **JWT (JSON Web Token)** is a compact, URL-safe token used for securely transmitting information between parties.  
- It consists of three parts:
  - **Header**: Specifies the algorithm used (e.g., HS256).  
  - **Payload**: Contains claims (e.g., username, role).  
  - **Signature**: Verifies the token's authenticity and integrity.  

### **Token Structure Example**  
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJ1c2VyMSIsInJvbGUiOiJVU0VSIiwiaWF0IjoxNjE1NzY5MDAwLCJleHAiOjE2MTU3NzI2MDB9.abc123signature
```

---

## **9.2. Dependencies Required**  
Add the following dependencies to your `pom.xml` file:  
```xml
<!-- JWT Dependencies -->
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
</dependency>
```

---

## **9.3. Create JWT Utility Class**  
**src/main/java/com/cashflowapp/userservice/security/JwtTokenUtil.java**  
```java
package com.cashflowapp.userservice.security;

import io.jsonwebtoken.*;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

import java.util.Date;
import java.util.HashMap;
import java.util.Map;
import java.util.function.Function;

@Component
public class JwtTokenUtil {

    @Value("${jwt.secret}")
    private String secret;

    @Value("${jwt.expiration}")
    private Long expiration;

    public String generateToken(String username, String role) {
        Map<String, Object> claims = new HashMap<>();
        claims.put("role", role);
        return Jwts.builder()
                .setClaims(claims)
                .setSubject(username)
                .setIssuedAt(new Date(System.currentTimeMillis()))
                .setExpiration(new Date(System.currentTimeMillis() + expiration * 1000))
                .signWith(SignatureAlgorithm.HS256, secret)
                .compact();
    }

    public String getUsernameFromToken(String token) {
        return getClaimFromToken(token, Claims::getSubject);
    }

    public String getRoleFromToken(String token) {
        Claims claims = getAllClaimsFromToken(token);
        return (String) claims.get("role");
    }

    public Date getExpirationDateFromToken(String token) {
        return getClaimFromToken(token, Claims::getExpiration);
    }

    public Boolean validateToken(String token, String username) {
        final String extractedUsername = getUsernameFromToken(token);
        return (extractedUsername.equals(username) && !isTokenExpired(token));
    }

    private Boolean isTokenExpired(String token) {
        final Date expiration = getExpirationDateFromToken(token);
        return expiration.before(new Date());
    }

    private <T> T getClaimFromToken(String token, Function<Claims, T> claimsResolver) {
        final Claims claims = getAllClaimsFromToken(token);
        return claimsResolver.apply(claims);
    }

    private Claims getAllClaimsFromToken(String token) {
        return Jwts.parser()
                .setSigningKey(secret)
                .parseClaimsJws(token)
                .getBody();
    }
}
```

---

## **9.4. Add Security Configuration**  
**src/main/java/com/cashflowapp/userservice/security/SecurityConfig.java**  
```java
package com.cashflowapp.userservice.security;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .authorizeHttpRequests()
            .requestMatchers("/api/users/register", "/api/users/login").permitAll()
            .anyRequest().authenticated()
            .and()
            .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);
        
        return http.build();
    }
}
```

---

## **9.5. Add Login Endpoint**  
**src/main/java/com/cashflowapp/userservice/controller/AuthController.java**  
```java
package com.cashflowapp.userservice.controller;

import com.cashflowapp.userservice.model.User;
import com.cashflowapp.userservice.security.JwtTokenUtil;
import com.cashflowapp.userservice.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.HashMap;
import java.util.Map;

@RestController
@RequestMapping("/api/users")
public class AuthController {

    @Autowired
    private UserService userService;

    @Autowired
    private JwtTokenUtil jwtTokenUtil;

    @PostMapping("/login")
    public Map<String, String> login(@RequestBody User user) {
        User existingUser = userService.findByUsername(user.getUsername()).orElse(null);
        if (existingUser != null && existingUser.getPassword().equals(user.getPassword())) {
            String token = jwtTokenUtil.generateToken(existingUser.getUsername(), existingUser.getRole());
            Map<String, String> response = new HashMap<>();
            response.put("token", token);
            return response;
        }
        throw new RuntimeException("Invalid credentials");
    }
}
```

---

## **9.6. Update application.properties**  
**src/main/resources/application.properties**  
```properties
# JWT Configuration
jwt.secret=MySecretKey
jwt.expiration=3600
```

---

## **9.7. Test the Authentication Flow**  
1. **Register a User**  
```json
POST http://localhost:8081/api/users/register
{
  "username": "john_doe",
  "password": "password123",
  "role": "USER"
}
```

2. **Login and Obtain JWT Token**  
```json
POST http://localhost:8081/api/users/login
{
  "username": "john_doe",
  "password": "password123"
}
```
- This will return a JWT token.

3. **Access Protected Endpoint**  
- Use the token in the Authorization header:  
```text
Authorization: Bearer <JWT-TOKEN>
```

---

## **Next Step**  
1. Test the JWT Authentication using **Postman**.  
2. Once verified, we’ll move to **Securing Endpoints and Implementing Role-Based Authorization**.  
