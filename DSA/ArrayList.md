### **1. What is an ArrayList?**

In Java, a standard array (`int[] arr = new int[5]`) has a fixed size. If it gets full, you cannot add more elements.

An `ArrayList` solves this by acting as a **dynamic array**. It automatically grows when it runs out of space and shrinks when elements are removed. It is part of the Java Collections Framework and implements the `List` interface.

**Crucial Rule for ArrayLists:** They can only store **Objects**, not primitive types. You must use Wrapper classes (e.g., `Integer` instead of `int`, `Double` instead of `double`, `Character` instead of `char`).

### **2. Under the Hood (Interview Gold)**

Interviewers don't just want to know *how* to use it; they want to know *how it works*.

* **Default Capacity:** When you create an empty `ArrayList`, Java creates an internal array with a default capacity of **10**.
* **Dynamic Resizing:** When you try to add an 11th element, the ArrayList creates a brand new array, copies the old elements over, and adds the new one.
* **Growth Factor:** In Java, it grows by **50%**. The formula used internally is `newCapacity = oldCapacity + (oldCapacity >> 1)`.

### **3. When to Use vs. When NOT to Use**

Understanding the time complexity ($O$-notation) is critical for DSA interviews.

| Operation | Time Complexity | Explanation |
| --- | --- | --- |
| **Access (Get)** | $O(1)$ | Direct access via index, just like a regular array. **Best use case.** |
| **Add to End** | $O(1)$ | Usually instant (amortized time), unless a resize triggers. |
| **Insert/Delete (Middle)** | $O(n)$ | **Worst use case.** Every element after the insertion/deletion point must be shifted left or right. |
| **Search (Contains)** | $O(n)$ | Must check every element sequentially. |

**Verdict:** Use `ArrayList` when you need frequent reads and are mostly adding elements to the end. Avoid it if you need to frequently insert or delete elements at the beginning or middle (use `LinkedList` for that instead).

---

### **4. Creating an ArrayList**

```java
import java.util.ArrayList;
import java.util.List;

public class ArrayListMastery {
    public static void main(String[] args) {
        // Standard way (Best Practice: Code to the interface)
        List<String> names = new ArrayList<>();
        
        // Specifying initial capacity (Optimization if you know the exact size needed)
        List<Integer> numbers = new ArrayList<>(50); 
    }
}

```

---

### **5. Core Methods Masterclass**

Here are the built-in methods you need to know, grouped by functionality.

#### **Adding Elements**

```java
List<String> cities = new ArrayList<>();

// 1. add(E element) -> Appends to the end. O(1)
cities.add("New York");
cities.add("London");

// 2. add(int index, E element) -> Inserts at specific position. O(n)
// Shifts "London" to the right.
cities.add(1, "Tokyo"); 
// Result: [New York, Tokyo, London]

```

#### **Accessing & Modifying**

```java
// 3. get(int index) -> Retrieves element. O(1)
String city = cities.get(0); // "New York"

// 4. set(int index, E element) -> Replaces an element. O(1)
cities.set(1, "Paris"); 
// Result: [New York, Paris, London]

```

#### **Removing Elements**

```java
// 5. remove(int index) -> Removes by position. O(n)
cities.remove(2); // Removes "London"

// 6. remove(Object o) -> Removes first occurrence of a specific value. O(n)
cities.remove("Paris"); 

// 7. clear() -> Removes all elements. O(n)
cities.clear();

```

#### **Checking Status & Searching**

```java
cities.add("Pune");
cities.add("Mumbai");

// 8. size() -> Returns the number of elements. O(1)
int count = cities.size(); // 2

// 9. isEmpty() -> Checks if size is 0. O(1)
boolean empty = cities.isEmpty(); // false

// 10. contains(Object o) -> Checks if element exists. O(n)
boolean hasPune = cities.contains("Pune"); // true

// 11. indexOf(Object o) -> Returns first index of element, or -1 if not found. O(n)
int index = cities.indexOf("Mumbai"); // 1

```

#### **Advanced & Utility Methods (Java 8+)**

```java
List<Integer> nums = new ArrayList<>(List.of(10, 25, 30, 45, 50));

// 12. removeIf() -> Removes elements based on a condition (Predicate)
nums.removeIf(n -> n % 2 != 0); // Removes all odd numbers
// Result: [10, 30, 50]

// 13. toArray() -> Converts ArrayList to standard Array
Integer[] arr = nums.toArray(new Integer[0]);

```

---

### **6. Iteration (Traversing the ArrayList)**

Interviewers will often expect you to loop through an `ArrayList`. Here are the four ways, from traditional to modern.

```java
List<String> tech = new ArrayList<>(List.of("Java", "Python", "C++"));

// 1. Traditional For Loop (Use when you need the index)
for (int i = 0; i < tech.size(); i++) {
    System.out.println(tech.get(i));
}

// 2. Enhanced For Loop (Cleanest for simple reading)
for (String lang : tech) {
    System.out.println(lang);
}

// 3. Iterator (Use if you need to REMOVE elements while looping)
Iterator<String> it = tech.iterator();
while (it.hasNext()) {
    String lang = it.next();
    if (lang.equals("C++")) {
        it.remove(); // Safe removal during iteration
    }
}

// 4. Java 8 forEach (Functional approach)
tech.forEach(lang -> System.out.println(lang));

```

### **1. The "Two Pointers" Pattern**

This is the most common pattern for searching or manipulating a 1D list, especially if it is **sorted**. You place one pointer (index) at the beginning and one at the end, moving them inward based on a condition.

* **Best for:** Finding pairs that sum to a target, reversing an array, or removing duplicates in-place.
* **Time Complexity:** Usually $O(n)$
* **Space Complexity:** $O(1)$ (In-place)

**Example: Two Sum on a Sorted ArrayList**
Find two numbers in a sorted list that add up to a specific target.

```java
public boolean hasTwoSum(List<Integer> nums, int target) {
    int left = 0;
    int right = nums.size() - 1;
    
    while (left < right) {
        int currentSum = nums.get(left) + nums.get(right);
        
        if (currentSum == target) {
            return true; // Found the pair
        } else if (currentSum < target) {
            left++; // We need a bigger sum, move left pointer to the right
        } else {
            right--; // We need a smaller sum, move right pointer to the left
        }
    }
    return false;
}

```

### **2. The "Sliding Window" Pattern**

This pattern is a subset of Two Pointers. Instead of looking at individual elements, you look at a "window" (a contiguous sublist) of elements. You expand the right side of the window to add elements, and shrink the left side to remove them.

* **Best for:** Problems asking for "maximum/minimum sum of a contiguous subarray," "longest substring," or "subarrays of size K."
* **Time Complexity:** $O(n)$

**Example: Maximum Sum Subarray of Size K**
Find the maximum sum of any contiguous sequence of `k` elements.

```java
public int maxSumSubarray(List<Integer> nums, int k) {
    if (nums.size() < k) return 0;
    
    int maxSum = 0;
    int windowSum = 0;
    
    // 1. Calculate the sum of the first window (first 'k' elements)
    for (int i = 0; i < k; i++) {
        windowSum += nums.get(i);
    }
    maxSum = windowSum;
    
    // 2. Slide the window across the rest of the list
    for (int i = k; i < nums.size(); i++) {
        // Add the next element, subtract the element that fell out of the window
        windowSum += nums.get(i) - nums.get(i - k);
        maxSum = Math.max(maxSum, windowSum);
    }
    
    return maxSum;
}

```

---

### **3. Built-in Algorithms (`Collections` Utility)**

Never write your own sorting or binary search algorithm in an interview unless explicitly asked to. Use Java's highly optimized built-in methods from the `java.util.Collections` class.

#### **Sorting (Timsort)**

Java uses a hybrid algorithm (Timsort) which is incredibly efficient for real-world data.

```java
List<Integer> nums = new ArrayList<>(List.of(5, 2, 8, 1, 9));

// Sort Ascending -> O(n log n)
Collections.sort(nums); 
// Result: [1, 2, 5, 8, 9]

// Sort Descending 
Collections.sort(nums, Collections.reverseOrder());
// Result: [9, 8, 5, 2, 1]

```

#### **Binary Search**

**Rule:** The list *must* be sorted before you use this, otherwise the results are unpredictable.

```java
List<Integer> sortedNums = new ArrayList<>(List.of(10, 20, 30, 40, 50));

// Returns the index of the element. O(log n) time complexity.
int index = Collections.binarySearch(sortedNums, 30); // Returns 2

// If the element is not found, it returns (-(insertion point) - 1)
int missingIndex = Collections.binarySearch(sortedNums, 25); // Returns -3

```