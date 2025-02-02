
### **Task 1: Notification System Using Observer & Dependency Inversion**

**Scenario:**  
Develop a notification system for an IT company that can send alerts through multiple channels—Email, SMS, and Push notifications.  
   
**Requirements:**  
- **SOLID Focus:**  
  - **Dependency Inversion Principle (DIP):** High-level modules (e.g., NotificationManager) should not depend on low-level modules (e.g., EmailNotifier); both should depend on abstractions.
  - **Interface Segregation Principle (ISP):** Create fine-grained notification interfaces so that classes only implement what they need.
- **Design Pattern:**  
  - **Observer Pattern:** The NotificationManager should maintain a list of observers (notifiers) that subscribe to notification events.  
- **Deliverables:**  
  - Define an abstraction (e.g., `INotificationChannel`) for notification channels.  
  - Implement concrete channels like `EmailNotifier`, `SMSNotifier`, and `PushNotifier`.  
  - Create a `NotificationManager` that can register/deregister channels and broadcast messages.
  - Write unit tests to simulate different notification scenarios.

---

### **Task 2: Order Processing System Using Single Responsibility & Factory Pattern**

**Scenario:**  
Design an order processing system for an e-commerce module where orders for various product types need to be created, validated, and processed.  

**Requirements:**  
- **SOLID Focus:**  
  - **Single Responsibility Principle (SRP):** Separate concerns such as order creation, validation, and processing into distinct classes.
- **Design Pattern:**  
  - **Factory Pattern:** Create a factory that instantiates different types of orders (e.g., digital, physical) based on given criteria.
- **Deliverables:**  
  - Define an `Order` interface or abstract class.
  - Implement concrete order classes like `DigitalOrder` and `PhysicalOrder`.
  - Develop an `OrderFactory` that returns the appropriate order instance based on input parameters.
  - Include validation and processing classes that interact with the orders while keeping responsibilities separated.

---

### **Task 3: Report Generation System Using Open/Closed & Strategy Pattern**

**Scenario:**  
Create a report generation application for the IT department that can output reports in multiple formats (e.g., PDF, HTML, CSV) without modifying existing code when new formats are added.

**Requirements:**  
- **SOLID Focus:**  
  - **Open/Closed Principle (OCP):** The system should be open for extension (new report formats) but closed for modification.
- **Design Pattern:**  
  - **Strategy Pattern:** Define a strategy interface for report formatting and implement various strategies for each format.
- **Deliverables:**  
  - Create a `ReportFormatter` interface.
  - Implement format-specific classes such as `PdfFormatter`, `HtmlFormatter`, and `CsvFormatter`.
  - Develop a `ReportGenerator` class that uses a `ReportFormatter` strategy to generate reports.
  - Demonstrate adding a new report format without altering existing classes.

---

### **Task 4: Parking Lot Management System Using Liskov Substitution & Template Method**

**Scenario:**  
Design a parking lot management system that handles various vehicle types (e.g., Car, Motorcycle, Truck) ensuring that each can be managed interchangeably without unexpected behavior.

**Requirements:**  
- **SOLID Focus:**  
  - **Liskov Substitution Principle (LSP):** Subtypes (e.g., specific vehicle classes) should be substitutable for their base type without breaking the system.
- **Design Pattern:**  
  - **Template Method Pattern:** Define a skeleton for parking operations (e.g., park, unpark) that subclasses can customize.
- **Deliverables:**  
  - Create an abstract `Vehicle` class defining basic properties and operations.
  - Implement concrete classes (`Car`, `Motorcycle`, `Truck`) that override necessary methods.
  - Develop a `ParkingOperationTemplate` abstract class that outlines the steps for parking/unparking, with hooks for vehicle-specific behavior.
  - Validate that all vehicle types work seamlessly within the parking system.

---

### **Task 5: Payment Gateway System Using Interface Segregation & Adapter Pattern**

**Scenario:**  
Build a payment gateway that supports multiple external payment services (e.g., PayPal, Credit Card, Cryptocurrency) by offering a unified interface for processing payments.

**Requirements:**  
- **SOLID Focus:**  
  - **Interface Segregation Principle (ISP):** Split large payment interfaces into smaller, client-specific interfaces.
- **Design Pattern:**  
  - **Adapter Pattern:** Create adapters to integrate external payment APIs into the common payment processing interface.
- **Deliverables:**  
  - Define a lean `PaymentProcessor` interface.
  - Implement adapters such as `PayPalAdapter`, `CreditCardAdapter`, and `CryptoAdapter` that map external APIs to your interface.
  - Create a `PaymentService` that uses these adapters to process transactions.
  - Include error handling and logging to manage integration issues.

---

### **Task 6: Plugin-Based Application Using Dependency Inversion & Decorator Pattern**

**Scenario:**  
Develop a modular application that supports runtime loading of plugins (e.g., for extending functionalities like analytics or reporting). The system should be easily extensible without changing the core code.

**Requirements:**  
- **SOLID Focus:**  
  - **Dependency Inversion Principle (DIP):** High-level modules should depend on abstractions (interfaces) instead of concrete plugin implementations.
- **Design Pattern:**  
  - **Decorator Pattern:** Allow dynamic addition of responsibilities (e.g., logging, caching) to plugins without altering their structure.
- **Deliverables:**  
  - Define a plugin interface (e.g., `IPlugin`) for all modules.
  - Implement a plugin loader that uses dependency injection to manage plugins.
  - Develop decorators (e.g., `LoggingPluginDecorator`, `CachingPluginDecorator`) that wrap plugin functionality.
  - Demonstrate how new plugins and behaviors can be added at runtime with minimal changes to the core system.

---

### **Task 7: Legacy System Refactoring with SOLID Principles & Facade/Mediator Pattern**

**Scenario:**  
Take an existing legacy module (for example, a monolithic customer service system) that mixes multiple responsibilities and complex interactions. Refactor it to adhere to SOLID principles and simplify interactions between components.

**Requirements:**  
- **SOLID Focus:**  
  - Identify and refactor code that violates multiple SOLID principles (e.g., SRP, OCP, DIP).
- **Design Pattern:**  
  - **Facade or Mediator Pattern:** Introduce a Facade to provide a simple interface to the refactored subsystems, or a Mediator to handle complex inter-component communications.
- **Deliverables:**  
  - Analyze the legacy code to pinpoint violations of SOLID principles.
  - Refactor the code by separating concerns into dedicated classes/modules.
  - Implement a `CustomerServiceFacade` (or a `Mediator`) that streamlines interactions between the refactored modules.
  - Provide before-and-after comparisons and unit tests to demonstrate improved maintainability and reduced coupling.

---

### **Guidelines for All Tasks**

- **Documentation & Code Comments:**  
  Encourage detailed documentation that explains the SOLID principles applied, the choice of design patterns, and how they solve specific problems.
  
- **Unit Testing:**  
  Each intern should include comprehensive unit tests (using frameworks like JUnit) to verify the correctness of their implementation and the adherence to SOLID principles.

- **Code Reviews & Knowledge Sharing:**  
  Organize regular sessions where interns present their tasks, discuss design decisions, and receive constructive feedback.

- **Version Control:**  
  Use Git (or another version control system) for code management, ensuring that each task is well-documented in commit messages.
