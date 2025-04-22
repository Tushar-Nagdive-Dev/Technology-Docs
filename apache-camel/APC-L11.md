
## 🧭 **Stage 2 – Lesson 12: Camel Route Design Best Practices & Patterns**

This lesson is all about writing **clean**, **scalable**, and **maintainable** Camel routes — exactly what enterprise-grade systems demand.

---

## 🧱 1. **Route Modularization**

✅ **Why?** Easier to test, extend, and understand.

📦 Split monolithic routes into:
- **Core route** (input → output)
- **Processing beans** (reusable logic)
- **Common components** (error handlers, formatters)

### ❌ Bad:
```java
from("kafka:events")
    .unmarshal().json()
    .bean(Validator.class, "validate")
    .bean(Transformer.class, "transform")
    .to("jms:queue:output")
    .to("log:done");
```

### ✅ Better:
```java
from("kafka:events")
    .to("direct:validate")
    .to("direct:transform")
    .to("jms:queue:output");

from("direct:validate")
    .bean(Validator.class);

from("direct:transform")
    .bean(Transformer.class);
```

---

## 🔄 2. **Reusability with `direct:` + Beans**

Use `direct:` to call sub-routes and inject **business logic** into `@Component` Beans (good for testing + DI).

---

## 🚨 3. **Centralized Error Handling**

Implement a **global error strategy**:

```java
errorHandler(deadLetterChannel("jms:deadQueue")
    .maximumRedeliveries(3)
    .redeliveryDelay(2000)
    .retryAttemptedLogLevel(LoggingLevel.WARN));
```

Also add per-route safety with:

```java
.onException(JsonParseException.class)
    .handled(true)
    .to("log:bad-json")
    .to("jms:invalid");
```

---

## 🔍 4. **Use Meaningful Route IDs & Logs**

```java
from("direct:start")
    .routeId("OrderProcessingRoute")
    .log("Received order: ${body}")
    .to("bean:orderService")
    .log("Order processed");
```

💡 This helps with:
- Traceability
- Route lifecycle control
- Monitoring

---

## 🧾 5. **Naming Conventions**

| Type | Prefix |
|------|--------|
| Internal route | `direct:` |
| Temporary testing | `mock:` |
| HTTP endpoint | `rest:` or `http:` |
| Kafka topic | `kafka:` |
| Queue name | `jms:` |

---

## 🧪 6. **Enable Observability**

✅ Enable **tracing, metrics, and health checks** in Spring Boot:

```properties
management.endpoints.web.exposure.include=camelroutes,camelhealth,health,info
camel.springboot.tracing=true
```

Use:
- `actuator/camelroutes`
- `actuator/camelhealth`

---

## ✅ 7. **Design Pattern Tips**

| Pattern | When to Use |
|--------|-------------|
| **Splitter** | One → Many (e.g. batch orders) |
| **Aggregator** | Many → One (e.g. invoice merge) |
| **Recipient List** | Dynamic routing |
| **WireTap** | Async side effects |
| **Enricher** | Enrich message with external call |

---

## ⚠️ Common Anti-Patterns

| Mistake | Fix |
|--------|-----|
| Overusing `.bean()` | Split into routes with `direct:` instead |
| Huge single route | Break into subroutes |
| Complex if-else in processors | Use `.choice()` with EIP |
| Inline business logic | Move to injectable `@Component` beans |
