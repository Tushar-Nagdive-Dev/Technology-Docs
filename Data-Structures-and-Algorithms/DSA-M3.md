## 🚀 **1.5 Introduction to Big O Notation**  

Big O Notation is crucial for analyzing the efficiency of algorithms. It helps us understand how the runtime or space requirements of an algorithm grow as the input size increases.

---

## **🔥 1.5.1 What is Big O Notation?**  
- **Definition:** Big O Notation describes the upper bound of an algorithm's time or space complexity.  
- **Purpose:** To measure the performance and scalability of an algorithm.  

---

## **🔥 1.5.2 Why Use Big O Notation?**  
- It provides a high-level understanding of the algorithm’s efficiency.  
- Helps in comparing different algorithms to find the most optimal one.  
- Essential for technical interviews and performance-critical applications.  

---

## **🔥 1.5.3 Types of Complexities**  
1. **Time Complexity:** Amount of time taken by an algorithm to run, as a function of the input size.  
2. **Space Complexity:** Amount of memory used by an algorithm, including input size.  

---

## **🔥 1.5.4 Common Big O Notations**  

| Notation | Name               | Description                                | Example                              |
|----------|--------------------|--------------------------------------------|---------------------------------------|
| **O(1)** | Constant Time       | Time doesn't change with input size         | Accessing an array element             |
| **O(N)** | Linear Time         | Time grows linearly with input size          | Loop through an array                  |
| **O(N²)**| Quadratic Time      | Time grows quadratically with input size     | Nested loops                           |
| **O(log N)** | Logarithmic Time| Time grows logarithmically                  | Binary Search                          |
| **O(N log N)**| Linearithmic   | Time grows linearly and logarithmically     | Merge Sort, Quick Sort (average case)   |
| **O(2^N)** | Exponential Time  | Time doubles with each input addition       | Recursive Fibonacci                    |
| **O(N!)** | Factorial Time     | Time grows factorially with input size      | Recursive Permutations                 |

---

## **🔥 1.5.5 Examples of Common Complexities**  

### 📘 **1. Constant Time Complexity - O(1)**  
- Example: Accessing a specific element in an array.  
- **Explanation:** No matter the size of the array, accessing an element takes the same time.  

```java
public class ConstantTimeExample {
    public static void main(String[] args) {
        int[] numbers = {10, 20, 30, 40, 50};
        // Accessing the third element (Index 2)
        System.out.println("Third Element: " + numbers[2]);
    }
}
```

- **Time Complexity:** O(1)  

---

### 📘 **2. Linear Time Complexity - O(N)**  
- Example: Looping through an array.  
- **Explanation:** Time taken grows linearly with the size of the input.  

```java
public class LinearTimeExample {
    public static void main(String[] args) {
        int[] numbers = {10, 20, 30, 40, 50};
        System.out.println("Array Elements:");
        for (int num : numbers) {
            System.out.print(num + " ");
        }
    }
}
```

- **Time Complexity:** O(N)  

---

### 📘 **3. Quadratic Time Complexity - O(N²)**  
- Example: Nested loops over an array.  
- **Explanation:** Time grows quadratically due to the nested loop.  

```java
public class QuadraticTimeExample {
    public static void main(String[] args) {
        int[] numbers = {1, 2, 3, 4, 5};
        System.out.println("Pairs:");
        for (int i = 0; i < numbers.length; i++) {
            for (int j = 0; j < numbers.length; j++) {
                System.out.println(numbers[i] + ", " + numbers[j]);
            }
        }
    }
}
```

- **Time Complexity:** O(N²)  

---

### 📘 **4. Logarithmic Time Complexity - O(log N)**  
- Example: Binary Search.  
- **Explanation:** Input size is halved with each step.  

```java
public class LogarithmicTimeExample {
    public static int binarySearch(int[] arr, int target) {
        int left = 0;
        int right = arr.length - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (arr[mid] == target) return mid;
            else if (arr[mid] < target) left = mid + 1;
            else right = mid - 1;
        }
        return -1; // Target not found
    }

    public static void main(String[] args) {
        int[] sortedArray = {2, 4, 6, 8, 10, 12, 14};
        int target = 10;
        int index = binarySearch(sortedArray, target);
        System.out.println("Index of " + target + ": " + index);
    }
}
```

- **Time Complexity:** O(log N)  

---

### 📘 **5. Linearithmic Time Complexity - O(N log N)**  
- Example: Merge Sort or Quick Sort (average case).  
- **Explanation:** Divides the problem in half (log N) and processes each division (N).  

```java
// Pseudo-code for Merge Sort
public class MergeSortExample {
    public static void mergeSort(int[] arr, int left, int right) {
        if (left < right) {
            int mid = (left + right) / 2;
            mergeSort(arr, left, mid);
            mergeSort(arr, mid + 1, right);
            merge(arr, left, mid, right);
        }
    }
}
```

- **Time Complexity:** O(N log N)  

---

## **🔥 1.5.6 Why Ignore Constants and Lower Order Terms?**  
- Big O focuses on the growth rate, not the exact number of operations.  
- Constants become negligible as input size grows.  
- Example:  
    - `O(2N + 10)` becomes `O(N)`  
    - `O(N² + N)` becomes `O(N²)`  

---

## **🔥 1.5.7 Space Complexity**  
- Measures the amount of memory an algorithm uses.  
- **Example:** Storing an array of size N requires O(N) space.  

---

## **📌 Common Mistakes to Avoid**  
- Confusing constant time O(1) with linear time O(N).  
- Misjudging nested loops as O(N) instead of O(N²).  
- Forgetting about space complexity in recursive algorithms.  

---

## **📝 Exercise Set:**  
1. Write a program to find the minimum element in an array and analyze its time complexity.  
2. Implement a Binary Search algorithm and analyze its time complexity.  
3. Create a nested loop that prints all pairs in an array and analyze the time complexity.  
4. Write a program to check for duplicates in an array and analyze its time complexity.  

---
---
## Solutions
Great job! Your implementation is clean, efficient, and well-structured. Let's review each solution with insights, optimizations, and expert tips to enhance your understanding.

---

## **🔍 Code Review and Insights**  

---

### 🔥 **1. Find Minimum Element in an Array**  
```java
public static int findMin(int[] arr) {
    if (arr == null || arr.length == 0) {
        throw new IllegalArgumentException("Array cannot be null or empty");
    }
    
    int min = arr[0];
    for (int i = 1; i < arr.length; i++) {
        if (arr[i] < min) {
            min = arr[i];
        }
    }
    return min;
}
```

- **Time Complexity:** `O(N)` — Linear time, as the loop runs once for each element.  
- **Space Complexity:** `O(1)` — Constant space, as only one variable (`min`) is used.  

### 💡 **Insights and Optimization:**  
- This is the most efficient approach as we need to examine each element at least once.  
- Java 8+ offers a more readable solution using `Arrays.stream()`:
    ```java
    return Arrays.stream(arr).min().orElseThrow(() -> new IllegalArgumentException("Array cannot be null or empty"));
    ```
  This achieves the same time and space complexity but improves readability.

---

### 🔥 **2. Binary Search Implementation**  
```java
public static int binarySearch(int[] arr, int target) {
    if (arr == null || arr.length == 0) {
        return -1;
    }
    
    int left = 0;
    int right = arr.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2; // Prevents integer overflow
        
        if (arr[mid] == target) {
            return mid;
        }
        
        if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1;
}
```

- **Time Complexity:** `O(log N)` — Logarithmic time, as the array is halved at each step.  
- **Space Complexity:** `O(1)` — Constant space, as no additional data structures are used.  
- **Prerequisite:** The array must be sorted.  

### 💡 **Insights and Optimization:**  
- This is the most efficient solution for searching in a sorted array.  
- **Recursive Approach:** You can implement this using recursion, which has the same time complexity but uses more space due to the call stack:
    ```java
    public static int recursiveBinarySearch(int[] arr, int target, int left, int right) {
        if (left > right) return -1;
        
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) return recursiveBinarySearch(arr, target, mid + 1, right);
        else return recursiveBinarySearch(arr, target, left, mid - 1);
    }
    ```

---

### 🔥 **3. Print All Pairs in an Array**  
```java
public static void printAllPairs(int[] arr) {
    if (arr == null || arr.length < 2) {
        System.out.println("Array must have at least 2 elements");
        return;
    }
    
    for (int i = 0; i < arr.length; i++) {
        for (int j = 0; j < arr.length; j++) {
            System.out.println("(" + arr[i] + ", " + arr[j] + ")");
        }
    }
}
```

- **Time Complexity:** `O(N²)` — Quadratic time, due to the nested loops.  
- **Space Complexity:** `O(1)` — Constant space, as no additional data structures are used.  

### 💡 **Insights and Optimization:**  
- This approach prints pairs, including pairs of the same element (`(i, i)`).  
- **Optimization:** To avoid repeating pairs in reverse order (`(a, b)` and `(b, a)`), modify the inner loop to start from `i + 1`:
    ```java
    for (int i = 0; i < arr.length; i++) {
        for (int j = i + 1; j < arr.length; j++) {
            System.out.println("(" + arr[i] + ", " + arr[j] + ")");
        }
    }
    ```
  - This reduces the number of iterations and removes duplicate pairs.

---

### 🔥 **4. Check for Duplicates in an Array**  
```java
public static boolean hasDuplicates(int[] arr) {
    if (arr == null || arr.length <= 1) {
        return false;
    }
    
    for (int i = 0; i < arr.length - 1; i++) {
        for (int j = i + 1; j < arr.length; j++) {
            if (arr[i] == arr[j]) {
                return true;
            }
        }
    }
    return false;
}
```

- **Time Complexity:** `O(N²)` — Quadratic time, due to the nested loops.  
- **Space Complexity:** `O(1)` — Constant space, as no additional data structures are used.  

### 💡 **Insights and Optimization:**  
- This approach compares every pair, which is inefficient for large arrays.  
- **Optimization Using HashSet:** By leveraging a `HashSet`, we can reduce the time complexity to `O(N)`:
    ```java
    public static boolean hasDuplicates(int[] arr) {
        Set<Integer> set = new HashSet<>();
        for (int num : arr) {
            if (set.contains(num)) {
                return true;
            }
            set.add(num);
        }
        return false;
    }
    ```
  - `HashSet` provides `O(1)` complexity for insertions and lookups.  
  - **Space Complexity:** `O(N)` due to the additional `HashSet`.

---

## **📌 Common Mistakes to Avoid**  
- Using nested loops unnecessarily when more efficient solutions exist.  
- Not handling edge cases (e.g., null or empty arrays).  
- Forgetting to check if an array is sorted before performing a binary search.  

---

## **📝 Exercise Set:**  
1. Implement a function to find the maximum difference between two elements such that the larger element comes after the smaller one.  
2. Write a program to reverse an array in place and analyze its time and space complexity.  
3. Implement a method to rotate an array by `k` positions to the right.  
4. Given a sorted array, implement a two-pointer approach to find two numbers that sum up to a given target.  

---
