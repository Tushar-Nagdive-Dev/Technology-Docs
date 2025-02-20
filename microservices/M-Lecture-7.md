# **Phase 2 - Step 12: Building the Enrollment Service**  

We have successfully built the **Course Service** with role-based authorization. Now, let’s move on to the **Enrollment Service**, which will:  
1. Manage course enrollments and access.  
2. Allow users to enroll in courses.  
3. Allow users to view their enrolled courses.  
4. Secure endpoints so that only authenticated users can enroll in and view their courses.

---

## **12.1. Enrollment Service Overview**  
**Enrollment Service** will include:  
- **Enroll in Course**: Any authenticated user can enroll in a course.  
- **List User Enrollments**: Users can view their enrolled courses.  
- **Cancel Enrollment**: Users can cancel their enrollment.  

---

## **12.2. Setting Up the Project**  

### **Step 1: Create a New Spring Boot Project**  
**Go to [start.spring.io](https://start.spring.io)** and select:  
- **Project**: Maven  
- **Language**: Java  
- **Spring Boot Version**: 3.x.x  
- **Group**: `com.cashflowapp`  
- **Artifact**: `enrollment-service`  
- **Name**: `Enrollment Service`  
- **Description**: `Enrollment Management Microservice`  
- **Package Name**: `com.cashflowapp.enrollmentservice`  
- **Dependencies**:  
  - Spring Web  
  - Spring Data JPA  
  - PostgreSQL Driver  
  - Spring Security  
  - Lombok  
  - Spring Cloud Netflix Eureka Client  
  - Spring Boot Starter Validation  
  - JWT (for authentication)  
  - OpenFeign (for inter-service communication)  

**Download** the project and open it in **VSCode**.

---

### **Step 2: Project Structure**  
Your project structure should look like this:  
```
enrollment-service
├── src
│   ├── main
│   │   ├── java
│   │   │   └── com.cashflowapp.enrollmentservice
│   │   │       ├── controller
│   │   │       ├── model
│   │   │       ├── repository
│   │   │       ├── service
│   │   │       └── EnrollmentServiceApplication.java
│   │   └── resources
│   │       ├── application.properties
│   │       └── application-dev.properties
└── pom.xml
```

---

### **Step 3: Configure application.properties**  
Configure the database connection and Eureka client.  

**src/main/resources/application.properties**  
```properties
spring.application.name=enrollment-service
server.port=8083

# Database Configuration
spring.datasource.url=jdbc:postgresql://localhost:5432/enrollmentdb
spring.datasource.username=postgres
spring.datasource.password=postgres
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect

# Eureka Client Configuration
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/
eureka.instance.prefer-ip-address=true

# JWT Configuration
jwt.secret=MySecretKey

# Feign Client Configuration
feign.hystrix.enabled=true
```

---

## **12.3. Create Model Class**  
**src/main/java/com/cashflowapp/enrollmentservice/model/Enrollment.java**  
```java
package com.cashflowapp.enrollmentservice.model;

import jakarta.persistence.*;
import lombok.Data;

@Entity
@Data
public class Enrollment {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private Long userId;

    @Column(nullable = false)
    private Long courseId;

    @Column(nullable = false)
    private String status;
}
```

---

## **12.4. Create Repository Interface**  
**src/main/java/com/cashflowapp/enrollmentservice/repository/EnrollmentRepository.java**  
```java
package com.cashflowapp.enrollmentservice.repository;

import com.cashflowapp.enrollmentservice.model.Enrollment;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
public interface EnrollmentRepository extends JpaRepository<Enrollment, Long> {
    List<Enrollment> findByUserId(Long userId);
    List<Enrollment> findByCourseId(Long courseId);
}
```

---

## **12.5. Create Service Class**  
**src/main/java/com/cashflowapp/enrollmentservice/service/EnrollmentService.java**  
```java
package com.cashflowapp.enrollmentservice.service;

import com.cashflowapp.enrollmentservice.model.Enrollment;
import com.cashflowapp.enrollmentservice.repository.EnrollmentRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class EnrollmentService {
    @Autowired
    private EnrollmentRepository enrollmentRepository;

    public Enrollment enroll(Long userId, Long courseId) {
        Enrollment enrollment = new Enrollment();
        enrollment.setUserId(userId);
        enrollment.setCourseId(courseId);
        enrollment.setStatus("ENROLLED");
        return enrollmentRepository.save(enrollment);
    }

    public List<Enrollment> getUserEnrollments(Long userId) {
        return enrollmentRepository.findByUserId(userId);
    }

    public void cancelEnrollment(Long id) {
        enrollmentRepository.deleteById(id);
    }
}
```

---

## **12.6. Create Controller Class**  
**src/main/java/com/cashflowapp/enrollmentservice/controller/EnrollmentController.java**  
```java
package com.cashflowapp.enrollmentservice.controller;

import com.cashflowapp.enrollmentservice.model.Enrollment;
import com.cashflowapp.enrollmentservice.service.EnrollmentService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/enrollments")
public class EnrollmentController {

    @Autowired
    private EnrollmentService enrollmentService;

    @PostMapping("/enroll")
    public Enrollment enroll(@RequestParam Long userId, @RequestParam Long courseId) {
        return enrollmentService.enroll(userId, courseId);
    }

    @GetMapping("/user/{userId}")
    public List<Enrollment> getUserEnrollments(@PathVariable Long userId) {
        return enrollmentService.getUserEnrollments(userId);
    }

    @DeleteMapping("/{id}")
    public void cancelEnrollment(@PathVariable Long id) {
        enrollmentService.cancelEnrollment(id);
    }
}
```

---

## **12.7. Secure Endpoints with Authentication**  
Only authenticated users can enroll in and view their courses.

**src/main/java/com/cashflowapp/enrollmentservice/security/SecurityConfig.java**  
```java
package com.cashflowapp.enrollmentservice.security;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
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
                .requestMatchers("/api/enrollments/**").authenticated()
                .anyRequest().permitAll()
            .and()
            .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);
        
        return http.build();
    }
}
```

---

## **12.8. Run and Test the Enrollment Service**  
1. **Create Database**: Start **PostgreSQL** and create a database named `enrollmentdb`.  
2. **Start Enrollment Service** on port `8083`.  
3. **Test with Postman**:  
   - **Enroll in Course**: `POST /api/enrollments/enroll` (Authenticated)  
   - **View User Enrollments**: `GET /api/enrollments/user/{userId}` (Authenticated)  
   - **Cancel Enrollment**: `DELETE /api/enrollments/{id}` (Authenticated)  

---

## **Next Step**  
1. Test the **Enrollment Service** endpoints with **Postman**.  
2. Next, we’ll implement the **Payment Service**.  
