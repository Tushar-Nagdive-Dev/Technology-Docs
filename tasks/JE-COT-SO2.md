## **Task 1: Notification System Using Observer & Dependency Inversion**

### **Solution**

1. **Define a common interface for notification channels (DIP & ISP):**

```java
public interface INotificationChannel {
    void send(String message);
}
```

2. **Implement concrete channels:**

```java
public class EmailNotifier implements INotificationChannel {
    @Override
    public void send(String message) {
        System.out.println("Sending Email: " + message);
    }
}

public class SMSNotifier implements INotificationChannel {
    @Override
    public void send(String message) {
        System.out.println("Sending SMS: " + message);
    }
}

public class PushNotifier implements INotificationChannel {
    @Override
    public void send(String message) {
        System.out.println("Sending Push Notification: " + message);
    }
}
```

3. **Implement the NotificationManager using the Observer Pattern:**

```java
import java.util.ArrayList;
import java.util.List;

public class NotificationManager {
    private List<INotificationChannel> channels = new ArrayList<>();

    public void registerChannel(INotificationChannel channel) {
        channels.add(channel);
    }

    public void unregisterChannel(INotificationChannel channel) {
        channels.remove(channel);
    }

    public void broadcast(String message) {
        for (INotificationChannel channel : channels) {
            channel.send(message);
        }
    }
}
```

4. **Test the notification system:**

```java
public class NotificationTest {
    public static void main(String[] args) {
        NotificationManager manager = new NotificationManager();
        manager.registerChannel(new EmailNotifier());
        manager.registerChannel(new SMSNotifier());
        manager.registerChannel(new PushNotifier());

        manager.broadcast("System maintenance scheduled at midnight.");
    }
}
```

---

## **Task 2: Order Processing System Using SRP & Factory Pattern**

### **Solution**

1. **Define an abstract Order class (SRP):**

```java
public abstract class Order {
    protected String orderId;
    protected double amount;

    public Order(String orderId, double amount) {
        this.orderId = orderId;
        this.amount = amount;
    }

    public abstract void processOrder();
    public abstract boolean validate();
}
```

2. **Implement concrete orders:**

```java
public class DigitalOrder extends Order {
    public DigitalOrder(String orderId, double amount) {
        super(orderId, amount);
    }

    @Override
    public void processOrder() {
        System.out.println("Processing digital order: " + orderId);
    }

    @Override
    public boolean validate() {
        return amount > 0;  // Example validation
    }
}

public class PhysicalOrder extends Order {
    public PhysicalOrder(String orderId, double amount) {
        super(orderId, amount);
    }

    @Override
    public void processOrder() {
        System.out.println("Processing physical order: " + orderId);
    }

    @Override
    public boolean validate() {
        return amount > 0;  // Additional validations could be added here
    }
}
```

3. **Implement the OrderFactory (Factory Pattern):**

```java
public class OrderFactory {
    public static Order createOrder(String type, String orderId, double amount) {
        if ("digital".equalsIgnoreCase(type)) {
            return new DigitalOrder(orderId, amount);
        } else if ("physical".equalsIgnoreCase(type)) {
            return new PhysicalOrder(orderId, amount);
        }
        throw new IllegalArgumentException("Unknown order type: " + type);
    }
}
```

4. **Test the order processing system:**

```java
public class OrderProcessingTest {
    public static void main(String[] args) {
        Order order1 = OrderFactory.createOrder("digital", "D001", 100.0);
        if (order1.validate()) {
            order1.processOrder();
        }

        Order order2 = OrderFactory.createOrder("physical", "P001", 150.0);
        if (order2.validate()) {
            order2.processOrder();
        }
    }
}
```

---

## **Task 3: Report Generation System Using OCP & Strategy Pattern**

### **Solution**

1. **Define the strategy interface for report formatting:**

```java
public interface ReportFormatter {
    String format(String content);
}
```

2. **Implement concrete formatting strategies:**

```java
public class PdfFormatter implements ReportFormatter {
    @Override
    public String format(String content) {
        return "PDF Format: " + content;
    }
}

public class HtmlFormatter implements ReportFormatter {
    @Override
    public String format(String content) {
        return "<html><body>" + content + "</body></html>";
    }
}

public class CsvFormatter implements ReportFormatter {
    @Override
    public String format(String content) {
        return "CSV," + content;
    }
}
```

3. **Implement the ReportGenerator that uses a formatter (Strategy Pattern):**

```java
public class ReportGenerator {
    private ReportFormatter formatter;
    
    public ReportGenerator(ReportFormatter formatter) {
        this.formatter = formatter;
    }
    
    public void generateReport(String content) {
        String formattedReport = formatter.format(content);
        System.out.println("Generated Report: ");
        System.out.println(formattedReport);
    }
}
```

4. **Test the report generation:**

```java
public class ReportGenerationTest {
    public static void main(String[] args) {
        ReportGenerator pdfGenerator = new ReportGenerator(new PdfFormatter());
        pdfGenerator.generateReport("Annual Report Content");
        
        ReportGenerator htmlGenerator = new ReportGenerator(new HtmlFormatter());
        htmlGenerator.generateReport("Annual Report Content");
        
        ReportGenerator csvGenerator = new ReportGenerator(new CsvFormatter());
        csvGenerator.generateReport("Annual Report Content");
    }
}
```

---

## **Task 4: Parking Lot Management System Using LSP & Template Method**

### **Solution**

1. **Define an abstract Vehicle class (LSP):**

```java
public abstract class Vehicle {
    protected String licensePlate;
    
    public Vehicle(String licensePlate) {
        this.licensePlate = licensePlate;
    }
    
    public abstract void park();
    public abstract void unpark();
}
```

2. **Implement concrete vehicle types:**

```java
public class Car extends Vehicle {
    public Car(String licensePlate) {
        super(licensePlate);
    }
    
    @Override
    public void park() {
        System.out.println("Parking car with plate: " + licensePlate);
    }
    
    @Override
    public void unpark() {
        System.out.println("Unparking car with plate: " + licensePlate);
    }
}

public class Motorcycle extends Vehicle {
    public Motorcycle(String licensePlate) {
        super(licensePlate);
    }
    
    @Override
    public void park() {
        System.out.println("Parking motorcycle with plate: " + licensePlate);
    }
    
    @Override
    public void unpark() {
        System.out.println("Unparking motorcycle with plate: " + licensePlate);
    }
}

public class Truck extends Vehicle {
    public Truck(String licensePlate) {
        super(licensePlate);
    }
    
    @Override
    public void park() {
        System.out.println("Parking truck with plate: " + licensePlate);
    }
    
    @Override
    public void unpark() {
        System.out.println("Unparking truck with plate: " + licensePlate);
    }
}
```

3. **Create a template for parking operations (Template Method Pattern):**

```java
public abstract class ParkingOperationTemplate {
    public final void execute(Vehicle vehicle) {
        preOperation(vehicle);
        performOperation(vehicle);
        postOperation(vehicle);
    }
    
    protected void preOperation(Vehicle vehicle) {
        System.out.println("Preparing for operation for vehicle: " + vehicle.licensePlate);
    }
    
    protected abstract void performOperation(Vehicle vehicle);
    
    protected void postOperation(Vehicle vehicle) {
        System.out.println("Operation completed for vehicle: " + vehicle.licensePlate);
    }
}
```

4. **Implement concrete operations:**

```java
public class ParkOperation extends ParkingOperationTemplate {
    @Override
    protected void performOperation(Vehicle vehicle) {
        vehicle.park();
    }
}

public class UnparkOperation extends ParkingOperationTemplate {
    @Override
    protected void performOperation(Vehicle vehicle) {
        vehicle.unpark();
    }
}
```

5. **Test the parking operations:**

```java
public class ParkingTest {
    public static void main(String[] args) {
        Vehicle car = new Car("ABC-123");
        Vehicle motorcycle = new Motorcycle("XYZ-987");
        Vehicle truck = new Truck("TRK-456");
        
        ParkingOperationTemplate parkOperation = new ParkOperation();
        parkOperation.execute(car);
        parkOperation.execute(motorcycle);
        parkOperation.execute(truck);
        
        ParkingOperationTemplate unparkOperation = new UnparkOperation();
        unparkOperation.execute(car);
        unparkOperation.execute(motorcycle);
        unparkOperation.execute(truck);
    }
}
```

---

## **Task 5: Payment Gateway System Using ISP & Adapter Pattern**

### **Solution**

1. **Define a simplified PaymentProcessor interface (ISP):**

```java
public interface PaymentProcessor {
    void processPayment(double amount);
}
```

2. **Simulate external services and create adapter classes:**

- **PayPal Adapter:**

```java
// Simulated external PayPal API
class PayPalService {
    public void makePayment(String email, double amount) {
        System.out.println("Processing PayPal payment for " + email + " amount: " + amount);
    }
}

public class PayPalAdapter implements PaymentProcessor {
    private PayPalService payPalService;
    private String email;
    
    public PayPalAdapter(String email) {
        this.payPalService = new PayPalService();
        this.email = email;
    }
    
    @Override
    public void processPayment(double amount) {
        payPalService.makePayment(email, amount);
    }
}
```

- **Credit Card Adapter:**

```java
// Simulated external Credit Card API
class CreditCardService {
    public void chargeCard(String cardNumber, double amount) {
        System.out.println("Charging credit card " + cardNumber + " for amount: " + amount);
    }
}

public class CreditCardAdapter implements PaymentProcessor {
    private CreditCardService creditCardService;
    private String cardNumber;
    
    public CreditCardAdapter(String cardNumber) {
        this.creditCardService = new CreditCardService();
        this.cardNumber = cardNumber;
    }
    
    @Override
    public void processPayment(double amount) {
        creditCardService.chargeCard(cardNumber, amount);
    }
}
```

- **Cryptocurrency Adapter:**

```java
// Simulated external Crypto API
class CryptoService {
    public void sendCrypto(String walletAddress, double amount) {
        System.out.println("Processing cryptocurrency payment to " + walletAddress + " for amount: " + amount);
    }
}

public class CryptoAdapter implements PaymentProcessor {
    private CryptoService cryptoService;
    private String walletAddress;
    
    public CryptoAdapter(String walletAddress) {
        this.cryptoService = new CryptoService();
        this.walletAddress = walletAddress;
    }
    
    @Override
    public void processPayment(double amount) {
        cryptoService.sendCrypto(walletAddress, amount);
    }
}
```

3. **Create a PaymentService to use the adapters:**

```java
public class PaymentService {
    private PaymentProcessor paymentProcessor;
    
    public PaymentService(PaymentProcessor paymentProcessor) {
        this.paymentProcessor = paymentProcessor;
    }
    
    public void pay(double amount) {
        paymentProcessor.processPayment(amount);
    }
}
```

4. **Test the payment gateway system:**

```java
public class PaymentTest {
    public static void main(String[] args) {
        // Using PayPal
        PaymentProcessor payPalProcessor = new PayPalAdapter("user@example.com");
        PaymentService paymentService = new PaymentService(payPalProcessor);
        paymentService.pay(200.0);
        
        // Using Credit Card
        PaymentProcessor creditCardProcessor = new CreditCardAdapter("4111-1111-1111-1111");
        paymentService = new PaymentService(creditCardProcessor);
        paymentService.pay(150.0);
        
        // Using Cryptocurrency
        PaymentProcessor cryptoProcessor = new CryptoAdapter("wallet1234");
        paymentService = new PaymentService(cryptoProcessor);
        paymentService.pay(300.0);
    }
}
```

---

## **Task 6: Plugin-Based Application Using DIP & Decorator Pattern**

### **Solution**

1. **Define the plugin interface (DIP):**

```java
public interface IPlugin {
    void execute();
}
```

2. **Implement a concrete plugin:**

```java
public class AnalyticsPlugin implements IPlugin {
    @Override
    public void execute() {
        System.out.println("Executing analytics plugin...");
    }
}
```

3. **Create a PluginManager for loading and managing plugins:**

```java
import java.util.ArrayList;
import java.util.List;

public class PluginManager {
    private List<IPlugin> plugins = new ArrayList<>();
    
    public void registerPlugin(IPlugin plugin) {
        plugins.add(plugin);
    }
    
    public void executeAll() {
        for (IPlugin plugin : plugins) {
            plugin.execute();
        }
    }
}
```

4. **Implement the Decorator pattern to add responsibilities:**

- **Decorator Base Class:**

```java
public abstract class PluginDecorator implements IPlugin {
    protected IPlugin decoratedPlugin;
    
    public PluginDecorator(IPlugin decoratedPlugin) {
        this.decoratedPlugin = decoratedPlugin;
    }
    
    @Override
    public void execute() {
        decoratedPlugin.execute();
    }
}
```

- **Logging Decorator:**

```java
public class LoggingPluginDecorator extends PluginDecorator {
    public LoggingPluginDecorator(IPlugin decoratedPlugin) {
        super(decoratedPlugin);
    }
    
    @Override
    public void execute() {
        System.out.println("Logging: Plugin execution started.");
        super.execute();
        System.out.println("Logging: Plugin execution ended.");
    }
}
```

- **Caching Decorator:**

```java
public class CachingPluginDecorator extends PluginDecorator {
    public CachingPluginDecorator(IPlugin decoratedPlugin) {
        super(decoratedPlugin);
    }
    
    @Override
    public void execute() {
        System.out.println("Caching: Checking cache before executing plugin.");
        super.execute();
        System.out.println("Caching: Storing result in cache.");
    }
}
```

5. **Test the plugin system with decorators:**

```java
public class PluginTest {
    public static void main(String[] args) {
        IPlugin analyticsPlugin = new AnalyticsPlugin();
        // Wrap the plugin with caching and logging decorators
        IPlugin decoratedPlugin = new LoggingPluginDecorator(new CachingPluginDecorator(analyticsPlugin));
        
        PluginManager pluginManager = new PluginManager();
        pluginManager.registerPlugin(decoratedPlugin);
        pluginManager.executeAll();
    }
}
```

---

## **Task 7: Legacy System Refactoring with SOLID Principles & Facade Pattern**

### **Solution**

1. **Simulate the legacy system (pre-refactoring):**

```java
// Legacy code with mixed responsibilities
class LegacyCustomerService {
    public void addCustomer(String name, String email) {
        // Database logic
        System.out.println("Customer " + name + " added with email " + email);
    }
    public void updateCustomer(String id, String name, String email) {
        // Update logic
        System.out.println("Customer " + id + " updated.");
    }
    public void sendWelcomeEmail(String email) {
        // Email logic
        System.out.println("Welcome email sent to " + email);
    }
}
```

2. **Refactor by separating concerns:**

- **Customer Repository (Data Access):**

```java
public class CustomerRepository {
    public void addCustomer(String name, String email) {
        System.out.println("Customer " + name + " added with email " + email);
    }
    public void updateCustomer(String id, String name, String email) {
        System.out.println("Customer " + id + " updated.");
    }
}
```

- **Email Service (Notification):**

```java
public class EmailService {
    public void sendEmail(String email, String subject, String body) {
        System.out.println("Email sent to " + email + " with subject: " + subject);
    }
}
```

3. **Introduce a Facade to streamline interactions:**

```java
public class CustomerServiceFacade {
    private CustomerRepository repository;
    private EmailService emailService;
    
    public CustomerServiceFacade(CustomerRepository repository, EmailService emailService) {
        this.repository = repository;
        this.emailService = emailService;
    }
    
    public void registerCustomer(String name, String email) {
        repository.addCustomer(name, email);
        emailService.sendEmail(email, "Welcome", "Thank you for registering, " + name);
    }
    
    public void updateCustomer(String id, String name, String email) {
        repository.updateCustomer(id, name, email);
        emailService.sendEmail(email, "Profile Updated", "Your profile has been updated, " + name);
    }
}
```

4. **Test the refactored legacy system:**

```java
public class LegacyRefactoringTest {
    public static void main(String[] args) {
        CustomerRepository repository = new CustomerRepository();
        EmailService emailService = new EmailService();
        CustomerServiceFacade customerService = new CustomerServiceFacade(repository, emailService);
        
        customerService.registerCustomer("John Doe", "john@example.com");
        customerService.updateCustomer("C001", "John Doe", "john.new@example.com");
    }
}
```
