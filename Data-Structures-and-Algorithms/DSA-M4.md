## 🚀 **1.6 Mastering Recursion - In Simple Terms**  

Recursion is a way of solving a problem where the solution depends on solutions to smaller instances of the same problem. It can be a bit confusing at first, but once you get the hang of it, it's super powerful!

---

## **🔍 What is Recursion?**  
- **Recursion** is when a function calls itself to solve a smaller version of the same problem.  
- Think of it like looking at a mirror reflecting another mirror, which shows an endless series of smaller reflections.  
- **Example:** When you want to solve a problem, you break it down into smaller, similar problems until you reach the simplest form (base case).  

---

## **💡 How Does Recursion Work?**  
Recursion works in two main parts:  
1. **Base Case:** This is the condition that stops the recursion. Without this, the function would call itself forever!  
2. **Recursive Case:** This is where the function calls itself with a smaller or simpler input.  

### 🎨 **Visual Example:**  
Imagine you are standing on a staircase, and you want to reach the top.  
- You can either:
  - Take 1 step and then figure out how to climb the remaining steps.  
  - Or, if you're at the last step (base case), you’re already at the top!  

---

## **🔥 Example 1: Factorial Calculation**  
Factorial of a number (`n!`) is the product of all positive numbers less than or equal to `n`.  
- **Example:** `5! = 5 * 4 * 3 * 2 * 1 = 120`  

### 📘 **In Simple Terms:**  
- To find the factorial of 5 (`5!`), you need to multiply 5 by the factorial of 4 (`4!`).  
- To find `4!`, you need `4 * 3!`, and so on, until you reach `1!`, which is `1`.  
- This is a smaller version of the same problem!  

---

### 📘 **Mathematical Definition:**  
```
n! = n * (n-1) * (n-2) * ... * 1
0! = 1 (By definition)
```

### 📘 **Recursive Definition:**  
```
n! = n * (n-1)!
Base Case: 0! = 1
```

---

### 📘 **Example Code: Factorial using Recursion**  
```java
public class RecursionExample {
    // Recursive method to calculate factorial
    public static long factorial(int n) {
        // Base Case
        if (n <= 1) {
            return 1;  // If n is 0 or 1, return 1 (Base Case)
        }
        // Recursive Case
        return n * factorial(n - 1);  // Multiply n by factorial of (n-1)
    }

    public static void main(String[] args) {
        int num = 5;
        System.out.println("Factorial of " + num + " is: " + factorial(num));
    }
}
```

---

### 📊 **Output:**  
```
Factorial of 5 is: 120
```

---

### 🔥 **What's Happening Behind the Scenes?**  
1. `factorial(5)` → Calls `factorial(4)` → Calls `factorial(3)` → Calls `factorial(2)` → Calls `factorial(1)`  
2. `factorial(1)` returns `1` (Base Case)  
3. `factorial(2)` becomes `2 * 1 = 2`  
4. `factorial(3)` becomes `3 * 2 = 6`  
5. `factorial(4)` becomes `4 * 6 = 24`  
6. `factorial(5)` becomes `5 * 24 = 120`  

**Key Point:** The function keeps calling itself until it hits the base case, then the results are calculated as it "returns" from each call.  

---

### 🔥 **Important Points to Remember:**  
- **Base Case** is crucial to stop the recursion and prevent infinite loops.  
- **Recursive Case** breaks down the problem into smaller instances.  
- The function calls are stored in the **Call Stack** and resolved one by one as the base case is reached.  

---

## **🔥 Example 2: Fibonacci Series**  
Fibonacci series is a sequence of numbers where each number is the sum of the two preceding ones.  
- **Example:** `0, 1, 1, 2, 3, 5, 8, 13, ...`  

---

### 📘 **Recursive Definition:**  
```
F(n) = F(n-1) + F(n-2)
Base Cases:
    F(0) = 0
    F(1) = 1
```

---

### 📘 **Example Code: Fibonacci using Recursion**  
```java
public class RecursionExample {
    // Recursive method to calculate Fibonacci
    public static int fibonacci(int n) {
        // Base Cases
        if (n == 0) return 0;
        if (n == 1) return 1;
        
        // Recursive Case
        return fibonacci(n - 1) + fibonacci(n - 2);
    }

    public static void main(String[] args) {
        int num = 6;
        System.out.println("Fibonacci of " + num + " is: " + fibonacci(num));
    }
}
```

---

### 📊 **Output:**  
```
Fibonacci of 6 is: 8
```

---

### 🔥 **What's Happening Behind the Scenes?**  
1. `fibonacci(6)` → Calls `fibonacci(5)` and `fibonacci(4)`  
2. `fibonacci(5)` → Calls `fibonacci(4)` and `fibonacci(3)`  
3. This continues until the base cases are reached (`fibonacci(0)` or `fibonacci(1)`)  
4. The results are added up as the function returns from each call.  

---

### 🔥 **Key Points to Remember:**  
- Recursive Fibonacci is not efficient because it recalculates the same values multiple times.  
- **Optimization:** Use **Memoization** to store previously calculated values.  

---

## **🔥 Common Mistakes to Avoid**  
- **Missing Base Case:** This leads to infinite recursion and a `StackOverflowError`.  
- **Infinite Recursion:** Occurs when the recursive call does not reduce the problem size.  
- **Confusing Flow:** Remember that recursive calls are stacked and resolved last-in, first-out.  

---

## **📝 Exercise Set:**  
1. Write a recursive function to calculate the sum of digits of a number.  
2. Implement a recursive function to reverse a string.  
3. Write a recursive function to find the greatest common divisor (GCD) of two numbers.  
4. Implement a recursive solution for the Tower of Hanoi problem.  
5. Write a recursive function to count the number of ways to climb `n` stairs, taking either 1 or 2 steps at a time.  

---
### 🚀 **1.6 Mastering Recursion - Let's Continue!**  

We’ve covered the basics of recursion, but to truly master it, we need to see more examples and get comfortable with how the call stack works. Let's continue with more practical examples and solve some real problems!

---

## **🔥 Example 3: Sum of Digits of a Number**  
Let's find the sum of digits of a number using recursion.  

### 📘 **Problem Statement:**  
- Given a number, find the sum of its digits.  
- **Example:**  
    - `Input: 1234`  
    - `Output: 1 + 2 + 3 + 4 = 10`

---

### 📘 **Mathematical Breakdown:**  
- Last digit of a number = `n % 10`  
- Remaining digits = `n / 10`  
- **Recursive Definition:**  
    ```
    sumOfDigits(n) = lastDigit + sumOfDigits(remainingDigits)
                   = (n % 10) + sumOfDigits(n / 10)
    Base Case: if n == 0 → return 0
    ```

---

### 📘 **Example Code: Sum of Digits using Recursion**  
```java
public class RecursionExample {
    // Recursive method to calculate sum of digits
    public static int sumOfDigits(int n) {
        // Base Case
        if (n == 0) {
            return 0;
        }
        // Recursive Case
        return (n % 10) + sumOfDigits(n / 10);
    }

    public static void main(String[] args) {
        int num = 1234;
        System.out.println("Sum of digits of " + num + " is: " + sumOfDigits(num));
    }
}
```

---

### 📊 **Output:**  
```
Sum of digits of 1234 is: 10
```

---

### 🔥 **What's Happening Behind the Scenes?**  
Let's see how the function calls are made for `sumOfDigits(1234)`:
```
sumOfDigits(1234) = 4 + sumOfDigits(123)
sumOfDigits(123)  = 3 + sumOfDigits(12)
sumOfDigits(12)   = 2 + sumOfDigits(1)
sumOfDigits(1)    = 1 + sumOfDigits(0)
sumOfDigits(0)    = 0 (Base Case)
```

Then it resolves backwards:
```
sumOfDigits(1) = 1 + 0 = 1
sumOfDigits(2) = 2 + 1 = 3
sumOfDigits(3) = 3 + 3 = 6
sumOfDigits(4) = 4 + 6 = 10
```

### 🔥 **Key Points to Remember:**  
- The last digit is calculated first (`n % 10`).  
- The problem size is reduced (`n / 10`) in each step.  
- The base case stops the recursion when `n` becomes `0`.  

---

## **🔥 Example 4: Reverse a String using Recursion**  
Let's reverse a string using recursion.  

### 📘 **Problem Statement:**  
- Given a string, reverse it using recursion.  
- **Example:**  
    - `Input: "Maya"`  
    - `Output: "ayaM"`

---

### 📘 **Recursive Definition:**  
- Last character of the string = `str.charAt(str.length() - 1)`  
- Remaining substring = `str.substring(0, str.length() - 1)`  
- **Recursive Formula:**  
    ```
    reverse(str) = lastChar + reverse(remainingString)
    Base Case: if str is empty → return ""
    ```

---

### 📘 **Example Code: Reverse a String using Recursion**  
```java
public class RecursionExample {
    // Recursive method to reverse a string
    public static String reverseString(String str) {
        // Base Case
        if (str.isEmpty()) {
            return str;
        }
        // Recursive Case
        return reverseString(str.substring(1)) + str.charAt(0);
    }

    public static void main(String[] args) {
        String input = "Maya";
        System.out.println("Reversed string: " + reverseString(input));
    }
}
```

---

### 📊 **Output:**  
```
Reversed string: ayaM
```

---

### 🔥 **What's Happening Behind the Scenes?**  
Let's see how the function calls are made for `reverseString("Maya")`:
```
reverseString("Maya") = reverseString("aya") + 'M'
reverseString("aya")  = reverseString("ya") + 'a'
reverseString("ya")   = reverseString("a") + 'y'
reverseString("a")    = reverseString("") + 'a'
reverseString("")     = "" (Base Case)
```

Then it resolves backwards:
```
reverseString("a")  = "" + 'a' = "a"
reverseString("ya") = "a" + 'y' = "ay"
reverseString("aya") = "ay" + 'a' = "aya"
reverseString("Maya") = "aya" + 'M' = "ayaM"
```

---

### 🔥 **Key Points to Remember:**  
- The last character is added first.  
- The problem size is reduced by slicing the string (`str.substring(1)`).  
- The base case stops the recursion when the string becomes empty.  

---

## **🔥 Common Mistakes to Avoid**  
- **Missing Base Case:** Leads to infinite recursion and `StackOverflowError`.  
- **Misunderstanding Flow:** Remember, recursion goes deeper first, then resolves backwards.  
- **Confusing Return Statement:** `return` sends the result back to the previous call.  

---

## **📝 Exercise Set:**  
1. Write a recursive function to calculate the product of digits of a number.  
2. Implement a recursive function to check if a string is a palindrome.  
3. Write a recursive function to find the maximum element in an array.  
4. Implement a recursive solution for the Tower of Hanoi problem.  
5. Write a recursive function to calculate the power of a number (i.e., `x^n`).  

---

## 🔥 **Ready for Module 2?**  
Next, we will dive into **Arrays and Strings** — the building blocks of data structures.  

- Do you feel more comfortable with Recursion now?  
- Would you like more examples or simpler explanations?  
- Were you able to run the example programs successfully?
