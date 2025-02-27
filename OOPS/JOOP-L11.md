### **Level 2: Intermediate - Dependency Inversion Principle (DIP)**  

---

## **1. What is Dependency Inversion Principle (DIP)?**
**Dependency Inversion Principle (DIP)** states:
> *"High-level modules should not depend on low-level modules. Both should depend on abstractions."*  
> *"Abstractions should not depend on details. Details should depend on abstractions."*

### **Key Points:**
- **High-level modules** (business logic) should not be tightly coupled to **low-level modules** (concrete implementations).
- Both should depend on **abstractions** (interfaces or abstract classes).
- Ensures **loose coupling** and **high cohesion**.

---

## **2. Why is DIP Important?**
1. **Loose Coupling:**
   - DIP reduces dependencies between components, making the system more flexible.
2. **Maintainability and Extensibility:**
   - Changes in low-level modules don’t affect high-level modules.
   - New implementations can be added without modifying high-level modules.
3. **Testability:**
   - DIP makes unit testing easier by allowing the use of mock implementations.
4. **Scalable Architecture:**
   - Promotes layered architecture and dependency injection patterns.

---

## **3. Violation of DIP: Example and Problem Analysis**
### **Scenario: Notification Service**
- A `NotificationService` sends notifications through different channels:
  - **Email**
  - **SMS**
- Currently, it directly depends on the `EmailService` and `SMSService` classes.

### **Incorrect Design: Tight Coupling**
```java
public class EmailService {
    public void sendEmail(String message) {
        System.out.println("Sending Email: " + message);
    }
}

public class SMSService {
    public void sendSMS(String message) {
        System.out.println("Sending SMS: " + message);
    }
}
```

```java
public class NotificationService {
    private EmailService emailService;
    private SMSService smsService;

    public NotificationService() {
        this.emailService = new EmailService();
        this.smsService = new SMSService();
    }

    public void sendNotification(String message, String type) {
        if (type.equals("EMAIL")) {
            emailService.sendEmail(message);
        } else if (type.equals("SMS")) {
            smsService.sendSMS(message);
        }
    }
}
```

### **Usage:**
```java
public class Main {
    public static void main(String[] args) {
        NotificationService notificationService = new NotificationService();
        notificationService.sendNotification("Hello Email", "EMAIL");
        notificationService.sendNotification("Hello SMS", "SMS");
    }
}
```

### **Output:**
```
Sending Email: Hello Email
Sending SMS: Hello SMS
```

---

### **What's Wrong and Why DIP is Violated?**
- **NotificationService** is tightly coupled to the **EmailService** and **SMSService**.
- If a new notification type (e.g., Push Notification) is added:
  - **NotificationService** must be modified.
  - **Open/Closed Principle** is violated.
- **DIP Violation**: High-level module (`NotificationService`) directly depends on low-level modules (`EmailService` and `SMSService`).
- It should depend on an **abstraction** (e.g., an interface), not on concrete implementations.

---

## **4. Solution: Refactor to Respect DIP**
### **Step 1: Create an Abstraction (Interface)**
```java
public interface Notification {
    void send(String message);
}
```

### **Step 2: Implement Concrete Classes**
```java
public class EmailService implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending Email: " + message);
    }
}
```

```java
public class SMSService implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending SMS: " + message);
    }
}
```

### **Step 3: Dependency Injection in NotificationService**
```java
public class NotificationService {
    private Notification notification;

    // Constructor Injection
    public NotificationService(Notification notification) {
        this.notification = notification;
    }

    public void sendNotification(String message) {
        notification.send(message);
    }
}
```

### **Step 4: Usage with Dependency Injection**
```java
public class Main {
    public static void main(String[] args) {
        // Dependency Injection through constructor
        Notification emailNotification = new EmailService();
        NotificationService emailService = new NotificationService(emailNotification);
        emailService.sendNotification("Hello Email");

        Notification smsNotification = new SMSService();
        NotificationService smsService = new NotificationService(smsNotification);
        smsService.sendNotification("Hello SMS");
    }
}
```

### **Output:**
```
Sending Email: Hello Email
Sending SMS: Hello SMS
```

---

### **What's Improved and Why DIP is Preserved?**
- **NotificationService** now depends on the **Notification** interface, not on concrete classes.
- **EmailService** and **SMSService** implement the **Notification** interface.
- **Loose Coupling**:
  - `NotificationService` is loosely coupled with `EmailService` and `SMSService`.
  - New notification types (e.g., Push Notification) can be added without modifying `NotificationService`.
- **Dependency Injection** is used to inject dependencies, enhancing testability and maintainability.
- **Open/Closed Principle** is also preserved:
  - New implementations of `Notification` can be added without changing the existing code.

---

## **5. Implementing DIP using Dependency Injection (DI)**
- **Dependency Injection** is a design pattern to implement DIP.
- It involves passing dependencies to a class, rather than the class creating them itself.
- Types of Dependency Injection:
  1. **Constructor Injection:** Dependencies are passed through the constructor.
  2. **Setter Injection:** Dependencies are passed through setter methods.
  3. **Interface Injection:** Dependencies are passed through an interface method.

---

### **Example: Constructor Injection**
```java
public class NotificationService {
    private Notification notification;

    // Constructor Injection
    public NotificationService(Notification notification) {
        this.notification = notification;
    }

    public void sendNotification(String message) {
        notification.send(message);
    }
}
```

### **Example: Setter Injection**
```java
public class NotificationService {
    private Notification notification;

    // Setter Injection
    public void setNotification(Notification notification) {
        this.notification = notification;
    }

    public void sendNotification(String message) {
        notification.send(message);
    }
}
```

### **Usage:**
```java
public class Main {
    public static void main(String[] args) {
        NotificationService service = new NotificationService();

        // Using Setter Injection
        service.setNotification(new EmailService());
        service.sendNotification("Hello Email");

        service.setNotification(new SMSService());
        service.sendNotification("Hello SMS");
    }
}
```

### **Output:**
```
Sending Email: Hello Email
Sending SMS: Hello SMS
```

---

## **6. Guidelines for Following DIP**
1. **Depend on Abstractions:**
   - Depend on interfaces or abstract classes, not on concrete implementations.
2. **Use Dependency Injection:**
   - Use Constructor Injection or Setter Injection to inject dependencies.
3. **Separate High-level and Low-level Modules:**
   - High-level modules should focus on business logic.
   - Low-level modules should focus on implementation details.
4. **Apply Inversion of Control (IoC):**
   - Inversion of Control frameworks like **Spring** can be used to manage dependencies automatically.

---

## **7. Common Mistakes to Avoid:**
1. **Directly Instantiating Dependencies:**
   - Avoid creating objects directly in high-level modules.
2. **Tightly Coupled Dependencies:**
   - Do not hardcode dependencies. Use Dependency Injection.
3. **Violating Open/Closed Principle:**
   - Avoid modifying high-level modules when adding new features.

---

## **8. What's Next?**
We’ve now mastered:
- **Dependency Inversion Principle (DIP)**:
  - High-level modules depend on abstractions.
  - Low-level modules implement abstractions.
  - Achieved using Dependency Injection and Inversion of Control.

### Next Up: **Design Patterns**
- **Creational Patterns**:
  - Singleton, Factory, Builder, Prototype.
- **Structural Patterns**:
  - Adapter, Composite, Proxy, Facade, Decorator.
- **Behavioral Patterns**:
  - Strategy, Observer, Template Method, Chain of Responsibility.

