### **Phase 1: Foundational Concepts of Multithreading**  
We’ll start with the basics and gradually move to advanced topics. This will ensure you build a **solid foundation** before handling complex multithreading scenarios.  

---

## **Lesson 1: Understanding Concurrency & Multithreading**
Before diving into coding, let's first understand the **why** and **how** of multithreading.

### **1.1 What is Concurrency?**
Concurrency means **executing multiple tasks in overlapping time intervals** instead of sequentially. However, these tasks **may not run in parallel**—they can share the same CPU and switch execution back and forth.

🔹 Example: **You are listening to music while writing code.**  
   - The processor rapidly switches between playing music and handling your typing.  
   - You feel like both tasks are happening together (concurrently), but they might not actually be running in parallel.

### **1.2 What is Parallelism?**
Parallelism means **executing multiple tasks at the same exact time** on different CPU cores.

🔹 Example: **Having multiple chefs cooking different dishes in a kitchen at the same time.**  
   - Each chef (CPU core) is cooking their own dish (task) without waiting for another chef to finish.

### **1.3 Difference Between Concurrency & Parallelism**
| Feature         | Concurrency                         | Parallelism                    |
|---------------|--------------------------------|-------------------------------|
| Execution     | Tasks start but may not finish together | Tasks run at the exact same time |
| CPU Utilization | Single CPU rapidly switches tasks  | Requires multiple CPU cores |
| Example       | Listening to music while coding  | Running video rendering & gaming together |

✅ **Key takeaway:**  
- **Concurrency** is about switching between tasks effectively.  
- **Parallelism** is about running multiple tasks at the same time using multiple cores.

---

## **Lesson 2: Why Do We Need Multithreading?**
Multithreading allows a Java program to **execute multiple tasks at the same time, improving efficiency and performance**.

🔹 **Key Use Cases:**
1. **UI Responsiveness:**  
   - If a UI thread is handling a long operation (e.g., file download), the application will freeze.  
   - A separate thread can handle the download while the UI remains responsive.

2. **Performance Optimization:**  
   - Web servers like **Apache Tomcat** handle thousands of user requests simultaneously using multiple threads.  

3. **Efficient CPU Utilization:**  
   - A **single-threaded program wastes CPU cycles** when waiting for tasks like I/O operations.  
   - **Multiple threads** keep the CPU busy, reducing idle time.

4. **Real-world Examples:**
   - **Web Browsers**: Open multiple tabs (each tab runs in a separate thread).
   - **Video Processing**: Editing software uses multiple threads for rendering.
   - **Gaming**: AI computations, physics calculations, and rendering run on separate threads.

✅ **Key takeaway:**  
Multithreading helps **improve performance**, **enhance responsiveness**, and **optimize CPU usage** in Java applications.

---

## **Lesson 3: Java Thread Model**
Java provides **built-in support** for multithreading through the `Thread` class and `Runnable` interface.

### **3.1 What is a Thread?**
A **Thread** is the smallest unit of execution in a program.  
Each Java program has at least **one thread** called the **main thread**.

### **3.2 Thread Lifecycle**
A thread in Java has several **states**:

1. **NEW**: Thread is created but not started yet.  
2. **RUNNABLE**: Thread is ready to run, waiting for CPU time.  
3. **RUNNING**: Thread is currently executing.  
4. **BLOCKED**: Thread is waiting for access to a locked resource.  
5. **WAITING**: Thread is waiting indefinitely for another thread to signal.  
6. **TIMED_WAITING**: Thread is waiting for a fixed time (`sleep()`, `join(time)`).  
7. **TERMINATED**: Thread has finished execution.

### **3.3 Understanding Main Thread**
```java
public class MainThreadExample {
    public static void main(String[] args) {
        // Get the current thread
        Thread t = Thread.currentThread();
        System.out.println("Current Thread: " + t);
        
        // Setting thread name
        t.setName("MainAppThread");
        System.out.println("Updated Thread Name: " + t.getName());

        // Checking thread priority
        System.out.println("Thread Priority: " + t.getPriority());
    }
}
```
🔹 **Output Example:**
```
Current Thread: Thread[main,5,main]
Updated Thread Name: MainAppThread
Thread Priority: 5
```
✅ **Key takeaway:** Every Java application starts with the **main thread**, which runs the `main()` method.

---

## **Lesson 4: Creating Threads in Java**
### **4.1 Two Ways to Create Threads**
#### **Method 1: Extending `Thread` Class**
```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread is running: " + Thread.currentThread().getName());
    }
}
public class ThreadExample {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        t1.start(); // Start thread
    }
}
```
✅ **Best Practice:** Avoid extending `Thread` unless necessary.

---

#### **Method 2: Implementing `Runnable` Interface (Recommended)**
```java
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("Thread is running: " + Thread.currentThread().getName());
    }
}
public class RunnableExample {
    public static void main(String[] args) {
        Thread t1 = new Thread(new MyRunnable());
        t1.start();
    }
}
```
✅ **Why is `Runnable` preferred?**  
- Allows better flexibility (Java supports **single inheritance**, so using `Runnable` allows extending another class).
- Promotes loose coupling.

---

## **Lesson 5: Managing Threads**
### **5.1 Starting and Stopping Threads**
- **`start()`** → Starts a new thread.
- **`run()`** → Runs in the same thread (not recommended for multithreading).
- **`sleep(milliseconds)`** → Puts thread to sleep for a specified time.
- **`join()`** → Waits for a thread to finish execution.

```java
class MyThread extends Thread {
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + " - " + i);
        }
    }
}
public class ThreadMethods {
    public static void main(String[] args) throws InterruptedException {
        MyThread t1 = new MyThread();
        MyThread t2 = new MyThread();
        
        t1.start();
        t2.start();

        t1.join(); // Main thread waits for t1 to finish
        System.out.println("Main thread completed execution.");
    }
}
```
✅ **Key takeaway:**  
- **Use `start()` instead of calling `run()` directly.**
- **Use `join()` to make one thread wait for another to complete.**

---

### **🚀 Hands-On Exercise 1**
🔹 **Task:**  
- Create a Java program where **three threads** (`T1`, `T2`, `T3`) print numbers **1 to 10** in sequence.

---

## **What's Next?**
Now that you understand **basic multithreading concepts**, in the next lesson, we'll cover **Thread Synchronization and Race Conditions**.

Let me know if you want **more explanations, examples, or hands-on exercises** before moving forward! 🚀

**review JTL-P1.md**
