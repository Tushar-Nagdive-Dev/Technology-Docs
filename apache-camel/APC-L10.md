
## ✅ **Stage 2 – Lesson 11: Testing Apache Camel Routes (Unit & Integration)**

> Apache Camel provides powerful **testing utilities** that make routes testable, flexible, and reliable.

We'll cover:
- **Unit Testing Camel Routes**
- **MockEndpoints**
- **AdviceWith** (to modify routes during test)
- **Best practices for testing routes**

---

## 🧪 1. Testing Setup with JUnit

### ✅ Maven Dependencies

```xml
<dependency>
  <groupId>org.apache.camel</groupId>
  <artifactId>camel-test-spring-junit5</artifactId>
  <version>3.20.2</version>
  <scope>test</scope>
</dependency>
```

---

## ✏️ 2. Example Route to Test

```java
@Component
public class SimpleRoute extends RouteBuilder {
    @Override
    public void configure() {
        from("direct:start")
            .to("log:original")
            .to("mock:result");
    }
}
```

---

## 🔍 3. Writing the Test

```java
@CamelSpringBootTest
@SpringBootTest
public class SimpleRouteTest {

    @Autowired
    private CamelContext camelContext;

    @EndpointInject("mock:result")
    private MockEndpoint mockEndpoint;

    @Produce("direct:start")
    private ProducerTemplate producerTemplate;

    @Test
    void testSimpleRoute() throws Exception {
        mockEndpoint.expectedMessageCount(1);
        mockEndpoint.expectedBodiesReceived("Hello Maya");

        producerTemplate.sendBody("Hello Maya");

        mockEndpoint.assertIsSatisfied();
    }
}
```

---

## 🔁 4. Modify Route During Testing (AdviceWith)

`AdviceWith` is **super handy** when you want to:
- Skip calling real APIs
- Inject mocks in place of actual endpoints
- Dynamically alter routes before testing

### ✅ Example:

```java
@Test
void testWithAdvice() throws Exception {
    camelContext.getRouteDefinition("myRouteId")
        .adviceWith(camelContext, new AdviceWithRouteBuilder() {
            @Override
            public void configure() {
                weaveByToUri("file:*").replace().to("mock:file");
            }
        });

    camelContext.start();

    MockEndpoint mockFile = camelContext.getEndpoint("mock:file", MockEndpoint.class);
    mockFile.expectedMessageCount(1);
    producerTemplate.sendBody("direct:start", "test");
    mockFile.assertIsSatisfied();
}
```

🧠 Replace `.to("file:data/outbox")` with `.to("mock:file")` only in the test context.

---

## 🛠️ Tips & Best Practices

| Practice | Benefit |
|---------|---------|
| Use `mock:*` in production route | Easily test without AdviceWith |
| Use `ProducerTemplate` to simulate input | Test source of route |
| Isolate each test class to one route | Easier debugging |
| Use `.expectedBodiesReceived(...)` | Validates correctness |
| Assert header values too | `mockEndpoint.expectedHeaderReceived(...)` |

---

## 📌 Challenge (Optional)

Write a test for a route:
- From `direct:input`
- Converts String to upper case
- Sends to `mock:result`
- Test that `MOCK` receives `"HELLO"` when input is `"hello"`

---
```java
import org.apache.camel.builder.RouteBuilder;
import org.apache.camel.test.junit5.CamelTestSupport;
import org.junit.jupiter.api.Test;

import static org.apache.camel.test.junit5.TestSupport.assertMessageReceived;

public class UpperCaseRouteTest extends CamelTestSupport {

    @Override
    protected RouteBuilder createRouteBuilder() {
        return new RouteBuilder() {
            @Override
            public void configure() {
                from("direct:input")
                    .transform(simple("${body.toUpperCase()}"))
                    .to("mock:result");
            }
        };
    }

    @Test
    public void testRouteConvertsToUpperCase() throws Exception {
        // Setup mock expectations
        getMockEndpoint("mock:result").expectedBodiesReceived("HELLO");

        // Send test message
        template.sendBody("direct:input", "hello");

        // Verify expectations
        assertMockEndpointsSatisfied();
    }
}
```
