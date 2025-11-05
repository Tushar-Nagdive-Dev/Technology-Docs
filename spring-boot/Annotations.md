## 🧱 1. **Core Spring Boot Annotations**

### `@SpringBootApplication`

* **Purpose:** The main entry point of a Spring Boot app.
* **Internally includes:**

  * `@Configuration` → marks class as a configuration class.
  * `@EnableAutoConfiguration` → automatically configures beans based on dependencies.
  * `@ComponentScan` → automatically scans and loads components (`@Component`, `@Service`, etc.) from the package.
* **Example:**

  ```java
  @SpringBootApplication
  public class MyApp {
      public static void main(String[] args) {
          SpringApplication.run(MyApp.class, args);
      }
  }
  ```

---

## 🧩 2. **Dependency Injection & Bean Management**

### `@Component`

* Marks a class as a Spring-managed bean (general purpose).
* Example:

  ```java
  @Component
  public class EmailService { ... }
  ```

### `@Service`

* A specialized form of `@Component` used for **service layer** classes.
* Helps make the code more readable and structured.

### `@Repository`

* Another specialization of `@Component`.
* Used for **DAO / database layer** classes.
* Also adds automatic exception translation for database errors.

### `@Controller`

* Marks a class as a **web controller** (for MVC web apps).
* Handles web requests and returns **views** (like HTML pages).

### `@RestController`

* Combines `@Controller` + `@ResponseBody`.
* Used in **REST APIs** — returns JSON or XML directly instead of HTML.

---

## ⚙️ 3. **Configuration and Bean Definition**

### `@Configuration`

* Marks a class as a source of **bean definitions**.
* Used instead of XML configuration.

  ```java
  @Configuration
  public class AppConfig {
      @Bean
      public MyService myService() {
          return new MyService();
      }
  }
  ```

### `@Bean`

* Defines a bean inside a `@Configuration` class.
* Used for manual bean creation when auto-scanning is not enough.

### `@Value`

* Injects values from **application.properties** or **environment variables**.

  ```java
  @Value("${app.name}")
  private String appName;
  ```

### `@PropertySource`

* Loads properties from a custom `.properties` file.

---

## 🧭 4. **Dependency Injection (DI) Helpers**

### `@Autowired`

* Automatically injects a bean dependency.

  ```java
  @Autowired
  private UserService userService;
  ```

### `@Qualifier`

* Used with `@Autowired` when multiple beans of the same type exist.

  ```java
  @Autowired
  @Qualifier("emailService")
  private NotificationService service;
  ```

### `@Primary`

* Marks a bean as the **default** bean if multiple candidates exist.

### `@Lazy`

* Delays bean creation until it’s actually needed.

### `@Scope`

* Defines bean scope:

  * `singleton` (default)
  * `prototype`
  * `request`, `session`, `application` (for web apps)

---

## 🌐 5. **Web & REST Controllers**

### `@RequestMapping`

* Maps an HTTP request to a method or class.

  ```java
  @RequestMapping("/users")
  public String getUsers() { ... }
  ```

### `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`, `@PatchMapping`

* Shortcut annotations for HTTP methods:

  ```java
  @GetMapping("/users/{id}")
  public User getUser(@PathVariable int id) { ... }
  ```

### `@PathVariable`

* Extracts a variable from the URL.

  ```java
  @GetMapping("/users/{id}")
  public User getUser(@PathVariable("id") int userId) { ... }
  ```

### `@RequestParam`

* Extracts query parameters from URL.

  ```java
  @GetMapping("/search")
  public List<User> search(@RequestParam String name) { ... }
  ```

### `@RequestBody`

* Maps request JSON body to a Java object.

### `@ResponseBody`

* Converts method return value to JSON/XML (used automatically with `@RestController`).

### `@CrossOrigin`

* Enables **CORS** for specific endpoints.

---

## 🧠 6. **Validation and Data Binding**

### `@Valid` / `@Validated`

* Used for validating request data before processing.

### `@NotNull`, `@Size`, `@Email`, etc.

* Bean Validation annotations (from `javax.validation.constraints`).

---

## 🗃️ 7. **JPA and Database Annotations**

### `@Entity`

* Marks a class as a **JPA entity** (database table).

### `@Table`

* Specifies table name and schema.

### `@Id`

* Marks the **primary key** field.

### `@GeneratedValue`

* Defines how the primary key is auto-generated.

### `@Column`

* Maps a field to a specific column name.

### `@OneToOne`, `@OneToMany`, `@ManyToOne`, `@ManyToMany`

* Define relationships between entities.

### `@JoinColumn`

* Defines foreign key mapping.

### `@Transactional`

* Ensures that a method or class runs within a **database transaction**.

---

## ☁️ 8. **Spring Boot Auto Configuration and Profiles**

### `@EnableAutoConfiguration`

* Tells Spring Boot to configure your app automatically based on classpath dependencies.

### `@Profile`

* Activates a bean only for a specific **environment** (like `dev`, `prod`).

### `@ConditionalOnProperty`, `@ConditionalOnClass`, `@ConditionalOnMissingBean`

* Used to create **conditional beans** (auto-configuration control).

---

## 🧩 9. **Scheduling & Async Processing**

### `@EnableScheduling`

* Enables scheduling tasks.

### `@Scheduled`

* Marks a method to run at specific intervals.

  ```java
  @Scheduled(cron = "0 0 * * * *") // every hour
  public void cleanup() { ... }
  ```

### `@EnableAsync`

* Enables asynchronous method execution.

### `@Async`

* Runs a method in a **separate thread**.

  ```java
  @Async
  public void sendEmail() { ... }
  ```

---

## 🛡️ 10. **Spring Security**

### `@EnableWebSecurity`

* Enables Spring Security configuration.

### `@PreAuthorize`, `@PostAuthorize`

* Used for **method-level security**.

  ```java
  @PreAuthorize("hasRole('ADMIN')")
  public void deleteUser() { ... }
  ```

### `@Secured`

* Simpler role-based method security.

---

## 🧰 11. **Testing Annotations**

### `@SpringBootTest`

* Loads full application context for integration testing.

### `@WebMvcTest`

* Loads only **web layer** for controller testing.

### `@DataJpaTest`

* Loads **JPA layer** for repository testing.

### `@MockBean`

* Creates mock beans to isolate test cases.

### `@TestConfiguration`

* Special configuration class used only for testing.

---

## 🪄 12. **Other Useful Annotations**

### `@Import`

* Imports configuration from another class.

### `@EnableConfigurationProperties`

* Enables `@ConfigurationProperties` beans.

### `@ConfigurationProperties`

* Maps properties from `.properties` file to a POJO.

  ```java
  @ConfigurationProperties(prefix = "app")
  public class AppProperties {
      private String name;
      private String version;
  }
  ```

---

## 💡 Summary Table (Quick Glance)

| Category        | Common Annotations                                                     |
| --------------- | ---------------------------------------------------------------------- |
| Core            | `@SpringBootApplication`, `@EnableAutoConfiguration`, `@ComponentScan` |
| Beans & DI      | `@Component`, `@Service`, `@Repository`, `@Autowired`, `@Qualifier`    |
| Web             | `@RestController`, `@GetMapping`, `@RequestBody`, `@PathVariable`      |
| JPA             | `@Entity`, `@Table`, `@Id`, `@Transactional`                           |
| Config          | `@Configuration`, `@Bean`, `@Value`, `@PropertySource`                 |
| Async/Scheduler | `@Async`, `@Scheduled`, `@EnableScheduling`                            |
| Profiles        | `@Profile`, `@ConditionalOnProperty`                                   |
| Security        | `@EnableWebSecurity`, `@PreAuthorize`, `@Secured`                      |
| Testing         | `@SpringBootTest`, `@MockBean`, `@WebMvcTest`                          |

---

Would you like me to give you a **PDF-style cheat sheet version** (visually summarized + categorized for revision) next, Tushar?
It’s perfect for interviews and quick reference.
