### **Level 3: Advanced - Creational Design Patterns (Continued)**  

---

## **4. Creational Design Patterns (Continued)**

---

### **4.1 Prototype Pattern**
- **Purpose:** Creates a new object by copying an existing object, known as a prototype.
- **Use Case:** When object creation is costly (e.g., complex initialization) or when you want to avoid the expense of creating objects repeatedly.
- **Key Features:**
  - Implements **clone()** method to copy existing objects.
  - Achieves performance by copying rather than creating from scratch.
  - Can be used to maintain **object state**.

---

### **Example: Prototype Pattern**
### **Step 1: Create a Prototype Interface**
```java
public interface Animal extends Cloneable {
    Animal clone();
}
```

### **Step 2: Implement Concrete Classes**
```java
public class Sheep implements Animal {
    private String name;

    public Sheep(String name) {
        this.name = name;
    }

    @Override
    public Animal clone() {
        return new Sheep(this.name);
    }

    @Override
    public String toString() {
        return "Sheep: " + this.name;
    }
}
```

```java
public class Cow implements Animal {
    private String name;

    public Cow(String name) {
        this.name = name;
    }

    @Override
    public Animal clone() {
        return new Cow(this.name);
    }

    @Override
    public String toString() {
        return "Cow: " + this.name;
    }
}
```

### **Step 3: Usage**
```java
public class Main {
    public static void main(String[] args) {
        Animal sheep1 = new Sheep("Dolly");
        Animal sheep2 = sheep1.clone(); // Cloning Dolly

        Animal cow1 = new Cow("Bessie");
        Animal cow2 = cow1.clone(); // Cloning Bessie

        System.out.println(sheep1);
        System.out.println(sheep2);
        System.out.println(cow1);
        System.out.println(cow2);
    }
}
```

### **Output:**
```
Sheep: Dolly
Sheep: Dolly
Cow: Bessie
Cow: Bessie
```

### **What's Happening:**
- The `Sheep` and `Cow` classes implement the `Animal` prototype interface.
- The `clone()` method creates a new object by copying the state of the existing object.
- This reduces the cost of creating new objects from scratch.

---

### **When to Use Prototype Pattern:**
- When creating an object is resource-intensive (e.g., complex initialization).
- When you need to create multiple instances with a few variations.
- When you want to maintain object state across different instances.

---

## **5. Structural Design Patterns**
Structural patterns deal with the composition of classes or objects. They help ensure that if one part of a system changes, the entire system doesn’t need to change with it.

---

### **5.1 Adapter Pattern**
- **Purpose:** Converts the interface of a class into another interface that a client expects.
- **Use Case:** When you want to use an existing class but its interface is incompatible with your current system.
- **Key Features:**
  - **Wrapper class** that acts as an intermediary.
  - Promotes reusability of existing classes without modifying them.

---

### **Example: Adapter Pattern**
### **Scenario: Voltage Adapter**
- You have a 120V socket but a 240V appliance.
- An `Adapter` converts 120V to 240V.

### **Step 1: Target Interface**
```java
public interface Voltage {
    void providePower();
}
```

### **Step 2: Adaptee Class**
```java
public class Socket120V {
    public void supply120V() {
        System.out.println("Supplying 120V power.");
    }
}
```

### **Step 3: Adapter Class**
```java
public class VoltageAdapter implements Voltage {
    private Socket120V socket;

    public VoltageAdapter(Socket120V socket) {
        this.socket = socket;
    }

    @Override
    public void providePower() {
        socket.supply120V();
        System.out.println("Converting 120V to 240V.");
    }
}
```

### **Step 4: Usage**
```java
public class Main {
    public static void main(String[] args) {
        Socket120V socket120V = new Socket120V();
        Voltage adapter = new VoltageAdapter(socket120V);

        adapter.providePower();
    }
}
```

### **Output:**
```
Supplying 120V power.
Converting 120V to 240V.
```

### **What's Happening:**
- `VoltageAdapter` adapts the interface of `Socket120V` to the `Voltage` interface.
- `VoltageAdapter` wraps the `Socket120V` class and converts the output.
- This allows reusability of `Socket120V` without modifying its code.

---

### **When to Use Adapter Pattern:**
- When you want to use an existing class but its interface is incompatible.
- When you need to adapt legacy code to work with new classes.
- When you want to reuse third-party libraries without changing their code.

---

### **5.2 Decorator Pattern**
- **Purpose:** Adds new functionality to an object dynamically without altering its structure.
- **Use Case:** When you want to add responsibilities to individual objects, not the entire class.
- **Key Features:**
  - Uses **composition** to add behavior to an object.
  - Promotes **Single Responsibility Principle** by allowing functionality to be divided among classes.

---

### **Example: Decorator Pattern**
### **Scenario: Coffee Shop**
- Basic `Coffee` can have additional toppings (`Milk`, `Sugar`) added dynamically.

### **Step 1: Component Interface**
```java
public interface Coffee {
    String getDescription();
    double getCost();
}
```

### **Step 2: Concrete Component**
```java
public class SimpleCoffee implements Coffee {
    @Override
    public String getDescription() {
        return "Simple Coffee";
    }

    @Override
    public double getCost() {
        return 5.0;
    }
}
```

### **Step 3: Decorator Class**
```java
public abstract class CoffeeDecorator implements Coffee {
    protected Coffee decoratedCoffee;

    public CoffeeDecorator(Coffee coffee) {
        this.decoratedCoffee = coffee;
    }

    @Override
    public String getDescription() {
        return decoratedCoffee.getDescription();
    }

    @Override
    public double getCost() {
        return decoratedCoffee.getCost();
    }
}
```

### **Step 4: Concrete Decorators**
```java
public class Milk extends CoffeeDecorator {
    public Milk(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return decoratedCoffee.getDescription() + ", Milk";
    }

    @Override
    public double getCost() {
        return decoratedCoffee.getCost() + 1.5;
    }
}
```

```java
public class Sugar extends CoffeeDecorator {
    public Sugar(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return decoratedCoffee.getDescription() + ", Sugar";
    }

    @Override
    public double getCost() {
        return decoratedCoffee.getCost() + 0.5;
    }
}
```

### **Step 5: Usage**
```java
public class Main {
    public static void main(String[] args) {
        Coffee coffee = new SimpleCoffee();
        System.out.println(coffee.getDescription() + " - $" + coffee.getCost());

        coffee = new Milk(coffee);
        System.out.println(coffee.getDescription() + " - $" + coffee.getCost());

        coffee = new Sugar(coffee);
        System.out.println(coffee.getDescription() + " - $" + coffee.getCost());
    }
}
```

### **Output:**
```
Simple Coffee - $5.0
Simple Coffee, Milk - $6.5
Simple Coffee, Milk, Sugar - $7.0
```

### **What's Happening:**
- `Milk` and `Sugar` are decorators that add behavior to `SimpleCoffee`.
- Each decorator adds its functionality while preserving the original class's behavior.
- This promotes **Open/Closed Principle** as new toppings can be added without modifying existing classes.

---

## **6. What's Next?**
We’ve now mastered:
- **Prototype Pattern**: Clones objects efficiently.
- **Adapter Pattern**: Converts one interface to another.
- **Decorator Pattern**: Adds behavior dynamically.

### Next Up: **Behavioral Patterns**:
- **Strategy, Observer, Template Method, Chain of Responsibility**.

---
