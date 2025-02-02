## **Interview Questions**

### **Task 1: Notification System Using Observer & Dependency Inversion**

1. **SOLID & Observer Fundamentals:**  
   - **Q1:** Which SOLID principles are applied in this design? Explain how the Dependency Inversion Principle (DIP) and Interface Segregation Principle (ISP) are implemented in your notification system.  
   - **Q2:** How does the Observer Pattern help in decoupling the notification mechanism? Provide examples from your implementation.  
   - **Q3:** What are the benefits of using a centralized `NotificationManager` to register and broadcast messages?

### **Task 2: Order Processing System Using SRP & Factory Pattern**

2. **Separation of Concerns & Object Creation:**  
   - **Q1:** Explain the Single Responsibility Principle (SRP) and how it is maintained in your order processing system.  
   - **Q2:** What is the Factory Pattern, and how does it help in creating different types of orders (e.g., digital vs. physical)?  
   - **Q3:** How would you extend the system to support new order types without altering the existing code?

### **Task 3: Report Generation System Using OCP & Strategy Pattern**

3. **Extensibility & Strategy Use:**  
   - **Q1:** What is the Open/Closed Principle (OCP), and how does your report generation design adhere to it?  
   - **Q2:** Describe the Strategy Pattern and how it provides flexibility for supporting various report formats.  
   - **Q3:** How would you integrate an additional report format (e.g., XML) into your system without modifying existing code?

### **Task 4: Parking Lot Management System Using LSP & Template Method**

4. **Substitutability & Process Standardization:**  
   - **Q1:** Explain the Liskov Substitution Principle (LSP) and its importance in your parking lot management system design.  
   - **Q2:** How does the Template Method Pattern streamline the operations (like parking/unparking) for different vehicle types?  
   - **Q3:** Under what circumstances might the Template Method Pattern not be the best choice?

### **Task 5: Payment Gateway System Using ISP & Adapter Pattern**

5. **Interface Design & Integration:**  
   - **Q1:** Describe the Interface Segregation Principle (ISP) and how it influenced the design of your payment processing interfaces.  
   - **Q2:** What is the Adapter Pattern, and how did it facilitate integrating multiple external payment services?  
   - **Q3:** What are some potential challenges when using adapters to integrate with external APIs, and how can these be mitigated?

### **Task 6: Plugin-Based Application Using DIP & Decorator Pattern**

6. **Modular Design & Dynamic Behavior:**  
   - **Q1:** How does the Dependency Inversion Principle (DIP) enable a modular, plugin-based architecture?  
   - **Q2:** Explain the Decorator Pattern and describe its role in adding responsibilities (like logging or caching) dynamically to plugins.  
   - **Q3:** In a real-world scenario, how would you manage multiple decorators applied to the same plugin to avoid complexity?

### **Task 7: Legacy System Refactoring with SOLID Principles & Facade Pattern**

7. **Refactoring & Simplification:**  
   - **Q1:** Identify common code smells that violate SOLID principles. How did you address these in your legacy system refactoring?  
   - **Q2:** What is the Facade Pattern, and how does it simplify the interactions between refactored subsystems?  
   - **Q3:** How does the separation of concerns in the refactored system improve maintainability and testability?

---

## **Knowledge Check and Scoring System**

### **Scoring Rubric**

Each question will be evaluated based on the following criteria:
- **Correctness:** Accuracy of the technical explanation.
- **Completeness:** Coverage of all aspects of the question (e.g., definition, application, examples).
- **Clarity:** Clear and concise explanation, including the use of examples or code references when appropriate.
- **Depth:** Demonstration of an understanding of trade-offs, potential challenges, and alternative approaches.

A sample scoring guideline per question is as follows:

| Score | Description                                                  |
|:-----:|--------------------------------------------------------------|
| **0** | No answer or completely incorrect answer.                  |
| **1-2** | Partial answer; shows basic or incomplete understanding.   |
| **3-4** | Good answer; demonstrates solid understanding with minor gaps. |
| **5** | Excellent answer; clear, detailed, and fully addresses the question with examples. |

### **Example Scoring Breakdown**

- **Total Questions:** 21 (3 questions per task × 7 tasks)
- **Maximum Score:** 21 questions × 5 points each = **105 points**
- **Minimum Passing Score:** 70% of 105 points ≈ **74 points**

### **Interview Evaluation Process**

1. **Preparation:**  
   Each intern should provide a brief explanation (or live coding) for each question, supported by examples from their tasks.

2. **Scoring:**  
   Evaluate each answer using the above rubric.  
   - For example, if an intern scores 4, 5, and 3 on the three questions for Task 1, the total for Task 1 is 12/15.
   - Repeat for all tasks and sum the total points.

3. **Feedback:**  
   Provide specific feedback for answers scoring below 3 to help the intern improve their understanding. Encourage follow-up questions or code walk-throughs if clarification is needed.

4. **Final Decision:**  
   Interns scoring **74 points or higher** are considered to have demonstrated a sufficient understanding of SOLID principles and design patterns in Java. Those below this threshold may require additional mentoring and targeted training on the areas where they struggled.
