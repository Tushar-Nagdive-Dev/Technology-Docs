# **Phase 2 - Step 13: Building the Payment Service**  

We’ve successfully built the **Enrollment Service** with secured endpoints. Now, let’s move on to the **Payment Service**, which will:  
1. Handle payment processing for course enrollments.  
2. Only allow authenticated users to make payments.  
3. Integrate with external payment gateways (e.g., Stripe, PayPal) in future enhancements.  
4. Communicate asynchronously with **Enrollment Service** to update enrollment status upon successful payment.

---

## **13.1. Payment Service Overview**  
**Payment Service** will include:  
- **Process Payment**: Allows users to pay for a course.  
- **Payment Status**: Provides the status of a payment.  
- **Payment History**: Lists payment history for a user.  
- **Integration with Enrollment Service**: Updates enrollment status after successful payment.  

---

## **13.2. Setting Up the Project**  

### **Step 1: Create a New Spring Boot Project**  
**Go to [start.spring.io](https://start.spring.io)** and select:  
- **Project**: Maven  
- **Language**: Java  
- **Spring Boot Version**: 3.x.x  
- **Group**: `com.cashflowapp`  
- **Artifact**: `payment-service`  
- **Name**: `Payment Service`  
- **Description**: `Payment Processing Microservice`  
- **Package Name**: `com.cashflowapp.paymentservice`  
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
  - Spring Cloud Stream (for asynchronous communication with Enrollment Service)  

**Download** the project and open it in **VSCode**.

---

### **Step 2: Project Structure**  
Your project structure should look like this:  
```
payment-service
├── src
│   ├── main
│   │   ├── java
│   │   │   └── com.cashflowapp.paymentservice
│   │   │       ├── controller
│   │   │       ├── model
│   │   │       ├── repository
│   │   │       ├── service
│   │   │       ├── event
│   │   │       └── PaymentServiceApplication.java
│   │   └── resources
│   │       ├── application.properties
│   │       └── application-dev.properties
└── pom.xml
```

---

### **Step 3: Configure application.properties**  
Configure the database connection, Eureka client, and Spring Cloud Stream for asynchronous communication.  

**src/main/resources/application.properties**  
```properties
spring.application.name=payment-service
server.port=8084

# Database Configuration
spring.datasource.url=jdbc:postgresql://localhost:5432/paymentdb
spring.datasource.username=postgres
spring.datasource.password=postgres
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect

# Eureka Client Configuration
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/
eureka.instance.prefer-ip-address=true

# JWT Configuration
jwt.secret=MySecretKey

# Spring Cloud Stream Configuration (using Kafka)
spring.cloud.stream.defaultBinder=kafka
spring.cloud.stream.binders.kafka.environment.spring.kafka.bootstrap-servers=localhost:9092
```

---

## **13.3. Create Model Class**  
**src/main/java/com/cashflowapp/paymentservice/model/Payment.java**  
```java
package com.cashflowapp.paymentservice.model;

import jakarta.persistence.*;
import lombok.Data;

@Entity
@Data
public class Payment {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private Long userId;

    @Column(nullable = false)
    private Long courseId;

    @Column(nullable = false)
    private Double amount;

    @Column(nullable = false)
    private String status;
}
```

---

## **13.4. Create Repository Interface**  
**src/main/java/com/cashflowapp/paymentservice/repository/PaymentRepository.java**  
```java
package com.cashflowapp.paymentservice.repository;

import com.cashflowapp.paymentservice.model.Payment;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
public interface PaymentRepository extends JpaRepository<Payment, Long> {
    List<Payment> findByUserId(Long userId);
}
```

---

## **13.5. Create Event Class**  
We will use an event-driven architecture to notify the **Enrollment Service** about successful payments.

**src/main/java/com/cashflowapp/paymentservice/event/PaymentEvent.java**  
```java
package com.cashflowapp.paymentservice.event;

import lombok.Data;

@Data
public class PaymentEvent {
    private Long userId;
    private Long courseId;
    private String status;
}
```

---

## **13.6. Create Service Class**  
**src/main/java/com/cashflowapp/paymentservice/service/PaymentService.java**  
```java
package com.cashflowapp.paymentservice.service;

import com.cashflowapp.paymentservice.model.Payment;
import com.cashflowapp.paymentservice.repository.PaymentRepository;
import com.cashflowapp.paymentservice.event.PaymentEvent;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.stream.function.StreamBridge;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class PaymentService {

    @Autowired
    private PaymentRepository paymentRepository;

    @Autowired
    private StreamBridge streamBridge;

    public Payment processPayment(Payment payment) {
        payment.setStatus("SUCCESS");
        Payment savedPayment = paymentRepository.save(payment);

        // Publish Payment Event to Kafka
        PaymentEvent paymentEvent = new PaymentEvent();
        paymentEvent.setUserId(payment.getUserId());
        paymentEvent.setCourseId(payment.getCourseId());
        paymentEvent.setStatus("SUCCESS");

        streamBridge.send("paymentEvent-out-0", paymentEvent);

        return savedPayment;
    }

    public List<Payment> getUserPayments(Long userId) {
        return paymentRepository.findByUserId(userId);
    }
}
```

---

## **13.7. Create Controller Class**  
**src/main/java/com/cashflowapp/paymentservice/controller/PaymentController.java**  
```java
package com.cashflowapp.paymentservice.controller;

import com.cashflowapp.paymentservice.model.Payment;
import com.cashflowapp.paymentservice.service.PaymentService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/payments")
public class PaymentController {

    @Autowired
    private PaymentService paymentService;

    @PostMapping("/process")
    public Payment processPayment(@RequestBody Payment payment) {
        return paymentService.processPayment(payment);
    }

    @GetMapping("/user/{userId}")
    public List<Payment> getUserPayments(@PathVariable Long userId) {
        return paymentService.getUserPayments(userId);
    }
}
```

---

## **13.8. Secure Endpoints with Authentication**  
Only authenticated users can make payments or view payment history.

**src/main/java/com/cashflowapp/paymentservice/security/SecurityConfig.java**  
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .authorizeHttpRequests()
                .requestMatchers("/api/payments/**").authenticated()
                .anyRequest().permitAll()
            .and()
            .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);

        return http.build();
    }
}
```

---

## **13.9. Run and Test the Payment Service**  
1. **Create Database**: Start **PostgreSQL** and create a database named `paymentdb`.  
2. **Start Payment Service** on port `8084`.  
3. **Test with Postman**:  
   - **Process Payment**: `POST /api/payments/process` (Authenticated)  
   - **View Payment History**: `GET /api/payments/user/{userId}` (Authenticated)  

---

## **Next Step**  
1. Test the **Payment Service** endpoints with **Postman**.  
2. Next, we’ll build the **Notification Service**.  
