
## 📘 MODULE 7: Methods & Recursion in Java

---

## 🧠 7.1 What is a Method?

A **method** is a **block of code** that performs a specific task and can be **called** whenever needed.

### ✅ Syntax:

```java
returnType methodName(parameters) {
    // code block
    return value;
}
```

---

### 🧪 Example:

```java
public class Greeting {

    public static void sayHello(String name) {
        System.out.println("Hello, " + name + "!");
    }

    public static void main(String[] args) {
        sayHello("Tushar");
        sayHello("Java");
    }
}
```

🟢 Output:

```
Hello, Tushar!
Hello, Java!
```

---

## 🧠 7.2 Return Types & Parameters

| Part        | Example                 |
| ----------- | ----------------------- |
| Return type | `int`, `String`, `void` |
| Parameters  | `(int a, int b)`        |
| Method name | `add`, `printName`      |

### ✅ Example – Sum of Two Numbers:

```java
public static int add(int a, int b) {
    return a + b;
}
```

---

## 🧠 7.3 Method Overloading

Multiple methods with the **same name** but **different parameters**.

```java
public static int square(int a) {
    return a * a;
}

public static double square(double a) {
    return a * a;
}
```

---

## 🧠 7.4 What is Recursion?

A method that **calls itself** to solve a problem.

### ✅ Real-World Example: Russian dolls (each one inside a bigger one)

---

### 🧪 Recursive Example: Factorial

```java
public static int factorial(int n) {
    if (n == 1) return 1;
    return n * factorial(n - 1);
}
```

🟢 `factorial(5)` calls:

```
5 * factorial(4)
4 * factorial(3)
3 * factorial(2)
2 * factorial(1) → returns 1
```

---

### ⚠️ Recursion Best Practices

| Tip                               | Why                                |
| --------------------------------- | ---------------------------------- |
| Always have a **base condition**  | To stop infinite recursion         |
| Prefer loops for large iterations | Recursion can cause stack overflow |

---

## 📝 Exercises

### ✅ Q1: Write a method to return the **square** of a number.

---

### ✅ Q2: Write a method that takes two numbers and returns their **maximum**.

---

### ✅ Q3: Write a method `isEven(int n)` that returns `true` if number is even, otherwise `false`.

---

### ✅ Q4: Write a **recursive method** to calculate factorial of a number.

---

### ✅ Q5: What is the difference between recursion and iteration?

---

```java
public class MethodsExercises {
    // Q1: Method to return the square of a number
    public static int square(int number) {
        return number * number;
    }

    // Q2: Method to return the maximum of two numbers
    public static int max(int a, int b) {
        return a > b ? a : b;
    }

    // Q3: Method to check if a number is even
    public static boolean isEven(int n) {
        return n % 2 == 0;
    }

    // Q4: Recursive method to calculate factorial
    public static long factorial(int n) {
        if (n < 0) {
            throw new IllegalArgumentException("Factorial is not defined for negative numbers");
        }
        if (n == 0 || n == 1) {
            return 1;
        }
        return n * factorial(n - 1);
    }

    // Main method to test the above methods
    public static void main(String[] args) {
        // Testing Q1
        System.out.println("Q1: Square of 5: " + square(5)); // Output: 25

        // Testing Q2
        System.out.println("Q2: Maximum of 10 and 7: " + max(10, 7)); // Output: 10

        // Testing Q3
        System.out.println("Q3: Is 4 even? " + isEven(4)); // Output: true
        System.out.println("Q3: Is 7 even? " + isEven(7)); // Output: false

        // Testing Q4
        System.out.println("Q4: Factorial of 5: " + factorial(5)); // Output: 120
    }
}
```

**Q5: Difference between recursion and iteration**  
- **Recursion**: A method calls itself to solve a problem by breaking it into smaller subproblems. It uses a call stack, which can lead to higher memory usage. Example: Calculating factorial by calling `factorial(n-1)`. It’s often more elegant but can cause stack overflow for large inputs.  
- **Iteration**: Uses loops (e.g., for, while) to repeatedly execute code until a condition is met. It typically uses less memory since it doesn’t rely on a call stack. Example: Using a for loop to sum numbers. It’s generally faster and more efficient for large datasets.  
- **Key Differences**:  
  - Recursion is declarative and often simpler to write for problems like tree traversal; iteration is imperative and better for performance in repetitive tasks.  
  - Recursion risks stack overflow; iteration doesn’t.  
  - Recursion may be less intuitive for simple tasks; iteration is straightforward but can be verbose for complex problems.
