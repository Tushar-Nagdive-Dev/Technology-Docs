---

# **Phase 2 - Lesson 7: Exception Testing and Timeout**

---

## **1. Exception Testing in JUnit 5**

In software testing, it's crucial to verify that methods **handle exceptional scenarios** correctly. In JUnit 5, this is achieved using:
- `assertThrows()`: Checks if an exception is thrown.
- `assertDoesNotThrow()`: Ensures that no exception is thrown.

---

## **2. Using assertThrows()**

### **1. What is assertThrows()?**
- Verifies that a specific exception is thrown.
- Captures the exception for further validation (e.g., checking the message).

### **Syntax:**
```java
Exception exception = assertThrows(ExceptionType.class, () -> {
    // Code that should throw the exception
});
```

### **Example: Testing Division by Zero**

Let's test the `divide()` method in the `Calculator` class to ensure it throws an `IllegalArgumentException` for division by zero.

#### **Calculator Class:**
```java
public class Calculator {
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
    void testDivideByZero() {
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            calculator.divide(10, 0);
        });
        assertEquals("Division by zero is not allowed.", exception.getMessage());
    }
}
```

### **Explanation:**
- `assertThrows(IllegalArgumentException.class, () -> {...})`: Checks if an `IllegalArgumentException` is thrown.
- `exception.getMessage()`: Verifies the exception message.

### **Expected Output:**
- The test passes if the exception is thrown with the expected message.

---

## **3. Using assertDoesNotThrow()**

### **1. What is assertDoesNotThrow()?**
- Verifies that a block of code does **not** throw any exception.

### **Example:**
```java
@Test
void testDivide_ValidInput() {
    assertDoesNotThrow(() -> {
        int result = calculator.divide(10, 2);
        assertEquals(5, result);
    });
}
```

### **Explanation:**
- Ensures that no exception is thrown for a valid division.
- Combines `assertDoesNotThrow()` with other assertions (`assertEquals`).

---

## **4. Exception Testing Best Practices**
- **Check Exception Type:** Always check the type of exception being thrown.
- **Verify Exception Message:** If possible, verify the exception message for more accurate testing.
- **Avoid Catching Exceptions:** Use `assertThrows()` instead of `try-catch` blocks for cleaner and more maintainable tests.

---

## **5. Timeout in JUnit 5**

### **1. Why Use Timeout?**
- Ensures that a test completes within a **specified time limit**.
- Detects performance bottlenecks or infinite loops.

### **2. Using @Timeout Annotation**
- Fails the test if the execution time exceeds the specified duration.
- Can be applied to both methods and classes.

### **Example:**
```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.Timeout;

import java.util.concurrent.TimeUnit;

public class TimeoutTest {

    @Test
    @Timeout(value = 500, unit = TimeUnit.MILLISECONDS)
    void testQuickOperation() {
        int sum = 0;
        for (int i = 0; i < 1000; i++) {
            sum += i;
        }
        assertEquals(499500, sum);
    }

    @Test
    @Timeout(value = 1, unit = TimeUnit.SECONDS)
    void testLongRunningOperation() {
        // Simulate long running operation
        try {
            Thread.sleep(800);  // Sleep for 800 ms
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        assertTrue(true);
    }
}
```

### **Explanation:**
- `@Timeout(value = 500, unit = TimeUnit.MILLISECONDS)`: Sets a maximum execution time of 500 milliseconds.
- The test fails if execution exceeds the specified time.

### **Expected Output:**
- Both tests should pass since the operations complete within the time limits.

---

## **6. Combining Exception Testing and Timeout**

You can combine exception testing and timeout to ensure a method:
1. **Throws an exception** for invalid input.
2. **Completes within a time limit** for valid input.

### **Example:**
```java
@Test
@Timeout(value = 1, unit = TimeUnit.SECONDS)
void testDivideWithTimeout() {
    IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
        calculator.divide(10, 0);
    });
    assertEquals("Division by zero is not allowed.", exception.getMessage());
}
```

### **Explanation:**
- Combines `assertThrows()` and `@Timeout`.
- Ensures the test completes within **1 second**.

---

## **7. Common Mistakes to Avoid**
- **Wrong Exception Type:** Make sure to specify the correct exception class.
- **Incorrect Timeout Unit:** Double-check the `TimeUnit` to avoid confusion (e.g., `SECONDS` vs. `MILLISECONDS`).
- **Hardcoding Sleep:** Avoid hardcoding `Thread.sleep()` in tests. Use mocks for time-dependent methods.

---

## **8. Expert Insights and Best Practices**
- **Specific Exception Testing:** Always test for specific exceptions rather than catching `Exception`.
- **Reasonable Timeout Values:** Set realistic timeouts based on the expected performance of the method.
- **Fail Fast Principle:** Combine exception testing and timeout to catch issues early.

---

## **9. Hands-On Exercise**
1. **Test Edge Cases:**
   - Test for null inputs and negative numbers in the `divide()` method.
2. **Combine Exception and Timeout:**
   - Write a test that checks for exceptions with a timeout.
3. **Test Performance:**
   - Test the performance of a sorting algorithm with `@Timeout`.

---

## **Next Steps: Phase 2 - Lesson 8**
In the next lesson, we'll cover:
- **Advanced Assertions and Assumptions**
  - Using `assertAll()` for grouped assertions.
  - Using `Assumptions` for conditional test execution.
- **Practical Scenarios:**
  - Grouping multiple assertions for better test readability.
  - Conditional tests for platform-specific scenarios.
