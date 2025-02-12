# 🚀 **Phase 1 - Lesson 3: Dependency Injection & Spring Boot Beans** 🚀  

## **📌 Lesson Objective**
By the end of this lesson, you will:  
✅ Understand **Dependency Injection (DI) and Inversion of Control (IoC)**  
✅ Learn about **Spring Boot Beans and their lifecycle**  
✅ Explore different types of **Dependency Injection**  
✅ Use **`@Component`, `@Service`, `@Repository`, and `@Autowired`**  
✅ Implement **Manual and Auto Bean Configuration**  

---

## **1️⃣ What is Dependency Injection (DI)?**
### 🔹 **Definition**
Dependency Injection (DI) is a **design pattern** used in Spring Boot where objects (dependencies) are **automatically provided** to classes instead of being manually created.

In simpler terms:  
👉 **Instead of creating objects manually using `new`**, Spring Boot **injects them for you**.

### 🔹 **Example Without DI (Traditional Approach)**
```java
public class UserService {
    private UserRepository userRepository = new UserRepository(); // Manual object creation

    public void getUser() {
        userRepository.findUser();
    }
}
```
❌ **Issue**: `UserService` **directly depends on** `UserRepository`, making it **hard to test and modify**.

---

## **2️⃣ What is Inversion of Control (IoC)?**
**IoC** means the **control over object creation and management is given to the Spring framework**, instead of us handling it manually.

### 🔹 **Example With DI (Using Spring Boot)**
```java
@Service
public class UserService {
    private final UserRepository userRepository;

    @Autowired // Spring Boot injects UserRepository automatically
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public void getUser() {
        userRepository.findUser();
    }
}
```
💡 **Spring Boot creates and injects `UserRepository` automatically!** 🚀

---

## **3️⃣ What are Spring Boot Beans?**
A **Bean** is an object managed by the **Spring IoC container**.

Spring Boot **automatically detects and registers Beans** using annotations like:
- `@Component`
- `@Service`
- `@Repository`
- `@Controller`

👉 **Beans are automatically created, managed, and injected wherever needed.**

---

## **4️⃣ Creating and Injecting Beans in Spring Boot**
### **🔹 Step 1: Define a Simple Bean using `@Component`**
```java
package com.example.demo.service;

import org.springframework.stereotype.Component;

@Component  // Marks this class as a Spring Bean
public class MyService {
    public String getMessage() {
        return "Hello from MyService!";
    }
}
```
Spring Boot **automatically detects and manages this bean**.

---

### **🔹 Step 2: Inject the Bean into a Controller using `@Autowired`**
```java
package com.example.demo.controller;

import com.example.demo.service.MyService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api")
public class MyController {
    private final MyService myService;

    @Autowired  // Injecting MyService Bean
    public MyController(MyService myService) {
        this.myService = myService;
    }

    @GetMapping("/message")
    public String showMessage() {
        return myService.getMessage();  // Calls the MyService method
    }
}
```

### **🔹 Step 3: Run & Test**
1️⃣ **Run the Spring Boot application** (`mvn spring-boot:run`).  
2️⃣ **Test the API** in the browser or Postman:
   ```
   http://localhost:8080/api/message
   ```
3️⃣ **Output:**
   ```
   Hello from MyService!
   ```

🎉 **Congratulations! You just implemented Dependency Injection in Spring Boot!** 🎉

---

## **5️⃣ Spring Boot Bean Annotations**
Spring Boot provides different **annotations** to register Beans in the Spring IoC container.

| Annotation | Purpose |
|------------|---------|
| `@Component` | Generic Spring Bean |
| `@Service` | Specialized for **business logic/services** |
| `@Repository` | Specialized for **database repositories** |
| `@Controller` | Handles **web requests** (Spring MVC) |

💡 **Spring Boot automatically detects these annotations and registers them as Beans.**

---

## **6️⃣ Types of Dependency Injection**
Spring Boot supports **three types of Dependency Injection**:

| Type | Example | Use Case |
|------|---------|----------|
| **Constructor Injection** (✅ Recommended) | `@Autowired on constructor` | Best for **mandatory dependencies** |
| **Field Injection** (❌ Not recommended) | `@Autowired on field` | Difficult to test and modify |
| **Setter Injection** | `@Autowired on setter method` | Best for **optional dependencies** |

---

### **✅ Constructor Injection (Best Practice)**
💡 Recommended because it ensures **mandatory dependencies** are injected.

```java
@Service
public class OrderService {
    private final OrderRepository orderRepository;

    @Autowired  // Constructor Injection
    public OrderService(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }
}
```

---

### **❌ Field Injection (Not Recommended)**
⚠️ Hard to test and not flexible.

```java
@Service
public class OrderService {
    @Autowired  // Field Injection (Not recommended)
    private OrderRepository orderRepository;
}
```

---

### **✅ Setter Injection (For Optional Dependencies)**
Use when **a dependency is optional**.

```java
@Service
public class PaymentService {
    private PaymentGateway paymentGateway;

    @Autowired
    public void setPaymentGateway(PaymentGateway paymentGateway) {
        this.paymentGateway = paymentGateway;
    }
}
```

---

## **7️⃣ Manual Bean Configuration (Using `@Bean`)**
If you don’t want to use `@Component`, you can manually **define a Bean using `@Bean`** inside a `@Configuration` class.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {
    
    @Bean
    public MyService myService() {
        return new MyService();  // Manual Bean creation
    }
}
```
💡 **Spring Boot will manage this Bean like an `@Component` Bean.**

---

## 🎯 **Lesson 3 - Summary**
✅ **Dependency Injection (DI)** automatically injects objects instead of manually creating them.  
✅ **Spring Boot Beans** are managed objects inside the Spring IoC container.  
✅ `@Component`, `@Service`, `@Repository`, and `@Controller` create Spring Beans.  
✅ **Constructor Injection** is the **best practice** for DI.  
✅ **`@Bean` annotation** can be used for **manual Bean configuration**.  

---

## **💻 Hands-on Exercise: Implement DI in Spring Boot**
🎯 **Task**:  
1️⃣ **Create a new service** called `GreetingService` that returns `"Welcome to Spring Boot!"`.  
2️⃣ **Inject this service** into a Controller and expose an endpoint `/api/greet`.  
3️⃣ **Run the application** and test it in Postman.  

---
