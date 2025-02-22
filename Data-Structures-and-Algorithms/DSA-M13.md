## 🚀 **Module 8: Mastering Dynamic Programming (DP)**  

Dynamic Programming (DP) is a powerful technique for solving complex optimization problems by breaking them down into overlapping subproblems. It is widely used in algorithm design, competitive programming, and real-world applications like route optimization, financial modeling, and game theory.

---

## **🔥 8.1 What is Dynamic Programming?**  
- **Definition:** A method for solving problems by breaking them down into simpler overlapping subproblems and storing their solutions to avoid redundant calculations.  
- **Key Idea:** **“Save the result of subproblems to avoid re-computation.”**  
- **Example:** Fibonacci series, shortest path problems, and knapsack problem.  

---

## **🔥 8.2 Why Use Dynamic Programming?**  
- To **avoid redundant calculations** in recursive algorithms with overlapping subproblems.  
- To **optimize time complexity** by storing intermediate results.  
- To **simplify complex problems** using a bottom-up approach.  

---

## **🔥 8.3 Characteristics of DP Problems**  
1. **Optimal Substructure:** The optimal solution of the problem can be obtained by combining optimal solutions of subproblems.  
2. **Overlapping Subproblems:** The problem can be broken down into subproblems which are reused several times.  

---

## **🔥 8.4 Types of Dynamic Programming Approaches**  
1. **Top-Down Approach (Memoization):**  
    - Solves the problem recursively and stores the result of subproblems.  
    - Avoids redundant calculations by looking up the stored results.  
2. **Bottom-Up Approach (Tabulation):**  
    - Solves smaller subproblems first and builds up the solution to the larger problem.  
    - Uses an iterative approach with a table to store intermediate results.  

---

## **🔥 8.5 Example 1: Fibonacci Series using DP**  
The Fibonacci series is a classic example of overlapping subproblems:  
```
F(n) = F(n-1) + F(n-2)
Base Cases:
    F(0) = 0
    F(1) = 1
```

---

### 📘 **1. Top-Down Approach (Memoization)**  
```java
public class FibonacciMemoization {
    private static int[] memo;

    // Recursive method with Memoization
    public static int fibonacci(int n) {
        // Base Cases
        if (n <= 1) {
            return n;
        }

        // Check if already calculated
        if (memo[n] != 0) {
            return memo[n];
        }

        // Store the result in the memo array
        memo[n] = fibonacci(n - 1) + fibonacci(n - 2);
        return memo[n];
    }

    public static void main(String[] args) {
        int num = 10;
        memo = new int[num + 1];  // Initialize memo array
        System.out.println("Fibonacci of " + num + " is: " + fibonacci(num));
    }
}
```

---

### 📊 **Output:**  
```
Fibonacci of 10 is: 55
```

---

### 🔥 **Explanation:**  
- **Memoization Array (`memo[]`):** Stores the result of subproblems to avoid redundant calculations.  
- **Recursive Calls:** Calculate the result only if it is not already stored in `memo[]`.  
- **Time Complexity:** `O(N)` — Each subproblem is calculated only once.  
- **Space Complexity:** `O(N)` — Due to the memoization array and call stack.  

---

### 📘 **2. Bottom-Up Approach (Tabulation)**  
```java
public class FibonacciTabulation {
    public static int fibonacci(int n) {
        if (n <= 1) {
            return n;
        }

        int[] dp = new int[n + 1];
        dp[0] = 0;
        dp[1] = 1;

        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }

    public static void main(String[] args) {
        int num = 10;
        System.out.println("Fibonacci of " + num + " is: " + fibonacci(num));
    }
}
```

---

### 📊 **Output:**  
```
Fibonacci of 10 is: 55
```

---

### 🔥 **Explanation:**  
- **Tabulation Array (`dp[]`):** Stores the solution of subproblems iteratively.  
- **Bottom-Up Approach:** Solves smaller subproblems first and builds up to the final solution.  
- **Time Complexity:** `O(N)` — Linear time.  
- **Space Complexity:** `O(N)` — Due to the tabulation array.  

---

## **🔥 8.6 Example 2: Longest Common Subsequence (LCS)**  
### 📘 **Problem Statement:**  
- Given two strings, find the length of the longest subsequence common to both strings.  
- **Example:**  
    ```
    String A = "AGGTAB"
    String B = "GXTXAYB"
    LCS = "GTAB" → Length = 4
    ```

---

### 📘 **Recursive Definition:**  
```
LCS(X, Y, m, n) = 
    LCS(X, Y, m-1, n-1) + 1   if X[m-1] == Y[n-1]
    max(LCS(X, Y, m-1, n), LCS(X, Y, m, n-1))   otherwise
Base Case:
    LCS(X, Y, m, n) = 0  if m == 0 or n == 0
```

---

### 📘 **Example Code: LCS using Bottom-Up DP**  
```java
public class LongestCommonSubsequence {
    public static int lcs(String X, String Y) {
        int m = X.length();
        int n = Y.length();
        int[][] dp = new int[m + 1][n + 1];

        // Fill the dp table
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (X.charAt(i - 1) == Y.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[m][n];
    }

    public static void main(String[] args) {
        String X = "AGGTAB";
        String Y = "GXTXAYB";
        System.out.println("Length of LCS: " + lcs(X, Y));
    }
}
```

---

### 📊 **Output:**  
```
Length of LCS: 4
```

---

### 🔥 **Explanation:**  
- **2D DP Array (`dp[][]`):** Stores the length of LCS for substrings of X and Y.  
- **Time Complexity:** `O(M * N)` — M and N are the lengths of strings X and Y.  
- **Space Complexity:** `O(M * N)` — Due to the 2D DP array.  

---

## **🔥 8.7 Common Dynamic Programming Problems**  
1. **0/1 Knapsack Problem**  
2. **Longest Increasing Subsequence (LIS)**  
3. **Matrix Chain Multiplication**  
4. **Coin Change Problem**  
5. **Edit Distance (Levenshtein Distance)**  

---

## **📝 Exercise Set:**  
1. Write a DP solution for the 0/1 Knapsack problem.  
2. Implement the Longest Increasing Subsequence (LIS) using DP.  
3. Write a function to calculate the minimum number of coins needed for a given amount.  
4. Implement the Edit Distance problem using DP.  
5. Solve the Matrix Chain Multiplication problem using DP.  

---

## 🔥 **Next: Sorting Algorithms**  
Dynamic Programming is often combined with **Sorting Algorithms** for optimized solutions. Next, we will learn about various sorting techniques, including Quick Sort, Merge Sort, and Heap Sort.

---
