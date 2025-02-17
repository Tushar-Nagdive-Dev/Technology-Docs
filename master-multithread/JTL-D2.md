# **Phase 3: Advanced Multithreading Concepts**
Now that you've mastered **thread synchronization** and **deadlock prevention**, it's time to explore **Java Concurrency utilities** (`java.util.concurrent`), which provide efficient alternatives to manual thread management.

---

## **Lesson 11: Java Concurrency Utilities (`java.util.concurrent`)**
Java introduced the **java.util.concurrent** package to make multithreading **easier and more efficient**. Instead of manually creating and managing threads, we use **ExecutorService**, **ThreadPool**, **ReentrantLock**, and more.

---

## **Lesson 12: Executor Framework & Thread Pooling**
### **12.1 Problems with Manually Creating Threads**
1. **High Resource Consumption**: Creating too many threads can exhaust system resources.
2. **Difficult to Manage**: Manually starting and stopping threads is error-prone.
3. **Inefficient CPU Utilization**: Too many threads lead to context-switching overhead.

### **12.2 What is the Executor Framework?**
The **Executor Framework** is a higher-level abstraction for handling thread management. Instead of manually managing threads, we submit tasks to a **thread pool**.

🔹 **Key Classes:**
| Class/Interface  | Description |
|-----------------|-------------|
| `Executor` | Basic interface for executing tasks asynchronously |
| `ExecutorService` | Provides methods to manage thread lifecycle |
| `ScheduledExecutorService` | Schedules tasks at fixed intervals |
| `ThreadPoolExecutor` | A flexible and configurable thread pool |

---

### **12.3 Creating a Fixed Thread Pool**
```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

class Task implements Runnable {
    private int id;

    public Task(int id) {
        this.id = id;
    }

    public void run() {
        System.out.println("Task " + id + " executed by " + Thread.currentThread().getName());
        try {
            Thread.sleep(1000);  // Simulate work
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

public class ThreadPoolExample {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(3); // 3 threads in pool

        for (int i = 1; i <= 5; i++) {
            executor.execute(new Task(i));
        }

        executor.shutdown();  // Shutdown after all tasks finish
    }
}
```
✅ **Key takeaway:**
- **FixedThreadPool** reuses a set number of threads.
- **Threads are reused**, improving performance.
- **Use `shutdown()`** to gracefully terminate the thread pool.

🔹 **Output Example** *(order may vary due to threading)*
```
Task 1 executed by pool-1-thread-1
Task 2 executed by pool-1-thread-2
Task 3 executed by pool-1-thread-3
Task 4 executed by pool-1-thread-1
Task 5 executed by pool-1-thread-2
```
---

### **12.4 Other Types of Thread Pools**
| Method | Description |
|----------------|-------------|
| `Executors.newCachedThreadPool()` | Creates threads as needed, but reuses them when possible |
| `Executors.newSingleThreadExecutor()` | Single-threaded execution (tasks run sequentially) |
| `Executors.newScheduledThreadPool(n)` | Runs tasks at scheduled intervals |

🔹 **Example: Running a Task Periodically**
```java
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.TimeUnit;

public class ScheduledThreadPoolExample {
    public static void main(String[] args) {
        ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(2);

        Runnable task = () -> System.out.println("Task executed at: " + System.currentTimeMillis());

        scheduler.scheduleAtFixedRate(task, 2, 3, TimeUnit.SECONDS); // Start after 2s, repeat every 3s
    }
}
```
✅ **Best Practice:** Use `ScheduledExecutorService` instead of `TimerTask`.

---

## **Lesson 13: Advanced Synchronization Mechanisms**
### **13.1 Using `ReentrantLock` Instead of `synchronized`**
🔹 **Why `ReentrantLock`?**
- More flexibility than `synchronized`
- Allows **tryLock()** to avoid deadlocks
- Supports **fair locking** (FIFO order)

🔹 **Example: Using `ReentrantLock`**
```java
import java.util.concurrent.locks.ReentrantLock;

class SharedResource {
    private final ReentrantLock lock = new ReentrantLock();

    public void printData(String message) {
        lock.lock();
        try {
            System.out.println(Thread.currentThread().getName() + ": " + message);
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock(); // Always release lock
        }
    }
}

public class ReentrantLockExample {
    public static void main(String[] args) {
        SharedResource resource = new SharedResource();

        Runnable task = () -> resource.printData("Processing...");

        Thread t1 = new Thread(task, "Thread A");
        Thread t2 = new Thread(task, "Thread B");

        t1.start();
        t2.start();
    }
}
```
✅ **Best Practice:** Always unlock in a `finally` block to avoid deadlocks.

---

### **13.2 Using `CountDownLatch` for Thread Coordination**
🔹 **What is CountDownLatch?**
- Ensures multiple threads **wait** until a set number of operations **complete**.

🔹 **Example: Waiting for 3 services to initialize before starting main thread**
```java
import java.util.concurrent.CountDownLatch;

class Service extends Thread {
    private CountDownLatch latch;

    public Service(String name, CountDownLatch latch) {
        super(name);
        this.latch = latch;
    }

    public void run() {
        System.out.println(Thread.currentThread().getName() + " started");
        try { Thread.sleep(2000); } catch (InterruptedException e) {}
        latch.countDown();  // Reduce count
        System.out.println(Thread.currentThread().getName() + " finished");
    }
}

public class CountDownLatchExample {
    public static void main(String[] args) throws InterruptedException {
        CountDownLatch latch = new CountDownLatch(3);

        new Service("Service 1", latch).start();
        new Service("Service 2", latch).start();
        new Service("Service 3", latch).start();

        latch.await();  // Wait until all services complete
        System.out.println("All services initialized. Starting main application...");
    }
}
```
✅ **Best Practice:** Use `CountDownLatch` when multiple threads must finish before continuing.

---

### **13.3 Using `Semaphore` for Controlling Thread Access**
🔹 **What is Semaphore?**
- Controls **how many threads** can access a resource at a time.
- Example: **Limiting concurrent users to 3 in a system.**

🔹 **Example: Restricting 3 users at a time**
```java
import java.util.concurrent.Semaphore;

class UserThread extends Thread {
    private Semaphore semaphore;

    public UserThread(String name, Semaphore semaphore) {
        super(name);
        this.semaphore = semaphore;
    }

    public void run() {
        try {
            semaphore.acquire(); // Acquire permit
            System.out.println(Thread.currentThread().getName() + " is accessing the system");
            Thread.sleep(2000); // Simulate processing
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            System.out.println(Thread.currentThread().getName() + " is leaving");
            semaphore.release(); // Release permit
        }
    }
}

public class SemaphoreExample {
    public static void main(String[] args) {
        Semaphore semaphore = new Semaphore(3); // Limit to 3 threads

        for (int i = 1; i <= 5; i++) {
            new UserThread("User " + i, semaphore).start();
        }
    }
}
```
✅ **Best Practice:** Use `Semaphore` for **limiting concurrent access** to resources.

---

### 🚀 **Hands-On Exercise 3**
🔹 **Task:**  
- Modify the **ThreadPoolExample** to execute **10 tasks** using a `FixedThreadPool` of **4 threads**.
- Add a `Semaphore` to **limit access** to a shared resource.

---

## **What's Next?**
Now that you’ve mastered **ExecutorService, ThreadPool, and Synchronization Utilities**, next, we’ll cover:
- **Fork/Join Framework (Parallel Computing)**
- **CompletableFuture (Asynchronous Programming)**
- **Reactive Programming with WebFlux**

**Move to JTL-D3.md**
