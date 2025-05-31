## 📘 MODULE 5: Control Flow in Java

*(if, else, switch, while, for, do-while)*

---

## 🧠 5.1 The `if` Statement – Make Decisions

### ✅ Syntax:

```java
if (condition) {
    // Code runs only if condition is true
}
```

### 🧪 Example:

```java
int age = 20;
if (age >= 18) {
    System.out.println("You are eligible to vote.");
}
```

---

## 🧠 5.2 The `if-else` and `else if` Ladder

```java
if (marks >= 90) {
    System.out.println("Grade A");
} else if (marks >= 75) {
    System.out.println("Grade B");
} else {
    System.out.println("Grade C");
}
```

---

## 🧠 5.3 The `switch` Statement

Used when you have **multiple exact values** to check.

### ✅ Example:

```java
int day = 2;
switch (day) {
    case 1: System.out.println("Monday"); break;
    case 2: System.out.println("Tuesday"); break;
    case 3: System.out.println("Wednesday"); break;
    default: System.out.println("Invalid day");
}
```

> 🔥 Best for **menus, options, enums, constant choices**

---

## 🧠 5.4 Loops – Run Code Repeatedly

### 🔁 `while` loop

```java
int i = 1;
while (i <= 5) {
    System.out.println(i);
    i++;
}
```

### 🔁 `do-while` loop

```java
int i = 1;
do {
    System.out.println(i);
    i++;
} while (i <= 5);
```

### 🔁 `for` loop

```java
for (int i = 1; i <= 5; i++) {
    System.out.println(i);
}
```

---

## 🧠 5.5 `break` and `continue`

* `break`: exit the loop
* `continue`: skip current iteration

```java
for (int i = 1; i <= 5; i++) {
    if (i == 3) continue;
    System.out.println(i);  // 1, 2, 4, 5
}
```

---

## 📝 Exercises

### ✅ Q1: Write a program that checks if a number is:

* Positive
* Negative
* Or zero

---

### ✅ Q2: Using `if-else`, print:

```
"Pass" if marks >= 40  
"Fail" otherwise
```

---

### ✅ Q3: Using a `for` loop, print:

```
Numbers from 1 to 10 and their squares.
```

---

### ✅ Q4: Write a `switch` program that takes a digit (0–6) and prints the weekday.

---

### ✅ Q5: Write a loop that sums numbers from 1 to 100.

---

```java
public class NumberChecks {
    public static void main(String[] args) {
        // Q1: Check if a number is positive, negative, or zero
        int number = 42; // Example number, can be changed
        if (number > 0) {
            System.out.println(number + " is positive");
        } else if (number < 0) {
            System.out.println(number + " is negative");
        } else {
            System.out.println(number + " is zero");
        }

        // Q2: Check if marks >= 40 for Pass/Fail
        int marks = 75; // Example marks, can be changed
        if (marks >= 40) {
            System.out.println("Pass");
        } else {
            System.out.println("Fail");
        }

        // Q3: Print numbers 1 to 10 and their squares
        for (int i = 1; i <= 10; i++) {
            System.out.println("Number: " + i + ", Square: " + (i * i));
        }

        // Q4: Switch program for weekday based on digit 0-6
        int day = 3; // Example digit (0=Sunday, 1=Monday, ..., 6=Saturday)
        switch (day) {
            case 0:
                System.out.println("Sunday");
                break;
            case 1:
                System.out.println("Monday");
                break;
            case 2:
                System.out.println("Tuesday");
                break;
            case 3:
                System.out.println("Wednesday");
                break;
            case 4:
                System.out.println("Thursday");
                break;
            case 5:
                System.out.println("Friday");
                break;
            case 6:
                System.out.println("Saturday");
                break;
            default:
                System.out.println("Invalid day");
                break;
        }

        // Q5: Sum numbers from 1 to 100
        int sum = 0;
        for (int i = 1; i <= 100; i++) {
            sum += i;
        }
        System.out.println("Sum of numbers from 1 to 100: " + sum);
    }
}
```
