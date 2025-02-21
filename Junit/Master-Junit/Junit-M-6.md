---

# **Phase 6: Code Coverage with JaCoCo**

---

## **Objective**

In this phase, we will:
- Integrate **JaCoCo** to measure code coverage.
- Ensure **80%+ code coverage** for:
  - **Unit Tests**
  - **Integration Tests**
- Generate **detailed coverage reports** for:
  - **Class Coverage**
  - **Method Coverage**
  - **Line Coverage**
  - **Branch Coverage**
- Identify **uncovered lines** and **optimize test cases** for better coverage.

---

## **1. Why Use JaCoCo for Code Coverage?**

- **Comprehensive Coverage Metrics:** Measures class, method, line, and branch coverage.
- **Detailed Reports:** Generates detailed HTML reports showing:
  - **Covered and uncovered lines**
  - **Conditionals and branches**
- **Integration with CI/CD Pipelines:** Works seamlessly with:
  - **GitHub Actions**
  - **GitLab CI**
- **Feedback Loop:** Provides instant feedback on test quality and gaps.

---

## **2. Key Metrics in Code Coverage**

- **Line Coverage:** Percentage of lines executed by the tests.
- **Branch Coverage:** Percentage of branches (if-else) executed.
- **Method Coverage:** Percentage of methods called during testing.
- **Class Coverage:** Percentage of classes that have been tested.

---

## **3. Setting Up JaCoCo**

### **1. Add JaCoCo Plugin**

**Maven:**
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.jacoco</groupId>
            <artifactId>jacoco-maven-plugin</artifactId>
            <version>0.8.8</version>
            <executions>
                <execution>
                    <goals>
                        <goal>prepare-agent</goal>
                        <goal>report</goal>
                    </goals>
                </execution>
            </executions>
            <configuration>
                <excludes>
                    <exclude>**/entity/**</exclude>
                    <exclude>**/config/**</exclude>
                    <exclude>**/exception/**</exclude>
                </excludes>
            </configuration>
        </plugin>
    </plugins>
</build>
```

**Gradle:**
```groovy
plugins {
    id 'jacoco'
}

jacoco {
    toolVersion = "0.8.8"
}

tasks.test {
    useJUnitPlatform()
    finalizedBy tasks.jacocoTestReport
}

jacocoTestReport {
    reports {
        xml.required = true
        csv.required = false
        html.outputLocation = file("${buildDir}/reports/jacoco")
    }
}
```

---

### **2. Configuration Explanation:**
- `prepare-agent`: Prepares JaCoCo for code instrumentation.
- `report`: Generates the coverage report after tests are executed.
- `excludes`: Excludes packages that **do not require testing**, such as:
  - **Entities** (POJOs without logic)
  - **Configurations**
  - **Custom Exceptions**

---

## **4. Running Code Coverage Report**

**Maven:**
```bash
mvn clean test
mvn jacoco:report
```

**Gradle:**
```bash
./gradlew clean test
./gradlew jacocoTestReport
```

---

### **Expected Output:**
- The report is generated at:
```
target/site/jacoco/index.html (Maven)
build/reports/jacoco/test/html/index.html (Gradle)
```
- Open `index.html` in a browser to view detailed coverage.

---

## **5. Analyzing the JaCoCo Report**

### **1. Report Overview:**
- **Green Lines:** Covered by tests.
- **Red Lines:** Not covered by tests.
- **Yellow Branches:** Partially covered branches.
- **Metrics Displayed:**
  - Class Coverage
  - Method Coverage
  - Line Coverage
  - Branch Coverage

---

### **2. Example Analysis:**

**OrderService.createOrder() Coverage:**
- **Line Coverage:** 90% (1 line not covered)
- **Branch Coverage:** 75% (1 branch not covered)
- **Method Coverage:** 100%

### **3. Optimizing Coverage:**
- Identify uncovered lines and branches.
- Write additional test cases to cover:
  - **Edge Cases**
  - **Negative Scenarios**
  - **Conditionals** (if-else branches)
- Re-run the tests and **regenerate the report** to ensure improved coverage.

---

## **6. Example: Improving Branch Coverage**

Let's improve the **Branch Coverage** for `OrderService.createOrder()`:

### **1. OrderService Class (Recap)**

```java
package com.example.order.service;

import com.example.order.entity.Order;
import com.example.order.repository.OrderRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class OrderService {

    @Autowired
    private OrderRepository orderRepository;

    public Order createOrder(Order order) {
        if (order.getQuantity() <= 0) {
            throw new IllegalArgumentException("Quantity must be greater than zero");
        }
        order.setTotalPrice(order.getQuantity() * order.getProductPrice());
        return orderRepository.save(order);
    }
}
```

---

### **2. Improved Unit Test for Branch Coverage**

```java
package com.example.order.service;

import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.Mockito.*;

import com.example.order.entity.Order;
import com.example.order.repository.OrderRepository;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;

@ExtendWith(MockitoExtension.class)
public class OrderServiceTest {

    @Mock
    private OrderRepository orderRepository;

    @InjectMocks
    private OrderService orderService;

    private Order order;

    @BeforeEach
    void setUp() {
        order = new Order();
        order.setUserId(1L);
        order.setProductId(1L);
        order.setQuantity(2);
        order.setProductPrice(50.0);
    }

    @Test
    void testCreateOrder_Success() {
        when(orderRepository.save(order)).thenReturn(order);

        Order savedOrder = orderService.createOrder(order);

        assertNotNull(savedOrder);
        assertEquals(100.0, savedOrder.getTotalPrice());
        verify(orderRepository, times(1)).save(order);
    }

    @Test
    void testCreateOrder_InvalidQuantity() {
        order.setQuantity(0);

        Exception exception = assertThrows(IllegalArgumentException.class, () -> {
            orderService.createOrder(order);
        });

        String expectedMessage = "Quantity must be greater than zero";
        String actualMessage = exception.getMessage();

        assertTrue(actualMessage.contains(expectedMessage));
    }
}
```

---

### **3. Explanation:**
- **New Test Case Added:** `testCreateOrder_InvalidQuantity()`
  - Tests the scenario where the quantity is `0`.
  - Expects an `IllegalArgumentException` to be thrown.
- **Branch Covered:**
  - The branch `if (order.getQuantity() <= 0)` is now fully covered.

---

### **4. Re-run Code Coverage Report**
```bash
mvn test
mvn jacoco:report
```

### **Expected Output:**
- **Branch Coverage:** 100%
- **Line Coverage:** 100%
- **Method Coverage:** 100%

---

## **7. Enforcing Coverage Thresholds**

You can enforce **coverage thresholds** to ensure the team maintains high coverage.

### **Maven:**
```xml
<configuration>
    <rules>
        <rule>
            <element>BUNDLE</element>
            <limits>
                <limit>
                    <counter>LINE</counter>
                    <value>COVEREDRATIO</value>
                    <minimum>0.80</minimum>
                </limit>
                <limit>
                    <counter>BRANCH</counter>
                    <value>COVEREDRATIO</value>
                    <minimum>0.75</minimum>
                </limit>
            </limits>
        </rule>
    </rules>
</configuration>
```

### **Explanation:**
- **Minimum Coverage:**
  - **Line Coverage:** 80%
  - **Branch Coverage:** 75%
- **Build Failure:** The build will **fail** if the coverage is below the threshold.

---

## **Next Steps: Phase 7 - CI/CD Integration**

In the next phase, we'll:
- Integrate **GitHub Actions** or **GitLab CI** for:
  - **Building** the application.
  - **Running all tests** (Unit, Integration, E2E).
  - **Generating coverage reports**.
  - **Deploying** the application.
- Ensure **continuous testing** and **fast feedback**.
