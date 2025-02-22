## 🚀 **Module 9: Mastering Sorting Algorithms**  

Sorting algorithms are fundamental in computer science for organizing data. They optimize searching, enable efficient data processing, and are crucial in algorithms like binary search, dynamic programming, and graph algorithms. Let's explore the most important sorting algorithms, their implementations, and real-world applications.

---

## **🔥 9.1 What is Sorting?**  
- **Definition:** Rearranging elements in a list or array in a specific order (ascending or descending).  
- **Example:**  
    ```
    Input: [3, 1, 4, 1, 5, 9]
    Output (Ascending): [1, 1, 3, 4, 5, 9]
    Output (Descending): [9, 5, 4, 3, 1, 1]
    ```

---

## **🔥 9.2 Why Use Sorting Algorithms?**  
- **Improves Search Efficiency:** Sorted data allows faster searching (e.g., binary search).  
- **Data Organization:** Sorted data is easier to understand and analyze.  
- **Enables Efficient Algorithms:** Many algorithms (e.g., merge sort, quick sort) require sorted input.  

---

## **🔥 9.3 Types of Sorting Algorithms**  
1. **Comparison-Based Sorting:** Elements are compared to each other.  
    - Examples: Bubble Sort, Selection Sort, Insertion Sort, Merge Sort, Quick Sort, Heap Sort.  
2. **Non-Comparison Sorting:** Elements are sorted based on keys.  
    - Examples: Counting Sort, Radix Sort, Bucket Sort.  

---

## **🔥 9.4 Bubble Sort**  
- **Definition:** Repeatedly swaps adjacent elements if they are in the wrong order.  
- **Algorithm Type:** Comparison-based, In-place, Stable.  
- **Time Complexity:** `O(N²)` — Inefficient for large datasets.  
- **Space Complexity:** `O(1)` — Only uses a temporary variable for swapping.  

---

### 📘 **Example Code: Bubble Sort**  
```java
public class BubbleSort {
    public static void bubbleSort(int[] arr) {
        int n = arr.length;
        boolean swapped;
        for (int i = 0; i < n - 1; i++) {
            swapped = false;
            for (int j = 0; j < n - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    // Swap arr[j] and arr[j+1]
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    swapped = true;
                }
            }
            // If no two elements were swapped, the array is sorted
            if (!swapped) break;
        }
    }

    public static void main(String[] args) {
        int[] arr = {64, 34, 25, 12, 22, 11, 90};
        bubbleSort(arr);

        System.out.println("Sorted Array:");
        for (int num : arr) {
            System.out.print(num + " ");
        }
    }
}
```

---

### 📊 **Output:**  
```
Sorted Array:
11 12 22 25 34 64 90 
```

---

### 🔥 **Explanation:**  
- **Inner Loop:** Compares adjacent elements and swaps them if out of order.  
- **Outer Loop:** Repeats the process for all elements.  
- **Optimization:** If no swap occurs during an inner loop iteration, the array is already sorted.  

---

## **🔥 9.5 Selection Sort**  
- **Definition:** Selects the minimum element and swaps it with the current position.  
- **Algorithm Type:** Comparison-based, In-place, Not stable.  
- **Time Complexity:** `O(N²)` — Always makes the same number of comparisons.  
- **Space Complexity:** `O(1)`  

---

### 📘 **Example Code: Selection Sort**  
```java
public class SelectionSort {
    public static void selectionSort(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n - 1; i++) {
            int minIndex = i;
            for (int j = i + 1; j < n; j++) {
                if (arr[j] < arr[minIndex]) {
                    minIndex = j;
                }
            }
            // Swap the found minimum element with the current element
            int temp = arr[minIndex];
            arr[minIndex] = arr[i];
            arr[i] = temp;
        }
    }

    public static void main(String[] args) {
        int[] arr = {29, 10, 14, 37, 13};
        selectionSort(arr);

        System.out.println("Sorted Array:");
        for (int num : arr) {
            System.out.print(num + " ");
        }
    }
}
```

---

### 📊 **Output:**  
```
Sorted Array:
10 13 14 29 37 
```

---

### 🔥 **Explanation:**  
- **Inner Loop:** Finds the minimum element in the unsorted part.  
- **Outer Loop:** Swaps the minimum element with the first unsorted position.  
- **No Early Exit:** Always performs `N²` comparisons.  

---

## **🔥 9.6 Insertion Sort**  
- **Definition:** Picks an element and places it at the correct position among the previously sorted elements.  
- **Algorithm Type:** Comparison-based, In-place, Stable.  
- **Time Complexity:**  
    - Best Case: `O(N)` — Array is already sorted.  
    - Worst Case: `O(N²)` — Array is sorted in reverse order.  
- **Space Complexity:** `O(1)`  

---

### 📘 **Example Code: Insertion Sort**  
```java
public class InsertionSort {
    public static void insertionSort(int[] arr) {
        int n = arr.length;
        for (int i = 1; i < n; i++) {
            int key = arr[i];
            int j = i - 1;

            // Move elements greater than key to one position ahead
            while (j >= 0 && arr[j] > key) {
                arr[j + 1] = arr[j];
                j--;
            }
            arr[j + 1] = key;
        }
    }

    public static void main(String[] args) {
        int[] arr = {12, 11, 13, 5, 6};
        insertionSort(arr);

        System.out.println("Sorted Array:");
        for (int num : arr) {
            System.out.print(num + " ");
        }
    }
}
```

---

### 📊 **Output:**  
```
Sorted Array:
5 6 11 12 13 
```

---

### 🔥 **Explanation:**  
- **Inner Loop:** Shifts elements greater than the key to the right.  
- **Outer Loop:** Picks the next element and places it at its correct position.  
- **Efficient for Small Arrays:** Insertion Sort is fast for small or nearly sorted arrays.  

---

## **🔥 9.7 When to Use Which Sorting Algorithm?**  
- **Bubble Sort:** Educational purposes. Rarely used in practice due to poor performance.  
- **Selection Sort:** Small datasets where memory usage is a concern.  
- **Insertion Sort:** Small or nearly sorted arrays. Efficient for incremental sorting.  
- **Merge Sort:** Stable and efficient for large datasets (`O(N log N)` complexity).  
- **Quick Sort:** Faster in practice for large datasets, but not stable.  
- **Heap Sort:** In-place sorting with `O(N log N)` complexity.  

---

## **📝 Exercise Set:**  
1. Implement Merge Sort and analyze its time complexity.  
2. Write Quick Sort and understand the importance of pivot selection.  
3. Implement Heap Sort using a Max-Heap.  
4. Write a program to find the `k-th` smallest element using Quick Sort partition.  
5. Implement Counting Sort for non-negative integers.  

---

## 🔥 **Next: Searching Algorithms**  
Sorting is often followed by **Searching Algorithms**. Next, we will learn about searching techniques like Linear Search, Binary Search, and more advanced searching methods.

---
