# **🚀 Lesson 8: Bridge Pattern – Decoupling Abstraction from Implementation**

The **Bridge Pattern** is a **structural design pattern** that helps **separate abstraction from implementation** so that they can evolve **independently**.

---

## **📌 1. What is the Bridge Pattern?**
The **Bridge Pattern**:
✔ **Decouples abstraction from implementation**.  
✔ Allows **independent changes** in both abstraction and implementation.  
✔ Uses **composition instead of inheritance**.  

---

## **📌 2. When to Use the Bridge Pattern?**
✔ When you want **separation between abstraction (interface) and implementation (behavior)**.  
✔ When you **don’t want to be forced into a rigid class hierarchy**.  
✔ When you expect **changes in both abstraction and implementation** independently.  
✔ When following the **Open/Closed Principle** (extend functionality without modifying existing code).  

---

## **📌 3. Real-World Analogy – Remote Control for Devices 📺**
Imagine **TV remote controls**:
- **Abstraction**: The **remote control** interface (`turnOn()`, `turnOff()`).
- **Implementation**: Different **TV brands** (Samsung, Sony, LG).

The **same remote can work with different TV brands** without knowing their exact details.

---

## **📌 4. Implementing Bridge Pattern in Java**
We will build a **Remote Control System** that works with multiple TV brands.

---

### **Step 1: Create the Implementation Interface**
This interface represents different **TV brands**.

```java
// Implementation Interface
public interface TV {
    void turnOn();
    void turnOff();
}
```

---

### **Step 2: Implement Concrete Implementations**
These are **different TV brands**.

```java
// Concrete Implementation 1
public class SamsungTV implements TV {
    @Override
    public void turnOn() {
        System.out.println("Samsung TV is now ON");
    }

    @Override
    public void turnOff() {
        System.out.println("Samsung TV is now OFF");
    }
}

// Concrete Implementation 2
public class SonyTV implements TV {
    @Override
    public void turnOn() {
        System.out.println("Sony TV is now ON");
    }

    @Override
    public void turnOff() {
        System.out.println("Sony TV is now OFF");
    }
}
```
✔ **Different TV brands implement the `TV` interface**.  
✔ **Each brand has its own turnOn() and turnOff() implementation**.  

---

### **Step 3: Create the Abstraction (Remote Control)**
The **Remote Control** works **independently** of the TV brand.

```java
// Abstraction
public abstract class RemoteControl {
    protected TV tv; // Bridge between abstraction and implementation

    public RemoteControl(TV tv) {
        this.tv = tv;
    }

    public abstract void turnOn();
    public abstract void turnOff();
}
```
✔ Holds a **reference to a TV object** (`TV tv`).  
✔ Remote does **not depend on specific TV brands**.  

---

### **Step 4: Implement Concrete Remote Controls**
```java
// Concrete Remote 1 - Basic Remote
public class BasicRemote extends RemoteControl {
    public BasicRemote(TV tv) {
        super(tv);
    }

    @Override
    public void turnOn() {
        System.out.print("Basic Remote: ");
        tv.turnOn();
    }

    @Override
    public void turnOff() {
        System.out.print("Basic Remote: ");
        tv.turnOff();
    }
}

// Concrete Remote 2 - Advanced Remote with Mute
public class AdvancedRemote extends RemoteControl {
    public AdvancedRemote(TV tv) {
        super(tv);
    }

    @Override
    public void turnOn() {
        System.out.print("Advanced Remote: ");
        tv.turnOn();
    }

    @Override
    public void turnOff() {
        System.out.print("Advanced Remote: ");
        tv.turnOff();
    }

    public void mute() {
        System.out.println("Advanced Remote: Muting TV");
    }
}
```
✔ **Multiple Remote types (`BasicRemote`, `AdvancedRemote`) work with any TV brand**.  
✔ **New remote controls can be added without modifying TV classes**.  

---

### **Step 5: Using the Bridge Pattern**
```java
public class Main {
    public static void main(String[] args) {
        // Using Basic Remote with Samsung TV
        TV samsung = new SamsungTV();
        RemoteControl basicRemote = new BasicRemote(samsung);
        basicRemote.turnOn();
        basicRemote.turnOff();

        // Using Advanced Remote with Sony TV
        TV sony = new SonyTV();
        AdvancedRemote advancedRemote = new AdvancedRemote(sony);
        advancedRemote.turnOn();
        advancedRemote.mute();
        advancedRemote.turnOff();
    }
}
```

---

## **📌 5. Expected Output**
```
Basic Remote: Samsung TV is now ON
Basic Remote: Samsung TV is now OFF
Advanced Remote: Sony TV is now ON
Advanced Remote: Muting TV
Advanced Remote: Sony TV is now OFF
```
✔ **Any remote control can work with any TV brand**.  
✔ **No need to modify existing TV classes** when adding new remotes.  

---

## **📌 6. Advantages of the Bridge Pattern**
✅ **Decouples abstraction from implementation** – Remote and TV can evolve separately.  
✅ **Increases flexibility** – New remotes and TVs can be added independently.  
✅ **Promotes Open/Closed Principle** – Extend functionality without modifying existing classes.  
✅ **Reduces inheritance complexity** – Avoids deep class hierarchies.  

---

## **📌 7. Common Mistakes & How to Avoid Them**
❌ **Using inheritance instead of composition** – Always **use composition (holding a reference)**.  
❌ **Overcomplicating simple cases** – Use **only when multiple abstractions and implementations exist**.  
❌ **Direct dependency on implementations** – Keep the abstraction **independent** from specific implementations.  

---

## **🔥 Hands-On Challenge**
✔ Implement a **Payment System** where `PaymentMethod` (CreditCard, PayPal) works with `PaymentGateway` (Stripe, Paytm).  
✔ Implement a **Shape Drawing System** where different `Shapes` (Circle, Square) work with different `Color` implementations (Red, Blue).  

---

## **🚀 Summary**
| **Concept** | **Explanation** |
|------------|----------------|
| **Bridge Pattern** | Decouples **abstraction from implementation**. |
| **Abstraction** | Defines **general behavior** (e.g., `RemoteControl`). |
| **Implementation** | Defines **specific behavior** (e.g., `SamsungTV`, `SonyTV`). |
| **Example** | A **Remote Control that works with any TV brand**. |

---
