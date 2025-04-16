## 🧠 **Stage 1 – Lesson 6: Enterprise Integration Patterns (EIPs)**

> Apache Camel’s real strength lies in how it implements **Enterprise Integration Patterns**, a set of design patterns from the book *Enterprise Integration Patterns* by Gregor Hohpe and Bobby Woolf.

Camel provides **out-of-the-box DSL** to implement these patterns with minimal code.

---

## 🧩 Key EIP Patterns We'll Cover (Beginner to Advanced)

| Pattern | Purpose |
|--------|---------|
| 🔀 **Content-Based Router** | Route messages based on content |
| ✂️ **Splitter** | Split one message into many |
| 🧵 **Aggregator** | Combine multiple messages into one |
| 📬 **Recipient List** | Send to multiple dynamic endpoints |
| 🪞 **WireTap** | Send a copy of the message elsewhere |
| ⏱ **Throttler** | Limit message rate |
| 🔁 **Loop** | Repeat actions N times |

We’ll focus on **Content-Based Router**, **Splitter**, and **Aggregator** first.

---

## 🔀 1. **Content-Based Router**

Route messages to different paths based on condition (like `if-else`).

### ✅ Example:

```java
from("direct:start")
    .choice()
        .when(body().contains("order"))
            .to("log:orderLogger")
        .when(body().contains("payment"))
            .to("log:paymentLogger")
        .otherwise()
            .to("log:defaultLogger");
```

🧠 **Use Case:**  
"Send messages with 'order' to Order Service, 'payment' to Payment Service."

---

## ✂️ 2. **Splitter**

Split a big message (like CSV, JSON array, XML) into individual parts.

### ✅ Example:

```java
from("direct:split")
    .split(body().tokenize(","))
    .to("log:splitLogger");
```

📦 Input: `"apple,banana,mango"`  
📨 Output:
```
apple
banana
mango
```

🧠 **Use Case:**  
"Break order list into individual orders for processing."

---

## 🧵 3. **Aggregator**

Collect multiple messages and combine them based on a condition (e.g., count, timeout, correlation id).

### ✅ Example (Basic Aggregation by Count):

```java
from("direct:aggregate")
    .aggregate(constant(true), new GroupedBodyAggregationStrategy())
    .completionSize(3)
    .to("log:aggregatedLogger");
```

📦 Sends 3 messages → Aggregates them into a list and sends together.

🧠 **Use Case:**  
"Wait for 3 invoice messages, then send a single summary."

---

## 💡 Pro Tip: Aggregation Strategy

Use this built-in strategy:
```java
import org.apache.camel.processor.aggregate.GroupedBodyAggregationStrategy;
```

You can also build a **custom AggregationStrategy** by implementing the interface.

---

## ⚠️ Common Mistakes

| Mistake | Why it’s bad |
|--------|--------------|
| Using `.split()` without `.end()` in nested routes | May lead to route leakage or misbehavior |
| Not setting correlation key in complex aggregation | Results in messages being grouped incorrectly |
| Forgetting `.otherwise()` in `.choice()` | Can lead to unprocessed messages |

---

## 🧪 Mini-Challenge

Try this:

1. Create a route that splits a CSV list of names.
2. Logs each name individually.
3. Aggregates every 2 names and logs the combined list.

Let me know if you want the solution next or if you want to try it first.

---

```java
import org.apache.camel.builder.RouteBuilder;
import org.apache.camel.processor.aggregate.GroupedExchangeAggregationStrategy;

public class NameSplitterRoute extends RouteBuilder {
    @Override
    public void configure() throws Exception {
        from("direct:start")
            .split().tokenize(",")
                .log("Individual name: ${body}")
                .aggregate(constant(true), new GroupedExchangeAggregationStrategy())
                    .completionSize(2)
                    .log("Aggregated names: ${body}");
    }
}
```
