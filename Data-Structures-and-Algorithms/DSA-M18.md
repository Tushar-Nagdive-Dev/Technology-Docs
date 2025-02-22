## 🚀 **Module 13: Mastering Algorithm Techniques**  

Algorithm techniques provide strategic ways to solve complex problems efficiently. They are the foundation for designing advanced algorithms used in competitive programming, artificial intelligence, optimization problems, and real-world applications like routing, scheduling, and decision-making systems.

---

## **🔥 13.1 Why Learn Algorithm Techniques?**  
- **Optimize Time and Space Complexity:** Efficient problem-solving by reducing the number of operations.  
- **Enable Complex Problem Solving:** Breaking down complex problems into manageable subproblems.  
- **Competitive Programming:** Essential for solving challenging algorithmic problems.  
- **Real-World Applications:** Used in AI, ML, robotics, networking, and optimization systems.  

---

## **🔥 13.2 Overview of Algorithm Techniques**  
1. **Divide and Conquer** — Divide the problem into subproblems, solve them, and combine results.  
2. **Greedy Algorithms** — Make the locally optimal choice at each step.  
3. **Backtracking** — Explore all possible solutions and backtrack if a solution is not feasible.  
4. **Branch and Bound** — Prune branches that can't produce a better solution.  
5. **Dynamic Programming (DP)** — Solve overlapping subproblems and store intermediate results.  

---

## **🔥 13.3 Divide and Conquer**  
- **Definition:** Break the problem into smaller subproblems, solve them recursively, and combine the solutions.  
- **Key Idea:** **Divide → Conquer → Combine**  
- **Time Complexity:** `O(N log N)` in many cases.  
- **Space Complexity:** Depends on recursion depth.  
- **Examples:** Merge Sort, Quick Sort, Binary Search, Closest Pair of Points.  

---

### 📘 **Example Code: Merge Sort using Divide and Conquer**  
```java
public class MergeSort {
    // Merge two sorted subarrays
    private static void merge(int[] arr, int left, int mid, int right) {
        int n1 = mid - left + 1;
        int n2 = right - mid;

        int[] L = new int[n1];
        int[] R = new int[n2];

        for (int i = 0; i < n1; i++) {
            L[i] = arr[left + i];
        }
        for (int j = 0; j < n2; j++) {
            R[j] = arr[mid + 1 + j];
        }

        int i = 0, j = 0;
        int k = left;
        while (i < n1 && j < n2) {
            if (L[i] <= R[j]) {
                arr[k] = L[i];
                i++;
            } else {
                arr[k] = R[j];
                j++;
            }
            k++;
        }

        while (i < n1) {
            arr[k] = L[i];
            i++;
            k++;
        }

        while (j < n2) {
            arr[k] = R[j];
            j++;
            k++;
        }
    }

    // Recursive Merge Sort
    public static void mergeSort(int[] arr, int left, int right) {
        if (left < right) {
            int mid = left + (right - left) / 2;

            mergeSort(arr, left, mid);
            mergeSort(arr, mid + 1, right);

            merge(arr, left, mid, right);
        }
    }

    public static void main(String[] args) {
        int[] arr = {12, 11, 13, 5, 6, 7};
        mergeSort(arr, 0, arr.length - 1);

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
5 6 7 11 12 13 
```

---

### 🔥 **Explanation:**  
- **Divide:** Split the array into two halves.  
- **Conquer:** Recursively sort each half.  
- **Combine:** Merge the sorted halves.  
- **Time Complexity:** `O(N log N)` — Recursively divides and merges.  
- **Space Complexity:** `O(N)` — Temporary arrays for merging.  

---

## **🔥 13.4 Greedy Algorithms**  
- **Definition:** Make the locally optimal choice at each step with the hope of finding the global optimum.  
- **Key Idea:** **"Greed is Good"** — Choose the best option available at each step.  
- **Examples:**  
    - **Activity Selection Problem:** Select the maximum number of activities without overlapping.  
    - **Huffman Coding:** Optimal prefix codes for data compression.  
    - **Kruskal's and Prim's Algorithms:** Minimum Spanning Tree.  
    - **Dijkstra’s Algorithm:** Shortest path in a graph.  

---

### 📘 **Example Code: Activity Selection Problem**  
- **Problem Statement:** Given `n` activities with start and end times, select the maximum number of activities that don't overlap.  
- **Approach:**  
    - Sort activities by their end times.  
    - Select the activity that finishes first and continue selecting the next non-overlapping activity.  

```java
import java.util.Arrays;
import java.util.Comparator;

class Activity {
    int start;
    int end;

    Activity(int start, int end) {
        this.start = start;
        this.end = end;
    }
}

public class ActivitySelection {
    public static void selectActivities(Activity[] activities) {
        Arrays.sort(activities, Comparator.comparingInt(a -> a.end));

        System.out.println("Selected Activities:");
        int lastEnd = -1;
        for (Activity activity : activities) {
            if (activity.start >= lastEnd) {
                System.out.println("Activity: Start = " + activity.start + ", End = " + activity.end);
                lastEnd = activity.end;
            }
        }
    }

    public static void main(String[] args) {
        Activity[] activities = {
            new Activity(1, 4),
            new Activity(3, 5),
            new Activity(0, 6),
            new Activity(5, 7),
            new Activity(3, 8),
            new Activity(5, 9),
            new Activity(6, 10),
            new Activity(8, 11),
            new Activity(8, 12),
            new Activity(2, 13),
            new Activity(12, 14)
        };

        selectActivities(activities);
    }
}
```

---

### 📊 **Output:**  
```
Selected Activities:
Activity: Start = 1, End = 4
Activity: Start = 5, End = 7
Activity: Start = 8, End = 11
Activity: Start = 12, End = 14
```

---

### 🔥 **Explanation:**  
- **Sorting:** Activities are sorted by end times.  
- **Selection:** Greedily selects the activity that ends the earliest and is non-overlapping.  
- **Time Complexity:** `O(N log N)` for sorting and `O(N)` for selection.  
- **Space Complexity:** `O(1)`  

---

## **🔥 13.5 When to Use Which Algorithm Technique?**  
- **Divide and Conquer:** When a problem can be divided into independent subproblems.  
- **Greedy Algorithms:** When local optimization leads to the global optimum.  
- **Backtracking:** When you need to explore all possible solutions.  
- **Dynamic Programming:** When there are overlapping subproblems and optimal substructure.  

---

## **📝 Exercise Set:**  
1. Implement Quick Sort using Divide and Conquer.  
2. Solve the Fractional Knapsack Problem using Greedy Algorithm.  
3. Implement Huffman Coding for data compression.  
4. Write a solution for the N-Queens problem using Backtracking.  
5. Implement the Traveling Salesman Problem using Branch and Bound.  

---

## 🔥 **Next: Graph Algorithms**  
Algorithm techniques are heavily used in **Graph Algorithms** like Shortest Path, Minimum Spanning Tree, and Network Flow. Next, we will explore these powerful graph algorithms.  

---
