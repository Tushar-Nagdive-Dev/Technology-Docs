# **🚀 Lesson 9: Proxy Pattern – Controlling Access to Objects**

The **Proxy Pattern** is a **structural design pattern** that provides **a placeholder or surrogate** to control access to an object.

---

## **📌 1. What is the Proxy Pattern?**
The **Proxy Pattern**:
✔ **Acts as an intermediary** between the client and the real object.  
✔ Can add **security, logging, caching, or lazy loading** before delegating to the real object.  
✔ Helps in controlling **expensive operations like database queries or network calls**.  

---

## **📌 2. When to Use the Proxy Pattern?**
✔ When you need **lazy initialization** (e.g., **loading images only when needed**).  
✔ When you need **security control** before accessing an object.  
✔ When you want **logging or caching** before forwarding the request.  
✔ When using **remote objects in distributed systems** (e.g., **RMI** in Java).  

---

## **📌 3. Real-World Analogy – Security Guard at an Office 🏢**
- You want to **meet the CEO**.
- You **cannot go directly**—a **security guard (Proxy)** first **verifies your identity**.
- If approved, **the guard lets you in** to meet the CEO.

Similarly, a **Proxy** in Java **controls access** to a real object **by adding security, caching, or logging before forwarding requests**.

---

## **📌 4. Implementing Proxy Pattern in Java**
Let’s build a **Proxy for Secure File Access**.

---

### **Step 1: Create the Subject Interface**
This defines the common methods for both **Real Object** and **Proxy**.

```java
// Subject Interface (Common for Real and Proxy)
public interface FileAccess {
    void readFile();
}
```

---

### **Step 2: Implement the Real Object**
This is the actual class that handles **file reading**.

```java
// Real Object (Performs actual work)
public class RealFile implements FileAccess {
    private String fileName;

    public RealFile(String fileName) {
        this.fileName = fileName;
        loadFile(); // Simulate loading file
    }

    private void loadFile() {
        System.out.println("Loading file: " + fileName);
    }

    @Override
    public void readFile() {
        System.out.println("Reading file: " + fileName);
    }
}
```
✔ Loads the file **once** when the object is created.  
✔ Implements `FileAccess` to provide **real file reading functionality**.  

---

### **Step 3: Implement the Proxy Class**
The **Proxy controls access** to the **RealFile**.

```java
// Proxy (Controls access to RealFile)
public class FileProxy implements FileAccess {
    private RealFile realFile;
    private String fileName;
    private String userRole;

    public FileProxy(String fileName, String userRole) {
        this.fileName = fileName;
        this.userRole = userRole;
    }

    @Override
    public void readFile() {
        if (!userRole.equals("ADMIN")) {
            System.out.println("Access Denied! Only ADMIN can read the file.");
            return;
        }

        if (realFile == null) {
            realFile = new RealFile(fileName); // Lazy Initialization
        }
        realFile.readFile();
    }
}
```
✔ **Checks user role before allowing access**.  
✔ **Lazy initializes** `RealFile` only when needed.  

---

### **Step 4: Using the Proxy Pattern**
```java
public class Main {
    public static void main(String[] args) {
        FileAccess userFile = new FileProxy("secure_data.txt", "USER");
        userFile.readFile(); // Should deny access

        FileAccess adminFile = new FileProxy("secure_data.txt", "ADMIN");
        adminFile.readFile(); // Should allow access
    }
}
```

---

## **📌 5. Expected Output**
```
Access Denied! Only ADMIN can read the file.
Loading file: secure_data.txt
Reading file: secure_data.txt
```
✔ **Proxy controls access** based on user role.  
✔ **File loads only when ADMIN tries to access it** (Lazy Loading).  

---

## **📌 6. Types of Proxy Patterns**
| **Type**          | **Purpose** |
|-------------------|------------|
| **Virtual Proxy** | Delays object creation (Lazy Loading). |
| **Protection Proxy** | Controls access based on roles (Security). |
| **Remote Proxy** | Represents objects in **different JVMs or servers** (e.g., RMI). |
| **Smart Proxy** | Adds **logging, caching, or reference counting**. |

---

## **📌 7. Java Built-in Examples of Proxy Pattern**
✅ **`java.lang.reflect.Proxy`** – Used in dynamic proxies.  
✅ **Spring AOP (Aspect-Oriented Programming)** – Uses proxies for method interception.  
✅ **Hibernate Lazy Loading** – Uses proxies to delay fetching database objects.  
✅ **RMI (Remote Method Invocation)** – Uses remote proxies for distributed computing.  

---

## **📌 8. Common Mistakes & How to Avoid Them**
❌ **Directly creating the real object instead of using a proxy** – Always **call methods via the proxy**.  
❌ **Forgetting Lazy Initialization** – Load heavy objects **only when needed**.  
❌ **Overusing Proxies** – Avoid **excessive proxying** for simple cases.  

---

## **🔥 Hands-On Challenge**
✔ Implement a **Bank Account Proxy** that **restricts withdrawals for non-premium users**.  
✔ Create a **Logging Proxy** that logs all method calls before forwarding them to the real object.  

---

## **🚀 Summary**
| **Concept** | **Explanation** |
|------------|----------------|
| **Proxy Pattern** | Controls access to objects with extra functionality (security, logging, caching). |
| **Real Object** | Performs the actual work (e.g., `RealFile`). |
| **Proxy Class** | Controls and restricts access (e.g., `FileProxy`). |
| **Example** | Security Proxy for file access (ADMIN vs. USER). |

---
