## 🛑 **Stage 1 – Lesson 8: Exception Handling in Apache Camel**

> “An integration system is only as strong as its failure strategy.” – Every Architect Ever 😄

Apache Camel provides **powerful and flexible exception handling mechanisms**, both global and route-level.

---

## 🧩 Key Concepts

| Feature | Purpose |
|--------|---------|
| `onException` | Global error handling |
| `doTry`, `doCatch`, `doFinally` | Route-level try-catch |
| `deadLetterChannel` | Send failed messages elsewhere |
| `handled(true)` | Swallow exception, mark it handled |
| `continued(true)` | Let route continue after error |

---

## ✅ 1. **Global Error Handling with `onException`**

```java
onException(IOException.class)
    .log("Handled IOException: ${exception.message}")
    .handled(true);

from("direct:start")
    .to("file:non-existent-dir")  // Will throw IOException
    .to("log:afterFile");
```

🔍 If the directory doesn’t exist, this prevents the app from crashing and logs the error.

---

## 🧪 Want to Retry?

```java
onException(IOException.class)
    .maximumRedeliveries(3)
    .redeliveryDelay(1000)
    .log("Retrying due to: ${exception.message}");
```

---

## 🔁 2. **Route-Level Try-Catch**

```java
from("direct:tryRoute")
    .doTry()
        .to("bean:riskyBean")  // May throw
    .doCatch(Exception.class)
        .log("Caught error: ${exception.message}")
    .doFinally()
        .log("Done with try-catch");
```

✅ Useful for **localized exception handling**.

---

## 💌 3. **Dead Letter Channel (DLQ)**

Sends failed messages to a dead letter queue.

```java
errorHandler(deadLetterChannel("file:data/dlq")
    .maximumRedeliveries(2)
    .redeliveryDelay(1000));
```

📦 If processing fails after retries, message is saved to `data/dlq`.

---

## 🔄 4. **Mark as Handled or Continue**

| Use | Purpose |
|-----|---------|
| `handled(true)` | Stop the exception from propagating |
| `continued(true)` | Continue processing after exception |

```java
onException(NullPointerException.class)
    .handled(true)
    .log("Handled NPE, route continues.");
```

---

## 🔍 Real-World Use Case

```java
onException(JsonParseException.class)
    .handled(true)
    .to("log:bad-json")
    .to("jms:queue:invalidMessages");

from("kafka:invoices")
    .unmarshal().json()
    .to("bean:invoiceValidator")
    .to("jms:queue:processedInvoices");
```

- Bad JSON is rerouted to `invalidMessages` queue.

---

## ⚠️ Common Mistakes

| Mistake | Problem |
|--------|---------|
| Missing `.handled(true)` | Exceptions bubble up and crash the app |
| No dead letter route | Messages are lost |
| Mixing `onException` inside route | Not allowed! It must be global (before `from`) |

---

## 🧪 Challenge (Optional)

Write a route:
1. Reads from a file
2. Simulates a failure (throw exception manually)
3. Catches it using `onException`
4. Logs error and continues gracefully

---

```java
import org.apache.camel.builder.RouteBuilder;
import org.springframework.stereotype.Component;

@Component
public class ErrorHandlingRoute extends RouteBuilder {
    
    @Override
    public void configure() throws Exception {
        // Define exception handling
        onException(Exception.class)
            .handled(true)
            .log("Error occurred: ${exception.message}")
            .to("log:error?level=ERROR");
        
        // Route that reads from file and simulates failure
        from("file:input?noop=true")
            .process(exchange -> {
                // Simulate a failure
                throw new RuntimeException("Simulated processing failure");
            });
    }
}
```
