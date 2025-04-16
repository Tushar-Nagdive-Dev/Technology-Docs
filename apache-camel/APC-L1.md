---

### 1. **Route**
A **Route** defines the flow of messages from a **source (from)** to one or more **destinations (to)** with optional **processing** in between.

> 🔁 It’s like defining a pipeline: _"Take message from A → do something → send to B."_

```java
from("file:data/inbox?noop=true")
  .to("file:data/outbox");
```

📌 **Here:**  
- Input: `data/inbox`
- Output: `data/outbox`

---

### 2. **Endpoint**
An **Endpoint** is the starting or ending point of a route. It’s represented by a URI (like `file:`, `http:`, `timer:`).

📌 **Examples:**
- `file:data/inbox`
- `timer:trigger?period=1000`
- `kafka:orders`
- `direct:start` (used to call routes internally)

---

### 3. **Exchange**
An **Exchange** represents the entire message lifecycle during routing. It contains:
- **IN message** (before processing)
- **OUT message** (after processing)
- **Headers, Body, Properties, Exception**

```java
from("direct:start")
  .process(exchange -> {
      String body = exchange.getIn().getBody(String.class);
      System.out.println("Message: " + body);
  });
```

---

### 4. **Message**
A **Message** is the data carried by the route—usually contains:
- Body: The actual data (JSON, XML, String, etc.)
- Headers: Metadata (like HTTP headers)
- Attachments (optional)

```java
exchange.getIn().getBody(String.class); // Get body
exchange.getIn().getHeader("myHeader"); // Get header
```

---

### 5. **Processor**
A **Processor** allows custom logic to manipulate an exchange.

🔧 It implements:

```java
import org.apache.camel.Processor;
import org.apache.camel.Exchange;

public class MyProcessor implements Processor {
    @Override
    public void process(Exchange exchange) {
        String body = exchange.getIn().getBody(String.class);
        exchange.getMessage().setBody("Processed: " + body);
    }
}
```

📌 Use it in a route like:

```java
from("direct:start")
  .process(new MyProcessor())
  .to("log:result");
```

---

## 🧠 Real-World Example

Imagine a payment processing route:

```java
from("kafka:paymentTopic")
  .process(new ValidatePaymentProcessor())       // Check if valid payment
  .to("bean:saveToDB")                           // Save to database
  .to("smtp://support@yourbank.com")             // Send confirmation email
  .to("log:payment");
```

---

## 🚫 Common Mistakes to Avoid
| Mistake | Why it's wrong |
|--------|----------------|
| Using `.to()` before `.process()` when modification is needed | Processing won’t affect the outgoing message if done after `.to()` |
| Forgetting to start the `CamelContext` | The route won't run |
| Using `getOut()` in newer Camel versions | Deprecated! Use `getMessage()` instead |

---

## 🧪 Exercise
Create a route that:
1. Starts with a `direct:start` endpoint.
2. Logs the original message.
3. Appends " - Processed" to the message body.
4. Logs the updated message.
