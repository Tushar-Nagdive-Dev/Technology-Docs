---

# **Phase 2 - Lesson 6: Parameterized Tests**

---

## **1. What are Parameterized Tests?**

**Parameterized Tests** in JUnit allow you to **run the same test multiple times** with different inputs. This approach:
- **Reduces Code Duplication:** Avoids repeating the same test logic.
- **Increases Test Coverage:** Covers a wider range of inputs and edge cases.
- **Enhances Maintainability:** Easier to maintain and update as only test data changes.

---

## **2. Why Use Parameterized Tests?**

- **Data-Driven Testing:** Ideal for scenarios where the logic is the same but the input values vary.
- **Edge Case Verification:** Efficiently tests edge cases with multiple inputs.
- **Consistent Behavior Verification:** Ensures consistent behavior across a range of inputs.

---

## **3. JUnit 5 Parameterized Tests**

JUnit 5 provides the `@ParameterizedTest` annotation to support parameterized tests. You need to specify the data source using one of the following:
- `@ValueSource`
- `@CsvSource`
- `@CsvFileSource`
- `@MethodSource`
- `@EnumSource`

---

## **4. Using @ValueSource**

### **1. What is @ValueSource?**
- Used to provide a single array of literal values as input.
- Supports primitive types, `String`, and `Class`.

### **Example: Testing Palindrome Method**
```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;

public class StringUtilsTest {

    public static boolean isPalindrome(String str) {
        if (str == null) return false;
        String reversed = new StringBuilder(str).reverse().toString();
        return str.equalsIgnoreCase(reversed);
    }

    @ParameterizedTest
    @ValueSource(strings = {"radar", "madam", "racecar", "level", "noon"})
    void testIsPalindrome_TrueCases(String input) {
        assertTrue(StringUtilsTest.isPalindrome(input));
    }

    @ParameterizedTest
    @ValueSource(strings = {"hello", "world", "java", "JUnit"})
    void testIsPalindrome_FalseCases(String input) {
        assertFalse(StringUtilsTest.isPalindrome(input));
    }
}
```

### **Explanation:**
- `@ParameterizedTest`: Marks the method as a parameterized test.
- `@ValueSource`: Provides an array of strings as input to the test method.
- The test runs once for each value in `@ValueSource`.

### **Expected Output:**
- `testIsPalindrome_TrueCases` will pass for all palindromes.
- `testIsPalindrome_FalseCases` will pass for non-palindromes.

---

## **5. Using @CsvSource**

### **1. What is @CsvSource?**
- Provides multiple sets of arguments as comma-separated values.
- Ideal for testing multiple inputs and expected outputs together.

### **Example: Testing Addition Method**
```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvSource;

public class CalculatorTest {

    public static int add(int a, int b) {
        return a + b;
    }

    @ParameterizedTest
    @CsvSource({
        "1, 2, 3",
        "2, 3, 5",
        "-1, -2, -3",
        "-2, 3, 1"
    })
    void testAdd(int a, int b, int expected) {
        assertEquals(expected, CalculatorTest.add(a, b));
    }
}
```

### **Explanation:**
- `@CsvSource`: Provides multiple sets of input-output values.
- Each line is a separate test case with comma-separated arguments.
- The method parameters are automatically mapped to the CSV values.

### **Expected Output:**
- The test method runs once for each set of values in `@CsvSource`.

---

## **6. Using @CsvFileSource**

### **1. What is @CsvFileSource?**
- Loads test data from a CSV file.
- Useful for large datasets and externalized test data.

### **CSV File: `src/test/resources/test-data.csv`**
```
1, 2, 3
2, 3, 5
-1, -2, -3
-2, 3, 1
```

### **Example:**
```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvFileSource;

public class CalculatorTest {

    @ParameterizedTest
    @CsvFileSource(resources = "/test-data.csv", numLinesToSkip = 0)
    void testAddFromFile(int a, int b, int expected) {
        assertEquals(expected, CalculatorTest.add(a, b));
    }
}
```

### **Explanation:**
- `@CsvFileSource`: Loads data from the specified CSV file.
- `resources`: Path to the CSV file relative to `src/test/resources`.
- `numLinesToSkip`: Skips the first line (useful if there's a header row).

---

## **7. Using @MethodSource**

### **1. What is @MethodSource?**
- Provides test data from a static method in the test class.
- Useful for dynamic data generation or complex data structures.

### **Example:**
```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.MethodSource;
import java.util.stream.Stream;

public class CalculatorTest {

    static Stream<org.junit.jupiter.params.provider.Arguments> provideNumbersForAddition() {
        return Stream.of(
            org.junit.jupiter.params.provider.Arguments.of(1, 2, 3),
            org.junit.jupiter.params.provider.Arguments.of(2, 3, 5),
            org.junit.jupiter.params.provider.Arguments.of(-1, -2, -3),
            org.junit.jupiter.params.provider.Arguments.of(-2, 3, 1)
        );
    }

    @ParameterizedTest
    @MethodSource("provideNumbersForAddition")
    void testAddWithMethodSource(int a, int b, int expected) {
        assertEquals(expected, CalculatorTest.add(a, b));
    }
}
```

### **Explanation:**
- `@MethodSource`: Refers to a static method that returns a `Stream<Arguments>`.
- The test method parameters are automatically mapped to the arguments.

---

## **8. Common Mistakes to Avoid**
- **Mismatched Parameters:** Ensure the number of parameters matches the values provided.
- **Incorrect Data Type:** Use compatible data types for the input values.
- **Wrong CSV Format:** Check for correct CSV syntax (no trailing commas).

---

## **9. Expert Insights and Best Practices**
- **Keep Test Logic Consistent:** Only data should vary; test logic remains the same.
- **Externalize Test Data:** Use `@CsvFileSource` for large or frequently changing datasets.
- **Organize Complex Data:** Use `@MethodSource` for dynamic or complex data structures.

---

## **10. Hands-On Exercise**
1. **Create Additional Parameterized Tests:**
   - Implement parameterized tests for subtraction and multiplication.
2. **Experiment with Different Data Sources:**
   - Use `@CsvFileSource` with a custom CSV file for subtraction tests.
3. **Test Edge Cases:**
   - Add edge cases like negative numbers and zero inputs.

---

## **Next Steps: Phase 2 - Lesson 7**
In the next lesson, we'll cover:
- **Exception Testing and Timeout**
  - Using `assertThrows` for exception testing.
  - Using `@Timeout` to enforce maximum execution time.
- **Advanced Assertions and Assumptions**
  - Using `assertAll` for grouped assertions.
  - Using `Assumptions` for conditional test execution.
