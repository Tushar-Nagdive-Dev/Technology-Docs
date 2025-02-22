## 🚀 **Module 23: Real-World Case Studies and Applications**  

Design patterns are not just theoretical concepts but practical solutions applied in real-world systems. This module covers case studies and real-world applications of design patterns in popular frameworks, libraries, and enterprise systems.

---

## **🔥 23.1 Why Study Real-World Case Studies?**  
- **Practical Application:** Understand how design patterns solve real-world problems.  
- **Architectural Decisions:** Learn how architects make design decisions in complex systems.  
- **Performance Optimization:** Observe performance trade-offs and optimizations.  
- **Scalable Solutions:** See how scalable and maintainable systems are designed.  
- **Interview Preparation:** Prepare for system design interviews with real-world examples.  

---

## **🔥 23.2 Design Patterns in Popular Frameworks and Libraries**  
1. **Java SDK and Core Libraries:**  
    - **Singleton Pattern:** `Runtime.getRuntime()`  
    - **Factory Method Pattern:** `Calendar.getInstance()`  
    - **Observer Pattern:** `java.util.Observable` and `Observer`  
    - **Iterator Pattern:** `java.util.Iterator`  
    - **Decorator Pattern:** `java.io.BufferedInputStream` and `BufferedOutputStream`  

2. **Spring Framework:**  
    - **Singleton Pattern:** Spring Beans are Singleton by default.  
    - **Factory Pattern:** `BeanFactory` and `ApplicationContext` for Bean creation.  
    - **Proxy Pattern:** AOP (Aspect-Oriented Programming) uses Dynamic Proxy.  
    - **Template Method Pattern:** `JdbcTemplate` and `RestTemplate`  

3. **JDBC (Java Database Connectivity):**  
    - **Factory Pattern:** `DriverManager.getConnection()`  
    - **Adapter Pattern:** `ResultSet` and `PreparedStatement`  

4. **Hibernate:**  
    - **Proxy Pattern:** Lazy loading uses Proxies.  
    - **DAO Pattern:** Data Access Objects for persistence logic.  

5. **Android Development:**  
    - **Observer Pattern:** `ViewModel` and `LiveData` in MVVM architecture.  
    - **Strategy Pattern:** Different animations and transitions.  
    - **Factory Pattern:** `LayoutInflater` for dynamic UI components.  

---

## **🔥 23.3 Case Study 1: Singleton Pattern in Spring Framework**  
- **Scenario:** Spring Framework uses Singleton pattern for managing Beans.  
- **Problem Solved:** Ensures only one instance of a Bean per Spring container, reducing memory usage and improving performance.  
- **Implementation:**  
    - Spring Beans are Singleton by default.  
    - Managed by the Spring `ApplicationContext`.  
    - Lazy initialization is used to improve performance.  

### 📘 **Example Code: Singleton Bean in Spring**  
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {

    @Bean
    public SingletonService singletonService() {
        return new SingletonService();
    }
}

public class SingletonService {
    public SingletonService() {
        System.out.println("SingletonService Instance Created");
    }

    public void serve() {
        System.out.println("Serving using SingletonService");
    }
}

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class SpringSingletonExample {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

        SingletonService service1 = context.getBean(SingletonService.class);
        SingletonService service2 = context.getBean(SingletonService.class);

        service1.serve();
        service2.serve();

        System.out.println("Are both instances same? " + (service1 == service2));
    }
}
```

---

### 📊 **Output:**  
```
SingletonService Instance Created
Serving using SingletonService
Serving using SingletonService
Are both instances same? true
```

---

### 🔥 **Explanation:**  
- **Configuration Class (`AppConfig`):** Defines a Singleton Bean.  
- **Singleton Scope:** Spring manages the instance lifecycle and ensures a single instance per container.  
- **Dependency Injection:** Automatically injects the Singleton Bean.  
- **Time Complexity:** `O(1)` — Constant time for Bean retrieval.  
- **Space Complexity:** `O(1)` — Only one instance is created.  

---

## **🔥 23.4 Case Study 2: Factory Pattern in JDBC**  
- **Scenario:** JDBC uses the Factory pattern to create connections without exposing the instantiation logic.  
- **Problem Solved:** Decouples client code from database driver implementations.  
- **Implementation:**  
    - `DriverManager.getConnection()` creates a `Connection` object.  
    - The specific database driver is determined at runtime.  

### 📘 **Example Code: Factory Pattern in JDBC**  
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class JDBCFactoryExample {
    private static final String URL = "jdbc:mysql://localhost:3306/mydatabase";
    private static final String USER = "root";
    private static final String PASSWORD = "password";

    public static Connection getConnection() {
        try {
            return DriverManager.getConnection(URL, USER, PASSWORD);
        } catch (SQLException e) {
            throw new RuntimeException("Failed to create connection", e);
        }
    }

    public static void main(String[] args) {
        Connection connection = getConnection();
        if (connection != null) {
            System.out.println("Database Connection Established");
        } else {
            System.out.println("Failed to Establish Connection");
        }
    }
}
```

---

### 📊 **Output:**  
```
Database Connection Established
```

---

### 🔥 **Explanation:**  
- **Factory Method (`getConnection()`):** Hides connection creation logic.  
- **Runtime Binding:** Determines the driver implementation at runtime.  
- **Loose Coupling:** Decouples the client from specific driver classes.  
- **Time Complexity:** `O(1)` — Constant time for connection creation.  
- **Space Complexity:** `O(1)` — Only one connection object is created.  

---

## **🔥 23.5 Case Study 3: Proxy Pattern in Hibernate**  
- **Scenario:** Hibernate uses Proxy pattern for lazy loading of entities.  
- **Problem Solved:** Improves performance by loading related entities on demand.  
- **Implementation:**  
    - Hibernate uses CGLIB or Java Dynamic Proxy.  
    - A Proxy object is returned instead of the actual entity.  
    - The real object is loaded only when required (e.g., accessing a getter).  

---

### 📘 **Example: Proxy Pattern in Hibernate**  
```java
@Entity
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;

    @OneToMany(mappedBy = "employee", fetch = FetchType.LAZY)
    private List<Address> addresses;
}

@Entity
public class Address {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String city;

    @ManyToOne
    @JoinColumn(name = "employee_id")
    private Employee employee;
}
```

### 🔥 **Explanation:**  
- **Lazy Loading (`FetchType.LAZY`):** Returns a Proxy object for `addresses`.  
- **On-Demand Loading:** Actual `Address` objects are loaded only when accessed.  
- **Performance Optimization:** Reduces memory usage and improves performance.  

---

## **🔥 23.6 Real-World Applications of Design Patterns**  
1. **Microservices Architecture:**  
    - **Proxy Pattern** — API Gateway pattern for routing requests.  
    - **Factory Pattern** — Service creation with Dependency Injection.  
    - **Observer Pattern** — Event-driven communication with message queues.  

2. **E-commerce Systems:**  
    - **Strategy Pattern** — Payment gateways with different algorithms.  
    - **Decorator Pattern** — Dynamic pricing and discount systems.  

---

## 🔥 **Next: Recap and Advanced Exercises**  
Next, we will recap all the design patterns and provide **Advanced Exercises** to reinforce learning and test your understanding.

---
