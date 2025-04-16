## ✅ **Exercise Solution: Append “ - Processed” to message and log before/after**

```java
import org.apache.camel.CamelContext;
import org.apache.camel.builder.RouteBuilder;
import org.apache.camel.impl.DefaultCamelContext;

public class ProcessMessageRoute {
    public static void main(String[] args) throws Exception {
        CamelContext context = new DefaultCamelContext();

        context.addRoutes(new RouteBuilder() {
            @Override
            public void configure() {
                from("direct:start")
                    .log("Original message: ${body}")
                    .process(exchange -> {
                        String body = exchange.getMessage().getBody(String.class);
                        exchange.getMessage().setBody(body + " - Processed");
                    })
                    .log("Processed message: ${body}");
            }
        });

        context.start();

        // Send a test message to the route
        context.createProducerTemplate().sendBody("direct:start", "Hello Camel");
        
        Thread.sleep(2000); // Let it process
        context.stop();
    }
}
```

### 🔍 Output:
```
Original message: Hello Camel
Processed message: Hello Camel - Processed
```

Simple, clear, and shows the power of Processors!

---

## 📘 **Stage 1 – Lesson 3: Java DSL vs XML DSL**

Apache Camel supports **two primary ways** to define routes:
1. **Java DSL** (fluent style in Java classes)
2. **XML DSL** (used in Spring or XML configs)


### 1. 🧪 **Java DSL (Most Popular)**
> You’ve already been using this!

✅ **Pros:**
- Type-safe
- IDE-friendly (auto-complete)
- Easy to debug and refactor

```java
from("file:data/inbox?noop=true")
  .to("file:data/outbox");
```

✅ **Where used?**
- Standalone Java apps
- Spring Boot + Camel apps
- Embedded Camel apps

---

### 2. 🧾 **XML DSL**
Used heavily in Spring XML-based applications (older setups).

✅ Example:
```xml
<camelContext xmlns="http://camel.apache.org/schema/spring">
    <route>
        <from uri="file:data/inbox?noop=true"/>
        <to uri="file:data/outbox"/>
    </route>
</camelContext>
```

✅ **Where used?**
- Legacy enterprise Spring apps
- When configuration must be externalized

---

### 🔄 Which One Should You Use?
| Criteria | Java DSL | XML DSL |
|---------|----------|---------|
| Modern usage | ✅ Yes | ❌ Declining |
| Spring Boot integration | ✅ Excellent | ⚠️ Tricky |
| Readability | ✅ Clear and fluent | ❌ Verbose |
| Dynamic route creation | ✅ Yes | ❌ No |
| IDE support | ✅ Strong | ⚠️ Limited |

💡 **Best Practice:** Use **Java DSL** unless you're in a legacy Spring/XML-heavy environment.

---

## 🔥 Mini-Challenge
Can you rewrite the file route in XML?

Java:
```java
from("file:data/inbox?noop=true")
  .to("file:data/outbox");
```

Try converting that to XML on your own.

---
