## 🚀 **Module 19: Mastering Design Patterns**  

Design patterns are proven solutions to recurring software design problems. They provide a standard way to organize code, improve maintainability, and promote reusability. This module covers the most important design patterns, their implementations in Java, and practical applications.

---

## **🔥 19.1 Why Learn Design Patterns?**  
- **Maintainable Code:** Write modular, maintainable, and reusable code.  
- **Consistent Design:** Standardize design approaches across teams.  
- **Object-Oriented Design Principles:** Follow SOLID principles for robust software architecture.  
- **Efficient Communication:** Improve communication with team members using standard design terminology.  
- **Interview Preparation:** Frequently asked in system design and coding interviews.  

---

## **🔥 19.2 Types of Design Patterns**  
1. **Creational Patterns** — Deal with object creation mechanisms.  
    - **Singleton** — Ensure a class has only one instance.  
    - **Factory Method** — Create objects without exposing instantiation logic.  
    - **Abstract Factory** — Create families of related objects.  
    - **Builder** — Construct complex objects step by step.  
    - **Prototype** — Clone existing objects.  
2. **Structural Patterns** — Deal with object composition and structure.  
    - **Adapter** — Convert one interface to another.  
    - **Bridge** — Separate abstraction from implementation.  
    - **Composite** — Compose objects into tree structures.  
    - **Decorator** — Add behavior to objects dynamically.  
    - **Facade** — Provide a simplified interface to a complex system.  
    - **Flyweight** — Minimize memory usage by sharing objects.  
    - **Proxy** — Control access to an object.  
3. **Behavioral Patterns** — Deal with object communication and responsibility.  
    - **Chain of Responsibility** — Pass requests along a chain of handlers.  
    - **Command** — Encapsulate requests as objects.  
    - **Interpreter** — Define grammar for a language and an interpreter.  
    - **Iterator** — Sequentially access elements of a collection.  
    - **Mediator** — Centralize complex communications between objects.  
    - **Memento** — Capture and restore an object's internal state.  
    - **Observer** — Notify dependent objects of state changes.  
    - **State** — Allow an object to alter its behavior when its state changes.  
    - **Strategy** — Encapsulate algorithms within a class hierarchy.  
    - **Template Method** — Define the skeleton of an algorithm in a method.  
    - **Visitor** — Separate an algorithm from an object structure.  

---

## **🔥 19.3 Creational Patterns**  
### 📘 **1. Singleton Pattern**  
- **Definition:** Ensures a class has only one instance and provides a global point of access to it.  
- **Usage:**  
    - Database connections.  
    - Logging services.  
    - Caching and configuration management.  
- **Key Points:**  
    - Private constructor.  
    - Static method for object creation.  
    - Static instance variable.  
- **Thread Safety:** Double-checked locking is used for thread safety.  

---

### 📘 **Example Code: Singleton Pattern**  
```java
public class Singleton {
    // Private static variable of the single instance
    private static volatile Singleton instance;

    // Private constructor to prevent instantiation
    private Singleton() {
        System.out.println("Singleton Instance Created");
    }

    // Public static method to provide global access point
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }

    public void showMessage() {
        System.out.println("Hello from Singleton!");
    }

    public static void main(String[] args) {
        Singleton s1 = Singleton.getInstance();
        Singleton s2 = Singleton.getInstance();

        s1.showMessage();
        System.out.println("Are both instances same? " + (s1 == s2));
    }
}
```

---

### 📊 **Output:**  
```
Singleton Instance Created
Hello from Singleton!
Are both instances same? true
```

---

### 🔥 **Explanation:**  
- **Double-Checked Locking:** Ensures thread safety with lazy initialization.  
- **Private Constructor:** Prevents instantiation from outside the class.  
- **Static Method:** Provides a global access point.  
- **Time Complexity:** `O(1)` — Constant time.  
- **Space Complexity:** `O(1)` — Only one instance is created.  

---

### 📘 **Variants of Singleton Pattern**  
1. **Eager Initialization:** Instance is created at the time of class loading.  
2. **Lazy Initialization:** Instance is created when needed.  
3. **Bill Pugh Singleton:** Uses a static inner helper class.  
4. **Enum Singleton:** Most recommended, provides serialization safety.  

---

## **🔥 19.4 Factory Method Pattern**  
- **Definition:** Defines an interface for creating an object but lets subclasses decide which class to instantiate.  
- **Usage:**  
    - Class instantiation is complex or requires dynamic selection.  
    - Avoids direct instantiation with `new`.  
- **Key Points:**  
    - Factory class contains a method to create objects.  
    - Subclasses override the factory method to create specific objects.  
- **Examples:**  
    - `Calendar.getInstance()` in Java.  
    - `Logger.getLogger()` in logging frameworks.  

---

### 📘 **Example Code: Factory Method Pattern**  
```java
// Step 1: Create a Product Interface
interface Vehicle {
    void drive();
}

// Step 2: Concrete Product Classes
class Car implements Vehicle {
    @Override
    public void drive() {
        System.out.println("Driving a car...");
    }
}

class Bike implements Vehicle {
    @Override
    public void drive() {
        System.out.println("Riding a bike...");
    }
}

// Step 3: Factory Class
class VehicleFactory {
    // Factory Method
    public static Vehicle getVehicle(String type) {
        if ("car".equalsIgnoreCase(type)) {
            return new Car();
        } else if ("bike".equalsIgnoreCase(type)) {
            return new Bike();
        }
        throw new IllegalArgumentException("Unknown vehicle type");
    }
}

// Step 4: Client Code
public class FactoryMethodExample {
    public static void main(String[] args) {
        Vehicle vehicle1 = VehicleFactory.getVehicle("car");
        vehicle1.drive();

        Vehicle vehicle2 = VehicleFactory.getVehicle("bike");
        vehicle2.drive();
    }
}
```

---

### 📊 **Output:**  
```
Driving a car...
Riding a bike...
```

---

### 🔥 **Explanation:**  
- **Product Interface (`Vehicle`):** Defines the common interface for products.  
- **Concrete Products (`Car`, `Bike`):** Implement the product interface.  
- **Factory Class (`VehicleFactory`):** Contains a static factory method to create products.  
- **Time Complexity:** `O(1)` — Constant time for object creation.  
- **Space Complexity:** `O(1)` — Only one instance is created at a time.  

---

### 📘 **Advantages of Factory Method Pattern**  
- **Encapsulation:** Encapsulates object creation logic.  
- **Loose Coupling:** Decouples client code from specific implementations.  
- **Scalability:** Easily extendable to add new products without modifying existing code.  

---

## 🔥 **Next: Structural Design Patterns**  
Next, we will explore **Structural Design Patterns** like Adapter, Bridge, Composite, Decorator, and Facade.

---
