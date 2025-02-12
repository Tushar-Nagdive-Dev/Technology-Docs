# 🚀 **Phase 1 - Lesson 8: Implementing Spring Boot Scheduling (Cron Jobs & Background Tasks)** 🚀  

## **📌 Lesson Objective**
By the end of this lesson, you will:  
✅ Understand **what task scheduling is** and why it's useful  
✅ Learn how to **enable scheduling in Spring Boot**  
✅ Implement **simple scheduled tasks using `@Scheduled`**  
✅ Configure **Cron Jobs for precise scheduling**  
✅ Handle **asynchronous tasks using `@Async`**  

---

## **1️⃣ What is Task Scheduling & Why is it Important?**
Task Scheduling allows an application to **execute tasks at predefined intervals** without manual intervention.

### **🔥 Real-World Examples**
✔ **Send automated emails every morning** 📧  
✔ **Generate reports at midnight** 📊  
✔ **Clean up old database records weekly** 🗑  
✔ **Fetch latest stock prices every minute** 💹  

Spring Boot provides **built-in support** for scheduling tasks using the `@Scheduled` annotation.

---

## **2️⃣ Enabling Scheduling in Spring Boot**
📌 **Step 1: Add `@EnableScheduling` to the Main Class**
Modify `DemoApplication.java`:

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.scheduling.annotation.EnableScheduling;

@SpringBootApplication
@EnableScheduling  // Enables Spring Boot scheduling
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

✅ **Scheduling is now enabled!** 🚀  

---

## **3️⃣ Implementing a Simple Scheduled Task**
📌 **Step 2: Create a Scheduled Task**
Create a new package **`com.example.demo.scheduler`** and add `ScheduledTasks.java`:

```java
package com.example.demo.scheduler;

import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;
import java.time.LocalDateTime;

@Component  // Marks this class as a Spring-managed Bean
public class ScheduledTasks {

    @Scheduled(fixedRate = 5000)  // Runs every 5 seconds
    public void runTask() {
        System.out.println("Task executed at: " + LocalDateTime.now());
    }
}
```

✅ **How It Works?**
- `@Scheduled(fixedRate = 5000)` → Runs every **5 seconds**.
- `System.out.println("Task executed at: " + LocalDateTime.now());`  
  → Prints a message in the console.

### **Step 3: Run & Test**
1️⃣ **Start the application**  
```bash
mvn spring-boot:run
```
2️⃣ **Observe Console Logs**
```
Task executed at: 2025-02-12T12:00:00
Task executed at: 2025-02-12T12:00:05
Task executed at: 2025-02-12T12:00:10
...
```
🎉 **Your first scheduled task is working!**  

---

## **4️⃣ Using Fixed Delay vs Fixed Rate**
Spring Boot allows different scheduling strategies:

| Annotation | Execution Type | Example |
|------------|---------------|---------|
| `@Scheduled(fixedRate = 5000)` | Runs every **5 seconds**, even if the previous task is still running | Every 5 sec: `12:00:00`, `12:00:05`, `12:00:10` |
| `@Scheduled(fixedDelay = 5000)` | Runs **5 seconds after the last task finishes** | If task takes 2 sec, next runs after 5 sec from finish |
| `@Scheduled(initialDelay = 10000, fixedRate = 5000)` | Starts **after 10 sec**, then runs every 5 sec | First: `12:00:10`, then every 5 sec |

📌 **Example with Fixed Delay**
```java
@Scheduled(fixedDelay = 5000)
public void runTaskWithDelay() {
    System.out.println("Task executed with delay at: " + LocalDateTime.now());
}
```

---

## **5️⃣ Configuring Cron Jobs for Precise Scheduling**
📌 **Cron expressions allow precise scheduling**, like:
- Run every **day at 6 AM**  
- Execute **only on weekdays**  
- Schedule tasks for **specific months**  

📌 Modify `ScheduledTasks.java`:
```java
@Scheduled(cron = "0 0 6 * * ?") // Runs every day at 6 AM
public void runDailyTask() {
    System.out.println("Daily Task executed at: " + LocalDateTime.now());
}
```

✅ **How Cron Jobs Work?**
```
┌───────────── second (0-59)
│ ┌───────────── minute (0-59)
│ │ ┌───────────── hour (0-23)
│ │ │ ┌───────────── day of the month (1-31)
│ │ │ │ ┌───────────── month (1-12)
│ │ │ │ │ ┌───────────── day of the week (0 - 7) (Sunday = 0 or 7)
│ │ │ │ │ │
│ │ │ │ │ │
* * * * * *
```

| Cron Expression | Meaning |
|----------------|---------|
| `"0 0 * * * *"` | Every hour |
| `"0 0 6 * * *"` | Every day at 6 AM |
| `"0 0 12 * * MON-FRI"` | Every weekday at 12 PM |
| `"0 0 0 1 * *"` | First day of every month |

---

## **6️⃣ Running Background Jobs Asynchronously**
🔹 **Problem:** Scheduled tasks run in the **same thread** by default, blocking execution.  
🔹 **Solution:** Use `@Async` to execute tasks **in parallel**.

📌 Modify `DemoApplication.java` to enable async tasks:
```java
import org.springframework.scheduling.annotation.EnableAsync;
import org.springframework.context.annotation.Configuration;

@Configuration
@EnableAsync  // Enables async execution
public class AsyncConfig {
}
```

📌 Modify `ScheduledTasks.java` to use `@Async`:
```java
import org.springframework.scheduling.annotation.Async;

@Async
@Scheduled(fixedRate = 10000)
public void runAsyncTask() {
    System.out.println("Async task running in background: " + LocalDateTime.now());
}
```

✅ **How It Works?**
- `@Async` → Runs tasks in a **separate thread**.
- Now **background jobs** won't block the main application.

---

## **7️⃣ Real-World Use Case: Database Cleanup Task**
📌 Modify `UserService.java` to clean inactive users:

```java
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Service;
import java.time.LocalDateTime;
import java.util.List;

@Service
public class UserService {

    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    @Scheduled(cron = "0 0 0 * * ?")  // Runs every midnight
    public void cleanInactiveUsers() {
        System.out.println("Running cleanup task: " + LocalDateTime.now());

        List<User> inactiveUsers = userRepository.findInactiveUsers();
        userRepository.deleteAll(inactiveUsers);

        System.out.println("Inactive users deleted: " + inactiveUsers.size());
    }
}
```

✅ **Now, inactive users are deleted automatically every midnight!** 🌙  

---

## 🎯 **Lesson 8 - Summary**
✅ Enabled **Spring Boot scheduling** using `@EnableScheduling`  
✅ Implemented **`@Scheduled(fixedRate, fixedDelay, cron)`**  
✅ Used **cron jobs for precise scheduling**  
✅ Optimized tasks with **`@Async` for background execution**  
✅ Created a **database cleanup job**  

---
