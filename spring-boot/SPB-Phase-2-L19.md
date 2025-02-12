# 🚀 **Phase 2 - Lesson 19: Deploying Spring Boot on AWS (EC2, S3, RDS, Lambda)**  

## **📌 Lesson Objective**  
By the end of this lesson, you will:  
✅ Deploy a **Spring Boot app on AWS EC2**  
✅ Store application data in **AWS RDS (PostgreSQL/MySQL)**  
✅ Upload and manage files using **AWS S3**  
✅ Run **serverless Spring Boot functions using AWS Lambda**  
✅ Automate deployment using **AWS Elastic Beanstalk**  

---

## **1️⃣ Why Deploy Spring Boot Apps on AWS?**  
📌 **Amazon Web Services (AWS)** provides a **scalable, secure, and cost-effective** cloud infrastructure to run Spring Boot applications.  

### **🔥 Why Use AWS?**
✔ **High Availability** – AWS runs in multiple regions & data centers.  
✔ **Auto Scaling** – Handles high traffic automatically.  
✔ **Fully Managed Services** – AWS RDS, S3, Lambda reduce operational workload.  
✔ **Security & Monitoring** – IAM, CloudWatch, CloudTrail ensure security & logging.  

✅ **Use AWS when:**  
- You need **scalable deployments** (EC2, Elastic Beanstalk).  
- You want **managed databases** (RDS).  
- You need **serverless computing** (Lambda).  

---

# 🌐 **Part 1: Deploying Spring Boot on AWS EC2**  

## **2️⃣ Setting Up AWS EC2 for Spring Boot Deployment**  
📌 **Step 1: Launch an EC2 Instance**  
1️⃣ Go to **AWS Console → EC2 → Launch Instance**  
2️⃣ Select **Amazon Linux 2 / Ubuntu**  
3️⃣ Choose **Instance Type:** `t2.micro` (Free Tier)  
4️⃣ Add **Security Group:**  
   - Allow **port 22 (SSH)** for remote access.  
   - Allow **port 8080 (Spring Boot)** for external access.  
5️⃣ Create and Download **Key Pair** (For SSH login).  

📌 **Step 2: Connect to EC2 via SSH**  
```bash
ssh -i your-key.pem ec2-user@your-ec2-public-ip
```

📌 **Step 3: Install Java & Maven on EC2**  
```bash
sudo yum update -y
sudo yum install java-17-amazon-corretto -y
sudo yum install maven -y
```

📌 **Step 4: Transfer the Spring Boot JAR to EC2**  
On your local machine:  
```bash
scp -i your-key.pem target/spring-boot-app.jar ec2-user@your-ec2-public-ip:/home/ec2-user/
```

📌 **Step 5: Run the Spring Boot App on EC2**  
```bash
nohup java -jar spring-boot-app.jar > output.log 2>&1 &
```
✔ Now your **Spring Boot app is running on EC2!** 🎉  

📌 **Step 6: Access the App from Browser**  
```bash
http://your-ec2-public-ip:8080/api/hello
```

✅ **Spring Boot app is successfully deployed on AWS EC2!** 🚀  

---

# 💾 **Part 2: Storing Data in AWS RDS (Managed Database Service)**  

## **3️⃣ Setting Up AWS RDS for Spring Boot**  
📌 **Step 1: Create an RDS Instance**  
1️⃣ Go to **AWS Console → RDS → Create Database**  
2️⃣ Select **Database Engine:** PostgreSQL/MySQL  
3️⃣ Choose **Instance Type:** Free Tier (`db.t2.micro`)  
4️⃣ Configure **Master Username & Password**  
5️⃣ Enable **Public Access** to connect from EC2  

📌 **Step 2: Configure `application.properties` for RDS**  
```properties
spring.datasource.url=jdbc:postgresql://your-rds-endpoint:5432/your-database
spring.datasource.username=your-username
spring.datasource.password=your-password
spring.datasource.driver-class-name=org.postgresql.Driver
spring.jpa.hibernate.ddl-auto=update
```

📌 **Step 3: Restart the Spring Boot App**  
```bash
nohup java -jar spring-boot-app.jar > output.log 2>&1 &
```

📌 **Step 4: Verify Database Connection**  
```bash
curl -X GET "http://your-ec2-public-ip:8080/api/users"
```
✔ Your app is now **using AWS RDS for data storage!** 🎉  

---

# 🗂 **Part 3: Uploading Files to AWS S3**  

## **4️⃣ Storing Files in AWS S3 from Spring Boot**  
📌 **Step 1: Create an S3 Bucket**  
1️⃣ Go to **AWS Console → S3 → Create Bucket**  
2️⃣ Enable **Public Access** if needed.  

📌 **Step 2: Add AWS SDK Dependency in `pom.xml`**  
```xml
<dependency>
    <groupId>software.amazon.awssdk</groupId>
    <artifactId>s3</artifactId>
    <version>2.17.98</version>
</dependency>
```

📌 **Step 3: Configure `S3Config.java`**  
```java
package com.example.demo.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import software.amazon.awssdk.auth.credentials.ProfileCredentialsProvider;
import software.amazon.awssdk.services.s3.S3Client;

@Configuration
public class S3Config {
    @Bean
    public S3Client s3Client() {
        return S3Client.builder()
                .credentialsProvider(ProfileCredentialsProvider.create())
                .build();
    }
}
```

📌 **Step 4: Implement File Upload Service**  
```java
package com.example.demo.service;

import org.springframework.stereotype.Service;
import software.amazon.awssdk.services.s3.S3Client;
import software.amazon.awssdk.services.s3.model.PutObjectRequest;
import java.nio.file.Path;

@Service
public class S3Service {
    private final S3Client s3Client;

    public S3Service(S3Client s3Client) {
        this.s3Client = s3Client;
    }

    public void uploadFile(String bucketName, String key, Path filePath) {
        s3Client.putObject(PutObjectRequest.builder().bucket(bucketName).key(key).build(), filePath);
    }
}
```

📌 **Step 5: Test File Upload to S3**  
```bash
curl -X POST "http://your-ec2-public-ip:8080/api/upload?file=/path/to/file.jpg"
```
✔ File is now **stored in AWS S3!** 🎉  

---

# ⚡ **Part 4: Running Spring Boot as a Serverless Function with AWS Lambda**  

## **5️⃣ Deploying Spring Boot as a Serverless Function**  
📌 **Step 1: Add AWS Lambda Dependency in `pom.xml`**  
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-function-adapter-aws</artifactId>
    <version>3.2.4</version>
</dependency>
```

📌 **Step 2: Implement a Simple Lambda Handler**  
```java
package com.example.demo.lambda;

import org.springframework.cloud.function.adapter.aws.SpringBootRequestHandler;

public class LambdaHandler extends SpringBootRequestHandler<String, String> {}
```

📌 **Step 3: Package & Deploy Lambda Function**  
```bash
mvn clean package
aws lambda create-function --function-name spring-boot-lambda \
  --zip-file fileb://target/demo-0.0.1-SNAPSHOT.jar \
  --handler com.example.demo.lambda.LambdaHandler \
  --runtime java11 --role your-iam-role-arn
```
✔ **Spring Boot is now running as a serverless function in AWS Lambda!** 🎉  

---

## 🎯 **Lesson 19 - Summary**  
✅ Deployed **Spring Boot app on AWS EC2**  
✅ Configured **AWS RDS for database storage**  
✅ Uploaded **files to AWS S3 from Spring Boot**  
✅ Ran **Spring Boot as an AWS Lambda Function**  

---
