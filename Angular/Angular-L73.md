---

## **Lesson 73: JWT Authentication and Authorization with Spring Boot and Angular**

---

## **What You Will Learn:**
1. **Introduction to JWT Authentication and Authorization**
   - What is JWT (JSON Web Token)?
   - Why Use JWT for Authentication and Authorization?
   - Understanding JWT Structure and Workflow

2. **Setting up Spring Boot for JWT Authentication**
   - Configuring Spring Security for JWT Authentication
   - Creating UserDetailsService for User Authentication
   - Implementing AuthenticationController for Login and Token Generation
   - Implementing Authorization Filter for Secured Endpoints

3. **Creating JWT Utility Class in Spring Boot**
   - Generating JWT Token
   - Validating JWT Token
   - Extracting User Details from JWT Token

4. **Integrating Angular with JWT Authentication**
   - Creating Authentication Service in Angular
   - Storing JWT Token in Local Storage
   - Sending JWT Token in HTTP Headers
   - Securing Routes with AuthGuard

5. **Hands-on Exercise: Implementing Full Authentication and Authorization Flow**
   - Spring Boot Backend with JWT Authentication and Authorization
   - Angular Frontend with Login, Logout, and Role-Based Authorization
   - Testing Secure Endpoints with Postman and Angular UI

---

## **1. Introduction to JWT Authentication and Authorization**

---

### **1.1 What is JWT (JSON Web Token)?**
- **JWT (JSON Web Token)** is an open standard (RFC 7519) for securely transmitting information between parties as a JSON object.
- It is **digitally signed** using either:
  - **HMAC (symmetric key)** or
  - **RSA/ECDSA (asymmetric key pair)**.
- Ensures **data integrity** and **authenticity**.

---

### **1.2 Why Use JWT for Authentication and Authorization?**
- **Stateless Authentication**:
  - Server does not store session data.
  - Token is sent with every HTTP request, allowing scalability and easier stateless API design.
- **Cross-Domain Authentication**:
  - Works across different domains, making it ideal for **SPA (Single Page Applications)** like Angular.
- **Security and Flexibility**:
  - Digitally signed, preventing data tampering.
  - Customizable payload for user roles and permissions.

---

### **1.3 Understanding JWT Structure and Workflow**
- **JWT Structure**:
  - **Header**: Contains the algorithm used for signing (e.g., HS256, RS256).
  - **Payload**: Contains user claims, roles, and expiration time.
  - **Signature**: Ensures the token's integrity and authenticity.

- **Example of JWT Structure**:
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.          // Header
eyJzdWIiOiJqb2huZG9lIiwicm9sZXMiOlsiQURNSU4iXSwiaWF0IjoxNjE2MjM5MDIyfQ.     // Payload
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c   // Signature
```

---

### **1.4 JWT Authentication and Authorization Workflow**
1. **User Login**:
   - User sends **login credentials** (username and password) to the Spring Boot backend.
2. **Authentication and Token Generation**:
   - Spring Boot **authenticates the user**.
   - If valid, a **JWT token** is generated and sent back to Angular.
3. **Storing JWT Token**:
   - Angular **stores the token** in **Local Storage** or **Session Storage**.
4. **Sending JWT Token in Requests**:
   - Angular **attaches the token** to the **Authorization header** of each request.
5. **Authorization and Secured Endpoints**:
   - Spring Boot **validates the token** and **authorizes** access to secured endpoints.
   - If the token is invalid or expired, a **401 Unauthorized** response is returned.

---

## **2. Setting up Spring Boot for JWT Authentication**

---

### **2.1 Configuring Spring Security for JWT Authentication**

---

#### **Add Dependencies in pom.xml (Maven)**

```xml
<dependencies>
    <!-- Spring Security for authentication and authorization -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>

    <!-- Spring Web for RESTful APIs -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Spring Data JPA for database access -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- JWT Library for token generation and validation -->
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt</artifactId>
        <version>0.9.1</version>
    </dependency>

    <!-- MySQL Driver -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
    </dependency>

    <!-- Lombok for reducing boilerplate code -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>
```

- **spring-boot-starter-security** – For authentication and authorization.
- **spring-boot-starter-web** – For building RESTful APIs.
- **spring-boot-starter-data-jpa** – For database operations with JPA.
- **jjwt** – Java JWT library for token creation and validation.

---

### **2.2 Creating UserDetailsService for User Authentication**

- **UserDetailsService** is a core interface in Spring Security for loading user-specific data.
- We will implement **UserDetailsService** to:
  - Load users from the database.
  - Verify username and password.
  - Fetch user roles for authorization.

---

**Example: CustomUserDetailsService**

```java
package com.example.demo.security;

import com.example.demo.model.User;
import com.example.demo.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

@Service
public class CustomUserDetailsService implements UserDetailsService {

    @Autowired
    private UserRepository userRepository;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        User user = userRepository.findByUsername(username)
            .orElseThrow(() -> new UsernameNotFoundException("User not found with username: " + username));
        
        return new org.springframework.security.core.userdetails.User(
            user.getUsername(),
            user.getPassword(),
            user.getRoles()
        );
    }
}
```

- **UserDetailsService** – Loads user-specific data.
- **loadUserByUsername()** – Fetches user from the database.
- **UsernameNotFoundException** – Thrown if the user is not found.
- **org.springframework.security.core.userdetails.User** – Adapts our `User` entity to Spring Security's UserDetails implementation.

---

## **Next Steps:**
- **2.3 Implementing AuthenticationController for Login and Token Generation**
- **2.4 Implementing Authorization Filter for Secured Endpoints**
- **3. Creating JWT Utility Class in Spring Boot**
  - **Generating JWT Token**
  - **Validating JWT Token**
  - **Extracting User Details from JWT Token**
