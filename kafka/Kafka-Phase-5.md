### **Phase 5: Kafka Streams and KSQL** 🚀  
Now that you're comfortable with advanced Kafka operations, it's time to unlock Kafka's true power with **real-time stream processing**. In this phase, you'll learn how to process and transform data in motion using **Kafka Streams** and **KSQL (Kafka SQL)**. By the end of this phase, you'll be able to build complex real-time analytics applications.

---

## **1. Introduction to Kafka Streams**  
### **1. What is Kafka Streams?**
- **Kafka Streams** is a **real-time stream processing library** built on top of Kafka.
- It enables you to **transform**, **filter**, **aggregate**, and **enrich** data in real-time.
- It runs on **JVM** and integrates seamlessly with Kafka topics.

### **2. Why Use Kafka Streams?**
- **Event-Driven Architecture**: Ideal for event sourcing and CQRS patterns.
- **Scalable and Fault-Tolerant**: Scales automatically with Kafka's partitioning model.
- **Exactly-Once Processing**: Ensures data consistency.
- **Stateful Processing**: Supports windowing, joins, and aggregations.

---

## **2. Core Concepts of Kafka Streams**  
### **1. Streams and Tables**:
- **Stream**: An **unbounded sequence of events**. Each event is immutable.
- **Table**: A **current state** of data, built by aggregating streams. Similar to a database table.

### **2. Stateless vs. Stateful Processing**:  
- **Stateless Processing**:
  - No state is maintained between records.
  - Examples: Filtering, mapping, flat-mapping.

- **Stateful Processing**:
  - Maintains state between records.
  - Examples: Aggregation, joins, windowing.

### **3. Windows in Stream Processing**:  
- Windows group records within a specified time range:
  - **Tumbling Windows**: Fixed-size, non-overlapping windows.
  - **Hopping Windows**: Fixed-size, overlapping windows.
  - **Session Windows**: Dynamic windows based on user activity.

### **4. Kafka Streams API Components**:
- **KStream**: Represents a stream of records.
- **KTable**: Represents a changelog stream, maintaining the latest state.
- **GlobalKTable**: Distributed table available to all application instances.
- **Topology**: Defines the data flow graph of processors and state stores.

---

## **3. Building a Simple Kafka Streams Application**  
We'll build a simple application that:
- **Reads messages** from a Kafka topic.
- **Transforms** the messages.
- **Writes the transformed messages** to another topic.

### **1. Setting up the Project**  
- Use **Maven** or **Gradle** to set up a Spring Boot application with Kafka Streams.

**Maven Dependency**:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.kafka</groupId>
    <artifactId>spring-kafka</artifactId>
</dependency>
<dependency>
    <groupId>org.apache.kafka</groupId>
    <artifactId>kafka-streams</artifactId>
</dependency>
```

### **2. Configuration**  
In `application.properties`:
```properties
spring.kafka.bootstrap-servers=localhost:9092
spring.kafka.streams.application-id=wordcount-app
spring.kafka.streams.auto-startup=true
spring.kafka.streams.properties.commit.interval.ms=1000
spring.kafka.streams.properties.cache.max.bytes.buffering=10485760
```

### **3. Kafka Streams Processor**  
```java
@Configuration
public class KafkaStreamsConfig {

    @Bean
    public KStream<String, String> wordCountStream(StreamsBuilder builder) {
        KStream<String, String> stream = builder.stream("input-topic");

        KTable<String, Long> wordCounts = stream
            .flatMapValues(value -> Arrays.asList(value.toLowerCase().split("\\W+")))
            .groupBy((key, word) -> word)
            .count(Materialized.as("word-counts"));

        wordCounts.toStream().to("output-topic", Produced.with(Serdes.String(), Serdes.Long()));

        return stream;
    }
}
```

### **4. Explanation**:
- **StreamsBuilder**: Used to construct the topology.
- **KStream**: Represents the input stream from `input-topic`.
- **flatMapValues**: Splits each line into words.
- **groupBy**: Groups words for counting.
- **count**: Counts the occurrences of each word.
- **to**: Writes the result to `output-topic`.

### **5. Running the Application**:
- Start **Zookeeper** and **Kafka Broker**:
```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
bin/kafka-server-start.sh config/server.properties
```

- Create the required topics:
```bash
bin/kafka-topics.sh --create --topic input-topic --partitions 3 --replication-factor 1 --bootstrap-server localhost:9092
bin/kafka-topics.sh --create --topic output-topic --partitions 3 --replication-factor 1 --bootstrap-server localhost:9092
```

- Produce messages to the input topic:
```bash
bin/kafka-console-producer.sh --topic input-topic --bootstrap-server localhost:9092
```

- Consume messages from the output topic:
```bash
bin/kafka-console-consumer.sh --topic output-topic --from-beginning --bootstrap-server localhost:9092
```

---

## **4. Introduction to KSQL (Kafka SQL)**  
### **1. What is KSQL?**
- **KSQL** is a **SQL-like query language** for performing real-time data processing on Kafka topics.
- It allows you to **create streams and tables**, perform **transformations**, and **aggregate data**.

### **2. Why Use KSQL?**
- **Declarative Syntax**: Uses SQL syntax for stream processing.
- **Real-Time Analytics**: Ideal for monitoring and analytics dashboards.
- **No Coding Required**: Suitable for non-programmers.

### **3. Core Concepts of KSQL**:
- **Stream**: An unbounded sequence of events.
- **Table**: A changelog stream that maintains the latest state.

### **4. Basic KSQL Operations**  
1. **Creating a Stream**:
```sql
CREATE STREAM orders_stream (order_id VARCHAR, amount DOUBLE) WITH (KAFKA_TOPIC='orders', VALUE_FORMAT='JSON');
```

2. **Filtering Events**:
```sql
SELECT * FROM orders_stream WHERE amount > 100;
```

3. **Aggregation and Grouping**:
```sql
CREATE TABLE total_sales AS SELECT order_id, SUM(amount) AS total FROM orders_stream GROUP BY order_id;
```

4. **Joining Streams and Tables**:
```sql
CREATE STREAM enriched_orders AS SELECT o.order_id, o.amount, c.customer_name 
FROM orders_stream o 
LEFT JOIN customers_table c 
ON o.customer_id = c.customer_id;
```

---

## **5. Exercises and Resources**  
### **Hands-On Lab**  
1. Build a Kafka Streams application to:
   - Filter messages containing a keyword.
   - Count the occurrence of each keyword.
   - Output the result to another topic.
2. Create a **KSQL** stream to analyze real-time order data:
   - Filter high-value orders.
   - Calculate total sales by category.

### **Watch**:
- [Kafka Streams Overview](https://www.youtube.com/watch?v=3DBwwQjfA8w)
- [KSQL Tutorial](https://www.youtube.com/watch?v=kFZZe4IhC-8)

### **Read**:
- [Kafka Streams Documentation](https://kafka.apache.org/documentation/streams)
- [KSQL Documentation](https://docs.confluent.io/ksqldb/current/overview.html)

### **Quiz**:
- What is the difference between **KStream** and **KTable**?
- When to use **Stateful** vs. **Stateless** processing?
- How do **Tumbling Windows** and **Hopping Windows** differ?

---

## **Next Step: Phase 6 - Performance Tuning and Monitoring**  
In the next phase, we will:
- Learn how to **tune Kafka for high performance**.
- Explore **monitoring tools** like Prometheus and Grafana.
- Optimize **producer, consumer, and broker configurations**.

---
