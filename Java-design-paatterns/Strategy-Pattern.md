# **🚀 Lesson 14: Strategy Pattern – Flexible Behavior Selection at Runtime**

The **Strategy Pattern** is a **behavioral design pattern** that enables selecting **an algorithm at runtime** without modifying the client code.

---

## **📌 1. What is the Strategy Pattern?**
The **Strategy Pattern**:
✔ **Defines multiple algorithms** within separate classes.  
✔ Allows **switching algorithms dynamically at runtime**.  
✔ **Decouples the behavior from the context** (client code).  

---

## **📌 2. When to Use the Strategy Pattern?**
✔ When you need **multiple algorithms for the same task**.  
✔ When **if-else or switch statements** are used to select algorithms.  
✔ When following the **Open/Closed Principle** (new strategies can be added without modifying existing code).  
✔ When different **clients need different behaviors dynamically**.  

---

## **📌 3. Real-World Analogy – Payment Methods 💳**
- A shopping app allows users to **pay via Credit Card, PayPal, or Bitcoin**.
- The **payment process remains the same**, but the **method varies**.
- The **Strategy Pattern allows switching payment methods at runtime**.

---

## **📌 4. Implementing Strategy Pattern in Java**
Let’s build a **Payment Processing System** where **users can switch payment methods dynamically**.

---

### **Step 1: Create the Strategy Interface**
This interface defines **multiple payment strategies**.

```java
// Strategy Interface (Defines different payment methods)
public interface PaymentStrategy {
    void pay(double amount);
}
```

---

### **Step 2: Implement Concrete Strategies**
These classes **provide different payment behaviors**.

#### **Credit Card Payment Strategy**
```java
// Concrete Strategy 1: Credit Card Payment
public class CreditCardPayment implements PaymentStrategy {
    private String cardNumber;

    public CreditCardPayment(String cardNumber) {
        this.cardNumber = cardNumber;
    }

    @Override
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " using Credit Card: " + cardNumber);
    }
}
```

#### **PayPal Payment Strategy**
```java
// Concrete Strategy 2: PayPal Payment
public class PayPalPayment implements PaymentStrategy {
    private String email;

    public PayPalPayment(String email) {
        this.email = email;
    }

    @Override
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " using PayPal: " + email);
    }
}
```

#### **Bitcoin Payment Strategy**
```java
// Concrete Strategy 3: Bitcoin Payment
public class BitcoinPayment implements PaymentStrategy {
    private String walletAddress;

    public BitcoinPayment(String walletAddress) {
        this.walletAddress = walletAddress;
    }

    @Override
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " using Bitcoin Wallet: " + walletAddress);
    }
}
```
✔ Each **strategy class implements `PaymentStrategy` independently**.  
✔ **Clients can choose any payment method dynamically**.  

---

### **Step 3: Create the Context Class**
This class allows **switching strategies at runtime**.

```java
// Context Class: Shopping Cart
public class ShoppingCart {
    private PaymentStrategy paymentStrategy;

    // Set payment method dynamically
    public void setPaymentStrategy(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    // Perform payment
    public void checkout(double amount) {
        if (paymentStrategy == null) {
            System.out.println("No payment method selected!");
        } else {
            paymentStrategy.pay(amount);
        }
    }
}
```
✔ **Holds a reference to a `PaymentStrategy`** (can be changed anytime).  
✔ Calls `pay()` on the selected strategy at **checkout**.  

---

### **Step 4: Using the Strategy Pattern**
```java
public class Main {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();

        // Pay using Credit Card
        cart.setPaymentStrategy(new CreditCardPayment("1234-5678-9012-3456"));
        cart.checkout(100.0);

        // Switch to PayPal
        cart.setPaymentStrategy(new PayPalPayment("tushar@example.com"));
        cart.checkout(50.0);

        // Switch to Bitcoin
        cart.setPaymentStrategy(new BitcoinPayment("bc1qwxyz..."));
        cart.checkout(75.0);
    }
}
```

---

## **📌 5. Expected Output**
```
Paid $100.0 using Credit Card: 1234-5678-9012-3456
Paid $50.0 using PayPal: tushar@example.com
Paid $75.0 using Bitcoin Wallet: bc1qwxyz...
```
✔ **Dynamically switches between payment methods**.  
✔ **No modifications were needed in `ShoppingCart` to add new payment methods**.  

---

## **📌 6. Key Features of the Strategy Pattern**
| **Feature** | **Explanation** |
|------------|----------------|
| **Encapsulates Different Behaviors** | Defines multiple algorithms in separate classes. |
| **Promotes Open/Closed Principle** | New strategies can be added **without modifying the existing code**. |
| **Allows Dynamic Strategy Selection** | Algorithms can be **switched at runtime**. |
| **Avoids Complex If-Else Conditions** | Eliminates long `if-else` or `switch` statements. |

---

## **📌 7. Common Mistakes & How to Avoid Them**
❌ **Not making strategies interchangeable** – Ensure all strategies implement the same interface.  
❌ **Using an if-else block instead of Strategy Pattern** – Always **delegate behavior to the selected strategy**.  
❌ **Tightly coupling the Context with a specific Strategy** – Always **set the strategy dynamically**.  

---

## **🔥 Hands-On Challenge**
✔ Implement a **Sorting System** where `BubbleSort`, `QuickSort`, and `MergeSort` can be switched dynamically.  
✔ Implement a **Compression System** where files can be **compressed using ZIP, RAR, or TAR** strategies.  

---

## **🚀 Summary**
| **Concept** | **Explanation** |
|------------|----------------|
| **Strategy Pattern** | Allows selecting an **algorithm dynamically at runtime**. |
| **Context Class** | Holds the current strategy (`ShoppingCart`). |
| **Concrete Strategy** | Implements specific behavior (`CreditCardPayment`, `PayPalPayment`). |
| **Example** | **Payment methods in a shopping cart**. |

---
