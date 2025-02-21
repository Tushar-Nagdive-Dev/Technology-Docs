---

# **JUnit Mastery Project: Building a Fully-Tested Microservices Application**

---

## **Project Overview**

You are about to embark on the **JUnit Mastery Project**, designed to solidify your expertise in JUnit testing. This project involves building a **Spring Boot microservices application** with:
- **Comprehensive Unit Tests** using JUnit and Mockito.
- **Integration Tests** with TestContainers.
- **End-to-End Tests** with Selenium.
- **Performance Benchmarks** using JMH.
- **80%+ Code Coverage** using JaCoCo.
- **CI/CD Integration** using **GitHub Actions** or **GitLab CI**.

---

## **1. Project Architecture**

The application is an **Order Management System** built with the following microservices:
1. **User Service**: Manages user accounts.
2. **Order Service**: Handles order creation and tracking.
3. **Product Service**: Manages product catalog and inventory.

### **Technology Stack:**
- **Spring Boot** for microservices.
- **Spring Data JPA** with **PostgreSQL** for data persistence.
- **Spring Cloud** for service discovery and communication.
- **JUnit 5** for unit and integration testing.
- **Mockito** for mocking dependencies.
- **TestContainers** for integration testing with PostgreSQL.
- **Selenium** for end-to-end testing.
- **JMH** for performance benchmarks.
- **JaCoCo** for code coverage.
- **GitHub Actions** or **GitLab CI** for CI/CD.

---

## **2. Microservices Overview**

### **1. User Service**
- **Endpoints:**
  - `POST /api/users`: Register a new user.
  - `GET /api/users/{id}`: Get user details by ID.
- **Entity:** `User`
  - `id`: Long
  - `name`: String
  - `email`: String

### **2. Product Service**
- **Endpoints:**
  - `POST /api/products`: Add a new product.
  - `GET /api/products/{id}`: Get product details by ID.
- **Entity:** `Product`
  - `id`: Long
  - `name`: String
  - `price`: Double
  - `quantity`: Integer

### **3. Order Service**
- **Endpoints:**
  - `POST /api/orders`: Create a new order.
  - `GET /api/orders/{id}`: Get order details by ID.
- **Entity:** `Order`
  - `id`: Long
  - `userId`: Long
  - `productId`: Long
  - `quantity`: Integer
  - `totalPrice`: Double

---

## **3. Project Phases**

The project is divided into **7 Phases**:

### **Phase 1: Project Setup and Architecture Design**
- Set up the microservices using **Spring Boot**.
- Use **Spring Cloud Netflix Eureka** for service discovery.
- Create separate modules for **User Service**, **Product Service**, and **Order Service**.
- Set up **PostgreSQL** for each service.

### **Phase 2: Unit Testing with JUnit and Mockito**
- Write comprehensive **Unit Tests** for:
  - Service layer methods.
  - Repository layer methods.
- Use **Mockito** for mocking dependencies.

### **Phase 3: Integration Testing with TestContainers**
- Test the **User Service** with a real **PostgreSQL** container.
- Mock dependencies using **@MockBean**.
- Test the interaction between the **Order Service** and **Product Service**.

### **Phase 4: End-to-End Testing with Selenium**
- Automate E2E tests for:
  - User registration and login.
  - Product addition and listing.
  - Order creation and tracking.
- Use **Selenium WebDriver** for browser automation.

### **Phase 5: Performance Benchmarking with JMH**
- Benchmark:
  - `OrderService.createOrder()`
  - `ProductService.getProductById()`
- Measure average execution time and throughput.

### **Phase 6: Code Coverage with JaCoCo**
- Integrate **JaCoCo** to measure code coverage.
- Ensure **80%+ code coverage** for all microservices.

### **Phase 7: CI/CD Integration**
- Integrate **GitHub Actions** or **GitLab CI** to:
  - **Build** the application.
  - **Run all tests** (Unit, Integration, E2E).
  - **Generate code coverage reports**.
  - **Deploy** the application to **Docker Hub** or **Kubernetes**.

---

## **4. Tools and Prerequisites**

### **1. Prerequisites:**
- **Java 17** or later
- **Maven** or **Gradle**
- **Docker** (for TestContainers and CI/CD)
- **PostgreSQL**
- **ChromeDriver** (for Selenium)
- **IntelliJ IDEA** or **VS Code** (with JUnit and Docker support)

### **2. Tools and Libraries:**
- **Spring Boot** (3.x) with Spring Data JPA and Spring Cloud Netflix Eureka
- **JUnit 5** and **Mockito** for testing
- **TestContainers** for integration testing
- **Selenium** for E2E testing
- **JMH** for performance benchmarking
- **JaCoCo** for code coverage
- **GitHub Actions** or **GitLab CI** for CI/CD

---

## **5. Phase 1: Project Setup and Architecture Design**

### **Step 1: Create a Multi-Module Maven Project**

```bash
mvn archetype:generate \
  -DgroupId=com.example \
  -DartifactId=order-management-system \
  -DarchetypeArtifactId=maven-archetype-quickstart \
  -DinteractiveMode=false
```

### **Step 2: Create Submodules**
- **user-service**
- **product-service**
- **order-service**
- **discovery-server** (Eureka)

```bash
cd order-management-system
mvn archetype:generate -DgroupId=com.example.user -DartifactId=user-service
mvn archetype:generate -DgroupId=com.example.product -DartifactId=product-service
mvn archetype:generate -DgroupId=com.example.order -DartifactId=order-service
mvn archetype:generate -DgroupId=com.example.discovery -DartifactId=discovery-server
```

### **Step 3: Configure Parent POM**
Edit the `order-management-system/pom.xml`:
```xml
<modules>
    <module>user-service</module>
    <module>product-service</module>
    <module>order-service</module>
    <module>discovery-server</module>
</modules>
```

---

### **Step 4: Service Discovery with Spring Cloud Netflix Eureka**

- Add the Eureka Server dependency in `discovery-server/pom.xml`:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```

- **Enable Eureka Server**:
```java
@SpringBootApplication
@EnableEurekaServer
public class DiscoveryServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(DiscoveryServerApplication.class, args);
    }
}
```

- **Configure Application Properties**:
```properties
server.port=8761
eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false
```

### **Step 5: Register Services with Eureka**

- Add Eureka Client dependency in each service:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

- **Configure Application Properties** for `user-service`, `product-service`, and `order-service`:
```properties
eureka.client.service-url.defaultZone=http://localhost:8761/eureka
spring.application.name=user-service
```

---

## **Next Steps: Phase 2 - Unit Testing with JUnit and Mockito**

In the next phase, we'll:
- Write **Unit Tests** for service and repository layers.
- Use **Mockito** to mock dependencies.
- Achieve high code coverage with **JaCoCo**.
