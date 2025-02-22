I'll write these five Java programs with simple explanations for each. We'll create a single class containing all methods and include a main method to test them.

```java
import java.util.*;

public class ArrayOperations {
    // 1. Find the second largest element in an array
    public static int findSecondLargest(int[] arr) {
        // If array has less than 2 elements, no second largest exists
        if (arr.length < 2) return -1;
        
        int largest = Integer.MIN_VALUE;    // Keep track of biggest number
        int secondLargest = Integer.MIN_VALUE;  // Keep track of second biggest
        
        // One pass through array
        for (int num : arr) {
            if (num > largest) {
                secondLargest = largest;    // Old largest becomes second
                largest = num;             // New largest
            } else if (num > secondLargest && num != largest) {
                secondLargest = num;       // Update second largest if different from largest
            }
        }
        
        // If no second largest found (all elements same)
        return secondLargest == Integer.MIN_VALUE ? -1 : secondLargest;
    }
    
    // 2. Check if array is sorted (non-decreasing order)
    public static boolean isSorted(int[] arr) {
        // Empty or single element arrays are always sorted
        if (arr.length <= 1) return true;
        
        // Compare each adjacent pair
        for (int i = 0; i < arr.length - 1; i++) {
            if (arr[i] > arr[i + 1]) {    // If current > next, not sorted
                return false;
            }
        }
        return true;
    }
    
    // 3. Find all pairs that sum up to given number
    public static List<int[]> findPairs(int[] arr, int target) {
        List<int[]> pairs = new ArrayList<>();
        HashMap<Integer, Integer> map = new HashMap<>();
        
        // Count frequency of each number
        for (int num : arr) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        
        // Check for pairs
        for (int num : arr) {
            int complement = target - num;
            if (map.containsKey(complement)) {
                // If complement exists and has frequency
                if (complement == num && map.get(num) > 1) {
                    pairs.add(new int[]{num, num});
                    map.put(num, map.get(num) - 2);
                } else if (complement != num && map.get(complement) > 0 && map.get(num) > 0) {
                    pairs.add(new int[]{Math.min(num, complement), Math.max(num, complement)});
                    map.put(num, map.get(num) - 1);
                    map.put(complement, map.get(complement) - 1);
                }
            }
        }
        return pairs;
    }
    
    // 4. Rotate array right by k positions without extra space
    public static void rotateArray(int[] arr, int k) {
        if (arr.length == 0 || k == 0) return;
        
        // Handle if k is larger than array length
        k = k % arr.length;
        
        // Reverse helper method
        reverse(arr, 0, arr.length - 1);      // Reverse entire array
        reverse(arr, 0, k - 1);               // Reverse first k elements
        reverse(arr, k, arr.length - 1);      // Reverse remaining elements
    }
    
    private static void reverse(int[] arr, int start, int end) {
        while (start < end) {
            int temp = arr[start];
            arr[start] = arr[end];
            arr[end] = temp;
            start++;
            end--;
        }
    }
    
    // 5. Merge two sorted arrays
    public static int[] mergeSortedArrays(int[] arr1, int[] arr2) {
        int[] result = new int[arr1.length + arr2.length];
        int i = 0;  // pointer for arr1
        int j = 0;  // pointer for arr2
        int k = 0;  // pointer for result
        
        // Compare and merge while both arrays have elements
        while (i < arr1.length && j < arr2.length) {
            if (arr1[i] <= arr2[j]) {
                result[k++] = arr1[i++];
            } else {
                result[k++] = arr2[j++];
            }
        }
        
        // Copy remaining elements of arr1, if any
        while (i < arr1.length) result[k++] = arr1[i++];
        // Copy remaining elements of arr2, if any
        while (j < arr2.length) result[k++] = arr2[j++];
        
        return result;
    }

    // Main method to test all functions
    public static void main(String[] args) {
        // Test 1: Second Largest
        int[] arr1 = {5, 2, 8, 1, 9};
        System.out.println("Second Largest: " + findSecondLargest(arr1));  // Should print 8
        
        // Test 2: Is Sorted
        int[] arr2 = {1, 2, 3, 4};
        System.out.println("Is Sorted: " + isSorted(arr2));  // Should print true
        
        // Test 3: Find Pairs
        int[] arr3 = {2, 4, 3, 5, 7};
        List<int[]> pairs = findPairs(arr3, 7);
        System.out.println("Pairs summing to 7:");
        for (int[] pair : pairs) {
            System.out.println(pair[0] + ", " + pair[1]);  // Should print 2,5 and 3,4
        }
        
        // Test 4: Rotate Array
        int[] arr4 = {1, 2, 3, 4, 5};
        rotateArray(arr4, 2);
        System.out.println("Rotated array: " + Arrays.toString(arr4));  // Should print [4, 5, 1, 2, 3]
        
        // Test 5: Merge Sorted Arrays
        int[] arr5 = {1, 3, 5};
        int[] arr6 = {2, 4, 6};
        int[] merged = mergeSortedArrays(arr5, arr6);
        System.out.println("Merged array: " + Arrays.toString(merged));  // Should print [1, 2, 3, 4, 5, 6]
    }
}
```

### Simple Explanation of Each Method:

1. **Find Second Largest:**
   - We use two variables: `largest` and `secondLargest`.
   - For each number, if it's bigger than `largest`, we update both variables.
   - If it's between `largest` and `secondLargest`, we only update `secondLargest`.
   - Example: For [5, 2, 8, 1, 9], largest = 9, secondLargest = 8.

2. **Check if Sorted:**
   - Compare each number with the next one.
   - If any number is bigger than the following number, array isn't sorted.
   - Example: [1, 2, 3, 4] → all numbers increase or stay same → sorted.

3. **Find Pairs with Sum:**
   - Use a HashMap to count how many times each number appears.
   - For each number, check if (target - number) exists in map.
   - Add valid pairs and reduce their counts to avoid duplicates.
   - Example: [2, 4, 3, 5] with target 7 → pairs are (2,5) and (3,4).

4. **Rotate Array Without Extra Space:**
   - Uses a clever trick: reverse the whole array, then reverse first k elements, then reverse rest.
   - Example: [1, 2, 3, 4, 5] with k=2:
     - Reverse all: [5, 4, 3, 2, 1]
     - Reverse first 2: [4, 5, 3, 2, 1]
     - Reverse rest: [4, 5, 1, 2, 3]

5. **Merge Sorted Arrays:**
   - Use two pointers to compare elements from both arrays.
   - Pick smaller element each time and add to result.
   - Add any remaining elements from either array.
   - Example: [1, 3, 5] and [2, 4, 6] → compare and merge to get [1, 2, 3, 4, 5, 6].

### Output from main():
```
Second Largest: 8
Is Sorted: true
Pairs summing to 7:
2, 5
3, 4
Rotated array: [4, 5, 1, 2, 3]
Merged array: [1, 2, 3, 4, 5, 6]
```
