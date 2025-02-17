# **Phase 4: Expert-Level Multithreading**
Now that you've mastered **Thread Pools, Synchronization, and Concurrency Utilities**, let's move to **Parallel Computing and Asynchronous Programming** with:
1. **Fork/Join Framework (Parallel Processing)**
2. **CompletableFuture (Asynchronous Programming)**
3. **Reactive Programming with WebFlux (Optional)**

---

## **Lesson 14: Fork/Join Framework (Parallel Processing)**
The **Fork/Join Framework** is used for efficiently executing **divide-and-conquer tasks** using multiple CPU cores.

### **14.1 Why Fork/Join Instead of Traditional Threads?**
- Traditional threads **don’t optimize CPU usage** for recursive or large data-processing tasks.
- **Fork/Join** is optimized for **parallel execution** of tasks by dividing them into subtasks.

### **14.2 Understanding Fork/Join Mechanism**
1. **Fork:** Split a large task into **smaller independent subtasks**.
2. **Join:** Combine results from subtasks **after execution completes**.

---

### **14.3 Example: Summing a Large Array Using Fork/Join**
```java
import java.util.concurrent.RecursiveTask;
import java.util.concurrent.ForkJoinPool;

class SumTask extends RecursiveTask<Integer> {
    private final int[] array;
    private final int start, end;
    private static final int THRESHOLD = 10; // Base condition for recursion

    public SumTask(int[] array, int start, int end) {
        this.array = array;
        this.start = start;
        this.end = end;
    }

    @Override
    protected Integer compute() {
        if (end - start <= THRESHOLD) {
            // Base condition: Direct computation
            int sum = 0;
            for (int i = start; i < end; i++) sum += array[i];
            return sum;
        } else {
            // Divide task into subtasks
            int mid = (start + end) / 2;
            SumTask leftTask = new SumTask(array, start, mid);
            SumTask rightTask = new SumTask(array, mid, end);

            leftTask.fork(); // Fork left subtask
            int rightResult = rightTask.compute(); // Compute right subtask
            int leftResult = leftTask.join(); // Wait for left subtask

            return leftResult + rightResult;
        }
    }
}

public class ForkJoinExample {
    public static void main(String[] args) {
        int[] array = new int[100];
        for (int i = 0; i < array.length; i++) array[i] = i + 1; // Fill array with values

        ForkJoinPool pool = new ForkJoinPool();
        int totalSum = pool.invoke(new SumTask(array, 0, array.length));

        System.out.println("Total Sum: " + totalSum); // Expected: 5050
    }
}
```
✅ **Best Practices for Fork/Join**
- Use for **data-intensive** recursive tasks.
- Set a **threshold** to avoid excessive task splitting.
- Use **ForkJoinPool.invoke()** to start execution.

---

## **Lesson 15: Asynchronous Programming with CompletableFuture**
`CompletableFuture` provides **non-blocking, asynchronous programming** in Java.

### **15.1 Why Use CompletableFuture?**
1. **Runs tasks asynchronously** without blocking main thread.
2. **Chaining tasks** without callback hell.
3. **Handles exceptions elegantly**.

---

### **15.2 Creating Asynchronous Tasks**
```java
import java.util.concurrent.CompletableFuture;

public class CompletableFutureExample {
    public static void main(String[] args) {
        CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
            System.out.println("Running async task on thread: " + Thread.currentThread().getName());
        });

        future.join(); // Wait for task to complete
    }
}
```
✅ **Key Takeaways**
- `runAsync()` runs a task **without returning a result**.
- `join()` waits for completion **without throwing exceptions**.

---

### **15.3 Chaining Asynchronous Tasks**
```java
import java.util.concurrent.CompletableFuture;

public class CompletableFutureChaining {
    public static void main(String[] args) {
        CompletableFuture.supplyAsync(() -> {
            System.out.println("Fetching Data...");
            return 100;
        }).thenApply(data -> {
            System.out.println("Processing Data...");
            return data * 2;
        }).thenAccept(result -> {
            System.out.println("Final Result: " + result);
        }).join();  // Ensures execution completes before main thread exits
    }
}
```
✅ **Key Features**
- `supplyAsync()` **returns a value**.
- `thenApply()` **modifies the result**.
- `thenAccept()` **consumes the final value**.

---

### **15.4 Handling Exceptions in Asynchronous Tasks**
```java
import java.util.concurrent.CompletableFuture;

public class CompletableFutureExceptionHandling {
    public static void main(String[] args) {
        CompletableFuture.supplyAsync(() -> {
            if (Math.random() > 0.5) throw new RuntimeException("Error occurred!");
            return 100;
        }).exceptionally(ex -> {
            System.out.println("Handling error: " + ex.getMessage());
            return 0; // Default value on error
        }).thenAccept(result -> System.out.println("Final Result: " + result))
          .join();
    }
}
```
✅ **Best Practice:** Always handle errors using `exceptionally()`.

---

## **Lesson 16: Reactive Programming (Optional)**
### **16.1 Why Reactive Programming?**
- Traditional threads **block resources** while waiting.
- **Reactive programming** uses **asynchronous streams** to handle millions of requests efficiently.

---

### **16.2 Basics of Project Reactor**
🔹 **Reactive Types**
| Type            | Description |
|----------------|-------------|
| `Mono<T>`      | Emits **0 or 1** value |
| `Flux<T>`      | Emits **multiple** values |

---

### **16.3 Example: Mono & Flux**
```java
import reactor.core.publisher.Mono;
import reactor.core.publisher.Flux;

public class ReactiveExample {
    public static void main(String[] args) {
        Mono.just("Hello, Reactive World!")
            .subscribe(System.out::println);

        Flux.just("Apple", "Banana", "Orange")
            .subscribe(System.out::println);
    }
}
```
✅ **Reactive programming is used in**:
- **Spring WebFlux** for non-blocking APIs.
- **High-performance applications** handling thousands of users.

---

### 🚀 **Final Hands-On Challenge**
🔹 **Task:**
- Implement a **multi-threaded web scraper**:
  - Use `CompletableFuture` to fetch **multiple URLs in parallel**.
  - Use `Fork/Join` to **process large datasets efficiently**.
  - Optimize execution using **ExecutorService**.

---

## **What's Next?**
Tushar, you've now completed **Java Multithreading from Beginner to Expert** 🚀. Here’s how you can continue:
✅ **Practice:** Implement **real-world multi-threaded apps**.  
✅ **Deep Dive:** Explore **Reactive Programming & WebFlux** in Spring.  
✅ **Profiling:** Use **JProfiler** or **Java Flight Recorder** to analyze thread performance.

📚 **Recommended Books**
1. **Java Concurrency in Practice** – Brian Goetz
2. **Effective Java** – Joshua Bloch (Concurrency Section)

🎯 **Next Steps:**  
Let me know if you want to:
- **Implement real-world projects**
- **Optimize thread performance**
- **Learn advanced concurrency patterns**

🔥 **You've mastered Java Multithreading! What do you want to learn next?** 🚀
