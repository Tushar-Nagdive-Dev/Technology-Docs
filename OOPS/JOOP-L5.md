### **Level 1: Foundation - Abstraction**  

---

## **1. What is Abstraction?**
**Abstraction** is one of the four fundamental principles of Object-Oriented Programming (OOP). It is the process of hiding complex implementation details and exposing only the necessary functionalities.

### **Key Points:**
- **Focus on "What" not "How":** Abstraction allows you to focus on what an object does rather than how it does it.
- **Reduce Complexity:** By hiding unnecessary details, abstraction simplifies the interaction with complex systems.
- **Implementation Hiding:** Internal workings are hidden from the outside world, ensuring security and flexibility.

---

## **2. Real-World Example:**
### **Example: Using a TV Remote**
- **What you see:** Buttons to power on/off, change channels, adjust volume.
- **What you don't see:** Internal circuit connections, how signals are processed, or how the TV screen adjusts brightness.
- **Abstraction in Action:** The complexity is hidden, and you only see a simple interface.

---

## **3. Why is Abstraction Important?**
1. **Simplifies Complex Systems:** By exposing only relevant details.
2. **Security and Protection:** Internal data and implementation are hidden.
3. **Maintainability and Flexibility:** Internal implementation can change without affecting how the user interacts with the object.
4. **Reduce Code Duplication:** Common behavior can be defined once and reused by multiple classes.

---

## **4. How to Achieve Abstraction in Java?**
In Java, **Abstraction** is achieved using:
1. **Abstract Classes** - Using the `abstract` keyword.
2. **Interfaces** - Using the `interface` keyword.

---

## **5. Abstract Classes in Java**
### **What is an Abstract Class?**
- An **Abstract Class** is a class that cannot be instantiated.
- It may contain:
  - Abstract methods (methods without a body, declared using `abstract` keyword).
  - Concrete methods (methods with a body).
  - Fields (attributes) and constructors.
- **Purpose:** To provide a common template for subclasses to implement specific details.

---

### **Example: Abstract Class**
Let's consider an example of a `Vehicle` class. Different types of vehicles (like `Car`, `Bike`) have common behaviors but are implemented differently.

```java
// Abstract Class
public abstract class Vehicle {
    // Common attribute
    protected String brand;

    // Constructor
    public Vehicle(String brand) {
        this.brand = brand;
    }

    // Abstract Method (No implementation)
    public abstract void startEngine();

    // Concrete Method (Has implementation)
    public void displayBrand() {
        System.out.println("Brand: " + brand);
    }
}
```

### **Concrete Class: Car**
```java
// Concrete Class extending Abstract Class
public class Car extends Vehicle {
    private int seats;

    // Constructor
    public Car(String brand, int seats) {
        super(brand);
        this.seats = seats;
    }

    // Implementing abstract method
    @Override
    public void startEngine() {
        System.out.println(brand + " Car engine starting with key ignition.");
    }

    // Additional method
    public void displayDetails() {
        displayBrand();
        System.out.println("Seats: " + seats);
    }
}
```

### **Concrete Class: Bike**
```java
// Concrete Class extending Abstract Class
public class Bike extends Vehicle {
    private String type;

    // Constructor
    public Bike(String brand, String type) {
        super(brand);
        this.type = type;
    }

    // Implementing abstract method
    @Override
    public void startEngine() {
        System.out.println(brand + " Bike engine starting with kick start.");
    }

    // Additional method
    public void displayDetails() {
        displayBrand();
        System.out.println("Type: " + type);
    }
}
```

### **Usage:**
```java
public class Main {
    public static void main(String[] args) {
        // Creating objects of Car and Bike
        Car car = new Car("Toyota", 4);
        Bike bike = new Bike("Yamaha", "Sport");

        System.out.println("Car Details:");
        car.displayDetails();
        car.startEngine();
        System.out.println();

        System.out.println("Bike Details:");
        bike.displayDetails();
        bike.startEngine();
    }
}
```

### **Output:**
```
Car Details:
Brand: Toyota
Seats: 4
Toyota Car engine starting with key ignition.

Bike Details:
Brand: Yamaha
Type: Sport
Yamaha Bike engine starting with kick start.
```

---

## **6. Key Features of Abstract Class:**
1. **Cannot be Instantiated:** You cannot create an object of an abstract class directly.
2. **Abstract Methods:** Must be implemented by the subclass.
3. **Concrete Methods:** Can be directly used by the subclass.
4. **Constructors:** Can have constructors but cannot be called directly (called via subclass).

### **Example of Illegal Instantiation:**
```java
// This will cause a compilation error
Vehicle vehicle = new Vehicle("Generic");
```

---

## **7. When to Use Abstract Class:**
1. When you want to provide a common base with some default implementation.
2. When you have shared state or behavior among multiple subclasses.
3. When you expect subclasses to provide specific implementation for abstract methods.

---

## **8. Interfaces in Java**
### **What is an Interface?**
- An **Interface** is a completely abstract class that is used to group related methods with empty bodies.
- In Java, an interface:
  - Cannot have instance fields.
  - Can have:
    - Abstract methods (implicitly public and abstract).
    - **Default methods** (with body) using the `default` keyword.
    - **Static methods** (with body) using the `static` keyword.
- **Purpose:** To achieve complete abstraction and multiple inheritance.

---

### **Example: Interface**
Let's define an interface `Engine` and implement it in multiple classes.

```java
// Interface
public interface Engine {
    // Abstract Method
    void start();

    // Default Method
    default void stop() {
        System.out.println("Engine stopped.");
    }
}
```

### **Implementation in Car Class:**
```java
public class Car implements Engine {
    private String brand;

    // Constructor
    public Car(String brand) {
        this.brand = brand;
    }

    // Implementing interface method
    @Override
    public void start() {
        System.out.println(brand + " Car engine starting with push button.");
    }
}
```

### **Implementation in Bike Class:**
```java
public class Bike implements Engine {
    private String brand;

    // Constructor
    public Bike(String brand) {
        this.brand = brand;
    }

    // Implementing interface method
    @Override
    public void start() {
        System.out.println(brand + " Bike engine starting with self-start.");
    }
}
```

### **Usage:**
```java
public class Main {
    public static void main(String[] args) {
        Engine car = new Car("Honda");
        car.start();
        car.stop();

        Engine bike = new Bike("Ducati");
        bike.start();
        bike.stop();
    }
}
```

### **Output:**
```
Honda Car engine starting with push button.
Engine stopped.
Ducati Bike engine starting with self-start.
Engine stopped.
```

---

## **9. Difference between Abstract Class and Interface**

| **Abstract Class**                        | **Interface**                           |
|--------------------------------------------|------------------------------------------|
| Can have both abstract and concrete methods. | Only abstract methods (until Java 8).    |
| Can have instance variables.               | Cannot have instance variables.           |
| Supports single inheritance.               | Supports multiple inheritance.            |
| Used for sharing common code among subclasses. | Used for defining a contract for classes. |

---

## **10. Exercise: Mastering Abstraction**
### **Task: Create an `Appliance` Abstract Class and an `ElectronicDevice` Interface**
- **Abstract Class: `Appliance`**
  - `brand`, `power` as attributes.
  - `turnOn()` as an abstract method.
  - `turnOff()` as a concrete method.
- **Interface: `ElectronicDevice`**
  - `connectToPower()` as an abstract method.
  - `default void disconnectFromPower()` to print "Disconnected from power."
- Implement the classes:
  - `WashingMachine` and `Refrigerator` using `Appliance` and `ElectronicDevice`.

---

Here's a Java implementation that includes an abstract `Appliance` class, an `ElectronicDevice` interface, and two concrete classes (`WashingMachine` and `Refrigerator`) implementing both:

```java
// Abstract Class: Appliance
abstract class Appliance {
    private String brand;
    private int power; // in watts

    // Constructor
    public Appliance(String brand, int power) {
        this.brand = brand;
        this.power = power;
    }

    // Abstract method
    public abstract void turnOn();

    // Concrete method
    public void turnOff() {
        System.out.println(brand + " appliance is turned off.");
    }

    // Getters (for accessing private fields if needed)
    public String getBrand() {
        return brand;
    }

    public int getPower() {
        return power;
    }
}

// Interface: ElectronicDevice
interface ElectronicDevice {
    void connectToPower();

    default void disconnectFromPower() {
        System.out.println("Disconnected from power.");
    }
}

// Concrete Class: WashingMachine
class WashingMachine extends Appliance implements ElectronicDevice {
    private boolean isRunning;

    public WashingMachine(String brand, int power) {
        super(brand, power);
        this.isRunning = false;
    }

    @Override
    public void turnOn() {
        if (!isRunning) {
            System.out.println(getBrand() + " washing machine is turned on. Power consumption: " + getPower() + "W");
            isRunning = true;
        } else {
            System.out.println(getBrand() + " washing machine is already running.");
        }
    }

    @Override
    public void connectToPower() {
        System.out.println(getBrand() + " washing machine is connected to power.");
    }

    // Override turnOff for additional behavior
    @Override
    public void turnOff() {
        if (isRunning) {
            super.turnOff();
            isRunning = false;
        } else {
            System.out.println(getBrand() + " washing machine is already off.");
        }
    }
}

// Concrete Class: Refrigerator
class Refrigerator extends Appliance implements ElectronicDevice {
    private boolean isCooling;

    public Refrigerator(String brand, int power) {
        super(brand, power);
        this.isCooling = false;
    }

    @Override
    public void turnOn() {
        if (!isCooling) {
            System.out.println(getBrand() + " refrigerator is turned on. Power consumption: " + getPower() + "W");
            isCooling = true;
        } else {
            System.out.println(getBrand() + " refrigerator is already running.");
        }
    }

    @Override
    public void connectToPower() {
        System.out.println(getBrand() + " refrigerator is connected to power.");
    }

    // Override turnOff for additional behavior
    @Override
    public void turnOff() {
        if (isCooling) {
            super.turnOff();
            isCooling = false;
        } else {
            System.out.println(getBrand() + " refrigerator is already off.");
        }
    }
}

// Example usage
class Main {
    public static void main(String[] args) {
        // Create a WashingMachine
        WashingMachine washer = new WashingMachine("LG", 500);
        System.out.println("Washing Machine Demo:");
        washer.connectToPower();
        washer.turnOn();
        washer.turnOn(); // Already on
        washer.turnOff();
        washer.disconnectFromPower();
        System.out.println();

        // Create a Refrigerator
        Refrigerator fridge = new Refrigerator("Samsung", 150);
        System.out.println("Refrigerator Demo:");
        fridge.connectToPower();
        fridge.turnOn();
        fridge.turnOff();
        fridge.turnOff(); // Already off
        fridge.disconnectFromPower();
    }
}
```

### Explanation:

1. **Abstract Class: `Appliance`**:
   - **Attributes**: `brand` (String) and `power` (int) are private, with public getters for access.
   - **Abstract Method**: `turnOn()` must be implemented by subclasses.
   - **Concrete Method**: `turnOff()` provides a default behavior that prints a shutdown message.

2. **Interface: `ElectronicDevice`**:
   - **Abstract Method**: `connectToPower()` must be implemented by classes that implement this interface.
   - **Default Method**: `disconnectFromPower()` provides a default implementation that prints "Disconnected from power."

3. **Concrete Class: `WashingMachine`**:
   - Extends `Appliance` and implements `ElectronicDevice`.
   - Adds `isRunning` to track state.
   - Implements `turnOn()` to start the machine if not already running.
   - Overrides `turnOff()` to update state and call the parent’s method.
   - Implements `connectToPower()` with a custom message.

4. **Concrete Class: `Refrigerator`**:
   - Similarly extends `Appliance` and implements `ElectronicDevice`.
   - Uses `isCooling` to track state.
   - Implements `turnOn()`, overrides `turnOff()`, and implements `connectToPower()` with refrigerator-specific behavior.

### Sample Output:
```
Washing Machine Demo:
LG washing machine is connected to power.
LG washing machine is turned on. Power consumption: 500W
LG washing machine is already running.
LG appliance is turned off.
Disconnected from power.

Refrigerator Demo:
Samsung refrigerator is connected to power.
Samsung refrigerator is turned on. Power consumption: 150W
Samsung appliance is turned off.
Samsung refrigerator is already off.
Disconnected from power.
```

This implementation adheres to object-oriented principles, including abstraction, inheritance, and interface implementation. The use of state variables (`isRunning`, `isCooling`) enhances the realism of the appliance behavior. 
