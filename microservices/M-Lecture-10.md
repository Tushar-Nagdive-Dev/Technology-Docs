# **Phase 2 - Step 15: Integrating All Microservices with Spring Cloud Gateway**  

We have successfully built and tested all the individual microservices:
- **User Service**: Authentication and authorization with JWT.  
- **Course Service**: Course management with role-based authorization.  
- **Enrollment Service**: Course enrollment and access management.  
- **Payment Service**: Payment processing with asynchronous communication.  
- **Notification Service**: Event-driven notifications using Kafka.  

Now, it’s time to integrate all these microservices using **Spring Cloud Gateway**. The gateway will:
1. Serve as a single entry point for all client requests.  
2. Handle routing and load balancing.  
3. Centralize authentication and authorization using JWT.  
4. Enable cross-origin requests (CORS) for the frontend application.  

---

## **15.1. Why Spring Cloud Gateway?**  
- **Centralized Routing**: Single entry point for all APIs.  
- **Security**: Centralized authentication and authorization using JWT.  
- **Cross-cutting Concerns**: Easily handle CORS, logging, and rate limiting.  
- **Resilience**: Built-in support for resilience patterns like circuit breakers and retries.

---

## **15.2. Setting Up Spring Cloud Gateway Project**  

### **Step 1: Create a New Spring Boot Project**  
**Go to [start.spring.io](https://start.spring.io)** and select:  
- **Project**: Maven  
- **Language**: Java  
- **Spring Boot Version**: 3.x.x  
- **Group**: `com.cashflowapp`  
- **Artifact**: `api-gateway`  
- **Name**: `API Gateway`  
- **Description**: `API Gateway for Microservices`  
- **Package Name**: `com.cashflowapp.apigateway`  
- **Dependencies**:  
  - Spring Cloud Gateway  
  - Spring Cloud Netflix Eureka Client  
  - Spring Security  
  - Lombok  
  - Spring Boot Starter Validation  
  - JWT (for centralized authentication)  

**Download** the project and open it in **VSCode**.

---

### **Step 2: Project Structure**  
Your project structure should look like this:  
```
api-gateway
├── src
│   ├── main
│   │   ├── java
│   │   │   └── com.cashflowapp.apigateway
│   │   │       ├── config
│   │   │       ├── security
│   │   │       └── ApiGatewayApplication.java
│   │   └── resources
│   │       ├── application.properties
│   │       └── application-dev.properties
└── pom.xml
```

---

### **Step 3: Configure application.properties**  
Configure Spring Cloud Gateway and Eureka client.

**src/main/resources/application.properties**  
```properties
spring.application.name=api-gateway
server.port=8080

# Eureka Client Configuration
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/
eureka.instance.prefer-ip-address=true

# JWT Configuration
jwt.secret=MySecretKey

# Gateway Configuration
spring.cloud.gateway.discovery.locator.enabled=true
spring.cloud.gateway.discovery.locator.lower-case-service-id=true
```

---

### **Step 4: Enable Service Discovery**  
Enable service discovery with Eureka for automatic routing.

**src/main/java/com/cashflowapp/apigateway/ApiGatewayApplication.java**  
```java
package com.cashflowapp.apigateway;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient
public class ApiGatewayApplication {
    public static void main(String[] args) {
        SpringApplication.run(ApiGatewayApplication.class, args);
    }
}
```

---

## **15.3. Implement JWT Token Validation**  
Since the gateway is the single entry point, we’ll validate JWT tokens here.

### **Step 1: Create JwtTokenUtil Class**  
**src/main/java/com/cashflowapp/apigateway/security/JwtTokenUtil.java**  
```java
package com.cashflowapp.apigateway.security;

import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

import java.util.Date;
import java.util.function.Function;

@Component
public class JwtTokenUtil {

    @Value("${jwt.secret}")
    private String secret;

    public String getUsernameFromToken(String token) {
        return getClaimFromToken(token, Claims::getSubject);
    }

    public Date getExpirationDateFromToken(String token) {
        return getClaimFromToken(token, Claims::getExpiration);
    }

    public Boolean isTokenExpired(String token) {
        final Date expiration = getExpirationDateFromToken(token);
        return expiration.before(new Date());
    }

    public Boolean validateToken(String token) {
        return !isTokenExpired(token);
    }

    private <T> T getClaimFromToken(String token, Function<Claims, T> claimsResolver) {
        final Claims claims = Jwts.parser()
                .setSigningKey(secret)
                .parseClaimsJws(token)
                .getBody();
        return claimsResolver.apply(claims);
    }
}
```

---

### **Step 2: Create Authentication Filter**  
**src/main/java/com/cashflowapp/apigateway/security/JwtAuthenticationFilter.java**  
```java
package com.cashflowapp.apigateway.security;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.server.reactive.ServerHttpRequest;
import org.springframework.stereotype.Component;
import org.springframework.web.server.ServerWebExchange;
import org.springframework.web.server.WebFilter;
import org.springframework.web.server.WebFilterChain;
import reactor.core.publisher.Mono;

@Component
public class JwtAuthenticationFilter implements WebFilter {

    @Autowired
    private JwtTokenUtil jwtTokenUtil;

    @Override
    public Mono<Void> filter(ServerWebExchange exchange, WebFilterChain chain) {
        ServerHttpRequest request = exchange.getRequest();
        if (request.getHeaders().containsKey(HttpHeaders.AUTHORIZATION)) {
            String token = request.getHeaders().getOrEmpty(HttpHeaders.AUTHORIZATION).get(0);
            token = token.replace("Bearer ", "");

            if (!jwtTokenUtil.validateToken(token)) {
                exchange.getResponse().setStatusCode(HttpStatus.UNAUTHORIZED);
                return exchange.getResponse().setComplete();
            }
        }
        return chain.filter(exchange);
    }
}
```

---

### **Step 3: Register Authentication Filter**  
**src/main/java/com/cashflowapp/apigateway/config/SecurityConfig.java**  
```java
package com.cashflowapp.apigateway.config;

import com.cashflowapp.apigateway.security.JwtAuthenticationFilter;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.web.server.ServerHttpSecurity;
import org.springframework.security.web.server.SecurityWebFilterChain;

@Configuration
public class SecurityConfig {

    @Bean
    public SecurityWebFilterChain securityWebFilterChain(ServerHttpSecurity http, JwtAuthenticationFilter jwtAuthenticationFilter) {
        http
            .csrf().disable()
            .authorizeExchange()
                .pathMatchers("/api/users/login", "/api/users/register").permitAll()
                .anyExchange().authenticated()
            .and()
            .addFilterAt(jwtAuthenticationFilter, SecurityWebFilterChain.class);

        return http.build();
    }
}
```

---

## **15.4. Run and Test the API Gateway**  
1. **Start Eureka Server** on port `8761`.  
2. **Start All Microservices** (User, Course, Enrollment, Payment, Notification).  
3. **Start API Gateway** on port `8080`.  
4. **Test with Postman**:  
   - Use the Gateway URL to access all services.  
   - Verify that:
     - Public endpoints (login and registration) are accessible without a token.  
     - Secured endpoints require a valid JWT token.

---

## **Next Step**  
1. Test the **API Gateway** with **Postman**.  
2. Next, we’ll **Deploy all Microservices on Docker**.  
