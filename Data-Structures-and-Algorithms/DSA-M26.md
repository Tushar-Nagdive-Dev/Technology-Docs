## 🚀 **Module 21: Behavioral Design Patterns**  

Behavioral design patterns focus on the interaction and responsibility between objects. They define how objects communicate, collaborate, and manage complex control flows, improving code organization and maintainability.

---

## **🔥 21.1 Why Learn Behavioral Design Patterns?**  
- **Communication and Interaction:** Simplify complex communication between objects.  
- **Maintainable Code:** Separate algorithms from object responsibilities.  
- **Flexible Control Flow:** Implement dynamic behavior changes at runtime.  
- **Decoupled Objects:** Reduce coupling between objects, enhancing flexibility.  
- **Reusable Behavior:** Reuse common behavior without duplicating code.  

---

## **🔥 21.2 Types of Behavioral Design Patterns**  
1. **Chain of Responsibility** — Pass requests along a chain of handlers.  
2. **Command** — Encapsulate requests as objects for parameterization and queuing.  
3. **Interpreter** — Define grammar and interpreter for a language.  
4. **Iterator** — Access elements sequentially without exposing underlying structure.  
5. **Mediator** — Centralize complex communications between objects.  
6. **Memento** — Capture and restore an object's internal state.  
7. **Observer** — Notify dependent objects of state changes.  
8. **State** — Allow an object to alter its behavior when its state changes.  
9. **Strategy** — Encapsulate algorithms within a class hierarchy.  
10. **Template Method** — Define the skeleton of an algorithm in a method.  
11. **Visitor** — Separate an algorithm from an object structure.  

---

## **🔥 21.3 Strategy Pattern**  
- **Definition:** Defines a family of algorithms, encapsulates each one, and makes them interchangeable. The strategy pattern lets the algorithm vary independently from clients that use it.  
- **Usage:**  
    - To choose an algorithm at runtime.  
    - To avoid multiple conditional statements.  
    - To encapsulate algorithms within a class hierarchy.  
- **Examples:**  
    - `Comparator` interface in Java Collections.  
    - `javax.crypto.Cipher` for encryption algorithms.  

---

### 📘 **Example Code: Strategy Pattern**  
- **Scenario:** Implement a payment processing system supporting multiple payment methods (Credit Card, PayPal).  
- **Problem:** The payment method needs to be chosen dynamically at runtime.  
- **Solution:** Use the Strategy Pattern to encapsulate payment algorithms as separate classes.  

```java
// Step 1: Strategy Interface
interface PaymentStrategy {
    void pay(double amount);
}

// Step 2: Concrete Strategy Classes
class CreditCardPayment implements PaymentStrategy {
    private String cardNumber;
    private String cardHolder;

    public CreditCardPayment(String cardNumber, String cardHolder) {
        this.cardNumber = cardNumber;
        this.cardHolder = cardHolder;
    }

    @Override
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " using Credit Card (" + cardHolder + ").");
    }
}

class PayPalPayment implements PaymentStrategy {
    private String email;

    public PayPalPayment(String email) {
        this.email = email;
    }

    @Override
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " using PayPal (" + email + ").");
    }
}

// Step 3: Context Class
class ShoppingCart {
    private PaymentStrategy paymentStrategy;

    public void setPaymentStrategy(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    public void checkout(double amount) {
        if (paymentStrategy == null) {
            throw new IllegalStateException("Payment method not set.");
        }
        paymentStrategy.pay(amount);
    }
}

// Step 4: Client Code
public class StrategyPatternExample {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();

        // Pay with Credit Card
        cart.setPaymentStrategy(new CreditCardPayment("1234-5678-9012-3456", "John Doe"));
        cart.checkout(250.75);

        // Pay with PayPal
        cart.setPaymentStrategy(new PayPalPayment("john@example.com"));
        cart.checkout(125.50);
    }
}
```

---

### 📊 **Output:**  
```
Paid $250.75 using Credit Card (John Doe).
Paid $125.50 using PayPal (john@example.com).
```

---

### 🔥 **Explanation:**  
- **Strategy Interface (`PaymentStrategy`):** Defines the payment method contract.  
- **Concrete Strategies (`CreditCardPayment`, `PayPalPayment`):** Implement specific payment methods.  
- **Context (`ShoppingCart`):** Maintains a reference to a `PaymentStrategy` object.  
- **Time Complexity:** `O(1)` — Constant time for payment processing.  
- **Space Complexity:** `O(1)` — Only one strategy instance is used at a time.  

---

### 📘 **When to Use Strategy Pattern?**  
- **Algorithm Selection:** When multiple algorithms can be applied to a problem.  
- **Avoid Conditional Statements:** Replace `if-else` or `switch` statements.  
- **Behavior Change at Runtime:** Change object behavior dynamically.  
- **Encapsulation of Algorithms:** Encapsulate related algorithms into a class hierarchy.  

---

## **🔥 21.4 Observer Pattern**  
- **Definition:** Defines a one-to-many relationship between objects so that when one object changes state, all its dependents are notified and updated automatically.  
- **Usage:**  
    - Implementing event listeners.  
    - Building publish-subscribe systems.  
    - Notifications and messaging systems.  
- **Examples:**  
    - **Java:** `java.util.Observer` and `Observable`.  
    - **Java Swing:** Event listeners in GUI components.  

---

### 📘 **Example Code: Observer Pattern**  
- **Scenario:** Implement a notification system for a stock price tracker.  
- **Problem:** Multiple investors should be notified when the stock price changes.  
- **Solution:** Use the Observer Pattern to notify investors automatically.  

```java
// Step 1: Observer Interface
interface Observer {
    void update(double stockPrice);
}

// Step 2: Concrete Observers
class InvestorA implements Observer {
    @Override
    public void update(double stockPrice) {
        System.out.println("Investor A: Stock price updated to $" + stockPrice);
    }
}

class InvestorB implements Observer {
    @Override
    public void update(double stockPrice) {
        System.out.println("Investor B: Stock price updated to $" + stockPrice);
    }
}

// Step 3: Subject Interface
interface Stock {
    void addObserver(Observer observer);
    void removeObserver(Observer observer);
    void notifyObservers();
}

// Step 4: Concrete Subject
class StockPrice implements Stock {
    private List<Observer> observers = new ArrayList<>();
    private double price;

    public void setPrice(double price) {
        this.price = price;
        notifyObservers();
    }

    @Override
    public void addObserver(Observer observer) {
        observers.add(observer);
    }

    @Override
    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    @Override
    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(price);
        }
    }
}

// Step 5: Client Code
public class ObserverPatternExample {
    public static void main(String[] args) {
        StockPrice stock = new StockPrice();

        Observer investorA = new InvestorA();
        Observer investorB = new InvestorB();

        stock.addObserver(investorA);
        stock.addObserver(investorB);

        stock.setPrice(100.50);
        stock.setPrice(101.75);
    }
}
```

---

### 📊 **Output:**  
```
Investor A: Stock price updated to $100.5
Investor B: Stock price updated to $100.5
Investor A: Stock price updated to $101.75
Investor B: Stock price updated to $101.75
```

---

## 🔥 **Next: Design Patterns Best Practices**  
Next, we will explore **Design Patterns Best Practices** and guidelines for choosing and implementing patterns effectively.

---
