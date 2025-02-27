### **Level 1: Foundation - Encapsulation**  

---

## **1. What is Encapsulation?**
**Encapsulation** is one of the four fundamental principles of Object-Oriented Programming (OOP). It is the practice of wrapping data (attributes) and methods (functions) that operate on the data into a single unit, called a class.

### **Key Points:**
- **Data Hiding:** Encapsulation restricts direct access to an object's data, protecting the integrity of that data.
- **Controlled Access:** Access to the data is provided through public methods (getters and setters), ensuring controlled manipulation.
- **Maintainability and Security:** By hiding the internal implementation details, encapsulation enhances maintainability and security.

### **Real-World Example:**
Think of **Encapsulation** like a **capsule** that contains medicine. The medicine (data) is hidden inside the capsule, and you can only access it in a controlled manner.

---

## **2. Why is Encapsulation Important?**
1. **Security and Data Integrity:** 
   - Sensitive data can be protected from unauthorized access or modification.
   - For example, a `BankAccount` class should not allow direct manipulation of the balance.

2. **Flexibility and Maintainability:**
   - Internal implementation can be changed without affecting external code.
   - For example, changing the calculation logic of interest in a bank account won't affect how the balance is accessed.

3. **Modularity:**
   - Encapsulation allows breaking down a complex system into smaller, manageable classes or modules.

---

## **3. How to Achieve Encapsulation in Java?**
In Java, **Encapsulation** is achieved by:
- Making class attributes `private`.
- Providing public **Getters and Setters** to access and modify these attributes.

### **Example: BankAccount Class**
```java
public class BankAccount {
    // Private fields (Data Hiding)
    private String accountNumber;
    private String accountHolderName;
    private double balance;

    // Constructor
    public BankAccount(String accountNumber, String accountHolderName, double balance) {
        this.accountNumber = accountNumber;
        this.accountHolderName = accountHolderName;
        this.balance = Math.max(0, balance); // Ensures balance is non-negative
    }

    // Getters
    public String getAccountNumber() {
        return accountNumber;
    }

    public String getAccountHolderName() {
        return accountHolderName;
    }

    public double getBalance() {
        return balance;
    }

    // Setters with Validation
    public void setAccountHolderName(String accountHolderName) {
        if (accountHolderName != null && !accountHolderName.isEmpty()) {
            this.accountHolderName = accountHolderName;
        } else {
            System.out.println("Account holder name cannot be empty.");
        }
    }

    // Method to deposit money
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println("Deposited: " + amount + ". New Balance: " + balance);
        } else {
            System.out.println("Deposit amount must be positive.");
        }
    }

    // Method to withdraw money with validation
    public boolean withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            System.out.println("Withdrawn: " + amount + ". Remaining Balance: " + balance);
            return true;
        } else {
            System.out.println("Insufficient balance or invalid amount.");
            return false;
        }
    }
}
```

### **Usage:**
```java
public class Main {
    public static void main(String[] args) {
        // Creating an object of BankAccount
        BankAccount account1 = new BankAccount("12345", "John Doe", 1000);

        // Accessing data using Getters
        System.out.println("Account Number: " + account1.getAccountNumber());
        System.out.println("Account Holder: " + account1.getAccountHolderName());
        System.out.println("Balance: " + account1.getBalance());

        // Modifying data using Setters and Methods
        account1.setAccountHolderName("Jane Doe");
        account1.deposit(500);
        account1.withdraw(300);
        account1.withdraw(1500); // Insufficient balance
    }
}
```

### **Output:**
```
Account Number: 12345
Account Holder: John Doe
Balance: 1000.0
Deposited: 500.0. New Balance: 1500.0
Withdrawn: 300.0. Remaining Balance: 1200.0
Insufficient balance or invalid amount.
```

---

## **4. Access Modifiers in Java**

Java provides four access modifiers to control access to class members:

| **Modifier** | **Class** | **Package** | **Subclass** | **World** |
|--------------|-----------|-------------|---------------|-----------|
| **public**   | ✔         | ✔           | ✔             | ✔         |
| **protected**| ✔         | ✔           | ✔             | ✖         |
| **default**  | ✔         | ✔           | ✖             | ✖         |
| **private**  | ✔         | ✖           | ✖             | ✖         |

### **Details:**
1. **public:** Accessible from anywhere.
2. **protected:** Accessible within the package and by subclasses.
3. **default:** (No modifier) Accessible within the same package.
4. **private:** Accessible only within the same class.

---

## **5. Best Practices for Encapsulation:**
1. Always make fields **private**.
2. Provide **public Getters and Setters** for accessing and modifying fields.
3. **Validate data** in Setters to ensure data integrity.
4. If a field should not be modified after initialization, use the `final` keyword.

### **Example: Using Final Keyword**
```java
private final String accountNumber;
```

---

## **6. Common Mistakes to Avoid:**
1. **Exposing Internal Data:**
   - Avoid returning mutable objects directly from getters.
   - Example:
     ```java
     public Date getDate() {
         return new Date(this.date.getTime()); // Return a copy, not the original
     }
     ```

2. **Inconsistent State:**
   - Ensure the object state is consistent by validating inputs in setters.

3. **Public Fields:** 
   - Never make fields `public`, as it breaks encapsulation.

---

## **7. Exercise: Mastering Encapsulation**
### **Task: Create a `Person` Class**
- **Attributes:** `name`, `age`, `email`.
- **Methods:**
  - `displayDetails()` — Display person's details.
  - `setName(String name)` — Update name with validation (non-empty).
  - `setAge(int age)` — Update age with validation (non-negative).
  - `setEmail(String email)` — Update email with validation (contains `@` and a domain).
  - `getName()`, `getAge()`, `getEmail()` — Getters for encapsulated fields.

### **Requirements:**
- Use `private` access for all fields.
- Provide `public` getters and setters with proper validation.
- Use `this` keyword wherever applicable.
- Ensure `name` is always capitalized (e.g., "John Doe" instead of "john doe").

---

## **8. What's Next?**
1. **Complete the Exercise** and share your code if you need feedback.
2. Next, we will explore **Abstraction**:
   - What is Abstraction, and how is it different from Encapsulation?
   - Abstract Classes vs. Interfaces.
   - Real-world examples to understand the need for Abstraction.
   - How to use `abstract` keyword in Java.
   - Best practices and common mistakes to avoid.

---

Here's a Java implementation of the `Person` class that meets all the specified requirements:

```java
public class Person {
    private String name;
    private int age;
    private String email;

    // Constructor
    public Person(String name, int age, String email) {
        setName(name); // Use setters for validation and capitalization
        setAge(age);
        setEmail(email);
    }

    // Getter for name
    public String getName() {
        return this.name;
    }

    // Getter for age
    public int getAge() {
        return this.age;
    }

    // Getter for email
    public String getEmail() {
        return this.email;
    }

    // Setter for name with validation and capitalization
    public void setName(String name) {
        if (name != null && !name.trim().isEmpty()) {
            // Capitalize each word in the name
            String[] words = name.trim().split("\\s+");
            StringBuilder capitalizedName = new StringBuilder();
            for (String word : words) {
                if (!word.isEmpty()) {
                    capitalizedName.append(Character.toUpperCase(word.charAt(0)))
                                 .append(word.substring(1).toLowerCase())
                                 .append(" ");
                }
            }
            this.name = capitalizedName.toString().trim();
        } else {
            System.out.println("Name cannot be empty or null.");
            if (this.name == null) { // Set default if not yet initialized
                this.name = "Unknown";
            }
        }
    }

    // Setter for age with validation
    public void setAge(int age) {
        if (age >= 0) {
            this.age = age;
        } else {
            System.out.println("Age cannot be negative.");
            if (this.age == 0) { // Only warn, don't overwrite if already set
                this.age = 0; // Default if not yet initialized
            }
        }
    }

    // Setter for email with validation
    public void setEmail(String email) {
        if (email != null && email.contains("@") && email.contains(".") && email.indexOf("@") < email.lastIndexOf(".")) {
            this.email = email;
        } else {
            System.out.println("Invalid email format. Must contain '@' and a domain.");
            if (this.email == null) { // Set default if not yet initialized
                this.email = "unknown@example.com";
            }
        }
    }

    // Method to display person's details
    public void displayDetails() {
        System.out.println("Name: " + this.name);
        System.out.println("Age: " + this.age);
        System.out.println("Email: " + this.email);
    }
}

// Example usage
class Main {
    public static void main(String[] args) {
        // Create a person with valid initial values
        Person person1 = new Person("tushar nagdive", 25, "tushar@example.com");
        System.out.println("Person 1 Details:");
        person1.displayDetails();
        System.out.println();

        // Test invalid updates
        person1.setName(""); // Empty name
        person1.setAge(-5);  // Negative age
        person1.setEmail("invalid-email"); // Invalid email
        System.out.println("Person 1 Details after invalid updates:");
        person1.displayDetails();
        System.out.println();

        // Test valid updates
        person1.setName("john doe");
        person1.setAge(30);
        person1.setEmail("john.doe@domain.com");
        System.out.println("Person 1 Details after valid updates:");
        person1.displayDetails();
    }
}
```

### Explanation:

1. **Attributes**:
   - `name`, `age`, and `email` are private for encapsulation.

2. **Getters**:
   - `getName()`, `getAge()`, and `getEmail()` provide public access to the private fields, using `this` for clarity.

3. **Setters with Validation**:
   - `setName(String name)`:
     - Checks for null or empty input.
     - Capitalizes each word (e.g., "tushar nagdive" → "Tushar Nagdive").
     - Uses a default ("Unknown") if invalid and not yet set.
   - `setAge(int age)`:
     - Ensures age is non-negative.
     - Uses 0 as default if invalid and not yet set.
   - `setEmail(String email)`:
     - Validates that email contains "@" and a domain (".") with "@" before the last ".".
     - Uses a default ("unknown@example.com") if invalid and not yet set.

4. **Constructor**:
   - Uses setters for initial values to enforce validation and capitalization from the start.

5. **Display Method**:
   - `displayDetails()` prints all attributes using `this`.

### Sample Output:
```
Person 1 Details:
Name: Tushar Nagdive
Age: 25
Email: tushar@example.com

Name cannot be empty or null.
Age cannot be negative.
Invalid email format. Must contain '@' and a domain.
Person 1 Details after invalid updates:
Name: Tushar Nagdive
Age: 25
Email: tushar@example.com

Person 1 Details after valid updates:
Name: John Doe
Age: 30
Email: john.doe@domain.com
```
