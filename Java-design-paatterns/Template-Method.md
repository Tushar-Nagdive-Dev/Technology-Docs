# **🚀 Lesson 18: Template Method Pattern – Defining Algorithm Steps in a Base Class**

The **Template Method Pattern** is a **behavioral design pattern** that defines **the skeleton of an algorithm** in a base class while allowing subclasses to **implement specific steps**.

---

## **📌 1. What is the Template Method Pattern?**
The **Template Method Pattern**:
✔ **Defines an algorithm’s structure in a base class**.  
✔ Allows **subclasses to customize specific steps** without modifying the overall flow.  
✔ **Reduces duplicate code** by reusing the algorithm structure.  

---

## **📌 2. When to Use the Template Method Pattern?**
✔ When multiple classes **share a common algorithm structure** but have different implementations for some steps.  
✔ When you want to **ensure a specific sequence of steps in an algorithm**.  
✔ When following the **Open/Closed Principle** (new implementations can be added without modifying existing code).  
✔ When implementing **workflows, report generation, or game AI behavior**.  

---

## **📌 3. Real-World Analogy – Making Tea vs. Coffee ☕**
1️⃣ **Boil Water** – Common step.  
2️⃣ **Add Ingredient** – Different for tea (tea leaves) and coffee (coffee powder).  
3️⃣ **Pour in Cup** – Common step.  
4️⃣ **Add Condiments** – Different for tea (lemon/honey) and coffee (milk/sugar).  

✔ The **Template Method Pattern ensures that the process remains the same**, while allowing customization for **tea vs. coffee**.  

---

## **📌 4. Implementing Template Method Pattern in Java**
Let’s build a **Beverage Preparation System** where different drinks **follow the same preparation steps** but customize ingredients.

---

### **Step 1: Create the Abstract Base Class**
This class defines the **algorithm template**.

```java
// Abstract Base Class
public abstract class Beverage {
    
    // Template Method - Defines the algorithm steps
    public final void prepareBeverage() {
        boilWater();
        addMainIngredient();
        pourInCup();
        addCondiments();
    }

    // Common steps (Implemented in base class)
    private void boilWater() {
        System.out.println("Boiling water...");
    }

    private void pourInCup() {
        System.out.println("Pouring into cup...");
    }

    // Steps to be customized by subclasses
    protected abstract void addMainIngredient();
    protected abstract void addCondiments();
}
```
✔ **Defines the template method `prepareBeverage()`** (final so it cannot be overridden).  
✔ **Implements common steps (`boilWater()`, `pourInCup()`)**.  
✔ **Leaves `addMainIngredient()` and `addCondiments()` for subclasses to define**.  

---

### **Step 2: Implement Concrete Classes**
Each class **implements the unique steps**.

#### **Tea Preparation**
```java
// Concrete Class 1: Tea
public class Tea extends Beverage {
    @Override
    protected void addMainIngredient() {
        System.out.println("Adding tea leaves...");
    }

    @Override
    protected void addCondiments() {
        System.out.println("Adding lemon and honey...");
    }
}
```

#### **Coffee Preparation**
```java
// Concrete Class 2: Coffee
public class Coffee extends Beverage {
    @Override
    protected void addMainIngredient() {
        System.out.println("Adding coffee powder...");
    }

    @Override
    protected void addCondiments() {
        System.out.println("Adding milk and sugar...");
    }
}
```
✔ **Each subclass customizes `addMainIngredient()` and `addCondiments()`**.  
✔ **The overall structure (boiling water, pouring in cup) remains unchanged**.  

---

### **Step 3: Using the Template Method Pattern**
```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Preparing Tea...");
        Beverage tea = new Tea();
        tea.prepareBeverage();

        System.out.println("\nPreparing Coffee...");
        Beverage coffee = new Coffee();
        coffee.prepareBeverage();
    }
}
```

---

## **📌 5. Expected Output**
```
Preparing Tea...
Boiling water...
Adding tea leaves...
Pouring into cup...
Adding lemon and honey...

Preparing Coffee...
Boiling water...
Adding coffee powder...
Pouring into cup...
Adding milk and sugar...
```
✔ **Both Tea and Coffee follow the same sequence of steps**.  
✔ **Each has different main ingredients and condiments**.  
✔ **No duplicate code for `boilWater()` and `pourInCup()`**.  

---

## **📌 6. Key Features of the Template Method Pattern**
| **Feature** | **Explanation** |
|------------|----------------|
| **Encapsulates Algorithm Steps** | Defines a **fixed sequence of operations**. |
| **Prevents Code Duplication** | Common steps are implemented in the **base class**. |
| **Supports Customization** | Subclasses override **only necessary steps**. |
| **Follows Open/Closed Principle** | New implementations can be added **without modifying existing code**. |

---

## **📌 7. Common Mistakes & How to Avoid Them**
❌ **Not making the template method `final`** – Prevent overriding to **ensure a fixed algorithm structure**.  
❌ **Forcing unnecessary steps on all subclasses** – Provide **default implementations** for optional steps.  
❌ **Breaking encapsulation** – Subclasses should **only override necessary steps**, not modify the overall flow.  

---

## **🔥 Hands-On Challenge**
✔ Implement a **Report Generation System** where `ExcelReport` and `PDFReport` share a common workflow but customize data formatting.  
✔ Implement a **Game Character AI System** where different characters (Knight, Mage) follow the same decision-making process but differ in attack strategies.  

---

## **🚀 Summary**
| **Concept** | **Explanation** |
|------------|----------------|
| **Template Method Pattern** | Defines a fixed **algorithm structure** in a base class. |
| **Template Method (`prepareBeverage()`)** | Ensures the **same process flow** for all subclasses. |
| **Abstract Steps (`addMainIngredient()`, `addCondiments()`)** | Subclasses implement these to customize behavior. |
| **Example** | **Tea vs. Coffee Preparation System**. |

---
