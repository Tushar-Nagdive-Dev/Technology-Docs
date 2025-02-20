# **Phase 8 - Step 22: API Versioning, Documentation, and Backward Compatibility**  

We have successfully implemented **CI/CD Best Practices** and **DevOps Automation** with infrastructure as code, secrets management, and security automation. Now, it’s time to enhance the **API Design** by implementing:  
1. **API Versioning**: Managing multiple versions of APIs without breaking existing clients.  
2. **API Documentation**: Using **Swagger** and **OpenAPI** for documenting RESTful APIs.  
3. **Backward Compatibility**: Ensuring new changes don’t break existing functionality.  
4. **API Gateway Enhancements**: Implementing version routing and request validation.  

---

## **22.1. Why API Versioning, Documentation, and Backward Compatibility?**  
- **API Versioning** allows introducing new features without breaking existing clients.  
- **API Documentation** enhances developer experience and improves API adoption.  
- **Backward Compatibility** ensures smooth transitions for users during upgrades.  
- **API Gateway Enhancements** provide centralized version routing and validation.  

---

## **22.2. API Versioning Strategy**  

### **Common Versioning Strategies**  
1. **URI Versioning**: `/api/v1/resource`  
2. **Query Parameter Versioning**: `/api/resource?version=1`  
3. **Header Versioning**: `Accept: application/vnd.company.v1+json`  
4. **Media Type Versioning**: `Accept: application/vnd.company.resource-v1+json`

### **Recommended Strategy**: **URI Versioning**  
- It is widely adopted, easy to implement, and SEO-friendly.  
- Example:  
  - `GET /api/v1/courses` → Version 1  
  - `GET /api/v2/courses` → Version 2 (New enhancements)  

---

## **22.3. Implementing API Versioning in Spring Boot**  

We will implement **URI Versioning** for the **Course Service**.

### **Step 1: Create Versioned Controller**  
- We will maintain separate controllers for each version for better maintainability.  
- Create **CourseV1Controller** and **CourseV2Controller**.

---

### **1. CourseV1Controller (Version 1)**  
**src/main/java/com/cashflowapp/courseservice/controller/v1/CourseV1Controller.java**  
```java
package com.cashflowapp.courseservice.controller.v1;

import com.cashflowapp.courseservice.model.Course;
import com.cashflowapp.courseservice.service.CourseService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/v1/courses")
public class CourseV1Controller {

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

### **2. CourseV2Controller (Version 2)**  
In Version 2, we introduce a new attribute `courseLevel`.

**src/main/java/com/cashflowapp/courseservice/controller/v2/CourseV2Controller.java**  
```java
package com.cashflowapp.courseservice.controller.v2;

import com.cashflowapp.courseservice.model.Course;
import com.cashflowapp.courseservice.service.CourseService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/v2/courses")
public class CourseV2Controller {

    @Autowired
    private CourseService courseService;

    @GetMapping
    public List<Course> getAllCourses() {
        List<Course> courses = courseService.getAllCourses();
        courses.forEach(course -> course.setCourseLevel("Beginner"));
        return courses;
    }

    @GetMapping("/{id}")
    public Course getCourseById(@PathVariable Long id) {
        Course course = courseService.getCourseById(id).orElse(null);
        if (course != null) {
            course.setCourseLevel("Beginner");
        }
        return course;
    }
}
```

### **Change Summary**:  
- **Version 1** retains the original structure.  
- **Version 2** introduces a new attribute: `courseLevel`.  
- Existing clients using `v1` remain unaffected.  
- New clients can access enhanced functionality using `v2`.  

---

## **22.4. Update Model Class for Backward Compatibility**  

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

    // New attribute for v2 (Backward Compatibility)
    @Transient
    private String courseLevel;
}
```

### **Explanation**:  
- **@Transient**: Ensures the new attribute `courseLevel` is only used in responses and not persisted in the database.  
- This maintains **backward compatibility** with existing database schemas.  

---

## **22.5. API Documentation with Swagger and OpenAPI**  

We will use **SpringDoc OpenAPI** for generating Swagger documentation for versioned APIs.

### **Step 1: Add Dependency**  
**pom.xml**  
```xml
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-ui</artifactId>
    <version>1.6.4</version>
</dependency>
```

### **Step 2: Enable Swagger Documentation**  
**src/main/java/com/cashflowapp/courseservice/config/OpenApiConfig.java**  
```java
package com.cashflowapp.courseservice.config;

import io.swagger.v3.oas.models.OpenAPI;
import io.swagger.v3.oas.models.info.Info;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class OpenApiConfig {

    @Bean
    public OpenAPI customOpenAPI() {
        return new OpenAPI()
                .info(new Info()
                        .title("CashFlowApp Course Service API")
                        .version("v1.0")
                        .description("API Documentation for Course Service"));
    }
}
```

### **Step 3: Accessing Swagger UI**  
- **Version 1 Documentation**: `http://localhost:8082/swagger-ui.html#/api/v1`  
- **Version 2 Documentation**: `http://localhost:8082/swagger-ui.html#/api/v2`

### **Step 4: Update API Gateway for Version Routing**  
**src/main/resources/application.yml**  
```yaml
spring:
  cloud:
    gateway:
      routes:
        - id: course-service-v1
          uri: lb://course-service
          predicates:
            - Path=/api/v1/courses/**
        - id: course-service-v2
          uri: lb://course-service
          predicates:
            - Path=/api/v2/courses/**
```

---

## **22.6. Verify and Test**  

1. **Test Versioned Endpoints**:  
   - `GET /api/v1/courses` → Version 1 without `courseLevel`.  
   - `GET /api/v2/courses` → Version 2 with `courseLevel`.

2. **Check Swagger Documentation**:  
   - Verify versioned documentation is correctly generated.

3. **API Gateway Routing**:  
   - Confirm correct routing to v1 and v2 endpoints.

---

## **Next Step**  
1. Test all versioning and backward compatibility features.  
2. Next, we’ll move on to **Phase 9: Advanced API Management and GraphQL Integration**.  
