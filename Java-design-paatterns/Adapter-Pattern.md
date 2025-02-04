# **🚀 Lesson 6: Adapter Pattern – Bridging Incompatible Interfaces (Easiest Explanation!)**

## **📌 What is the Adapter Pattern?**
The **Adapter Pattern** helps two incompatible systems work together **without modifying their code**.

👉 It **acts as a bridge** between two different interfaces.  
👉 It **translates requests** from one class into a format another class understands.  

---

## **📌 Real-Life Example: Power Plug Adapter ⚡**
Imagine **you travel from the USA to Europe**.  
- The **USA** uses **110V and Type A plugs**.  
- **Europe** uses **220V and Type C plugs**.  
- You **cannot directly plug in** your American charger in Europe!  
- You need a **power adapter** to **convert the plug and voltage**.

### **🛠 How This Relates to Java**
- The **US Plug** is the existing class (`USPlug`).
- The **European Socket** is the expected interface (`EuropeanSocket`).
- The **Voltage Adapter** converts the US Plug to work with the European socket (`VoltageAdapter`).

---

## **📌 How Adapter Pattern Works in Java**
There are **three main components** in the Adapter Pattern:

| **Component**  | **Purpose** |
|--------------|------------|
| **Target Interface** | Defines the method the client expects. |
| **Adaptee (Existing Class)** | Has incompatible methods. |
| **Adapter Class** | Converts Adaptee’s methods to match the Target Interface. |

---

## **📌 Step-by-Step Implementation**
Let’s build a **Voltage Adapter** that converts a **US plug (110V)** to work with a **European socket (220V).**

### **Step 1: Define the Target Interface (Expected Interface)**
The European socket expects this method:
```java
// Target Interface (Client expects this)
public interface EuropeanSocket {
    void supplyPower(); // European sockets use this method
}
```

---

### **Step 2: Create the Adaptee (Incompatible Class)**
This is an **existing class** that does **not match the required interface**.

```java
// Adaptee (Existing US Plug – Incompatible with European Socket)
public class USPlug {
    public void connectToUSOutlet() {
        System.out.println("Connected to US outlet (110V)");
    }
}
```
- This **US plug only works in 110V** sockets.
- But **we need it to work in a 220V European socket**.

---

### **Step 3: Create the Adapter Class**
This class **bridges the gap** by converting **110V to 220V** and allowing the **US plug to work in a European socket**.

```java
// Adapter Class (Bridges the gap)
public class VoltageAdapter implements EuropeanSocket {
    private USPlug usPlug;

    public VoltageAdapter(USPlug usPlug) {
        this.usPlug = usPlug;
    }

    @Override
    public void supplyPower() {
        System.out.println("Converting 110V to 220V...");
        usPlug.connectToUSOutlet(); // Calls existing method
        System.out.println("Power supplied via European socket (220V)");
    }
}
```
✔ Implements `EuropeanSocket`, so the client can use it.  
✔ **Internally uses the `USPlug`** to make it compatible.  

---

### **Step 4: Using the Adapter**
Now, let’s test it in the `main` method.

```java
public class Main {
    public static void main(String[] args) {
        USPlug usPlug = new USPlug(); // Original US Plug (110V)
        EuropeanSocket adapter = new VoltageAdapter(usPlug); // Adapter converts it

        adapter.supplyPower(); // Client calls EuropeanSocket interface
    }
}
```

---

## **📌 Expected Output**
```
Converting 110V to 220V...
Connected to US outlet (110V)
Power supplied via European socket (220V)
```
✔ **The US plug now works in a European socket.**  
✔ **No changes were made to the original `USPlug` or `EuropeanSocket`.**  

---

## **📌 Two Ways to Implement Adapter Pattern**
| **Approach**  | **Uses**  | **Example** |
|--------------|-----------|------------|
| **Class Adapter** | Uses **inheritance** | `VoltageAdapter extends USPlug` |
| **Object Adapter** | Uses **composition** | `VoltageAdapter has a USPlug instance` |

---

## **📌 Class Adapter Example (Using Inheritance)**
Instead of **composition**, we can use **inheritance**:

```java
// Adapter using Inheritance
public class VoltageAdapter extends USPlug implements EuropeanSocket {
    @Override
    public void supplyPower() {
        System.out.println("Converting 110V to 220V...");
        connectToUSOutlet(); // Directly call inherited method
        System.out.println("Power supplied via European socket (220V)");
    }
}
```
✔ **Simpler**, but **less flexible** (single inheritance restriction in Java).  
✔ Use **Object Adapter** when you need **more flexibility**.  

---

## **📌 Advantages of the Adapter Pattern**
✅ **No changes to existing classes** – Works with old code without modifying it.  
✅ **Better flexibility** – Easily integrates with third-party or legacy code.  
✅ **Encapsulation** – Hides complex conversion logic inside the adapter.  
✅ **Open/Closed Principle** – New adapters can be added without modifying existing code.  

---

## **📌 Common Mistakes & How to Avoid Them**
❌ **Modifying the original class (`USPlug`) instead of using an adapter** – Always use an adapter.  
❌ **Overcomplicating simple conversions** – Use Adapter only when needed.  
❌ **Choosing Class Adapter when Object Adapter is better** – Use **Object Adapter** for flexibility.  

---

## **🔥 Hands-On Challenge**
✔ Implement a **Media Player Adapter** that allows an **MP4 Player** to play **MP3 files**.  
✔ Create a **Currency Adapter** that converts **USD transactions to EUR transactions**.  

---

## **🚀 Summary**
| **Concept**  | **Explanation** |
|-------------|---------------|
| **Adapter Pattern** | Bridges the gap between incompatible interfaces. |
| **Class Adapter** | Uses **inheritance** to adapt. |
| **Object Adapter** | Uses **composition** to adapt. |
| **Example** | **VoltageAdapter** converts `USPlug` (110V) to `EuropeanSocket` (220V). |
| **Real-Life Example** | **Power Adapter for different plug types**. |

---
