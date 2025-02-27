### **Level 2: Intermediate - SOLID Design Principles**  

---

## **1. What are SOLID Principles?**
**SOLID** is an acronym representing five design principles that ensure scalable, maintainable, and flexible object-oriented design. These principles are the foundation of clean code and robust software architecture.

### **Key Points:**
- Ensures code **maintainability**, **scalability**, and **reusability**.
- Promotes **loose coupling** and **high cohesion**.
- Makes the code easier to understand, test, and extend.

---

## **2. What does SOLID stand for?**

| **SOLID Principle**         | **Description**                                          |
|-----------------------------|----------------------------------------------------------|
| **S** - Single Responsibility Principle (SRP) | A class should have only one reason to change. |
| **O** - Open/Closed Principle (OCP)            | Classes should be open for extension, but closed for modification. |
| **L** - Liskov Substitution Principle (LSP)    | Subtypes must be substitutable for their base types. |
| **I** - Interface Segregation Principle (ISP)  | Clients should not be forced to implement methods they don't use. |
| **D** - Dependency Inversion Principle (DIP)   | High-level modules should not depend on low-level modules. Both should depend on abstractions. |

---

## **3. Single Responsibility Principle (SRP)**
### **Definition:**
A class should have only **one reason to change**, meaning it should only have one job or responsibility.

### **Why SRP is Important:**
- Increases **maintainability** and **testability**.
- Promotes **reusability** by separating responsibilities.
- Changes in one responsibility do not affect others.

### **Violation Example:**
```java
public class Invoice {
    private int amount;

    public Invoice(int amount) {
        this.amount = amount;
    }

    public void printInvoice() {
        System.out.println("Printing Invoice of Amount: " + amount);
    }

    public void saveToDatabase() {
        System.out.println("Saving Invoice to Database");
    }
}
```
### **Problem:**
- `Invoice` class has two responsibilities:
  - **Business Logic** (Handling the invoice)
  - **Persistence** (Saving to the database)
- Changes in one responsibility (e.g., saving to a file instead of a database) require modification of the class, violating SRP.

---

### **Solution: Refactor using SRP**
- Separate the responsibilities into different classes.

```java
public class Invoice {
    private int amount;

    public Invoice(int amount) {
        this.amount = amount;
    }

    public int getAmount() {
        return amount;
    }
}
```

```java
public class InvoicePrinter {
    public void printInvoice(Invoice invoice) {
        System.out.println("Printing Invoice of Amount: " + invoice.getAmount());
    }
}
```

```java
public class InvoiceRepository {
    public void saveToDatabase(Invoice invoice) {
        System.out.println("Saving Invoice of Amount: " + invoice.getAmount() + " to Database");
    }
}
```

### **Usage:**
```java
public class Main {
    public static void main(String[] args) {
        Invoice invoice = new Invoice(100);
        InvoicePrinter printer = new InvoicePrinter();
        InvoiceRepository repository = new InvoiceRepository();

        printer.printInvoice(invoice);
        repository.saveToDatabase(invoice);
    }
}
```

### **Output:**
```
Printing Invoice of Amount: 100
Saving Invoice of Amount: 100 to Database
```

### **What's Improved?**
- **Invoice** handles only the business logic.
- **InvoicePrinter** is responsible for printing.
- **InvoiceRepository** is responsible for saving.
- Each class has a **single responsibility** and **one reason to change**.

---

## **4. Open/Closed Principle (OCP)**
### **Definition:**
A class should be **open for extension** but **closed for modification**.
- New functionality can be added by extending the class, but existing code should not be modified.

### **Why OCP is Important:**
- Promotes **extensibility** without modifying existing code.
- Reduces the risk of breaking existing functionality.

---

### **Violation Example:**
```java
public class NotificationService {
    public void sendNotification(String message, String type) {
        if (type.equals("EMAIL")) {
            System.out.println("Sending Email: " + message);
        } else if (type.equals("SMS")) {
            System.out.println("Sending SMS: " + message);
        }
    }
}
```

### **Problem:**
- If a new notification type (e.g., Push Notification) is added, the class must be modified.
- Violates OCP as the class is not **closed for modification**.

---

### **Solution: Refactor using OCP**
- Use **Abstraction** and **Polymorphism** to extend functionality without modifying the existing code.

### **Step 1: Create an Interface**
```java
public interface Notification {
    void send(String message);
}
```

### **Step 2: Implement Concrete Classes**
```java
public class EmailNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending Email: " + message);
    }
}
```

```java
public class SMSNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending SMS: " + message);
    }
}
```

### **Step 3: NotificationService Class**
```java
public class NotificationService {
    public void sendNotification(Notification notification, String message) {
        notification.send(message);
    }
}
```

### **Step 4: Usage**
```java
public class Main {
    public static void main(String[] args) {
        NotificationService service = new NotificationService();
        
        Notification email = new EmailNotification();
        Notification sms = new SMSNotification();
        
        service.sendNotification(email, "Hello via Email!");
        service.sendNotification(sms, "Hello via SMS!");
    }
}
```

### **Output:**
```
Sending Email: Hello via Email!
Sending SMS: Hello via SMS!
```

### **Adding New Notification Type:**
- To add **PushNotification**, create a new class:

```java
public class PushNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending Push Notification: " + message);
    }
}
```

### **Usage:**
```java
Notification push = new PushNotification();
service.sendNotification(push, "Hello via Push Notification!");
```

### **Output:**
```
Sending Push Notification: Hello via Push Notification!
```

### **What's Improved?**
- **NotificationService** is **closed for modification** but **open for extension**.
- New notification types can be added without modifying existing code.

---

## **5. What's Next?**
We’ve now mastered:
- **Single Responsibility Principle (SRP)**: One class, one responsibility.
- **Open/Closed Principle (OCP)**: Extend without modifying existing code.

### Next Up:
1. **Liskov Substitution Principle (LSP)**
   - Subtypes should be substitutable for their base types.
   - How to implement and maintain consistency in inheritance.
2. **Interface Segregation Principle (ISP)**
   - Avoid forcing clients to implement unnecessary methods.
   - Best practices for designing interfaces.
3. **Dependency Inversion Principle (DIP)**
   - High-level modules should not depend on low-level modules.
   - Implementing DIP using Dependency Injection and Inversion of Control.

---

## **6. Exercise: Apply SRP and OCP**
### **Task: Create a `Shape` Hierarchy with SRP and OCP**
1. **Abstract Class: `Shape`**
   - `calculateArea()` as an abstract method.
2. **Concrete Classes:**
   - `Circle` with `radius` attribute.
   - `Rectangle` with `length` and `width` attributes.
   - Implement `calculateArea()` in each subclass.
3. **Requirements:**
   - Use **SRP**: Separate classes for calculation, display, and storage.
   - Use **OCP**: Add new shapes (e.g., `Triangle`) without modifying existing code.
   - Test the implementation with multiple shapes.

---

Here's a Java implementation of a Shape hierarchy that adheres to the **Single Responsibility Principle (SRP)** and **Open-Closed Principle (OCP)**:

- **SRP**: Responsibilities are separated into different classes (calculation in `Shape` subclasses, display in a `ShapeDisplay` class, and storage in a `ShapeStorage` class).
- **OCP**: The design allows adding new shapes (like `Triangle`) without modifying existing classes, only extending the hierarchy.

```java
// Abstract Class: Shape (responsible for area calculation)
abstract class Shape {
    public abstract double calculateArea();
}

// Concrete Class: Circle
class Circle extends Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }

    public double getRadius() {
        return radius;
    }
}

// Concrete Class: Rectangle
class Rectangle extends Shape {
    private double length;
    private double width;

    public Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }

    @Override
    public double calculateArea() {
        return length * width;
    }

    public double getLength() {
        return length;
    }

    public double getWidth() {
        return width;
    }
}

// New Shape: Triangle (demonstrates OCP)
class Triangle extends Shape {
    private double base;
    private double height;

    public Triangle(double base, double height) {
        this.base = base;
        this.height = height;
    }

    @Override
    public double calculateArea() {
        return 0.5 * base * height;
    }

    public double getBase() {
        return base;
    }

    public double getHeight() {
        return height;
    }
}

// Class for Display (SRP: responsible only for displaying shapes)
class ShapeDisplay {
    public void display(Shape shape) {
        if (shape instanceof Circle) {
            Circle circle = (Circle) shape;
            System.out.printf("Circle (radius: %.2f) - Area: %.2f%n", circle.getRadius(), shape.calculateArea());
        } else if (shape instanceof Rectangle) {
            Rectangle rectangle = (Rectangle) shape;
            System.out.printf("Rectangle (length: %.2f, width: %.2f) - Area: %.2f%n", 
                            rectangle.getLength(), rectangle.getWidth(), shape.calculateArea());
        } else if (shape instanceof Triangle) {
            Triangle triangle = (Triangle) shape;
            System.out.printf("Triangle (base: %.2f, height: %.2f) - Area: %.2f%n", 
                            triangle.getBase(), triangle.getHeight(), shape.calculateArea());
        } else {
            System.out.println("Unknown shape - Area: " + shape.calculateArea());
        }
    }
}

// Class for Storage (SRP: responsible only for storing shapes)
class ShapeStorage {
    private java.util.List<Shape> shapes;

    public ShapeStorage() {
        this.shapes = new java.util.ArrayList<>();
    }

    public void addShape(Shape shape) {
        shapes.add(shape);
    }

    public java.util.List<Shape> getShapes() {
        return shapes;
    }
}

// Test Class
class Main {
    public static void main(String[] args) {
        // Create instances of shapes
        Circle circle = new Circle(5.0);
        Rectangle rectangle = new Rectangle(4.0, 6.0);
        Triangle triangle = new Triangle(3.0, 4.0);

        // Create storage and display objects
        ShapeStorage storage = new ShapeStorage();
        ShapeDisplay display = new ShapeDisplay();

        // Add shapes to storage
        storage.addShape(circle);
        storage.addShape(rectangle);
        storage.addShape(triangle);

        // Display all shapes
        System.out.println("Displaying all stored shapes:");
        for (Shape shape : storage.getShapes()) {
            display.display(shape);
        }
    }
}
```

### Explanation:

1. **Abstract Class: `Shape`**:
   - Defines only the `calculateArea()` method as abstract, adhering to SRP by focusing solely on area calculation logic.

2. **Concrete Classes (`Circle`, `Rectangle`, `Triangle`)**:
   - Each class has its own attributes (`radius`, `length`/`width`, `base`/`height`) and implements `calculateArea()` specific to its geometry.
   - Getters are provided for display purposes, keeping encapsulation intact.

3. **SRP Implementation**:
   - **Calculation**: Handled by `Shape` and its subclasses (`calculateArea`).
   - **Display**: Handled by `ShapeDisplay`, which knows how to print details for each shape type.
   - **Storage**: Handled by `ShapeStorage`, which manages a list of shapes.

4. **OCP Implementation**:
   - Adding a new shape (e.g., `Triangle`) requires only creating a new class that extends `Shape` and updating `ShapeDisplay` to recognize it (ideally, `ShapeDisplay` could be further abstracted, but this keeps it simple for demonstration).
   - Existing classes (`Circle`, `Rectangle`, `Shape`, etc.) remain unchanged when `Triangle` is added.

5. **Testing**:
   - The `Main` class creates instances of `Circle`, `Rectangle`, and `Triangle`.
   - Stores them in `ShapeStorage`.
   - Uses `ShapeDisplay` to show details in a loop, demonstrating polymorphic behavior.

### Sample Output:
```
Displaying all stored shapes:
Circle (radius: 5.00) - Area: 78.54
Rectangle (length: 4.00, width: 6.00) - Area: 24.00
Triangle (base: 3.00, height: 4.00) - Area: 6.00
```

### Key Points:
- **SRP**: Each class has a single responsibility:
  - `Shape` and subclasses: Calculate area.
  - `ShapeDisplay`: Display shape details.
  - `ShapeStorage`: Store shapes.
- **OCP**: The system is open for extension (new shapes like `Triangle`) but closed for modification (no need to change existing code except potentially `ShapeDisplay` for display logic, which could be further improved with a strategy pattern if needed).
- **Polymorphism**: The loop in `Main` uses a `Shape` reference to call `calculateArea()`, which resolves to the correct subclass implementation at runtime.
