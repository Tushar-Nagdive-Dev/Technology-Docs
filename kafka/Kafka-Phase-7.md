### **Phase 7: Integrating Kafka with Other Systems** 🚀  
Now that you’ve mastered performance tuning and monitoring, it's time to make Kafka work in complex enterprise ecosystems. In this phase, you will learn how to **integrate Kafka with other systems** using **Kafka Connect** and how to use Kafka with **Spring Boot and Microservices**. By the end of this phase, you'll be able to build **event-driven architectures** and **seamlessly integrate data pipelines**.

---

## **1. Introduction to Kafka Connect**  
### **1. What is Kafka Connect?**  
- **Kafka Connect** is a **scalable and reliable data integration framework**.
- It is used to **stream data between Kafka and other systems** such as databases, cloud storage, and data warehouses.

### **2. Why Use Kafka Connect?**  
- **Simplifies Data Integration**: No custom code needed for data pipelines.
- **Scalable and Fault Tolerant**: Scales horizontally with distributed workers.
- **Configuration-Driven**: Connectors are configured via JSON files or REST API.

### **3. Kafka Connect Architecture**  
- **Connectors**: Pre-built components for integrating with external systems.
  - **Source Connectors**: Ingest data into Kafka from external systems (e.g., databases, files).
  - **Sink Connectors**: Export data from Kafka to external systems (e.g., databases, cloud storage).

- **Workers**:
  - **Standalone Mode**: Single worker process for development and testing.
  - **Distributed Mode**: Multiple workers for scalability and fault tolerance.

- **Tasks**:
  - Tasks are **parallel units** of work.
  - Each connector is divided into multiple tasks for parallelism.

---

## **2. Setting Up Kafka Connect**  
### **1. Installing Kafka Connect**  
- Kafka Connect is included in the Kafka distribution (`bin/connect-*` scripts).
- Required dependencies:
  - **Kafka** (already installed)
  - **Confluent Hub** for connectors (optional)

### **2. Downloading Connectors**  
- Download connectors from [Confluent Hub](https://www.confluent.io/hub).
- Example: JDBC Connector for integrating with databases.
```bash
confluent-hub install confluentinc/kafka-connect-jdbc:latest
```

### **3. Configuring Kafka Connect**  
**Standalone Mode Configuration** (`config/connect-standalone.properties`):
```properties
bootstrap.servers=localhost:9092
key.converter=org.apache.kafka.connect.json.JsonConverter
value.converter=org.apache.kafka.connect.json.JsonConverter
offset.storage.file.filename=/tmp/connect.offsets
```

**Distributed Mode Configuration** (`config/connect-distributed.properties`):
```properties
bootstrap.servers=localhost:9092
group.id=connect-cluster
config.storage.topic=connect-configs
offset.storage.topic=connect-offsets
status.storage.topic=connect-status
```

### **4. Starting Kafka Connect**  
- **Standalone Mode**:
```bash
bin/connect-standalone.sh config/connect-standalone.properties config/source-connector.properties
```

- **Distributed Mode**:
```bash
bin/connect-distributed.sh config/connect-distributed.properties
```

---

## **3. Using Source and Sink Connectors**  
### **1. Source Connector Example: JDBC Source Connector**  
- Streams data from a relational database to Kafka.

**JDBC Source Connector Configuration**:
```json
{
  "name": "jdbc-source-connector",
  "config": {
    "connector.class": "io.confluent.connect.jdbc.JdbcSourceConnector",
    "tasks.max": "1",
    "connection.url": "jdbc:mysql://localhost:3306/mydb",
    "connection.user": "user",
    "connection.password": "password",
    "topic.prefix": "jdbc-",
    "mode": "incrementing",
    "incrementing.column.name": "id",
    "table.whitelist": "orders"
  }
}
```

- Start the connector using the REST API:
```bash
curl -X POST -H "Content-Type: application/json" --data @jdbc-source.json http://localhost:8083/connectors
```

### **2. Sink Connector Example: Elasticsearch Sink Connector**  
- Streams data from Kafka to Elasticsearch.

**Elasticsearch Sink Connector Configuration**:
```json
{
  "name": "elasticsearch-sink-connector",
  "config": {
    "connector.class": "io.confluent.connect.elasticsearch.ElasticsearchSinkConnector",
    "tasks.max": "1",
    "topics": "jdbc-orders",
    "connection.url": "http://localhost:9200",
    "type.name": "_doc",
    "key.ignore": "true",
    "schema.ignore": "true"
  }
}
```

- Start the connector using the REST API:
```bash
curl -X POST -H "Content-Type: application/json" --data @elasticsearch-sink.json http://localhost:8083/connectors
```

---

## **4. Schema Management with Confluent Schema Registry**  
### **1. What is Schema Registry?**  
- Manages and enforces **schemas for Kafka topics**.
- Ensures **backward and forward compatibility**.
- Supports **Avro**, **JSON Schema**, and **Protobuf** formats.

### **2. Why Use Schema Registry?**  
- Ensures **data consistency** across producers and consumers.
- **Versioning and Compatibility**:
  - **Backward Compatibility**: New schema can read old data.
  - **Forward Compatibility**: Old schema can read new data.

### **3. Using Schema Registry**  
- Install Schema Registry from [Confluent Platform](https://docs.confluent.io/platform/current/schema-registry/index.html).
- Start Schema Registry:
```bash
bin/schema-registry-start config/schema-registry.properties
```

### **4. Schema Registry REST API**  
- Register a new schema:
```bash
curl -X POST -H "Content-Type: application/vnd.schemaregistry.v1+json" \
--data '{"schema": "{\"type\":\"record\",\"name\":\"Order\",\"fields\":[{\"name\":\"id\",\"type\":\"int\"},{\"name\":\"amount\",\"type\":\"double\"}]}"}' \
http://localhost:8081/subjects/orders-value/versions
```

- Get schema versions:
```bash
curl -X GET http://localhost:8081/subjects/orders-value/versions
```

---

## **5. Kafka with Microservices Architecture**  
### **1. Event-Driven Architecture**  
- **Producers** publish events to Kafka topics.
- **Consumers** subscribe to topics to react to events.
- Promotes **loose coupling** and **asynchronous communication**.

### **2. Using Kafka with Spring Boot**  
- Add **Spring Kafka** dependency:
```xml
<dependency>
    <groupId>org.springframework.kafka</groupId>
    <artifactId>spring-kafka</artifactId>
</dependency>
```

### **3. Producer Example**  
```java
@Service
public class KafkaProducerService {
    private final KafkaTemplate<String, String> kafkaTemplate;

    public KafkaProducerService(KafkaTemplate<String, String> kafkaTemplate) {
        this.kafkaTemplate = kafkaTemplate;
    }

    public void sendMessage(String message) {
        kafkaTemplate.send("my-topic", message);
    }
}
```

### **4. Consumer Example**  
```java
@Service
public class KafkaConsumerService {

    @KafkaListener(topics = "my-topic", groupId = "my-group")
    public void listen(String message) {
        System.out.println("Received Message: " + message);
    }
}
```

---

## **6. Exercises and Resources**  
### **Hands-On Lab**  
1. Install and configure **Kafka Connect**.
2. Use the **JDBC Source Connector** to stream data from MySQL to Kafka.
3. Use the **Elasticsearch Sink Connector** to index data into Elasticsearch.
4. Integrate Kafka with **Spring Boot** to build an event-driven microservice.

### **Watch**:
- [Kafka Connect Overview](https://www.youtube.com/watch?v=_zVt8gn6_vE)
- [Spring Boot + Kafka](https://www.youtube.com/watch?v=ZIy2AtfF4NQ)

### **Read**:
- [Kafka Connect Documentation](https://kafka.apache.org/documentation/#connect)
- [Spring Kafka Documentation](https://spring.io/projects/spring-kafka)

### **Quiz**:
- What are the differences between **Source** and **Sink** connectors?
- How does **Schema Registry** ensure compatibility?
- What is the role of **Kafka Connect Workers**?

---

## **Next Step: Phase 8 - Kafka in Production**  
- Learn best practices for **deploying and maintaining Kafka in production**.
- Deploy **Kafka clusters on cloud platforms** (AWS, GCP, Azure).
- Implement **Disaster Recovery and Backup** strategies.

---
