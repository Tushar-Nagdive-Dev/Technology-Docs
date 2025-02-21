---

# **Phase 5 - Lesson 14: Performance Testing and Parallel Execution**

---

## **1. What is Performance Testing?**

**Performance Testing** is the process of evaluating the speed, responsiveness, and stability of a system under a given workload. It helps in:
- **Measuring execution time** of methods or code blocks.
- **Identifying performance bottlenecks**.
- **Ensuring system reliability** under high load.

---

## **2. Why Use JMH for Performance Testing?**

**JMH (Java Microbenchmark Harness)** is a tool designed for **benchmarking Java code**. It is developed by the **OpenJDK team** and is ideal for:
- **Measuring method execution time** with high precision.
- **Benchmarking small code snippets** or algorithms.
- **Eliminating JVM optimizations** like Just-In-Time (JIT) compilation that can affect results.

### **Key Features of JMH:**
- **Accurate Measurement:** Uses **nanoseconds precision**.
- **JVM Warmup:** Automatically warms up the JVM to eliminate startup overhead.
- **Multiple Modes:** Supports different benchmark modes (e.g., throughput, average time).

---

## **3. Setting Up JMH**

### **1. Add JMH Dependency**

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

### **2. Configure Maven Plugin**

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

## **4. Writing a Benchmark Using JMH**

Let's benchmark the performance of two different methods for **string concatenation**:
1. Using `StringBuilder`.
2. Using the `+` operator.

---

### **Example: StringConcatenationBenchmark Class**

```java
import org.openjdk.jmh.annotations.Benchmark;
import org.openjdk.jmh.annotations.BenchmarkMode;
import org.openjdk.jmh.annotations.Fork;
import org.openjdk.jmh.annotations.Measurement;
import org.openjdk.jmh.annotations.Mode;
import org.openjdk.jmh.annotations.OutputTimeUnit;
import org.openjdk.jmh.annotations.Warmup;

import java.util.concurrent.TimeUnit;

public class StringConcatenationBenchmark {

    private static final String BASE = "Hello";
    private static final String SUFFIX = "World";

    @Benchmark
    @BenchmarkMode(Mode.AverageTime)
    @OutputTimeUnit(TimeUnit.NANOSECONDS)
    @Warmup(iterations = 3, time = 1)
    @Measurement(iterations = 5, time = 1)
    @Fork(1)
    public String testStringBuilder() {
        StringBuilder sb = new StringBuilder();
        sb.append(BASE);
        sb.append(SUFFIX);
        return sb.toString();
    }

    @Benchmark
    @BenchmarkMode(Mode.AverageTime)
    @OutputTimeUnit(TimeUnit.NANOSECONDS)
    @Warmup(iterations = 3, time = 1)
    @Measurement(iterations = 5, time = 1)
    @Fork(1)
    public String testStringConcatenation() {
        return BASE + SUFFIX;
    }
}
```

---

### **Explanation:**
- `@Benchmark`: Marks the method as a benchmark.
- `@BenchmarkMode(Mode.AverageTime)`: Measures the **average time** taken by the method.
- `@OutputTimeUnit(TimeUnit.NANOSECONDS)`: Outputs the result in **nanoseconds**.
- `@Warmup`: **Warms up the JVM** before measuring, eliminating startup overhead.
- `@Measurement`: Specifies the number of iterations for the benchmark.
- `@Fork(1)`: Runs the benchmark in a **separate JVM instance** to avoid JVM optimizations.

---

### **5. Running JMH Benchmark**

**Maven:**
```bash
mvn clean install
java -jar target/benchmark.jar
```

### **Expected Output:**
```
Benchmark                           Mode  Cnt    Score    Error  Units
StringConcatenationBenchmark.testStringBuilder     avgt    5    10.123 ± 0.456  ns/op
StringConcatenationBenchmark.testStringConcatenation avgt    5    15.789 ± 0.567  ns/op
```

- **Score**: The average time taken per operation.
- **Error**: The error margin.
- **Units**: Nanoseconds per operation (`ns/op`).

---

## **6. Parallel Test Execution in JUnit 5**

### **1. Why Use Parallel Test Execution?**
- **Faster Feedback Loop:** Runs tests in parallel, reducing the total execution time.
- **Efficient CI/CD Pipeline:** Speeds up continuous integration by running multiple tests simultaneously.

### **2. Enabling Parallel Execution**

JUnit 5 supports parallel execution through the **Jupiter configuration**. You can enable it using:
- **JUnit Platform Properties File**
- **Maven Surefire Plugin**
- **Gradle Configuration**

---

### **3. Configuring Parallel Execution Using JUnit Platform Properties**

Create a file named `junit-platform.properties` in the `src/test/resources` directory.

```properties
junit.jupiter.execution.parallel.enabled = true
junit.jupiter.execution.parallel.mode.default = concurrent
junit.jupiter.execution.parallel.config.strategy = dynamic
junit.jupiter.execution.parallel.config.dynamic.factor = 2
```

### **Explanation:**
- `parallel.enabled`: Enables parallel execution.
- `mode.default = concurrent`: Executes tests concurrently.
- `dynamic.factor = 2`: Dynamically assigns threads based on CPU cores.

### **Expected Behavior:**
- JUnit **automatically** distributes the tests across available CPU cores.
- The tests **run concurrently**, reducing the total execution time.

---

## **7. Configuring Parallel Execution with Maven Surefire Plugin**

You can also configure parallel execution using the **Maven Surefire Plugin**.

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>3.0.0-M5</version>
            <configuration>
                <parallel>methods</parallel>
                <threadCount>4</threadCount>
                <forkCount>2</forkCount>
                <reuseForks>true</reuseForks>
            </configuration>
        </plugin>
    </plugins>
</build>
```

### **Explanation:**
- `parallel = methods`: Runs test methods in parallel.
- `threadCount = 4`: Sets the number of threads.
- `forkCount = 2`: Runs tests in 2 separate JVMs.
- `reuseForks = true`: Reuses JVM instances to improve performance.

---

## **8. Hands-On Exercise**
1. **Create Additional Benchmarks:**
   - Benchmark the performance of `ArrayList` vs. `LinkedList` operations.
2. **Experiment with Parallel Execution:**
   - Enable parallel execution and observe the performance improvement.
3. **Edge Case Performance:**
   - Benchmark edge cases like large input sizes and boundary conditions.

---

## **Next Steps: Phase 5 - Lesson 15**
In the next lesson, we'll cover:
- **Real-World Projects and Case Studies**
  - End-to-End Testing with JUnit and Selenium.
  - Testing Microservices Architecture using Spring Cloud and WireMock.
  - CI/CD integration with JUnit using **GitLab CI** and **GitHub Actions**.
