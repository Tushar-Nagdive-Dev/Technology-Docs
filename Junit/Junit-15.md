---

# **Phase 5 - Lesson 15: Real-World Projects and Case Studies**

---

## **1. End-to-End Testing with JUnit and Selenium**

### **1. What is End-to-End (E2E) Testing?**
- End-to-End Testing **simulates real user scenarios**.
- It tests the entire application flow, from the frontend (UI) to the backend (API) and database.
- Ensures that all components work together seamlessly.

---

### **2. Why Use Selenium with JUnit?**
- **Selenium** is the most popular web automation framework for testing web applications.
- It allows:
  - **Automating browser interactions** (clicks, form submissions, navigation).
  - **Cross-browser testing** (Chrome, Firefox, Edge, etc.).
  - **Integration with JUnit** for structured test cases and reports.

---

### **3. Setting Up Selenium with JUnit**

### **1. Add Selenium Dependency**

**Maven:**
```xml
<dependency>
    <groupId>org.seleniumhq.selenium</groupId>
    <artifactId>selenium-java</artifactId>
    <version>4.11.0</version>
</dependency>
```

**Gradle:**
```groovy
testImplementation 'org.seleniumhq.selenium:selenium-java:4.11.0'
```

### **2. Download WebDriver**
- **ChromeDriver**: [https://chromedriver.chromium.org/downloads](https://chromedriver.chromium.org/downloads)
- **GeckoDriver (Firefox)**: [https://github.com/mozilla/geckodriver/releases](https://github.com/mozilla/geckodriver/releases)
- **EdgeDriver**: [https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/](https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/)

**Add the WebDriver to System Path** or specify the path in the code.

---

### **4. Example: End-to-End Test for a Login Page**

Let's write an E2E test for a **Login Page** that:
- Navigates to the login page.
- Enters the username and password.
- Clicks the login button.
- Verifies the successful login by checking the URL or welcome message.

---

#### **LoginTest Class**

```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

public class LoginTest {

    private WebDriver driver;

    @BeforeEach
    void setUp() {
        System.setProperty("webdriver.chrome.driver", "path/to/chromedriver");
        driver = new ChromeDriver();
        driver.manage().window().maximize();
    }

    @Test
    void testLogin_Successful() {
        driver.get("https://example.com/login");

        WebElement usernameField = driver.findElement(By.id("username"));
        WebElement passwordField = driver.findElement(By.id("password"));
        WebElement loginButton = driver.findElement(By.id("loginBtn"));

        usernameField.sendKeys("testuser");
        passwordField.sendKeys("testpassword");
        loginButton.click();

        String expectedUrl = "https://example.com/dashboard";
        assertEquals(expectedUrl, driver.getCurrentUrl(), "User should be redirected to dashboard");
    }

    @Test
    void testLogin_InvalidCredentials() {
        driver.get("https://example.com/login");

        WebElement usernameField = driver.findElement(By.id("username"));
        WebElement passwordField = driver.findElement(By.id("password"));
        WebElement loginButton = driver.findElement(By.id("loginBtn"));

        usernameField.sendKeys("wronguser");
        passwordField.sendKeys("wrongpassword");
        loginButton.click();

        WebElement errorMessage = driver.findElement(By.id("error"));
        assertTrue(errorMessage.isDisplayed(), "Error message should be displayed for invalid credentials");
    }

    @AfterEach
    void tearDown() {
        driver.quit();
    }
}
```

---

### **Explanation:**
- `@BeforeEach`: Sets up the ChromeDriver before each test.
- `driver.get("URL")`: Navigates to the login page.
- `driver.findElement(By.id(...))`: Locates elements using their HTML `id`.
- `sendKeys(...)`: Simulates typing into the input fields.
- `click()`: Clicks the login button.
- `@AfterEach`: Quits the browser after each test to **avoid memory leaks**.
- **Assertions**:
  - `assertEquals()` checks the current URL after successful login.
  - `assertTrue()` checks if the error message is displayed for invalid credentials.

### **Running the Test:**
- In **IntelliJ** or **Eclipse**: Right-click the `LoginTest` class → **Run**.
- Using **Maven**:
```bash
mvn test
```
- Using **Gradle**:
```bash
./gradlew test
```

### **Expected Output:**
- **Successful Login Test**:
  - The browser navigates to the dashboard.
  - The test passes if the URL matches the expected dashboard URL.
- **Invalid Credentials Test**:
  - An error message is displayed.
  - The test passes if the error message is visible.

---

## **2. Testing Microservices Architecture Using Spring Cloud and WireMock**

### **1. What is WireMock?**
- **WireMock** is a library for **mocking HTTP services**.
- It allows you to:
  - **Stub external APIs** for integration testing.
  - **Simulate network errors** and latency.
  - **Verify HTTP requests** made by the application.

### **2. Why Use WireMock for Microservices?**
- **Isolate Microservices:** Test a microservice in isolation by mocking its dependencies.
- **Consistent Test Results:** Avoid dependency on unstable or unavailable external APIs.
- **Simulate Real-World Scenarios:** Test edge cases like timeouts, 404 errors, or slow responses.

---

### **3. Example: Mocking an External API with WireMock**

Let's test a microservice that fetches user data from an external API.

### **Add WireMock Dependency**

**Maven:**
```xml
<dependency>
    <groupId>com.github.tomakehurst</groupId>
    <artifactId>wiremock-jre8</artifactId>
    <version>2.35.0</version>
    <scope>test</scope>
</dependency>
```

**Gradle:**
```groovy
testImplementation 'com.github.tomakehurst:wiremock-jre8:2.35.0'
```

---

### **4. Example: UserService Test with WireMock**

```java
import static com.github.tomakehurst.wiremock.client.WireMock.*;
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import com.github.tomakehurst.wiremock.WireMockServer;
import com.github.tomakehurst.wiremock.core.WireMockConfiguration;

public class UserServiceWireMockTest {

    private WireMockServer wireMockServer;

    @BeforeEach
    void setUp() {
        wireMockServer = new WireMockServer(WireMockConfiguration.wireMockConfig().port(8089));
        wireMockServer.start();
        configureFor("localhost", 8089);
    }

    @Test
    void testGetUser_Success() {
        stubFor(get(urlEqualTo("/api/users/1"))
                .willReturn(aResponse()
                        .withStatus(200)
                        .withHeader("Content-Type", "application/json")
                        .withBody("{\"id\": 1, \"name\": \"John Doe\", \"email\": \"john@example.com\"}")));

        UserService userService = new UserService();
        User user = userService.getUserById(1);

        assertNotNull(user);
        assertEquals("John Doe", user.getName());
    }

    @Test
    void testGetUser_NotFound() {
        stubFor(get(urlEqualTo("/api/users/999"))
                .willReturn(aResponse()
                        .withStatus(404)));

        UserService userService = new UserService();
        User user = userService.getUserById(999);

        assertNull(user);
    }

    @AfterEach
    void tearDown() {
        wireMockServer.stop();
    }
}
```

### **Explanation:**
- **Stubbing Responses:** Mocks the external API responses using `stubFor()`.
- **Simulating Errors:** Simulates a `404 Not Found` error.
- **Isolated Testing:** Tests the `UserService` without calling the real API.

---

## **Next Steps: Conclusion and Mastery Roadmap**
You've completed the **JUnit Mastery Course**! Next, we'll:
- Recap the key learnings.
- Provide a mastery roadmap for continuous learning.
- Share advanced resources and industry insights.
