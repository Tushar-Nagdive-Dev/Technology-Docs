---

# **Phase 1 - Lesson 2: Setting Up JUnit**

---

## **1. Setting Up JUnit in Your Project**

To start writing and running JUnit tests, you first need to **set up JUnit** in your project. This involves adding the necessary dependencies and configuring your development environment. 

We'll cover the following:
- Setting up JUnit using **Maven** and **Gradle**.
- Configuring JUnit in popular IDEs:
  - **IntelliJ IDEA**
  - **Eclipse**
  - **Visual Studio Code**

---

## **2. Using JUnit with Maven**

Maven is a popular build automation tool for Java projects. Let's add JUnit 5 dependencies in a Maven project.

### **Adding JUnit 5 Dependency in Maven**
1. **Open your `pom.xml` file**.
2. **Add the following dependencies**:

```xml
<dependencies>
    <!-- JUnit 5 Jupiter API and Engine -->
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-engine</artifactId>
        <version>5.9.3</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-api</artifactId>
        <version>5.9.3</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

### **Build and Refresh the Maven Project:**
- In **IntelliJ**: Right-click the project → **Maven** → **Reload Project**.
- In **Eclipse**: Right-click the project → **Maven** → **Update Project**.

### **Running Tests in Maven:**
- **Command Line**:
  ```bash
  mvn test
  ```
- This will **compile and run all test cases** in the `src/test/java` directory.

---

## **3. Using JUnit with Gradle**

Gradle is another popular build tool, especially in modern Java projects. 

### **Adding JUnit 5 Dependency in Gradle**

1. **Open your `build.gradle` file**.
2. **Add the following dependencies**:

```groovy
dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-engine:5.9.3'
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.9.3'
}

test {
    useJUnitPlatform()
}
```

### **Build and Refresh the Gradle Project:**
- In **IntelliJ**: Right-click the project → **Gradle** → **Refresh Gradle Project**.
- In **Eclipse**: Right-click the project → **Gradle** → **Refresh Gradle Project**.

### **Running Tests in Gradle:**
- **Command Line**:
  ```bash
  ./gradlew test
  ```
- This will **compile and run all test cases** in the `src/test/java` directory.

---

## **4. Setting Up JUnit in IDEs**

### **1. IntelliJ IDEA**
- IntelliJ natively supports JUnit 5.
- **Enable JUnit 5:**
  - Go to **File → Project Structure → Libraries**.
  - Ensure that the JUnit dependencies are properly linked.
- **Running Tests:**
  - Right-click on the test class or method → **Run**.
  - Use the shortcut **Shift + F10** to run the last test.

---

### **2. Eclipse**
- **Installing JUnit Plugin:**
  - Go to **Help → Eclipse Marketplace**.
  - Search for "JUnit 5" and install it.
- **Running Tests:**
  - Right-click on the test class or method → **Run As → JUnit Test**.
  - Use the **JUnit view** to see the results.

---

### **3. Visual Studio Code**
- **Installing Extensions:**
  - Install the following extensions:
    - **Java Extension Pack** by Microsoft.
    - **JUnit 5 Integration** (optional).
- **Running Tests:**
  - Navigate to the test file.
  - Click the **Run Test** icon next to the method or class.
  - Alternatively, open the terminal and use Maven or Gradle commands.

---

## **5. Creating Your First JUnit 5 Test**

Let's write a simple test to ensure everything is set up correctly.

### **Calculator Class:**

```java
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
}
```

### **JUnit Test Class:**

```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;

public class CalculatorTest {

    @Test
    void testAdd() {
        Calculator calculator = new Calculator();
        assertEquals(5, calculator.add(2, 3), "2 + 3 should equal 5");
    }
}
```

### **Running the Test:**
- In **IntelliJ** or **Eclipse**: Right-click the `CalculatorTest` class → **Run**.
- In **VS Code**: Click on the **Run Test** icon next to the method.
- Using **Maven**:
  ```bash
  mvn test
  ```
- Using **Gradle**:
  ```bash
  ./gradlew test
  ```

### **Expected Output:**
- If successful, you should see a green checkmark.
- If failed, you will see a red mark with the error details.

---

## **6. Common Mistakes to Avoid**
- **Incorrect Dependency Versions:** Ensure you're using compatible versions of JUnit and your build tool.
- **Misconfigured Classpaths:** Double-check that your IDE recognizes the JUnit dependencies.
- **Incorrect Annotations:** JUnit 5 uses `@Test` from `org.junit.jupiter.api.Test`, not from JUnit 4.

---

## **7. Expert Insights and Best Practices**
- **Consistency in Build Tools:** Use the same build tool (Maven or Gradle) throughout the project to avoid conflicts.
- **Organize Tests in Packages:** Follow the same package structure as your main code for easier maintenance.
- **CI Integration:** Configure continuous integration (CI) pipelines to automatically run tests on code commits (e.g., using GitLab CI or GitHub Actions).

---

## **8. Hands-On Exercise**
1. **Install JUnit:** Set up JUnit in your preferred environment using either Maven or Gradle.
2. **Create and Run the Test:**
   - Write the `Calculator` class and `CalculatorTest` as shown above.
   - Run the test and confirm that it passes.
3. **Experiment:**
   - Add another method in `Calculator` (e.g., `subtract`) and write a corresponding test.
   - Change the expected value in the test to see a failing test scenario.

---

## **Next Steps: Phase 1 - Lesson 3**
In the next lesson, we'll cover:
- **Anatomy of a JUnit Test:**
  - @Test annotation
  - Assertions (`assertEquals`, `assertTrue`, `assertFalse`, `assertNotNull`, etc.)
- **Creating and Running a Basic Test:**
  - Writing a test case for a simple method.
  - Running the test using IDE's test runner.
