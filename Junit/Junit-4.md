---

# **Phase 1 - Lesson 4: Basic Assertions and Test Annotations**

---

## **1. Introduction to Assertions**

Assertions in JUnit are used to **verify the expected outcome** of a test. They compare the expected result with the actual result and **fail the test** if they don't match.

JUnit 5 provides several powerful assertions, grouped under:
- `Assertions` class for standard assertions.
- `Assumptions` class for conditional checks.

---

## **2. Common Assertions in JUnit 5**

### **1. assertEquals()**
Checks if two values are equal.

```java
assertEquals(expected, actual);
assertEquals(expected, actual, message);
```

### **Example:**
```java
int expected = 5;
int actual = calculator.add(2, 3);
assertEquals(expected, actual, "2 + 3 should equal 5");
```

---

### **2. assertNotEquals()**
Checks if two values are not equal.

```java
assertNotEquals(unexpected, actual);
```

### **Example:**
```java
assertNotEquals(10, calculator.add(2, 3));
```

---

### **3. assertTrue() and assertFalse()**
Checks if a condition is `true` or `false`.

```java
assertTrue(condition);
assertFalse(condition);
```

### **Example:**
```java
assertTrue(calculator.add(2, 3) > 0, "Result should be positive");
assertFalse(calculator.subtract(2, 3) > 0, "Result should not be positive");
```

---

### **4. assertNull() and assertNotNull()**
Checks if an object is `null` or `not null`.

```java
assertNull(object);
assertNotNull(object);
```

### **Example:**
```java
Object obj = null;
assertNull(obj, "Object should be null");

Object obj2 = new Object();
assertNotNull(obj2, "Object should not be null");
```

---

### **5. assertArrayEquals()**
Checks if two arrays are equal.

```java
assertArrayEquals(expectedArray, actualArray);
```

### **Example:**
```java
int[] expected = {1, 2, 3};
int[] actual = {1, 2, 3};
assertArrayEquals(expected, actual, "Arrays should be equal");
```

---

### **6. assertThrows()**
Checks if a specific exception is thrown.

```java
Exception exception = assertThrows(ExceptionType.class, () -> {
    // Code that should throw the exception
});
assertEquals("Expected Message", exception.getMessage());
```

### **Example:**
```java
Exception exception = assertThrows(IllegalArgumentException.class, () -> {
    calculator.divide(10, 0);
});
assertEquals("Division by zero is not allowed.", exception.getMessage());
```

---

### **7. assertAll()**
Groups multiple assertions together. If any assertion fails, all errors are reported together.

```java
assertAll("Math operations",
    () -> assertEquals(5, calculator.add(2, 3)),
    () -> assertEquals(2, calculator.subtract(5, 3)),
    () -> assertEquals(6, calculator.multiply(2, 3)),
    () -> assertEquals(2, calculator.divide(10, 5))
);
```

---

## **3. Test Annotations in JUnit 5**

### **1. @Test**
- Marks a method as a test case.
- Methods annotated with `@Test` are executed as test methods.

```java
@Test
void testAddition() {
    assertEquals(5, calculator.add(2, 3));
}
```

---

### **2. @BeforeEach**
- Runs before each test method.
- Used to set up common test data or state.

```java
@BeforeEach
void setUp() {
    calculator = new Calculator();
}
```

---

### **3. @AfterEach**
- Runs after each test method.
- Used for cleanup tasks.

```java
@AfterEach
void tearDown() {
    calculator = null;
}
```

---

### **4. @BeforeAll**
- Runs once before all test methods in the class.
- Must be `static`.

```java
@BeforeAll
static void initAll() {
    System.out.println("Starting all tests...");
}
```

---

### **5. @AfterAll**
- Runs once after all test methods in the class.
- Must be `static`.

```java
@AfterAll
static void tearDownAll() {
    System.out.println("All tests completed.");
}
```

---

### **6. @Disabled**
- Temporarily disables a test method or class.

```java
@Disabled("Not implemented yet")
@Test
void testFeature() {
    fail("This test is disabled");
}
```

---

## **4. Example: Comprehensive JUnit Test Class**

Let's combine all these assertions and annotations in a more detailed `CalculatorTest` class.

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

### **Comprehensive CalculatorTest Class:**
```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.AfterAll;
import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.Test;

public class CalculatorTest {

    private Calculator calculator;

    @BeforeAll
    static void initAll() {
        System.out.println("Starting all tests...");
    }

    @BeforeEach
    void setUp() {
        calculator = new Calculator();
    }

    @Test
    void testAdd() {
        assertEquals(5, calculator.add(2, 3), "2 + 3 should equal 5");
        assertNotEquals(10, calculator.add(2, 3));
        assertTrue(calculator.add(2, 3) > 0);
        assertFalse(calculator.add(2, -3) > 0);
    }

    @Test
    void testMultiply() {
        assertEquals(15, calculator.multiply(3, 5), "3 * 5 should equal 15");
        assertNotNull(calculator, "Calculator object should not be null");
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

    @Test
    @Disabled("Not implemented yet")
    void testFeatureNotImplemented() {
        fail("This test is disabled");
    }

    @AfterEach
    void tearDown() {
        System.out.println("Test completed.");
    }

    @AfterAll
    static void tearDownAll() {
        System.out.println("All tests completed.");
    }
}
```

---

## **5. Running the Test**

- In **IntelliJ** or **Eclipse**: Right-click the `CalculatorTest` class → **Run**.
- Using **Maven**:
```bash
mvn test
```
- Using **Gradle**:
```bash
./gradlew test
```

---

## **6. Expected Output**

- All tests should pass, except the one marked with `@Disabled`.
- You should see output from `@BeforeAll`, `@BeforeEach`, `@AfterEach`, and `@AfterAll`.

---

## **7. Hands-On Exercise**
1. **Add More Tests:**
   - Implement and test the `modulus` operation in `Calculator`.
2. **Experiment with Assertions:**
   - Try using `assertArrayEquals` and `assertAll`.
3. **Negative Scenarios:**
   - Test with negative numbers and zero inputs.

---

## **Next Steps: Phase 1 - Lesson 5**
In the next lesson, we'll cover:
- **Test Suites and Test Order**
- **Grouping Tests with Test Suites**
- **Controlling Test Execution Order**
