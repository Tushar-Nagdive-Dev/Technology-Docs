### **Phase 4: Advanced Kafka Operations** 🚀  
Now that you have a solid understanding of Kafka's architecture, it's time to explore advanced configurations and operations. In this phase, you'll learn how to optimize Kafka topics, ensure high availability, and secure your Kafka clusters. By the end of this phase, you'll be able to configure Kafka for **performance, fault tolerance, and security** in real-world applications.

---

## **1. Advanced Topic Configurations**  
Kafka topics are highly configurable to meet various requirements. Let's explore the most important settings.

### **1. Topic Retention Policies**  
Kafka retains messages for a configurable period. You can set this at the **topic level** or **broker level**.

- **log.retention.ms**: Retain messages for a specific time (in milliseconds).
- **log.retention.bytes**: Retain messages until the log reaches a specified size.
- **log.segment.bytes**: Breaks logs into segments of specified size for easier deletion.

**Example**: Retain messages for 7 days:
```bash
bin/kafka-topics.sh --alter --topic my-topic --config retention.ms=604800000 --bootstrap-server localhost:9092
```

### **2. Log Compaction**  
- **Log Compaction** retains the latest value for a key, allowing upserts.
- Ideal for **change data capture** and **event sourcing**.
- Enabled with `cleanup.policy=compact`.

**Example**:
```bash
bin/kafka-topics.sh --alter --topic my-topic --config cleanup.policy=compact --bootstrap-server localhost:9092
```

### **3. Topic Cleanup and Purging**  
- You can manually delete or purge topics using:
```bash
bin/kafka-topics.sh --delete --topic my-topic --bootstrap-server localhost:9092
```
- Enable delete with:
```properties
delete.topic.enable=true
```

---

## **2. High Availability and Fault Tolerance**  
Kafka achieves high availability and fault tolerance through **replication** and **leader election**.

### **1. Replication in Depth**  
- Each partition is **replicated** across multiple brokers.
- **Replication Factor**:
  - `replication.factor=3` means each partition is stored on 3 brokers.
- Guarantees **data durability** and **high availability**.

**Example**:
```bash
bin/kafka-topics.sh --create --topic my-topic --partitions 3 --replication-factor 3 --bootstrap-server localhost:9092
```

### **2. Leader and Follower Partitions**  
- **Leader**:
  - Handles all reads and writes.
- **Follower**:
  - Replicates data from the leader.
  - Promoted to leader in case of failure.

### **3. ISR (In-Sync Replicas) and Failover Mechanism**  
- **ISR (In-Sync Replicas)** are followers fully synced with the leader.
- Kafka elects a new leader from the ISR if the current leader fails.
- Configurable using:
  ```properties
  min.insync.replicas=2
  ```

### **4. Ensuring Zero Data Loss**  
- Set `acks=all` in the producer:
```java
Properties props = new Properties();
props.put("acks", "all");
```
- Ensure at least 2 replicas are in-sync:
```properties
min.insync.replicas=2
```

### **5. Rack Awareness**  
- Ensures replicas are distributed across different data centers or racks.
- Configured using:
```properties
broker.rack=us-east-1a
```

**Example**:
```bash
bin/kafka-topics.sh --create --topic my-topic --partitions 3 --replication-factor 3 --config min.insync.replicas=2 --bootstrap-server localhost:9092
```

---

## **3. Kafka Security**  
Kafka provides robust security features for **authentication, authorization, and encryption**.

### **1. SSL/TLS Encryption**  
- Secures data in transit between clients and brokers.
- Configured using:
```properties
ssl.keystore.location=/path/to/keystore.jks
ssl.keystore.password=secret
ssl.key.password=secret
ssl.truststore.location=/path/to/truststore.jks
ssl.truststore.password=secret
```

### **2. Authentication with SASL**  
- **SASL (Simple Authentication and Security Layer)** provides authentication mechanisms:
  - **SASL/PLAIN**: Username and password authentication.
  - **SASL/SCRAM**: Secure challenge-response mechanism.
  - **SASL/OAUTHBEARER**: OAuth2 token-based authentication.

**Example**: SASL/SCRAM Configuration:
```properties
sasl.mechanism=SCRAM-SHA-256
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.scram.ScramLoginModule required username="user" password="password";
```

### **3. Authorization and ACLs**  
- **ACLs (Access Control Lists)** control access to topics, consumer groups, and brokers.
- Operations:
  - **READ**: Read messages from a topic.
  - **WRITE**: Write messages to a topic.
  - **DESCRIBE**: Describe topic or consumer group.
  - **ALTER**: Modify topic configuration.

**Example**: Allow a user to read from a topic:
```bash
bin/kafka-acls.sh --authorizer-properties zookeeper.connect=localhost:2181 --add --allow-principal User:alice --operation READ --topic my-topic
```

### **4. Securing Zookeeper**  
- Use SASL authentication to secure Zookeeper:
```properties
authProvider.1=org.apache.zookeeper.server.auth.SASLAuthenticationProvider
requireClientAuthScheme=sasl
```

---

## **4. Monitoring and Managing Kafka**  
Kafka provides various tools to **monitor and manage** brokers, topics, and consumers.

### **1. Kafka Metrics and Monitoring Tools**  
- **JMX (Java Management Extensions)**: Exposes Kafka metrics.
- **Prometheus + Grafana**: Popular combination for monitoring Kafka.

**Example**: Expose JMX metrics:
```properties
JMX_PORT=9999
```

### **2. Kafka Manager and Kafka UI Tools**  
- **Kafka Manager**: Monitors brokers, topics, and partitions.
- **Kafka UI Tools**: Provide web-based UIs to manage Kafka clusters.

---

## **5. Exercises and Resources**  
### **Hands-On Lab**  
1. Create a topic with:
   - `replication.factor=3`
   - `min.insync.replicas=2`
   - `cleanup.policy=compact`
2. Enable SSL/TLS for encrypted communication.
3. Set up ACLs for role-based authorization.
4. Monitor Kafka metrics using **Prometheus and Grafana**.

### **Watch**:  
- [Kafka Security Deep Dive](https://www.youtube.com/watch?v=3YvY2qmD6yA)
- [Kafka Monitoring with Prometheus](https://www.youtube.com/watch?v=XtT5GHAYkeM)

### **Read**:  
- [Kafka Security Documentation](https://kafka.apache.org/documentation/#security)
- [Kafka Monitoring Guide](https://www.confluent.io/blog/monitoring-kafka-performance-metrics)

### **Quiz**:
- How does Kafka achieve high availability and fault tolerance?
- What are the differences between **log retention** and **log compaction**?
- How do **acks** and **min.insync.replicas** work together?

---

## **Next Step: Phase 5 - Kafka Streams and KSQL**  
In the next phase, we will:
- Learn about **Kafka Streams** for real-time stream processing.
- Master **KSQL (Kafka SQL)** to perform real-time data transformations.
- Build real-time analytics applications using Kafka Streams and KSQL.

---
