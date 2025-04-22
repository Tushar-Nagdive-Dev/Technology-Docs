
## 🌱 **Stage 2 – Lesson 9: Apache Camel with Spring Boot**

> Apache Camel + Spring Boot = 🔥 Powerful, production-ready integration microservices.

---

## ✅ Why Use Camel with Spring Boot?

- **Auto-configuration** of Camel Context
- **Dependency injection** with Spring
- **RESTful endpoints + Camel routes**
- Easy **testing**, **packaging**, and **deployment**

---

## 📦 1. Project Setup

### 🔧 Maven Dependencies (`pom.xml`)

```xml
<dependencies>
  <!-- Spring Boot Starter -->
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
  </dependency>

  <!-- Camel Spring Boot Starter -->
  <dependency>
    <groupId>org.apache.camel.springboot</groupId>
    <artifactId>camel-spring-boot-starter</artifactId>
    <version>3.20.2</version>
  </dependency>

  <!-- Optional: File, Timer, Rest, etc. -->
  <dependency>
    <groupId>org.apache.camel.springboot</groupId>
    <artifactId>camel-file-starter</artifactId>
  </dependency>
</dependencies>
```

---

## 🏗️ 2. Directory Structure

```
src/
├── main/
│   ├── java/
│   │   └── com.tushar.camelapp/
│   │       ├── CamelSpringBootApp.java
│   │       └── routes/
│   │           └── MyFileRoute.java
│   └── resources/
│       └── application.properties
```

---

## 📝 3. Application Class

```java
@SpringBootApplication
public class CamelSpringBootApp {
    public static void main(String[] args) {
        SpringApplication.run(CamelSpringBootApp.class, args);
    }
}
```

---

## 🛣️ 4. Camel Route Example in Spring Boot

```java
@Component
public class MyFileRoute extends RouteBuilder {
    @Override
    public void configure() {
        from("file:data/inbox?noop=true")
            .to("file:data/outbox")
            .log("File moved: ${file:name}");
    }
}
```

---

## ⚙️ 5. application.properties

```properties
camel.springboot.name=CamelSpringApp
logging.level.org.apache.camel=INFO
```

---

## ✅ 6. Run the Application

```bash
mvn spring-boot:run
```

---

## 💡 Bonus: REST Integration with Camel

You can expose REST endpoints using Camel DSL itself.

### Example:

```java
@Component
public class RestRoute extends RouteBuilder {
    @Override
    public void configure() {
        restConfiguration().component("servlet").port(8080);

        rest("/api")
            .get("/hello")
            .route()
            .setBody(simple("Hello from Camel REST!"))
            .endRest();
    }
}
```

📌 Make sure to include `camel-servlet-starter` in `pom.xml`.

---

## ⚠️ Common Pitfalls

| Issue | Fix |
|-------|-----|
| Camel route not picked up | Ensure route class is annotated with `@Component` |
| Port not available | Change port in `application.properties` |
| REST not responding | Use `camel-servlet-starter` and map `/camel/*` in `web.xml` or Spring Boot config |

---

## 🧪 Challenge

- Create a REST endpoint: `/api/reverse`
- Accept a query param `text`
- Reverse the string using a Bean
- Return the reversed string in response

---

```java
import org.apache.camel.builder.RouteBuilder;
import org.apache.camel.model.rest.RestBindingMode;
import org.springframework.stereotype.Component;

@Component
public class ReverseStringRoute extends RouteBuilder {

    @Override
    public void configure() throws Exception {
        // Configure REST DSL
        restConfiguration()
            .component("servlet")
            .bindingMode(RestBindingMode.json);

        // Define REST endpoint
        rest("/api")
            .get("/reverse")
            .produces("text/plain")
            .param()
                .name("text")
                .type(org.apache.camel.model.rest.RestParamType.query)
                .description("Text to reverse")
            .endParam()
            .route()
            .bean(StringReverserBean.class, "reverse")
            .endRest();
    }
}

@Component
class StringReverserBean {
    public String reverse(String text) {
        if (text == null) {
            return "";
        }
        return new StringBuilder(text).reverse().toString();
    }
}
```
