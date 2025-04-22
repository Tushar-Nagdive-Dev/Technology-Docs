
## 📊 **Stage 3 – Lesson 16: Monitoring, Metrics, and Health Checks in Apache Camel**

> "If you can't monitor it, you can't manage it."

Apache Camel integrates beautifully with **Micrometer**, **Spring Boot Actuator**, **Prometheus**, and more — enabling full visibility of your routes.

---

## 🧩 Key Observability Features in Camel

| Feature | Purpose |
|--------|---------|
| 🔍 Camel Route Metrics | Measure message count, errors, duration |
| 🟢 Health Checks | Live/ready status of routes and components |
| 📡 Actuator Endpoints | Inspect routes, contexts, components |
| 📈 Integration with Prometheus/Grafana | Export metrics for dashboards |

---

## ✅ 1. Enable Actuator + Camel Metrics

### 🔧 Dependencies

```xml
<dependency>
  <groupId>org.apache.camel.springboot</groupId>
  <artifactId>camel-micrometer-starter</artifactId>
</dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

---

## ⚙️ 2. Configure Actuator in `application.properties`

```properties
management.endpoints.web.exposure.include=camelroutes,camelhealth,health,info,metrics
camel.springboot.routesIncludePattern=*
camel.health.enabled=true
camel.metrics.enabled=true
```

---

## 🔗 3. Actuator Endpoints

| Endpoint | Purpose |
|---------|---------|
| `/actuator/health` | Basic Spring Boot health |
| `/actuator/camelhealth` | Detailed Camel route health |
| `/actuator/camelroutes` | All registered routes and their state |
| `/actuator/metrics` | All Micrometer metrics (can be filtered) |

---

## 📈 4. Micrometer Metrics

Automatically tracks:
- Message count
- Exchanges failed
- Processing time
- Route status

Example:
```
camel.route.exchanges.total{name="fileToLog"} => 5
camel.route.exchanges.failed{name="fileToLog"} => 1
```

---

## 📊 5. Prometheus Integration

Add this dependency:

```xml
<dependency>
  <groupId>io.micrometer</groupId>
  <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```

Expose metrics via:
```properties
management.endpoint.prometheus.enabled=true
management.endpoints.web.exposure.include=prometheus
```

Then access:
```
/actuator/prometheus
```

✅ Ready to plug into **Grafana dashboards**!

---

## 🧠 Real-World Dashboard Use Case

Track:
- Route latency
- Failed Kafka messages
- Throughput per second
- Queue lag
- File I/O failures

Use `Prometheus` + `Grafana` for visualizing time series metrics like these.

---

## ⚠️ Common Mistakes

| Mistake | Fix |
|--------|-----|
| Not enabling `camelhealth` | Add `camel.health.enabled=true` |
| Missing metrics dependency | Add `camel-micrometer-starter` |
| Metrics not showing | Ensure actuator is configured correctly |

---

## 🧪 Challenge

Create a simple Camel route:
- From `timer:metricsTest`
- Logs a message every 5s
- Enable Micrometer and view metrics in `/actuator/metrics`


---
```java
import org.apache.camel.builder.RouteBuilder;
import org.springframework.stereotype.Component;

@Component
public class MetricsTimerRoute extends RouteBuilder {

    @Override
    public void configure() throws Exception {
        from("timer:metricsTest?period=5000")
            .routeId("metricsTestRoute")
            .log("Timer triggered at ${date:now:yyyy-MM-dd HH:mm:ss}")
            .to("micrometer:counter:metricsTestCounter");
    }
}
```
