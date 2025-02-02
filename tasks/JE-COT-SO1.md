
### **Goal 1: Single Responsibility Principle (SRP)**

#### Task:
Create a `Product` class that only handles product-related attributes and a separate `ProductPrinter` class to display product information.

#### Solution:
```java
// Product class responsible only for product details
class Product {
    private String productId;
    private String name;
    private double price;

    public Product(String productId, String name, double price) {
        this.productId = productId;
        this.name = name;
        this.price = price;
    }

    // Getters and setters
    public String getProductId() { return productId; }
    public String getName() { return name; }
    public double getPrice() { return price; }
}

// ProductPrinter class responsible for displaying product information
class ProductPrinter {
    public void printProductDetails(Product product) {
        System.out.println("Product ID: " + product.getProductId());
        System.out.println("Name: " + product.getName());
        System.out.println("Price: $" + product.getPrice());
    }
}

// Example usage
public class Main {
    public static void main(String[] args) {
        Product product = new Product("P101", "Laptop", 999.99);
        ProductPrinter printer = new ProductPrinter();
        printer.printProductDetails(product);
    }
}
```

---

### **Goal 2: Open/Closed Principle (OCP)**

#### Task:
Implement a discount system where new discount types can be added without modifying existing code.

#### Solution:
```java
// Discount interface
interface Discount {
    double applyDiscount(double price);
}

// FlatDiscount implementation
class FlatDiscount implements Discount {
    private double discountAmount;

    public FlatDiscount(double discountAmount) {
        this.discountAmount = discountAmount;
    }

    @Override
    public double applyDiscount(double price) {
        return price - discountAmount;
    }
}

// PercentageDiscount implementation
class PercentageDiscount implements Discount {
    private double discountPercentage;

    public PercentageDiscount(double discountPercentage) {
        this.discountPercentage = discountPercentage;
    }

    @Override
    public double applyDiscount(double price) {
        return price * (1 - discountPercentage / 100);
    }
}

// Product class with discount application
class Product {
    private String name;
    private double price;

    public Product(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public double getPrice() { return price; }

    public double getDiscountedPrice(Discount discount) {
        return discount.applyDiscount(price);
    }
}

// Example usage
public class Main {
    public static void main(String[] args) {
        Product product = new Product("Laptop", 999.99);

        Discount flatDiscount = new FlatDiscount(50);
        System.out.println("Price after flat discount: $" + product.getDiscountedPrice(flatDiscount));

        Discount percentageDiscount = new PercentageDiscount(10);
        System.out.println("Price after percentage discount: $" + product.getDiscountedPrice(percentageDiscount));
    }
}
```

---

### **Goal 3: Liskov Substitution Principle (LSP)**

#### Task:
Create a hierarchy of payment methods that can be used interchangeably.

#### Solution:
```java
// Base class for PaymentMethod
abstract class PaymentMethod {
    public abstract void pay(double amount);
}

// CreditCard implementation
class CreditCard extends PaymentMethod {
    @Override
    public void pay(double amount) {
        System.out.println("Paying $" + amount + " via Credit Card.");
    }
}

// PayPal implementation
class PayPal extends PaymentMethod {
    @Override
    public void pay(double amount) {
        System.out.println("Paying $" + amount + " via PayPal.");
    }
}

// PaymentProcessor class
class PaymentProcessor {
    public void processPayment(PaymentMethod paymentMethod, double amount) {
        paymentMethod.pay(amount);
    }
}

// Example usage
public class Main {
    public static void main(String[] args) {
        PaymentProcessor processor = new PaymentProcessor();

        PaymentMethod creditCard = new CreditCard();
        processor.processPayment(creditCard, 100);

        PaymentMethod payPal = new PayPal();
        processor.processPayment(payPal, 200);
    }
}
```

---

### **Goal 4: Interface Segregation Principle (ISP)**

#### Task:
Design interfaces for user roles to ensure they only have relevant methods.

#### Solution:
```java
// Customer interface
interface Customer {
    void viewProducts();
    void addToCart();
}

// Admin interface
interface Admin {
    void addProduct();
    void removeProduct();
}

// User class implementing Customer interface
class User implements Customer {
    @Override
    public void viewProducts() {
        System.out.println("Viewing products...");
    }

    @Override
    public void addToCart() {
        System.out.println("Adding product to cart...");
    }
}

// AdminUser class implementing Admin interface
class AdminUser implements Admin {
    @Override
    public void addProduct() {
        System.out.println("Adding product...");
    }

    @Override
    public void removeProduct() {
        System.out.println("Removing product...");
    }
}

// Example usage
public class Main {
    public static void main(String[] args) {
        Customer customer = new User();
        customer.viewProducts();
        customer.addToCart();

        Admin admin = new AdminUser();
        admin.addProduct();
        admin.removeProduct();
    }
}
```

---

### **Goal 5: Dependency Inversion Principle (DIP)**

#### Task:
Implement a notification system where the e-commerce platform can send notifications via email or SMS without tightly coupling the classes.

#### Solution:
```java
// NotificationService interface
interface NotificationService {
    void sendNotification(String message);
}

// EmailNotification implementation
class EmailNotification implements NotificationService {
    @Override
    public void sendNotification(String message) {
        System.out.println("Sending email: " + message);
    }
}

// SmsNotification implementation
class SmsNotification implements NotificationService {
    @Override
    public void sendNotification(String message) {
        System.out.println("Sending SMS: " + message);
    }
}

// OrderService class depending on NotificationService interface
class OrderService {
    private NotificationService notificationService;

    public OrderService(NotificationService notificationService) {
        this.notificationService = notificationService;
    }

    public void placeOrder() {
        System.out.println("Order placed!");
        notificationService.sendNotification("Your order has been placed.");
    }
}

// Example usage
public class Main {
    public static void main(String[] args) {
        NotificationService emailNotification = new EmailNotification();
        OrderService orderService = new OrderService(emailNotification);
        orderService.placeOrder();

        NotificationService smsNotification = new SmsNotification();
        orderService = new OrderService(smsNotification);
        orderService.placeOrder();
    }
}
```

---

### **Goal 6: Design Pattern - Singleton**

#### Task:
Ensure that the e-commerce platform’s `Cart` class is a singleton.

#### Solution:
```java
// Singleton Cart class
class Cart {
    private static Cart instance;
    private Cart() {} // Private constructor

    public static Cart getInstance() {
        if (instance == null) {
            instance = new Cart();
        }
        return instance;
    }

    public void addProduct(String product) {
        System.out.println("Added " + product + " to cart.");
    }
}

// Example usage
public class Main {
    public static void main(String[] args) {
        Cart cart1 = Cart.getInstance();
        cart1.addProduct("Laptop");

        Cart cart2 = Cart.getInstance();
        cart2.addProduct("Phone");

        System.out.println(cart1 == cart2); // true, same instance
    }
}
```

---

### **Goal 7: Design Pattern - Observer**

#### Task:
Implement a notification system where users are notified when a product is back in stock.

#### Solution:
```java
import java.util.ArrayList;
import java.util.List;

// Observer interface
interface Observer {
    void update(String productName);
}

// Subject (Product) class
class Product {
    private String name;
    private boolean inStock;
    private List<Observer> observers = new ArrayList<>();

    public Product(String name) {
        this.name = name;
    }

    public void setInStock(boolean inStock) {
        this.inStock = inStock;
        if (inStock) {
            notifyObservers();
        }
    }

    public void addObserver(Observer observer) {
        observers.add(observer);
    }

    private void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(name);
        }
    }
}

// User class implementing Observer
class User implements Observer {
    private String name;

    public User(String name) {
        this.name = name;
    }

    @Override
    public void update(String productName) {
        System.out.println(name + ", " + productName + " is back in stock!");
    }
}

// Example usage
public class Main {
    public static void main(String[] args) {
        Product laptop = new Product("Laptop");

        User user1 = new User("Alice");
        User user2 = new User("Bob");

        laptop.addObserver(user1);
        laptop.addObserver(user2);

        laptop.setInStock(true); // Notifies all observers
    }
}
```
