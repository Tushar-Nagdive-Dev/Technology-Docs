# 🚀 **Phase 2 - Lesson 17: Deploying Spring Boot Microservices using Docker**  

## **📌 Lesson Objective**  
By the end of this lesson, you will:  
✅ Understand **Docker & why it's used in microservices**  
✅ Learn how to **containerize a Spring Boot application**  
✅ Create a **Dockerfile & build a Docker image**  
✅ Run a **Spring Boot app inside a Docker container**  
✅ Use **Docker Compose** to manage multiple microservices  

---

## **1️⃣ What is Docker & Why Use It?**  
📌 **Docker** is a containerization platform that allows you to package applications and their dependencies into lightweight, portable containers.  

### **🔥 Why Use Docker for Microservices?**
✔ **Runs Anywhere** – Works on any system without compatibility issues.  
✔ **Lightweight** – Uses fewer resources compared to virtual machines.  
✔ **Scalable** – Easily scale microservices up/down.  
✔ **Fast Deployment** – No need to install dependencies manually.  

✅ **Use Docker when:**  
- You want **consistent deployments** across environments (Dev → QA → Prod).  
- You need to **isolate microservices** for better scaling.  

---

## **2️⃣ Installing Docker (If Not Installed)**  
📌 Install Docker from: **[Docker Official Website](https://www.docker.com/get-started/)**  

📌 **Verify Installation**  
```bash
docker --version
```
✔ Expected output:  
```
Docker version 20.xx.xx, build xxxxxx
```

---

## **3️⃣ Containerizing a Spring Boot Application**  
📌 **Step 1: Create a Simple Spring Boot App (If Not Created)**  
Use [Spring Initializr](https://start.spring.io/) with dependencies:  
- ✅ Spring Web  
- ✅ Spring Boot Actuator  

📌 **Step 2: Create a Simple Controller (`HelloController.java`)**  
```java
package com.example.demo.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api")
public class HelloController {
    @GetMapping("/hello")
    public String sayHello() {
        return "Hello from Dockerized Spring Boot App!";
    }
}
```

📌 **Step 3: Build the JAR File**  
```bash
mvn clean package
```
✔ The JAR file will be generated in `target/` directory.

---

## **4️⃣ Writing a Dockerfile for Spring Boot App**  
📌 **Step 4: Create a `Dockerfile` in the Root Directory**  
```dockerfile
# Use official OpenJDK image as base
FROM openjdk:17-jdk-slim

# Set working directory
WORKDIR /app

# Copy JAR file into the container
COPY target/*.jar app.jar

# Expose the application port
EXPOSE 8080

# Run the application
ENTRYPOINT ["java", "-jar", "app.jar"]
```

✅ **What’s happening?**
- `FROM openjdk:17-jdk-slim` → Uses OpenJDK 17 as the base image.  
- `WORKDIR /app` → Sets the working directory inside the container.  
- `COPY target/*.jar app.jar` → Copies the JAR file into the container.  
- `EXPOSE 8080` → Exposes port 8080 for external access.  
- `ENTRYPOINT ["java", "-jar", "app.jar"]` → Runs the Spring Boot app.  

---

## **5️⃣ Building & Running the Docker Container**  
📌 **Step 5: Build the Docker Image**  
```bash
docker build -t spring-boot-app .
```
✔ Expected output:  
```
Successfully built spring-boot-app
```

📌 **Step 6: Run the Docker Container**  
```bash
docker run -p 8080:8080 spring-boot-app
```
✔ Now your Spring Boot app is running inside Docker! 🎉  

📌 **Step 7: Test the API**  
```bash
curl -X GET "http://localhost:8080/api/hello"
```
✔ Expected response:  
```
Hello from Dockerized Spring Boot App!
```

---

## **6️⃣ Managing Microservices with Docker Compose**  
If you have multiple microservices (e.g., `user-service`, `order-service`), Docker Compose helps manage them.

📌 **Step 1: Create a `docker-compose.yml`**  
```yaml
version: '3.8'
services:
  user-service:
    build: ./user-service
    ports:
      - "8081:8081"
    depends_on:
      - db

  order-service:
    build: ./order-service
    ports:
      - "8082:8082"
    depends_on:
      - db

  db:
    image: postgres:14
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: microservices_db
    ports:
      - "5432:5432"
```

📌 **Step 2: Run All Microservices with One Command**  
```bash
docker-compose up -d
```
✔ Now **User Service, Order Service, and Database** are running in separate containers!  

📌 **Step 3: Stop All Services**  
```bash
docker-compose down
```

---

## **7️⃣ Pushing Docker Images to Docker Hub**  
📌 **Step 1: Log in to Docker Hub**  
```bash
docker login
```

📌 **Step 2: Tag and Push the Image**  
```bash
docker tag spring-boot-app your-dockerhub-username/spring-boot-app
docker push your-dockerhub-username/spring-boot-app
```

📌 **Step 3: Run from Any Server**  
```bash
docker run -p 8080:8080 your-dockerhub-username/spring-boot-app
```
✔ Now your **Spring Boot app is deployable anywhere!** 🚀  

---

## 🎯 **Lesson 17 - Summary**  
✅ Built a **Docker image for a Spring Boot app**  
✅ Created a **Dockerfile to containerize the application**  
✅ Ran the **Spring Boot app inside a Docker container**  
✅ Used **Docker Compose to manage multiple microservices**  
✅ Pushed the **Docker image to Docker Hub**  

---
