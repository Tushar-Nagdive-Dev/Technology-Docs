### **🚀 Lesson 1: Singleton Pattern – Mastering the Most Common Creational Pattern**

The **Singleton Pattern** is one of the most widely used design patterns in Java. It ensures that a **class has only one instance** and provides a **global access point** to that instance.

---

## 📌 **1. What is the Singleton Pattern?**
- The **Singleton Pattern** restricts the instantiation of a class to **only one object**.
- It provides a **global point of access** to that object.
- Often used for **logging, configuration management, database connections**, and other shared resources.

### **🛠 Real-World Analogy**
Think of a **government**—a country has **only one government** that manages everything. No matter where you go, you refer to the same **government instance**.

---

## 📌 **2. When to Use Singleton Pattern?**
✔ When you need to **control access to a shared resource** (e.g., database connection, configuration manager).  
✔ When having **multiple instances would create conflicts** (e.g., managing system logs).  
✔ When the object **needs to be instantiated once and used globally**.

---

## 📌 **3. Common Implementations of Singleton in Java**
There are **several ways** to implement a Singleton. We will go step by step from **basic** to **advanced**.

### **🟢 1. Lazy Initialization (Not Thread-Safe) – Basic Implementation**
This is the simplest way to implement a Singleton. The instance is **created only when needed**.

```java
public class Singleton {
    private static Singleton instance; // Static instance variable

    private Singleton() { 
        // Private constructor prevents instantiation
    }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton(); // Create instance lazily
        }
        return instance;
    }
}
```

### ⚠ **Problem**: Not **thread-safe**—if multiple threads call `getInstance()`, they might create multiple instances.

---

### **🟢 2. Thread-Safe Singleton (Synchronized Method)**
To fix the above issue, we can use **synchronization**.

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() { }

    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```
✔ **Thread-safe**, but **synchronization adds performance overhead**.

---

### **🟢 3. Double-Checked Locking – Optimized Thread Safety**
A better approach for thread safety and performance is **Double-Checked Locking**.

```java
public class Singleton {
    private static volatile Singleton instance;

    private Singleton() { }

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```
✔ **Thread-safe and optimized**  
✔ Uses **volatile** to prevent instruction reordering.  
✔ Best for **high-performance applications**.

---

### **🟢 4. Bill Pugh Singleton (Best Practice)**
The **Bill Pugh Singleton** is the **best way** to implement a Singleton in Java.

```java
public class Singleton {
    private Singleton() { }

    private static class SingletonHelper {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return SingletonHelper.INSTANCE;
    }
}
```
✔ **Lazy-loaded**  
✔ **Thread-safe** without synchronization  
✔ **Uses inner static class** which is only loaded when needed.

---

### **🟢 5. Enum Singleton (Best for Serialization & Reflection)**
The **best way** to prevent reflection and serialization issues is using an **enum**.

```java
public enum Singleton {
    INSTANCE;

    public void showMessage() {
        System.out.println("Hello from Singleton!");
    }
}
```
✔ **Thread-safe**  
✔ **Prevents reflection and serialization attacks**  
✔ **Simple to use** (`Singleton.INSTANCE.showMessage();`)

---

## 📌 **4. Common Mistakes & How to Avoid Them**
❌ **Multiple instances due to improper synchronization**  
✔ Use **Double-Checked Locking or Bill Pugh Singleton**.  

❌ **Reflection can break Singleton**  
✔ Use **enum Singleton** to prevent reflection.  

❌ **Serialization can create multiple instances**  
✔ Implement `readResolve()` method in `Serializable` Singletons.  

---

## 📌 **5. Practical Applications of Singleton Pattern**
💡 **Logging** – `Logger.getInstance().log("Message")`  
💡 **Database Connection Pool** – Managing shared DB connections  
💡 **Cache Management** – Store frequently used data  
💡 **Thread Pools** – Reuse limited threads efficiently  

---

## 📌 **6. Hands-On Exercise**
✅ **Implement a Singleton for a Configuration Manager** that loads settings from a file.  
✅ **Modify the Singleton to be thread-safe** using the best method.  

---
### **💻 Hands-On Exercise: Singleton Configuration Manager**  

Let's implement a **Configuration Manager** using the **Singleton Pattern**. This Configuration Manager will:
- **Load configuration settings** from a properties file.
- **Ensure only one instance** exists throughout the application.
- **Provide a global access point** for configuration retrieval.

---

### **Step 1: Create a Configuration File**
Create a file named **`config.properties`** in the `src/main/resources` folder (or your project directory).

#### **config.properties**
```
app.name=CashFlowApp
app.version=1.0.0
db.url=jdbc:postgresql://localhost:5432/cashflow
db.username=admin
db.password=secret
```

---

### **Step 2: Implement the Singleton Configuration Manager**

#### **ConfigManager.java**
```java
import java.io.FileInputStream;
import java.io.IOException;
import java.util.Properties;

public class ConfigManager {
    private static volatile ConfigManager instance;
    private Properties properties;

    // Private constructor to restrict instantiation
    private ConfigManager() {
        properties = new Properties();
        try (FileInputStream input = new FileInputStream("src/main/resources/config.properties")) {
            properties.load(input);
        } catch (IOException e) {
            throw new RuntimeException("Failed to load configuration file", e);
        }
    }

    // Double-Checked Locking Singleton
    public static ConfigManager getInstance() {
        if (instance == null) {
            synchronized (ConfigManager.class) {
                if (instance == null) {
                    instance = new ConfigManager();
                }
            }
        }
        return instance;
    }

    // Method to retrieve property values
    public String getProperty(String key) {
        return properties.getProperty(key);
    }
}
```
✔ **Thread-safe** with **Double-Checked Locking**  
✔ Reads configuration from **`config.properties`**  
✔ **Lazy initialization** (loaded only when needed)  

---

### **Step 3: Using the Singleton in a Main Class**
#### **Main.java**
```java
public class Main {
    public static void main(String[] args) {
        ConfigManager configManager = ConfigManager.getInstance();

        System.out.println("Application Name: " + configManager.getProperty("app.name"));
        System.out.println("Version: " + configManager.getProperty("app.version"));
        System.out.println("Database URL: " + configManager.getProperty("db.url"));
        System.out.println("DB Username: " + configManager.getProperty("db.username"));
    }
}
```

**💡 Output:**  
```
Application Name: CashFlowApp
Version: 1.0.0
Database URL: jdbc:postgresql://localhost:5432/cashflow
DB Username: admin
```

---

### **🔍 How This Works**
1. **First time `getInstance()` is called**, it **creates** the `ConfigManager` instance.
2. It **reads `config.properties` only once** and stores values in a `Properties` object.
3. Any subsequent calls to `getInstance()` return **the same instance**, avoiding duplicate object creation.

---

### **🔥 Next Challenge**
✔ Modify the **ConfigManager** to support **writing to the configuration file**.  
✔ Implement an **enum-based Singleton** for this manager and compare performance.  

### **🚀 Enum-Based Singleton – The Most Robust Singleton Implementation**
  
The **Enum Singleton** is the **simplest, safest, and best** way to implement a Singleton in Java. Unlike traditional implementations, it is **thread-safe by default**, protects against **reflection attacks**, and ensures **serialization safety**.

---

## **📌 Why Use Enum for Singleton?**
### **Problems in Other Singleton Implementations**
1. **Thread Safety Issues** – Lazy initialization requires **synchronization** to be safe.
2. **Serialization Issues** – Regular singletons create **new instances** when deserialized.
3. **Reflection Attack** – Using reflection, we can **break** a singleton and create multiple instances.

🔹 **Enum-based Singleton solves all of these problems!**

---

## **📌 How Enum Singleton Works?**
- Java **ensures only one instance of an Enum** exists in memory.
- Reflection **cannot instantiate enums** because the Java compiler prohibits it.
- Java ensures **enum serialization is safe** by default.

---

## **🛠 Implementing Enum Singleton**
Here’s how we implement a Singleton using **Enum**:

```java
public enum Singleton {
    INSTANCE;

    // You can add methods and properties
    private int value;

    public void setValue(int value) {
        this.value = value;
    }

    public int getValue() {
        return value;
    }

    public void showMessage() {
        System.out.println("Hello from Enum Singleton!");
    }
}
```

✔ **Thread-safe by default**  
✔ **Serialization-safe by default**  
✔ **Cannot be broken by Reflection**  

---

## **📌 Using the Enum Singleton**
You can use the Singleton like this:

```java
public class Main {
    public static void main(String[] args) {
        // Get instance of Singleton
        Singleton instance = Singleton.INSTANCE;

        // Call method
        instance.showMessage();

        // Modify and retrieve a value
        instance.setValue(42);
        System.out.println("Singleton Value: " + instance.getValue());
    }
}
```

**💡 Output:**  
```
Hello from Enum Singleton!
Singleton Value: 42
```

---

## **📌 How Enum Singleton Prevents Common Issues**
| **Problem**          | **Enum Singleton Solution** |
|----------------------|---------------------------|
| **Thread Safety**    | Enums are inherently **thread-safe** in Java. |
| **Serialization**    | Java **guarantees only one instance** of an Enum even after serialization. |
| **Reflection Attack** | Enums **cannot be instantiated via reflection** (throws an exception). |
| **Lazy Initialization** | Java **loads enums lazily** (only when first accessed). |

---

## **📌 Reflection Attack Test**
Let’s see what happens when we try to break the Enum Singleton using reflection.

```java
import java.lang.reflect.Constructor;

public class ReflectionBreakSingleton {
    public static void main(String[] args) {
        try {
            Constructor<Singleton> constructor = Singleton.class.getDeclaredConstructor();
            constructor.setAccessible(true);
            Singleton instance = constructor.newInstance();
            System.out.println("New Singleton Instance Created: " + instance);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

**💡 Output:**  
```
java.lang.NoSuchMethodException: Singleton.<init>()
```

✔ **Java prevents creating multiple instances of Enum using reflection.**

---

## **📌 Enum Singleton vs. Traditional Singleton**
| Feature                  | Traditional Singleton | Enum Singleton |
|--------------------------|----------------------|---------------|
| **Thread Safety**        | Requires extra code  | ✅ Built-in |
| **Serialization Safety** | Requires `readResolve()` | ✅ Built-in |
| **Reflection Safe**      | ❌ Can be broken | ✅ Safe |
| **Lazy Initialization**  | Possible with extra logic | ✅ By default |
| **Ease of Use**          | More code needed | ✅ Simple |

---

## **📌 When Should You Use Enum Singleton?**
✅ When you need **the safest Singleton implementation**.  
✅ When you want **thread-safety and serialization safety out of the box**.  
✅ When you want **simpler code** without synchronization headaches.  

---

## **📌 Hands-On Challenge**
🔹 Modify the Enum Singleton to **read configuration properties** and store values.  
🔹 Implement an **enum-based Logger Singleton** that writes logs to a file.  

---
