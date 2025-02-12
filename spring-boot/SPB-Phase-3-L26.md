# 🚀 **Phase 3 - Lesson 26: Implementing Distributed Tracing & Logging with OpenTelemetry in Spring Boot**  

## **📌 Lesson Objective**  
By the end of this lesson, you will:  
✅ Understand **Distributed Tracing & Observability**  
✅ Integrate **OpenTelemetry for Spring Boot Monitoring**  
✅ Capture **Logs, Traces & Metrics for Microservices**  
✅ Use **Jaeger & Prometheus for Distributed Tracing**  
✅ Monitor APIs with **Grafana & Kibana**  

---

# 🔍 **Part 1: Understanding Distributed Tracing & Why It’s Important**  

📌 **What is Distributed Tracing?**  
Distributed Tracing **tracks requests across multiple microservices**, helping you:  
✔ Debug **slow API calls**  
✔ Identify **bottlenecks in microservices**  
✔ Monitor **latency & service dependencies**  

### **🔥 Why Use OpenTelemetry?**
✔ **Open-Source Standard** – Works with any tech stack.  
✔ **Integrates with Prometheus, Grafana, Jaeger**.  
✔ **Tracks requests across microservices in real-time**.  

✅ **Use OpenTelemetry when:**  
- You have **microservices that communicate via APIs**.  
- You need **end-to-end visibility of request execution**.  

---

# ⚙ **Part 2: Setting Up OpenTelemetry in Spring Boot**  

## **1️⃣ Adding OpenTelemetry Dependencies**  
📌 **Step 1: Add OpenTelemetry Java Agent & Dependencies in `pom.xml`**  
```xml
<dependency>
    <groupId>io.opentelemetry</groupId>
    <artifactId>opentelemetry-api</artifactId>
    <version>1.24.0</version>
</dependency>

<dependency>
    <groupId>io.opentelemetry.instrumentation</groupId>
    <artifactId>opentelemetry-spring-boot-starter</artifactId>
    <version>1.24.0</version>
</dependency>
```

📌 **Step 2: Download OpenTelemetry Java Agent**  
```bash
wget https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/download/v1.24.0/opentelemetry-javaagent.jar
```

📌 **Step 3: Start Spring Boot App with OpenTelemetry Agent**  
```bash
java -javaagent:opentelemetry-javaagent.jar \
  -Dotel.service.name=spring-boot-app \
  -jar target/spring-boot-app.jar
```
✔ Now **OpenTelemetry collects traces automatically!** 🚀  

---

# 📊 **Part 3: Capturing Traces with OpenTelemetry & Jaeger**  

## **2️⃣ Setting Up Jaeger for Distributed Tracing**  
📌 **Step 1: Run Jaeger in Docker**  
```bash
docker run -d --name jaeger \
  -e COLLECTOR_ZIPKIN_HTTP_PORT=9411 \
  -p 5775:5775/udp -p 6831:6831/udp -p 6832:6832/udp -p 5778:5778 \
  -p 16686:16686 -p 14250:14250 -p 14268:14268 -p 14269:14269 -p 9411:9411 \
  jaegertracing/all-in-one:latest
```
✔ Jaeger is now running on `http://localhost:16686`  

📌 **Step 2: Configure OpenTelemetry to Send Traces to Jaeger**  
```properties
otel.traces.exporter=jaeger
otel.exporter.jaeger.endpoint=http://localhost:14250
otel.service.name=spring-boot-app
```

📌 **Step 3: Start Your Application**  
```bash
java -javaagent:opentelemetry-javaagent.jar \
  -Dotel.traces.exporter=jaeger \
  -Dotel.exporter.jaeger.endpoint=http://localhost:14250 \
  -jar target/spring-boot-app.jar
```

📌 **Step 4: Test API & View Traces in Jaeger**  
```bash
curl -X GET "http://localhost:8080/api/users"
```
✔ Open **Jaeger UI (`http://localhost:16686`)** → Select **spring-boot-app** to view traces! 🎯  

---

# 📈 **Part 4: Monitoring Metrics with OpenTelemetry & Prometheus**  

## **3️⃣ Setting Up Prometheus for Metrics Collection**  
📌 **Step 1: Run Prometheus in Docker**  
```bash
docker run -d --name prometheus -p 9090:9090 prom/prometheus
```

📌 **Step 2: Configure `prometheus.yml` for OpenTelemetry Metrics**  
```yaml
global:
  scrape_interval: 5s

scrape_configs:
  - job_name: 'spring-boot-app'
    static_configs:
      - targets: ['host.docker.internal:9464']
```

📌 **Step 3: Start Spring Boot App with Prometheus Exporter**  
```properties
otel.metrics.exporter=prometheus
otel.exporter.prometheus.port=9464
otel.service.name=spring-boot-app
```

📌 **Step 4: View Metrics in Prometheus UI**  
Go to **`http://localhost:9090`** and query:  
```bash
otel_http_server_duration
```
✔ **Prometheus now collects metrics from Spring Boot!** 🚀  

---

# 📊 **Part 5: Visualizing Metrics in Grafana**  

## **4️⃣ Setting Up Grafana for Real-Time Monitoring**  
📌 **Step 1: Run Grafana in Docker**  
```bash
docker run -d --name grafana -p 3000:3000 grafana/grafana
```

📌 **Step 2: Add Prometheus as a Data Source in Grafana**  
1️⃣ Go to **`http://localhost:3000`** → Login (`admin/admin`)  
2️⃣ Navigate to **Data Sources** → Add **Prometheus** (`http://localhost:9090`)  
3️⃣ Create **Dashboards for API Latency & CPU Usage**  

📌 **Step 3: Test API Calls & Monitor Metrics in Grafana**  
```bash
curl -X GET "http://localhost:8080/api/users"
```
✔ **Live monitoring of Spring Boot API performance is now in Grafana!** 🚀  

---

# 🔎 **Part 6: Centralized Logging with OpenTelemetry & Elastic Stack (ELK - Elasticsearch, Logstash, Kibana)**  

## **5️⃣ Setting Up ELK Stack for Log Aggregation**  
📌 **Step 1: Run Elasticsearch, Logstash & Kibana in Docker**  
```bash
docker-compose up -d
```

📌 **Step 2: Configure OpenTelemetry for Logging**  
```properties
otel.logs.exporter=otlp
otel.exporter.otlp.endpoint=http://localhost:9200
```

📌 **Step 3: View Logs in Kibana (`http://localhost:5601`)**  
✔ Now **all Spring Boot logs are centralized in Kibana!** 🚀  

---

## 🎯 **Lesson 26 - Summary**  
✅ Integrated **OpenTelemetry for Distributed Tracing**  
✅ Captured **Traces using Jaeger**  
✅ Monitored **Metrics with Prometheus & Grafana**  
✅ Centralized **Logs in Kibana (ELK Stack)**  

---
