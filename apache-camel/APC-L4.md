

## 📦 **Stage 1 – Lesson 5: Camel Components (Beginner Essentials)**

Apache Camel is loved for its **huge library of 200+ components** that let you connect to nearly anything.

---

### 🔍 **What is a Camel Component?**
A **Component** is a pluggable module that provides **endpoint support** for various protocols, APIs, databases, file systems, and more.

> You use components in URIs like `file:`, `http:`, `timer:`, `kafka:`, `jms:`, etc.

---

## 📚 Commonly Used Core Components

| Component | Description | URI Example |
|----------|-------------|-------------|
| `file` | Read/write files | `file:data/inbox` |
| `timer` | Scheduled trigger | `timer:tick?period=1000` |
| `http` / `https` | HTTP request/response | `http://example.com/api` |
| `log` | Logs message to console | `log:myLogger` |
| `direct` | Internal route call | `direct:routeA` |
| `kafka` | Kafka topic integration | `kafka:topicName` |
| `jms` | JMS queue integration | `jms:queue:orders` |

---

### ✅ Let's explore the most basic ones hands-on:

---

### 1. 🗂️ **File Component**
Reads files from a directory.

```java
from("file:data/inbox?noop=true")
    .to("file:data/outbox");
```

- `noop=true`: Don't delete or move the file (read-only)
- `file:` works with relative or absolute paths

---

### 2. ⏰ **Timer Component**
Triggers periodically (used to simulate events).

```java
from("timer:tick?period=2000")
    .log("Triggered at: ${date:now}");
```

---

### 3. 🌐 **HTTP Component**
Calls external HTTP endpoints.

```java
from("timer:callApi?period=5000")
    .to("https://api.chucknorris.io/jokes/random")
    .log("Joke: ${body}");
```

📌 Add Maven dependency:
```xml
<dependency>
    <groupId>org.apache.camel</groupId>
    <artifactId>camel-http</artifactId>
    <version>3.20.2</version>
</dependency>
```

---

### 4. 🔁 **Direct Component**
Used for internal routing. Great for unit testing and modular flows.

```java
from("direct:start")
    .log("Hello from direct:start");

context.createProducerTemplate().sendBody("direct:start", "Triggered!");
```

---

## ⚠️ Common Mistakes to Avoid
| Mistake | Explanation |
|--------|-------------|
| Missing component dependency | HTTP, Kafka, JMS need to be added manually in `pom.xml` |
| Using wrong URI syntax | Always double-check the format: `file:`, not `files:` |
| Forgetting query params | `file:dir?noop=true` is valid, `file:dir` alone may delete the file |

---

## 🛠️ Exercise for You

💪 Create a route that:
1. Uses a `timer` to trigger every 2 seconds.
2. Makes a call to a public API (like [https://catfact.ninja/fact](https://catfact.ninja/fact)).
3. Logs the response to the console.

Let me know if you want to try this on your own or want me to show you the complete solution.

---

### ✅ Coming Up Next

```java
import org.apache.camel.builder.RouteBuilder;

public class CatFactRoute extends RouteBuilder {
    @Override
    public void configure() throws Exception {
        from("timer:catFact?period=2000")
            .to("https://catfact.ninja/fact")
            .log("Cat Fact: ${body}");
    }
}
```
