# 🚀 **Phase 2 - Lesson 16: Spring Boot + RabbitMQ & Kafka for Asynchronous Messaging**  

## **📌 Lesson Objective**  
By the end of this lesson, you will:  
✅ Understand **Message Queues & Event-Driven Architecture**  
✅ Learn **RabbitMQ & Apache Kafka basics**  
✅ Implement **RabbitMQ messaging in Spring Boot**  
✅ Implement **Kafka messaging in Spring Boot**  
✅ Learn when to use **RabbitMQ vs Kafka**  

---

## **1️⃣ What is Message Queuing & Why Use It?**  
📌 **Message Queuing** is a method where **messages are sent asynchronously** between services via a message broker (e.g., RabbitMQ, Kafka).  

### **🔥 Why Use Message Queues?**
✔ **Decouples Microservices**  
✔ **Improves Scalability**  
✔ **Ensures Reliable Communication**  
✔ **Handles High Traffic**  

✅ **Use Message Queues when:**  
- You need **asynchronous processing** (e.g., Order placed → Send email later).  
- You want to **retry messages** in case of failure.  
- You need to **process large volumes of messages** efficiently.  

---

# 🐰 **Part 1: Implementing RabbitMQ in Spring Boot**  

## **2️⃣ Setting Up RabbitMQ in Spring Boot**  
📌 **Step 1: Add RabbitMQ Dependency in `pom.xml`**  
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

📌 **Step 2: Configure RabbitMQ in `application.properties`**  
```properties
spring.rabbitmq.host=localhost
spring.rabbitmq.port=5672
spring.rabbitmq.username=guest
spring.rabbitmq.password=guest
```

📌 **Step 3: Install & Start RabbitMQ (Docker Command)**  
```bash
docker run -d --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:management
```
Go to **`http://localhost:15672/`** (RabbitMQ Management UI)  
**Login:** Username → `guest`, Password → `guest`  

✅ **Now RabbitMQ is running!** 🎉  

---

## **3️⃣ Creating a Message Queue in Spring Boot**  
📌 **Step 4: Create `RabbitMQConfig.java`**  
```java
package com.example.demo.config;

import org.springframework.amqp.core.Queue;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class RabbitMQConfig {
    
    @Bean
    public Queue myQueue() {
        return new Queue("myQueue", false);
    }
}
```

📌 **Step 5: Create a Message Producer (`RabbitMQProducer.java`)**  
```java
package com.example.demo.service;

import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.stereotype.Service;

@Service
public class RabbitMQProducer {

    private final RabbitTemplate rabbitTemplate;

    public RabbitMQProducer(RabbitTemplate rabbitTemplate) {
        this.rabbitTemplate = rabbitTemplate;
    }

    public void sendMessage(String message) {
        rabbitTemplate.convertAndSend("myQueue", message);
        System.out.println("Sent message: " + message);
    }
}
```

📌 **Step 6: Create a Message Consumer (`RabbitMQConsumer.java`)**  
```java
package com.example.demo.service;

import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Service;

@Service
public class RabbitMQConsumer {

    @RabbitListener(queues = "myQueue")
    public void receiveMessage(String message) {
        System.out.println("Received message: " + message);
    }
}
```

📌 **Step 7: Expose an API to Send Messages (`RabbitMQController.java`)**  
```java
package com.example.demo.controller;

import com.example.demo.service.RabbitMQProducer;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/rabbitmq")
public class RabbitMQController {

    private final RabbitMQProducer producer;

    public RabbitMQController(RabbitMQProducer producer) {
        this.producer = producer;
    }

    @PostMapping("/send")
    public String sendMessage(@RequestParam String message) {
        producer.sendMessage(message);
        return "Message sent!";
    }
}
```

📌 **Step 8: Run & Test the API**  
```bash
mvn spring-boot:run
```
```bash
curl -X POST "http://localhost:8080/rabbitmq/send?message=HelloRabbit"
```
🎉 **RabbitMQ messaging is now working!**  

---

# 🦜 **Part 2: Implementing Apache Kafka in Spring Boot**  

## **4️⃣ Setting Up Kafka in Spring Boot**  
📌 **Step 1: Add Kafka Dependency in `pom.xml`**  
```xml
<dependency>
    <groupId>org.springframework.kafka</groupId>
    <artifactId>spring-kafka</artifactId>
</dependency>
```

📌 **Step 2: Configure Kafka in `application.properties`**  
```properties
spring.kafka.bootstrap-servers=localhost:9092
spring.kafka.consumer.group-id=my-group
spring.kafka.consumer.auto-offset-reset=earliest
```

📌 **Step 3: Install & Start Kafka (Docker Command)**  
```bash
docker run -d --name kafka -p 9092:9092 -e KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181 -e KAFKA_LISTENERS=PLAINTEXT://:9092 wurstmeister/kafka
```

✅ **Now Kafka is running!** 🎉  

---

## **5️⃣ Creating a Kafka Topic & Messaging System**  
📌 **Step 4: Create `KafkaConfig.java`**  
```java
package com.example.demo.config;

import org.apache.kafka.clients.admin.NewTopic;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class KafkaConfig {
    
    @Bean
    public NewTopic myTopic() {
        return new NewTopic("myTopic", 1, (short) 1);
    }
}
```

📌 **Step 5: Create a Kafka Message Producer (`KafkaProducer.java`)**  
```java
package com.example.demo.service;

import org.springframework.kafka.core.KafkaTemplate;
import org.springframework.stereotype.Service;

@Service
public class KafkaProducer {

    private final KafkaTemplate<String, String> kafkaTemplate;

    public KafkaProducer(KafkaTemplate<String, String> kafkaTemplate) {
        this.kafkaTemplate = kafkaTemplate;
    }

    public void sendMessage(String message) {
        kafkaTemplate.send("myTopic", message);
        System.out.println("Sent message to Kafka: " + message);
    }
}
```

📌 **Step 6: Create a Kafka Message Consumer (`KafkaConsumer.java`)**  
```java
package com.example.demo.service;

import org.springframework.kafka.annotation.KafkaListener;
import org.springframework.stereotype.Service;

@Service
public class KafkaConsumer {

    @KafkaListener(topics = "myTopic", groupId = "my-group")
    public void receiveMessage(String message) {
        System.out.println("Received message from Kafka: " + message);
    }
}
```

📌 **Step 7: Expose API to Send Kafka Messages (`KafkaController.java`)**  
```java
package com.example.demo.controller;

import com.example.demo.service.KafkaProducer;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/kafka")
public class KafkaController {

    private final KafkaProducer producer;

    public KafkaController(KafkaProducer producer) {
        this.producer = producer;
    }

    @PostMapping("/send")
    public String sendMessage(@RequestParam String message) {
        producer.sendMessage(message);
        return "Message sent to Kafka!";
    }
}
```

📌 **Step 8: Run & Test Kafka Messaging**  
```bash
mvn spring-boot:run
```
```bash
curl -X POST "http://localhost:8080/kafka/send?message=HelloKafka"
```
🎉 **Kafka messaging is now working!**  

---

## 🎯 **Lesson 16 - Summary**  
✅ Integrated **RabbitMQ for message queuing**  
✅ Implemented **Kafka for event-driven messaging**  
✅ Created **message producers & consumers**  
✅ Compared **RabbitMQ vs Kafka**  

---
