Excellent, Tushar! Now that you’ve built Camel routes in Spring Boot, it’s time to level up with **real-time messaging** using **Kafka**—a staple in modern event-driven systems.

---

## 📡 **Stage 2 – Lesson 10: Apache Camel Kafka Integration (with Spring Boot)**

Apache Camel + Kafka = ⚡ Seamless streaming + enterprise routing

---

## ✅ Why Camel + Kafka?

- Easily route messages between Kafka and other systems (DB, REST, file, etc.)
- Apply transformation, validation, enrichment between producer ↔ consumer
- Use Camel's built-in error handling, retries, aggregation, etc.

---

## 📦 1. Add Kafka Dependencies

```xml
<dependency>
  <groupId>org.apache.camel.springboot</groupId>
  <artifactId>camel-kafka-starter</artifactId>
  <version>3.20.2</version>
</dependency>
```

---

## ⚙️ 2. Kafka Configuration (`application.properties`)

```properties
camel.component.kafka.brokers=localhost:9092
```

✅ You should have Kafka running locally. You can use Docker:

```bash
docker run -d --name kafka -p 9092:9092 \
  -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9092 \
  -e KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181 \
  confluentinc/cp-kafka
```

---

## 📝 3. Kafka Producer Route (Send Message to Kafka)

```java
@Component
public class KafkaProducerRoute extends RouteBuilder {
    @Override
    public void configure() {
        from("timer:kafkaTimer?period=5000")
            .setBody(simple("Hello Kafka at ${date:now}"))
            .to("kafka:my-topic")
            .log("Sent to Kafka: ${body}");
    }
}
```

- Every 5 seconds, sends a message to topic `my-topic`.

---

## 📥 4. Kafka Consumer Route (Read from Kafka)

```java
@Component
public class KafkaConsumerRoute extends RouteBuilder {
    @Override
    public void configure() {
        from("kafka:my-topic?groupId=my-group")
            .log("Consumed from Kafka: ${body}");
    }
}
```

---

## 🧠 Real-World Flow

Let’s say you want to:
- Receive payment events in Kafka
- Validate them in Java bean
- Store to database
- Send confirmation email

Camel Route Example:

```java
from("kafka:payment-topic")
    .unmarshal().json()
    .bean(PaymentValidator.class, "validate")
    .to("jpa:com.tushar.model.Payment")
    .to("smtp://support@bank.com")
    .log("Payment processed: ${body}");
```

---

## 🚫 Common Mistakes

| Issue | Fix |
|------|-----|
| Connection refused | Make sure Kafka is up and accessible on `localhost:9092` |
| Missing topic | Create it manually or configure auto-creation |
| Multiple consumers with same group ID | Only one will receive message (Kafka design) |

---

## 🧪 Challenge (Optional)

✅ Create a Camel route that:
1. Accepts a REST request (`/api/publish`)
2. Accepts query param `msg`
3. Sends that `msg` to Kafka topic `custom-messages`
4. Logs confirmation

---

```java
import org.apache.camel.builder.RouteBuilder;
import org.apache.camel.model.rest.RestBindingMode;
import org.springframework.stereotype.Component;

@Component
public class KafkaPublishRoute extends RouteBuilder {

    @Override
    public void configure() throws Exception {
        // Configure REST DSL
        restConfiguration()
            .component("servlet")
            .bindingMode(RestBindingMode.json);

        // Define REST endpoint
        rest("/api")
            .get("/publish")
            .produces("text/plain")
            .param()
                .name("msg")
                .type(org.apache.camel.model.rest.RestParamType.query)
                .description("Message to publish to Kafka")
            .endParam()
            .route()
            .to("kafka:custom-messages?brokers=localhost:9092")
            .log("Message published to Kafka: ${header.msg}");
    }
}
```
