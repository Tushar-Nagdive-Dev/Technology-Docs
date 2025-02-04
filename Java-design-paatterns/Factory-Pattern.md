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
### **🚀 Extending Factory Pattern with Reflection – A More Scalable Approach**

Instead of using multiple `if-else` conditions in the factory method, we can use **Reflection** to create objects dynamically. This approach makes the factory **more flexible, maintainable, and scalable**.

---

## **📌 1. Why Use Reflection in Factory Pattern?**
### **Problems with Traditional Factory**
- Every time a new class (e.g., `VeganPizza`) is added, we must **modify the Factory** (`if-else` block).
- Violates the **Open/Closed Principle** (code should be open for extension but closed for modification).

### **Solution: Reflection-Based Factory**
- Uses **fully qualified class names**.
- Removes **hardcoded logic** (`if-else` conditions).
- Makes the factory **truly dynamic**.

---

## **📌 2. Implementing Reflection-Based Factory**
We’ll modify the **PizzaFactory** to use **Reflection**.

---

### **Step 1: Define the Interface (Product)**
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

public class VeganPizza implements Pizza {
    @Override
    public void prepare() {
        System.out.println("Preparing Vegan Pizza 🌱");
    }
}
```

---

### **Step 3: Create a Reflection-Based Factory**
```java
public class PizzaFactory {
    public static Pizza getPizza(String className) {
        try {
            // Load the class dynamically using Reflection
            Class<?> clazz = Class.forName(className);

            // Ensure the class implements Pizza interface
            if (!Pizza.class.isAssignableFrom(clazz)) {
                throw new IllegalArgumentException(className + " does not implement Pizza interface");
            }

            // Create a new instance of the class
            return (Pizza) clazz.getDeclaredConstructor().newInstance();
        } catch (Exception e) {
            throw new RuntimeException("Failed to create pizza: " + e.getMessage(), e);
        }
    }
}
```

✔ **Dynamically creates objects without `if-else`**  
✔ **No need to modify the Factory when adding new Pizza types**  

---

### **Step 4: Using the Reflection Factory**
```java
public class Main {
    public static void main(String[] args) {
        // Get Pizza instances dynamically
        Pizza pizza1 = PizzaFactory.getPizza("MargheritaPizza");
        pizza1.prepare();

        Pizza pizza2 = PizzaFactory.getPizza("PepperoniPizza");
        pizza2.prepare();

        Pizza pizza3 = PizzaFactory.getPizza("VeganPizza");
        pizza3.prepare();
    }
}
```

---

## **📌 3. Explanation**
1. **Reflection (`Class.forName()`)** loads the class dynamically.
2. **Ensures the class implements the `Pizza` interface** (`isAssignableFrom` check).
3. **Creates an instance using `newInstance()`** without hardcoded conditions.
4. **If a new class (e.g., `BBQPizza`) is added**, **no modifications** are needed in the `PizzaFactory`.

---

## **📌 4. Advantages of Reflection-Based Factory**
| **Feature**              | **Traditional Factory** | **Reflection Factory** |
|--------------------------|------------------------|------------------------|
| **New Class Addition**   | Modify Factory (`if-else`) | No modification needed |
| **Code Maintenance**     | High (every new class requires update) | Low (truly dynamic) |
| **Open/Closed Principle** | ❌ Violates (modifies factory often) | ✅ Follows (extend without changes) |
| **Performance**          | ✅ Fast | ❌ Slight overhead due to Reflection |

---

## **📌 5. Common Mistakes & How to Avoid Them**
❌ **Passing an invalid class name** – Handle with exception handling.  
❌ **Performance overhead of Reflection** – Use caching for frequently created objects.  
❌ **Forgetting to implement the interface** – Use `isAssignableFrom()` to check.  

---

## **🔥 Hands-On Challenge**
✔ Modify the factory to use **a configuration file (`pizza-config.properties`)** to store available pizza types.  
✔ Extend the factory to support **different pizza sizes dynamically**.  

---
