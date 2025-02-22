## 🚀 **Module 20: Structural Design Patterns**  

Structural design patterns focus on how classes and objects are composed to form larger structures. They provide flexible and efficient solutions for class and object composition, promoting code maintainability and scalability.

---

## **🔥 20.1 Why Learn Structural Design Patterns?**  
- **Modular Design:** Create complex systems by composing simpler objects.  
- **Maintainable Code:** Simplify complex relationships between classes.  
- **Scalable Architecture:** Easily add or modify functionality without changing existing code.  
- **Reusability:** Promote reusability through composition and delegation.  
- **Consistent Object Structure:** Ensure consistent object composition across the codebase.  

---

## **🔥 20.2 Types of Structural Design Patterns**  
1. **Adapter** — Convert one interface to another expected by the client.  
2. **Bridge** — Separate abstraction from implementation, allowing them to vary independently.  
3. **Composite** — Compose objects into tree structures to represent part-whole hierarchies.  
4. **Decorator** — Add behavior to objects dynamically without modifying their structure.  
5. **Facade** — Provide a simplified interface to a complex system of classes.  
6. **Flyweight** — Minimize memory usage by sharing common objects.  
7. **Proxy** — Control access to an object, adding additional functionality.  

---

## **🔥 20.3 Adapter Pattern**  
- **Definition:** Converts the interface of a class into another interface the client expects. It allows incompatible interfaces to work together.  
- **Usage:**  
    - Integrate legacy code with new systems.  
    - Use third-party libraries with incompatible interfaces.  
- **Types:**  
    - **Class Adapter:** Uses inheritance.  
    - **Object Adapter:** Uses composition.  
- **Examples:**  
    - `java.util.Arrays.asList()` — Converts an array to a list.  
    - `InputStreamReader` — Adapts `InputStream` to `Reader`.  

---

### 📘 **Example Code: Object Adapter Pattern**  
- **Scenario:** Integrate a legacy payment system with a new payment gateway.  
- **Problem:** The legacy system uses `processPayment()` while the new system uses `makePayment()`.  
- **Solution:** Use an Adapter to convert the legacy method call to the new method call.  

```java
// Step 1: Target Interface
interface PaymentGateway {
    void makePayment(double amount);
}

// Step 2: Adaptee (Legacy System)
class LegacyPayment {
    public void processPayment(double amount) {
        System.out.println("Processing payment of $" + amount + " using Legacy Payment System.");
    }
}

// Step 3: Adapter Class
class PaymentAdapter implements PaymentGateway {
    private LegacyPayment legacyPayment;

    public PaymentAdapter(LegacyPayment legacyPayment) {
        this.legacyPayment = legacyPayment;
    }

    @Override
    public void makePayment(double amount) {
        // Adapting processPayment to makePayment
        legacyPayment.processPayment(amount);
    }
}

// Step 4: Client Code
public class AdapterPatternExample {
    public static void main(String[] args) {
        LegacyPayment legacyPayment = new LegacyPayment();
        PaymentGateway paymentGateway = new PaymentAdapter(legacyPayment);

        // Client uses the new PaymentGateway interface
        paymentGateway.makePayment(100.50);
    }
}
```

---

### 📊 **Output:**  
```
Processing payment of $100.5 using Legacy Payment System.
```

---

### 🔥 **Explanation:**  
- **Target Interface (`PaymentGateway`):** Defines the new method (`makePayment`).  
- **Adaptee (`LegacyPayment`):** The legacy system with `processPayment()`.  
- **Adapter (`PaymentAdapter`):** Converts `makePayment()` to `processPayment()`.  
- **Time Complexity:** `O(1)` — Constant time for method call.  
- **Space Complexity:** `O(1)` — Only one adapter instance is created.  

---

### 📘 **When to Use Adapter Pattern?**  
- **Integrate Legacy Code:** Integrate legacy systems with new applications.  
- **Incompatible Interfaces:** Use third-party libraries with incompatible interfaces.  
- **Reusable Code:** Reuse existing code with a new interface.  

---

## **🔥 20.4 Bridge Pattern**  
- **Definition:** Decouples an abstraction from its implementation, allowing them to vary independently.  
- **Usage:**  
    - Avoid a complex inheritance hierarchy.  
    - Implement platform-independent code.  
    - Separate interface from implementation for flexibility.  
- **Examples:**  
    - `java.sql.DriverManager` — Abstraction for database connections.  
    - `AWT (Abstract Window Toolkit)` — Abstracts platform-specific implementations.  

---

### 📘 **Example Code: Bridge Pattern**  
- **Scenario:** Design a Drawing Application supporting different shapes and rendering platforms.  
- **Problem:** Shapes (Circle, Rectangle) can be rendered on different platforms (OpenGL, DirectX).  
- **Solution:** Separate shape abstraction from rendering implementation using the Bridge Pattern.  

```java
// Step 1: Implementor Interface
interface Renderer {
    void renderCircle(float radius);
    void renderRectangle(float width, float height);
}

// Step 2: Concrete Implementors
class OpenGLRenderer implements Renderer {
    @Override
    public void renderCircle(float radius) {
        System.out.println("Rendering Circle with radius " + radius + " using OpenGL.");
    }

    @Override
    public void renderRectangle(float width, float height) {
        System.out.println("Rendering Rectangle with width " + width + " and height " + height + " using OpenGL.");
    }
}

class DirectXRenderer implements Renderer {
    @Override
    public void renderCircle(float radius) {
        System.out.println("Rendering Circle with radius " + radius + " using DirectX.");
    }

    @Override
    public void renderRectangle(float width, float height) {
        System.out.println("Rendering Rectangle with width " + width + " and height " + height + " using DirectX.");
    }
}

// Step 3: Abstraction
abstract class Shape {
    protected Renderer renderer;

    protected Shape(Renderer renderer) {
        this.renderer = renderer;
    }

    public abstract void draw();
}

// Step 4: Refined Abstraction
class Circle extends Shape {
    private float radius;

    public Circle(Renderer renderer, float radius) {
        super(renderer);
        this.radius = radius;
    }

    @Override
    public void draw() {
        renderer.renderCircle(radius);
    }
}

class Rectangle extends Shape {
    private float width, height;

    public Rectangle(Renderer renderer, float width, float height) {
        super(renderer);
        this.width = width;
        this.height = height;
    }

    @Override
    public void draw() {
        renderer.renderRectangle(width, height);
    }
}

// Step 5: Client Code
public class BridgePatternExample {
    public static void main(String[] args) {
        Renderer opengl = new OpenGLRenderer();
        Renderer directx = new DirectXRenderer();

        Shape circle = new Circle(opengl, 5.0f);
        Shape rectangle = new Rectangle(directx, 4.0f, 6.0f);

        circle.draw();
        rectangle.draw();
    }
}
```

---

### 📊 **Output:**  
```
Rendering Circle with radius 5.0 using OpenGL.
Rendering Rectangle with width 4.0 and height 6.0 using DirectX.
```

---

### 🔥 **Explanation:**  
- **Implementor (`Renderer`):** Interface for rendering operations.  
- **Concrete Implementors (`OpenGLRenderer`, `DirectXRenderer`):** Platform-specific implementations.  
- **Abstraction (`Shape`):** Base class for all shapes.  
- **Refined Abstraction (`Circle`, `Rectangle`):** Specific shape classes that use rendering implementations.  
- **Time Complexity:** `O(1)` — Constant time for rendering.  
- **Space Complexity:** `O(1)` — Only one instance of each object is created.  

---

## 🔥 **Next: Behavioral Design Patterns**  
Next, we will explore **Behavioral Design Patterns** like Observer, Strategy, Command, and State.

---
