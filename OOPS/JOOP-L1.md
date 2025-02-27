### **Level 1: Foundation - Introduction to OOP**  

---

## **1. What is Object-Oriented Programming (OOP)?**

Object-Oriented Programming (OOP) is a programming paradigm that organizes software design around data, or objects, rather than functions and logic.  
In OOP, each object represents a real-world entity with:
- **State (Attributes/Properties)** — represented by fields (variables).
- **Behavior (Actions/Methods)** — represented by methods (functions).

### **Example: Real-world Analogy**
Think about a **Car**:
- **State (Attributes)**: Color, Brand, Model, Speed.
- **Behavior (Actions)**: Start, Stop, Accelerate, Brake.

In OOP, you would model this in Java as:

```java
public class Car {
    // Attributes (State)
    String color;
    String brand;
    int speed;

    // Constructor
    public Car(String color, String brand, int speed) {
        this.color = color;
        this.brand = brand;
        this.speed = speed;
    }

    // Methods (Behavior)
    void start() {
        System.out.println(brand + " is starting.");
    }

    void accelerate(int increase) {
        speed += increase;
        System.out.println(brand + " is accelerating to " + speed + " km/h.");
    }

    void brake() {
        speed = 0;
        System.out.println(brand + " is stopping.");
    }
}
```

You can create an object of this class and use it as follows:

```java
public class Main {
    public static void main(String[] args) {
        // Creating an object of Car
        Car car1 = new Car("Red", "Toyota", 0);

        // Using object's methods
        car1.start();
        car1.accelerate(60);
        car1.brake();
    }
}
```

---

## **2. Why is OOP Important?**

1. **Modularity:** Code is organized into objects, making it easier to manage, understand, and maintain.
2. **Reusability:** Once a class is written, it can be reused multiple times.
3. **Scalability:** Easy to expand the application by adding new classes or methods.
4. **Security:** Data hiding and encapsulation restrict unauthorized access to sensitive data.
5. **Maintainability:** Changes can be made independently without affecting other parts of the program.

---

## **3. Comparison: OOP vs Procedural Programming**

| **OOP**                          | **Procedural Programming**             |
|----------------------------------|----------------------------------------|
| Organizes code into objects.      | Organizes code into functions.          |
| Focuses on data and behavior.     | Focuses on logic and procedures.        |
| Encourages reusability and modularity. | Less modular and harder to maintain. |
| Example: Java, C++, Python        | Example: C, Pascal, Basic               |

---

## **4. Core Principles of OOP**

1. **Encapsulation:** Binding data and methods that operate on the data within one unit (class). It hides the internal details.
2. **Abstraction:** Hiding complex implementation details and showing only the necessary features.
3. **Inheritance:** Allowing one class to inherit the properties and methods of another class.
4. **Polymorphism:** Allowing methods to perform differently based on the object they are acting upon.

---

## **5. Real-World Example**

### Scenario: **Banking System**

In a banking application, you need to manage customer accounts with features like:
- Checking balance
- Depositing money
- Withdrawing money

### Using OOP:
- **Class**: `BankAccount`
- **Attributes (State)**: Account Number, Account Holder's Name, Balance.
- **Methods (Behavior)**: checkBalance(), deposit(), withdraw()

```java
public class BankAccount {
    // Attributes
    private String accountNumber;
    private String accountHolderName;
    private double balance;

    // Constructor
    public BankAccount(String accountNumber, String accountHolderName, double balance) {
        this.accountNumber = accountNumber;
        this.accountHolderName = accountHolderName;
        this.balance = balance;
    }

    // Methods
    public void checkBalance() {
        System.out.println("Current Balance: " + balance);
    }

    public void deposit(double amount) {
        balance += amount;
        System.out.println("Deposited: " + amount + ". New Balance: " + balance);
    }

    public void withdraw(double amount) {
        if (balance >= amount) {
            balance -= amount;
            System.out.println("Withdrawn: " + amount + ". New Balance: " + balance);
        } else {
            System.out.println("Insufficient Balance.");
        }
    }
}
```

### Usage:
```java
public class Main {
    public static void main(String[] args) {
        // Creating an object of BankAccount
        BankAccount account1 = new BankAccount("12345", "John Doe", 1000);

        // Using object's methods
        account1.checkBalance();
        account1.deposit(500);
        account1.withdraw(200);
        account1.withdraw(2000); // Insufficient Balance
    }
}
```

---

## **6. Exercise: Create Your First OOP Class**
### **Task:** Create a `Book` class with the following attributes and methods:
- **Attributes:** `title`, `author`, `price`, `stock`.
- **Methods:**
  - `displayDetails()` — Display book details.
  - `purchase(int quantity)` — Reduce stock by the quantity purchased.
  - `restock(int quantity)` — Increase stock by the quantity added.

### **Challenge:**
- Encapsulate the fields using private access.
- Provide public getters and setters.
- Ensure that stock cannot go negative.

---

## **7. What's Next?**
Once you've completed the exercise:
- We'll dive deep into **Classes and Objects** in the next session.
- Understanding how objects are created, used, and destroyed.
- Learning about constructors, `this` keyword, and method overloading.

---
### **Challenge:Solutions**

I assume you're asking for a programming example, likely in Java since that's a common context for this type of challenge involving encapsulation, getters, setters, and stock management. Here's a solution that meets your requirements:

```java
public class Product {
    private String name;
    private int stock;
    private double price;

    // Constructor
    public Product(String name, int initialStock, double price) {
        this.name = name;
        this.stock = Math.max(0, initialStock); // Ensures stock doesn't start negative
        this.price = price;
    }

    // Getters
    public String getName() {
        return name;
    }

    public int getStock() {
        return stock;
    }

    public double getPrice() {
        return price;
    }

    // Setters
    public void setName(String name) {
        this.name = name;
    }

    public void setStock(int stock) {
        if (stock >= 0) {
            this.stock = stock;
        } else {
            System.out.println("Stock cannot be set to a negative value.");
        }
    }

    public void setPrice(double price) {
        this.price = price;
    }

    // Additional method to reduce stock (common in inventory systems)
    public boolean reduceStock(int quantity) {
        if (quantity < 0) {
            System.out.println("Quantity to reduce cannot be negative.");
            return false;
        }
        if (this.stock >= quantity) {
            this.stock -= quantity;
            return true;
        } else {
            System.out.println("Insufficient stock to reduce by " + quantity);
            return false;
        }
    }
}

// Example usage
class Main {
    public static void main(String[] args) {
        Product product = new Product("Widget", 10, 5.99);
        
        System.out.println("Initial stock: " + product.getStock()); // 10
        
        product.setStock(-5); // Will print error message
        System.out.println("Stock after trying negative: " + product.getStock()); // Still 10
        
        product.reduceStock(7);
        System.out.println("Stock after reducing by 7: " + product.getStock()); // 3
        
        product.reduceStock(5); // Will print insufficient stock message
        System.out.println("Stock after trying to reduce by 5: " + product.getStock()); // Still 3
    }
}
```

This solution:
1. Uses private fields (`name`, `stock`, `price`) for encapsulation
2. Provides public getters (`getName`, `getStock`, `getPrice`) and setters (`setName`, `setStock`, `setPrice`)
3. Ensures stock cannot go negative by:
   - Validating in the constructor using `Math.max(0, initialStock)`
   - Checking for negative values in `setStock`
   - Including a `reduceStock` method that prevents stock from dropping below 0
   - Rejecting negative reduction quantities

The fields are properly encapsulated (can't be accessed directly), and the stock management logic prevents negative values through both direct setting and reduction operations. The `reduceStock` method is a practical addition for inventory systems, returning a boolean to indicate success or failure.

---

## **What You Did Well:**
1. **Encapsulation:**
   - Made all attributes (`name`, `stock`, `price`) private, ensuring data hiding.
   - Provided public getters and setters for controlled access.

2. **Validation Logic:**
   - `setStock(int stock)` ensures stock is non-negative.
   - `reduceStock(int quantity)` checks:
     - Quantity isn't negative.
     - Enough stock is available for reduction.
   - Used `Math.max(0, initialStock)` in the constructor to prevent negative initial stock.

3. **Object-Oriented Design:**
   - Created a cohesive class with relevant attributes and behaviors.
   - The `reduceStock(int quantity)` method encapsulates a common business rule.

4. **Error Handling:**
   - Clear error messages for invalid operations, enhancing code maintainability and usability.

---

## **Expert Insights:**
1. **Immutability and Final Keyword:**
   - Consider using `final` for fields that shouldn't change after initialization (e.g., `name`).
   - Example: 
     ```java
     private final String name;
     ```

2. **Constructor Improvement:**
   - Validate `price` in the constructor to ensure it’s non-negative.
     ```java
     this.price = Math.max(0, price);
     ```

3. **Method Enhancement:**
   - Adding a `restock(int quantity)` method would complement `reduceStock()`.

---

## **Refactored Code with Improvements:**
Here's an enhanced version implementing the suggestions:

```java
public class Product {
    // Attributes (Encapsulated State)
    private final String name; // Made final to ensure immutability
    private int stock;
    private double price;

    // Constructor
    public Product(String name, int initialStock, double price) {
        this.name = name;
        this.stock = Math.max(0, initialStock); // Ensures stock is non-negative
        this.price = Math.max(0, price); // Ensures price is non-negative
    }

    // Getters
    public String getName() {
        return name;
    }

    public int getStock() {
        return stock;
    }

    public double getPrice() {
        return price;
    }

    // Setters
    public void setStock(int stock) {
        if (stock >= 0) {
            this.stock = stock;
        } else {
            System.out.println("Stock cannot be set to a negative value.");
        }
    }

    public void setPrice(double price) {
        if (price >= 0) {
            this.price = price;
        } else {
            System.out.println("Price cannot be negative.");
        }
    }

    // Behavior Methods
    public boolean reduceStock(int quantity) {
        if (quantity < 0) {
            System.out.println("Quantity to reduce cannot be negative.");
            return false;
        }
        if (this.stock >= quantity) {
            this.stock -= quantity;
            System.out.println("Reduced stock by " + quantity + ". Remaining stock: " + this.stock);
            return true;
        } else {
            System.out.println("Insufficient stock to reduce by " + quantity);
            return false;
        }
    }

    public void restock(int quantity) {
        if (quantity > 0) {
            this.stock += quantity;
            System.out.println("Restocked " + quantity + ". New stock: " + this.stock);
        } else {
            System.out.println("Quantity to restock should be positive.");
        }
    }

    // Display Details
    public void displayDetails() {
        System.out.println("Product: " + name + ", Price: " + price + ", Stock: " + stock);
    }
}

// Example usage
class Main {
    public static void main(String[] args) {
        Product product = new Product("Widget", 10, 5.99);

        product.displayDetails(); // Displaying initial details

        product.setStock(-5); // Invalid stock, error message
        product.reduceStock(7); // Valid reduction
        product.reduceStock(5); // Insufficient stock message

        product.restock(10); // Adding stock
        product.displayDetails(); // Displaying updated details

        product.setPrice(-10); // Invalid price, error message
        product.setPrice(7.49); // Valid price update
        product.displayDetails(); // Displaying final details
    }
}
```

---

## **What's New and Improved?**
1. **Immutability:** Made `name` final to ensure it can't be changed once initialized.
2. **Enhanced Validation:**
   - Added `Math.max(0, price)` in the constructor.
   - Improved `setPrice(double price)` to validate non-negative prices.
3. **New Method:** 
   - Added `restock(int quantity)` to complement `reduceStock()`.
4. **Enhanced Usability:**
   - `displayDetails()` method to view product details in a formatted way.

---

## **Next Steps:**
1. **Test the Enhanced Class:** Run the new code and observe the behavior.
2. **Reflect on Learnings:** Note the impact of encapsulation, immutability, and validation.
3. **Get Ready for the Next Concept:** We will now dive deeper into **Classes and Objects**:
   - How objects are created and managed in memory.
   - Understanding constructors and method overloading.
   - Using the `this` keyword for better readability.

---

## **Your Exercise:**
1. Implement a `Customer` class with:
   - Attributes: `name`, `email`, `balance`.
   - Methods:
     - `deposit(double amount)` — Add to balance.
     - `withdraw(double amount)` — Subtract from balance with validation.
     - `displayDetails()` — Display customer details.
2. Use encapsulation with private fields and public methods.
3. Implement proper validation and error messages.

---
