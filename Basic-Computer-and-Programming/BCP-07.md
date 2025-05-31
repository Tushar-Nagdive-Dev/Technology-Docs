
## 📘 MODULE 6: Arrays & Collections in Java

---

## 🧠 6.1 What is an Array?

An **array** is a **container** that stores multiple values of the **same data type** at **fixed size** and **fixed positions (index-based)**.

### ✅ Syntax:

```java
int[] numbers = new int[5]; // array of size 5
```

You can also declare & initialize directly:

```java
int[] numbers = {10, 20, 30, 40, 50};
```

> Index starts from `0`
> So `numbers[0]` is `10`, `numbers[4]` is `50`.

---

## 🧠 6.2 Accessing and Modifying Array Elements

```java
System.out.println(numbers[2]); // prints 30
numbers[2] = 35;                // change value at index 2
```

---

## 🧠 6.3 Looping Through Arrays

```java
for (int i = 0; i < numbers.length; i++) {
    System.out.println(numbers[i]);
}
```

OR

```java
for (int num : numbers) {
    System.out.println(num);
}
```

---

## 🧠 6.4 2D Arrays (Matrix)

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6}
};
System.out.println(matrix[1][2]); // prints 6
```

---

## 🧠 6.5 Common Mistakes with Arrays

| Mistake                                        | Explanation                                  |
| ---------------------------------------------- | -------------------------------------------- |
| Accessing invalid index                        | `numbers[5]` when size is 5 → Error          |
| Mixing data types                              | Arrays are strongly typed                    |
| Forgetting `.length` is a property, not method | Use `numbers.length`, not `numbers.length()` |

---

## 🧠 6.6 Introduction to Collections (Preview)

Java has a powerful **Collections Framework** which includes:

| Type        | Description                         |
| ----------- | ----------------------------------- |
| `ArrayList` | Resizable array                     |
| `HashSet`   | No duplicates                       |
| `HashMap`   | Key-value pairs (like a dictionary) |

👉 We’ll go deep into collections later. For now, focus on **arrays**.

---

## 📝 Exercises

### ✅ Q1: Create an array of 5 integers and print them using a loop.

---

### ✅ Q2: Create a `String[]` array with names of 3 cities. Print each one with `"Welcome to <city>"`.

---

### ✅ Q3: Find the **sum of all elements** in an integer array using a `for` loop.

---

### ✅ Q4: Create a 2D array representing a 3x3 matrix and print it as a grid.

---

### ✅ Q5: What happens when you try to access an index outside the array’s size?

---

```java
public class ArrayExercises {
    public static void main(String[] args) {
        // Q1: Create an array of 5 integers and print them using a loop
        int[] numbers = {10, 20, 30, 40, 50};
        System.out.println("Q1: Printing array of 5 integers:");
        for (int i = 0; i < numbers.length; i++) {
            System.out.println(numbers[i]);
        }

        // Q2: Create a String[] array with 3 cities and print with "Welcome to <city>"
        String[] cities = {"Paris", "Tokyo", "New York"};
        System.out.println("\nQ2: Printing welcome messages for cities:");
        for (String city : cities) {
            System.out.println("Welcome to " + city);
        }

        // Q3: Find the sum of all elements in an integer array using a for loop
        int[] numbersForSum = {1, 2, 3, 4, 5};
        int sum = 0;
        for (int num : numbersForSum) {
            sum += num;
        }
        System.out.println("\nQ3: Sum of array elements: " + sum);

        // Q4: Create a 2D array for a 3x3 matrix and print it as a grid
        int[][] matrix = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };
        System.out.println("\nQ4: Printing 3x3 matrix:");
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[i].length; j++) {
                System.out.print(matrix[i][j] + " ");
            }
            System.out.println(); // New line after each row
        }
    }
}
```

**Q5: What happens when you try to access an index outside the array’s size?**  
When you try to access an index outside the array’s size in Java, it throws an `ArrayIndexOutOfBoundsException`. For example, if an array has 5 elements (indices 0 to 4), attempting to access index 5 or -1 will cause this runtime exception, halting the program unless handled with try-catch. This happens because Java checks array bounds to ensure safe access.
