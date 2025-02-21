---

# **Phase 5: Performance Benchmarking with JMH**

---

## **Objective**

In this phase, we will:
- Benchmark key methods for:
  - **OrderService.createOrder()**
  - **ProductService.getProductById()**
- Measure:
  - **Average execution time** (`Mode.AverageTime`)
  - **Throughput** (`Mode.Throughput`)
- Use **JMH (Java Microbenchmark Harness)** for high-precision performance testing.
- Identify **performance bottlenecks** and **optimize code** for better efficiency.

---

## **1. Why Use JMH for Performance Benchmarking?**

- **Accurate Measurement:** JMH uses **nanosecond precision**.
- **JVM Warmup:** Eliminates JVM startup overhead with **automatic warmup iterations**.
- **Multiple Benchmark Modes:**
  - **AverageTime:** Measures the average execution time per operation.
  - **Throughput:** Measures the number of operations per time unit.
  - **SampleTime:** Samples the time taken for each operation.
- **Isolated Benchmarking:** JMH executes each benchmark in a **separate JVM instance**, ensuring no interference from other tests.

---

## **2. Key Concepts in JMH**

### **1. Annotations Used in JMH**
- `@Benchmark`: Marks a method as a benchmark.
- `@BenchmarkMode`: Sets the benchmark mode (`AverageTime`, `Throughput`, etc.).
- `@OutputTimeUnit`: Specifies the time unit for output (`TimeUnit.MILLISECONDS`, `TimeUnit.NANOSECONDS`).
- `@Warmup`: Specifies the number of warmup iterations to **warm up the JVM**.
- `@Measurement`: Sets the number of measurement iterations.
- `@Fork`: Runs the benchmark in a **new JVM instance** to avoid JVM optimizations.

---

## **3. Setting Up JMH for Performance Benchmarking**

### **1. Add JMH Dependencies**

**Maven:**
```xml
<dependency>
    <groupId>org.openjdk.jmh</groupId>
    <artifactId>jmh-core</artifactId>
    <version>1.37</version>
</dependency>
<dependency>
    <groupId>org.openjdk.jmh</groupId>
    <artifactId>jmh-generator-annprocess</artifactId>
    <version>1.37</version>
</dependency>
```

**Gradle:**
```groovy
dependencies {
    implementation 'org.openjdk.jmh:jmh-core:1.37'
    annotationProcessor 'org.openjdk.jmh:jmh-generator-annprocess:1.37'
}
```

---

### **2. Configure Maven Plugin for Running JMH**

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>3.2.4</version>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                    <configuration>
                        <transformers>
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                <mainClass>org.openjdk.jmh.Main</mainClass>
                            </transformer>
                        </transformers>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

---

## **4. Benchmarking OrderService.createOrder()**

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
        // Business logic for order creation
        order.setTotalPrice(order.getQuantity() * order.getProductPrice());
        return orderRepository.save(order);
    }
}
```

---

### **2. JMH Benchmark for createOrder()**

```java
package com.example.order.benchmark;

import com.example.order.entity.Order;
import com.example.order.repository.OrderRepository;
import com.example.order.service.OrderService;
import org.openjdk.jmh.annotations.*;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.context.annotation.Import;
import org.springframework.test.context.ActiveProfiles;
import java.util.concurrent.TimeUnit;

@SpringBootTest
@Import(OrderService.class)
@ActiveProfiles("test")
@BenchmarkMode(Mode.AverageTime)
@OutputTimeUnit(TimeUnit.MILLISECONDS)
@Warmup(iterations = 3, time = 1)
@Measurement(iterations = 5, time = 1)
@Fork(1)
public class OrderServiceBenchmark {

    @Autowired
    private OrderService orderService;

    @Autowired
    private OrderRepository orderRepository;

    @Benchmark
    public void benchmarkCreateOrder() {
        Order order = new Order();
        order.setUserId(1L);
        order.setProductId(1L);
        order.setQuantity(2);
        order.setProductPrice(50.0);
        orderService.createOrder(order);
    }
}
```

---

### **3. Explanation:**
- `@Benchmark`: Marks `benchmarkCreateOrder()` as a benchmark method.
- `@BenchmarkMode(Mode.AverageTime)`: Measures the **average execution time** in milliseconds.
- `@OutputTimeUnit(TimeUnit.MILLISECONDS)`: Outputs the result in milliseconds.
- `@Warmup(iterations = 3, time = 1)`: Warms up the JVM with **3 iterations** of 1 second each.
- `@Measurement(iterations = 5, time = 1)`: Measures the benchmark for **5 iterations** of 1 second each.
- `@Fork(1)`: Runs the benchmark in **1 new JVM instance**.
- **Benchmark Flow:**
  - Creates a new `Order` object.
  - Calls `orderService.createOrder()` to measure the execution time.
  - **No Assertions** are used in performance tests.

---

### **4. Running JMH Benchmark**

**Maven:**
```bash
mvn clean install
java -jar target/benchmark.jar
```

### **Expected Output:**
```
Benchmark                           Mode  Cnt    Score    Error  Units
OrderServiceBenchmark.benchmarkCreateOrder    avgt    5    12.345 ± 0.456  ms/op
```

- **Score**: The average time taken per operation.
- **Error**: The error margin.
- **Units**: Milliseconds per operation (`ms/op`).

---

## **5. Benchmarking ProductService.getProductById()**

### **1. JMH Benchmark for getProductById()**

```java
package com.example.product.benchmark;

import com.example.product.service.ProductService;
import org.openjdk.jmh.annotations.*;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.context.annotation.Import;
import org.springframework.test.context.ActiveProfiles;

import java.util.concurrent.TimeUnit;

@SpringBootTest
@Import(ProductService.class)
@ActiveProfiles("test")
@BenchmarkMode(Mode.Throughput)
@OutputTimeUnit(TimeUnit.SECONDS)
@Warmup(iterations = 3, time = 1)
@Measurement(iterations = 5, time = 1)
@Fork(1)
public class ProductServiceBenchmark {

    @Autowired
    private ProductService productService;

    @Benchmark
    public void benchmarkGetProductById() {
        productService.getProductById(1L);
    }
}
```

### **Explanation:**
- `@BenchmarkMode(Mode.Throughput)`: Measures the **throughput** (operations per second).
- **Benchmark Flow:**
  - Calls `productService.getProductById()` to measure the throughput.

---

## **6. Performance Optimization and Analysis**
- **Analyze the results** and identify methods with high execution time.
- **Optimize code** by:
  - Refactoring complex logic.
  - Caching frequently accessed data.
  - Reducing database queries.
- **Re-run benchmarks** to validate improvements.

---

## **Next Steps: Phase 6 - Code Coverage with JaCoCo**

In the next phase, we'll:
- Integrate **JaCoCo** to measure code coverage.
- Ensure **80%+ code coverage** for all microservices.
- Generate detailed coverage reports.
