# **🚀 Lesson 7: Decorator Pattern – Dynamically Adding Functionality**

The **Decorator Pattern** is a **structural design pattern** that allows adding **new behaviors to objects dynamically** **without modifying their structure**.

---

## **📌 1. What is the Decorator Pattern?**
The **Decorator Pattern**:
✔ **Extends functionality** of an object **at runtime**.  
✔ Uses **composition** instead of inheritance.  
✔ **Wraps an existing object** and adds new behavior dynamically.  

---

## **📌 2. When to Use the Decorator Pattern?**
✔ When you need to **add behavior dynamically at runtime**.  
✔ When **modifying existing code is not an option** (e.g., third-party libraries).  
✔ When you need **multiple combinations** of functionality.  
✔ When you want to **follow the Open/Closed Principle** (new features without modifying old code).  

---

## **📌 3. Real-World Analogy – Coffee Customization ☕**
Imagine a **coffee shop** where you can customize your coffee:
- **Base Coffee**: Plain coffee ☕.
- **Add Sugar**: Coffee + Sugar 🍚.
- **Add Milk**: Coffee + Milk 🥛.
- **Add Chocolate**: Coffee + Chocolate 🍫.

Instead of **modifying the Coffee class**, we **wrap** it with new behaviors dynamically.

---

## **📌 4. Implementing Decorator Pattern in Java**
Let’s build a **Coffee Customization System** using the Decorator Pattern.

---

### **Step 1: Create the Component Interface**
This is the **base interface** for coffee.

```java
// Component Interface
public interface Coffee {
    String getDescription();
    double getCost();
}
```

---

### **Step 2: Implement the Concrete Component**
This is the **plain coffee** without any extra features.

```java
// Concrete Component
public class SimpleCoffee implements Coffee {
    @Override
    public String getDescription() {
        return "Simple Coffee";
    }

    @Override
    public double getCost() {
        return 5.0;
    }
}
```
✔ **Base class** that provides the original coffee.  
✔ Costs **$5** by default.  

---

### **Step 3: Create the Decorator Class**
The **Decorator Class** wraps the original object.

```java
// Decorator Class (Base for all decorators)
public abstract class CoffeeDecorator implements Coffee {
    protected Coffee coffee; // Wrapped object

    public CoffeeDecorator(Coffee coffee) {
        this.coffee = coffee;
    }

    @Override
    public String getDescription() {
        return coffee.getDescription(); // Forwarding to the wrapped object
    }

    @Override
    public double getCost() {
        return coffee.getCost(); // Forwarding to the wrapped object
    }
}
```
✔ Holds a **reference to the base Coffee object**.  
✔ Passes method calls to the **wrapped object**.  

---

### **Step 4: Implement Concrete Decorators**
#### **Adding Milk to Coffee**
```java
public class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return super.getDescription() + ", Milk";
    }

    @Override
    public double getCost() {
        return super.getCost() + 1.5; // Adds $1.5 for milk
    }
}
```
✔ Adds **Milk** dynamically.  
✔ Increases **cost by $1.5**.  

#### **Adding Sugar to Coffee**
```java
public class SugarDecorator extends CoffeeDecorator {
    public SugarDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return super.getDescription() + ", Sugar";
    }

    @Override
    public double getCost() {
        return super.getCost() + 0.5; // Adds $0.5 for sugar
    }
}
```
✔ Adds **Sugar** dynamically.  
✔ Increases **cost by $0.5**.  

---

### **Step 5: Using the Decorator Pattern**
```java
public class Main {
    public static void main(String[] args) {
        // Base coffee
        Coffee coffee = new SimpleCoffee();
        System.out.println(coffee.getDescription() + " → $" + coffee.getCost());

        // Add Milk
        coffee = new MilkDecorator(coffee);
        System.out.println(coffee.getDescription() + " → $" + coffee.getCost());

        // Add Sugar
        coffee = new SugarDecorator(coffee);
        System.out.println(coffee.getDescription() + " → $" + coffee.getCost());
    }
}
```

---

## **📌 5. Expected Output**
```
Simple Coffee → $5.0
Simple Coffee, Milk → $6.5
Simple Coffee, Milk, Sugar → $7.0
```
✔ **Milk and Sugar are added dynamically**.  
✔ **No need to modify existing Coffee classes**.  

---

## **📌 6. Key Features of the Decorator Pattern**
| **Feature** | **Explanation** |
|------------|----------------|
| **Encapsulation** | Wraps an object to add new behavior dynamically. |
| **Open/Closed Principle** | Allows adding new features **without modifying** existing code. |
| **Flexible Composition** | Can apply multiple decorators **in any order**. |
| **Better than Inheritance** | Avoids subclass explosion (e.g., `MilkCoffee`, `SugarCoffee`, `MilkSugarCoffee`). |

---

## **📌 7. Common Mistakes & How to Avoid Them**
❌ **Modifying the base class instead of using a decorator** – Use **composition, not inheritance**.  
❌ **Not forwarding method calls in the decorator** – Always call `super.getDescription()` and `super.getCost()`.  
❌ **Using too many nested decorators** – If too many wrappers are applied, consider using **Builder Pattern** instead.  

---

## **🔥 Hands-On Challenge**
✔ Implement a **Pizza Decorator** where toppings (Cheese, Olives, Pepperoni) can be added dynamically.  
✔ Implement a **Logger Decorator** that adds **timestamp and log level** to existing logs.  

---

## **🚀 Summary**
| **Concept** | **Explanation** |
|------------|----------------|
| **Decorator Pattern** | Adds new behavior to objects dynamically **without modifying them**. |
| **Base Component** | Defines the interface (e.g., `Coffee`). |
| **Concrete Component** | The core object (e.g., `SimpleCoffee`). |
| **Decorator Class** | A wrapper that adds new functionality (e.g., `MilkDecorator`). |
| **Example** | Coffee customization (Milk, Sugar, Chocolate). |

---
