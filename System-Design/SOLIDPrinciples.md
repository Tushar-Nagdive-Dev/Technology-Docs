# SOLID Principles - Complete System Design Guide

**SOLID** is a set of five object-oriented design principles that help you write clean, maintainable, and scalable code. Think of them as rules that prevent your code from becoming a tangled mess as your project grows.

Let me break down each principle with clear explanations, real-world examples, and visual diagrams.

---

## **1. Single Responsibility Principle (SRP)**

### Definition
A class should have **only one reason to change** — meaning it should have only one job or responsibility.

### Simple Explanation
Imagine a Swiss Army knife vs. specialized tools. While a Swiss Army knife does many things poorly, a specialized screwdriver does one thing excellently. Your classes should be like specialized tools.### Real-World Example
Think of a restaurant:
- **Violation**: One person cooking, taking orders, cleaning, AND managing finances (overwhelmed!)
- **Correct**: Chef cooks, waiter takes orders, cleaner maintains hygiene, accountant manages money

### Code Example - Violation

```java
// ❌ BAD: Employee class doing too many things
public class Employee {
    private String name;
    private double salary;
    
    // Business logic
    public double calculateSalary() {
        return salary * 1.1; // 10% bonus
    }
    
    // Database operations
    public void saveToDatabase() {
        // Database connection code
        // SQL queries
        System.out.println("Saving to database...");
    }
    
    // Reporting
    public String generateReport() {
        return "Employee Report: " + name;
    }
    
    // Email notifications
    public void sendEmail(String message) {
        System.out.println("Sending email: " + message);
    }
}
```

**Problems with this approach:**
1. If database schema changes → modify Employee class
2. If email service changes → modify Employee class
3. If salary calculation changes → modify Employee class
4. Hard to test each responsibility independently
5. Class becomes huge and unmaintainable

### Code Example - Correct Implementation

```java
// ✓ GOOD: Each class has one responsibility

// 1. Employee: Just holds employee data
public class Employee {
    private String name;
    private String id;
    private double baseSalary;
    
    public Employee(String name, String id, double baseSalary) {
        this.name = name;
        this.id = id;
        this.baseSalary = baseSalary;
    }
    
    // Getters
    public String getName() { return name; }
    public String getId() { return id; }
    public double getBaseSalary() { return baseSalary; }
}

// 2. SalaryCalculator: Only calculates salary
public class SalaryCalculator {
    public double calculateTotalSalary(Employee employee) {
        return employee.getBaseSalary() * 1.1; // 10% bonus
    }
    
    public double calculateTax(Employee employee) {
        return calculateTotalSalary(employee) * 0.2; // 20% tax
    }
}

// 3. EmployeeRepository: Only handles database operations
public class EmployeeRepository {
    public void save(Employee employee) {
        // Database connection
        // SQL INSERT operation
        System.out.println("Saving employee: " + employee.getName());
    }
    
    public Employee findById(String id) {
        // SQL SELECT operation
        return new Employee("John", id, 50000);
    }
}

// 4. ReportGenerator: Only generates reports
public class ReportGenerator {
    public String generateEmployeeReport(Employee employee) {
        StringBuilder report = new StringBuilder();
        report.append("=== Employee Report ===\n");
        report.append("Name: ").append(employee.getName()).append("\n");
        report.append("ID: ").append(employee.getId()).append("\n");
        report.append("Salary: $").append(employee.getBaseSalary());
        return report.toString();
    }
}

// 5. EmailService: Only sends emails
public class EmailService {
    public void sendWelcomeEmail(Employee employee) {
        String message = "Welcome " + employee.getName();
        // SMTP connection logic
        System.out.println("Sending email: " + message);
    }
}
```

### Usage Example

```java
public class Main {
    public static void main(String[] args) {
        // Create employee
        Employee emp = new Employee("Alice", "E001", 60000);
        
        // Each service handles its responsibility
        SalaryCalculator calculator = new SalaryCalculator();
        EmployeeRepository repository = new EmployeeRepository();
        ReportGenerator reportGen = new ReportGenerator();
        EmailService emailService = new EmailService();
        
        // Use services independently
        double totalSalary = calculator.calculateTotalSalary(emp);
        repository.save(emp);
        String report = reportGen.generateEmployeeReport(emp);
        emailService.sendWelcomeEmail(emp);
        
        System.out.println("Total Salary: $" + totalSalary);
        System.out.println(report);
    }
}
```

### Edge Cases & Considerations

1. **When to split?** Split when a class has multiple reasons to change
2. **Too many classes?** Balance is key - don't create a class for every single method
3. **Data classes**: Simple data holders (DTOs) are fine with just getters/setters
4. **God Objects**: Watch out for "Manager" or "Util" classes doing too much

---

## **2. Open/Closed Principle (OCP)**

### Definition
Software entities should be **open for extension** but **closed for modification** — you should be able to add new functionality without changing existing code.

### Simple Explanation
Think of a power outlet with USB ports. You can add new devices (extension) without rewiring your house's electrical system (modification).### Real-World Example
Imagine a pizza restaurant:
- **Violation**: Every time a customer wants a new topping, you rewrite the entire recipe book
- **Correct**: You have a base pizza recipe, and you simply add new toppings without changing the base

### Code Example - Violation

```java
// ❌ BAD: Have to modify this class every time we add a shape
public class AreaCalculator {
    public double calculateArea(Object shape) {
        if (shape instanceof Circle) {
            Circle circle = (Circle) shape;
            return Math.PI * circle.radius * circle.radius;
        } 
        else if (shape instanceof Rectangle) {
            Rectangle rect = (Rectangle) shape;
            return rect.width * rect.height;
        }
        // Every new shape requires modifying this method!
        else if (shape instanceof Triangle) {
            Triangle tri = (Triangle) shape;
            return 0.5 * tri.base * tri.height;
        }
        return 0;
    }
}

class Circle {
    double radius;
    Circle(double radius) { this.radius = radius; }
}

class Rectangle {
    double width, height;
    Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
}
```

**Problems:**
1. Adding a new shape requires modifying `AreaCalculator`
2. Violates Single Responsibility (AreaCalculator knows about all shapes)
3. High risk of breaking existing functionality
4. Difficult to test

### Code Example - Correct Implementation

```java
// ✓ GOOD: Abstract base that is closed for modification
public abstract class Shape {
    // Abstract method forces subclasses to implement
    public abstract double calculateArea();
    
    // Can have common methods
    public void printArea() {
        System.out.println("Area: " + calculateArea());
    }
}

// Each shape extends and implements its own logic
public class Circle extends Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

public class Rectangle extends Shape {
    private double width;
    private double height;
    
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    @Override
    public double calculateArea() {
        return width * height;
    }
}

public class Triangle extends Shape {
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
}

// NEW SHAPE: Add without modifying existing code!
public class Pentagon extends Shape {
    private double side;
    private double apothem;
    
    public Pentagon(double side, double apothem) {
        this.side = side;
        this.apothem = apothem;
    }
    
    @Override
    public double calculateArea() {
        return 2.5 * side * apothem;
    }
}

// Calculator works with ALL shapes without modification
public class AreaCalculator {
    public double calculateTotalArea(Shape[] shapes) {
        double total = 0;
        for (Shape shape : shapes) {
            total += shape.calculateArea(); // Polymorphism!
        }
        return total;
    }
    
    public void displayAreas(Shape[] shapes) {
        for (Shape shape : shapes) {
            System.out.println(shape.getClass().getSimpleName() + 
                             ": " + shape.calculateArea());
        }
    }
}
```

### Usage Example

```java
public class Main {
    public static void main(String[] args) {
        // Create different shapes
        Shape[] shapes = {
            new Circle(5),
            new Rectangle(4, 6),
            new Triangle(3, 8),
            new Pentagon(5, 3.5) // New shape added seamlessly!
        };
        
        // Calculator works with all shapes
        AreaCalculator calculator = new AreaCalculator();
        
        System.out.println("Individual Areas:");
        calculator.displayAreas(shapes);
        
        System.out.println("\nTotal Area: " + 
                         calculator.calculateTotalArea(shapes));
    }
}
```

**Output:**
```
Individual Areas:
Circle: 78.53981633974483
Rectangle: 24.0
Triangle: 12.0
Pentagon: 43.75

Total Area: 158.28981633974482
```

### Another Example - Payment Processing

```java
// Payment interface (closed for modification)
public interface PaymentProcessor {
    void processPayment(double amount);
    boolean validatePayment();
}

// Original implementations
public class CreditCardPayment implements PaymentProcessor {
    private String cardNumber;
    
    public CreditCardPayment(String cardNumber) {
        this.cardNumber = cardNumber;
    }
    
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing $" + amount + " via Credit Card");
    }
    
    @Override
    public boolean validatePayment() {
        return cardNumber.length() == 16;
    }
}

public class PayPalPayment implements PaymentProcessor {
    private String email;
    
    public PayPalPayment(String email) {
        this.email = email;
    }
    
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing $" + amount + " via PayPal");
    }
    
    @Override
    public boolean validatePayment() {
        return email.contains("@");
    }
}

// NEW: Add cryptocurrency payment WITHOUT changing existing code
public class CryptoPayment implements PaymentProcessor {
    private String walletAddress;
    
    public CryptoPayment(String walletAddress) {
        this.walletAddress = walletAddress;
    }
    
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing $" + amount + " via Cryptocurrency");
    }
    
    @Override
    public boolean validatePayment() {
        return walletAddress.length() == 42;
    }
}

// Payment service works with all payment methods
public class PaymentService {
    public void executePayment(PaymentProcessor processor, double amount) {
        if (processor.validatePayment()) {
            processor.processPayment(amount);
        } else {
            System.out.println("Payment validation failed!");
        }
    }
}
```

### Edge Cases & Best Practices

1. **Use interfaces/abstractions**: Define contracts that won't change
2. **Strategy Pattern**: Great for implementing OCP
3. **Plugin architectures**: Load new functionality at runtime
4. **When to break the rule**: Sometimes modifying is simpler than over-engineering
5. **Anticipate change points**: Don't create abstractions everywhere, focus on areas likely to change

---

## **3. Liskov Substitution Principle (LSP)**

### Definition
Objects of a superclass should be **replaceable with objects of its subclasses** without breaking the application — subtypes must be substitutable for their base types.

### Simple Explanation
If you have a remote control for your TV, a universal remote should work exactly the same way. You shouldn't need to learn new buttons or behaviors.### Real-World Example
Think about electric vehicles:
- **Violation**: An electric car that can't use a gas station pretending to be a regular car
- **Correct**: Electric cars have their own interface (charging stations), gas cars have theirs (gas pumps)

### Code Example - Violation

```java
// ❌ BAD: Penguin breaks the contract of Bird
class Bird {
    public void fly() {
        System.out.println("Flying in the sky!");
    }
    
    public void eat() {
        System.out.println("Eating food");
    }
}

class Sparrow extends Bird {
    // Works fine - sparrows can fly
}

class Penguin extends Bird {
    @Override
    public void fly() {
        // Penguin can't fly!
        throw new UnsupportedOperationException("Penguins can't fly!");
    }
}

// This violates LSP!
class BirdWatcher {
    public void watchBirdFly(Bird bird) {
        bird.fly(); // Will crash if bird is a Penguin!
    }
}
```

**Problems:**
1. Substituting Penguin for Bird causes runtime exceptions
2. Breaks the expectation that all Birds can fly
3. Client code needs to check bird type before calling methods

### Code Example - Correct Implementation

```java
// ✓ GOOD: Proper abstraction that respects LSP

// Base interface - only common behaviors
interface Bird {
    void eat();
    void makeSound();
}

// Separate interface for flying capability
interface Flyable {
    void fly();
    int getMaxAltitude();
}

// Separate interface for swimming capability
interface Swimmable {
    void swim();
    int getDiveDepth();
}

// Sparrow implements Bird and Flyable
class Sparrow implements Bird, Flyable {
    @Override
    public void eat() {
        System.out.println("Sparrow eating seeds");
    }
    
    @Override
    public void makeSound() {
        System.out.println("Chirp chirp!");
    }
    
    @Override
    public void fly() {
        System.out.println("Sparrow flying gracefully");
    }
    
    @Override
    public int getMaxAltitude() {
        return 1000; // meters
    }
}

// Penguin implements Bird and Swimmable
class Penguin implements Bird, Swimmable {
    @Override
    public void eat() {
        System.out.println("Penguin eating fish");
    }
    
    @Override
    public void makeSound() {
        System.out.println("Squawk!");
    }
    
    @Override
    public void swim() {
        System.out.println("Penguin swimming underwater");
    }
    
    @Override
    public int getDiveDepth() {
        return 250; // meters
    }
}

// Eagle implements both!
class Eagle implements Bird, Flyable {
    @Override
    public void eat() {
        System.out.println("Eagle hunting prey");
    }
    
    @Override
    public void makeSound() {
        System.out.println("Screech!");
    }
    
    @Override
    public void fly() {
        System.out.println("Eagle soaring high");
    }
    
    @Override
    public int getMaxAltitude() {
        return 3000; // meters
    }
}

// Services work with specific capabilities
class AirShowOrganizer {
    public void performAirShow(Flyable flyer) {
        flyer.fly();
        System.out.println("Max altitude: " + flyer.getMaxAltitude() + "m");
    }
}

class AquariumManager {
    public void demonstrateSwimming(Swimmable swimmer) {
        swimmer.swim();
        System.out.println("Dive depth: " + swimmer.getDiveDepth() + "m");
    }
}

class BirdSanctuary {
    public void feedBird(Bird bird) {
        bird.eat();
        bird.makeSound();
    }
}
```

### Usage Example

```java
public class Main {
    public static void main(String[] args) {
        Sparrow sparrow = new Sparrow();
        Penguin penguin = new Penguin();
        Eagle eagle = new Eagle();
        
        // Bird sanctuary can work with ALL birds
        BirdSanctuary sanctuary = new BirdSanctuary();
        sanctuary.feedBird(sparrow);  // ✓ Works
        sanctuary.feedBird(penguin);  // ✓ Works
        sanctuary.feedBird(eagle);    // ✓ Works
        
        // Air show only works with flying birds
        AirShowOrganizer airShow = new AirShowOrganizer();
        airShow.performAirShow(sparrow);  // ✓ Works
        airShow.performAirShow(eagle);    // ✓ Works
        // airShow.performAirShow(penguin); // ✗ Compile error - good!
        
        // Aquarium only works with swimming birds
        AquariumManager aquarium = new AquariumManager();
        aquarium.demonstrateSwimming(penguin);  // ✓ Works
        // aquarium.demonstrateSwimming(sparrow); // ✗ Compile error - good!
    }
}
```

### Another Example - Rectangle/Square Problem

```java
// ❌ VIOLATION: Classic LSP violation
class Rectangle {
    protected int width;
    protected int height;
    
    public void setWidth(int width) {
        this.width = width;
    }
    
    public void setHeight(int height) {
        this.height = height;
    }
    
    public int getArea() {
        return width * height;
    }
}

class Square extends Rectangle {
    @Override
    public void setWidth(int width) {
        this.width = width;
        this.height = width; // Square must keep both equal!
    }
    
    @Override
    public void setHeight(int height) {
        this.width = height;
        this.height = height;
    }
}

// This breaks!
public void testRectangle(Rectangle rect) {
    rect.setWidth(5);
    rect.setHeight(4);
    assert rect.getArea() == 20; // Fails if rect is a Square!
}
```

```java
// ✓ CORRECT: Separate hierarchies
interface Shape {
    int getArea();
}

class Rectangle implements Shape {
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
    
    @Override
    public int getArea() {
        return width * height;
    }
}

class Square implements Shape {
    private int side;
    
    public Square(int side) {
        this.side = side;
    }
    
    public void setSide(int side) {
        this.side = side;
    }
    
    @Override
    public int getArea() {
        return side * side;
    }
}
```

### Edge Cases & Best Practices

1. **Preconditions cannot be strengthened**: Child class can't require more than parent
2. **Postconditions cannot be weakened**: Child must deliver at least what parent promises
3. **Invariants must be preserved**: Rules that hold for parent must hold for child
4. **History constraint**: Child shouldn't allow state changes parent doesn't allow
5. **Use composition over inheritance** when behavior differs significantly

---

## **4. Interface Segregation Principle (ISP)**

### Definition
**No client should be forced to depend on methods it does not use** — split large interfaces into smaller, more specific ones.

### Simple Explanation
Think of a Swiss Army knife vs. specialized tools. Don't force someone who just needs a screwdriver to carry the entire Swiss Army knife.### Real-World Example
Think of a smartphone:
- **Violation**: Forcing everyone to use every single app installed
- **Correct**: Each app exists separately, you only open what you need

### Code Example - Violation

```java
// ❌ BAD: Fat interface forcing unnecessary implementation
interface Worker {
    void work();
    void eat();
    void sleep();
    void getPaid();
    void attendMeeting();
    void takeBreak();
}

class HumanWorker implements Worker {
    @Override
    public void work() {
        System.out.println("Human working");
    }
    
    @Override
    public void eat() {
        System.out.println("Human eating lunch");
    }
    
    @Override
    public void sleep() {
        System.out.println("Human sleeping");
    }
    
    @Override
    public void getPaid() {
        System.out.println("Human receiving salary");
    }
    
    @Override
    public void attendMeeting() {
        System.out.println("Human attending meeting");
    }
    
    @Override
    public void takeBreak() {
        System.out.println("Human taking coffee break");
    }
}

class RobotWorker implements Worker {
    @Override
    public void work() {
        System.out.println("Robot working 24/7");
    }
    
    @Override
    public void eat() {
        // Robots don't eat!
        throw new UnsupportedOperationException("Robots don't eat");
    }
    
    @Override
    public void sleep() {
        // Robots don't sleep!
        throw new UnsupportedOperationException("Robots don't sleep");
    }
    
    @Override
    public void getPaid() {
        // Robots don't get paid!
        throw new UnsupportedOperationException("Robots don't get paid");
    }
    
    @Override
    public void attendMeeting() {
        throw new UnsupportedOperationException("Robots don't attend meetings");
    }
    
    @Override
    public void takeBreak() {
        throw new UnsupportedOperationException("Robots don't take breaks");
    }
}
```

**Problems:**
1. RobotWorker forced to implement methods it doesn't need
2. Many methods throw exceptions at runtime
3. Interface changes affect all implementers
4. Violates LSP (can't substitute safely)

### Code Example - Correct Implementation

```java
// ✓ GOOD: Segregated interfaces

// Core interface - all workers can work
interface Workable {
    void work();
}

// Optional capabilities
interface Eatable {
    void eat();
}

interface Sleepable {
    void sleep();
}

interface Payable {
    void getPaid();
}

interface Attendable {
    void attendMeeting();
}

interface Breakable {
    void takeBreak();
}

// Human implements all interfaces
class HumanWorker implements Workable, Eatable, Sleepable, 
                              Payable, Attendable, Breakable {
    private String name;
    
    public HumanWorker(String name) {
        this.name = name;
    }
    
    @Override
    public void work() {
        System.out.println(name + " is working");
    }
    
    @Override
    public void eat() {
        System.out.println(name + " is eating lunch");
    }
    
    @Override
    public void sleep() {
        System.out.println(name + " is sleeping");
    }
    
    @Override
    public void getPaid() {
        System.out.println(name + " received salary");
    }
    
    @Override
    public void attendMeeting() {
        System.out.println(name + " is attending meeting");
    }
    
    @Override
    public void takeBreak() {
        System.out.println(name + " is taking a coffee break");
    }
}

// Robot only implements what it needs
class RobotWorker implements Workable {
    private String model;
    
    public RobotWorker(String model) {
        this.model = model;
    }
    
    @Override
    public void work() {
        System.out.println("Robot " + model + " is working 24/7");
    }
}

// Contractor implements fewer interfaces than full-time employee
class ContractorWorker implements Workable, Payable, Attendable {
    private String name;
    
    public ContractorWorker(String name) {
        this.name = name;
    }
    
    @Override
    public void work() {
        System.out.println("Contractor " + name + " is working remotely");
    }
    
    @Override
    public void getPaid() {
        System.out.println("Contractor " + name + " received hourly payment");
    }
    
    @Override
    public void attendMeeting() {
        System.out.println("Contractor " + name + " joined virtual meeting");
    }
}

// Services depend only on specific interfaces
class WorkManager {
    public void manageWork(Workable worker) {
        worker.work(); // Only depends on Workable
    }
}

class LunchService {
    public void serveLunch(Eatable eater) {
        eater.eat(); // Only works with things that can eat
    }
}

class PayrollDepartment {
    public void processPayment(Payable employee) {
        employee.getPaid(); // Only needs Payable interface
    }
}

class MeetingCoordinator {
    public void scheduleMeeting(Attendable attendee) {
        attendee.attendMeeting();
    }
}
```

### Usage Example

```java
public class Main {
    public static void main(String[] args) {
        HumanWorker human = new HumanWorker("Alice");
        RobotWorker robot = new RobotWorker("R2D2");
        ContractorWorker contractor = new ContractorWorker("Bob");
        
        // Work manager works with all
        WorkManager workMgr = new WorkManager();
        workMgr.manageWork(human);       // ✓ Works
        workMgr.manageWork(robot);       // ✓ Works
        workMgr.manageWork(contractor);  // ✓ Works
        
        // Lunch service only for humans
        LunchService lunchSvc = new LunchService();
        lunchSvc.serveLunch(human);      // ✓ Works
        // lunchSvc.serveLunch(robot);   // ✗ Compile error - good!
        
        // Payroll works with humans and contractors
        PayrollDepartment payroll = new PayrollDepartment();
        payroll.processPayment(human);       // ✓ Works
        payroll.processPayment(contractor);  // ✓ Works
        // payroll.processPayment(robot);    // ✗ Compile error - good!
        
        // Meetings for humans and contractors
        MeetingCoordinator meetings = new MeetingCoordinator();
        meetings.scheduleMeeting(human);       // ✓ Works
        meetings.scheduleMeeting(contractor);  // ✓ Works
        // meetings.scheduleMeeting(robot);    // ✗ Compile error - good!
    }
}
```

### Another Example - Multifunction Printer

```java
// ❌ VIOLATION: Fat interface
interface MultifunctionDevice {
    void print(Document doc);
    void scan(Document doc);
    void fax(Document doc);
    void photocopy(Document doc);
}

class OldPrinter implements MultifunctionDevice {
    public void print(Document doc) {
        System.out.println("Printing");
    }
    
    // Old printer can't scan!
    public void scan(Document doc) {
        throw new UnsupportedOperationException();
    }
    
    // Old printer can't fax!
    public void fax(Document doc) {
        throw new UnsupportedOperationException();
    }
    
    public void photocopy(Document doc) {
        throw new UnsupportedOperationException();
    }
}
```

```java
// ✓ CORRECT: Segregated interfaces
interface Printer {
    void print(Document doc);
}

interface Scanner {
    void scan(Document doc);
}

interface Fax {
    void fax(Document doc);
}

interface Photocopier {
    void photocopy(Document doc);
}

// Simple printer
class SimplePrinter implements Printer {
    @Override
    public void print(Document doc) {
        System.out.println("Printing: " + doc.getName());
    }
}

// Modern all-in-one
class ModernPrinter implements Printer, Scanner, Fax, Photocopier {
    @Override
    public void print(Document doc) {
        System.out.println("Modern print: " + doc.getName());
    }
    
    @Override
    public void scan(Document doc) {
        System.out.println("Scanning: " + doc.getName());
    }
    
    @Override
    public void fax(Document doc) {
        System.out.println("Faxing: " + doc.getName());
    }
    
    @Override
    public void photocopy(Document doc) {
        System.out.println("Photocopying: " + doc.getName());
    }
}

// Document class
class Document {
    private String name;
    
    public Document(String name) {
        this.name = name;
    }
    
    public String getName() {
        return name;
    }
}
```

### Edge Cases & Best Practices

1. **Role interfaces**: Create interfaces based on client roles, not all possible behaviors
2. **Prefer many small interfaces** over one large interface
3. **Client-specific interfaces**: Design interfaces based on what clients need
4. **Avoid marker interfaces**: Unless they truly represent a capability
5. **Don't over-segregate**: Too many interfaces can be confusing; find balance

---

## **5. Dependency Inversion Principle (DIP)**

### Definition
**High-level modules should not depend on low-level modules**. Both should depend on abstractions. **Abstractions should not depend on details**. Details should depend on abstractions.

### Simple Explanation
Think of electrical outlets. Your devices don't need to know about the power plant - they just plug into a standard socket (abstraction). The power plant can be coal, solar, or nuclear, and your device still works.### Real-World Example
Think of charging cables:
- **Violation**: Each phone has a unique charging port that only works with its own charger
- **Correct**: USB-C standard (abstraction) - any USB-C cable works with any USB-C device

### Code Example - Violation

```java
// ❌ BAD: High-level module depends on low-level concrete classes
class EmailSender {
    public void sendEmail(String message) {
        System.out.println("Email sent: " + message);
    }
}

class SMSSender {
    public void sendSMS(String message) {
        System.out.println("SMS sent: " + message);
    }
}

// High-level module directly depends on concrete implementations
class NotificationService {
    private EmailSender emailSender;
    private SMSSender smsSender;
    
    public NotificationService() {
        this.emailSender = new EmailSender(); // Tight coupling!
        this.smsSender = new SMSSender();     // Tight coupling!
    }
    
    public void sendEmailNotification(String message) {
        emailSender.sendEmail(message);
    }
    
    public void sendSMSNotification(String message) {
        smsSender.sendSMS(message);
    }
    
    // Adding Push notification requires modifying this class!
}
```

**Problems:**
1. NotificationService tightly coupled to specific implementations
2. Can't easily add new notification types
3. Hard to test (can't mock dependencies)
4. Changes in EmailSender affect NotificationService

### Code Example - Correct Implementation

```java
// ✓ GOOD: Abstraction layer

// Step 1: Define abstraction (interface)
interface MessageSender {
    void send(String recipient, String message);
    String getChannelName();
}

// Step 2: Low-level modules implement abstraction
class EmailSender implements MessageSender {
    @Override
    public void send(String recipient, String message) {
        System.out.println("Email to " + recipient + ": " + message);
        // SMTP logic here
    }
    
    @Override
    public String getChannelName() {
        return "Email";
    }
}

class SMSSender implements MessageSender {
    @Override
    public void send(String recipient, String message) {
        System.out.println("SMS to " + recipient + ": " + message);
        // SMS gateway logic here
    }
    
    @Override
    public String getChannelName() {
        return "SMS";
    }
}

class PushNotificationSender implements MessageSender {
    @Override
    public void send(String recipient, String message) {
        System.out.println("Push to " + recipient + ": " + message);
        // Push notification service logic
    }
    
    @Override
    public String getChannelName() {
        return "Push Notification";
    }
}

// NEW: Easy to add without modifying existing code
class SlackSender implements MessageSender {
    @Override
    public void send(String recipient, String message) {
        System.out.println("Slack to " + recipient + ": " + message);
        // Slack API logic
    }
    
    @Override
    public String getChannelName() {
        return "Slack";
    }
}

// Step 3: High-level module depends on abstraction
class NotificationService {
    private List<MessageSender> senders;
    
    // Dependency injection - receives abstractions, not concrete classes
    public NotificationService(List<MessageSender> senders) {
        this.senders = senders;
    }
    
    public void sendToAll(String recipient, String message) {
        for (MessageSender sender : senders) {
            try {
                sender.send(recipient, message);
                System.out.println("✓ Sent via " + sender.getChannelName());
            } catch (Exception e) {
                System.out.println("✗ Failed via " + sender.getChannelName());
            }
        }
    }
    
    public void sendViaChannel(String channel, String recipient, String message) {
        for (MessageSender sender : senders) {
            if (sender.getChannelName().equalsIgnoreCase(channel)) {
                sender.send(recipient, message);
                return;
            }
        }
        System.out.println("Channel not found: " + channel);
    }
}
```

### Usage Example with Dependency Injection

```java
public class Main {
    public static void main(String[] args) {
        // Configure which senders to use (dependency injection)
        List<MessageSender> senders = Arrays.asList(
            new EmailSender(),
            new SMSSender(),
            new PushNotificationSender(),
            new SlackSender()  // Easy to add new channel!
        );
        
        // Inject dependencies
        NotificationService service = new NotificationService(senders);
        
        // Use the service
        System.out.println("=== Sending to all channels ===");
        service.sendToAll("user@example.com", "Your order has shipped!");
        
        System.out.println("\n=== Sending via specific channel ===");
        service.sendViaChannel("Email", "user@example.com", "Password reset link");
        service.sendViaChannel("Slack", "#general", "Deploy complete!");
    }
}
```

**Output:**
```
=== Sending to all channels ===
Email to user@example.com: Your order has shipped!
✓ Sent via Email
SMS to user@example.com: Your order has shipped!
✓ Sent via SMS
Push to user@example.com: Your order has shipped!
✓ Sent via Push Notification
Slack to user@example.com: Your order has shipped!
✓ Sent via Slack

=== Sending via specific channel ===
Email to user@example.com: Password reset link
Slack to #general: Deploy complete!
```

### Another Example - Database Access

```java
// ❌ VIOLATION
class UserService {
    private MySQLDatabase database; // Direct dependency on MySQL!
    
    public UserService() {
        this.database = new MySQLDatabase();
    }
    
    public User getUser(int id) {
        return database.query("SELECT * FROM users WHERE id = " + id);
    }
}
```

```java
// ✓ CORRECT
// Abstraction
interface Database {
    User queryUser(int id);
    void saveUser(User user);
    void deleteUser(int id);
}

// Implementations
class MySQLDatabase implements Database {
    @Override
    public User queryUser(int id) {
        System.out.println("Querying MySQL for user " + id);
        // MySQL-specific code
        return new User(id, "John from MySQL");
    }
    
    @Override
    public void saveUser(User user) {
        System.out.println("Saving to MySQL: " + user.getName());
    }
    
    @Override
    public void deleteUser(int id) {
        System.out.println("Deleting from MySQL: " + id);
    }
}

class PostgreSQLDatabase implements Database {
    @Override
    public User queryUser(int id) {
        System.out.println("Querying PostgreSQL for user " + id);
        return new User(id, "John from PostgreSQL");
    }
    
    @Override
    public void saveUser(User user) {
        System.out.println("Saving to PostgreSQL: " + user.getName());
    }
    
    @Override
    public void deleteUser(int id) {
        System.out.println("Deleting from PostgreSQL: " + id);
    }
}

class MongoDatabase implements Database {
    @Override
    public User queryUser(int id) {
        System.out.println("Querying MongoDB for user " + id);
        return new User(id, "John from MongoDB");
    }
    
    @Override
    public void saveUser(User user) {
        System.out.println("Saving to MongoDB: " + user.getName());
    }
    
    @Override
    public void deleteUser(int id) {
        System.out.println("Deleting from MongoDB: " + id);
    }
}

// High-level module
class UserService {
    private Database database; // Depends on abstraction!
    
    // Dependency injection via constructor
    public UserService(Database database) {
        this.database = database;
    }
    
    public User getUser(int id) {
        return database.queryUser(id);
    }
    
    public void createUser(User user) {
        database.saveUser(user);
    }
    
    public void removeUser(int id) {
        database.deleteUser(id);
    }
}

// User class
class User {
    private int id;
    private String name;
    
    public User(int id, String name) {
        this.id = id;
        this.name = name;
    }
    
    public int getId() { return id; }
    public String getName() { return name; }
}
```

### Usage - Easy to Switch Databases

```java
public class Main {
    public static void main(String[] args) {
        // Can easily switch database implementation
        Database mysqlDb = new MySQLDatabase();
        Database postgresDb = new PostgreSQLDatabase();
        Database mongoDb = new MongoDatabase();
        
        // Same service works with different databases
        System.out.println("=== Using MySQL ===");
        UserService service1 = new UserService(mysqlDb);
        User user1 = service1.getUser(1);
        System.out.println("Got: " + user1.getName());
        
        System.out.println("\n=== Using PostgreSQL ===");
        UserService service2 = new UserService(postgresDb);
        User user2 = service2.getUser(1);
        System.out.println("Got: " + user2.getName());
        
        System.out.println("\n=== Using MongoDB ===");
        UserService service3 = new UserService(mongoDb);
        User user3 = service3.getUser(1);
        System.out.println("Got: " + user3.getName());
        
        // Easy to test with mock database
        System.out.println("\n=== Testing with Mock ===");
        Database mockDb = new MockDatabase();
        UserService testService = new UserService(mockDb);
        testService.createUser(new User(99, "Test User"));
    }
}

// Mock for testing
class MockDatabase implements Database {
    @Override
    public User queryUser(int id) {
        return new User(id, "Mock User");
    }
    
    @Override
    public void saveUser(User user) {
        System.out.println("Mock save: " + user.getName());
    }
    
    @Override
    public void deleteUser(int id) {
        System.out.println("Mock delete: " + id);
    }
}
```

### Edge Cases & Best Practices

1. **Dependency Injection**: Use constructor injection, setter injection, or DI frameworks (Spring, Guice)
2. **Factory Pattern**: Creates objects that implement interfaces
3. **IoC Containers**: Spring, CDI manage dependencies automatically
4. **Testing**: Easily mock interfaces for unit tests
5. **Configuration**: Switch implementations via config files without code changes

---

## **SOLID Summary with Framework Examples**## **Tools and Frameworks Supporting SOLID**

### **1. Spring Framework (Java)**
- **@Component, @Service, @Repository, @Controller**: Enforces SRP by separating concerns
- **Dependency Injection (@Autowired)**: Implements DIP perfectly
- **AOP (Aspect-Oriented Programming)**: Separates cross-cutting concerns
- **Interfaces and Polymorphism**: Supports OCP and LSP

```java
@Service
public class UserService {
    private final UserRepository repository; // DIP - depends on interface
    
    @Autowired
    public UserService(UserRepository repository) {
        this.repository = repository;
    }
}

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    // Spring Data automatically implements this
}
```

### **2. Design Patterns**
- **Strategy Pattern**: OCP - add new strategies without modifying context
- **Factory Pattern**: DIP - creates objects through abstractions
- **Observer Pattern**: OCP - add observers without changing subject
- **Decorator Pattern**: OCP - add functionality without modification
- **Adapter Pattern**: LSP - makes incompatible interfaces compatible

### **3. Testing Frameworks**
- **JUnit/TestNG**: Tests each responsibility separately (SRP)
- **Mockito**: Mocks interfaces (DIP, ISP)
- **PowerMock**: Tests with dependency injection

```java
@Test
public void testUserService() {
    // Mock the dependency (DIP benefit)
    UserRepository mockRepo = Mockito.mock(UserRepository.class);
    when(mockRepo.findById(1L)).thenReturn(Optional.of(new User(1L, "John")));
    
    UserService service = new UserService(mockRepo);
    User user = service.getUser(1L);
    
    assertEquals("John", user.getName());
}
```

### **4. Build Tools**
- **Maven/Gradle**: Modular dependencies (DIP)
- **Multi-module projects**: Enforces SRP at module level

---

## **When NOT to Use SOLID**

1. **Simple scripts**: Overkill for one-off utilities
2. **Prototypes**: Premature abstraction slows exploration
3. **Performance-critical code**: Sometimes tight coupling is faster
4. **Over-engineering**: Don't create 10 classes for a 20-line feature
5. **Legacy code**: Gradual refactoring is better than rewrite

---

## **Complete Working Example - E-commerce System**

Here's a complete example demonstrating all SOLID principles together:

```java
// ===== INTERFACES (ISP + DIP) =====

interface Product {
    String getId();
    String getName();
    double getPrice();
}

interface PaymentProcessor {
    boolean processPayment(double amount);
}

interface OrderRepository {
    void save(Order order);
    Order findById(String id);
}

interface NotificationSender {
    void send(String message);
}

// ===== DOMAIN MODELS (SRP) =====

class PhysicalProduct implements Product {
    private String id;
    private String name;
    private double price;
    private double weight;
    
    public PhysicalProduct(String id, String name, double price, double weight) {
        this.id = id;
        this.name = name;
        this.price = price;
        this.weight = weight;
    }
    
    @Override
    public String getId() { return id; }
    @Override
    public String getName() { return name; }
    @Override
    public double getPrice() { return price; }
    public double getWeight() { return weight; }
}

class DigitalProduct implements Product {
    private String id;
    private String name;
    private double price;
    private String downloadUrl;
    
    public DigitalProduct(String id, String name, double price, String downloadUrl) {
        this.id = id;
        this.name = name;
        this.price = price;
        this.downloadUrl = downloadUrl;
    }
    
    @Override
    public String getId() { return id; }
    @Override
    public String getName() { return name; }
    @Override
    public double getPrice() { return price; }
    public String getDownloadUrl() { return downloadUrl; }
}

class Order {
    private String id;
    private List<Product> products;
    private double totalAmount;
    
    public Order(String id, List<Product> products) {
        this.id = id;
        this.products = products;
        this.totalAmount = calculateTotal();
    }
    
    private double calculateTotal() {
        return products.stream().mapToDouble(Product::getPrice).sum();
    }
    
    public String getId() { return id; }
    public List<Product> getProducts() { return products; }
    public double getTotalAmount() { return totalAmount; }
}

// ===== IMPLEMENTATIONS (OCP + LSP) =====

class CreditCardPayment implements PaymentProcessor {
    @Override
    public boolean processPayment(double amount) {
        System.out.println("Processing $" + amount + " via Credit Card");
        return true;
    }
}

class PayPalPayment implements PaymentProcessor {
    @Override
    public boolean processPayment(double amount) {
        System.out.println("Processing $" + amount + " via PayPal");
        return true;
    }
}

class DatabaseOrderRepository implements OrderRepository {
    @Override
    public void save(Order order) {
        System.out.println("Saving order " + order.getId() + " to database");
    }
    
    @Override
    public Order findById(String id) {
        System.out.println("Finding order " + id + " in database");
        return null;
    }
}

class EmailNotification implements NotificationSender {
    @Override
    public void send(String message) {
        System.out.println("Email: " + message);
    }
}

class SMSNotification implements NotificationSender {
    @Override
    public void send(String message) {
        System.out.println("SMS: " + message);
    }
}

// ===== HIGH-LEVEL SERVICE (DIP) =====

class OrderService {
    private final PaymentProcessor paymentProcessor;
    private final OrderRepository orderRepository;
    private final NotificationSender notificationSender;
    
    // Dependency Injection
    public OrderService(PaymentProcessor paymentProcessor,
                       OrderRepository orderRepository,
                       NotificationSender notificationSender) {
        this.paymentProcessor = paymentProcessor;
        this.orderRepository = orderRepository;
        this.notificationSender = notificationSender;
    }
    
    public void placeOrder(Order order) {
        // Process payment
        boolean paymentSuccess = paymentProcessor.processPayment(order.getTotalAmount());
        
        if (paymentSuccess) {
            // Save order
            orderRepository.save(order);
            
            // Send notification
            notificationSender.send("Order " + order.getId() + " placed successfully!");
            
            System.out.println("✓ Order completed: " + order.getId());
        } else {
            System.out.println("✗ Payment failed for order: " + order.getId());
        }
    }
}

// ===== USAGE =====

public class EcommerceDemo {
    public static void main(String[] args) {
        // Create products (LSP - both implement Product)
        List<Product> products = Arrays.asList(
            new PhysicalProduct("P001", "Laptop", 999.99, 2.5),
            new DigitalProduct("D001", "Software License", 49.99, "http://download.com/sw")
        );
        
        // Create order
        Order order = new Order("ORD-001", products);
        
        // Configure dependencies (DIP)
        PaymentProcessor payment = new CreditCardPayment();
        OrderRepository repository = new DatabaseOrderRepository();
        NotificationSender notification = new EmailNotification();
        
        // Create service with injected dependencies
        OrderService orderService = new OrderService(payment, repository, notification);
        
        // Place order
        orderService.placeOrder(order);
        
        System.out.println("\n=== Easy to change implementations ===");
        
        // Switch to different implementations without changing OrderService
        PaymentProcessor paypal = new PayPalPayment();
        NotificationSender sms = new SMSNotification();
        OrderService orderService2 = new OrderService(paypal, repository, sms);
        
        orderService2.placeOrder(order);
    }
}
```

**Output:**
```
Processing $1049.98 via Credit Card
Saving order ORD-001 to database
Email: Order ORD-001 placed successfully!
✓ Order completed: ORD-001

=== Easy to change implementations ===
Processing $1049.98 via PayPal
Saving order ORD-001 to database
SMS: Order ORD-001 placed successfully!
✓ Order completed: ORD-001
```

---

## **Key Takeaways**

✅ **SOLID makes code:**
- Easier to understand and maintain
- Easier to test
- Easier to extend with new features
- More resilient to change
- More reusable

✅ **Remember:**
- **S**: One class, one job
- **O**: Extend, don't modify
- **L**: Subtypes must substitute perfectly
- **I**: Many small interfaces beat one large
- **D**: Depend on abstractions, not details

✅ **Balance is key:**
- Don't over-engineer simple code
- Apply SOLID where it adds value
- Refactor toward SOLID as code grows
- Use frameworks that enforce good design

This comprehensive guide covers SOLID principles from basic definitions to advanced implementation patterns with real-world examples!