### **Level 1: Foundation - Polymorphism**  

---

## **1. What is Polymorphism?**
**Polymorphism** is one of the four fundamental principles of Object-Oriented Programming (OOP). It allows methods to perform differently based on the object they are acting upon.

### **Key Points:**
- **Polymorphism** means "many forms."
- It enables a single method or operator to behave differently in different contexts.
- It promotes flexibility and maintainability in code.

---

## **2. Types of Polymorphism in Java**
1. **Compile-time Polymorphism (Static Polymorphism)**
   - Achieved using **Method Overloading**.
   - Method binding happens at compile time.
2. **Runtime Polymorphism (Dynamic Polymorphism)**
   - Achieved using **Method Overriding**.
   - Method binding happens at runtime using dynamic method dispatch.

---

## **3. Compile-time Polymorphism (Method Overloading)**
### **What is Method Overloading?**
- **Method Overloading** allows multiple methods in the same class to have the same name but different parameters.
- It increases the readability and flexibility of code.

### **Rules for Method Overloading:**
1. Methods must have the **same name**.
2. Methods must have **different parameters** (number, type, or order).
3. Return type can be the same or different, but it is not considered for overloading.

---

### **Example: Method Overloading**
Let's consider an example of a `Calculator` class with multiple `add()` methods.

```java
public class Calculator {
    // Method Overloading with different parameter types
    public int add(int a, int b) {
        return a + b;
    }

    public double add(double a, double b) {
        return a + b;
    }

    // Method Overloading with different number of parameters
    public int add(int a, int b, int c) {
        return a + b + c;
    }

    // Method Overloading with different parameter order
    public String add(String a, int b) {
        return a + b;
    }
}
```

### **Usage:**
```java
public class Main {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        System.out.println(calc.add(2, 3));             // Calls add(int, int)
        System.out.println(calc.add(2.5, 3.5));         // Calls add(double, double)
        System.out.println(calc.add(1, 2, 3));          // Calls add(int, int, int)
        System.out.println(calc.add("Number: ", 5));    // Calls add(String, int)
    }
}
```

### **Output:**
```
5
6.0
6
Number: 5
```

---

## **4. Runtime Polymorphism (Method Overriding)**
### **What is Method Overriding?**
- **Method Overriding** allows a subclass to provide a specific implementation of a method already defined in its superclass.
- It enables dynamic method dispatch at runtime.

### **Rules for Method Overriding:**
1. Methods must have the **same name**, **same parameters**, and **same return type**.
2. The method in the subclass should have the **same access level** or more accessible than the superclass.
3. **Static methods** cannot be overridden (they are hidden, not overridden).
4. **final** methods cannot be overridden.

---

### **Example: Method Overriding**
Let's consider an example of a `Vehicle` hierarchy with overridden methods.

### **Superclass: Vehicle**
```java
public class Vehicle {
    public void start() {
        System.out.println("Vehicle is starting.");
    }

    public void stop() {
        System.out.println("Vehicle is stopping.");
    }
}
```

### **Subclass: Car**
```java
public class Car extends Vehicle {
    // Overriding start() method
    @Override
    public void start() {
        System.out.println("Car is starting with key ignition.");
    }
}
```

### **Subclass: Bike**
```java
public class Bike extends Vehicle {
    // Overriding start() method
    @Override
    public void start() {
        System.out.println("Bike is starting with kick start.");
    }
}
```

### **Usage: Dynamic Method Dispatch**
```java
public class Main {
    public static void main(String[] args) {
        // Upcasting: Reference type is Vehicle, object is Car
        Vehicle myVehicle = new Car();
        myVehicle.start(); // Calls Car's start() due to runtime polymorphism
        myVehicle.stop();  // Calls Vehicle's stop()

        // Upcasting: Reference type is Vehicle, object is Bike
        myVehicle = new Bike();
        myVehicle.start(); // Calls Bike's start() due to runtime polymorphism
        myVehicle.stop();  // Calls Vehicle's stop()
    }
}
```

### **Output:**
```
Car is starting with key ignition.
Vehicle is stopping.
Bike is starting with kick start.
Vehicle is stopping.
```

---

## **5. Dynamic Method Dispatch**
- Dynamic Method Dispatch is the mechanism by which a call to an overridden method is resolved at runtime rather than compile-time.
- It enables **Runtime Polymorphism**.
- Java uses **method overriding** to achieve dynamic method dispatch.

---

## **6. Polymorphic Behavior**
- A **Superclass Reference** can point to a **Subclass Object**.
- Method calls on the superclass reference will execute the overridden method in the subclass.

### **Example: Polymorphism in Action**
```java
public class Main {
    public static void main(String[] args) {
        Vehicle vehicle;

        // Pointing to Car Object
        vehicle = new Car();
        vehicle.start(); // Calls Car's start()

        // Pointing to Bike Object
        vehicle = new Bike();
        vehicle.start(); // Calls Bike's start()
    }
}
```

### **Output:**
```
Car is starting with key ignition.
Bike is starting with kick start.
```

---

## **7. Why Use Polymorphism?**
1. **Flexibility and Extensibility:**
   - New subclasses can be added with minimal changes to existing code.
2. **Code Reusability:**
   - Common interfaces or base classes can be reused.
3. **Maintainability:**
   - Changes in subclass implementations do not affect the client code.
4. **Loose Coupling:**
   - Client code interacts with the base class or interface, not with specific implementations.

---

## **8. Common Mistakes to Avoid:**
1. **Confusing Overloading and Overriding:**
   - **Overloading** is compile-time (same method name, different parameters).
   - **Overriding** is runtime (same method name, same parameters, different implementation).
2. **Incorrect Access Modifiers:**
   - Overridden methods must have the same or a more accessible access level.
3. **Incorrect Return Type:**
   - Covariant return types are allowed in Java, but primitive return types must be identical.

---

## **9. Exercise: Mastering Polymorphism**
### **Task: Create a `Payment` System**
1. **Superclass: `Payment`**
   - `double amount` as an attribute.
   - `processPayment()` as an abstract method.
2. **Subclasses:**
   - `CreditCardPayment`
     - Implement `processPayment()` to display credit card processing.
   - `PayPalPayment`
     - Implement `processPayment()` to display PayPal processing.
   - `UPIPayment`
     - Implement `processPayment()` to display UPI processing.
3. **Requirements:**
   - Use **Method Overriding** for `processPayment()`.
   - Use **Polymorphism** to create a `Payment` reference for each type of payment.
   - Test polymorphic behavior by storing all payment objects in an array and processing them in a loop.

---

## **10. What's Next?**
We’ve now mastered **Polymorphism** with:
- Method Overloading and Overriding.
- Compile-time vs. Runtime Polymorphism.
- Dynamic Method Dispatch and Polymorphic Behavior.

### Next Up: **OOP Design Principles and Patterns**
- Understanding **SOLID Principles**.
- Applying **Creational, Structural, and Behavioral Design Patterns**.
- Real-world examples and exercises to implement these patterns.

---

Here's a Java implementation of the Payment system with the specified requirements:

```java
// Superclass: Payment
abstract class Payment {
    protected double amount;

    // Constructor
    public Payment(double amount) {
        this.amount = amount;
    }

    // Abstract method to be overridden by subclasses
    public abstract void processPayment();

    // Getter for amount (if needed)
    public double getAmount() {
        return amount;
    }
}

// Subclass: CreditCardPayment
class CreditCardPayment extends Payment {
    public CreditCardPayment(double amount) {
        super(amount);
    }

    // Override processPayment for credit card
    @Override
    public void processPayment() {
        System.out.printf("Processing Credit Card payment of $%.2f... Payment completed.%n", amount);
    }
}

// Subclass: PayPalPayment
class PayPalPayment extends Payment {
    public PayPalPayment(double amount) {
        super(amount);
    }

    // Override processPayment for PayPal
    @Override
    public void processPayment() {
        System.out.printf("Processing PayPal payment of $%.2f... Payment completed.%n", amount);
    }
}

// Subclass: UPIPayment
class UPIPayment extends Payment {
    public UPIPayment(double amount) {
        super(amount);
    }

    // Override processPayment for UPI
    @Override
    public void processPayment() {
        System.out.printf("Processing UPI payment of $%.2f... Payment completed.%n", amount);
    }
}

// Test Class
class Main {
    public static void main(String[] args) {
        // Create payment objects using polymorphism
        Payment creditCard = new CreditCardPayment(150.75);
        Payment payPal = new PayPalPayment(89.99);
        Payment upi = new UPIPayment(45.50);

        // Store payments in an array to demonstrate polymorphic behavior
        Payment[] payments = {creditCard, payPal, upi};

        // Process all payments in a loop
        System.out.println("Processing all payments:");
        for (Payment payment : payments) {
            payment.processPayment();
        }
    }
}
```

### Explanation:

1. **Superclass: `Payment`**:
   - **Attribute**: `amount` (protected double) allows subclasses to access it directly while still being encapsulated from external classes.
   - **Constructor**: Initializes `amount`.
   - **Abstract Method**: `processPayment()` is abstract, requiring subclasses to provide their own implementation.
   - **Getter**: `getAmount()` provides access to `amount` if needed.

2. **Subclass: `CreditCardPayment`**:
   - Extends `Payment` and uses `super(amount)` to set the amount.
   - Overrides `processPayment()` to simulate credit card processing with a custom message.

3. **Subclass: `PayPalPayment`**:
   - Similarly extends `Payment` and overrides `processPayment()` for PayPal-specific processing.

4. **Subclass: `UPIPayment`**:
   - Extends `Payment` and overrides `processPayment()` for UPI-specific processing.

5. **Polymorphism**:
   - Objects are created with `Payment` reference types (e.g., `Payment creditCard = new CreditCardPayment(150.75)`), demonstrating that a superclass reference can hold subclass instances.
   - An array of `Payment` objects is used to store all payment types, showcasing polymorphic behavior when calling `processPayment()`.

6. **Testing**:
   - The `Main` class creates instances of each payment type, stores them in a `Payment[]` array, and processes them in a loop.
   - The overridden `processPayment()` method of the actual object type is called at runtime, demonstrating dynamic polymorphism.

### Sample Output:
```
Processing all payments:
Processing Credit Card payment of $150.75... Payment completed.
Processing PayPal payment of $89.99... Payment completed.
Processing UPI payment of $45.50... Payment completed.
```

### Key Points:
- **Method Overriding**: Each subclass overrides `processPayment()` with its own implementation.
- **Polymorphism**: `Payment` references can refer to any subclass object, and the correct `processPayment()` is called based on the actual object type.
- **Array and Loop**: The array of `Payment` objects and the loop demonstrate how polymorphism simplifies processing multiple payment types uniformly.
