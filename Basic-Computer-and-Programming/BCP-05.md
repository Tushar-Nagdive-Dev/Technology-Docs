## 📘 MODULE 4: Java Variables, Data Types & Operators

---

## 🧠 4.1 What Are Variables?

A **variable** is like a **labeled box** in memory that stores data.

📌 Think of it like:

```
int age = 25;
```

💬 “I want to store the number 25 inside a box called `age`, and that box only accepts whole numbers (int).”

---

## 🧠 4.2 Java Data Types

Java is a **strongly typed** language — you must declare the type of every variable.

### ✅ Primitive Data Types:

| Type      | Description            | Example     |
| --------- | ---------------------- | ----------- |
| `int`     | Whole numbers          | 10, -5      |
| `double`  | Decimal numbers        | 10.5, -3.14 |
| `char`    | Single character       | 'A', 'x'    |
| `boolean` | True or false          | true, false |
| `String`  | Sequence of characters | "Hello"     |

---

## 🧪 Example Code:

```java
public class MyData {
    public static void main(String[] args) {
        int age = 27;
        double salary = 50500.75;
        char grade = 'A';
        boolean isProgrammer = true;
        String name = "Tushar";

        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
        System.out.println("Salary: ₹" + salary);
        System.out.println("Grade: " + grade);
        System.out.println("Is Programmer: " + isProgrammer);
    }
}
```

---

## 🧠 4.3 Java Operators

Java supports many operators to **work with data**.

### 🔢 Arithmetic Operators:

```java
+   Addition  
-   Subtraction  
*   Multiplication  
/   Division  
%   Modulo (Remainder)
```

### 🔁 Example:

```java
int a = 10, b = 3;
System.out.println(a + b);  // 13
System.out.println(a / b);  // 3
System.out.println(a % b);  // 1
```

---

### 🔍 Comparison Operators:

Used in conditions:

```java
==  equal to  
!=  not equal to  
>   greater than  
<   less than  
>=  greater or equal  
<=  less or equal
```

---

### 🔀 Logical Operators:

Used with booleans

```java
&&  AND  
||  OR  
!   NOT
```

---

## 📝 Exercises

### ✅ Q1: Declare variables for:

* Your name
* Age
* Monthly income
* Is married (true/false)
* Gender (char)

Then print them all.

---

### ✅ Q2: Given:

```java
int x = 25;
int y = 4;
```

Print:

* Sum
* Difference
* Multiplication
* Remainder
* Is `x > y`?

---

### ✅ Q3: What happens when you divide an `int` by another `int`? Try:

```java
int result = 7 / 2;
System.out.println(result);  // ?
```

---

**Q1: Declare variables and print them**  
```java
public class Variables {
    public static void main(String[] args) {
        String name = "Tushar";
        int age = 25;
        double monthlyIncome = 5000.00;
        boolean isMarried = false;
        char gender = 'M';
        
        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
        System.out.println("Monthly Income: $" + monthlyIncome);
        System.out.println("Is Married: " + isMarried);
        System.out.println("Gender: " + gender);
    }
}
```

**Q2: Perform operations and print results**  
```java
public class Operations {
    public static void main(String[] args) {
        int x = 25;
        int y = 4;
        
        System.out.println("Sum: " + (x + y));          // 29
        System.out.println("Difference: " + (x - y));   // 21
        System.out.println("Multiplication: " + (x * y)); // 100
        System.out.println("Remainder: " + (x % y));    // 1
        System.out.println("Is x > y? " + (x > y));     // true
    }
}
```

**Q3: What happens when you divide an int by another int?**  
When you divide an `int` by another `int` in Java, the result is an `int`, and any fractional part is truncated (not rounded).  
For the given code:  
```java
int result = 7 / 2;
System.out.println(result); // Prints 3
```  
Explanation: 7 ÷ 2 = 3.5, but since both operands are `int`, the result is truncated to 3. To get a decimal result, at least one operand must be a `double` or `float` (e.g., `7.0 / 2` would give `3.5`).

Let me know if you want to proceed or need further clarification!
