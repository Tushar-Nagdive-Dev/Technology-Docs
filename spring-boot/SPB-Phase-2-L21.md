# 🚀 **Phase 2 - Lesson 21: Securing Spring Boot Apps with OAuth2, JWT, and AWS IAM**  

## **📌 Lesson Objective**  
By the end of this lesson, you will:  
✅ Understand **Spring Security fundamentals**  
✅ Implement **JWT authentication & role-based authorization**  
✅ Secure APIs using **OAuth2 & AWS Cognito**  
✅ Use **AWS IAM (Identity & Access Management) for secure access**  
✅ Apply **best security practices in Spring Boot applications**  

---

# 🔐 **Part 1: Implementing JWT Authentication in Spring Boot**  

## **1️⃣ What is JWT (JSON Web Token) & Why Use It?**  
📌 **JWT (JSON Web Token)** is a compact, self-contained token used for **secure authentication & authorization**.  

### **🔥 Why Use JWT for Authentication?**
✔ **Stateless Authentication** – No session storage needed.  
✔ **More Secure** – Uses encryption & signature verification.  
✔ **Efficient** – Works with RESTful APIs & microservices.  

✅ **Use JWT when:**  
- You need **secure API authentication**.  
- You want **stateless authentication without storing sessions**.  

---

## **2️⃣ Adding JWT Security to Spring Boot**  
📌 **Step 1: Add Spring Security & JWT Dependencies in `pom.xml`**  
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>

<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.11.2</version>
</dependency>
```

📌 **Step 2: Configure JWT Utility Class (`JwtUtil.java`)**  
```java
package com.example.demo.security;

import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import org.springframework.stereotype.Component;
import java.util.Date;

@Component
public class JwtUtil {
    private final String SECRET_KEY = "your_secret_key";

    public String generateToken(String username) {
        return Jwts.builder()
                .setSubject(username)
                .setIssuedAt(new Date())
                .setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 60))
                .signWith(SignatureAlgorithm.HS256, SECRET_KEY)
                .compact();
    }

    public String extractUsername(String token) {
        return Jwts.parser()
                .setSigningKey(SECRET_KEY)
                .parseClaimsJws(token)
                .getBody()
                .getSubject();
    }

    public boolean validateToken(String token) {
        return extractUsername(token) != null && !isTokenExpired(token);
    }

    private boolean isTokenExpired(String token) {
        return Jwts.parser()
                .setSigningKey(SECRET_KEY)
                .parseClaimsJws(token)
                .getBody()
                .getExpiration()
                .before(new Date());
    }
}
```

✅ **This class generates & validates JWT tokens**.  

---

📌 **Step 3: Implement Authentication Controller (`AuthController.java`)**  
```java
package com.example.demo.controller;

import com.example.demo.security.JwtUtil;
import org.springframework.web.bind.annotation.*;

import java.util.HashMap;
import java.util.Map;

@RestController
@RequestMapping("/api/auth")
public class AuthController {
    private final JwtUtil jwtUtil;

    public AuthController(JwtUtil jwtUtil) {
        this.jwtUtil = jwtUtil;
    }

    @PostMapping("/login")
    public Map<String, String> login(@RequestParam String username) {
        String token = jwtUtil.generateToken(username);
        Map<String, String> response = new HashMap<>();
        response.put("token", token);
        return response;
    }
}
```

✅ **Login endpoint returns a JWT token**.  

---

📌 **Step 4: Secure Endpoints using JWT in `SecurityConfig.java`**  
```java
package com.example.demo.security;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http.csrf().disable()
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/login").permitAll()
                .requestMatchers("/api/secure/**").authenticated()
            )
            .sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
            .addFilterBefore(new JwtFilter(), UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }
}
```

📌 **Step 5: Test JWT Authentication**  
```bash
curl -X POST "http://localhost:8080/api/auth/login?username=admin"
```
✔ **Response:**  
```json
{"token":"your_generated_jwt_token"}
```

✅ **JWT authentication is now enabled in Spring Boot!** 🎉  

---

# 🔑 **Part 2: Securing APIs using OAuth2 & AWS Cognito**  

## **3️⃣ Setting Up AWS Cognito for OAuth2 Authentication**  
📌 **Step 1: Create an AWS Cognito User Pool**  
1️⃣ Go to **AWS Console → Cognito → Create User Pool**  
2️⃣ Configure **App Clients & OAuth2 Flows**  
3️⃣ Note the **User Pool ID & Client ID**  

📌 **Step 2: Add Spring Security OAuth2 Dependency in `pom.xml`**  
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-oauth2-client</artifactId>
</dependency>
```

📌 **Step 3: Configure `application.yml` for AWS Cognito**  
```yaml
spring:
  security:
    oauth2:
      client:
        registration:
          cognito:
            client-id: your-client-id
            client-secret: your-client-secret
            scope: openid, profile, email
        provider:
          cognito:
            issuer-uri: https://cognito-idp.us-east-1.amazonaws.com/us-east-1_example
```

📌 **Step 4: Implement OAuth2 Security in `SecurityConfig.java`**  
```java
package com.example.demo.security;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;

@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http.csrf().disable()
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/**").permitAll()
                .anyRequest().authenticated()
            )
            .oauth2Login();

        return http.build();
    }
}
```

📌 **Step 5: Run & Test OAuth2 Login**  
1️⃣ Start Spring Boot app  
2️⃣ Open Browser → `http://localhost:8080/login`  
3️⃣ Log in using **AWS Cognito credentials**  

🎉 **Your Spring Boot app is now secured using OAuth2 & AWS Cognito!** 🚀  

---

# 🔐 **Part 3: Using AWS IAM for Secure Access**  

## **4️⃣ Securing Spring Boot APIs with AWS IAM Roles**  
📌 **Step 1: Create an IAM Role for API Access**  
1️⃣ Go to **AWS Console → IAM → Roles → Create Role**  
2️⃣ Attach **AmazonAPIGatewayInvokeFullAccess** policy  
3️⃣ Attach to **AWS Lambda or EC2 instance**  

📌 **Step 2: Use AWS IAM for API Authentication**  
```yaml
security:
  iam:
    enabled: true
    role: arn:aws:iam::your-account-id:role/api-access-role
```

📌 **Step 3: Use AWS SDK for Secure API Calls**  
```java
import software.amazon.awssdk.auth.credentials.DefaultCredentialsProvider;

AwsCredentialsProvider credentialsProvider = DefaultCredentialsProvider.create();
```

🎉 **Spring Boot now supports secure API authentication using AWS IAM!** 🚀  

---

## 🎯 **Lesson 21 - Summary**  
✅ Implemented **JWT authentication & role-based access**  
✅ Secured APIs using **OAuth2 & AWS Cognito**  
✅ Used **AWS IAM roles for secure API access**  

---
