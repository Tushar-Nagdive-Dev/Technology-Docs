---

## **Lesson 75: Angular + Spring Boot Integration with Containerization and Deployment**

---

## **What You Will Learn:**
1. **Introduction to Containerization and Deployment**
   - What is Containerization?
   - Why Use Containers for Angular + Spring Boot?
   - Overview of Docker and Docker Compose

2. **Containerizing Angular Application**
   - Creating a Dockerfile for Angular
   - Building and Running Angular Docker Image
   - Serving Angular as Static Files with Nginx

3. **Containerizing Spring Boot Application**
   - Creating a Dockerfile for Spring Boot
   - Building and Running Spring Boot Docker Image
   - Configuring Environment Variables for Production

4. **Docker Compose for Full-Stack Integration**
   - Combining Angular and Spring Boot Containers
   - Networking Between Angular and Spring Boot
   - Using Docker Compose to Orchestrate Services

5. **Deploying on Cloud Platforms**
   - Preparing for Cloud Deployment
   - Deploying on **AWS EC2** with Docker Compose
   - Deploying on **Azure App Service** with Docker
   - CI/CD Integration using **GitHub Actions** and **GitLab CI/CD**

---

## **1. Introduction to Containerization and Deployment**

---

### **1.1 What is Containerization?**
- **Containerization** is the process of **packaging an application** and its dependencies into a **lightweight, portable container**.
- Containers are **platform-independent** and **consistent across environments**.
- They **run the same way** on development, testing, and production systems.

---

### **1.2 Why Use Containers for Angular + Spring Boot?**
- **Consistent Environment**:
  - Ensures consistency across different stages (dev, test, prod).
- **Simplified Deployment**:
  - Packages Angular frontend and Spring Boot backend as separate containers.
  - **Docker Compose** simplifies orchestration of multi-container applications.
- **Scalability**:
  - Easily scale Angular and Spring Boot containers **independently**.
- **Cross-platform Portability**:
  - Containers **run consistently** on any system with Docker support.

---

### **1.3 Overview of Docker and Docker Compose**
- **Docker**:
  - An open-source platform for containerization.
  - Uses **Dockerfiles** to define application images.
- **Docker Compose**:
  - A tool to **define and run multi-container Docker applications**.
  - Uses a **docker-compose.yml** file to configure services and networking.

---

## **2. Containerizing Angular Application**

---

### **2.1 Creating a Dockerfile for Angular**

---

#### **2.1.1 Dockerfile for Angular**

- We will create a **Dockerfile** for Angular to:
  - **Build Angular application** using Node.js.
  - **Serve static files** using **Nginx** for production.

**Example: Dockerfile for Angular**

```dockerfile
# Step 1: Build Angular Application
FROM node:18 AS build

# Set working directory
WORKDIR /app

# Copy package.json and install dependencies
COPY package*.json ./
RUN npm install

# Copy application source code
COPY . .

# Build the Angular app
RUN npm run build --prod

# Step 2: Serve with Nginx
FROM nginx:alpine

# Copy build output to Nginx static directory
COPY --from=build /app/dist/angular-springboot-frontend /usr/share/nginx/html

# Expose port
EXPOSE 80

# Start Nginx server
CMD ["nginx", "-g", "daemon off;"]
```

- **FROM node:18 AS build**:
  - Uses **Node.js** as the base image for building the Angular app.
- **RUN npm run build --prod**:
  - Builds the Angular application in **production mode**.
- **FROM nginx:alpine**:
  - Uses **Nginx** as the web server to serve static files.
- **COPY --from=build**:
  - Copies the **Angular build output** from the previous stage.
- **EXPOSE 80**:
  - Exposes port `80` for HTTP traffic.
- **CMD ["nginx", "-g", "daemon off;"]**:
  - Starts the **Nginx server**.

---

### **2.2 Building and Running Angular Docker Image**

```sh
# Build the Angular Docker image
docker build -t angular-frontend .

# Run the Angular container
docker run -d -p 8080:80 --name angular-container angular-frontend
```

- **docker build -t angular-frontend .**:
  - Builds the Docker image with the tag `angular-frontend`.
- **docker run -d -p 8080:80**:
  - Runs the Angular container in **detached mode**.
  - Maps port `8080` on the host to port `80` in the container.

---

### **2.3 Accessing Angular Application**

- Open a browser and navigate to:
```
http://localhost:8080
```

- The Angular application should be **served by Nginx**.

---

## **3. Containerizing Spring Boot Application**

---

### **3.1 Creating a Dockerfile for Spring Boot**

---

#### **3.1.1 Dockerfile for Spring Boot**

- We will create a **Dockerfile** for Spring Boot to:
  - **Build the JAR file** using Maven.
  - **Run the Spring Boot application** in a lightweight container.

**Example: Dockerfile for Spring Boot**

```dockerfile
# Step 1: Build JAR File
FROM maven:3.8.5-openjdk-17 AS build

# Set working directory
WORKDIR /app

# Copy pom.xml and download dependencies
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy application source code
COPY src ./src

# Package the application
RUN mvn clean package -DskipTests

# Step 2: Run Spring Boot Application
FROM eclipse-temurin:17-jre-alpine

# Set working directory
WORKDIR /app

# Copy JAR file from the build stage
COPY --from=build /app/target/*.jar app.jar

# Expose port
EXPOSE 8080

# Start Spring Boot application
ENTRYPOINT ["java", "-jar", "app.jar"]
```

- **FROM maven:3.8.5-openjdk-17 AS build**:
  - Uses **Maven with JDK 17** to build the Spring Boot application.
- **RUN mvn clean package -DskipTests**:
  - Packages the application as a **JAR file**.
- **FROM eclipse-temurin:17-jre-alpine**:
  - Uses a lightweight **OpenJDK 17 runtime** for the final container.
- **ENTRYPOINT ["java", "-jar", "app.jar"]**:
  - Starts the **Spring Boot application**.

---

### **3.2 Building and Running Spring Boot Docker Image**

```sh
# Build the Spring Boot Docker image
docker build -t springboot-backend .

# Run the Spring Boot container
docker run -d -p 8081:8080 --name springboot-container springboot-backend
```

- **docker build -t springboot-backend .**:
  - Builds the Docker image with the tag `springboot-backend`.
- **docker run -d -p 8081:8080**:
  - Runs the Spring Boot container in **detached mode**.
  - Maps port `8081` on the host to port `8080` in the container.

---

### **3.3 Accessing Spring Boot Application**

- Open a browser and navigate to:
```
http://localhost:8081/api
```

- The Spring Boot API endpoints should be **accessible**.

---

## **4. Docker Compose for Full-Stack Integration**

---

### **4.1 Creating docker-compose.yml**

- We will use **Docker Compose** to:
  - **Integrate Angular and Spring Boot containers**.
  - **Network** them together using a shared Docker network.
  - Manage **dependencies** and **orchestration**.

**Example: docker-compose.yml**

```yaml
version: '3.8'
services:
  angular-frontend:
    image: angular-frontend
    build:
      context: ./frontend
      dockerfile: Dockerfile
    ports:
      - "8080:80"
    depends_on:
      - springboot-backend

  springboot-backend:
    image: springboot-backend
    build:
      context: ./backend
      dockerfile: Dockerfile
    ports:
      - "8081:8080"
```

- **depends_on** – Ensures that **Spring Boot** starts before **Angular**.
- **ports** – Maps container ports to host ports.

---

### **4.2 Running Docker Compose**

```sh
# Start all containers using Docker Compose
docker-compose up --build
```

- **docker-compose up --build**:
  - Builds and starts all containers as defined in `docker-compose.yml`.

---

## **Next Steps:**
- **5. Deploying on Cloud Platforms**
  - **AWS EC2 with Docker Compose**
  - **Azure App Service with Docker**
  - **CI/CD with GitHub Actions and GitLab CI/CD**

---

## **Lesson 75 (Continued): Deploying Angular + Spring Boot on Cloud Platforms**

---

## **5. Deploying on Cloud Platforms**

In this section, we will:
- **Prepare for Cloud Deployment** by configuring Docker images for production.
- **Deploy Angular + Spring Boot** on:
  - **AWS EC2** using Docker Compose.
  - **Azure App Service** with Docker.
- **Automate CI/CD** with:
  - **GitHub Actions** for CI/CD on AWS.
  - **GitLab CI/CD** for deployment on Azure.

---

## **5.1 Preparing for Cloud Deployment**

---

### **5.1.1 Updating application.properties for Production**

- We will **update Spring Boot configurations** for cloud deployment:
  - **Enable CORS** for communication between Angular and Spring Boot.
  - **Configure environment-specific properties** for production.

---

#### **5.1.1.1 CORS Configuration in Spring Boot**

**Example: WebConfig Class**

```java
package com.example.demo.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig {

    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/**")
                        .allowedOrigins("http://localhost:8080", "https://your-production-domain.com")
                        .allowedMethods("GET", "POST", "PUT", "DELETE")
                        .allowedHeaders("*")
                        .allowCredentials(true);
            }
        };
    }
}
```

- **allowedOrigins**:
  - Allows requests from:
    - **Development environment**: `http://localhost:8080`
    - **Production environment**: `https://your-production-domain.com`
- **allowedMethods** – Specifies allowed HTTP methods.
- **allowCredentials(true)** – Allows cookies and authorization headers.

---

#### **5.1.1.2 application.properties (Production)**

```properties
# Production Database Configuration
spring.datasource.url=jdbc:mysql://prod-db-endpoint:3306/your-database
spring.datasource.username=your-username
spring.datasource.password=your-password
spring.jpa.hibernate.ddl-auto=none
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect

# Production Server Configuration
server.port=8080

# JWT Configuration
jwt.secret=YourSecretKeyHere
jwt.access.expiration=900000
jwt.refresh.expiration=604800000
```

- **spring.datasource.url** – Points to the **production database**.
- **server.port=8080** – Configures the **production server port**.

---

## **5.2 Deploying on AWS EC2 using Docker Compose**

---

### **5.2.1 Setting up an EC2 Instance on AWS**

1. **Login to AWS Console** – https://aws.amazon.com/
2. **Navigate to EC2 Dashboard**:
   - **Launch Instance** – Choose **Amazon Linux 2 AMI**.
   - **Instance Type** – Select `t2.micro` (free tier eligible).
   - **Security Group**:
     - Allow **HTTP (80)** and **HTTPS (443)**.
     - Allow **Custom TCP (8080, 8081)** for Angular and Spring Boot.
     - Allow **SSH (22)** for terminal access.
   - **Key Pair** – Create or use an existing key pair for SSH access.
   - **Launch the Instance**.

3. **Connect to EC2 Instance**:
```sh
# Connect to EC2 using SSH
ssh -i "path/to/keypair.pem" ec2-user@your-ec2-public-ip
```

---

### **5.2.2 Installing Docker on EC2**

```sh
# Update the package manager
sudo yum update -y

# Install Docker
sudo amazon-linux-extras install docker

# Start Docker service
sudo service docker start

# Add ec2-user to the docker group
sudo usermod -a -G docker ec2-user

# Log out and log back in to apply group changes
exit
```

- **amazon-linux-extras install docker** – Installs Docker on Amazon Linux 2.
- **service docker start** – Starts the Docker service.
- **usermod -a -G docker ec2-user** – Adds the current user to the Docker group.

---

### **5.2.3 Deploying Angular + Spring Boot using Docker Compose**

---

#### **5.2.3.1 Transfer Files to EC2 Instance**

```sh
# Transfer project files to EC2 using SCP
scp -i "path/to/keypair.pem" -r /path/to/project ec2-user@your-ec2-public-ip:/home/ec2-user
```

- **-r** – Recursively copies the entire project directory.
- **/path/to/project** – Path to your local project directory.
- **/home/ec2-user** – Target directory on the EC2 instance.

---

#### **5.2.3.2 Running Docker Compose**

```sh
# Navigate to the project directory
cd /home/ec2-user/project

# Start all containers using Docker Compose
sudo docker-compose up --build -d
```

- **docker-compose up --build -d**:
  - Builds and starts all containers in **detached mode**.
  - **Angular and Spring Boot** containers are orchestrated together.

---

### **5.2.4 Accessing Deployed Application**

- Open a browser and navigate to:
```
http://your-ec2-public-ip
```

- The **Angular frontend** and **Spring Boot backend** should be **fully functional**.

---

## **5.3 Deploying on Azure App Service with Docker**

---

### **5.3.1 Preparing Docker Images for Azure**

1. **Tag Docker Images** for Azure:
```sh
# Tag Angular Docker Image
docker tag angular-frontend your-azure-registry.azurecr.io/angular-frontend:latest

# Tag Spring Boot Docker Image
docker tag springboot-backend your-azure-registry.azurecr.io/springboot-backend:latest
```

2. **Push Images to Azure Container Registry**:
```sh
# Login to Azure Container Registry
az acr login --name your-azure-registry

# Push Angular Image
docker push your-azure-registry.azurecr.io/angular-frontend:latest

# Push Spring Boot Image
docker push your-azure-registry.azurecr.io/springboot-backend:latest
```

---

### **5.3.2 Creating Azure App Service**

1. **Navigate to Azure Portal** – https://portal.azure.com/
2. **Create Azure App Service**:
   - **Resource Group** – Create or select an existing group.
   - **App Service Plan** – Choose a region and select pricing tier.
   - **Container Settings**:
     - **Image Source** – Azure Container Registry.
     - **Container Registry** – Select your registry.
     - **Image and Tag** – Select `angular-frontend:latest` and `springboot-backend:latest`.
   - **Networking**:
     - Enable **Public Access**.
     - Configure **Custom Domains** and **SSL** (optional).

3. **Deploy and Access the Application**:
   - Deploy the images to Azure App Service.
   - Access the application using the **Azure App Service URL**.

---

## **5.4 CI/CD Integration using GitHub Actions**

---

### **5.4.1 Setting up GitHub Actions for CI/CD on AWS**

- **GitHub Actions** automates:
  - **Building and Testing** Angular and Spring Boot apps.
  - **Pushing Docker Images** to **Amazon ECR**.
  - **Deploying to AWS EC2** using **Docker Compose**.

---

### **5.4.2 Sample GitHub Actions Workflow**

**Example: .github/workflows/aws-deployment.yml**

```yaml
name: AWS Deployment

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v2

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v1

      - name: Build and Push Docker Images
        run: |
          docker build -t angular-frontend ./frontend
          docker build -t springboot-backend ./backend
          docker-compose up --build -d
```

- **on: push** – Triggers on pushes to the `main` branch.
- **docker-compose up --build -d** – Builds and deploys all containers.

---

## **Next Steps:**
- **Lesson 76: Latest Angular Features (Angular 19 and Beyond)**
  - **What's New in Angular 19?**
  - **Signals in Angular**
  - **Performance Improvements and Developer Enhancements**

