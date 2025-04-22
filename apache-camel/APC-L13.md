
## 🧰 **Stage 3 – Lesson 14: Custom Components & Data Formats in Camel**

In this lesson, you'll learn:

- How to **create your own Camel Component**
- How to work with **DataFormats** (JSON, XML, CSV)
- How to **marshal (convert object → text)** and **unmarshal (text → object)** messages
- Real-world transformations & integrations

---

## 🔧 1. What is a Custom Component?

> A **Component** provides endpoint URIs like `file:`, `kafka:`, or `http:`.  
You can build your own to wrap any **external system**, **library**, or **protocol**.

### ✅ When to build your own?
- No built-in component exists for your integration
- You want to hide logic behind a clean URI
- You want reusable custom logic

---

## 📦 Example: Creating a Custom Component

### 1️⃣ Create a class that extends `DefaultComponent`:

```java
public class MyHelloComponent extends DefaultComponent {
    @Override
    protected Endpoint createEndpoint(String uri, String remaining, Map<String, Object> params) {
        return new MyHelloEndpoint(uri, this);
    }
}
```

### 2️⃣ Create the endpoint class:

```java
public class MyHelloEndpoint extends DefaultEndpoint {
    public MyHelloEndpoint(String uri, Component component) {
        super(uri, component);
    }

    @Override
    public Producer createProducer() {
        return new MyHelloProducer(this);
    }

    @Override
    public Consumer createConsumer(Processor processor) {
        return new MyHelloConsumer(this, processor);
    }
}
```

### 3️⃣ Producer example:

```java
public class MyHelloProducer extends DefaultProducer {
    public MyHelloProducer(Endpoint endpoint) {
        super(endpoint);
    }

    @Override
    public void process(Exchange exchange) {
        String name = exchange.getIn().getBody(String.class);
        exchange.getMessage().setBody("Hello " + name + " from custom component!");
    }
}
```

### 4️⃣ Register & Use:

```java
camelContext.addComponent("hello", new MyHelloComponent());
```

Use in route:
```java
from("direct:start")
    .to("hello:maya")
    .log("${body}");
```

---

## 📚 2. DataFormats (JSON, XML, CSV)

> `DataFormat` = Marshalling & Unmarshalling logic in Camel.

---

### ✅ JSON (with Jackson)

**Maven:**
```xml
<dependency>
  <groupId>org.apache.camel.springboot</groupId>
  <artifactId>camel-jackson-starter</artifactId>
</dependency>
```

**POJO:**
```java
public class User {
    public String name;
    public int age;
}
```

**Route:**
```java
from("direct:jsonIn")
    .unmarshal().json(User.class)
    .process(exchange -> {
        User user = exchange.getMessage().getBody(User.class);
        user.age += 1;
        exchange.getMessage().setBody(user);
    })
    .marshal().json()
    .to("log:jsonOut");
```

---

### ✅ XML (with JAXB)

```java
from("direct:xml")
    .unmarshal().jaxb("com.tushar.models")
    .marshal().jaxb("com.tushar.models");
```

💡 JAXB requires annotations like `@XmlRootElement`.

---

### ✅ CSV

```java
from("file:data/in?fileName=input.csv")
    .unmarshal().csv()
    .split(body())
    .process(exchange -> {
        List<String> row = exchange.getMessage().getBody(List.class);
        // Work with CSV row
    });
```

---

## 🚫 Common Pitfalls

| Problem | Fix |
|--------|-----|
| Wrong POJO mapping | Use Jackson annotations like `@JsonProperty` |
| Custom Component not picked up | Register in context or SPI |
| Can't unmarshal | Check classpath and format |

---

## 🧠 Real-World Scenario

You're building a route that:
- Consumes JSON from Kafka
- Converts it to a POJO
- Validates fields
- Converts to XML and writes to file

Camel simplifies this with chained steps.

---

## 🧪 Challenge (Optional)

Create a simple POJO (e.g., `Order{id, amount}`), accept a JSON input, convert to object, increase amount, and output as XML.

---
```java
import org.apache.camel.builder.RouteBuilder;
import org.apache.camel.model.dataformat.JsonLibrary;
import org.springframework.stereotype.Component;

@Component
public class OrderProcessingRoute extends RouteBuilder {

    @Override
    public void configure() throws Exception {
        from("direct:processOrder")
            .unmarshal().json(JsonLibrary.Jackson, Order.class)
            .bean(OrderProcessor.class, "increaseAmount")
            .marshal().jaxb("com.example.order");
    }
}

@Component
class OrderProcessor {
    public Order increaseAmount(Order order) {
        order.setAmount(order.getAmount() + 10);
        return order;
    }
}

import javax.xml.bind.annotation.XmlRootElement;

@XmlRootElement
class Order {
    private int id;
    private double amount;

    // Default constructor for JAXB
    public Order() {}

    public Order(int id, double amount) {
        this.id = id;
        this.amount = amount;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public double getAmount() {
        return amount;
    }

    public void setAmount(double amount) {
        this.amount = amount;
    }
}
```
