### **Level 1: Foundation - Inheritance**  

---

## **1. What is Inheritance?**
**Inheritance** is one of the four fundamental principles of Object-Oriented Programming (OOP). It allows one class to **inherit** properties and behaviors (methods) from another class.

### **Key Points:**
- **Reusability:** Inheritance promotes code reuse. Common code can be written once in a base class and inherited by multiple derived classes.
- **Method Overriding:** A subclass can provide its own implementation for methods defined in the superclass.
- **Polymorphism:** Inheritance supports dynamic method dispatch (polymorphism), allowing a method to perform differently based on the object's type.

---

## **2. Real-World Example:**
### **Example: Parent-Child Relationship**
- **Parent:** General characteristics and behaviors (e.g., Human).
- **Child:** Specific characteristics and behaviors inherited from the parent (e.g., Man, Woman).
- **Inheritance in Action:** Both `Man` and `Woman` inherit the general properties of `Human` but can have their own unique behaviors.

---

## **3. Why is Inheritance Important?**
1. **Code Reusability:** Common behavior is written once in the base class and reused in derived classes.
2. **Maintainability:** Changes made to the base class are automatically inherited by all subclasses, promoting maintainability.
3. **Polymorphism:** Allows the same method to behave differently depending on the object's runtime type.
4. **Extensibility:** New functionality can be added by extending existing classes without modifying the original code.

---

## **4. Types of Inheritance in Java**
1. **Single Inheritance:**
   - A class inherits from one superclass.
     ```java
     class A { }
     class B extends A { }
     ```
2. **Multilevel Inheritance:**
   - A class inherits from a superclass, and another class inherits from it.
     ```java
     class A { }
     class B extends A { }
     class C extends B { }
     ```
3. **Hierarchical Inheritance:**
   - Multiple classes inherit from one superclass.
     ```java
     class A { }
     class B extends A { }
     class C extends A { }
     ```
4. **Multiple Inheritance (Not Supported Directly):**
   - Java does not support multiple inheritance with classes to avoid ambiguity.
   - Achieved using **Interfaces**.

---

## **5. How to Implement Inheritance in Java?**
In Java, inheritance is implemented using the `extends` keyword.

### **Syntax:**
```java
class SuperClass {
    // Superclass members
}

class SubClass extends SuperClass {
    // Subclass members
}
```

---

## **6. Example: Inheritance in Action**
### **Scenario: Animal Hierarchy**
- **Superclass:** Animal
- **Subclasses:** Dog, Cat

### **Superclass: Animal**
```java
public class Animal {
    // Attribute
    String name;

    // Constructor
    public Animal(String name) {
        this.name = name;
    }

    // Method (Behavior)
    public void eat() {
        System.out.println(name + " is eating.");
    }

    public void sleep() {
        System.out.println(name + " is sleeping.");
    }
}
```

### **Subclass: Dog**
```java
public class Dog extends Animal {
    // Additional attribute
    String breed;

    // Constructor
    public Dog(String name, String breed) {
        super(name); // Calling superclass constructor
        this.breed = breed;
    }

    // Additional method
    public void bark() {
        System.out.println(name + " is barking.");
    }
}
```

### **Subclass: Cat**
```java
public class Cat extends Animal {
    // Additional attribute
    String color;

    // Constructor
    public Cat(String name, String color) {
        super(name); // Calling superclass constructor
        this.color = color;
    }

    // Additional method
    public void meow() {
        System.out.println(name + " is meowing.");
    }
}
```

### **Usage:**
```java
public class Main {
    public static void main(String[] args) {
        // Creating a Dog object
        Dog dog = new Dog("Buddy", "Golden Retriever");
        dog.eat();  // Inherited from Animal
        dog.sleep(); // Inherited from Animal
        dog.bark(); // Specific to Dog

        System.out.println();

        // Creating a Cat object
        Cat cat = new Cat("Whiskers", "White");
        cat.eat();  // Inherited from Animal
        cat.sleep(); // Inherited from Animal
        cat.meow(); // Specific to Cat
    }
}
```

### **Output:**
```
Buddy is eating.
Buddy is sleeping.
Buddy is barking.

Whiskers is eating.
Whiskers is sleeping.
Whiskers is meowing.
```

---

## **7. `super` Keyword in Java**
- The `super` keyword is used to refer to the immediate parent class.
- It can be used for:
  1. **Calling the Parent Class Constructor:** `super(arguments)`
  2. **Accessing Parent Class Methods:** `super.methodName()`
  3. **Accessing Parent Class Fields:** `super.fieldName`

### **Example: Using `super` Keyword**
```java
public class Animal {
    String name;

    public Animal(String name) {
        this.name = name;
    }

    public void eat() {
        System.out.println(name + " is eating.");
    }
}
```

```java
public class Dog extends Animal {
    String breed;

    public Dog(String name, String breed) {
        super(name); // Calling parent class constructor
        this.breed = breed;
    }

    public void displayDetails() {
        System.out.println("Name: " + super.name); // Accessing parent class field
        System.out.println("Breed: " + this.breed);
    }
}
```

---

## **8. Method Overriding**
- **Method Overriding** allows a subclass to provide its own implementation of a method that is already defined in its superclass.
- It is achieved by defining a method in the subclass with the same signature as in the superclass.

### **Example: Method Overriding**
```java
public class Animal {
    public void makeSound() {
        System.out.println("Animal is making a sound.");
    }
}
```

```java
public class Dog extends Animal {
    // Overriding makeSound() method
    @Override
    public void makeSound() {
        System.out.println("Dog is barking.");
    }
}
```

### **Usage:**
```java
public class Main {
    public static void main(String[] args) {
        Animal myAnimal = new Animal();
        myAnimal.makeSound(); // Animal is making a sound.

        Dog myDog = new Dog();
        myDog.makeSound(); // Dog is barking.
    }
}
```

### **Output:**
```
Animal is making a sound.
Dog is barking.
```

---

## **9. `final` Keyword in Inheritance**
- **Final Class:** If a class is marked as `final`, it cannot be extended.
  ```java
  public final class Utility { }
  ```
- **Final Method:** If a method is marked as `final`, it cannot be overridden.
  ```java
  public final void display() { }
  ```
- **Final Variable:** A constant value that cannot be changed once initialized.
  ```java
  public final int MAX_VALUE = 100;
  ```

---

## **10. Exercise: Mastering Inheritance**
### **Task: Create a `Shape` Hierarchy**
1. **Abstract Class: `Shape`**
   - `String color` as an attribute.
   - `calculateArea()` as an abstract method.
   - `displayColor()` as a concrete method.
2. **Concrete Classes:**
   - `Circle` with `radius` attribute.
     - Implement `calculateArea()` for area of a circle (`π * radius^2`).
   - `Rectangle` with `length` and `width` attributes.
     - Implement `calculateArea()` for area of a rectangle (`length * width`).
3. **Requirements:**
   - Use **Method Overriding** for `calculateArea()`.
   - Use **super** to access parent class methods or fields where needed.
   - Test your implementation by creating objects of `Circle` and `Rectangle`.

---

Here's a Java implementation of the Shape hierarchy with the specified requirements:

```java
// Abstract Class: Shape
abstract class Shape {
    private String color;

    // Constructor
    public Shape(String color) {
        this.color = color;
    }

    // Abstract method to be overridden by subclasses
    public abstract double calculateArea();

    // Concrete method to display color
    public void displayColor() {
        System.out.println("Color: " + this.color);
    }

    // Getter for color (if needed)
    public String getColor() {
        return this.color;
    }
}

// Concrete Class: Circle
class Circle extends Shape {
    private double radius;

    // Constructor
    public Circle(String color, double radius) {
        super(color); // Call parent constructor to set color
        this.radius = radius;
    }

    // Override calculateArea for circle (π * radius^2)
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

// Concrete Class: Rectangle
class Rectangle extends Shape {
    private double length;
    private double width;

    // Constructor
    public Rectangle(String color, double length, double width) {
        super(color); // Call parent constructor to set color
        this.length = length;
        this.width = width;
    }

    // Override calculateArea for rectangle (length * width)
    @Override
    public double calculateArea() {
        return length * width;
    }
}

// Test Class
class Main {
    public static void main(String[] args) {
        // Create a Circle object
        Circle circle = new Circle("Red", 5.0);
        System.out.println("Circle Details:");
        circle.displayColor(); // Uses parent's concrete method
        System.out.printf("Area: %.2f square units%n", circle.calculateArea());
        System.out.println();

        // Create a Rectangle object
        Rectangle rectangle = new Rectangle("Blue", 4.0, 6.0);
        System.out.println("Rectangle Details:");
        rectangle.displayColor(); // Uses parent's concrete method
        System.out.printf("Area: %.2f square units%n", rectangle.calculateArea());
    }
}
```

### Explanation:

1. **Abstract Class: `Shape`**:
   - **Attribute**: `color` (private String) with a getter for access.
   - **Constructor**: Initializes `color`.
   - **Abstract Method**: `calculateArea()` is declared without implementation, forcing subclasses to override it.
   - **Concrete Method**: `displayColor()` displays the shape's color using the `color` attribute.

2. **Concrete Class: `Circle`**:
   - **Attribute**: `radius` (private double).
   - **Constructor**: Uses `super(color)` to pass the color to the `Shape` constructor.
   - **Method Override**: `calculateArea()` computes the area using the formula π * radius².

3. **Concrete Class: `Rectangle`**:
   - **Attributes**: `length` and `width` (private doubles).
   - **Constructor**: Uses `super(color)` to initialize the parent’s `color`.
   - **Method Override**: `calculateArea()` computes the area using length * width.

4. **Testing**:
   - The `Main` class creates objects of `Circle` and `Rectangle`, calls `displayColor()` (inherited from `Shape`), and `calculateArea()` (overridden in each subclass).
   - Output is formatted to two decimal places for readability.

### Sample Output:
```
Circle Details:
Color: Red
Area: 78.54 square units

Rectangle Details:
Color: Blue
Area: 24.00 square units
```

### Key Points:
- **Method Overriding**: `calculateArea()` is overridden in `Circle` and `Rectangle` with shape-specific formulas.
- **Use of `super`**: Used in constructors to access the parent class’s constructor and set the `color`.
- **Encapsulation**: All attributes are private, with access controlled via constructors and getters (where provided).
- **Testing**: Demonstrates polymorphism by treating `Circle` and `Rectangle` as `Shape` subtypes implicitly.
