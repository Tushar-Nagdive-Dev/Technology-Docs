## 🚀 **Module 10: Mastering Searching Algorithms**  

Searching algorithms are fundamental in computer science for finding specific elements in a data structure. Efficient searching techniques improve performance in databases, file systems, and numerous algorithms. This module covers essential searching algorithms, their implementations, and real-world applications.

---

## **🔥 10.1 What is Searching?**  
- **Definition:** The process of finding the position of a target element within a data structure (e.g., array, linked list, tree).  
- **Example:**  
    ```
    Input: Array = [10, 20, 30, 40, 50], Target = 30
    Output: Index = 2
    ```

---

## **🔥 10.2 Why Use Searching Algorithms?**  
- To **locate data** efficiently in large datasets.  
- To **optimize time complexity** in data retrieval operations.  
- To **enable advanced algorithms** like binary search and interpolation search.  

---

## **🔥 10.3 Types of Searching Algorithms**  
1. **Linear Search:** Sequentially checks each element.  
2. **Binary Search:** Efficiently searches in a sorted array using a divide-and-conquer approach.  
3. **Jump Search:** Jumps ahead by fixed steps and performs linear search within the block.  
4. **Interpolation Search:** Improves binary search by predicting the position using the value of the target.  
5. **Exponential Search:** Searches in a sorted array by doubling the index range and using binary search.  

---

## **🔥 10.4 Linear Search**  
- **Definition:** Sequentially checks each element until the target is found or the end of the list is reached.  
- **Algorithm Type:** Iterative, Unsorted Array, In-place.  
- **Time Complexity:** `O(N)` — Checks each element.  
- **Space Complexity:** `O(1)` — No additional space required.  

---

### 📘 **Example Code: Linear Search**  
```java
public class LinearSearch {
    public static int linearSearch(int[] arr, int target) {
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == target) {
                return i;  // Target found at index i
            }
        }
        return -1;  // Target not found
    }

    public static void main(String[] args) {
        int[] arr = {10, 20, 30, 40, 50};
        int target = 30;
        int result = linearSearch(arr, target);

        if (result != -1) {
            System.out.println("Element found at index: " + result);
        } else {
            System.out.println("Element not found");
        }
    }
}
```

---

### 📊 **Output:**  
```
Element found at index: 2
```

---

### 🔥 **Explanation:**  
- **For Loop:** Iterates through each element in the array.  
- **Condition Check:** Compares each element with the target.  
- **Returns Index:** If the element is found.  
- **Returns -1:** If the element is not found in the array.  

---

## **🔥 10.5 Binary Search**  
- **Definition:** Searches in a sorted array by repeatedly dividing the search interval in half.  
- **Algorithm Type:** Divide and Conquer, Sorted Array, In-place.  
- **Time Complexity:** `O(log N)` — Halves the search space each time.  
- **Space Complexity:** `O(1)` for iterative, `O(log N)` for recursive due to call stack.  

---

### 📘 **Binary Search Conditions**  
- **Array must be sorted.**  
- **Divides search space** into two halves and selects the half containing the target.  

---

### 📘 **Example Code: Binary Search (Iterative)**  
```java
public class BinarySearch {
    public static int binarySearch(int[] arr, int target) {
        int left = 0;
        int right = arr.length - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            // Check if target is at mid
            if (arr[mid] == target) {
                return mid;
            }
            // If target is greater, ignore left half
            if (arr[mid] < target) {
                left = mid + 1;
            }
            // If target is smaller, ignore right half
            else {
                right = mid - 1;
            }
        }
        return -1;  // Target not found
    }

    public static void main(String[] args) {
        int[] arr = {10, 20, 30, 40, 50};
        int target = 30;
        int result = binarySearch(arr, target);

        if (result != -1) {
            System.out.println("Element found at index: " + result);
        } else {
            System.out.println("Element not found");
        }
    }
}
```

---

### 📊 **Output:**  
```
Element found at index: 2
```

---

### 🔥 **Explanation:**  
- **Middle Element:** Calculated as `mid = left + (right - left) / 2`.  
- **Comparison:** Checks if the target is less than, greater than, or equal to the middle element.  
- **Adjusts Search Space:**  
    - `left = mid + 1` if the target is greater.  
    - `right = mid - 1` if the target is smaller.  
- **Efficient:** Reduces the search space by half each time.  

---

## **🔥 10.6 Jump Search**  
- **Definition:** Jumps ahead by fixed steps and performs linear search within the block.  
- **Algorithm Type:** Block Search, Sorted Array.  
- **Time Complexity:** `O(√N)` — Optimal jump step is √N.  
- **Space Complexity:** `O(1)`  

---

### 📘 **Example Code: Jump Search**  
```java
import java.util.Arrays;

public class JumpSearch {
    public static int jumpSearch(int[] arr, int target) {
        int n = arr.length;
        int step = (int) Math.floor(Math.sqrt(n));
        int prev = 0;

        while (arr[Math.min(step, n) - 1] < target) {
            prev = step;
            step += Math.floor(Math.sqrt(n));
            if (prev >= n) return -1;
        }

        // Linear search within the block
        while (arr[prev] < target) {
            prev++;
            if (prev == Math.min(step, n)) return -1;
        }
        if (arr[prev] == target) return prev;
        return -1;
    }

    public static void main(String[] args) {
        int[] arr = {0, 1, 4, 9, 16, 25, 36, 49, 64, 81};
        int target = 36;
        int result = jumpSearch(arr, target);

        if (result != -1) {
            System.out.println("Element found at index: " + result);
        } else {
            System.out.println("Element not found");
        }
    }
}
```

---

### 📊 **Output:**  
```
Element found at index: 6
```

---

### 🔥 **Explanation:**  
- **Jump Step:** Calculated as `√N`.  
- **Jumping Phase:** Skips ahead by fixed steps until the block containing the target is found.  
- **Linear Search:** Performed within the identified block.  
- **Efficient for Large Arrays:** Reduces the number of comparisons compared to linear search.  

---

## **🔥 10.7 When to Use Which Searching Algorithm?**  
- **Linear Search:** Unsorted arrays or small datasets.  
- **Binary Search:** Sorted arrays for fast and efficient searching.  
- **Jump Search:** Sorted arrays with large datasets.  
- **Interpolation Search:** Arrays with uniformly distributed elements.  
- **Exponential Search:** Unbounded or infinite-sized sorted arrays.  

---

## 🔥 **Next: Hashing and Hash Tables**  
Searching is optimized using **Hashing and Hash Tables**, which provide constant time complexity for search operations. Next, we will learn about Hash Functions, Collision Handling, and Hash Table implementations.

---
