# **Phase 2: Intermediate Multithreading**
Now that you understand **basic multithreading concepts**, let's dive into **Thread Synchronization** and **Race Conditions**, which are critical for building reliable and thread-safe applications.

---

## **Lesson 6: Thread Synchronization and Race Conditions**
### **6.1 Understanding Race Conditions**
A **Race Condition** occurs when multiple threads access a shared resource and **interfere** with each other, leading to inconsistent results.

🔹 **Example Scenario: Bank Account Withdrawal**
- Imagine **two threads** (`Thread A` and `Thread B`) trying to withdraw money from the same bank account.
- If both threads check the balance **before** updating it, they might withdraw more money than available!

#### **Example of Race Condition**
```java
class BankAccount {
    private int balance = 100;

    public void withdraw(int amount) {
        if (balance >= amount) {
            System.out.println(Thread.currentThread().getName() + " is withdrawing: " + amount);
            balance -= amount;
            System.out.println(Thread.currentThread().getName() + " new balance: " + balance);
        } else {
            System.out.println(Thread.currentThread().getName() + " Not enough balance.");
        }
    }
}

public class RaceConditionExample {
    public static void main(String[] args) {
        BankAccount account = new BankAccount();
        
        Runnable task = () -> {
            for (int i = 0; i < 3; i++) {
                account.withdraw(50);
            }
        };

        Thread t1 = new Thread(task, "Thread A");
        Thread t2 = new Thread(task, "Thread B");

        t1.start();
        t2.start();
    }
}
```
### **❌ Output (Inconsistent & Incorrect Balance)**
```
Thread A is withdrawing: 50
Thread B is withdrawing: 50
Thread A new balance: 50
Thread B new balance: 50
Thread A is withdrawing: 50
Thread B is withdrawing: 50
Thread A new balance: 0
Thread B new balance: -50  ❌ (Negative Balance!)
```
✅ **Key takeaway:** Multiple threads modifying the same data **without proper synchronization** can lead to incorrect results.

---

## **Lesson 7: Thread Synchronization**
### **7.1 Using the `synchronized` Keyword**
To prevent race conditions, Java provides the `synchronized` keyword to **lock** an object, ensuring only one thread can access it at a time.

### **Fixing the Race Condition with Synchronization**
```java
class BankAccount {
    private int balance = 100;

    public synchronized void withdraw(int amount) {  // Synchronized method
        if (balance >= amount) {
            System.out.println(Thread.currentThread().getName() + " is withdrawing: " + amount);
            balance -= amount;
            System.out.println(Thread.currentThread().getName() + " new balance: " + balance);
        } else {
            System.out.println(Thread.currentThread().getName() + " Not enough balance.");
        }
    }
}

public class SynchronizedExample {
    public static void main(String[] args) {
        BankAccount account = new BankAccount();

        Runnable task = () -> {
            for (int i = 0; i < 3; i++) {
                account.withdraw(50);
            }
        };

        Thread t1 = new Thread(task, "Thread A");
        Thread t2 = new Thread(task, "Thread B");

        t1.start();
        t2.start();
    }
}
```
✅ **Now, the output will always be correct (no negative balance).**
```
Thread A is withdrawing: 50
Thread A new balance: 50
Thread B is withdrawing: 50
Thread B new balance: 0
Thread A Not enough balance.
Thread B Not enough balance.
```

---

## **Lesson 8: Object-Level vs Class-Level Locks**
### **8.1 Object-Level Lock (`synchronized method`)**
- Each **instance** of a class has a separate lock.
- If two threads are working on **different objects**, they will execute concurrently.

🔹 **Example:**
```java
class SharedResource {
    public synchronized void printData(String message) {
        System.out.println(Thread.currentThread().getName() + ": " + message);
    }
}
```

### **8.2 Class-Level Lock (`static synchronized method`)**
- A **class-level lock** ensures that all threads **must wait**, even if they are working on different instances of the class.
- Applied to `static synchronized` methods.

🔹 **Example:**
```java
class SharedResource {
    public static synchronized void printData(String message) {
        System.out.println(Thread.currentThread().getName() + ": " + message);
    }
}
```

---

## **Lesson 9: Synchronization Blocks (More Fine-Grained Control)**
Instead of locking an entire method, we can **lock only a critical section** inside a method.

🔹 **Example: Locking only the critical section:**
```java
class BankAccount {
    private int balance = 100;

    public void withdraw(int amount) {
        synchronized (this) { // Locking only this block
            if (balance >= amount) {
                System.out.println(Thread.currentThread().getName() + " is withdrawing: " + amount);
                balance -= amount;
                System.out.println(Thread.currentThread().getName() + " new balance: " + balance);
            } else {
                System.out.println(Thread.currentThread().getName() + " Not enough balance.");
            }
        }
    }
}
```
✅ **Best Practice:**  
- **Use synchronization blocks instead of whole methods** when possible.  
- It improves performance by allowing other non-critical operations to run concurrently.

---

## **Lesson 10: Deadlocks and How to Avoid Them**
### **10.1 What is a Deadlock?**
A **deadlock** occurs when two threads **wait for each other** to release locks, and neither can proceed.

🔹 **Example: Two Threads Holding Locks on Different Resources**
```java
class Resource {
    void methodA() {
        synchronized (this) {
            System.out.println(Thread.currentThread().getName() + " locked methodA");
            try { Thread.sleep(1000); } catch (Exception e) {}
            synchronized (String.class) {  // Trying to acquire another lock
                System.out.println(Thread.currentThread().getName() + " locked methodB");
            }
        }
    }

    void methodB() {
        synchronized (String.class) {
            System.out.println(Thread.currentThread().getName() + " locked methodB");
            try { Thread.sleep(1000); } catch (Exception e) {}
            synchronized (this) {  // Trying to acquire the first lock
                System.out.println(Thread.currentThread().getName() + " locked methodA");
            }
        }
    }
}

public class DeadlockExample {
    public static void main(String[] args) {
        Resource r = new Resource();

        Thread t1 = new Thread(() -> r.methodA(), "Thread A");
        Thread t2 = new Thread(() -> r.methodB(), "Thread B");

        t1.start();
        t2.start();
    }
}
```
❌ **Deadlock Scenario:**
```
Thread A locked methodA
Thread B locked methodB
(Thread A is waiting for methodB, Thread B is waiting for methodA... infinite wait!)
```

### **10.2 How to Avoid Deadlocks**
✅ **Avoid nested locks:**  
   - Don't acquire multiple locks at the same time.

✅ **Use a Lock Ordering Strategy:**  
   - Always acquire locks in a fixed order (e.g., `Lock A → Lock B` in all places).

✅ **Use TryLock (ReentrantLock) Instead of synchronized:**  
   - It avoids indefinite blocking.

---

### 🚀 **Hands-On Exercise 2**
🔹 **Task:**  
- Modify the **BankAccount** example to allow multiple users to **deposit and withdraw** using synchronization blocks.  
- Ensure that **no negative balance** occurs and allow deposits **simultaneously**.

---

## **What's Next?**
Now that you've learned **Thread Synchronization, Race Conditions, and Deadlocks**, next, we’ll explore **Advanced Java Concurrency utilities** like:
1. **ExecutorService**
2. **ThreadPool**
3. **ReentrantLock**
4. **CountDownLatch, Semaphore, and CyclicBarrier**

**Move to JTL-P2.md**
