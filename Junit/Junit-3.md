---

# **Phase 1 - Lesson 3: Anatomy of a JUnit Test**

---

## **1. What is a JUnit Test?**

A **JUnit Test** is a method that checks whether a unit of code (e.g., a method or class) behaves as expected. It consists of:
- **Test Method:** Contains the logic for the test.
- **Assertions:** Validates the output against the expected result.
- **Annotations:** Provide metadata to control the test's behavior.

---

## **2. Anatomy of a JUnit Test**

A typical JUnit test consists of the following parts:

### **1. Test Class**
- The class that contains one or more test methods.
- Usually named with a `Test` suffix (e.g., `CalculatorTest`).

### **2. Test Method**
- A method that tests a specific behavior or functionality.
- Annotated with `@Test`.

### **3. Assertions**
- Used to verify the result of the test.
- Common assertions:
  - `assertEquals(expected, actual)`: Checks equality.
  - `assertTrue(condition)`: Checks if the condition is true.
  - `assertFalse(condition)`: Checks if the condition is false.
  - `assertNotNull(object)`: Checks if the object is not null.
  - `assertNull(object)`: Checks if the object is null.
  - `assertThrows(exception, executable)`: Checks if an exception is thrown.

### **4. Annotations**
- `@Test`: Marks a method as a test method.
- `@BeforeEach`: Runs before each test method.
- `@AfterEach`: Runs after each test method.
- `@BeforeAll`: Runs once before all test methods (static).
- `@AfterAll`: Runs once after all test methods (static).

---

## **3. Example: Basic JUnit Test**

Let's create a test for a simple `Calculator` class.

### **Calculator Class:**
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

---

### **JUnit Test Class:**
```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

public class CalculatorTest {

    private Calculator calculator;

    @BeforeEach
    void setUp() {
        calculator = new Calculator();
    }

    @Test
    void testAdd() {
        assertEquals(5, calculator.add(2, 3), "2 + 3 should equal 5");
        assertEquals(-1, calculator.add(-2, 1), "-2 + 1 should equal -1");
    }

    @Test
    void testSubtract() {
        assertEquals(2, calculator.subtract(5, 3), "5 - 3 should equal 2");
        assertEquals(-5, calculator.subtract(0, 5), "0 - 5 should equal -5");
    }

    @Test
    void testMultiply() {
        assertEquals(15, calculator.multiply(3, 5), "3 * 5 should equal 15");
        assertEquals(0, calculator.multiply(0, 10), "0 * 10 should equal 0");
    }

    @Test
    void testDivide() {
        assertEquals(2, calculator.divide(10, 5), "10 / 5 should equal 2");
    }

    @Test
    void testDivideByZero() {
        Exception exception = assertThrows(IllegalArgumentException.class, () -> {
            calculator.divide(10, 0);
        });
        assertEquals("Division by zero is not allowed.", exception.getMessage());
    }
}
```

---

## **4. Explanation of Test Class**

### **1. setUp() Method with @BeforeEach**
- `@BeforeEach`: Initializes the `Calculator` object before each test method runs.
- Ensures a fresh state for each test.

### **2. Test Methods with @Test**
- `@Test`: Marks methods as test cases.
- Test methods are usually named to reflect the scenario they are testing (e.g., `testAdd`, `testSubtract`).

### **3. Using Assertions**
- `assertEquals(expected, actual, message)`: Checks if the expected result matches the actual output.
- `assertThrows`: Checks if an exception is thrown.

### **4. Exception Testing**
- In `testDivideByZero()`, we check if the `IllegalArgumentException` is thrown for division by zero.

---

## **5. Running the Test**

### **Using IntelliJ IDEA:**
- Right-click on the `CalculatorTest` class → **Run**.
- Or use the shortcut **Shift + F10**.

### **Using Eclipse:**
- Right-click on the `CalculatorTest` class → **Run As → JUnit Test**.

### **Using VS Code:**
- Click on the **Run Test** icon next to the method.

### **Using Maven:**
```bash
mvn test
```

### **Using Gradle:**
```bash
./gradlew test
```

---

## **6. Expected Output**

- If all tests pass, you should see **green checkmarks**.
- If any test fails, a **red mark** will appear with detailed error messages.

---

## **7. Common Mistakes to Avoid**
- **Incorrect Assertions:** Double-check expected vs. actual values.
- **Missing Exception Handling:** Always check for exception scenarios.
- **Improper Test Isolation:** Use `@BeforeEach` to isolate tests.

---

## **8. Expert Insights and Best Practices**
- **Keep Test Methods Short and Focused:** Each test method should test one scenario.
- **Use Descriptive Messages:** Use meaningful messages in assertions to make failures easier to understand.
- **Organize Tests by Functionality:** Group related tests into test classes.

---

## **9. Hands-On Exercise**
1. **Write Additional Tests:**
   - Add a method in `Calculator` for modulus (`%`) operation.
   - Write a test method to check the modulus operation.
2. **Experiment with Assertions:**
   - Use `assertTrue()` and `assertFalse()` in one of the tests.
3. **Test Edge Cases:**
   - Test negative numbers, large numbers, and zero inputs.

---

## **Next Steps: Phase 1 - Lesson 4**
In the next lesson, we'll cover:
- **Basic Assertions and Test Annotations:**
  - Understanding and using JUnit assertions effectively.
  - Detailed explanation of annotations: `@BeforeEach`, `@AfterEach`, `@BeforeAll`, `@AfterAll`.
- **Creating More Complex Test Cases:**
  - Writing tests for edge cases and negative scenarios.
  - Practicing with real-world examples.
