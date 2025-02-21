## 🚀 **1.4 Java Basics Refresher**  

Before diving into Data Structures and Algorithms, we need to master the basics of Java. This section covers the core concepts that will be used frequently throughout your DSA journey.

---

## **🔥 1.4.1 Data Types in Java**  
Java is a statically-typed language, meaning every variable must have a data type.  

### 📘 **Primitive Data Types:**  
- **int**: Integer numbers (e.g., 1, 100, -25)  
- **float**: Decimal numbers (e.g., 1.2f, 3.14f)  
- **double**: More precise decimal numbers (e.g., 3.14159)  
- **char**: Single character (e.g., 'A', '9')  
- **boolean**: true or false  
- **byte**: 8-bit integer  
- **short**: 16-bit integer  
- **long**: 64-bit integer (e.g., 100000L)  

---

### 📘 **Example:**  
```java
public class DataTypeExample {
    public static void main(String[] args) {
        int age = 25;
        float price = 19.99f;
        double pi = 3.141592653589793;
        char grade = 'A';
        boolean isJavaFun = true;
        long population = 7800000000L;
        
        System.out.println("Age: " + age);
        System.out.println("Price: $" + price);
        System.out.println("Value of Pi: " + pi);
        System.out.println("Grade: " + grade);
        System.out.println("Is Java fun? " + isJavaFun);
        System.out.println("World Population: " + population);
    }
}
```

---

### 📊 **Output:**  
```
Age: 25
Price: $19.99
Value of Pi: 3.141592653589793
Grade: A
Is Java fun? true
World Population: 7800000000
```

---

## **🔥 1.4.2 Control Statements**  

### **Conditional Statements:**  
- **if-else**: Decision-making statement.  
- **switch**: Used when multiple conditions depend on a single variable.  

### 📘 **Example: if-else and switch**  
```java
public class ControlStatementsExample {
    public static void main(String[] args) {
        int number = 10;

        // if-else Statement
        if (number > 0) {
            System.out.println("Positive Number");
        } else if (number < 0) {
            System.out.println("Negative Number");
        } else {
            System.out.println("Number is Zero");
        }

        // switch Statement
        int day = 3;
        switch (day) {
            case 1: System.out.println("Monday"); break;
            case 2: System.out.println("Tuesday"); break;
            case 3: System.out.println("Wednesday"); break;
            default: System.out.println("Invalid Day");
        }
    }
}
```

---

### 📊 **Output:**  
```
Positive Number
Wednesday
```

---

### **Loops in Java:**  
- **for loop**: Repeats a block of code a known number of times.  
- **while loop**: Repeats a block until a condition becomes false.  
- **do-while loop**: Executes at least once before checking the condition.  

### 📘 **Example: Loops in Java**  
```java
public class LoopExample {
    public static void main(String[] args) {
        // for loop
        for (int i = 1; i <= 5; i++) {
            System.out.println("For Loop: " + i);
        }

        // while loop
        int j = 1;
        while (j <= 5) {
            System.out.println("While Loop: " + j);
            j++;
        }

        // do-while loop
        int k = 1;
        do {
            System.out.println("Do-While Loop: " + k);
            k++;
        } while (k <= 5);
    }
}
```

---

### 📊 **Output:**  
```
For Loop: 1
For Loop: 2
For Loop: 3
For Loop: 4
For Loop: 5
While Loop: 1
While Loop: 2
While Loop: 3
While Loop: 4
While Loop: 5
Do-While Loop: 1
Do-While Loop: 2
Do-While Loop: 3
Do-While Loop: 4
Do-While Loop: 5
```

---

## **🔥 1.4.3 Functions (Methods) in Java**  
- A block of code that performs a specific task.  
- Can take parameters and return a value.  
- Increases code reusability and modularity.  

### 📘 **Example: Methods in Java**  
```java
public class MethodExample {
    // Method to add two numbers
    public static int addNumbers(int a, int b) {
        return a + b;
    }

    // Main method
    public static void main(String[] args) {
        int sum = addNumbers(5, 10);
        System.out.println("Sum: " + sum);
    }
}
```

---

### 📊 **Output:**  
```
Sum: 15
```

---

## **🔥 1.4.4 Arrays in Java**  
- **Definition**: A collection of elements of the same type stored in contiguous memory locations.  
- **Declaration**:  
    ```java
    int[] numbers = new int[5];
    String[] names = {"John", "Jane", "Alex"};
    ```
- **Indexing**: Arrays are zero-indexed. First element is at index 0.  

---

### 📘 **Example: Arrays in Java**  
```java
public class ArrayExample {
    public static void main(String[] args) {
        int[] numbers = {10, 20, 30, 40, 50};

        // Accessing array elements
        System.out.println("First Element: " + numbers[0]);
        System.out.println("Second Element: " + numbers[1]);

        // Looping through array
        System.out.println("Array Elements:");
        for (int num : numbers) {
            System.out.print(num + " ");
        }
    }
}
```

---

### 📊 **Output:**  
```
First Element: 10
Second Element: 20
Array Elements:
10 20 30 40 50
```

---

## **🔥 1.4.5 Common Mistakes to Avoid**  
- Array Index Out of Bounds Exception: Accessing non-existent indexes.  
- Forgetting the `break` statement in `switch` cases.  
- Infinite loops due to wrong conditions.  
- Missing return statements in methods with non-void return types.  

---

## **📝 Exercise Set:**  
1. Write a program to find the maximum of three numbers using `if-else`.  
2. Implement a basic calculator using `switch-case`.  
3. Create an array of 10 numbers and find the sum and average.  
4. Write a method to calculate the factorial of a number using a loop.  
5. Implement a method that checks if a number is prime.  

---
---

### Solutions
Here's a Java program that implements all the requested functionalities:

```java
public class Calculations {
    // Method to find maximum of three numbers
    public static int findMax(int a, int b, int c) {
        if (a >= b && a >= c) {
            return a;
        } else if (b >= a && b >= c) {
            return b;
        } else {
            return c;
        }
    }

    // Basic calculator using switch-case
    public static double calculator(double num1, double num2, char operator) {
        switch (operator) {
            case '+':
                return num1 + num2;
            case '-':
                return num1 - num2;
            case '*':
                return num1 * num2;
            case '/':
                if (num2 != 0) {
                    return num1 / num2;
                } else {
                    System.out.println("Error: Division by zero!");
                    return 0;
                }
            default:
                System.out.println("Error: Invalid operator!");
                return 0;
        }
    }

    // Method to calculate factorial
    public static long factorial(int n) {
        if (n < 0) {
            System.out.println("Error: Factorial not defined for negative numbers!");
            return -1;
        }
        long result = 1;
        for (int i = 1; i <= n; i++) {
            result *= i;
        }
        return result;
    }

    // Method to check if a number is prime
    public static boolean isPrime(int n) {
        if (n <= 1) {
            return false;
        }
        for (int i = 2; i <= Math.sqrt(n); i++) {
            if (n % i == 0) {
                return false;
            }
        }
        return true;
    }

    public static void main(String[] args) {
        // Finding maximum of three numbers
        int num1 = 25, num2 = 12, num3 = 45;
        System.out.println("Maximum of " + num1 + ", " + num2 + ", " + num3 + " is: " 
            + findMax(num1, num2, num3));

        // Calculator demonstration
        System.out.println("\nCalculator Results:");
        System.out.println("10 + 5 = " + calculator(10, 5, '+'));
        System.out.println("10 - 5 = " + calculator(10, 5, '-'));
        System.out.println("10 * 5 = " + calculator(10, 5, '*'));
        System.out.println("10 / 5 = " + calculator(10, 5, '/'));

        // Array operations
        int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
        int sum = 0;
        System.out.println("\nArray elements:");
        for (int num : numbers) {
            System.out.print(num + " ");
            sum += num;
        }
        double average = (double) sum / numbers.length;
        System.out.println("\nSum: " + sum);
        System.out.println("Average: " + average);

        // Factorial demonstration
        int factNum = 5;
        System.out.println("\nFactorial of " + factNum + " is: " + factorial(factNum));

        // Prime number check
        int primeNum = 17;
        System.out.println("\nIs " + primeNum + " prime? " + isPrime(primeNum));
        primeNum = 24;
        System.out.println("Is " + primeNum + " prime? " + isPrime(primeNum));
    }
}
```

This program includes:

1. `findMax()`: Uses if-else statements to find the maximum of three numbers.

2. `calculator()`: Implements a basic calculator using switch-case that supports addition (+), subtraction (-), multiplication (*), and division (/). Includes error handling for division by zero.

3. Array operations: Creates an array of 10 numbers (1-10), calculates their sum, and computes the average.

4. `factorial()`: Calculates factorial using a for loop with error checking for negative numbers. Uses long to handle larger factorials.

5. `isPrime()`: Checks if a number is prime by testing divisibility up to the square root of the number (optimization).

When you run this program, it will output:
```
Maximum of 25, 12, 45 is: 45

Calculator Results:
10 + 5 = 15.0
10 - 5 = 5.0
10 * 5 = 50.0
10 / 5 = 2.0

Array elements:
1 2 3 4 5 6 7 8 9 10 
Sum: 55
Average: 5.5

Factorial of 5 is: 120

Is 17 prime? true
Is 24 prime? false
```

The program demonstrates:
- Conditional statements (if-else)
- Switch-case implementation
- Array operations
- Loop structures
- Basic mathematical operations
- Error handling
- Method creation and usage

Each method is self-contained and can be used independently. The main method provides example usage of all implemented functionalities.
---

## Enhancements 
Great job! Your implementation covers all the exercises effectively and demonstrates a solid understanding of the basics. Here's a quick review and some insights to enhance your learning:

---

## **🔍 Code Review and Insights**  

### 🔥 **1. Finding Maximum of Three Numbers**  
```java
public static int findMax(int a, int b, int c) {
    if (a >= b && a >= c) {
        return a;
    } else if (b >= a && b >= c) {
        return b;
    } else {
        return c;
    }
}
```
- Excellent use of `if-else` for comparisons.  
- **Insight:** In real-world scenarios, this approach is readable but can be optimized using the `Math.max()` function for simplicity:
    ```java
    return Math.max(a, Math.max(b, c));
    ```

---

### 🔥 **2. Basic Calculator using switch-case**  
```java
public static double calculator(double num1, double num2, char operator) {
    switch (operator) {
        case '+':
            return num1 + num2;
        case '-':
            return num1 - num2;
        case '*':
            return num1 * num2;
        case '/':
            if (num2 != 0) {
                return num1 / num2;
            } else {
                System.out.println("Error: Division by zero!");
                return 0;
            }
        default:
            System.out.println("Error: Invalid operator!");
            return 0;
    }
}
```
- Excellent handling of division by zero.  
- **Insight:** For robustness, consider using `double.NaN` for invalid cases:
    ```java
    return Double.NaN;
    ```
  This better indicates an undefined result rather than `0`.

---

### 🔥 **3. Array Operations**  
```java
int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
int sum = 0;
for (int num : numbers) {
    sum += num;
}
double average = (double) sum / numbers.length;
```
- Good use of enhanced for-loop for iteration.  
- **Insight:** In competitive programming, you can also use the `Arrays.stream()` method:
    ```java
    int sum = Arrays.stream(numbers).sum();
    double average = Arrays.stream(numbers).average().orElse(0);
    ```

---

### 🔥 **4. Factorial Calculation**  
```java
public static long factorial(int n) {
    if (n < 0) {
        System.out.println("Error: Factorial not defined for negative numbers!");
        return -1;
    }
    long result = 1;
    for (int i = 1; i <= n; i++) {
        result *= i;
    }
    return result;
}
```
- Excellent handling of negative inputs.  
- **Insight:** You can also implement this using **Recursion**:
    ```java
    public static long recursiveFactorial(int n) {
        if (n <= 1) return 1;
        return n * recursiveFactorial(n - 1);
    }
    ```
  This approach showcases the power of recursion, which is fundamental in DSA.

---

### 🔥 **5. Prime Number Check**  
```java
public static boolean isPrime(int n) {
    if (n <= 1) return false;
    for (int i = 2; i <= Math.sqrt(n); i++) {
        if (n % i == 0) {
            return false;
        }
    }
    return true;
}
```
- Efficient optimization using `Math.sqrt(n)`, reducing the loop iterations.  
- **Insight:** You can further optimize it by checking for divisibility by `2` and `3` first:
    ```java
    if (n <= 1) return false;
    if (n <= 3) return true;
    if (n % 2 == 0 || n % 3 == 0) return false;
    for (int i = 5; i * i <= n; i += 6) {
        if (n % i == 0 || n % (i + 2) == 0) return false;
    }
    return true;
    ```
  This reduces the number of iterations even further.

---

## **📌 Common Mistakes to Avoid**  
- **Using `==` for String Comparison:** Use `.equals()` for comparing strings in Java.  
- **Division by Zero:** Always handle it to avoid runtime errors.  
- **Array Index Out of Bounds:** Check the index limit while iterating over arrays.  

---
