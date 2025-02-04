# **🚀 Lesson 5: Prototype Pattern – Cloning Objects Efficiently**

The **Prototype Pattern** is a **creational design pattern** used when **creating a new object is costly** and **cloning an existing object is more efficient**.

---

## **📌 1. What is the Prototype Pattern?**
The **Prototype Pattern**:
✔ Allows **cloning objects** instead of creating them from scratch.  
✔ Uses a **prototype instance** to create new objects.  
✔ Reduces **performance overhead** when object creation is expensive.  

---

## **📌 2. When to Use the Prototype Pattern?**
✔ When **object creation is expensive** (e.g., loading data from a database).  
✔ When objects have **many configuration options** and should be **reused**.  
✔ When the system needs **to create copies of existing objects dynamically**.  
✔ When following the **Open/Closed Principle** (allows new object creation strategies without modifying existing code).  

---

## **📌 3. Real-World Analogy – Document Cloning 📄**
Think of a **resume template**:
- Instead of **writing a resume from scratch**, you **duplicate a template** and edit it.
- The **basic structure remains the same**, but you can **modify specific details**.

---

## **📌 4. Implementing the Prototype Pattern in Java**
Let's build a **Cloneable Shape Prototype**.

---

### **Step 1: Create an Interface for Cloning**
```java
public interface Prototype {
    Prototype clone();
}
```

---

### **Step 2: Implement Concrete Prototype Classes**
#### **Rectangle Class**
```java
public class Rectangle implements Prototype {
    private int width;
    private int height;
    private String color;

    public Rectangle(int width, int height, String color) {
        this.width = width;
        this.height = height;
        this.color = color;
    }

    // Copy Constructor
    public Rectangle(Rectangle rectangle) {
        this.width = rectangle.width;
        this.height = rectangle.height;
        this.color = rectangle.color;
    }

    @Override
    public Prototype clone() {
        return new Rectangle(this); // Cloning via Copy Constructor
    }

    @Override
    public String toString() {
        return "Rectangle [Width=" + width + ", Height=" + height + ", Color=" + color + "]";
    }
}
```

✔ Uses **Copy Constructor** for cloning.  
✔ Maintains **object properties** in the cloned instance.  

---

### **Step 3: Implement the Client Code**
```java
public class Main {
    public static void main(String[] args) {
        // Create an original rectangle
        Rectangle original = new Rectangle(10, 20, "Red");
        System.out.println("Original: " + original);

        // Clone the original rectangle
        Rectangle cloned = (Rectangle) original.clone();
        System.out.println("Cloned: " + cloned);
    }
}
```

---

## **📌 5. Expected Output**
```
Original: Rectangle [Width=10, Height=20, Color=Red]
Cloned: Rectangle [Width=10, Height=20, Color=Red]
```
✔ The **cloned object** is identical to the original.  
✔ No need to **create a new instance manually**.  

---

## **📌 6. Prototype Pattern Using `Cloneable` Interface**
Java provides a built-in **`Cloneable` interface**, but it has some drawbacks (deep cloning is not automatic).

### **Implementing `Cloneable` Interface**
```java
public class Circle implements Cloneable {
    private int radius;
    private String color;

    public Circle(int radius, String color) {
        this.radius = radius;
        this.color = color;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone(); // Uses Java's built-in cloning
    }

    @Override
    public String toString() {
        return "Circle [Radius=" + radius + ", Color=" + color + "]";
    }
}
```

---

### **Using the Cloneable Circle**
```java
public class Main {
    public static void main(String[] args) throws CloneNotSupportedException {
        Circle original = new Circle(15, "Blue");
        System.out.println("Original: " + original);

        Circle cloned = (Circle) original.clone();
        System.out.println("Cloned: " + cloned);
    }
}
```

✔ Uses **Java's built-in `clone()` method**.  
✔ Must handle **`CloneNotSupportedException`**.  

---

## **📌 7. Shallow Copy vs. Deep Copy**
| **Feature** | **Shallow Copy** | **Deep Copy** |
|------------|----------------|----------------|
| **Copies Object** | ✅ Yes | ✅ Yes |
| **Copies Object References** | ✅ Yes | ❌ No (Creates new instances) |
| **Modifying Clone Affects Original?** | ✅ Yes | ❌ No |
| **Performance** | ✅ Fast | ❌ Slightly Slower |

### **Deep Copy Example (Manually Copying Nested Objects)**
```java
public class Address {
    String city;

    public Address(String city) {
        this.city = city;
    }

    public Address(Address address) {
        this.city = address.city; // Manually copying
    }
}

public class Person implements Cloneable {
    String name;
    Address address;

    public Person(String name, Address address) {
        this.name = name;
        this.address = address;
    }

    // Deep Copy using Cloneable
    @Override
    protected Person clone() {
        return new Person(this.name, new Address(this.address));
    }

    @Override
    public String toString() {
        return "Person [Name=" + name + ", City=" + address.city + "]";
    }
}
```

---

### **Testing Deep Copy**
```java
public class Main {
    public static void main(String[] args) {
        Person original = new Person("Alice", new Address("New York"));
        Person cloned = original.clone();

        cloned.address.city = "Los Angeles"; // Modifying clone

        System.out.println("Original: " + original);
        System.out.println("Cloned: " + cloned);
    }
}
```

**💡 Output:**
```
Original: Person [Name=Alice, City=New York]
Cloned: Person [Name=Alice, City=Los Angeles]
```
✔ The **original object remains unchanged** (Deep Copy successful).  
✔ If we had used **Shallow Copy**, the original city would have also changed.  

---

## **📌 8. Advantages of the Prototype Pattern**
✅ **Reduces Object Creation Cost** – Useful when object creation is expensive.  
✅ **Improves Performance** – Faster than constructing new objects from scratch.  
✅ **Provides a Flexible Cloning Mechanism** – Allows deep or shallow copying.  
✅ **Encapsulates Object Creation Details** – Clients do not need to know object creation details.  

---

## **📌 9. Common Mistakes & How to Avoid Them**
❌ **Not Implementing Clone Properly** – Always override `clone()` method correctly.  
❌ **Using Shallow Copy When Deep Copy is Needed** – Nested objects should be copied manually.  
❌ **Forgetting to Implement `Cloneable`** – If using Java's built-in `clone()`, implement `Cloneable`.  

---

## **🔥 Hands-On Challenge**
✔ Implement a **Car Prototype** that allows cloning a car with different customizations.  
✔ Modify the `Circle` class to support **Deep Cloning** by copying the color reference manually.  

---
