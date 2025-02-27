### **Level 1: Foundation - Classes and Objects**  

---

## **1. What are Classes and Objects in OOP?**

In Java (and OOP in general), **Classes** and **Objects** are the fundamental building blocks of any program.

- **Class:** 
  - A blueprint or template for creating objects.
  - It defines the properties (attributes) and behaviors (methods) that the objects created from the class will have.

- **Object:** 
  - An instance of a class.
  - It has a state (defined by its attributes) and behavior (defined by its methods).

---

## **2. Real-World Analogy**

### **Example: Blueprint vs. House**
- A **Class** is like a blueprint of a house. It defines the structure and design but is not the house itself.
- An **Object** is an actual house built from the blueprint. You can create multiple houses (objects) from the same blueprint (class).

---

## **3. Defining a Class in Java**
In Java, a class is defined using the `class` keyword followed by the class name. 

```java
public class Car {
    // Attributes (State)
    String brand;
    String color;
    int speed;

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

---

## **4. Creating Objects in Java**

You create an object using the `new` keyword. This allocates memory for the object and returns a reference to it.

```java
public class Main {
    public static void main(String[] args) {
        // Creating an object of Car
        Car car1 = new Car();
        car1.brand = "Toyota";
        car1.color = "Red";
        car1.speed = 0;

        // Using object's methods
        car1.start();
        car1.accelerate(50);
        car1.brake();

        // Creating another object of Car
        Car car2 = new Car();
        car2.brand = "Honda";
        car2.color = "Blue";
        car2.speed = 0;

        car2.start();
        car2.accelerate(60);
    }
}
```

### **Output:**
```
Toyota is starting.
Toyota is accelerating to 50 km/h.
Toyota is stopping.
Honda is starting.
Honda is accelerating to 60 km/h.
```

---

## **5. Memory Allocation in Java**
- **Stack Memory:**
  - Stores references to objects and method call details.
  - Local variables are stored here.

- **Heap Memory:**
  - Stores the actual objects created using `new`.
  - Garbage Collection automatically frees up memory when objects are no longer in use.

---

## **6. Constructors**
A **Constructor** is a special method used to initialize objects. It is called when an object is created using the `new` keyword.

### **Types of Constructors:**
1. **Default Constructor:**
   - Created by the compiler if no other constructor is defined.
   - Example:
     ```java
     public Car() {
         // Default constructor
     }
     ```

2. **Parameterized Constructor:**
   - Takes arguments to initialize the object with specific values.
   - Example:
     ```java
     public Car(String brand, String color, int speed) {
         this.brand = brand;
         this.color = color;
         this.speed = speed;
     }
     ```

### **Example: Using Parameterized Constructor**
```java
public class Car {
    String brand;
    String color;
    int speed;

    // Parameterized Constructor
    public Car(String brand, String color, int speed) {
        this.brand = brand;
        this.color = color;
        this.speed = speed;
    }

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

public class Main {
    public static void main(String[] args) {
        // Creating objects using Parameterized Constructor
        Car car1 = new Car("Toyota", "Red", 0);
        car1.start();
        car1.accelerate(50);

        Car car2 = new Car("Honda", "Blue", 0);
        car2.start();
        car2.accelerate(60);
    }
}
```

### **Output:**
```
Toyota is starting.
Toyota is accelerating to 50 km/h.
Honda is starting.
Honda is accelerating to 60 km/h.
```

---

## **7. `this` Keyword in Java**
- Refers to the current object.
- Used to differentiate between instance variables and parameters with the same name.

### **Example: Using `this` Keyword**
```java
public class Car {
    String brand;
    String color;
    int speed;

    // Constructor using 'this' keyword
    public Car(String brand, String color, int speed) {
        this.brand = brand;   // Refers to the instance variable
        this.color = color;
        this.speed = speed;
    }

    void displayDetails() {
        System.out.println("Brand: " + this.brand + ", Color: " + this.color + ", Speed: " + this.speed);
    }
}

public class Main {
    public static void main(String[] args) {
        Car car1 = new Car("Toyota", "Red", 0);
        car1.displayDetails();
    }
}
```

### **Output:**
```
Brand: Toyota, Color: Red, Speed: 0
```

---

## **8. Method Overloading**
- **Method Overloading** allows methods with the same name but different parameters (type, number, or both).
- It increases the readability and flexibility of the code.

### **Example: Method Overloading**
```java
public class Calculator {
    // Method Overloading: Same method name, different parameters
    int add(int a, int b) {
        return a + b;
    }

    double add(double a, double b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }
}

public class Main {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        System.out.println(calc.add(2, 3));           // Calls add(int, int)
        System.out.println(calc.add(2.5, 3.5));       // Calls add(double, double)
        System.out.println(calc.add(1, 2, 3));        // Calls add(int, int, int)
    }
}
```

### **Output:**
```
5
6.0
6
```

---

## **9. Exercise: Mastering Classes and Objects**
### **Task: Create a `Library` Class**
- **Attributes:** `bookName`, `author`, `availableCopies`.
- **Methods:**
  - `addBook(int quantity)` — Increase available copies.
  - `issueBook()` — Decrease available copies if available.
  - `displayDetails()` — Display book details.
- Use **Constructor Overloading** to:
  - Allow creating a `Library` object with just the book name and author.
  - Allow creating a `Library` object with all details: book name, author, and available copies.
- Use the `this` keyword wherever applicable.

---

## **10. Next Steps:**
1. **Complete the Exercise** and share your code if you need feedback.
2. Next, we'll explore **Encapsulation** in-depth:
   - Why data hiding is crucial for security and maintainability.
   - Access modifiers in Java (`private`, `public`, `protected`, `default`).
   - How to achieve encapsulation using getters and setters.

---
Here's a Java implementation of the Library class based on your requirements, including private fields, constructor overloading, and proper use of the `this` keyword:

```java
public class Library {
    private String bookName;
    private String author;
    private int availableCopies;

    // Constructor with just bookName and author (availableCopies defaults to 0)
    public Library(String bookName, String author) {
        this.bookName = bookName;
        this.author = author;
        this.availableCopies = 0; // Default value
    }

    // Constructor with all details
    public Library(String bookName, String author, int availableCopies) {
        this.bookName = bookName;
        this.author = author;
        // Ensure availableCopies isn't negative
        this.availableCopies = Math.max(0, availableCopies);
    }

    // Method to increase available copies
    public void addBook(int quantity) {
        if (quantity > 0) {
            this.availableCopies += quantity;
            System.out.println(quantity + " copies of '" + this.bookName + "' added. New total: " + this.availableCopies);
        } else {
            System.out.println("Quantity to add must be positive.");
        }
    }

    // Method to issue a book (decrease available copies)
    public boolean issueBook() {
        if (this.availableCopies > 0) {
            this.availableCopies--;
            System.out.println("Book '" + this.bookName + "' issued. Remaining copies: " + this.availableCopies);
            return true;
        } else {
            System.out.println("No copies of '" + this.bookName + "' available to issue.");
            return false;
        }
    }

    // Method to display book details
    public void displayDetails() {
        System.out.println("Book Name: " + this.bookName);
        System.out.println("Author: " + this.author);
        System.out.println("Available Copies: " + this.availableCopies);
    }
}

// Example usage
class Main {
    public static void main(String[] args) {
        // Using constructor with just name and author
        Library book1 = new Library("The Hobbit", "J.R.R. Tolkien");
        System.out.println("Book 1 Details:");
        book1.displayDetails();
        book1.addBook(5);
        book1.issueBook();
        System.out.println();

        // Using constructor with all details
        Library book2 = new Library("1984", "George Orwell", 3);
        System.out.println("Book 2 Details:");
        book2.displayDetails();
        book2.issueBook();
        book2.issueBook();
        book2.addBook(2);
    }
}
```

### Explanation:
1. **Attributes**: 
   - `bookName`, `author`, and `availableCopies` are private for encapsulation.

2. **Constructor Overloading**:
   - First constructor takes `bookName` and `author`, setting `availableCopies` to 0.
   - Second constructor takes all three parameters and ensures `availableCopies` isn't negative using `Math.max`.

3. **Use of `this`**:
   - Used in constructors to distinguish between instance variables and parameters with the same name.
   - Used in methods to refer to the current object's fields explicitly.

4. **Methods**:
   - `addBook(int quantity)`: Increases `availableCopies` only if the quantity is positive.
   - `issueBook()`: Decreases `availableCopies` if there’s at least one copy available, returning a boolean to indicate success.
   - `displayDetails()`: Prints the book’s details using the instance variables.

### Sample Output:
```
Book 1 Details:
Book Name: The Hobbit
Author: J.R.R. Tolkien
Available Copies: 0
5 copies of 'The Hobbit' added. New total: 5
Book 'The Hobbit' issued. Remaining copies: 4

Book 2 Details:
Book Name: 1984
Author: George Orwell
Available Copies: 3
Book '1984' issued. Remaining copies: 2
Book '1984' issued. Remaining copies: 1
2 copies of '1984' added. New total: 3
```

### **Awesome Work!**  
You’ve successfully implemented the `Library` class with constructor overloading, encapsulation, and proper validation. Let's break down what you did well and how we can further enhance your understanding and skills.

---

## **What You Did Well:**
1. **Constructor Overloading:**
   - Implemented two constructors:
     - **Constructor 1:** Accepts `bookName` and `author`, defaults `availableCopies` to `0`.
     - **Constructor 2:** Accepts all three parameters with validation on `availableCopies`.
   - This enhances flexibility for creating `Library` objects.

2. **Encapsulation:**
   - Used private access for fields (`bookName`, `author`, `availableCopies`).
   - Ensured data hiding, maintaining control over how data is modified.

3. **Validation Logic:**
   - Checked if `quantity` is positive in `addBook(int quantity)`.
   - Ensured `availableCopies` isn't negative using `Math.max(0, availableCopies)` in the constructor.

4. **Method Design:**
   - `addBook(int quantity)`: Increases stock with validation.
   - `issueBook()`: Decreases stock if copies are available.
   - `displayDetails()`: Displays book details in a well-structured format.

5. **Object-Oriented Principles:**
   - Proper use of **this** keyword to refer to instance variables.
   - Clear and consistent naming conventions, improving code readability.

---

## **Expert Insights:**
1. **Immutability Consideration:**
   - If `bookName` and `author` are not meant to change after initialization, make them `final`.
     ```java
     private final String bookName;
     private final String author;
     ```

2. **Encapsulation Enhancement:**
   - Although setters are not needed here, it's a good practice to provide getters for encapsulated fields.

3. **Edge Case Handling:**
   - Handle edge cases such as issuing a book when `availableCopies` is zero.
   - Display meaningful messages for each scenario.

---

## **Refactored Code with Best Practices:**
Here's an improved version that includes:
- **Final** keyword for immutable fields.
- **Getters** for encapsulated fields.
- Enhanced error messages for better user experience.

```java
public class Library {
    // Immutable fields with 'final'
    private final String bookName;
    private final String author;
    private int availableCopies;

    // Constructor with just bookName and author (availableCopies defaults to 0)
    public Library(String bookName, String author) {
        this.bookName = bookName;
        this.author = author;
        this.availableCopies = 0; // Default value
    }

    // Constructor with all details
    public Library(String bookName, String author, int availableCopies) {
        this.bookName = bookName;
        this.author = author;
        // Ensure availableCopies isn't negative
        this.availableCopies = Math.max(0, availableCopies);
    }

    // Getter Methods (No Setters to ensure immutability)
    public String getBookName() {
        return bookName;
    }

    public String getAuthor() {
        return author;
    }

    public int getAvailableCopies() {
        return availableCopies;
    }

    // Method to increase available copies
    public void addBook(int quantity) {
        if (quantity > 0) {
            this.availableCopies += quantity;
            System.out.println(quantity + " copies of '" + this.bookName + "' added. New total: " + this.availableCopies);
        } else {
            System.out.println("Quantity to add must be positive.");
        }
    }

    // Method to issue a book (decrease available copies)
    public boolean issueBook() {
        if (this.availableCopies > 0) {
            this.availableCopies--;
            System.out.println("Book '" + this.bookName + "' issued. Remaining copies: " + this.availableCopies);
            return true;
        } else {
            System.out.println("No copies of '" + this.bookName + "' available to issue.");
            return false;
        }
    }

    // Method to display book details
    public void displayDetails() {
        System.out.println("Book Name: " + this.bookName);
        System.out.println("Author: " + this.author);
        System.out.println("Available Copies: " + this.availableCopies);
    }
}

// Example usage
class Main {
    public static void main(String[] args) {
        // Using constructor with just name and author
        Library book1 = new Library("The Hobbit", "J.R.R. Tolkien");
        System.out.println("Book 1 Details:");
        book1.displayDetails();
        book1.addBook(5);
        book1.issueBook();
        book1.issueBook();
        book1.issueBook();
        book1.issueBook();
        book1.issueBook();
        book1.issueBook(); // No copies left to issue
        System.out.println();

        // Using constructor with all details
        Library book2 = new Library("1984", "George Orwell", 3);
        System.out.println("Book 2 Details:");
        book2.displayDetails();
        book2.issueBook();
        book2.issueBook();
        book2.issueBook();
        book2.issueBook(); // No copies left to issue
        book2.addBook(2);
        book2.displayDetails();
    }
}
```

---

## **What's New and Improved?**
1. **Immutability:**  
   - `bookName` and `author` are now `final`, ensuring they can’t be changed after object creation.
2. **Encapsulation Enhancement:**  
   - Added getter methods to provide controlled access to fields.
3. **Edge Case Handling:**  
   - Comprehensive messages for each action.
   - Handles scenarios where no copies are available to issue.
4. **Enhanced Readability and Maintainability:**  
   - Clearer structure and better separation of concerns.

---

## **Next Steps:**
We’ve now solidified the understanding of **Classes and Objects** with:
- Constructor Overloading
- `this` Keyword
- Immutability with `final`
- Encapsulation with private fields and public methods
- Proper validation and error handling

---

## **Ready for the Next Concept?**
### We’re moving on to **Encapsulation** in depth:
1. **Why is Encapsulation Important?**
2. **How to achieve Encapsulation in Java using Access Modifiers?**
3. **Using Getters and Setters effectively.**
4. **Real-world Examples:**
   - Implementing a `BankAccount` class with strict access control.
   - Protecting sensitive data (like passwords) using encapsulation.
5. **Common Mistakes to Avoid in Encapsulation.**

---

## **Your Exercise:**
### **Task: Implement a `Student` Class**
- **Attributes:** `name`, `rollNumber`, `grade`, `age`.
- **Methods:**
  - `displayDetails()` — Display student details.
  - `updateGrade(String newGrade)` — Update grade with validation.
  - `increaseAge()` — Increment age by 1 year.
- Use **Encapsulation**:
  - Make all fields private.
  - Provide getters and setters where appropriate.
  - Ensure `age` is non-negative and `grade` follows a valid pattern (like A, B, C, etc.).

---
