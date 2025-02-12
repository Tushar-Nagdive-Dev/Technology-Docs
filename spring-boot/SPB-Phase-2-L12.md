# 🚀 **Phase 2 - Lesson 12: Securing WebSockets with Spring Security** 🔐  

## **📌 Lesson Objective**  
By the end of this lesson, you will:  
✅ Learn why **WebSocket security** is important  
✅ Secure **WebSocket connections using Spring Security**  
✅ Implement **JWT-based authentication for WebSockets**  
✅ Ensure only **authenticated users can access WebSockets**  

---

## **1️⃣ Why Secure WebSockets?**  
WebSockets **don’t use traditional HTTP request-response authentication** like REST APIs.  
By default, **anyone can connect** to your WebSocket if they know the endpoint! ❌  

📌 **Potential Security Threats:**  
- Unauthorized users **listening to messages** 👀  
- Hackers **sending fake messages** 🚨  
- **Data leaks** in chat systems 💀  

✅ **Solution**: Secure WebSockets using **Spring Security + JWT Authentication** 🔐  

---

## **2️⃣ Adding Spring Security to WebSockets**  
📌 **Step 1: Add Spring Security Dependency in `pom.xml`**
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

📌 **Step 2: Configure WebSocket Security in `SecurityConfig.java`**  
Create a new package **`com.example.demo.config`** and add:

```java
package com.example.demo.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.messaging.MessageSecurityMetadataSourceRegistry;
import org.springframework.security.config.annotation.web.socket.EnableWebSocketSecurity;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;

@Configuration
@EnableWebSocketSecurity
public class SecurityConfig {

    @Bean
    public UserDetailsService userDetailsService() {
        UserDetails user = User.withDefaultPasswordEncoder()
                .username("admin")
                .password("admin")
                .roles("USER")
                .build();
        return new InMemoryUserDetailsManager(user);
    }

    protected void configureMessages(MessageSecurityMetadataSourceRegistry messages) {
        messages
            .simpDestMatchers("/app/**").authenticated()  // Protect messages sent
            .simpSubscribeDestMatchers("/topic/**").authenticated()  // Protect message subscriptions
            .anyMessage().denyAll();
    }
}
```

✅ **What’s happening?**
- `@EnableWebSocketSecurity` → Enables WebSocket security.
- **User authentication** is required for:
  - `@MessageMapping("/app/**")`
  - `@SendTo("/topic/**")`
- **Unauthorized users cannot send or receive WebSocket messages**.  

---

## **3️⃣ Implementing JWT Authentication for WebSockets**  
### **Step 3: Add JWT Dependencies (`pom.xml`)**
```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.11.2</version>
</dependency>
```

📌 **Step 4: Create `JwtUtil.java` for Generating & Validating JWT Tokens**
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
                .setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 60)) // 1 hour expiry
                .signWith(SignatureAlgorithm.HS256, SECRET_KEY)
                .compact();
    }

    public String extractUsername(String token) {
        return getClaims(token).getSubject();
    }

    public boolean validateToken(String token) {
        return extractUsername(token) != null && !isTokenExpired(token);
    }

    private Claims getClaims(String token) {
        return Jwts.parser()
                .setSigningKey(SECRET_KEY)
                .parseClaimsJws(token)
                .getBody();
    }

    private boolean isTokenExpired(String token) {
        return getClaims(token).getExpiration().before(new Date());
    }
}
```

✅ **What’s happening?**
- `generateToken()` → Generates a **JWT token** valid for **1 hour**.
- `extractUsername()` → Extracts **username from JWT**.
- `validateToken()` → **Validates if JWT is expired or not**.

---

## **4️⃣ Authenticating Users & Generating JWT Tokens**
📌 **Step 5: Create `AuthController.java`**
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

✅ **How It Works?**
- **API Endpoint:** `POST http://localhost:8080/api/auth/login`
- **Sends JWT Token** when username is provided.

---

## **5️⃣ Adding JWT Authentication to WebSockets**
📌 **Step 6: Modify `WebSocketConfig.java` to Handle JWT**
```java
import org.springframework.http.server.ServerHttpRequest;
import org.springframework.http.server.ServerHttpResponse;
import org.springframework.http.server.ServletServerHttpRequest;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.web.socket.WebSocketHandler;
import org.springframework.web.socket.server.HandshakeInterceptor;

import java.util.Collections;
import java.util.Map;

public class JwtHandshakeInterceptor implements HandshakeInterceptor {

    private final JwtUtil jwtUtil;

    public JwtHandshakeInterceptor(JwtUtil jwtUtil) {
        this.jwtUtil = jwtUtil;
    }

    @Override
    public boolean beforeHandshake(ServerHttpRequest request, ServerHttpResponse response, WebSocketHandler wsHandler, Map<String, Object> attributes) {
        if (request instanceof ServletServerHttpRequest) {
            ServletServerHttpRequest servletRequest = (ServletServerHttpRequest) request;
            String token = servletRequest.getServletRequest().getParameter("token");

            if (token != null && jwtUtil.validateToken(token)) {
                String username = jwtUtil.extractUsername(token);
                UserDetails userDetails = new User(username, "", Collections.emptyList());
                UsernamePasswordAuthenticationToken authentication = new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());

                SecurityContextHolder.getContext().setAuthentication(authentication);
                return true;
            }
        }
        return false;
    }

    @Override
    public void afterHandshake(ServerHttpRequest request, ServerHttpResponse response, WebSocketHandler wsHandler, Exception exception) {
    }
}
```

📌 **Step 7: Register the JWT Interceptor in `WebSocketConfig.java`**
```java
@Override
public void registerStompEndpoints(StompEndpointRegistry registry) {
    registry.addEndpoint("/ws")
            .addInterceptors(new JwtHandshakeInterceptor(jwtUtil)) // Attach JWT auth
            .setAllowedOrigins("*")
            .withSockJS();
}
```

✅ **Now WebSockets require JWT tokens for authentication!** 🔐

---

## **6️⃣ Testing Secure WebSockets**
### **Step 1: Get JWT Token**
```bash
curl -X POST "http://localhost:8080/api/auth/login?username=admin"
```
**Response:**  
```json
{"token":"your_generated_jwt_token"}
```

### **Step 2: Connect to WebSocket with JWT**
Modify **Frontend `index.html`**:
```javascript
var token = "your_generated_jwt_token";
var socket = new SockJS('/ws?token=' + token); // Pass token
var stompClient = Stomp.over(socket);

stompClient.connect({}, function (frame) {
    console.log('Connected: ' + frame);
    stompClient.subscribe('/topic/public', function (message) {
        console.log(JSON.parse(message.body));
    });
});
```

✅ **Now, only authenticated users with JWT tokens can access WebSockets!** 🎉  

---

## 🎯 **Lesson 12 - Summary**
✅ Secured **WebSockets with Spring Security**  
✅ Implemented **JWT authentication for WebSockets**  
✅ Allowed **only authenticated users to send & receive messages**  
✅ Tested **secure WebSocket connections**  

---
