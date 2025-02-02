### **Interview Questions**

#### **General Questions**
1. What are the SOLID principles, and why are they important in software design?
2. Explain the difference between inheritance and composition in Java.
3. What is the purpose of design patterns, and can you name a few commonly used ones?

---

#### **Task-Specific Questions**

##### **Goal 1: Single Responsibility Principle (SRP)**
1. What is the Single Responsibility Principle, and how does it improve code maintainability?
2. Can you explain why the `ProductPrinter` class was separated from the `Product` class?
3. What would happen if the `Product` class also handled printing logic? How would it violate SRP?

##### **Goal 2: Open/Closed Principle (OCP)**
1. What is the Open/Closed Principle, and how does it promote extensibility?
2. How did you ensure that new discount types can be added without modifying the `Product` class?
3. Can you explain the role of the `Discount` interface in your implementation?

##### **Goal 3: Liskov Substitution Principle (LSP)**
1. What is the Liskov Substitution Principle, and why is it important in inheritance?
2. How did you ensure that `CreditCard` and `PayPal` can be used interchangeably in the `PaymentProcessor` class?
3. Can you give an example of a violation of LSP and how you would fix it?

##### **Goal 4: Interface Segregation Principle (ISP)**
1. What is the Interface Segregation Principle, and how does it prevent fat interfaces?
2. Why did you create separate `Customer` and `Admin` interfaces instead of a single interface?
3. Can you explain how ISP improves code readability and maintainability?

##### **Goal 5: Dependency Inversion Principle (DIP)**
1. What is the Dependency Inversion Principle, and how does it reduce coupling?
2. How did you ensure that the `OrderService` class is not tightly coupled to a specific notification implementation?
3. Can you explain the benefits of using interfaces for dependency injection?

##### **Goal 6: Design Pattern - Singleton**
1. What is the Singleton design pattern, and when should it be used?
2. How did you ensure that only one instance of the `Cart` class is created?
3. Can you explain the potential drawbacks of using the Singleton pattern?

##### **Goal 7: Design Pattern - Observer**
1. What is the Observer design pattern, and how does it facilitate event-driven communication?
2. How did you implement the Observer pattern in the e-commerce platform scenario?
3. Can you explain the difference between the Observer pattern and the Publisher-Subscriber pattern?

---

### **Knowledge Check**

#### **Multiple-Choice Questions**
1. Which SOLID principle states that a class should have only one reason to change?
   - a) Open/Closed Principle
   - b) Single Responsibility Principle
   - c) Liskov Substitution Principle
   - d) Interface Segregation Principle

2. Which design pattern ensures that only one instance of a class is created?
   - a) Factory
   - b) Singleton
   - c) Observer
   - d) Decorator

3. Which principle suggests that high-level modules should not depend on low-level modules but on abstractions?
   - a) Single Responsibility Principle
   - b) Dependency Inversion Principle
   - c) Liskov Substitution Principle
   - d) Interface Segregation Principle

4. Which design pattern is used to notify multiple objects when the state of another object changes?
   - a) Singleton
   - b) Observer
   - c) Strategy
   - d) Adapter

5. Which principle ensures that a subclass can replace its superclass without breaking functionality?
   - a) Open/Closed Principle
   - b) Liskov Substitution Principle
   - c) Interface Segregation Principle
   - d) Dependency Inversion Principle

---

#### **Coding Problems**
1. **SRP:** Write a `User` class that only handles user-related attributes and a separate `UserLogger` class to log user actions.
2. **OCP:** Implement a `Shipping` interface and two classes `StandardShipping` and `ExpressShipping` that calculate shipping costs.
3. **LSP:** Create a `Vehicle` class and two subclasses `Car` and `Bike` that can be used interchangeably in a `VehicleProcessor` class.
4. **ISP:** Design interfaces for `Reader` and `Writer` roles to ensure they only have relevant methods.
5. **DIP:** Implement a `ReportGenerator` class that depends on a `DataFetcher` interface to fetch data from different sources.
6. **Singleton:** Write a `Logger` class that ensures only one instance is created.
7. **Observer:** Implement a `WeatherStation` class that notifies multiple `Display` objects when the temperature changes.

---

### **Scoring System**

#### **Evaluation Criteria**
1. **Understanding of Concepts (0-10 points):**
   - How well does the intern understand the SOLID principles and design patterns?
   - Can they explain the concepts clearly and provide real-world examples?

2. **Implementation (0-10 points):**
   - Did the intern correctly implement the assigned task?
   - Is the code clean, modular, and follows best practices?

3. **Problem-Solving (0-10 points):**
   - How well did the intern handle edge cases and potential issues in their implementation?
   - Did they demonstrate creativity and logical thinking?

4. **Code Quality (0-10 points):**
   - Is the code readable, well-documented, and properly formatted?
   - Are variable names meaningful and consistent?

5. **Interview Performance (0-10 points):**
   - How well did the intern answer the interview questions?
   - Did they communicate their thought process clearly and confidently?

---

#### **Minimum Passing Score**
- **Total Score:** 50/50
- **Minimum Passing Score:** 35/50
  - **Understanding of Concepts:** 7/10
  - **Implementation:** 7/10
  - **Problem-Solving:** 7/10
  - **Code Quality:** 7/10
  - **Interview Performance:** 7/10

---

### **Grading Rubric**
| **Criteria**            | **Excellent (9-10)** | **Good (7-8)** | **Average (5-6)** | **Poor (0-4)** |
|--------------------------|----------------------|----------------|-------------------|----------------|
| **Understanding**        | Clear, detailed, and accurate explanation of concepts. | Good understanding with minor gaps. | Basic understanding with some confusion. | Lacks understanding of key concepts. |
| **Implementation**       | Code is correct, efficient, and follows best practices. | Code is mostly correct with minor issues. | Code has significant issues but works partially. | Code is incorrect or incomplete. |
| **Problem-Solving**      | Handles edge cases and demonstrates creative solutions. | Solves the problem effectively. | Solves the problem but misses edge cases. | Struggles to solve the problem. |
| **Code Quality**         | Clean, readable, and well-documented code. | Code is readable with minor issues. | Code is readable but lacks documentation. | Code is messy and hard to understand. |
| **Interview Performance**| Confident, clear, and articulate in responses. | Communicates well with minor hesitations. | Struggles to explain but provides some answers. | Poor communication and unclear responses. |

---
