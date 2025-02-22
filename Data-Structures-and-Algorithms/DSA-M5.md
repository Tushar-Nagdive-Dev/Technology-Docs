## 🚀 **Module 2: Mastering Arrays and Strings**  

Arrays and Strings are the building blocks of data structures. They are used in almost every algorithm, making them essential for mastering Data Structures and Algorithms (DSA). Let's dive into the core concepts, practical examples, and real-world applications.

---

## **🔥 2.1 What are Arrays?**  
- **Definition:** An array is a collection of elements of the same type stored in contiguous memory locations.  
- **Example:** `[10, 20, 30, 40, 50]` — An array of integers.  

---

## **🔥 2.2 Why Use Arrays?**  
- **Fast Access:** Constant time complexity (`O(1)`) for accessing elements using an index.  
- **Easy Iteration:** Elements are stored consecutively, making loops efficient.  
- **Fixed Size:** In Java, arrays have a fixed size, which cannot be changed once defined.  

---

## **🔥 2.3 Declaring and Initializing Arrays**  
```java
// Declaration and Initialization
int[] numbers = new int[5];  // Array of size 5 with default values (0)
String[] names = {"John", "Jane", "Alex"};  // Array with initial values

// Another way to declare and initialize
int[] scores = {85, 90, 78, 92, 88};
```

- **Default Values:**  
    - `int[]` → 0  
    - `double[]` → 0.0  
    - `boolean[]` → false  
    - `String[]` → null  

---

## **🔥 2.4 Accessing and Modifying Elements**  
```java
// Accessing elements
int firstScore = scores[0];  // 85
int lastScore = scores[4];   // 88

// Modifying elements
scores[2] = 95;  // Changing the third element from 78 to 95
```

- **Indexing:** Arrays are zero-indexed, meaning the first element is at index `0`.  
- **Out of Bounds:** Accessing an index outside the array size will throw `ArrayIndexOutOfBoundsException`.  

---

## **🔥 2.5 Looping through Arrays**  
### 📘 **Using for Loop**  
```java
for (int i = 0; i < scores.length; i++) {
    System.out.println(scores[i]);
}
```

### 📘 **Using Enhanced for Loop (for-each)**  
```java
for (int score : scores) {
    System.out.println(score);
}
```

- **Difference:**  
    - `for` loop provides access to the index.  
    - `for-each` loop is simpler but doesn't give access to the index.  

---

## **🔥 2.6 Example 1: Array Operations**  
Let's see an example where we:  
1. Create an array of integers.  
2. Find the sum and average of the array elements.  
3. Find the maximum and minimum elements.  

### 📘 **Code: ArrayOperations.java**  
```java
public class ArrayOperations {
    public static void main(String[] args) {
        // Array of integers
        int[] numbers = {10, 20, 30, 40, 50};

        // 1. Sum and Average
        int sum = 0;
        for (int num : numbers) {
            sum += num;
        }
        double average = (double) sum / numbers.length;
        System.out.println("Sum: " + sum);
        System.out.println("Average: " + average);

        // 2. Maximum and Minimum
        int max = numbers[0];
        int min = numbers[0];
        for (int num : numbers) {
            if (num > max) max = num;
            if (num < min) min = num;
        }
        System.out.println("Maximum: " + max);
        System.out.println("Minimum: " + min);
    }
}
```

---

### 📊 **Output:**  
```
Sum: 150
Average: 30.0
Maximum: 50
Minimum: 10
```

---

### 🔥 **Explanation:**  
- We loop through the array once to calculate the sum.  
- We find the maximum and minimum using simple comparisons.  
- This approach has a time complexity of `O(N)`, where `N` is the size of the array.  

---

## **🔥 2.7 Array Manipulation Techniques**  

### 📘 **1. Reversing an Array**  
```java
public static void reverseArray(int[] arr) {
    int left = 0;
    int right = arr.length - 1;

    while (left < right) {
        // Swap elements
        int temp = arr[left];
        arr[left] = arr[right];
        arr[right] = temp;
        
        // Move pointers
        left++;
        right--;
    }
}
```

---

### 📘 **2. Rotating an Array**  
Rotate the array `k` times to the right.  
```java
public static void rotateArray(int[] arr, int k) {
    k = k % arr.length;
    reverse(arr, 0, arr.length - 1);
    reverse(arr, 0, k - 1);
    reverse(arr, k, arr.length - 1);
}

public static void reverse(int[] arr, int start, int end) {
    while (start < end) {
        int temp = arr[start];
        arr[start] = arr[end];
        arr[end] = temp;
        start++;
        end--;
    }
}
```

---

### 📘 **3. Searching in an Array**  
- **Linear Search:** `O(N)` — Check each element one by one.  
- **Binary Search:** `O(log N)` — Works only on sorted arrays.

---

## **🔥 2.8 Common Mistakes to Avoid**  
- **Array Index Out of Bounds:** Make sure index is within `0` to `array.length - 1`.  
- **Misusing for-each Loop:** Use it when you don’t need the index.  
- **Forgetting to Check Array Length:** Always check if the array is not empty before accessing elements.  

---

## **📝 Exercise Set:**  
1. Write a program to find the second largest element in an array.  
2. Implement a method to check if an array is sorted.  
3. Write a function to find all pairs in an array that sum up to a given number.  
4. Implement an array rotation method without using extra space.  
5. Write a function to merge two sorted arrays into a single sorted array.  

---

## **🔥 Next: Strings**  
After mastering Arrays, the next step is to learn **Strings**. Strings are arrays of characters, but they have unique properties and methods in Java.

### 📘 **In the Next Section, We Will Cover:**  
1. What are Strings and how are they stored in memory?  
2. Common String operations like concatenation, substring, and comparison.  
3. String manipulation techniques and interview questions.  
