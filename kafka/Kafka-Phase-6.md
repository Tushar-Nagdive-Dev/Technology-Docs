### **Phase 6: Performance Tuning and Monitoring** 🚀  
Now that you're proficient with Kafka Streams and KSQL, it's time to **optimize Kafka's performance** and **ensure its health** in production environments. In this phase, you'll learn how to fine-tune Kafka configurations, monitor critical metrics, and identify bottlenecks. By the end of this phase, you'll be able to achieve **high throughput, low latency**, and **reliable performance** in real-world Kafka deployments.

---

## **1. Performance Tuning Overview**  
Kafka performance tuning involves optimizing the following components:
- **Producer**: Throughput and latency of message publishing.
- **Consumer**: Speed of message consumption.
- **Broker**: Storage and replication efficiency.
- **Network**: Efficient data transfer.

---

## **2. Producer Tuning**  
### **1. Batch Size and Compression**  
- **batch.size**: Controls the maximum size of a batch (in bytes).
  - Larger batch sizes improve throughput but increase latency.
  - Recommended: `batch.size=16384` (16 KB) or higher for high throughput.

- **compression.type**:
  - Reduces message size, improving network utilization.
  - Available options: `gzip`, `snappy`, `lz4`, `zstd`.
  - Recommended: `compression.type=snappy` for balanced performance.

**Example**:
```java
props.put("batch.size", 32768);
props.put("compression.type", "snappy");
```

### **2. Linger Time**  
- **linger.ms**: Time to wait before sending a batch.
  - Increases batching efficiency but adds latency.
  - Recommended: `linger.ms=10` for low latency and high throughput.

**Example**:
```java
props.put("linger.ms", 10);
```

### **3. Acknowledgments (acks)**  
- **acks** controls the durability and consistency of messages.
  - `acks=0`: No acknowledgment (fast but risky).
  - `acks=1`: Leader acknowledgment (balanced).
  - `acks=all`: All replicas acknowledgment (strongest consistency).

**Example**:
```java
props.put("acks", "all");
```

### **4. Idempotence and Retries**  
- **enable.idempotence**:
  - Guarantees exactly-once delivery.
  - Recommended: `enable.idempotence=true`.

- **retries**:
  - Number of retries for failed sends.
  - Recommended: `retries=Integer.MAX_VALUE`.

**Example**:
```java
props.put("enable.idempotence", "true");
props.put("retries", Integer.MAX_VALUE);
```

---

## **3. Consumer Tuning**  
### **1. Fetch Size and Max Records**  
- **fetch.min.bytes**:
  - Minimum amount of data for the consumer to fetch.
  - Recommended: `fetch.min.bytes=1048576` (1 MB).

- **max.poll.records**:
  - Maximum number of records per poll.
  - Recommended: `max.poll.records=500` for balanced throughput.

**Example**:
```java
props.put("fetch.min.bytes", 1048576);
props.put("max.poll.records", 500);
```

### **2. Session Timeouts and Heartbeats**  
- **session.timeout.ms**:
  - Time to detect a failed consumer.
  - Recommended: `session.timeout.ms=30000` (30 seconds).

- **heartbeat.interval.ms**:
  - Frequency of heartbeats to the coordinator.
  - Recommended: `heartbeat.interval.ms=10000` (10 seconds).

**Example**:
```java
props.put("session.timeout.ms", 30000);
props.put("heartbeat.interval.ms", 10000);
```

### **3. Offset Management**  
- **enable.auto.commit**:
  - Auto-commit offsets (not recommended for critical data).
  - Preferred: **Manual Offset Management**.

- **auto.offset.reset**:
  - Behavior when no offset is found.
  - Options: `earliest`, `latest`, `none`.
  - Recommended: `auto.offset.reset=latest`.

**Example**:
```java
props.put("enable.auto.commit", "false");
props.put("auto.offset.reset", "latest");
```

---

## **4. Broker Configuration Tuning**  
### **1. Log Segment and Retention Settings**  
- **log.segment.bytes**:
  - Maximum size of a log segment.
  - Recommended: `log.segment.bytes=1GB` for better cleanup.

- **log.retention.hours**:
  - Duration to retain messages.
  - Recommended: `log.retention.hours=168` (7 days).

**Example**:
```properties
log.segment.bytes=1073741824
log.retention.hours=168
```

### **2. Replication and In-Sync Replicas**  
- **num.replica.fetchers**:
  - Number of threads for replication.
  - Recommended: `num.replica.fetchers=4`.

- **min.insync.replicas**:
  - Minimum replicas required for a write.
  - Recommended: `min.insync.replicas=2` for fault tolerance.

**Example**:
```properties
num.replica.fetchers=4
min.insync.replicas=2
```

### **3. Page Cache and I/O Optimization**  
- **log.flush.interval.ms**:
  - Interval for flushing messages to disk.
  - Recommended: `log.flush.interval.ms=1000` (1 second).

- **log.flush.scheduler.interval.ms**:
  - Frequency of log flushing.
  - Recommended: `log.flush.scheduler.interval.ms=1000`.

**Example**:
```properties
log.flush.interval.ms=1000
log.flush.scheduler.interval.ms=1000
```

---

## **5. Monitoring Kafka**  
### **1. Key Metrics to Monitor**  
1. **Producer Metrics**:
   - `record-send-rate`: Rate of messages sent per second.
   - `request-latency-avg`: Average latency of requests.

2. **Consumer Metrics**:
   - `records-consumed-rate`: Rate of messages consumed per second.
   - `fetch-latency-avg`: Average fetch latency.

3. **Broker Metrics**:
   - `under-replicated-partitions`: Partitions without sufficient replicas.
   - `active-controller-count`: Number of active controllers.

4. **Zookeeper Metrics**:
   - `zookeeper.request.latency`: Request latency.
   - `zookeeper.node.count`: Number of nodes in Zookeeper.

### **2. Monitoring Tools**  
- **Prometheus + Grafana**:
  - Popular choice for monitoring Kafka metrics.
  - Exposes metrics using JMX Exporter.

- **Confluent Control Center**:
  - Enterprise-grade monitoring tool.
  - Provides detailed metrics, alerts, and dashboards.

---

## **6. Setting Up Monitoring with Prometheus and Grafana**  
### **1. JMX Exporter**  
- Export Kafka metrics via **JMX Exporter**.
- Download JMX Exporter from [JMX Exporter GitHub](https://github.com/prometheus/jmx_exporter).

**Example**: Configure `jmx_exporter.yml`:
```yaml
lowercaseOutputName: true
rules:
  - pattern: "kafka.server<type=(.+), name=(.+)><>Value"
    name: "kafka_server_$1_$2"
    type: GAUGE
```

### **2. Prometheus Configuration**  
- Add the JMX Exporter as a target in `prometheus.yml`:
```yaml
scrape_configs:
  - job_name: 'kafka'
    static_configs:
      - targets: ['localhost:7071']
```

### **3. Grafana Dashboards**  
- Import Kafka dashboards from Grafana's dashboard repository:
  - [Kafka Overview Dashboard](https://grafana.com/grafana/dashboards)

---

## **7. Exercises and Resources**  
### **Hands-On Lab**  
1. Tune producer and consumer configurations for high throughput.
2. Enable JMX metrics and monitor Kafka using **Prometheus and Grafana**.
3. Create custom Grafana dashboards to visualize key Kafka metrics.

### **Watch**:
- [Kafka Performance Tuning](https://www.youtube.com/watch?v=jtTujSK0n78)
- [Kafka Monitoring with Prometheus and Grafana](https://www.youtube.com/watch?v=XtT5GHAYkeM)

### **Read**:
- [Kafka Performance Tuning Guide](https://docs.confluent.io/platform/current/kafka/performance.html)
- [Prometheus JMX Exporter](https://github.com/prometheus/jmx_exporter)

---

## **Next Step: Phase 7 - Integrating Kafka with Other Systems**  
- Learn about **Kafka Connect** for data integration.
- Integrate Kafka with **Spring Boot** and **Microservices**.
- Build event-driven architectures with Kafka.

---
