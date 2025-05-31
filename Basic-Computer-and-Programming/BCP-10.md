You're doing fantastic, Tushar! 🎯
Now we move into the **real-world challenges** of programming: handling **errors gracefully** and working with **files**.

---

## 📘 MODULE 9: Exception Handling & File I/O in Java

---

## 🧠 9.1 What is an Exception?

An **exception** is an **unwanted event** (error) that occurs during program execution.

### ✅ Example:

```java
int a = 5 / 0;  // ❌ ArithmeticException: Divide by zero
```

---

## 🧠 9.2 Java Exception Types

| Type                | Description                                 |
| ------------------- | ------------------------------------------- |
| Checked Exception   | Checked at compile-time (`IOException`)     |
| Unchecked Exception | Happens at runtime (`NullPointerException`) |

---

## 🧠 9.3 Try-Catch Block

### ✅ Syntax:

```java
try {
    // risky code
} catch (ExceptionType e) {
    // handle error
}
```

### 🧪 Example:

```java
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero!");
}
```

---

## 🧠 9.4 Finally Block

```java
try {
    // risky code
} catch (Exception e) {
    // handle it
} finally {
    System.out.println("Cleanup code here (runs always)");
}
```

---

## 🧠 9.5 Throw and Throws

```java
public void checkAge(int age) throws Exception {
    if (age < 18)
        throw new Exception("Not eligible");
}
```

---

## 🧠 9.6 Custom Exception

```java
class InvalidAgeException extends Exception {
    public InvalidAgeException(String msg) {
        super(msg);
    }
}
```

---

## 🧠 9.7 Java File Handling (File I/O)

### ✅ Import Required:

```java
import java.io.*;
```

---

### ✍️ Writing to a File:

```java
FileWriter writer = new FileWriter("output.txt");
writer.write("Hello from Java!");
writer.close();
```

---

### 📖 Reading from a File:

```java
BufferedReader reader = new BufferedReader(new FileReader("output.txt"));
String line = reader.readLine();
while (line != null) {
    System.out.println(line);
    line = reader.readLine();
}
reader.close();
```

---

### ⚠️ Common Mistakes

| Mistake                     | Fix                          |
| --------------------------- | ---------------------------- |
| Not closing the file        | Always close using `close()` |
| Unhandled checked exception | Use `try-catch` or `throws`  |

---

## 📝 Exercises

### ✅ Q1: Write a program to divide two numbers with `try-catch` to handle division by zero.

---

### ✅ Q2: Create your own exception `InvalidPasswordException` and throw it when password length < 6.

---

### ✅ Q3: Write to a file `greetings.txt` with your name and message.

---

### ✅ Q4: Read the content of `greetings.txt` and display it on the console.

---

```java
import java.io.*;

// Q2: Custom exception for invalid password
class InvalidPasswordException extends Exception {
    public InvalidPasswordException(String message) {
        super(message);
    }
}

public class ExceptionFileHandling {
    public static void main(String[] args) {
        // Q1: Divide two numbers with try-catch for division by zero
        try {
            int a = 10;
            int b = 0; // Change to test different scenarios
            double result = divide(a, b);
            System.out.println("Q1: Result of " + a + " / " + b + " = " + result);
        } catch (ArithmeticException e) {
            System.out.println("Q1: Error: " + e.getMessage());
        }

        // Q2: Test custom InvalidPasswordException
        try {
            String password = "short"; // Change to test different passwords
            checkPassword(password);
            System.out.println("Q2: Password is valid");
        } catch (InvalidPasswordException e) {
            System.out.println("Q2: Error: " + e.getMessage());
        }

        // Q3: Write to greetings.txt
        String name = "Tushar";
        String message = "Hello, welcome to Java programming!";
        try (FileWriter writer = new FileWriter("greetings.txt")) {
            writer.write("Name: " + name + "\nMessage: " + message);
            System.out.println("Q3: Successfully wrote to greetings.txt");
        } catch (IOException e) {
            System.out.println("Q3: Error writing to file: " + e.getMessage());
        }

        // Q4: Read from greetings.txt and display
        try (BufferedReader reader = new BufferedReader(new FileReader("greetings.txt"))) {
            System.out.println("\nQ4: Content of greetings.txt:");
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            System.out.println("Q4: Error reading file: " + e.getMessage());
        }
    }

    // Q1: Method for division
    public static double divide(int a, int b) {
        if (b == 0) {
            throw new ArithmeticException("Division by zero is not allowed");
        }
        return (double) a / b;
    }

    // Q2: Method to check password length
    public static void checkPassword(String password) throws InvalidPasswordException {
        if (password.length() < 6) {
            throw new InvalidPasswordException("Password must be at least 6 characters long");
        }
    }
}
```
