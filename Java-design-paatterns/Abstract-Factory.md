# **🚀 Lesson 3: Abstract Factory Pattern – A Factory of Factories**

The **Abstract Factory Pattern** is an **extension** of the **Factory Pattern**. Instead of creating individual objects, it **creates factories** that produce related objects. This is also called the **Factory of Factories**.

---

## **📌 1. What is the Abstract Factory Pattern?**
The **Abstract Factory Pattern** provides an **interface** for creating families of related objects **without specifying their concrete classes**.

🔹 Instead of a **single Factory**, we have **multiple Factories**, each responsible for a specific type of object.  
🔹 This pattern is useful when **there are multiple variations of objects** (e.g., `Veg Pizza` vs `Non-Veg Pizza`).  

---

## **📌 2. When to Use Abstract Factory Pattern?**
✔ When you need to **create related families of objects** dynamically.  
✔ When you **don’t want the client to specify concrete classes**.  
✔ When you **need multiple Factory classes** instead of just one.  
✔ When following the **Open/Closed Principle** (new variations can be added without modifying the existing code).  

---

## **📌 3. Real-World Analogy – Abstract Factory as a Restaurant Chain**
Think of a **restaurant chain** that serves **Italian Food** and **Indian Food**:
- **Italian Factory** produces `Pizza` and `Pasta`.
- **Indian Factory** produces `Curry` and `Biryani`.

Instead of choosing a **specific dish**, the customer selects the **cuisine**, and the restaurant factory provides the appropriate dishes.

---

## **📌 4. Implementing Abstract Factory Pattern in Java**
We will build a **Pizza Restaurant** with two factories:  
✅ **Veg Pizza Factory** – Produces `Margherita` and `Veggie Delight` pizzas.  
✅ **Non-Veg Pizza Factory** – Produces `Pepperoni` and `BBQ Chicken` pizzas.  

---

### **Step 1: Create the Common Product Interface**
```java
public interface Pizza {
    void prepare();
}
```

---

### **Step 2: Implement Concrete Product Classes**
#### **Veg Pizzas**
```java
public class MargheritaPizza implements Pizza {
    @Override
    public void prepare() {
        System.out.println("Preparing Margherita Pizza 🍕");
    }
}

public class VeggieDelightPizza implements Pizza {
    @Override
    public void prepare() {
        System.out.println("Preparing Veggie Delight Pizza 🌱");
    }
}
```

#### **Non-Veg Pizzas**
```java
public class PepperoniPizza implements Pizza {
    @Override
    public void prepare() {
        System.out.println("Preparing Pepperoni Pizza 🍖");
    }
}

public class BBQChickenPizza implements Pizza {
    @Override
    public void prepare() {
        System.out.println("Preparing BBQ Chicken Pizza 🍗");
    }
}
```

---

### **Step 3: Create an Abstract Factory**
```java
public interface PizzaFactory {
    Pizza createPizza(String type);
}
```

---

### **Step 4: Implement Concrete Factories**
#### **Veg Pizza Factory**
```java
public class VegPizzaFactory implements PizzaFactory {
    @Override
    public Pizza createPizza(String type) {
        if (type.equalsIgnoreCase("margherita")) {
            return new MargheritaPizza();
        } else if (type.equalsIgnoreCase("veggie")) {
            return new VeggieDelightPizza();
        }
        throw new IllegalArgumentException("Unknown Veg Pizza type: " + type);
    }
}
```

#### **Non-Veg Pizza Factory**
```java
public class NonVegPizzaFactory implements PizzaFactory {
    @Override
    public Pizza createPizza(String type) {
        if (type.equalsIgnoreCase("pepperoni")) {
            return new PepperoniPizza();
        } else if (type.equalsIgnoreCase("bbq")) {
            return new BBQChickenPizza();
        }
        throw new IllegalArgumentException("Unknown Non-Veg Pizza type: " + type);
    }
}
```

---

### **Step 5: Create the Abstract Factory Provider**
This **factory of factories** returns the correct factory (Veg or Non-Veg).

```java
public class PizzaFactoryProvider {
    public static PizzaFactory getFactory(String choice) {
        if (choice.equalsIgnoreCase("veg")) {
            return new VegPizzaFactory();
        } else if (choice.equalsIgnoreCase("nonveg")) {
            return new NonVegPizzaFactory();
        }
        throw new IllegalArgumentException("Unknown Factory type: " + choice);
    }
}
```

---

### **Step 6: Client Code (Using the Abstract Factory)**
```java
public class Main {
    public static void main(String[] args) {
        // Get Veg Pizza Factory
        PizzaFactory vegFactory = PizzaFactoryProvider.getFactory("veg");
        Pizza vegPizza = vegFactory.createPizza("margherita");
        vegPizza.prepare();

        // Get Non-Veg Pizza Factory
        PizzaFactory nonVegFactory = PizzaFactoryProvider.getFactory("nonveg");
        Pizza nonVegPizza = nonVegFactory.createPizza("pepperoni");
        nonVegPizza.prepare();
    }
}
```

---

## **📌 5. Expected Output**
```
Preparing Margherita Pizza 🍕
Preparing Pepperoni Pizza 🍖
```

✔ **No hardcoded class references** in the main program.  
✔ **New factories or pizza types** can be added **without modifying existing code**.  

---

## **📌 6. Advantages of Abstract Factory Pattern**
✅ **Encapsulation** – Factory logic is hidden from the client.  
✅ **Extensibility** – New factories and objects can be added **without modifying existing code**.  
✅ **Scalability** – Supports different **variations** of products dynamically.  

---

## **📌 7. Common Mistakes & How to Avoid Them**
❌ **Creating too many factory classes** – Use only when truly needed.  
❌ **Mixing unrelated object creation in one factory** – Keep related objects together.  
❌ **Tight coupling with concrete classes** – Always use **interfaces** and **factories** for flexibility.  

---

## **🔥 Hands-On Challenge**
✔ Modify the factory to **support pasta creation** alongside pizzas.  
✔ Implement an **Abstract Factory for an Electronics Store** that sells **Laptops and Phones**.  

---
