# **Phase 2 - Step 11: Building the Course Service**  

We’ve successfully implemented **JWT Authentication** and **Role-Based Authorization** in the **User Service**. Now, let’s move on to building the **Course Service**, which will:  
1. Manage course creation, updates, and module organization.  
2. Provide APIs for listing, searching, and filtering courses.  
3. Only allow `ADMIN` users to create or update courses, while all users can view courses.

---

## **11.1. Course Service Overview**  
**Course Service** will include:  
- **Create Course**: Restricted to `ADMIN` role.  
- **Update Course**: Restricted to `ADMIN` role.  
- **Delete Course**: Restricted to `ADMIN` role.  
- **List All Courses**: Open to all authenticated users.  
- **Get Course by ID**: Open to all authenticated users.  

---

## **11.2. Setting Up the Project**  

### **Step 1: Create a New Spring Boot Project**  
**Go to [start.spring.io](https://start.spring.io)** and select:  
- **Project**: Maven  
- **Language**: Java  
- **Spring Boot Version**: 3.x.x  
- **Group**: `com.cashflowapp`  
- **Artifact**: `course-service`  
- **Name**: `Course Service`  
- **Description**: `Course Management Microservice`  
- **Package Name**: `com.cashflowapp.courseservice`  
- **Dependencies**:  
  - Spring Web  
  - Spring Data JPA  
  - PostgreSQL Driver  
  - Spring Security  
  - Lombok  
  - Spring Cloud Netflix Eureka Client  
  - Spring Boot Starter Validation  
  - JWT (for authentication)  

**Download** the project and open it in **VSCode**.

---

### **Step 2: Project Structure**  
Your project structure should look like this:  
```
course-service
├── src
│   ├── main
│   │   ├── java
│   │   │   └── com.cashflowapp.courseservice
│   │   │       ├── controller
│   │   │       ├── model
│   │   │       ├── repository
│   │   │       ├── service
│   │   │       └── CourseServiceApplication.java
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
spring.application.name=course-service
server.port=8082

# Database Configuration
spring.datasource.url=jdbc:postgresql://localhost:5432/coursedb
spring.datasource.username=postgres
spring.datasource.password=postgres
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect

# Eureka Client Configuration
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/
eureka.instance.prefer-ip-address=true

# JWT Configuration
jwt.secret=MySecretKey
```

---

## **11.3. Create Model Class**  
**src/main/java/com/cashflowapp/courseservice/model/Course.java**  
```java
package com.cashflowapp.courseservice.model;

import jakarta.persistence.*;
import lombok.Data;

@Entity
@Data
public class Course {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true)
    private String title;

    @Column(nullable = false)
    private String description;

    private String instructor;
    private Double price;
}
```

---

## **11.4. Create Repository Interface**  
**src/main/java/com/cashflowapp/courseservice/repository/CourseRepository.java**  
```java
package com.cashflowapp.courseservice.repository;

import com.cashflowapp.courseservice.model.Course;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface CourseRepository extends JpaRepository<Course, Long> {
}
```

---

## **11.5. Create Service Class**  
**src/main/java/com/cashflowapp/courseservice/service/CourseService.java**  
```java
package com.cashflowapp.courseservice.service;

import com.cashflowapp.courseservice.model.Course;
import com.cashflowapp.courseservice.repository.CourseRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Optional;

@Service
public class CourseService {
    @Autowired
    private CourseRepository courseRepository;

    public List<Course> getAllCourses() {
        return courseRepository.findAll();
    }

    public Optional<Course> getCourseById(Long id) {
        return courseRepository.findById(id);
    }

    public Course saveCourse(Course course) {
        return courseRepository.save(course);
    }

    public void deleteCourse(Long id) {
        courseRepository.deleteById(id);
    }
}
```

---

## **11.6. Create Controller Class**  
**src/main/java/com/cashflowapp/courseservice/controller/CourseController.java**  
```java
package com.cashflowapp.courseservice.controller;

import com.cashflowapp.courseservice.model.Course;
import com.cashflowapp.courseservice.service.CourseService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/courses")
public class CourseController {

    @Autowired
    private CourseService courseService;

    @GetMapping
    public List<Course> getAllCourses() {
        return courseService.getAllCourses();
    }

    @GetMapping("/{id}")
    public Course getCourseById(@PathVariable Long id) {
        return courseService.getCourseById(id).orElse(null);
    }

    @PostMapping
    public Course createCourse(@RequestBody Course course) {
        return courseService.saveCourse(course);
    }

    @PutMapping("/{id}")
    public Course updateCourse(@PathVariable Long id, @RequestBody Course course) {
        course.setId(id);
        return courseService.saveCourse(course);
    }

    @DeleteMapping("/{id}")
    public void deleteCourse(@PathVariable Long id) {
        courseService.deleteCourse(id);
    }
}
```

---

## **11.7. Secure Endpoints with Role-Based Authorization**  
We’ll now secure the endpoints so that only `ADMIN` users can create, update, or delete courses.

**src/main/java/com/cashflowapp/courseservice/security/SecurityConfig.java**  
```java
package com.cashflowapp.courseservice.security;

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
                .requestMatchers("/api/courses").hasAuthority("ADMIN")
                .requestMatchers("/api/courses/**").hasAuthority("ADMIN")
                .anyRequest().authenticated()
            .and()
            .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);
        
        return http.build();
    }
}
```

---

## **11.8. Run and Test the Course Service**  
1. **Create Database**: Start **PostgreSQL** and create a database named `coursedb`.  
2. **Start Course Service** on port `8082`.  
3. **Test with Postman**:  
   - Create, update, and delete courses with an `ADMIN` token.  
   - Get all courses and course by ID with any valid token.  

---

## **Next Step**  
1. Test the **Course Service** endpoints with **Postman**.  
2. Next, we’ll implement **Enrollment Service**.  
