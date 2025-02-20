# **Phase 2 - Step 10: Securing Endpoints and Role-Based Authorization**

We now have **JWT Authentication** in place. The next step is to **secure endpoints** and **implement role-based authorization** to control access based on user roles (`USER`, `ADMIN`, etc.).

---

## **10.1. Role-Based Authorization Overview**  
- **Authorization** controls what authenticated users can do in the application.  
- We’ll secure endpoints by assigning roles (`USER`, `ADMIN`) and allowing or restricting access accordingly.  
- Example: Only `ADMIN` users can access the `/api/users/all` endpoint to list all users.

---

## **10.2. Update User Model with Role Enum**  
We will enhance the `User` model by adding role-based authorization using an `enum` for roles.

### **Step 1: Create Role Enum**  
**src/main/java/com/cashflowapp/userservice/model/Role.java**  
```java
package com.cashflowapp.userservice.model;

public enum Role {
    USER,
    ADMIN
}
```

---

### **Step 2: Update User Model**  
**src/main/java/com/cashflowapp/userservice/model/User.java**  
```java
package com.cashflowapp.userservice.model;

import jakarta.persistence.*;
import lombok.Data;

@Entity
@Data
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true)
    private String username;

    @Column(nullable = false)
    private String password;

    @Enumerated(EnumType.STRING)
    @Column(nullable = false)
    private Role role;
}
```

### **Change Summary**:  
- `role` is now an enum of type `Role`, ensuring only valid roles (`USER`, `ADMIN`) are saved.

---

## **10.3. Enhance JWT Token to Include Roles**  
Update `JwtTokenUtil` to include roles in the token and validate them during authorization.

**src/main/java/com/cashflowapp/userservice/security/JwtTokenUtil.java**  
```java
public String generateToken(String username, String role) {
    Map<String, Object> claims = new HashMap<>();
    claims.put("role", role);  // Adding role to claims
    return Jwts.builder()
            .setClaims(claims)
            .setSubject(username)
            .setIssuedAt(new Date(System.currentTimeMillis()))
            .setExpiration(new Date(System.currentTimeMillis() + expiration * 1000))
            .signWith(SignatureAlgorithm.HS256, secret)
            .compact();
}

public String getRoleFromToken(String token) {
    Claims claims = getAllClaimsFromToken(token);
    return (String) claims.get("role");
}
```

---

## **10.4. Implementing JWT Request Filter**  
We need to intercept each request to:
1. Extract the JWT token from the `Authorization` header.  
2. Validate the token and extract the role and username.  
3. Set the authentication context.

**src/main/java/com/cashflowapp/userservice/security/JwtRequestFilter.java**  
```java
package com.cashflowapp.userservice.security;

import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
import org.springframework.stereotype.Component;
import org.springframework.web.filter.OncePerRequestFilter;

import java.io.IOException;
import java.util.Collections;

@Component
public class JwtRequestFilter extends OncePerRequestFilter {

    @Autowired
    private JwtTokenUtil jwtTokenUtil;

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain chain)
            throws ServletException, IOException {

        final String requestTokenHeader = request.getHeader("Authorization");

        String username = null;
        String jwtToken = null;

        if (requestTokenHeader != null && requestTokenHeader.startsWith("Bearer ")) {
            jwtToken = requestTokenHeader.substring(7);
            username = jwtTokenUtil.getUsernameFromToken(jwtToken);

            if (username != null && SecurityContextHolder.getContext().getAuthentication() == null) {
                String role = jwtTokenUtil.getRoleFromToken(jwtToken);

                UsernamePasswordAuthenticationToken authToken =
                        new UsernamePasswordAuthenticationToken(username, null, Collections.emptyList());
                authToken.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));

                SecurityContextHolder.getContext().setAuthentication(authToken);
            }
        }
        chain.doFilter(request, response);
    }
}
```

---

## **10.5. Update Security Configuration**  
Now, we will update the `SecurityConfig` class to:  
1. Register `JwtRequestFilter` for processing requests.  
2. Secure endpoints based on roles.

**src/main/java/com/cashflowapp/userservice/security/SecurityConfig.java**  
```java
package com.cashflowapp.userservice.security;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Autowired
    private JwtRequestFilter jwtRequestFilter;

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .authorizeHttpRequests()
                .requestMatchers("/api/users/register", "/api/users/login").permitAll()
                .requestMatchers("/api/users/all").hasAuthority("ADMIN") // Only ADMIN can access
                .anyRequest().authenticated()
            .and()
            .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);

        http.addFilterBefore(jwtRequestFilter, UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }
}
```

---

## **10.6. Secure Endpoint - List All Users**  
Only `ADMIN` users can access the endpoint to list all users.

**src/main/java/com/cashflowapp/userservice/controller/UserController.java**  
```java
@GetMapping("/all")
public List<User> getAllUsers() {
    return userService.getAllUsers();
}
```

---

## **10.7. Test Role-Based Authorization**  
### **1. Register Users with Different Roles**  
```json
POST http://localhost:8081/api/users/register
{
  "username": "john_admin",
  "password": "admin123",
  "role": "ADMIN"
}
```
```json
POST http://localhost:8081/api/users/register
{
  "username": "jane_user",
  "password": "user123",
  "role": "USER"
}
```

### **2. Obtain Tokens**  
- Login with `john_admin` → Get a JWT token for the `ADMIN` role.  
- Login with `jane_user` → Get a JWT token for the `USER` role.

### **3. Access Protected Endpoint**  
**Endpoint**: `GET http://localhost:8081/api/users/all`  
- **With ADMIN Token**: Should return a list of users.  
- **With USER Token**: Should return a `403 Forbidden` error.  

**Example Authorization Header**:  
```text
Authorization: Bearer <JWT-TOKEN>
```

---

## **Next Step**  
1. Test the role-based authorization using **Postman**.  
2. Once verified, we’ll move to the next microservice – **Course Service**.  
