---

## **What You Did Well:**
1. **Encapsulation:**
   - Made all fields (`name`, `age`, `email`) private, ensuring data hiding.
   - Provided public getters and setters for controlled access.

2. **Validation and Error Handling:**
   - **Name:**
     - Checks for non-null and non-empty values.
     - Capitalizes each word, ensuring consistent and professional naming format.
   - **Age:**
     - Ensures age is non-negative.
   - **Email:**
     - Checks for the presence of `@` and a domain (e.g., `.com`).
     - Validates the order of `@` and `.` to ensure a correct email format.

3. **Robustness:**
   - Used default values if invalid data is provided.
   - Ensured the object remains in a consistent state even after invalid inputs.

4. **Object-Oriented Principles:**
   - Proper use of `this` keyword to refer to instance variables.
   - Maintained immutability for certain fields by not providing setters where not needed.

5. **Capitalization Logic:**
   - Split the name by spaces and capitalized each word.
   - Ensured proper name format regardless of input case.

---

## **Expert Insights and Suggestions:**
1. **Enhanced Validation:**
   - Improve email validation using **Regular Expressions** for more robust checks.
   - Example: Ensuring no special characters at the start, no consecutive dots, etc.

2. **Immutable Fields:**
   - If `name` or `email` should not change once set, make them `final`.
   - Use `setName()` and `setEmail()` methods only if modification is needed.

3. **Consistent Error Handling:**
   - Centralize error messages for better maintainability.
   - Example: Using a `private` helper method for validation messages.

4. **Clean Code Practices:**
   - Improve readability by breaking down long methods into smaller private helper methods.
   - Example: Extract capitalization logic into a separate method.

---

## **Refactored Code with Improvements:**
Here's an improved version that includes:
- **Enhanced Email Validation** using Regular Expressions.
- **Final Keyword** for immutable fields.
- **Helper Methods** for better readability and maintainability.

```java
public class Person {
    // Immutable fields with 'final'
    private final String name;
    private int age;
    private String email;

    // Constructor
    public Person(String name, int age, String email) {
        this.name = capitalizeName(name);
        setAge(age);
        setEmail(email);
    }

    // Getter for name (No Setter for immutability)
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

    // Setter for age with validation
    public void setAge(int age) {
        if (age >= 0) {
            this.age = age;
        } else {
            System.out.println("Age cannot be negative.");
        }
    }

    // Setter for email with enhanced validation using Regular Expressions
    public void setEmail(String email) {
        String emailPattern = "^[A-Za-z0-9+_.-]+@[A-Za-z0-9.-]+$";
        if (email != null && email.matches(emailPattern)) {
            this.email = email;
        } else {
            System.out.println("Invalid email format. Must contain '@' and a domain.");
            if (this.email == null) {
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

    // Helper method to capitalize each word in the name
    private String capitalizeName(String name) {
        if (name != null && !name.trim().isEmpty()) {
            String[] words = name.trim().split("\\s+");
            StringBuilder capitalizedName = new StringBuilder();
            for (String word : words) {
                if (!word.isEmpty()) {
                    capitalizedName.append(Character.toUpperCase(word.charAt(0)))
                            .append(word.substring(1).toLowerCase())
                            .append(" ");
                }
            }
            return capitalizedName.toString().trim();
        } else {
            return "Unknown";
        }
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
        person1.setAge(-5);  // Negative age
        person1.setEmail("invalid-email"); // Invalid email
        System.out.println("Person 1 Details after invalid updates:");
        person1.displayDetails();
        System.out.println();

        // Test valid updates
        person1.setAge(30);
        person1.setEmail("john.doe@domain.com");
        System.out.println("Person 1 Details after valid updates:");
        person1.displayDetails();
    }
}
```

---

## **What's New and Improved?**
1. **Enhanced Email Validation:**  
   - Using **Regular Expressions** for more robust email format checks.
2. **Immutability:**  
   - Made `name` final to ensure it cannot be changed once set.
3. **Code Cleanliness and Maintainability:**  
   - Refactored name capitalization into a helper method (`capitalizeName()`).
   - Centralized validation logic for improved readability and maintainability.
4. **Enhanced Readability and Structure:**  
   - Clearer separation of concerns by breaking down complex methods into smaller private methods.

---

## **Next Steps:**
We’ve now mastered **Encapsulation** with:
- Private fields and controlled access using getters and setters.
- Validation logic to maintain consistent state.
- Immutability using `final` where applicable.
- Enhanced maintainability with helper methods and regex validation.

---

## **Ready for the Next Concept?**
### We’re moving on to **Abstraction**:
1. **What is Abstraction?**
2. **Difference between Abstraction and Encapsulation.**
3. **How to achieve Abstraction in Java:**
   - Using **Abstract Classes**.
   - Using **Interfaces**.
4. **Abstract Classes vs. Interfaces:**
   - When to use which one.
5. **Real-world Examples:**
   - Implementing a `Payment` system with different payment methods.
   - Using Interfaces for multiple inheritance and loose coupling.
6. **Common Mistakes to Avoid in Abstraction.**

---

## **Your Exercise:**
### **Task: Implement an `Employee` Class**
- **Attributes:** `name`, `id`, `designation`, `salary`.
- **Methods:**
  - `displayDetails()` — Display employee details.
  - `setDesignation(String designation)` — Update designation with validation (non-empty).
  - `setSalary(double salary)` — Update salary with validation (non-negative).
  - `getName()`, `getId()`, `getDesignation()`, `getSalary()` — Getters for encapsulated fields.
- Use **Encapsulation**:
  - Make all fields private.
  - Provide public getters and setters with proper validation.
  - Use `final` for `id` to make it immutable.
  - Ensure `name` is always capitalized (e.g., "John Doe" instead of "john doe").

---
