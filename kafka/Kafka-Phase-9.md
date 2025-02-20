### **Phase 9: Real-World Projects and Case Studies** 🚀  
You’ve reached the exciting phase where you’ll **apply everything you’ve learned** about Kafka in real-world scenarios. This phase focuses on building **end-to-end projects** using Kafka, integrating with microservices, and solving complex use cases. By the end of this phase, you’ll be able to design and implement **event-driven architectures** and **real-time analytics** solutions.

---

## **1. Real-World Project 1: Event-Driven Microservices with Kafka and Spring Boot**  
### **Objective**:  
Build an **Event-Driven Order Management System** using Kafka and Spring Boot Microservices.

### **Architecture Overview**:  
- **Order Service**:
  - Receives new orders and publishes events to `order-events` topic.
- **Inventory Service**:
  - Consumes events from `order-events` to update stock.
- **Payment Service**:
  - Consumes events to process payments.
- **Notification Service**:
  - Sends email/SMS notifications.

### **Tech Stack**:
- **Spring Boot** for microservices.
- **Spring Cloud Stream** for Kafka integration.
- **Kafka** as the event backbone.
- **MySQL** for persistent storage.
- **Docker** for containerization.

---

### **1. Order Service: Producing Order Events**  
- Receives new orders via REST API.
- Publishes `ORDER_CREATED` event to `order-events` topic.

**OrderEvent.java**:
```java
public class OrderEvent {
    private String orderId;
    private String productId;
    private int quantity;
    private String status;
    private LocalDateTime timestamp;
    // Getters and Setters
}
```

**OrderProducer.java**:
```java
@Service
public class OrderProducer {
    private final KafkaTemplate<String, OrderEvent> kafkaTemplate;

    public OrderProducer(KafkaTemplate<String, OrderEvent> kafkaTemplate) {
        this.kafkaTemplate = kafkaTemplate;
    }

    public void publishOrderEvent(OrderEvent event) {
        kafkaTemplate.send("order-events", event.getOrderId(), event);
    }
}
```

**OrderController.java**:
```java
@RestController
@RequestMapping("/api/orders")
public class OrderController {
    private final OrderProducer orderProducer;

    public OrderController(OrderProducer orderProducer) {
        this.orderProducer = orderProducer;
    }

    @PostMapping
    public ResponseEntity<String> createOrder(@RequestBody OrderEvent orderEvent) {
        orderEvent.setStatus("ORDER_CREATED");
        orderEvent.setTimestamp(LocalDateTime.now());
        orderProducer.publishOrderEvent(orderEvent);
        return ResponseEntity.status(HttpStatus.CREATED).body("Order Created");
    }
}
```

---

### **2. Inventory Service: Consuming Order Events**  
- Consumes events from `order-events` topic.
- Updates inventory stock accordingly.

**InventoryConsumer.java**:
```java
@Service
public class InventoryConsumer {
    @KafkaListener(topics = "order-events", groupId = "inventory-group")
    public void consumeOrderEvent(OrderEvent event) {
        System.out.println("Received Order Event: " + event);
        // Update inventory based on event data
    }
}
```

**application.properties**:
```properties
spring.kafka.consumer.group-id=inventory-group
spring.kafka.consumer.auto-offset-reset=earliest
spring.kafka.bootstrap-servers=localhost:9092
```

---

### **3. Payment Service: Consuming and Producing Events**  
- Consumes `ORDER_CREATED` event.
- Processes payment and publishes `ORDER_PAID` event.

**PaymentService.java**:
```java
@Service
public class PaymentService {
    private final KafkaTemplate<String, OrderEvent> kafkaTemplate;

    public PaymentService(KafkaTemplate<String, OrderEvent> kafkaTemplate) {
        this.kafkaTemplate = kafkaTemplate;
    }

    @KafkaListener(topics = "order-events", groupId = "payment-group")
    public void processPayment(OrderEvent event) {
        if ("ORDER_CREATED".equals(event.getStatus())) {
            // Process payment logic
            event.setStatus("ORDER_PAID");
            kafkaTemplate.send("order-events", event.getOrderId(), event);
        }
    }
}
```

---

### **4. Notification Service: Sending Notifications**  
- Consumes `ORDER_PAID` event.
- Sends email/SMS notifications to the customer.

**NotificationConsumer.java**:
```java
@Service
public class NotificationConsumer {
    @KafkaListener(topics = "order-events", groupId = "notification-group")
    public void sendNotification(OrderEvent event) {
        if ("ORDER_PAID".equals(event.getStatus())) {
            System.out.println("Sending Notification for Order ID: " + event.getOrderId());
            // Trigger email/SMS notification logic
        }
    }
}
```

---

### **5. Running the Microservices**  
- Start Zookeeper and Kafka Broker:
```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
bin/kafka-server-start.sh config/server.properties
```

- Create Kafka Topic:
```bash
bin/kafka-topics.sh --create --topic order-events --partitions 3 --replication-factor 1 --bootstrap-server localhost:9092
```

- Start Microservices using Docker:
```bash
docker-compose up -d
```

- Test by creating a new order:
```bash
curl -X POST -H "Content-Type: application/json" \
    -d '{"orderId": "123", "productId": "ABC", "quantity": 2}' \
    http://localhost:8080/api/orders
```

---

## **2. Real-World Project 2: Real-Time Analytics Dashboard with Kafka Streams and KSQL**  
### **Objective**:  
Build a **Real-Time Analytics Dashboard** to visualize sales data in real-time.

### **Tech Stack**:
- **Kafka Streams** for real-time processing.
- **KSQL** for querying and aggregating data.
- **Elasticsearch** for indexing sales data.
- **Kibana** for data visualization.

### **Architecture Overview**:
- **Sales Service**:
  - Produces `SALES_EVENT` to `sales-events` topic.
- **Kafka Streams**:
  - Aggregates sales data by region and product category.
  - Outputs to `sales-analytics` topic.
- **KSQL**:
  - Queries `sales-analytics` for real-time metrics.
- **Elasticsearch and Kibana**:
  - Indexes aggregated data for visualization.

### **Implementation**:
- Create `sales-events` topic:
```bash
bin/kafka-topics.sh --create --topic sales-events --partitions 3 --replication-factor 1 --bootstrap-server localhost:9092
```

- **KSQL Stream**:
```sql
CREATE STREAM sales_stream (product_id VARCHAR, region VARCHAR, amount DOUBLE) WITH (KAFKA_TOPIC='sales-events', VALUE_FORMAT='JSON');
```

- **Real-Time Aggregation**:
```sql
CREATE TABLE sales_analytics AS SELECT region, SUM(amount) AS total_sales FROM sales_stream GROUP BY region;
```

- **Elasticsearch Sink Connector**:
```json
{
  "name": "elasticsearch-sink-connector",
  "config": {
    "connector.class": "io.confluent.connect.elasticsearch.ElasticsearchSinkConnector",
    "topics": "sales-analytics",
    "connection.url": "http://localhost:9200",
    "type.name": "_doc",
    "key.ignore": "true",
    "schema.ignore": "true"
  }
}
```

---

## **3. Exercises and Resources**  
### **Hands-On Lab**  
1. Build and deploy the **Event-Driven Order Management System**.
2. Create a **Real-Time Analytics Dashboard** using **Kafka Streams, KSQL, and Kibana**.
3. Experiment with **Scaling Microservices** and **Optimizing Kafka Streams**.

### **Watch**:
- [Event-Driven Microservices with Kafka](https://www.youtube.com/watch?v=ZT5cxw8moaA)
- [Real-Time Analytics with Kafka Streams](https://www.youtube.com/watch?v=3DBwwQjfA8w)

### **Read**:
- [Spring Cloud Stream Documentation](https://spring.io/projects/spring-cloud-stream)
- [KSQL Documentation](https://docs.confluent.io/current/ksql/index.html)

---

## **Next Step: Phase 10 - Certification and Expert Insights**  
- **Prepare for Confluent Certified Developer for Apache Kafka**.
- **Practice Exams and Expert Insights**.
- **Best Practices and Common Pitfalls**.

---
