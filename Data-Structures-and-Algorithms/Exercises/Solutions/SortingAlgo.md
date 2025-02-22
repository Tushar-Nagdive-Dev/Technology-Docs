Below is a Java program implementing five sorting-related algorithms with clear explanations, time complexity analysis, and well-documented code. We'll create a `SortingAlgorithms` class containing all methods and a `main` method to test them.

```java
import java.util.*;

public class SortingAlgorithms {
    // 1. Merge Sort
    /**
     * Sorts array by dividing into halves, sorting, and merging.
     * Time Complexity: O(n log n) - always, stable sort.
     * Space Complexity: O(n) for temporary arrays.
     */
    public static void mergeSort(int[] arr, int left, int right) {
        if (left < right) {
            int mid = left + (right - left) / 2; // Avoid overflow
            mergeSort(arr, left, mid);          // Sort left half
            mergeSort(arr, mid + 1, right);     // Sort right half
            merge(arr, left, mid, right);       // Merge sorted halves
        }
    }
    
    private static void merge(int[] arr, int left, int mid, int right) {
        int n1 = mid - left + 1;
        int n2 = right - mid;
        int[] leftArr = new int[n1];
        int[] rightArr = new int[n2];
        
        // Copy data to temp arrays
        for (int i = 0; i < n1; i++) leftArr[i] = arr[left + i];
        for (int j = 0; j < n2; j++) rightArr[j] = arr[mid + 1 + j];
        
        // Merge back to original array
        int i = 0, j = 0, k = left;
        while (i < n1 && j < n2) {
            if (leftArr[i] <= rightArr[j]) arr[k++] = leftArr[i++];
            else arr[k++] = rightArr[j++];
        }
        while (i < n1) arr[k++] = leftArr[i++]; // Remaining left
        while (j < n2) arr[k++] = rightArr[j++]; // Remaining right
    }
    
    // 2. Quick Sort
    /**
     * Sorts array by partitioning around a pivot, recursive on subarrays.
     * Time Complexity: O(n log n) avg, O(n^2) worst (bad pivot), unstable.
     * Space Complexity: O(log n) avg for recursion stack.
     * Pivot choice: Here, we use last element; median-of-three or random improves avg case.
     */
    public static void quickSort(int[] arr, int low, int high) {
        if (low < high) {
            int pi = partition(arr, low, high); // Partition index
            quickSort(arr, low, pi - 1);       // Sort left of pivot
            quickSort(arr, pi + 1, high);      // Sort right of pivot
        }
    }
    
    private static int partition(int[] arr, int low, int high) {
        int pivot = arr[high]; // Last element as pivot
        int i = low - 1;       // Index of smaller element
        
        for (int j = low; j < high; j++) {
            if (arr[j] <= pivot) {
                i++;
                swap(arr, i, j);
            }
        }
        swap(arr, i + 1, high); // Place pivot in correct position
        return i + 1;
    }
    
    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
    
    // 3. Heap Sort using Max-Heap
    /**
     * Sorts array by building max-heap, extracting max repeatedly.
     * Time Complexity: O(n log n), Space Complexity: O(1), not stable.
     */
    public static void heapSort(int[] arr) {
        int n = arr.length;
        
        // Build max-heap
        for (int i = n / 2 - 1; i >= 0; i--) {
            heapify(arr, n, i);
        }
        
        // Extract max and heapify
        for (int i = n - 1; i > 0; i--) {
            swap(arr, 0, i);     // Move max to end
            heapify(arr, i, 0);  // Heapify reduced heap
        }
    }
    
    private static void heapify(int[] arr, int n, int i) {
        int largest = i;
        int left = 2 * i + 1;
        int right = 2 * i + 2;
        
        if (left < n && arr[left] > arr[largest]) largest = left;
        if (right < n && arr[right] > arr[largest]) largest = right;
        
        if (largest != i) {
            swap(arr, i, largest);
            heapify(arr, n, largest); // Recursively heapify subtree
        }
    }
    
    // 4. Find k-th Smallest Element using Quick Sort Partition
    /**
     * Finds k-th smallest element (1-based index) using partitioning.
     * Time Complexity: O(n) avg, O(n^2) worst, Space Complexity: O(1).
     */
    public static int findKthSmallest(int[] arr, int k) {
        if (k < 1 || k > arr.length) return -1;
        return quickSelect(arr, 0, arr.length - 1, k - 1); // Convert to 0-based
    }
    
    private static int quickSelect(int[] arr, int low, int high, int k) {
        if (low == high) return arr[low];
        
        int pivotIndex = partition(arr, low, high);
        if (k == pivotIndex) return arr[k];
        else if (k < pivotIndex) return quickSelect(arr, low, pivotIndex - 1, k);
        else return quickSelect(arr, pivotIndex + 1, high, k);
    }
    
    // 5. Counting Sort for Non-Negative Integers
    /**
     * Sorts array using counting of occurrences, works for non-negative integers.
     * Time Complexity: O(n + k) where k is range, Space Complexity: O(n + k), stable.
     */
    public static void countingSort(int[] arr) {
        int max = Arrays.stream(arr).max().getAsInt();
        int[] count = new int[max + 1];
        int[] output = new int[arr.length];
        
        // Count occurrences
        for (int num : arr) count[num]++;
        // Cumulative count
        for (int i = 1; i <= max; i++) count[i] += count[i - 1];
        
        // Build sorted array
        for (int i = arr.length - 1; i >= 0; i--) {
            output[count[arr[i]] - 1] = arr[i];
            count[arr[i]]--;
        }
        
        // Copy back to original array
        System.arraycopy(output, 0, arr, 0, arr.length);
    }
    
    // Main method to test all functions
    public static void main(String[] args) {
        // Test 1: Merge Sort
        int[] arr1 = {64, 34, 25, 12, 22, 11, 90};
        mergeSort(arr1, 0, arr1.length - 1);
        System.out.println("Merge Sort: " + Arrays.toString(arr1));
        
        // Test 2: Quick Sort
        int[] arr2 = {10, 7, 8, 9, 1, 5};
        quickSort(arr2, 0, arr2.length - 1);
        System.out.println("Quick Sort: " + Arrays.toString(arr2));
        
        // Test 3: Heap Sort
        int[] arr3 = {12, 11, 13, 5, 6, 7};
        heapSort(arr3);
        System.out.println("Heap Sort: " + Arrays.toString(arr3));
        
        // Test 4: K-th Smallest
        int[] arr4 = {7, 10, 4, 3, 20, 15};
        int k = 3;
        System.out.println(k + "-th smallest: " + findKthSmallest(arr4, k));
        
        // Test 5: Counting Sort
        int[] arr5 = {4, 2, 2, 8, 3, 3, 1};
        countingSort(arr5);
        System.out.println("Counting Sort: " + Arrays.toString(arr5));
    }
}
```

### Simple Explanation of Each Method:

1. **Merge Sort:**
   - **What it does:** Divides array into halves, sorts them, merges back in order.
   - **How:** 
     - Recursively split until single elements, merge by comparing.
   - **Example:** `[64, 34, 25, 12]` → `[64, 34]`, `[25, 12]` → `[34, 64]`, `[12, 25]` → `[12, 25, 34, 64]`.
   - **Time Complexity:** O(n log n) always – log n levels, n work per merge.
   - **Key Point:** Stable, predictable performance, uses extra space.

2. **Quick Sort:**
   - **What it does:** Picks a pivot, partitions array, sorts subarrays.
   - **How:** 
     - Partition: Move smaller elements left of pivot, larger right.
     - Recurse on both sides.
   - **Pivot Importance:** 
     - Last element here; if always smallest/largest (e.g., sorted array), O(n²).
     - Random or median-of-three pivot → O(n log n) average.
   - **Example:** `[10, 7, 8, 9, 1, 5]`, pivot 5 → `[1, 5, 10, 7, 8, 9]`.
   - **Time Complexity:** O(n log n) avg, O(n²) worst.
   - **Key Point:** In-place, but pivot choice critical for performance.

3. **Heap Sort:**
   - **What it does:** Builds a max-heap, extracts max repeatedly to sort.
   - **How:** 
     - Build max-heap (largest at root).
     - Swap root with end, reduce heap size, heapify.
   - **Example:** `[12, 11, 13]` → heap `[13, 11, 12]` → `[12, 11, 13]` → `[11, 12, 13]`.
   - **Time Complexity:** O(n log n) – n heapify calls, log n per heapify.
   - **Key Point:** In-place, not stable, good for fixed memory.

4. **K-th Smallest Element:**
   - **What it does:** Finds the k-th smallest element using Quick Sort’s partition.
   - **How:** 
     - Partition like Quick Sort, but only recurse on relevant side.
   - **Example:** `[7, 10, 4, 3, 20, 15]`, k=3 → partition around 15 → `[7, 10, 4, 3]` → 4.
   - **Time Complexity:** O(n) avg, O(n²) worst (bad pivot).
   - **Key Point:** Efficient for single element, stops early unlike full sort.

5. **Counting Sort:**
   - **What it does:** Sorts non-negative integers by counting occurrences.
   - **How:** 
     - Count each number, compute positions, build sorted array.
   - **Example:** `[4, 2, 2, 8]` → count `[0, 0, 2, 0, 1, 0, 0, 0, 1]` → `[2, 2, 4, 8]`.
   - **Time Complexity:** O(n + k) – n elements, k range of values.
   - **Key Point:** Stable, fast for small range, uses extra space.

### Output from main():
```
Merge Sort: [11, 12, 22, 25, 34, 64, 90]
Quick Sort: [1, 5, 7, 8, 9, 10]
Heap Sort: [5, 6, 7, 11, 12, 13]
3-th smallest: 4
Counting Sort: [1, 2, 2, 3, 3, 4, 8]
```


Run this code and tweak `main()` inputs to explore different arrays! Let me know if you need more details or examples.
