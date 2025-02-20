# **Phase 3 - Step 16: Deploying Microservices with Docker**  

We have successfully built and integrated all the microservices using **Spring Cloud Gateway**. Now, it’s time to **containerize** and **deploy** the entire system using **Docker**. This involves:  
1. Containerizing all microservices with Docker.  
2. Creating a **Docker Compose** file to manage multi-container deployment.  
3. Deploying the entire microservices architecture locally using Docker Compose.  
4. Preparing for cloud deployment on platforms like **AWS** or **Kubernetes** (to be covered in later phases).

---

## **16.1. Why Use Docker?**  
- **Consistency**: Ensures consistent environments from development to production.  
- **Isolation**: Each microservice runs in its own container, preventing conflicts.  
- **Scalability**: Easily scale services by running multiple container instances.  
- **Portability**: Run the system on any environment that supports Docker.  

---

## **16.2. Prerequisites**  
Make sure you have the following installed:  
1. **Docker Desktop**: Ensure it is running and configured correctly.  
2. **Docker Compose**: Comes with Docker Desktop. Confirm with:  
```bash
docker-compose --version
```

---

## **16.3. Create Dockerfile for Each Microservice**  
We need to **containerize** all microservices:  
- **User Service**  
- **Course Service**  
- **Enrollment Service**  
- **Payment Service**  
- **Notification Service**  
- **API Gateway**  
- **Eureka Server**  

---

### **Step 1: Dockerfile Template**  
We will use a common Dockerfile template for all Spring Boot microservices. 

### **Example Dockerfile**  
Create a **Dockerfile** in the root directory of each microservice:

```Dockerfile
# Use OpenJDK as base image
FROM openjdk:17-jdk-slim

# Add a volume pointing to /tmp
VOLUME /tmp

# Expose the application port
EXPOSE 8080

# The application's JAR file
ARG JAR_FILE=target/*.jar

# Copy the JAR file to the container
COPY ${JAR_FILE} app.jar

# Run the application
ENTRYPOINT ["java","-jar","/app.jar"]
```

---

### **Step 2: Update Ports and Build Commands**  
- For each microservice, update the **EXPOSE** port to the respective port:
  - **User Service** → `8081`  
  - **Course Service** → `8082`  
  - **Enrollment Service** → `8083`  
  - **Payment Service** → `8084`  
  - **Notification Service** → `8085`  
  - **API Gateway** → `8080`  
  - **Eureka Server** → `8761`  

### **Build Docker Images**  
In each microservice directory, run:  
```bash
mvn clean package -DskipTests
docker build -t cashflowapp/<microservice-name>:latest .
```

**Example**:  
```bash
docker build -t cashflowapp/user-service:latest .
```

Repeat this for each microservice.

---

## **16.4. Create Docker Compose File**  
We will use **Docker Compose** to orchestrate the deployment of all microservices.

### **Step 1: docker-compose.yml**  
Create a **docker-compose.yml** file at the root level of your project:

```yaml
version: '3.8'

services:
  eureka-server:
    image: cashflowapp/eureka-server:latest
    container_name: eureka-server
    ports:
      - "8761:8761"
    networks:
      - cashflow-network

  user-service:
    image: cashflowapp/user-service:latest
    container_name: user-service
    ports:
      - "8081:8081"
    environment:
      - EUREKA_CLIENT_SERVICEURL_DEFAULTZONE=http://eureka-server:8761/eureka
    depends_on:
      - eureka-server
    networks:
      - cashflow-network

  course-service:
    image: cashflowapp/course-service:latest
    container_name: course-service
    ports:
      - "8082:8082"
    environment:
      - EUREKA_CLIENT_SERVICEURL_DEFAULTZONE=http://eureka-server:8761/eureka
    depends_on:
      - eureka-server
    networks:
      - cashflow-network

  enrollment-service:
    image: cashflowapp/enrollment-service:latest
    container_name: enrollment-service
    ports:
      - "8083:8083"
    environment:
      - EUREKA_CLIENT_SERVICEURL_DEFAULTZONE=http://eureka-server:8761/eureka
    depends_on:
      - eureka-server
    networks:
      - cashflow-network

  payment-service:
    image: cashflowapp/payment-service:latest
    container_name: payment-service
    ports:
      - "8084:8084"
    environment:
      - EUREKA_CLIENT_SERVICEURL_DEFAULTZONE=http://eureka-server:8761/eureka
    depends_on:
      - eureka-server
    networks:
      - cashflow-network

  notification-service:
    image: cashflowapp/notification-service:latest
    container_name: notification-service
    ports:
      - "8085:8085"
    environment:
      - EUREKA_CLIENT_SERVICEURL_DEFAULTZONE=http://eureka-server:8761/eureka
    depends_on:
      - eureka-server
    networks:
      - cashflow-network

  api-gateway:
    image: cashflowapp/api-gateway:latest
    container_name: api-gateway
    ports:
      - "8080:8080"
    environment:
      - EUREKA_CLIENT_SERVICEURL_DEFAULTZONE=http://eureka-server:8761/eureka
    depends_on:
      - eureka-server
    networks:
      - cashflow-network

networks:
  cashflow-network:
    driver: bridge
```

---

### **Step 2: Build and Start All Containers**  
In the root directory of your project (where the `docker-compose.yml` is located), run:  
```bash
docker-compose up --build
```

- **--build**: Ensures all images are rebuilt before starting containers.  
- **-d**: Run containers in detached mode.  

---

## **16.5. Verify Deployment**  
1. **Check Running Containers**  
```bash
docker ps
```

2. **Access Services**:  
- **API Gateway**: `http://localhost:8080`  
- **Eureka Server**: `http://localhost:8761` (Verify service registration)  
- **User Service**: `http://localhost:8081/api/users`  
- **Course Service**: `http://localhost:8082/api/courses`  
- **Enrollment Service**: `http://localhost:8083/api/enrollments`  
- **Payment Service**: `http://localhost:8084/api/payments`  
- **Notification Service**: `http://localhost:8085/api/notifications`  

3. **Test with Postman**:  
- Verify all APIs are accessible via the API Gateway.  
- Confirm JWT authentication and authorization are working correctly.  

---

## **16.6. Stop and Remove Containers**  
To stop and remove all containers, run:  
```bash
docker-compose down
```

---

## **Next Step**  
1. Test the entire microservices architecture with **Docker Compose**.  
2. Next, we’ll move on to **Phase 4: Cloud-Native Deployment using Kubernetes and CI/CD Pipelines**.  
