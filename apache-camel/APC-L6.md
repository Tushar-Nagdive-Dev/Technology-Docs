Fantastic, Tushar! You're now entering the **core mechanics** of Camel's processing pipeline. Let's dive into:

---

## ⚙️ **Stage 1 – Lesson 7: Processors, Beans, and Type Conversions in Camel**

Apache Camel gives you a lot of **control** over messages during routing, especially through **Processors**, **Beans**, and **automatic type conversion**.

---

### 🔧 1. **Processor (Custom Java Logic)**

A `Processor` is a Java class that lets you **manipulate the message or exchange**.

#### ✅ Example – Uppercase Message

```java
from("direct:process")
    .process(exchange -> {
        String body = exchange.getMessage().getBody(String.class);
        exchange.getMessage().setBody(body.toUpperCase());
    })
    .to("log:processed");
```

---

### 🫘 2. **Beans (Reusable Java Components)**

You can call regular Java methods inside your routes using the `.bean()` DSL.

#### ✅ Step 1: Create a Bean

```java
public class GreetingBean {
    public String sayHello(String name) {
        return "Hello " + name;
    }
}
```

#### ✅ Step 2: Register and Use in Route

```java
GreetingBean bean = new GreetingBean();
context.getRegistry().bind("greetingBean", bean);

from("direct:greet")
    .bean("greetingBean", "sayHello")
    .to("log:greeting");
```

📌 You can also use **Spring Beans** if you're using Spring Boot.

---

### 🔄 3. **Type Conversions**

Camel can automatically convert between types using **TypeConverter**.

#### ✅ Example:

```java
from("direct:convert")
    .convertBodyTo(Integer.class)
    .process(exchange -> {
        Integer value = exchange.getMessage().getBody(Integer.class);
        exchange.getMessage().setBody("Doubled: " + (value * 2));
    })
    .to("log:converted");
```

✅ Input: `"4"` (as a String)  
✅ Output: `"Doubled: 8"`

Camel can convert:
- `String ↔ Integer`
- `InputStream ↔ String`
- `File ↔ String`
- JSON ↔ POJO (with Jackson, Gson)

📌 Use `@BindToRegistry` in Spring Boot to register beans easily.

---

## 🧠 Real-World Example

```java
from("file:orders")
    .convertBodyTo(String.class)
    .bean(OrderValidator.class, "validate")
    .to("bean:orderService?method=process")
    .to("jms:queue:processedOrders");
```

- Converts file content to String.
- Validates using a `bean`.
- Passes to service logic.

---

## 🚫 Common Mistakes

| Mistake | What happens |
|--------|--------------|
| Not registering a bean | Camel throws runtime error |
| Wrong method signature | Camel can’t find the method |
| Wrong type conversion | `TypeConversionException` at runtime |

---

## 🧪 Challenge

Create a bean called `MultiplierBean` with a method:
```java
public int multiplyByFive(int input)
```

Then create a route that:
1. Accepts input via `direct:start`
2. Converts input to `Integer`
3. Calls the bean method
4. Logs the result
