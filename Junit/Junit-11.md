---

# **Phase 3 - Lesson 11: Test-Driven Development (TDD)**

---

## **1. What is Test-Driven Development (TDD)?**

**Test-Driven Development (TDD)** is a software development process where:
- Tests are **written before the implementation**.
- Development is guided by the tests, ensuring the code meets the requirements.
- It follows the **Red-Green-Refactor** cycle:
  1. **Red:** Write a failing test.
  2. **Green:** Implement just enough code to make the test pass.
  3. **Refactor:** Improve the code without changing its behavior.

---

## **2. Why Use TDD?**

- **Ensures Code Quality:** Forces developers to write only the code necessary to pass the tests.
- **Encourages Simplicity:** Leads to simpler, more maintainable code.
- **Improves Confidence:** Developers can make changes with confidence, knowing that tests will catch regressions.
- **Enhances Documentation:** Tests act as living documentation of the code's behavior.

---

## **3. TDD Red-Green-Refactor Cycle**

### **1. Red Phase**
- Write a test for a new feature or functionality.
- The test should **fail** because the implementation doesn't exist yet.
- This ensures that the test is correctly testing the new behavior.

### **2. Green Phase**
- Implement the **simplest solution** to make the test pass.
- Write just enough code to satisfy the test.

### **3. Refactor Phase**
- **Clean up the code** while ensuring all tests still pass.
- Improve code readability, maintainability, and performance.
- **No new functionality** should be added during refactoring.

---

## **4. TDD Example: Implementing a FizzBuzz Service**

### **1. Requirement: FizzBuzz Service**
- For numbers that are **multiples of 3**, return **"Fizz"**.
- For numbers that are **multiples of 5**, return **"Buzz"**.
- For numbers that are **multiples of both 3 and 5**, return **"FizzBuzz"**.
- For all other numbers, return the **number as a string**.

### **2. Step 1: Red Phase - Write Failing Tests**

Let's start by writing tests **before** implementing the `FizzBuzzService`.

---

### **FizzBuzzServiceTest Class**

```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;

public class FizzBuzzServiceTest {

    @Test
    void testFizz() {
        FizzBuzzService fizzBuzzService = new FizzBuzzService();
        assertEquals("Fizz", fizzBuzzService.getFizzBuzz(3));
        assertEquals("Fizz", fizzBuzzService.getFizzBuzz(6));
    }

    @Test
    void testBuzz() {
        FizzBuzzService fizzBuzzService = new FizzBuzzService();
        assertEquals("Buzz", fizzBuzzService.getFizzBuzz(5));
        assertEquals("Buzz", fizzBuzzService.getFizzBuzz(10));
    }

    @Test
    void testFizzBuzz() {
        FizzBuzzService fizzBuzzService = new FizzBuzzService();
        assertEquals("FizzBuzz", fizzBuzzService.getFizzBuzz(15));
        assertEquals("FizzBuzz", fizzBuzzService.getFizzBuzz(30));
    }

    @Test
    void testNumber() {
        FizzBuzzService fizzBuzzService = new FizzBuzzService();
        assertEquals("1", fizzBuzzService.getFizzBuzz(1));
        assertEquals("2", fizzBuzzService.getFizzBuzz(2));
    }
}
```

### **Explanation:**
- **Red Phase:** Since `FizzBuzzService` is not implemented, all tests will **fail**.
- We wrote tests for all scenarios:
  - Multiples of 3 → **"Fizz"**
  - Multiples of 5 → **"Buzz"**
  - Multiples of both 3 and 5 → **"FizzBuzz"**
  - All other numbers → **Number as String**

### **Expected Output:**
- All tests **fail** because the `FizzBuzzService` class doesn't exist yet.

---

### **3. Step 2: Green Phase - Implement the Minimal Solution**

Let's create the `FizzBuzzService` and implement just enough code to make the tests pass.

---

### **FizzBuzzService Class**

```java
public class FizzBuzzService {

    public String getFizzBuzz(int number) {
        if (number % 3 == 0 && number % 5 == 0) {
            return "FizzBuzz";
        }
        if (number % 3 == 0) {
            return "Fizz";
        }
        if (number % 5 == 0) {
            return "Buzz";
        }
        return String.valueOf(number);
    }
}
```

### **Explanation:**
- We implemented the simplest solution to pass all tests.
- We did **not** over-engineer or add any extra features.
- **Green Phase:** All tests should now pass.

### **Running the Test:**
- In **IntelliJ** or **Eclipse**: Right-click the `FizzBuzzServiceTest` class → **Run**.
- Using **Maven**:
```bash
mvn test
```
- Using **Gradle**:
```bash
./gradlew test
```

### **Expected Output:**
- All tests **pass** successfully.
- The implementation meets all requirements.

---

### **4. Step 3: Refactor Phase - Clean Up the Code**

Now that all tests are passing, let's refactor the code for **readability and maintainability**.

---

### **Refactored FizzBuzzService Class**

```java
public class FizzBuzzService {

    public String getFizzBuzz(int number) {
        if (isDivisibleBy(number, 15)) {
            return "FizzBuzz";
        }
        if (isDivisibleBy(number, 3)) {
            return "Fizz";
        }
        if (isDivisibleBy(number, 5)) {
            return "Buzz";
        }
        return String.valueOf(number);
    }

    private boolean isDivisibleBy(int number, int divisor) {
        return number % divisor == 0;
    }
}
```

### **Explanation:**
- **Refactor Phase:**
  - Extracted the condition check into a private method `isDivisibleBy()`.
  - Improved readability and maintainability.
- **No New Functionality:** We only improved the structure, keeping the behavior the same.

### **Re-running the Test:**
- Run the tests again to ensure nothing is broken.

### **Expected Output:**
- All tests **pass**.
- Code is cleaner and easier to maintain.

---

## **5. TDD Best Practices**
- **Start with Failing Tests:** Always write tests before writing implementation code.
- **Small Increments:** Implement the simplest solution to make the test pass.
- **Refactor Often:** Clean up the code in each cycle while keeping the behavior intact.
- **Test All Scenarios:** Cover all edge cases and negative scenarios.

---

## **6. Common Mistakes to Avoid**
- **Skipping the Red Phase:** Never write code without a failing test.
- **Over-engineering:** Write only the minimum code needed to pass the test.
- **No Refactoring:** Always refactor to improve readability and maintainability.
- **Testing Too Much Logic Together:** Each test should check one behavior or scenario.

---

## **7. Expert Insights and Best Practices**
- **Short Red-Green-Refactor Cycles:** Keep the cycles short for better focus and momentum.
- **Consistent Naming Convention:** Use descriptive names for test methods (`testMethodName_WhenCondition_ExpectedBehavior()`).
- **Focus on Behavior:** TDD focuses on behavior rather than implementation details.
- **Continuous Integration:** Integrate TDD with CI/CD pipelines to catch regressions early.

---

## **8. Hands-On Exercise**
1. **Implement Another TDD Flow:**
   - Implement a `PalindromeChecker` using TDD.
2. **Test Edge Cases:**
   - Add tests for edge cases like negative numbers and null inputs.
3. **Refactor Phase Practice:**
   - Refactor the `FizzBuzzService` to use a `Map<Integer, String>` for condition checks.

---

## **Next Steps: Phase 4 - Lesson 12**
In the next lesson, we'll cover:
- **Best Practices in JUnit Testing**
  - Writing maintainable and readable tests.
  - Avoiding common pitfalls and anti-patterns.
  - Measuring code coverage with **JaCoCo**.
