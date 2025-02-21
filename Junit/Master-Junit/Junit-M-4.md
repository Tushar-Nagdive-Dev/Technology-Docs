---

# **Phase 4: End-to-End Testing with Selenium**

---

## **Objective**

In this phase, we will:
- Automate **End-to-End (E2E) Tests** for:
  - **User Registration and Login**
  - **Product Addition and Listing**
  - **Order Creation and Tracking**
- Use **Selenium WebDriver** for browser automation.
- Test the **entire flow** from the frontend (UI) to the backend (API) and database.
- Ensure the application works seamlessly **from the user's perspective**.

---

## **1. Why Use Selenium for End-to-End Testing?**

- **Real User Simulation:** Selenium **automates browser actions** like clicking, typing, and navigation.
- **Cross-Browser Testing:** Supports multiple browsers (Chrome, Firefox, Edge, Safari).
- **End-to-End Flow Verification:** Ensures all components (UI, API, Database) work together.

---

## **2. E2E Test Strategy and Scope**

### **1. What to Test:**
- **User Registration and Login:**
  - Registration with valid and invalid data.
  - Successful and unsuccessful login attempts.
- **Product Management:**
  - Adding new products.
  - Listing all products.
  - Viewing product details.
- **Order Management:**
  - Creating new orders.
  - Viewing order details.
  - Order tracking.

### **2. What Not to Test:**
- **Internal API Logic:** Covered in **Unit** and **Integration Tests**.
- **Database Queries:** Covered in **Integration Tests**.

---

## **3. Setting Up Selenium with JUnit**

### **1. Add Selenium Dependencies**

**Maven:**
```xml
<dependency>
    <groupId>org.seleniumhq.selenium</groupId>
    <artifactId>selenium-java</artifactId>
    <version>4.11.0</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-engine</artifactId>
    <version>5.10.0</version>
    <scope>test</scope>
</dependency>
```

**Gradle:**
```groovy
testImplementation 'org.seleniumhq.selenium:selenium-java:4.11.0'
testImplementation 'org.junit.jupiter:junit-jupiter-engine:5.10.0'
```

---

### **2. Download and Set Up WebDriver**

- **ChromeDriver**: [https://chromedriver.chromium.org/downloads](https://chromedriver.chromium.org/downloads)
- **GeckoDriver (Firefox)**: [https://github.com/mozilla/geckodriver/releases](https://github.com/mozilla/geckodriver/releases)

**Add WebDriver to System Path** or specify the path in the code.

---

## **4. End-to-End Test for User Registration and Login**

Let's automate the **User Registration and Login** flow:
- Navigate to the **Registration Page**.
- Enter **valid and invalid data**.
- Verify successful and unsuccessful registrations.
- Navigate to the **Login Page**.
- Test **successful and unsuccessful logins**.

---

### **1. User Registration and Login Page Structure**

**Registration Page (HTML Structure):**
```html
<form id="registrationForm">
    <input type="text" id="name" name="name" placeholder="Name">
    <input type="email" id="email" name="email" placeholder="Email">
    <input type="password" id="password" name="password" placeholder="Password">
    <button type="submit" id="registerBtn">Register</button>
</form>
<div id="registrationSuccess" style="display: none;">Registration Successful!</div>
<div id="registrationError" style="display: none;">Error: Email already exists.</div>
```

**Login Page (HTML Structure):**
```html
<form id="loginForm">
    <input type="email" id="email" name="email" placeholder="Email">
    <input type="password" id="password" name="password" placeholder="Password">
    <button type="submit" id="loginBtn">Login</button>
</form>
<div id="loginSuccess" style="display: none;">Login Successful!</div>
<div id="loginError" style="display: none;">Invalid credentials.</div>
```

---

### **2. E2E Test for User Registration**

```java
package com.example.e2e;

import static org.junit.jupiter.api.Assertions.assertTrue;

import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

public class UserRegistrationTest {

    private WebDriver driver;

    @BeforeEach
    void setUp() {
        System.setProperty("webdriver.chrome.driver", "path/to/chromedriver");
        driver = new ChromeDriver();
        driver.manage().window().maximize();
    }

    @Test
    void testSuccessfulRegistration() {
        driver.get("https://example.com/register");

        WebElement nameField = driver.findElement(By.id("name"));
        WebElement emailField = driver.findElement(By.id("email"));
        WebElement passwordField = driver.findElement(By.id("password"));
        WebElement registerButton = driver.findElement(By.id("registerBtn"));

        nameField.sendKeys("John Doe");
        emailField.sendKeys("john@example.com");
        passwordField.sendKeys("password123");
        registerButton.click();

        WebElement successMessage = driver.findElement(By.id("registrationSuccess"));
        assertTrue(successMessage.isDisplayed(), "Success message should be displayed");
    }

    @Test
    void testRegistrationWithExistingEmail() {
        driver.get("https://example.com/register");

        WebElement nameField = driver.findElement(By.id("name"));
        WebElement emailField = driver.findElement(By.id("email"));
        WebElement passwordField = driver.findElement(By.id("password"));
        WebElement registerButton = driver.findElement(By.id("registerBtn"));

        nameField.sendKeys("John Doe");
        emailField.sendKeys("existing@example.com");
        passwordField.sendKeys("password123");
        registerButton.click();

        WebElement errorMessage = driver.findElement(By.id("registrationError"));
        assertTrue(errorMessage.isDisplayed(), "Error message should be displayed");
    }

    @AfterEach
    void tearDown() {
        driver.quit();
    }
}
```

---

### **3. Explanation:**
- **WebDriver Setup:**
  - `@BeforeEach`: Initializes ChromeDriver before each test.
  - `@AfterEach`: Closes the browser after each test.
- **Test Flow:**
  - Navigates to the registration page.
  - Fills in the registration form with:
    - Valid data for successful registration.
    - Existing email for unsuccessful registration.
  - Verifies success and error messages using `assertTrue()`.
- **Element Locators:**
  - `By.id(...)` is used for locating elements.
  - It matches the `id` attributes defined in the HTML structure.

### **Running the Test:**
- In **IntelliJ** or **Eclipse**: Right-click the `UserRegistrationTest` class → **Run**.
- Using **Maven**:
```bash
mvn test
```
- Using **Gradle**:
```bash
./gradlew test
```

### **Expected Output:**
- The browser is launched, and the registration flow is automated.
- **Successful Registration Test**:
  - The success message is displayed.
- **Unsuccessful Registration Test**:
  - The error message is displayed.

---

## **5. Additional E2E Tests**

### **1. Login Flow:**
- Automate successful and unsuccessful login attempts.
- Verify URL redirects and error messages.

### **2. Product Management:**
- Add a new product and verify it appears in the product listing.
- Check product details and inventory updates.

### **3. Order Management:**
- Create a new order and verify order details.
- Track the order status through the entire lifecycle.

---

## **Next Steps: Phase 5 - Performance Benchmarking with JMH**

In the next phase, we'll:
- Benchmark key methods for:
  - `OrderService.createOrder()`
  - `ProductService.getProductById()`
- Measure **average execution time** and **throughput**.
- Use **JMH (Java Microbenchmark Harness)** for high-precision performance testing.
