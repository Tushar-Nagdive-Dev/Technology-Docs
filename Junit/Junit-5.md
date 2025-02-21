---

# **Phase 1 - Lesson 5: Test Suites and Test Order**

---

## **1. Introduction to Test Suites**

A **Test Suite** is a collection of related test classes grouped together to run as a single unit. It allows you to:
- Organize tests logically.
- Execute multiple test classes together.
- Group tests by functionality, module, or type (e.g., unit tests, integration tests).

---

## **2. Why Use Test Suites?**
- **Better Organization:** Grouping related tests makes the project structure cleaner.
- **Easier Maintenance:** Changes to one feature require running only its related test suite.
- **Selective Execution:** Run only relevant test suites for faster feedback during development.

---

## **3. Creating Test Suites in JUnit 5**

JUnit 5 provides the `@Suite` annotation to group multiple test classes.

### **Example Structure:**
Assume you have the following test classes:
1. `CalculatorTest`: Tests basic arithmetic operations.
2. `AdvancedMathTest`: Tests advanced math functions.
3. `StringUtilsTest`: Tests utility methods for strings.

### **Step 1: Add Required Dependencies**
Ensure the following dependencies are present in your **Maven `pom.xml`**:
```xml
<dependency>
    <groupId>org.junit.platform</groupId>
    <artifactId>junit-platform-suite-engine</artifactId>
    <version>1.9.3</version>
    <scope>test</scope>
</dependency>
```

Or in **Gradle `build.gradle`**:
```groovy
testImplementation 'org.junit.platform:junit-platform-suite-engine:1.9.3'
```

---

### **Step 2: Create Test Classes**

#### **CalculatorTest**
```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;

public class CalculatorTest {

    @Test
    void testAdd() {
        Calculator calculator = new Calculator();
        assertEquals(5, calculator.add(2, 3));
    }

    @Test
    void testSubtract() {
        Calculator calculator = new Calculator();
        assertEquals(1, calculator.subtract(5, 4));
    }
}
```

---

#### **AdvancedMathTest**
```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;

public class AdvancedMathTest {

    @Test
    void testSquare() {
        int input = 4;
        int expected = 16;
        assertEquals(expected, input * input);
    }

    @Test
    void testSquareRoot() {
        double input = 9;
        double expected = 3;
        assertEquals(expected, Math.sqrt(input));
    }
}
```

---

#### **StringUtilsTest**
```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;

public class StringUtilsTest {

    @Test
    void testIsEmpty() {
        String str = "";
        assertTrue(str.isEmpty());
    }

    @Test
    void testContains() {
        String str = "JUnit 5 is awesome";
        assertTrue(str.contains("awesome"));
    }
}
```

---

### **Step 3: Create a Test Suite**

You can create a test suite to group and run the above test classes together.

```java
import org.junit.platform.suite.api.SelectClasses;
import org.junit.platform.suite.api.Suite;

@Suite
@SelectClasses({
    CalculatorTest.class,
    AdvancedMathTest.class,
    StringUtilsTest.class
})
public class AllTestsSuite {
}
```

### **Explanation:**
- `@Suite`: Marks this class as a test suite.
- `@SelectClasses`: Specifies the classes to include in this suite.
- When you run `AllTestsSuite`, all tests from the listed classes will be executed together.

---

### **Step 4: Running the Test Suite**
- In **IntelliJ** or **Eclipse**: Right-click the `AllTestsSuite` class → **Run**.
- Using **Maven**:
```bash
mvn test
```
- Using **Gradle**:
```bash
./gradlew test
```

---

## **4. Controlling Test Execution Order**

By default, JUnit 5 executes tests in an **undefined order**. However, you can control the order using the following annotations:

### **1. @TestMethodOrder**
- Controls the execution order of test methods within a class.

### **Types of Ordering:**
1. **Order by Method Name:** Alphabetically by method name.
2. **Order by Display Name:** Alphabetically by display name.
3. **Order by Order Annotation:** Custom order using `@Order`.

### **Example: Order by @Order Annotation**
```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Order;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.TestMethodOrder;
import org.junit.jupiter.api.MethodOrderer.OrderAnnotation;

@TestMethodOrder(OrderAnnotation.class)
public class OrderedTest {

    @Test
    @Order(2)
    void secondTest() {
        System.out.println("Second test executed.");
        assertTrue(true);
    }

    @Test
    @Order(1)
    void firstTest() {
        System.out.println("First test executed.");
        assertTrue(true);
    }

    @Test
    @Order(3)
    void thirdTest() {
        System.out.println("Third test executed.");
        assertTrue(true);
    }
}
```

### **Explanation:**
- `@TestMethodOrder(OrderAnnotation.class)`: Specifies that the order is defined by `@Order`.
- `@Order(n)`: Specifies the execution order. Lower numbers are executed first.
- When you run this test class, the output will be:
  ```
  First test executed.
  Second test executed.
  Third test executed.
  ```

---

## **5. Common Mistakes to Avoid**
- **Misconfigured Test Suites:** Ensure all classes included in the suite are test classes.
- **Circular Dependencies:** Avoid circular dependencies among test classes.
- **Inconsistent Test Order:** Avoid relying on test order unless explicitly defined using `@Order`.

---

## **6. Expert Insights and Best Practices**
- **Group Tests Logically:** Group tests by functionality or module for better organization.
- **Consistent Ordering:** Use `@Order` sparingly; tests should ideally be independent of execution order.
- **Use Test Suites in CI/CD:** Test suites are ideal for continuous integration pipelines.

---

## **7. Hands-On Exercise**
1. **Create Additional Test Suites:**
   - Group `CalculatorTest` and `AdvancedMathTest` into a `MathTestsSuite`.
   - Group `StringUtilsTest` into a separate `StringTestsSuite`.
2. **Experiment with Ordering:**
   - Change the order of execution in `OrderedTest` and observe the output.
3. **Nested Suites:**
   - Create a master suite that runs all other suites.

---

## **Next Steps: Phase 2 - Lesson 6**
We are now moving to the next phase (Intermediate) where we'll cover:
- **Parameterized Tests:**
  - Using `@ParameterizedTest` for data-driven testing.
  - Different data sources (`@ValueSource`, `@CsvSource`, `@MethodSource`).
- **Dynamic Tests:**
  - Creating tests at runtime using `@TestFactory`.
