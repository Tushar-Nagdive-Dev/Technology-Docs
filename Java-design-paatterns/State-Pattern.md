# **🚀 Lesson 17: State Pattern – Managing Object Behavior Dynamically**

The **State Pattern** is a **behavioral design pattern** that allows an object to **change its behavior dynamically** based on its internal state.

---

## **📌 1. What is the State Pattern?**
The **State Pattern**:
✔ **Encapsulates different states as separate classes**.  
✔ Allows an object to **change behavior dynamically at runtime**.  
✔ Helps in **removing complex if-else or switch statements** from the code.  

---

## **📌 2. When to Use the State Pattern?**
✔ When an object **changes behavior based on its state**.  
✔ When using **multiple if-else or switch statements** to check state.  
✔ When following the **Open/Closed Principle** (new states can be added without modifying existing code).  
✔ When implementing **finite state machines** (e.g., ATMs, elevators, traffic lights).  

---

## **📌 3. Real-World Analogy – Traffic Light 🚦**
- A **Traffic Light** has three states: **Red, Yellow, Green**.
- **Each state determines how vehicles behave**.
- The **State Pattern allows switching between Red, Yellow, and Green dynamically**.

---

## **📌 4. Implementing State Pattern in Java**
Let’s build a **Traffic Light System** where signals **change dynamically**.

---

### **Step 1: Create the State Interface**
Each state **implements this interface**.

```java
// State Interface
public interface TrafficLightState {
    void handleRequest(TrafficLightContext context);
    String getState();
}
```
✔ **Defines `handleRequest()` to change the state dynamically**.  
✔ **Defines `getState()` to return the current state**.  

---

### **Step 2: Implement Concrete States**
Each class **represents a different traffic light state**.

#### **Red Light State**
```java
// Concrete State 1: Red Light
public class RedLight implements TrafficLightState {
    @Override
    public void handleRequest(TrafficLightContext context) {
        System.out.println("Red Light → Switching to Green Light");
        context.setState(new GreenLight());
    }

    @Override
    public String getState() {
        return "Red Light";
    }
}
```

#### **Green Light State**
```java
// Concrete State 2: Green Light
public class GreenLight implements TrafficLightState {
    @Override
    public void handleRequest(TrafficLightContext context) {
        System.out.println("Green Light → Switching to Yellow Light");
        context.setState(new YellowLight());
    }

    @Override
    public String getState() {
        return "Green Light";
    }
}
```

#### **Yellow Light State**
```java
// Concrete State 3: Yellow Light
public class YellowLight implements TrafficLightState {
    @Override
    public void handleRequest(TrafficLightContext context) {
        System.out.println("Yellow Light → Switching to Red Light");
        context.setState(new RedLight());
    }

    @Override
    public String getState() {
        return "Yellow Light";
    }
}
```
✔ **Each state handles its transition to the next state dynamically**.  
✔ **No if-else statements are needed in the main logic**.  

---

### **Step 3: Create the Context Class**
The **context manages the current state** and switches between states dynamically.

```java
// Context Class: Traffic Light
public class TrafficLightContext {
    private TrafficLightState state;

    public TrafficLightContext() {
        // Default state is Red
        this.state = new RedLight();
    }

    public void setState(TrafficLightState state) {
        this.state = state;
    }

    public void changeLight() {
        state.handleRequest(this);
    }

    public void showCurrentLight() {
        System.out.println("Current Light: " + state.getState());
    }
}
```
✔ **Stores the current state** (`TrafficLightState`).  
✔ **Calls `handleRequest()` to transition to the next state dynamically**.  

---

### **Step 4: Using the State Pattern**
```java
public class Main {
    public static void main(String[] args) {
        TrafficLightContext trafficLight = new TrafficLightContext();

        // Changing lights dynamically
        trafficLight.showCurrentLight();
        trafficLight.changeLight();

        trafficLight.showCurrentLight();
        trafficLight.changeLight();

        trafficLight.showCurrentLight();
        trafficLight.changeLight();

        trafficLight.showCurrentLight();
    }
}
```

---

## **📌 5. Expected Output**
```
Current Light: Red Light
Red Light → Switching to Green Light
Current Light: Green Light
Green Light → Switching to Yellow Light
Current Light: Yellow Light
Yellow Light → Switching to Red Light
Current Light: Red Light
```
✔ **The light changes dynamically without using if-else conditions**.  
✔ **Each state class controls the next transition**.  

---

## **📌 6. Key Features of the State Pattern**
| **Feature** | **Explanation** |
|------------|----------------|
| **Encapsulates Different States** | Each state has its own class. |
| **Eliminates Complex If-Else Blocks** | State transitions happen dynamically. |
| **Follows Open/Closed Principle** | New states can be added without modifying existing code. |
| **Dynamically Changes Behavior** | Object behavior changes at runtime. |

---

## **📌 7. Common Mistakes & How to Avoid Them**
❌ **Using if-else to handle state transitions** – Always **delegate state changes to state classes**.  
❌ **Tightly coupling states with context** – Each **state should be independent**.  
❌ **Forgetting to call `setState()` in `handleRequest()`** – Ensure states transition properly.  

---

## **🔥 Hands-On Challenge**
✔ Implement an **ATM State Machine** where states are **Idle, Card Inserted, Transaction Processing, Cash Dispensed**.  
✔ Implement a **Document Approval Process** where states are **Draft, Submitted, Approved, Rejected**.  

---

## **🚀 Summary**
| **Concept** | **Explanation** |
|------------|----------------|
| **State Pattern** | Allows objects to **change behavior dynamically** based on state. |
| **State Interface** | Defines common behavior for all states. |
| **Concrete State** | Implements state-specific behavior. |
| **Context Class** | Manages state transitions. |
| **Example** | **Traffic Light with Red, Yellow, Green States**. |

---
