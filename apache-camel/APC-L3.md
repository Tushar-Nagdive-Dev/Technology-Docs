

---

## 🚀 **Stage 1 – Lesson 4: CamelContext and RouteBuilder**

These two are the **core backbone classes** in Apache Camel when using **Java DSL**.

---

### 🔧 1. **CamelContext**
Think of `CamelContext` as the **engine** of Apache Camel. It:
- Holds all routes, components, endpoints, processors.
- Manages lifecycle (start/stop).
- Can be extended to plug in things like custom components, error handlers, beans, etc.

#### 🔍 Key Methods:
```java
CamelContext context = new DefaultCamelContext();
context.addRoutes(...);     // Add routes
context.start();            // Start routing
context.stop();             // Stop routing
```

---

### 🧱 2. **RouteBuilder**
An **abstract class** used to define routes in **Java DSL**.

#### 💡 How it works:
You override `configure()` method to write your routes:
```java
public class MyRoute extends RouteBuilder {
    @Override
    public void configure() {
        from("timer:hello?period=1000")
            .log("Triggered by timer");
    }
}
```

You can add this route like:
```java
context.addRoutes(new MyRoute());
```

---

### ✅ Complete Example: RouteBuilder with CamelContext

```java
import org.apache.camel.CamelContext;
import org.apache.camel.builder.RouteBuilder;
import org.apache.camel.impl.DefaultCamelContext;

public class MyApp {
    public static void main(String[] args) throws Exception {
        CamelContext context = new DefaultCamelContext();

        context.addRoutes(new RouteBuilder() {
            @Override
            public void configure() {
                from("timer:tick?period=2000")
                    .log("Tick: ${date:now}");
            }
        });

        context.start();
        Thread.sleep(6000);
        context.stop();
    }
}
```

🧪 **Explanation:**
- `CamelContext` manages routes.
- `RouteBuilder` defines routing logic.
- `timer:tick` triggers every 2 seconds.
- Message is logged using `${date:now}` (a Camel simple expression).

---

## 🚫 Common Pitfalls
| Mistake | What happens |
|--------|--------------|
| Forgetting to start context | Routes never execute |
| Modifying context after start | Throws exception |
| Mixing up `getIn()` and `getMessage()` in Camel 3.x+ | Old `getOut()` is deprecated |

---

## 🎯 Hands-on Challenge
Create a route using `RouteBuilder` that:
- Starts with a `timer` every 3 seconds
- Logs a custom message like: "Heartbeat at ${date:now}"
- Stops after 10 seconds

Want to try it yourself? I can review your solution. Or I can provide the full code and move on.

---

```java
import org.apache.camel.builder.RouteBuilder;

public class CamelHeartbeatRoute extends RouteBuilder {
    @Override
    public void configure() throws Exception {
        from("timer:heartbeat?period=3000")
            .setBody(simple("Heartbeat at ${date:now}"))
            .to("log:heartbeat?showAll=true")
            .to("controlbus:route?routeId=heartbeatRoute&action=stop")
            .delay(10000);
    }
}
```
