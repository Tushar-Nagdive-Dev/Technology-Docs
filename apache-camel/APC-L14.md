
## 🔁 **Stage 3 – Lesson 15: Transactions, Idempotency & Redelivery in Camel**

> In real-world systems, **messages fail**, **duplicate**, or even get **replayed** — Camel gives you powerful tools to handle this with grace.

---

### 🧱 What You’ll Learn:

1. 🔄 Transaction Management (JMS/DB)
2. ✅ Idempotency (Avoiding duplicates)
3. 🔁 Redelivery Policies (Retries with delay, backoff)
4. 🧯 Error Handling + Recovery
5. 🛠 Real-world use cases

---

## ✅ 1. Transaction Management

Used when working with **JMS, JDBC, JPA** or any transactional system.

### Example with JMS:

```java
from("jms:queue:orders")
    .transacted()
    .to("bean:orderProcessor")
    .to("jpa:com.tushar.model.Order");
```

🔒 This ensures:
- If `orderProcessor` or JPA save fails → **entire transaction is rolled back**
- JMS message will **not be acknowledged** until success

---

### 🧠 Requirements:
- Use `camel-spring-boot-starter` and `camel-jms-starter`
- Setup Spring-managed `JmsTransactionManager` or `JpaTransactionManager`

---

## ✅ 2. Idempotency (No Duplicates)

> Idempotency ensures that the **same message isn’t processed twice**.

### ✅ With Idempotent Consumer:

```java
from("jms:queue:payments")
    .idempotentConsumer(header("paymentId"))
    .messageIdRepository(new MemoryIdempotentRepository())
    .to("bean:paymentProcessor");
```

💡 `paymentId` in header is the **unique key**  
Camel will **remember** it was processed once, and **skip** on reprocessing.

---

### 🧠 Real Repositories You Can Use:

| Repository | Use Case |
|------------|----------|
| `MemoryIdempotentRepository` | Simple, in-memory (dev only) |
| `FileIdempotentRepository` | Persistent via file |
| `JdbcMessageIdRepository` | For DB-backed idempotency |
| `RedisIdempotentRepository` | Fast + scalable |

---

## ✅ 3. Redelivery & Backoff

Set retries when an error occurs.

```java
errorHandler(defaultErrorHandler()
    .maximumRedeliveries(3)
    .redeliveryDelay(2000)
    .backOffMultiplier(2)
    .retryAttemptedLogLevel(LoggingLevel.WARN));
```

🔥 Retry Sequence:
- 1st failure → wait 2s
- 2nd retry → 4s
- 3rd retry → 8s
- Then fail

---

## 🛡️ 4. Dead Letter Handling (DLQ)

Unrecoverable errors? Send them to a dead-letter route:

```java
errorHandler(deadLetterChannel("jms:dlq")
    .maximumRedeliveries(2)
    .redeliveryDelay(1000));
```

---

## 🧪 5. Real-World Scenario

> Payment Route:
- Ensure payment is processed only once
- Retry up to 3 times if failure
- If all fail, move to DLQ

```java
from("kafka:payment-topic")
    .idempotentConsumer(header("paymentId"), new JdbcMessageIdRepository(dataSource, "payment_ids"))
    .transacted()
    .to("bean:paymentValidator")
    .to("jpa:com.tushar.model.Payment")
    .log("Payment saved");
```

---

## ⚠️ Common Mistakes

| Mistake | Fix |
|--------|-----|
| Not declaring `.transacted()` | No rollback will occur |
| In-memory idempotent repo in prod | Use persistent storage |
| Forgetting error handler | App will crash or retry forever |

---

## 🧪 Mini-Challenge

Build a Camel route that:
1. Reads from a `file:orders` directory
2. Uses `.idempotentConsumer()` based on file name
3. Logs the order
4. Moves file to `processed/` folder
---
```java
import org.apache.camel.builder.RouteBuilder;
import org.apache.camel.processor.idempotent.FileIdempotentRepository;
import org.springframework.stereotype.Component;

@Component
public class OrderFileProcessingRoute extends RouteBuilder {

    @Override
    public void configure() throws Exception {
        from("file:orders?move=processed/${file:name}")
            .idempotentConsumer(header("CamelFileName"), 
                FileIdempotentRepository.fileIdempotentRepository(new java.io.File("idempotent-repo")))
            .log("Processing order file: ${header.CamelFileName}")
            .end();
    }
}
```
