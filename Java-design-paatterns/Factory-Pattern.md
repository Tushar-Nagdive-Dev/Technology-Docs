### **🚀 Lesson 2: Factory Pattern – Mastering Object Creation Efficiently**
  
The **Factory Pattern** is a **creational design pattern** that helps in **creating objects without exposing the instantiation logic** to the client. It provides **a centralized place** to create objects.

---

## 📌 **1. What is the Factory Pattern?**
The **Factory Pattern** is used to **create objects** dynamically based on the input parameters.

- The **client doesn't need to know** the exact class it is instantiating.
- The **Factory Method** decides **which subclass** to instantiate.
- Helps in **loosely coupling** code.

---

## 📌 **2. When to Use Factory Pattern?**
✔ When **object creation logic is complex** and needs to be centralized.  
✔ When you need **a common interface** for different object types.  
✔ When the **exact class type is unknown at compile time**.  
✔ When following the **Open/Closed Principle** (new classes can be added without modifying the factory).  

---

## 📌 **3. Real-World Analogy – Factory as a Pizza Maker 🍕**
Imagine a **Pizza Factory**:
- Customers order different types of pizzas (**Margherita, Pepperoni, BBQ Chicken**).
- They don’t know **how the pizzas are made** internally.
- They just request a **pizza type**, and the **Factory (Pizzeria)** prepares and returns the appropriate pizza.

---

## 📌 **4. Implementing Factory Pattern in Java**

### **Step 1: Create an Interface (Product)**
```java
public interface Pizza {
    void prepare();
}
```

---

### **Step 2: Implement Different Concrete Products**
```java
public class MargheritaPizza implements Pizza {
    @Override
    public void prepare() {
        System.out.println("Preparing Margherita Pizza 🍕");
    }
}

public class PepperoniPizza implements Pizza {
    @Override
    public void prepare() {
        System.out.println("Preparing Pepperoni Pizza 🍕");
    }
}
```

---

### **Step 3: Create the Factory Class**
The **PizzaFactory** class decides **which object to return** based on the input.

```java
public class PizzaFactory {
    public static Pizza getPizza(String type) {
        if (type.equalsIgnoreCase("margherita")) {
            return new MargheritaPizza();
        } else if (type.equalsIgnoreCase("pepperoni")) {
            return new PepperoniPizza();
        }
        throw new IllegalArgumentException("Unknown pizza type: " + type);
    }
}
```
✔ **Encapsulates object creation logic**  
✔ **Returns objects dynamically**  

---

### **Step 4: Client Code (Using the Factory)**
```java
public class Main {
    public static void main(String[] args) {
        Pizza pizza1 = PizzaFactory.getPizza("margherita");
        pizza1.prepare();

        Pizza pizza2 = PizzaFactory.getPizza("pepperoni");
        pizza2.prepare();
    }
}
```

**💡 Output:**
```
Preparing Margherita Pizza 🍕
Preparing Pepperoni Pizza 🍕
```

---

## 📌 **5. Advantages of Factory Pattern**
✅ **Encapsulation** – Object creation logic is hidden from the client.  
✅ **Loose Coupling** – Clients use an **interface**, not concrete classes.  
✅ **Open/Closed Principle** – New types can be added without modifying existing code.  
✅ **Centralized Object Creation** – Helps in managing dependencies efficiently.  

---

## 📌 **6. Common Mistakes & How to Avoid Them**
❌ **Forgetting to use an interface** – Factory should return an **interface type**, not a concrete class.  
❌ **Hardcoding dependencies** – Always **use abstraction** to make the factory flexible.  
❌ **Adding too many conditions** – Use **Reflection** or **Enum Factory** for better scalability.  

---

## 📌 **7. Advanced Factory Implementations**
1️⃣ **Factory with Reflection** – Instead of `if-else`, dynamically instantiate objects.  
2️⃣ **Abstract Factory Pattern** – A Factory **that returns other factories**.  
3️⃣ **Spring Framework FactoryBeans** – Used extensively in **Spring Boot dependency injection**.  

---

## **🔥 Hands-On Challenge**
✔ Modify the `PizzaFactory` to support **VeganPizza**.  
✔ Implement a **LoggerFactory** to return **ConsoleLogger** or **FileLogger** dynamically.  

---
