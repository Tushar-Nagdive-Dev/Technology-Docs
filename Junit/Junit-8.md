---

# **Phase 2 - Lesson 8: Advanced Assertions and Assumptions**

---

## **1. Advanced Assertions in JUnit 5**

In addition to basic assertions, JUnit 5 provides **advanced assertions** for:
- Grouping multiple assertions together.
- Checking complex conditions.
- Writing more **readable and maintainable tests**.

---

## **2. Using assertAll()**

### **1. What is assertAll()?**
- **Groups multiple assertions** together.
- Executes all assertions even if some fail.
- Reports all failures instead of stopping at the first failure.

### **Syntax:**
```java
assertAll("Group description",
    () -> assertEquals(expected1, actual1, "Message 1"),
    () -> assertTrue(condition, "Message 2"),
    () -> assertNotNull(object, "Message 3")
);
```

### **Example: Testing Multiple Calculations**

Let's enhance our `CalculatorTest` class to group multiple assertions.

#### **Calculator Class:**
```java
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }

    public int subtract(int a, int b) {
        return a - b;
    }

    public int multiply(int a, int b) {
        return a * b;
    }

    public int divide(int a, int b) {
        if (b == 0) {
            throw new IllegalArgumentException("Division by zero is not allowed.");
        }
        return a / b;
    }
}
```

#### **JUnit Test Class:**
```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;

public class CalculatorTest {

    private final Calculator calculator = new Calculator();

    @Test
    void testAllOperations() {
        assertAll("Arithmetic Operations",
            () -> assertEquals(5, calculator.add(2, 3), "2 + 3 should equal 5"),
            () -> assertEquals(1, calculator.subtract(5, 4), "5 - 4 should equal 1"),
            () -> assertEquals(6, calculator.multiply(2, 3), "2 * 3 should equal 6"),
            () -> assertEquals(2, calculator.divide(10, 5), "10 / 5 should equal 2")
        );
    }
}
```

### **Explanation:**
- `assertAll("Arithmetic Operations", ...)`: Groups all arithmetic operations under a single test.
- If one assertion fails, others continue to execute.
- **Output:** If a failure occurs, the report shows all failed assertions.

### **Expected Output:**
- If all assertions pass, the test is marked as successful.
- If any assertion fails, the report displays all failed assertions with detailed messages.

---

## **3. Using assertIterableEquals()**

### **1. What is assertIterableEquals()?**
- Compares two `Iterable` objects (e.g., `List`, `Set`) for equality.
- Useful for verifying collections.

### **Example: Checking a List of Values**
```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;
import java.util.List;

public class ListTest {

    @Test
    void testListEquality() {
        List<String> expected = List.of("apple", "banana", "cherry");
        List<String> actual = List.of("apple", "banana", "cherry");

        assertIterableEquals(expected, actual, "Lists should be equal");
    }
}
```

### **Explanation:**
- Compares each element in the `expected` and `actual` lists.
- The test passes if all elements are equal and in the same order.

---

## **4. Using assertLinesMatch()**

### **1. What is assertLinesMatch()?**
- Compares two lists of strings line by line.
- Supports **regular expressions** for flexible matching.

### **Example:**
```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;
import java.util.List;

public class LineMatchTest {

    @Test
    void testLineMatch() {
        List<String> expected = List.of("Hello World", "Welcome to JUnit");
        List<String> actual = List.of("Hello World", "Welcome to JUnit");

        assertLinesMatch(expected, actual, "Lines should match exactly");
    }

    @Test
    void testLineMatchWithRegex() {
        List<String> expected = List.of("Hello World", "Welcome to JUnit", "User: [A-Za-z]+");
        List<String> actual = List.of("Hello World", "Welcome to JUnit", "User: John");

        assertLinesMatch(expected, actual, "Lines should match with regex");
    }
}
```

### **Explanation:**
- `assertLinesMatch()` checks line by line.
- Supports **regular expressions** for flexible matching.

### **Expected Output:**
- Both tests should pass, demonstrating exact match and regex matching.

---

## **5. Using Assumptions in JUnit 5**

### **1. What are Assumptions?**
- Assumptions allow tests to run **conditionally**.
- If the assumption is `false`, the test is **skipped**.
- Useful for environment-specific tests (e.g., OS-specific tests, configuration-based tests).

### **2. Available Assumptions:**
- `assumeTrue(condition)`: Skips the test if the condition is `false`.
- `assumeFalse(condition)`: Skips the test if the condition is `true`.
- `assumingThat(condition, executable)`: Executes a block of code conditionally.

---

### **3. Example: Using assumeTrue() and assumeFalse()**
```java
import static org.junit.jupiter.api.Assertions.*;
import static org.junit.jupiter.api.Assumptions.*;
import org.junit.jupiter.api.Test;

public class AssumptionTest {

    @Test
    void testOnlyOnWindows() {
        assumeTrue(System.getProperty("os.name").startsWith("Windows"), 
                   "Test skipped: Not running on Windows");
        assertEquals(5, 2 + 3);
    }

    @Test
    void testOnlyOnLinux() {
        assumeTrue(System.getProperty("os.name").startsWith("Linux"), 
                   "Test skipped: Not running on Linux");
        assertEquals(5, 2 + 3);
    }
}
```

### **Explanation:**
- `assumeTrue()` checks if the operating system is Windows or Linux.
- If the assumption is `false`, the test is **skipped**.
- Useful for platform-specific logic.

### **Expected Output:**
- If running on Windows, `testOnlyOnWindows` passes and `testOnlyOnLinux` is skipped.
- If running on Linux, `testOnlyOnLinux` passes and `testOnlyOnWindows` is skipped.

---

### **4. Example: Using assumingThat()**
```java
@Test
void testConditionally() {
    String environment = "DEV";
    assumingThat("DEV".equals(environment), () -> {
        assertEquals(5, 2 + 3);
    });

    // This assertion always runs
    assertTrue(true);
}
```

### **Explanation:**
- `assumingThat()` conditionally executes the block of code.
- Other assertions outside the block **always** run.

---

## **6. Common Mistakes to Avoid**
- **Incorrect Grouping with assertAll:** Ensure all assertions are logically grouped.
- **Misusing Assumptions:** Assumptions should be used for environmental or configuration checks, not for business logic.
- **Order Sensitivity in assertIterableEquals:** Order matters. If order is not guaranteed, use `containsAll()`.

---

## **7. Expert Insights and Best Practices**
- **Group Assertions Logically:** Group assertions by functionality using `assertAll()` for better readability.
- **Use Assumptions for Conditional Tests:** Apply assumptions to skip tests that are not applicable in certain environments.
- **Readable Messages:** Always provide meaningful messages for better debugging.

---

## **8. Hands-On Exercise**
1. **Create Additional Grouped Assertions:**
   - Group assertions for subtraction and multiplication.
2. **Experiment with Assumptions:**
   - Write a test that runs only on a specific Java version.
3. **Test Complex Collections:**
   - Use `assertIterableEquals` for complex collection comparisons.

---

## **Next Steps: Phase 3 - Lesson 9**
In the next lesson, we'll cover:
- **Mocking with Mockito**
  - Introduction to mocking and why it's needed.
  - Using `@Mock`, `@InjectMocks`, and `@Spy`.
  - Stubbing and verifying method interactions.
