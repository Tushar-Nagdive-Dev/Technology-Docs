## 🚀 **Module 16: Competitive Programming Practice**  

Competitive programming is all about applying your knowledge of data structures and algorithms to solve challenging problems under time constraints. This module focuses on problem-solving techniques, advanced implementations, and competitive programming strategies to excel in coding competitions.

---

## **🔥 16.1 Why Practice Competitive Programming?**  
- **Improve Problem-Solving Skills:** Enhance logical and analytical thinking.  
- **Speed and Accuracy:** Solve complex problems quickly and accurately.  
- **Algorithmic Optimization:** Write time and space-efficient code.  
- **Coding Interviews:** Excel in technical interviews with top tech companies.  
- **Global Recognition:** Compete globally and earn recognition on competitive platforms.  

---

## **🔥 16.2 Competitive Programming Workflow**  
1. **Problem Understanding:** Read the problem statement carefully and understand the requirements.  
2. **Input and Output Analysis:** Identify the input format, constraints, and expected output.  
3. **Algorithm Design:** Choose the right algorithm and data structure.  
4. **Complexity Analysis:** Evaluate the time and space complexity of the solution.  
5. **Implementation:** Write clean and efficient code.  
6. **Testing:** Test the solution with sample inputs and edge cases.  
7. **Optimization:** Optimize the solution for better performance.  
8. **Submission and Debugging:** Submit the solution and debug if necessary.  

---

## **🔥 16.3 Competitive Programming Strategies**  
1. **Read Constraints Carefully:** Helps in choosing the right algorithm.  
2. **Optimize for Edge Cases:** Consider edge cases like empty arrays, negative numbers, large inputs.  
3. **Use Fast Input/Output:** Use `BufferedReader` and `PrintWriter` in Java for competitive programming.  
4. **Precompute Results:** Use preprocessing techniques like prefix sums and dynamic programming tables.  
5. **Time and Space Trade-Offs:** Optimize time complexity by using more space or vice versa.  
6. **Practice Regularly:** Consistent practice improves speed and accuracy.  

---

## **🔥 16.4 Advanced Problem-Solving Techniques**  
1. **Binary Search on Answer:** Binary search to find the minimum or maximum value that satisfies a condition.  
2. **Meet-in-the-Middle:** Divide the problem into two halves and combine results efficiently.  
3. **Two Pointers and Sliding Window:** Efficient subarray and substring problems.  
4. **Square Root Decomposition:** Divide the array into √N blocks for range queries.  
5. **Mo's Algorithm:** Efficient range queries with offline processing.  
6. **Game Theory Algorithms:** Minimax, Alpha-Beta Pruning for competitive games.  

---

## **🔥 16.5 Problem 1: Maximum Subarray Sum (Kadane's Algorithm)**  
- **Problem Statement:** Find the maximum sum of a contiguous subarray in a given array of integers.  
- **Example:**  
    ```
    Input: [-2, 1, -3, 4, -1, 2, 1, -5, 4]
    Output: 6
    Explanation: [4, -1, 2, 1] has the maximum sum = 6.
    ```
- **Algorithm Used:** Kadane's Algorithm.  
- **Time Complexity:** `O(N)` — Single pass.  
- **Space Complexity:** `O(1)`  

---

### 📘 **Example Code: Maximum Subarray Sum using Kadane's Algorithm**  
```java
public class MaximumSubarraySum {
    public static int maxSubArray(int[] nums) {
        int maxSum = nums[0];
        int currentSum = nums[0];

        for (int i = 1; i < nums.length; i++) {
            currentSum = Math.max(nums[i], currentSum + nums[i]);
            maxSum = Math.max(maxSum, currentSum);
        }

        return maxSum;
    }

    public static void main(String[] args) {
        int[] nums = {-2, 1, -3, 4, -1, 2, 1, -5, 4};
        System.out.println("Maximum Subarray Sum: " + maxSubArray(nums));
    }
}
```

---

### 📊 **Output:**  
```
Maximum Subarray Sum: 6
```

---

### 🔥 **Explanation:**  
- **Current Sum:** Keeps track of the maximum sum of the current subarray.  
- **Max Sum:** Stores the maximum subarray sum found so far.  
- **Reset Condition:** If `currentSum < 0`, start a new subarray.  
- **Time Complexity:** `O(N)` — Single pass.  
- **Space Complexity:** `O(1)` — In-place calculation.  

---

## **🔥 16.6 Problem 2: Longest Increasing Subsequence (LIS)**  
- **Problem Statement:** Find the length of the longest increasing subsequence in an array.  
- **Example:**  
    ```
    Input: [10, 9, 2, 5, 3, 7, 101, 18]
    Output: 4
    Explanation: The LIS is [2, 3, 7, 101], length = 4.
    ```
- **Algorithm Used:** Dynamic Programming.  
- **Time Complexity:** `O(N²)`  
- **Space Complexity:** `O(N)`  

---

### 📘 **Example Code: Longest Increasing Subsequence (LIS)**  
```java
public class LongestIncreasingSubsequence {
    public static int lengthOfLIS(int[] nums) {
        if (nums.length == 0) return 0;

        int[] dp = new int[nums.length];
        Arrays.fill(dp, 1);

        for (int i = 1; i < nums.length; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
        }

        int maxLength = 0;
        for (int length : dp) {
            maxLength = Math.max(maxLength, length);
        }

        return maxLength;
    }

    public static void main(String[] args) {
        int[] nums = {10, 9, 2, 5, 3, 7, 101, 18};
        System.out.println("Length of LIS: " + lengthOfLIS(nums));
    }
}
```

---

### 📊 **Output:**  
```
Length of LIS: 4
```

---

### 🔥 **Explanation:**  
- **Dynamic Programming Array (`dp[]`):** Stores the length of LIS ending at each index.  
- **Nested Loop:** Compares each element with the previous elements to find the LIS.  
- **Time Complexity:** `O(N²)` — Due to nested loops.  
- **Space Complexity:** `O(N)` — DP array for LIS lengths.  

---

## **🔥 16.7 Advanced Competitive Programming Techniques**  
1. **Square Root Decomposition** — Efficient range queries.  
2. **Mo's Algorithm** — Offline range queries.  
3. **Game Theory** — Minimax and Alpha-Beta pruning.  
4. **Graph Algorithms** — Strongly Connected Components, Topological Sorting.  
5. **Data Structures** — Segment Trees, Fenwick Trees, Tries, and Disjoint Sets.  
6. **Dynamic Programming Optimization** — Divide and Conquer DP, Convex Hull Trick.  

---

## **🔥 16.8 Practice Platforms and Resources**  
1. **LeetCode** — Wide variety of algorithmic challenges.  
2. **Codeforces** — Real-time contests with challenging problems.  
3. **CodeChef** — Monthly long challenges and cook-offs.  
4. **AtCoder** — Japanese platform with high-level problems.  
5. **HackerRank** — Algorithmic challenges focused on data structures.  
6. **TopCoder** — Competitive programming community with single-round matches.  

---

## 🔥 **Next: Interview Preparation**  
Competitive programming skills are crucial for coding interviews. Next, we will focus on **Interview Preparation**, including frequently asked questions, coding patterns, and mock interview problems.

---
