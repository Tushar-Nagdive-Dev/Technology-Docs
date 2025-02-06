# **🚀 Lesson 15: Chain of Responsibility Pattern – Handling Requests Dynamically**

The **Chain of Responsibility Pattern** is a **behavioral design pattern** where multiple handlers are linked together to **process requests sequentially** until one handles it.

---

## **📌 1. What is the Chain of Responsibility Pattern?**
The **Chain of Responsibility Pattern**:
✔ **Passes requests through a chain of handlers** until one handles it.  
✔ **Reduces coupling** between sender and receiver.  
✔ **Supports dynamic request handling**.  

---

## **📌 2. When to Use the Chain of Responsibility Pattern?**
✔ When multiple handlers **can process a request, but you don’t know which one will handle it**.  
✔ When you want to **avoid long if-else or switch statements** for handling requests.  
✔ When following the **Single Responsibility Principle** (each handler deals with one task).  
✔ When handling **logging, authentication, or validation in a flexible way**.  

---

## **📌 3. Real-World Analogy – Customer Support System ☎**
- A **customer request** can be handled by **different support levels**:
  1. **Basic Support** (handles simple queries).
  2. **Manager** (handles escalated issues).
  3. **Director** (handles critical cases).
- If **Basic Support** cannot handle it, they **pass it to the next level**.
- **The request stops once a handler processes it**.

---

## **📌 4. Implementing Chain of Responsibility Pattern in Java**
Let’s build a **Technical Support System** where **requests are handled by different support levels**.

---

### **Step 1: Create the Handler Interface**
Each handler **processes a request or passes it to the next handler**.

```java
// Handler Interface
public abstract class SupportHandler {
    protected SupportHandler nextHandler;

    public void setNextHandler(SupportHandler nextHandler) {
        this.nextHandler = nextHandler;
    }

    public abstract void handleRequest(String issueType);
}
```
✔ **Stores a reference to the next handler (`nextHandler`)**.  
✔ Calls `handleRequest()` to **process or pass the request**.  

---

### **Step 2: Implement Concrete Handlers**
Each handler **processes requests based on its level**.

#### **Basic Support Handler**
```java
// Concrete Handler 1: Basic Support
public class BasicSupport extends SupportHandler {
    @Override
    public void handleRequest(String issueType) {
        if (issueType.equalsIgnoreCase("basic")) {
            System.out.println("Basic Support: Resolving basic issue...");
        } else if (nextHandler != null) {
            nextHandler.handleRequest(issueType);
        } else {
            System.out.println("No handler found for issue: " + issueType);
        }
    }
}
```

#### **Manager Support Handler**
```java
// Concrete Handler 2: Manager Support
public class ManagerSupport extends SupportHandler {
    @Override
    public void handleRequest(String issueType) {
        if (issueType.equalsIgnoreCase("medium")) {
            System.out.println("Manager Support: Handling medium issue...");
        } else if (nextHandler != null) {
            nextHandler.handleRequest(issueType);
        } else {
            System.out.println("No handler found for issue: " + issueType);
        }
    }
}
```

#### **Director Support Handler**
```java
// Concrete Handler 3: Director Support
public class DirectorSupport extends SupportHandler {
    @Override
    public void handleRequest(String issueType) {
        if (issueType.equalsIgnoreCase("critical")) {
            System.out.println("Director Support: Addressing critical issue...");
        } else {
            System.out.println("No handler found for issue: " + issueType);
        }
    }
}
```
✔ **Each handler checks if it can handle the request**.  
✔ **If not, it passes the request to the next handler**.  

---

### **Step 3: Setting Up the Chain**
```java
public class Main {
    public static void main(String[] args) {
        // Creating Handlers
        SupportHandler basicSupport = new BasicSupport();
        SupportHandler managerSupport = new ManagerSupport();
        SupportHandler directorSupport = new DirectorSupport();

        // Setting up the chain
        basicSupport.setNextHandler(managerSupport);
        managerSupport.setNextHandler(directorSupport);

        // Making requests
        System.out.println("Request: Basic Issue");
        basicSupport.handleRequest("basic");

        System.out.println("\nRequest: Medium Issue");
        basicSupport.handleRequest("medium");

        System.out.println("\nRequest: Critical Issue");
        basicSupport.handleRequest("critical");

        System.out.println("\nRequest: Unknown Issue");
        basicSupport.handleRequest("unknown");
    }
}
```

---

## **📌 5. Expected Output**
```
Request: Basic Issue
Basic Support: Resolving basic issue...

Request: Medium Issue
Manager Support: Handling medium issue...

Request: Critical Issue
Director Support: Addressing critical issue...

Request: Unknown Issue
No handler found for issue: unknown
```
✔ **Basic issues are handled by Basic Support**.  
✔ **Medium issues are escalated to Manager Support**.  
✔ **Critical issues reach the Director**.  
✔ **Unknown issues have no handler, so they are not processed**.  

---

## **📌 6. Key Features of the Chain of Responsibility Pattern**
| **Feature** | **Explanation** |
|------------|----------------|
| **Decouples Sender & Receiver** | The client doesn't know which handler will process the request. |
| **Flexible Request Handling** | New handlers can be added **without modifying existing code**. |
| **Stops Once Handled** | The chain ends when **a handler processes the request**. |

---

## **📌 7. Common Mistakes & How to Avoid Them**
❌ **Not setting the next handler** – Always link handlers using `setNextHandler()`.  
❌ **Forgetting to terminate the chain** – Ensure **at least one handler processes the request**.  
❌ **Using too many handlers for simple cases** – Use **only when dynamic request handling is required**.  

---

## **🔥 Hands-On Challenge**
✔ Implement a **Bank Loan Approval System** where different levels (`Clerk`, `Manager`, `Director`) approve loans based on amount.  
✔ Implement a **Logging System** where logs are passed through `INFO`, `WARNING`, and `ERROR` handlers.  

---

## **🚀 Summary**
| **Concept** | **Explanation** |
|------------|----------------|
| **Chain of Responsibility Pattern** | Passes a request **through multiple handlers** until one processes it. |
| **Handler** | Each handler **processes or forwards the request**. |
| **Next Handler** | Links handlers to form a **chain**. |
| **Example** | **Customer Support System (Basic → Manager → Director)**. |

---
