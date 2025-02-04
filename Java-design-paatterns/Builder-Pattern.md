# **🚀 Lesson 4: Builder Pattern – Constructing Complex Objects Step by Step**

The **Builder Pattern** is a **creational design pattern** used when **object creation is complex** and involves multiple steps. It allows you to **construct an object step by step** instead of creating large, messy constructors with too many parameters.

---

## **📌 1. What is the Builder Pattern?**
The **Builder Pattern**:
✔ **Separates object construction from its representation**.  
✔ **Avoids telescoping constructors** (too many constructor arguments).  
✔ **Provides step-by-step customization** of object creation.  

---

## **📌 2. When to Use the Builder Pattern?**
✔ When an object has **too many optional parameters**.  
✔ When you want **better readability** in object creation.  
✔ When object creation involves **multiple steps**.  
✔ When following the **Open/Closed Principle** (extending without modifying existing code).  

---

## **📌 3. Real-World Analogy – Ordering a Custom Burger 🍔**
Imagine you're at a **burger shop**:
- You can **customize** your burger by selecting ingredients (`cheese`, `lettuce`, `extra patty`).
- Instead of passing **all ingredients upfront**, you **add them one by one**.

In Java, this translates to **chaining method calls** instead of passing everything into a single constructor.

---

## **📌 4. Implementing the Builder Pattern in Java**
Let's build a **Pizza Builder** to construct pizzas step by step.

---

### **Step 1: Create the Product (Immutable Object)**
```java
public class Pizza {
    private final String size;
    private final boolean cheese;
    private final boolean pepperoni;
    private final boolean olives;

    private Pizza(PizzaBuilder builder) {
        this.size = builder.size;
        this.cheese = builder.cheese;
        this.pepperoni = builder.pepperoni;
        this.olives = builder.olives;
    }

    @Override
    public String toString() {
        return "Pizza [Size=" + size + ", Cheese=" + cheese + ", Pepperoni=" + pepperoni + ", Olives=" + olives + "]";
    }

    // Builder Class
    public static class PizzaBuilder {
        private final String size;  // Required
        private boolean cheese;     // Optional
        private boolean pepperoni;  // Optional
        private boolean olives;     // Optional

        // Constructor for required fields
        public PizzaBuilder(String size) {
            this.size = size;
        }

        // Setter methods for optional fields (returning builder for method chaining)
        public PizzaBuilder addCheese() {
            this.cheese = true;
            return this;
        }

        public PizzaBuilder addPepperoni() {
            this.pepperoni = true;
            return this;
        }

        public PizzaBuilder addOlives() {
            this.olives = true;
            return this;
        }

        // Build method to return final Pizza object
        public Pizza build() {
            return new Pizza(this);
        }
    }
}
```
✔ Uses **inner static class** for the builder.  
✔ The **main class (Pizza) is immutable**.  
✔ **Chaining methods** for readability.  

---

### **Step 2: Using the Builder Pattern in Client Code**
```java
public class Main {
    public static void main(String[] args) {
        // Creating a pizza step by step
        Pizza pizza1 = new Pizza.PizzaBuilder("Large")
                .addCheese()
                .addPepperoni()
                .build();

        Pizza pizza2 = new Pizza.PizzaBuilder("Medium")
                .addOlives()
                .build();

        System.out.println(pizza1);
        System.out.println(pizza2);
    }
}
```

**💡 Output:**
```
Pizza [Size=Large, Cheese=true, Pepperoni=true, Olives=false]
Pizza [Size=Medium, Cheese=false, Pepperoni=false, Olives=true]
```

✔ **Readable and flexible** object creation.  
✔ **Avoids telescoping constructors**.  
✔ **Objects are immutable**, ensuring **thread safety**.  

---

## **📌 5. Advantages of the Builder Pattern**
✅ **Readability** – Easy to understand object creation.  
✅ **Customizability** – Only required fields are mandatory.  
✅ **Immutable Objects** – Thread-safe by default.  
✅ **Prevents Constructor Overload Issues** – No need for multiple constructors with many parameters.  

---

## **📌 6. Common Mistakes & How to Avoid Them**
❌ **Not using the final keyword in immutable classes** – Always use `final` fields.  
❌ **Allowing direct object instantiation** – The constructor should be `private`.  
❌ **Not providing a default build() method** – Always include `.build()` at the end.  

---

## **🔥 Hands-On Challenge**
✔ Extend the **PizzaBuilder** to allow adding **custom toppings** as a `List<String>`.  
✔ Implement a **CarBuilder** for a car dealership, allowing customers to customize their car.  

---
