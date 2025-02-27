### **Level 2: Intermediate - Liskov Substitution Principle (LSP)**  

---

## **1. What is Liskov Substitution Principle (LSP)?**
**Liskov Substitution Principle (LSP)** states:
> *"Objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program."*

### **Key Points:**
- A subclass should **extend** the behavior of the superclass, not **narrow** it.
- A subclass should **honor the contract** established by the superclass.
- Ensures that derived classes are **substitutable** for their base classes.

---

## **2. Why is LSP Important?**
1. **Polymorphism and Substitutability:**
   - LSP ensures that polymorphism is safely applied.
   - Code that works with the base class should seamlessly work with derived classes.

2. **Maintainability and Extensibility:**
   - Promotes maintainability and scalability by ensuring consistent behavior across base and derived classes.

3. **Correctness and Consistency:**
   - Ensures that the application’s correctness is maintained even when using subclass objects.

---

## **3. Violation of LSP: Example and Problem Analysis**
### **Scenario: Rectangle and Square**
- A `Square` is a specialized type of `Rectangle`.
- It seems logical to model `Square` as a subclass of `Rectangle`.

### **Superclass: Rectangle**
```java
public class Rectangle {
    protected int width;
    protected int height;

    public Rectangle(int width, int height) {
        this.width = width;
        this.height = height;
    }

    public void setWidth(int width) {
        this.width = width;
    }

    public void setHeight(int height) {
        this.height = height;
    }

    public int getWidth() {
        return width;
    }

    public int getHeight() {
        return height;
    }

    public int getArea() {
        return width * height;
    }
}
```

### **Subclass: Square**
```java
public class Square extends Rectangle {
    public Square(int side) {
        super(side, side);
    }

    @Override
    public void setWidth(int width) {
        this.width = this.height = width;
    }

    @Override
    public void setHeight(int height) {
        this.width = this.height = height;
    }
}
```

### **Usage and Test Case:**
```java
public class Main {
    public static void main(String[] args) {
        Rectangle rect = new Rectangle(10, 20);
        Rectangle square = new Square(10);

        rect.setHeight(30);
        rect.setWidth(40);

        square.setHeight(30);

        System.out.println("Rectangle Area: " + rect.getArea());
        System.out.println("Square Area: " + square.getArea());
    }
}
```

### **Output:**
```
Rectangle Area: 1200
Square Area: 900
```

---

### **What's Wrong and Why LSP is Violated?**
- In the example:
  - `Square` overrides `setWidth()` and `setHeight()` to ensure the properties of a square (equal sides).
  - When `setHeight()` is called on `Square`, both height and width are changed, violating the expectation set by `Rectangle`.
  - A `Square` cannot be used interchangeably with a `Rectangle` because it changes the behavior of inherited methods.
  - This violates **Liskov Substitution Principle**, as `Square` does not behave consistently with `Rectangle`.

---

## **4. Solution: Refactor to Respect LSP**
### **Separate the Hierarchy**
- **Rectangles** and **Squares** are different concepts.
- They should not share an inheritance relationship but instead implement a common interface.

### **Step 1: Create an Interface**
```java
public interface Shape {
    int getArea();
}
```

### **Step 2: Implement Rectangle as a Separate Class**
```java
public class Rectangle implements Shape {
    private int width;
    private int height;

    public Rectangle(int width, int height) {
        this.width = width;
        this.height = height;
    }

    public void setWidth(int width) {
        this.width = width;
    }

    public void setHeight(int height) {
        this.height = height;
    }

    public int getWidth() {
        return width;
    }

    public int getHeight() {
        return height;
    }

    @Override
    public int getArea() {
        return width * height;
    }
}
```

### **Step 3: Implement Square as a Separate Class**
```java
public class Square implements Shape {
    private int side;

    public Square(int side) {
        this.side = side;
    }

    public void setSide(int side) {
        this.side = side;
    }

    public int getSide() {
        return side;
    }

    @Override
    public int getArea() {
        return side * side;
    }
}
```

### **Step 4: Usage and Polymorphism**
```java
public class Main {
    public static void main(String[] args) {
        Shape rect = new Rectangle(10, 20);
        Shape square = new Square(10);

        System.out.println("Rectangle Area: " + rect.getArea());
        System.out.println("Square Area: " + square.getArea());
    }
}
```

### **Output:**
```
Rectangle Area: 200
Square Area: 100
```

---

### **What's Improved and Why LSP is Preserved?**
- **Rectangles** and **Squares** are now separate classes implementing the common `Shape` interface.
- **No inheritance conflict**:
  - `Square` no longer inherits behavior from `Rectangle`, preventing unintended side-effects.
  - They can be used interchangeably wherever `Shape` is expected.
- **Consistent Behavior**:
  - Each class behaves consistently with its own rules, preserving LSP.

---

## **5. Guidelines for Following LSP**
1. **Behavioral Consistency:**
   - Subclass should not violate the behavior of the superclass.
   - If the base class method expects certain inputs, the derived class should accept them without restrictions.
2. **No Side-Effects:**
   - Methods in the subclass should not have side-effects that alter the state expected by the base class.
3. **No Weakened Preconditions:**
   - The subclass should not impose stricter validation than the base class.
4. **No Strengthened Postconditions:**
   - The subclass should not provide less functionality or different output than the base class method.

---

## **6. Common Mistakes to Avoid:**
1. **Incorrect Method Overriding:**
   - Changing the behavior of an inherited method, violating the base class contract.
2. **Unintended Side-Effects:**
   - Overriding methods that change the state in ways not expected by the superclass.
3. **Improper Use of Inheritance:**
   - Using inheritance for code reuse rather than for an "is-a" relationship.

---

## **7. Exercise: Mastering LSP**
### **Task: Create a `Bird` Hierarchy**
1. **Superclass: `Bird` (Abstract)**
   - `fly()` as an abstract method.
   - `layEggs()` as a concrete method.
2. **Subclasses:**
   - `Eagle` — Implement `fly()` for high-altitude flying.
   - `Penguin` — Penguins cannot fly but can swim.
     - **Ensure LSP is preserved** (Penguin should not override `fly()` to do nothing).
3. **Requirements:**
   - Separate flying behavior using an interface (`Flyable`).
   - Use **Polymorphism** to handle birds consistently.
   - Implement **LSP** correctly by separating flying and non-flying birds.

---

## **8. What's Next?**
We’ve now mastered:
- **Liskov Substitution Principle (LSP)**:
  - Ensures that subclasses are substitutable for their base classes.
  - Promotes consistent behavior in inheritance hierarchies.

### Next Up:
1. **Interface Segregation Principle (ISP)**
   - Avoid forcing clients to implement unused methods.
   - Design lean and specific interfaces.
2. **Dependency Inversion Principle (DIP)**
   - High-level modules should not depend on low-level modules.
   - Implementing DIP using Dependency Injection and Inversion of Control.

---
Here's a Java implementation of the Bird hierarchy that adheres to the **Liskov Substitution Principle (LSP)** by separating flying behavior into an interface, ensuring that subclasses can be used interchangeably with their superclass without breaking functionality:

```java
// Interface for flying behavior
interface Flyable {
    void fly();
}

// Abstract Superclass: Bird
abstract class Bird {
    private String name;

    public Bird(String name) {
        this.name = name;
    }

    // Abstract method for bird-specific behavior (not necessarily flying)
    public abstract void move();

    // Concrete method common to all birds
    public void layEggs() {
        System.out.println(name + " is laying eggs.");
    }

    public String getName() {
        return name;
    }
}

// Concrete Class: Eagle (flies high)
class Eagle extends Bird implements Flyable {
    public Eagle(String name) {
        super(name);
    }

    @Override
    public void fly() {
        System.out.println(getName() + " is flying at high altitude.");
    }

    @Override
    public void move() {
        fly(); // Eagles move by flying
    }
}

// Concrete Class: Penguin (does not fly, swims instead)
class Penguin extends Bird {
    public Penguin(String name) {
        super(name);
    }

    @Override
    public void move() {
        swim(); // Penguins move by swimming
    }

    private void swim() {
        System.out.println(getName() + " is swimming in the water.");
    }
}

// Test Class
class Main {
    public static void main(String[] args) {
        // Create bird objects
        Bird eagle = new Eagle("Bald Eagle");
        Bird penguin = new Penguin("Emperor Penguin");

        // Demonstrate polymorphism with Bird references
        Bird[] birds = {eagle, penguin};

        System.out.println("Bird Movements:");
        for (Bird bird : birds) {
            bird.move(); // Calls the appropriate move method
            bird.layEggs(); // Common behavior
            System.out.println();
        }

        // Demonstrate Flyable behavior only for birds that can fly
        Flyable[] flyers = { (Eagle) eagle }; // Only Eagle implements Flyable
        System.out.println("Flying Birds:");
        for (Flyable flyer : flyers) {
            flyer.fly();
        }
    }
}
```

### Explanation:

1. **Interface: `Flyable`**:
   - Defines `fly()` for birds capable of flying.
   - Separates flying behavior from the `Bird` class, ensuring LSP by not forcing all birds to implement an irrelevant `fly()` method.

2. **Abstract Superclass: `Bird`**:
   - **Attribute**: `name` for identification.
   - **Abstract Method**: `move()` is abstract, allowing each bird to define its primary mode of movement (flying for Eagle, swimming for Penguin).
   - **Concrete Method**: `layEggs()` is common to all birds and implemented here.

3. **Concrete Class: `Eagle`**:
   - Extends `Bird` and implements `Flyable`.
   - Implements `fly()` to describe high-altitude flight.
   - Overrides `move()` to use `fly()`, as flying is its primary movement.

4. **Concrete Class: `Penguin`**:
   - Extends `Bird` but does not implement `Flyable` (since penguins don’t fly).
   - Overrides `move()` to call a private `swim()` method, reflecting its actual behavior.
   - Does not break LSP because it doesn’t need to handle an irrelevant `fly()` method.

5. **LSP Preservation**:
   - By moving `fly()` to the `Flyable` interface, `Penguin` isn’t forced to implement or bypass an inappropriate behavior.
   - `Bird` objects can be substituted with `Eagle` or `Penguin` without altering the expected behavior of `move()` or `layEggs()`.

6. **Polymorphism**:
   - The `Bird[]` array holds both `Eagle` and `Penguin`, and `move()` is called polymorphically.
   - A separate `Flyable[]` array handles only flying birds, ensuring type safety and clarity.

### Sample Output:
```
Bird Movements:
Bald Eagle is flying at high altitude.
Bald Eagle is laying eggs.

Emperor Penguin is swimming in the water.
Emperor Penguin is laying eggs.

Flying Birds:
Bald Eagle is flying at high altitude.
```

### Key Points:
- **SRP**: Flying behavior is separated into `Flyable`, while `Bird` handles general bird traits.
- **OCP**: New bird types (e.g., Ostrich) can extend `Bird` and optionally implement `Flyable` without modifying existing code.
- **LSP**: `Penguin` substitutes `Bird` without violating expectations—`move()` works appropriately, and there’s no dummy `fly()` implementation.
- **Polymorphism**: Demonstrated through the `Bird` array and loop, with behavior dispatched to the correct subclass.
