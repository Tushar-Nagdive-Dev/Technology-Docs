## 🔐 **Stage 2 – Lesson 13: Securing Apache Camel Applications**

In this lesson, we’ll cover how to **secure Camel routes**, REST APIs, Kafka communications, and even the message data itself.

---

## 🛡️ 1. **Securing REST Endpoints in Camel + Spring Boot**

Use **Spring Security** with Camel to secure exposed APIs.

### ✅ Step 1: Add Spring Security

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

### ✅ Step 2: Add Basic Auth Configuration

```java
@Configuration
public class SecurityConfig {
    @Bean
    public InMemoryUserDetailsManager userDetailsService() {
        return new InMemoryUserDetailsManager(
            User.withUsername("admin").password("{noop}password").roles("ADMIN").build()
        );
    }
}
```

### ✅ Step 3: Secure REST Camel Route

```java
@Component
public class RestSecureRoute extends RouteBuilder {
    @Override
    public void configure() {
        restConfiguration().component("servlet").contextPath("/camel");

        rest("/secure")
            .get("/hello")
            .route()
            .setBody(simple("Hello secured world"))
            .endRest();
    }
}
```

🔐 You must now call with basic auth:  
`curl -u admin:password http://localhost:8080/camel/secure/hello`

---

## 🔑 2. **Securing Kafka (SSL + SASL)**

### ✅ Add Kafka security properties in `application.properties`:

```properties
camel.component.kafka.brokers=localhost:9093
camel.component.kafka.security-protocol=SSL
camel.component.kafka.ssl-keystore-location=/path/to/keystore.jks
camel.component.kafka.ssl-keystore-password=changeit
```

📌 Camel internally maps these to Kafka consumer/producer props.

---

## 📜 3. **Input Validation**

Validate incoming requests using:
- Beans (manual validation)
- Javax Validation (`@Valid`)
- JSON schema (with Camel component)

### ✅ Bean-based validation

```java
@Component
public class ValidatorBean {
    public void validate(String body) {
        if (!body.matches("[a-zA-Z ]+")) {
            throw new IllegalArgumentException("Invalid characters in input");
        }
    }
}
```

In route:
```java
from("rest:get:/api/input")
    .bean(ValidatorBean.class)
    .to("log:validInput");
```

---

## 🔐 4. **Message Encryption/Decryption**

Use AES or RSA for secure payload handling.

### ✅ Example – AES (Java standard lib)

```java
public class CryptoUtils {
    private static final String KEY = "1234567812345678"; // 16-char key

    public String encrypt(String msg) {
        // Use javax.crypto.Cipher with AES/ECB/PKCS5Padding
    }

    public String decrypt(String encrypted) {
        // Same, reversed
    }
}
```

💡 You can inject this into a Camel bean step:
```java
.to("bean:cryptoUtils?method=decrypt")
```

✅ Or use Apache Camel `crypto` component (if more advanced needs).

---

## 🛠️ 5. Logging & Masking Sensitive Data

Never log passwords or credit card numbers directly.

```java
.log("Received order: ${body.replaceAll('\"cc\":\"[0-9]+\"', '\"cc\":\"****\"')}")
```

---

## 🧠 Real-World Use Case

Secure payment API:
- Accepts REST JSON payload
- Validates card number
- Encrypts payload
- Publishes to Kafka
- Logs only masked values

---

## ⚠️ Common Security Pitfalls

| Pitfall | Fix |
|--------|-----|
| Logging raw payloads | Always mask sensitive values |
| Unauthenticated REST APIs | Use Spring Security or API keys |
| No validation | Add input checks before processing |
| Kafka in plaintext | Use SSL/SASL in prod environments |

---

## 🧪 Challenge (Optional)

Create a secured `/api/secure/hello` endpoint:
- Requires basic auth
- Calls a bean to return `"Hi, <username>!"`

---
```java
import org.apache.camel.builder.RouteBuilder;
import org.apache.camel.model.rest.RestBindingMode;
import org.springframework.stereotype.Component;

@Component
public class SecureHelloRoute extends RouteBuilder {

    @Override
    public void configure() throws Exception {
        // Configure REST DSL
        restConfiguration()
            .component("servlet")
            .bindingMode(RestBindingMode.json);

        // Define secured REST endpoint
        rest("/api")
            .get("/secure/hello")
            .produces("text/plain")
            .route()
            .to("spring-security:basicAuth")
            .bean(GreetingBean.class, "greet")
            .endRest();
    }
}

@Component
class GreetingBean {
    public String greet(@org.apache.camel.Header("CamelAuthenticatedUser") String username) {
        return "Hi, " + username + "!";
    }
}
```
