Below is a detailed explanation of various design patterns with simple examples and well-documented notes in Java. Each section addresses a specific question, providing clarity on when to use certain patterns, their differences, and their practical applications.

```java
import java.util.*;

// Factory Method Example
interface Product {
    void use();
}

class ConcreteProductA implements Product {
    public void use() { System.out.println("Using Product A"); }
}

class ConcreteProductB implements Product {
    public void use() { System.out.println("Using Product B"); }
}

abstract class Creator {
    public abstract Product factoryMethod();
}

class ConcreteCreatorA extends Creator {
    public Product factoryMethod() { return new ConcreteProductA(); }
}

class ConcreteCreatorB extends Creator {
    public Product factoryMethod() { return new ConcreteProductB(); }
}

// Abstract Factory Example
interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

interface Button { void click(); }
interface Checkbox { void check(); }

class WinFactory implements GUIFactory {
    public Button createButton() { return new WinButton(); }
    public Checkbox createCheckbox() { return new WinCheckbox(); }
}

class MacFactory implements GUIFactory {
    public Button createButton() { return new MacButton(); }
    public Checkbox createCheckbox() { return new MacCheckbox(); }
}

class WinButton implements Button { public void click() { System.out.println("Windows Button"); } }
class WinCheckbox implements Checkbox { public void check() { System.out.println("Windows Checkbox"); } }
class MacButton implements Button { public void click() { System.out.println("Mac Button"); } }
class MacCheckbox implements Checkbox { public void check() { System.out.println("Mac Checkbox"); } }

// Strategy Example
interface Strategy {
    int execute(int a, int b);
}

class AddStrategy implements Strategy {
    public int execute(int a, int b) { return a + b; }
}

class SubtractStrategy implements Strategy {
    public int execute(int a, int b) { return a - b; }
}

class Context {
    private Strategy strategy;
    public Context(Strategy strategy) { this.strategy = strategy; }
    public int executeStrategy(int a, int b) { return strategy.execute(a, b); }
}

// State Example
interface State {
    void handle(ContextState context);
}

class StateA implements State {
    public void handle(ContextState context) {
        System.out.println("State A");
        context.setState(new StateB());
    }
}

class StateB implements State {
    public void handle(ContextState context) {
        System.out.println("State B");
        context.setState(new StateA());
    }
}

class ContextState {
    private State state;
    public ContextState() { this.state = new StateA(); }
    public void setState(State state) { this.state = state; }
    public void request() { state.handle(this); }
}

// Observer Example
interface Observer {
    void update(String message);
}

class Subject {
    private List<Observer> observers = new ArrayList<>();
    public void attach(Observer observer) { observers.add(observer); }
    public void notifyObservers(String message) {
        for (Observer o : observers) o.update(message);
    }
}

class ConcreteObserver implements Observer {
    private String name;
    public ConcreteObserver(String name) { this.name = name; }
    public void update(String message) { System.out.println(name + " received: " + message); }
}

// Singleton with Enum
enum Singleton {
    INSTANCE;
    public void doSomething() { System.out.println("Singleton working"); }
}

// Decorator Example
interface Coffee {
    String getDescription();
    double cost();
}

class SimpleCoffee implements Coffee {
    public String getDescription() { return "Simple Coffee"; }
    public double cost() { return 2.0; }
}

abstract class CoffeeDecorator implements Coffee {
    protected Coffee decoratedCoffee;
    public CoffeeDecorator(Coffee coffee) { this.decoratedCoffee = coffee; }
}

class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee coffee) { super(coffee); }
    public String getDescription() { return decoratedCoffee.getDescription() + ", Milk"; }
    public double cost() { return decoratedCoffee.cost() + 0.5; }
}

// Proxy Example
interface Image {
    void display();
}

class RealImage implements Image {
    private String filename;
    public RealImage(String filename) {
        this.filename = filename;
        System.out.println("Loading " + filename);
    }
    public void display() { System.out.println("Displaying " + filename); }
}

class ProxyImage implements Image {
    private RealImage realImage;
    private String filename;
    public ProxyImage(String filename) { this.filename = filename; }
    public void display() {
        if (realImage == null) realImage = new RealImage(filename);
        realImage.display();
    }
}

// Adapter Example
interface NewSystem { void newMethod(); }
class OldSystem { public void oldMethod() { System.out.println("Old method"); } }
class Adapter implements NewSystem {
    private OldSystem old;
    public Adapter(OldSystem old) { this.old = old; }
    public void newMethod() { old.oldMethod(); }
}

// Facade Example
class SubsystemA { void operationA() { System.out.println("Subsystem A"); } }
class SubsystemB { void operationB() { System.out.println("Subsystem B"); } }
class Facade {
    private SubsystemA a = new SubsystemA();
    private SubsystemB b = new SubsystemB();
    public void doSomething() { a.operationA(); b.operationB(); }
}

// Bridge Example
interface Color { String applyColor(); }
class Red implements Color { public String applyColor() { return "Red"; } }
abstract class Shape {
    protected Color color;
    public Shape(Color color) { this.color = color; }
    abstract String draw();
}
class Circle extends Shape {
    public Circle(Color color) { super(color); }
    public String draw() { return "Circle in " + color.applyColor(); }
}

// Chain of Responsibility Example
abstract class Handler {
    protected Handler next;
    public void setNext(Handler next) { this.next = next; }
    public abstract void handleRequest(int request);
}
class ConcreteHandlerA extends Handler {
    public void handleRequest(int request) {
        if (request < 10) System.out.println("Handler A: " + request);
        else if (next != null) next.handleRequest(request);
    }
}
class ConcreteHandlerB extends Handler {
    public void handleRequest(int request) {
        if (request >= 10) System.out.println("Handler B: " + request);
        else if (next != null) next.handleRequest(request);
    }
}

// Command Example
interface Command { void execute(); }
class Light { void on() { System.out.println("Light On"); } }
class LightOnCommand implements Command {
    private Light light;
    public LightOnCommand(Light light) { this.light = light; }
    public void execute() { light.on(); }
}

public class DesignPatterns {
    public static void main(String[] args) {
        // Factory Method
        Creator creatorA = new ConcreteCreatorA();
        Product productA = creatorA.factoryMethod();
        productA.use();

        // Abstract Factory
        GUIFactory factory = new WinFactory();
        factory.createButton().click();

        // Strategy vs State
        Context context = new Context(new AddStrategy());
        System.out.println(context.executeStrategy(5, 3)); // 8
        ContextState stateContext = new ContextState();
        stateContext.request(); // State A
        stateContext.request(); // State B

        // Observer
        Subject subject = new Subject();
        subject.attach(new ConcreteObserver("Observer1"));
        subject.notifyObservers("Hello");

        // Singleton Enum
        Singleton.INSTANCE.doSomething();

        // Decorator vs Proxy
        Coffee coffee = new MilkDecorator(new SimpleCoffee());
        System.out.println(coffee.getDescription() + " $" + coffee.cost());
        Image image = new ProxyImage("test.jpg");
        image.display();

        // Adapter vs Facade
        OldSystem old = new OldSystem();
        NewSystem adapter = new Adapter(old);
        adapter.newMethod();
        Facade facade = new Facade();
        facade.doSomething();

        // Bridge
        Shape circle = new Circle(new Red());
        System.out.println(circle.draw());

        // Chain vs Command
        Handler h1 = new ConcreteHandlerA();
        Handler h2 = new ConcreteHandlerB();
        h1.setNext(h2);
        h1.handleRequest(15);
        Command command = new LightOnCommand(new Light());
        command.execute();
    }
}
```

### Explanations:

1. **When to Use Factory Method vs. Abstract Factory:**
   - **Factory Method:**
     - **When:** You need to create one type of object, but subclasses decide the exact class.
     - **Example:** Creating a single product (e.g., `ConcreteProductA`) with flexibility in creation.
     - **Why:** Simplifies adding new product types without changing client code.
     - **Use Case:** Framework where subclasses customize object creation (e.g., logger type).
   - **Abstract Factory:**
     - **When:** You need to create families of related objects (e.g., UI elements for different OS).
     - **Example:** `WinFactory` creates `WinButton` and `WinCheckbox`.
     - **Why:** Ensures consistency across related objects (e.g., all Windows UI elements).
     - **Use Case:** GUI toolkit supporting multiple platforms.

2. **Difference Between Strategy and State Patterns:**
   - **Strategy:**
     - **Purpose:** Defines interchangeable algorithms (e.g., add vs. subtract).
     - **How:** Client selects strategy at runtime (`Context` uses `AddStrategy`).
     - **Key:** Focus on behavior variation, no state transition.
   - **State:**
     - **Purpose:** Manages state-dependent behavior with transitions (e.g., `StateA` to `StateB`).
     - **How:** Object changes its behavior by changing its state internally.
     - **Key:** Focus on state changes affecting behavior, not just algorithm choice.
   - **Difference:** Strategy is about choosing an algorithm; State is about evolving behavior with state.

3. **How Observer Pattern is Implemented in Java:**
   - **Implementation:** 
     - `Subject` maintains a list of `Observers`, notifies them on change (`notifyObservers`).
     - `Observer` interface defines `update` method.
   - **Java Built-in:** Uses `java.util.Observable` (subject) and `java.util.Observer` (interface).
   - **Example:** News agency (`Subject`) notifies subscribers (`Observers`) of updates.
   - **Why:** Decouples subject from observers, allows dynamic addition/removal.

4. **Why Use Singleton with Enum:**
   - **Reason:** Enums in Java are inherently singleton, thread-safe, and serialization-safe.
   - **How:** `enum Singleton { INSTANCE; }` ensures one instance, no instantiation via `new`.
   - **Benefits:** 
     - Prevents multiple instances via reflection or serialization.
     - Simpler than traditional Singleton with lazy initialization.
   - **Use Case:** Logger or configuration manager where one instance is needed.

5. **How Decorator Differs from Proxy:**
   - **Decorator:**
     - **Purpose:** Adds responsibilities to objects dynamically (e.g., adding milk to coffee).
     - **How:** Wraps object, extends behavior (`MilkDecorator` adds cost/description).
     - **Key:** Focus on enhancement, transparent to client.
   - **Proxy:**
     - **Purpose:** Controls access to an object (e.g., lazy loading of `RealImage`).
     - **How:** Acts as surrogate, may delay creation (`ProxyImage` loads on demand).
     - **Key:** Focus on access control, not just adding features.
   - **Difference:** Decorator enhances, Proxy regulates.

6. **Difference Between Adapter and Facade:**
   - **Adapter:**
     - **Purpose:** Converts one interface to another (e.g., adapts `OldSystem` to `NewSystem`).
     - **How:** Wraps an object to match expected interface (`Adapter` calls `oldMethod`).
     - **Key:** Focus on compatibility between mismatched interfaces.
   - **Facade:**
     - **Purpose:** Simplifies interaction with a complex subsystem (e.g., `Facade` hides `SubsystemA/B`).
     - **How:** Provides a unified interface for multiple components.
     - **Key:** Focus on ease of use, not interface conversion.
   - **Difference:** Adapter bridges interfaces; Facade simplifies subsystems.

7. **How Bridge Pattern Improves Maintainability:**
   - **How:** Separates abstraction (`Shape`) from implementation (`Color`).
     - `Shape` uses `Color` via composition, not inheritance.
   - **Example:** `Circle` can use `Red` or any new color without changing `Circle`.
   - **Benefit:** 
     - Add new shapes or colors independently (e.g., `Blue` doesn’t affect `Circle`).
     - Reduces class explosion (no `RedCircle`, `BlueCircle` subclasses).
   - **Maintainability:** Easier to extend, modify one side without impacting the other.

8. **When to Use Chain of Responsibility vs. Command:**
   - **Chain of Responsibility:**
     - **When:** Multiple objects might handle a request, order matters (e.g., request handlers).
     - **How:** Passes request along chain (`HandlerA` to `HandlerB`) until processed.
     - **Use Case:** Logging levels, event handling where priority exists.
   - **Command:**
     - **When:** Encapsulate a request as an object, support undo/queue (e.g., turn on light).
     - **How:** `Command` object (`LightOnCommand`) executes action on receiver (`Light`).
     - **Use Case:** Remote control, transaction systems with rollback.
   - **Difference:** Chain passes responsibility; Command encapsulates it.

### Output from main():
```
Using Product A
Windows Button
8
State A
State B
Observer1 received: Hello
Singleton working
Simple Coffee, Milk $2.5
Loading test.jpg
Displaying test.jpg
Old method
Subsystem A
Subsystem B
Circle in Red
Handler B: 15
Light On
```

Let me know if you want deeper examples or clarifications!
