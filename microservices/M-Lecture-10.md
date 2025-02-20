# **Phase 2 - Step 14: Building the Notification Service**  

We’ve successfully built the **Payment Service** with asynchronous communication using **Spring Cloud Stream**. Now, let’s move on to the **Notification Service**, which will:  
1. Send email and SMS notifications for various events like course enrollment, payment success, and course updates.  
2. Consume events from **Enrollment Service** and **Payment Service** using **Apache Kafka**.  
3. Use **Spring Cloud Stream** to subscribe to events.  
4. Integrate with external notification providers (like SendGrid or Twilio) in future enhancements.

---

## **14.1. Notification Service Overview**  
**Notification Service** will include:  
- **Send Enrollment Notification**: When a user enrolls in a course.  
- **Send Payment Notification**: When a user makes a payment.  
- **Send Course Update Notification**: When a course is updated.  

---

## **14.2. Setting Up the Project**  

### **Step 1: Create a New Spring Boot Project**  
**Go to [start.spring.io](https://start.spring.io)** and select:  
- **Project**: Maven  
- **Language**: Java  
- **Spring Boot Version**: 3.x.x  
- **Group**: `com.cashflowapp`  
- **Artifact**: `notification-service`  
- **Name**: `Notification Service`  
- **Description**: `Notification Management Microservice`  
- **Package Name**: `com.cashflowapp.notificationservice`  
- **Dependencies**:  
  - Spring Web  
  - Spring Cloud Netflix Eureka Client  
  - Lombok  
  - Spring Cloud Stream (for asynchronous communication)  
  - Spring Boot Starter Mail (for email notifications)  
  - Spring Security  
  - JWT (for authentication)  

**Download** the project and open it in **VSCode**.

---

### **Step 2: Project Structure**  
Your project structure should look like this:  
```
notification-service
├── src
│   ├── main
│   │   ├── java
│   │   │   └── com.cashflowapp.notificationservice
│   │   │       ├── controller
│   │   │       ├── model
│   │   │       ├── service
│   │   │       ├── event
│   │   │       └── NotificationServiceApplication.java
│   │   └── resources
│   │       ├── application.properties
│   │       └── application-dev.properties
└── pom.xml
```

---

### **Step 3: Configure application.properties**  
Configure Eureka client and Spring Cloud Stream for consuming events from Kafka.  

**src/main/resources/application.properties**  
```properties
spring.application.name=notification-service
server.port=8085

# Eureka Client Configuration
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/
eureka.instance.prefer-ip-address=true

# JWT Configuration
jwt.secret=MySecretKey

# Spring Cloud Stream Configuration (using Kafka)
spring.cloud.stream.defaultBinder=kafka
spring.cloud.stream.binders.kafka.environment.spring.kafka.bootstrap-servers=localhost:9092
spring.cloud.stream.bindings.paymentEvent-in-0.destination=payment-topic
spring.cloud.stream.bindings.enrollmentEvent-in-0.destination=enrollment-topic

# Email Configuration (Using Spring Boot Mail)
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=your-email@gmail.com
spring.mail.password=your-email-password
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
```

---

## **14.3. Create Event Classes**  

### **1. Payment Event**  
**src/main/java/com/cashflowapp/notificationservice/event/PaymentEvent.java**  
```java
package com.cashflowapp.notificationservice.event;

import lombok.Data;

@Data
public class PaymentEvent {
    private Long userId;
    private Long courseId;
    private String status;
}
```

---

### **2. Enrollment Event**  
**src/main/java/com/cashflowapp/notificationservice/event/EnrollmentEvent.java**  
```java
package com.cashflowapp.notificationservice.event;

import lombok.Data;

@Data
public class EnrollmentEvent {
    private Long userId;
    private Long courseId;
    private String status;
}
```

---

## **14.4. Create Service Class**  
**src/main/java/com/cashflowapp/notificationservice/service/NotificationService.java**  
```java
package com.cashflowapp.notificationservice.service;

import com.cashflowapp.notificationservice.event.EnrollmentEvent;
import com.cashflowapp.notificationservice.event.PaymentEvent;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.mail.SimpleMailMessage;
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.stereotype.Service;
import org.springframework.transaction.event.TransactionalEventListener;

@Service
public class NotificationService {

    @Autowired
    private JavaMailSender mailSender;

    public void sendPaymentNotification(PaymentEvent paymentEvent) {
        SimpleMailMessage message = new SimpleMailMessage();
        message.setTo("user@example.com");
        message.setSubject("Payment Successful");
        message.setText("Your payment for course ID " + paymentEvent.getCourseId() + " was successful.");
        mailSender.send(message);
    }

    public void sendEnrollmentNotification(EnrollmentEvent enrollmentEvent) {
        SimpleMailMessage message = new SimpleMailMessage();
        message.setTo("user@example.com");
        message.setSubject("Enrollment Successful");
        message.setText("You have successfully enrolled in course ID " + enrollmentEvent.getCourseId());
        mailSender.send(message);
    }
}
```

---

## **14.5. Create Event Listeners**  
**src/main/java/com/cashflowapp/notificationservice/event/EventListeners.java**  
```java
package com.cashflowapp.notificationservice.event;

import com.cashflowapp.notificationservice.service.NotificationService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.event.EventListener;
import org.springframework.messaging.Message;
import org.springframework.messaging.support.MessageBuilder;
import org.springframework.stereotype.Component;

@Component
public class EventListeners {

    @Autowired
    private NotificationService notificationService;

    @EventListener
    public void handlePaymentEvent(PaymentEvent paymentEvent) {
        if ("SUCCESS".equals(paymentEvent.getStatus())) {
            notificationService.sendPaymentNotification(paymentEvent);
        }
    }

    @EventListener
    public void handleEnrollmentEvent(EnrollmentEvent enrollmentEvent) {
        if ("ENROLLED".equals(enrollmentEvent.getStatus())) {
            notificationService.sendEnrollmentNotification(enrollmentEvent);
        }
    }
}
```

---

## **14.6. Secure Endpoints with Authentication**  
**src/main/java/com/cashflowapp/notificationservice/security/SecurityConfig.java**  
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .authorizeHttpRequests()
                .requestMatchers("/api/notifications/**").authenticated()
                .anyRequest().permitAll()
            .and()
            .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);

        return http.build();
    }
}
```

---

## **14.7. Run and Test the Notification Service**  
1. **Start Kafka**: Ensure **Apache Kafka** is running.  
2. **Start Notification Service** on port `8085`.  
3. **Test Notifications**: Trigger payment and enrollment events to verify email notifications.

---

## **Next Step**  
1. Test the **Notification Service** with **Postman** and Kafka events.  
2. Next, we’ll integrate all microservices with **Spring Cloud Gateway**.  
