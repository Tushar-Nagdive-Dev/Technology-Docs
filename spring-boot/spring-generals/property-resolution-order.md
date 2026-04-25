Thinking of property resolution in Spring Boot is like **deciding what to wear for the day**. You might have a plan, but different situations can override that plan. 

Here is the simple analogy:
1. **Your Default Wardrobe (Lowest Priority):** You usually wear jeans and a t-shirt. (`application.properties` inside your code).
2. **The Weather (Medium Priority):** You look outside and it's snowing. The weather overrides your normal plan, so you put on a heavy coat. (Environment Variables on your server).
3. **Your Boss (Highest Priority):** Your boss calls you right as you are walking out the door and says, "We have a huge client meeting, you *must* wear a suit." You immediately change. (Command-Line Arguments passed when starting the app).

In Spring Boot, if you define the exact same property (like `server.port`) in three different places, Spring Boot uses a strict hierarchy to decide which one "wins". The higher up on the list, the more power it has.

### The Spring Boot Property Resolution Order
*(From Highest Precedence to Lowest)*

1. **Command Line Arguments:** `java -jar myapp.jar --server.port=9090`
2. **Java System Properties:** `java -Dserver.port=9090 -jar myapp.jar`
3. **OS Environment Variables:** `SERVER_PORT=9090`
4. **Profile-Specific Properties (Outside Jar):** `application-prod.properties` located in the folder next to your running Jar.
5. **Profile-Specific Properties (Inside Jar):** `application-prod.properties` packaged inside your code.
6. **Standard Properties (Outside Jar):** `application.properties` next to your running Jar.
7. **Standard Properties (Inside Jar):** `application.properties` packaged inside your code.
8. **`@PropertySource`:** Annotations on your `@Configuration` classes.
9. **Default Properties:** Specified using `SpringApplication.setDefaultProperties`.

---

### Java Code Examples

Regardless of *where* the property comes from, you read it in your Java code the exact same way. Spring Boot handles the hierarchy for you invisibly.

**1. Using `@Value` (Good for single, simple properties)**
```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MessageController {

    // Spring Boot will look for "app.greeting" in the hierarchy.
    // If it can't find it anywhere, it defaults to "Hello Default!"
    @Value("${app.greeting:Hello Default!}")
    private String greeting;

    @GetMapping("/hello")
    public String sayHello() {
        return greeting;
    }
}
```

**2. Using `@ConfigurationProperties` (Best for grouping related properties)**
If you have properties like `mail.host`, `mail.port`, and `mail.username`.
```java
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Component
@ConfigurationProperties(prefix = "mail")
public class MailConfig {
    
    private String host;
    private int port;
    private String username;

    // Getters and Setters are required for this to work!
    public String getHost() { return host; }
    public void setHost(String host) { this.host = host; }
    // ... other getters and setters
}
```


[![Preview](https://img.shields.io/badge/🚀-Preview-blue?style=for-the-badge)](https://htmlpreview.github.io/?https://github.com/Tushar-Nagdive/StackBlueprint/blob/StackTech/spring-boot/spring-generals/visuals/property_resolution_visual.html)