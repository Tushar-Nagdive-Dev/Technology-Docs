Below is a Java program implementing five dynamic programming (DP) solutions with clear explanations and well-documented code. We'll create a `DynamicProgramming` class containing all methods and a `main` method to test them.

```java
import java.util.Arrays;

public class DynamicProgramming {
    // 1. 0/1 Knapsack Problem
    /**
     * Finds maximum value achievable with given capacity, items can be taken once or not at all.
     * Time Complexity: O(n * capacity), Space Complexity: O(n * capacity)
     */
    public static int knapsack(int[] weights, int[] values, int capacity) {
        int n = weights.length;
        int[][] dp = new int[n + 1][capacity + 1];
        
        // Build DP table bottom-up
        for (int i = 1; i <= n; i++) {
            for (int w = 0; w <= capacity; w++) {
                if (weights[i - 1] <= w) {
                    // Max of including item (value + remaining capacity) or excluding it
                    dp[i][w] = Math.max(values[i - 1] + dp[i - 1][w - weights[i - 1]], dp[i - 1][w]);
                } else {
                    // Can't include item, take previous value
                    dp[i][w] = dp[i - 1][w];
                }
            }
        }
        return dp[n][capacity];
    }
    
    // 2. Longest Increasing Subsequence (LIS)
    /**
     * Finds length of longest subsequence where elements are in increasing order.
     * Time Complexity: O(n^2), Space Complexity: O(n)
     */
    public static int longestIncreasingSubsequence(int[] arr) {
        int n = arr.length;
        int[] dp = new int[n];
        Arrays.fill(dp, 1); // Each element is an LIS of length 1 initially
        
        // Build DP array
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (arr[i] > arr[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1); // Extend subsequence if increasing
                }
            }
        }
        
        // Find maximum length
        int max = 0;
        for (int len : dp) {
            max = Math.max(max, len);
        }
        return max;
    }
    
    // 3. Minimum Coins for Amount
    /**
     * Calculates minimum number of coins needed to make the amount.
     * Returns -1 if impossible. Time Complexity: O(amount * coins), Space Complexity: O(amount)
     */
    public static int minCoins(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0; // Base case: 0 coins for amount 0
        
        // Build DP array
        for (int i = 1; i <= amount; i++) {
            for (int coin : coins) {
                if (coin <= i && dp[i - coin] != Integer.MAX_VALUE) {
                    dp[i] = Math.min(dp[i], dp[i - coin] + 1);
                }
            }
        }
        return dp[amount] == Integer.MAX_VALUE ? -1 : dp[amount];
    }
    
    // 4. Edit Distance Problem
    /**
     * Finds minimum operations (insert, delete, replace) to convert str1 to str2.
     * Time Complexity: O(m * n), Space Complexity: O(m * n)
     */
    public static int editDistance(String str1, String str2) {
        int m = str1.length(), n = str2.length();
        int[][] dp = new int[m + 1][n + 1];
        
        // Fill first row and column
        for (int i = 0; i <= m; i++) dp[i][0] = i; // Deleting all chars from str1
        for (int j = 0; j <= n; j++) dp[0][j] = j; // Inserting all chars from str2
        
        // Build DP table
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (str1.charAt(i - 1) == str2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1]; // No operation needed
                } else {
                    dp[i][j] = 1 + Math.min(dp[i - 1][j - 1], // Replace
                                  Math.min(dp[i][j - 1],       // Insert
                                           dp[i - 1][j]));     // Delete
                }
            }
        }
        return dp[m][n];
    }
    
    // 5. Matrix Chain Multiplication
    /**
     * Finds minimum cost to multiply chain of matrices given their dimensions.
     * Time Complexity: O(n^3), Space Complexity: O(n^2)
     */
    public static int matrixChainMultiplication(int[] dims) {
        int n = dims.length - 1; // Number of matrices
        int[][] dp = new int[n][n];
        
        // Cost is 0 for single matrix (length 1 chain)
        for (int i = 0; i < n; i++) {
            dp[i][i] = 0;
        }
        
        // Length of chain (L) from 2 to n
        for (int L = 2; L <= n; L++) {
            for (int i = 0; i < n - L + 1; i++) {
                int j = i + L - 1;
                dp[i][j] = Integer.MAX_VALUE;
                
                // Try all possible splits between i and j
                for (int k = i; k < j; k++) {
                    int cost = dp[i][k] + dp[k + 1][j] + 
                               dims[i] * dims[k + 1] * dims[j + 1];
                    dp[i][j] = Math.min(dp[i][j], cost);
                }
            }
        }
        return dp[0][n - 1];
    }
    
    // Main method to test all functions
    public static void main(String[] args) {
        // Test 1: 0/1 Knapsack
        int[] weights = {1, 3, 4, 5};
        int[] values = {1, 4, 5, 7};
        int capacity = 7;
        System.out.println("Max value in knapsack: " + knapsack(weights, values, capacity)); // 9
        
        // Test 2: Longest Increasing Subsequence
        int[] arr = {10, 9, 2, 5, 3, 7, 101, 18};
        System.out.println("Length of LIS: " + longestIncreasingSubsequence(arr)); // 4
        
        // Test 3: Minimum Coins
        int[] coins = {1, 4, 6};
        int amount = 8;
        System.out.println("Min coins for " + amount + ": " + minCoins(coins, amount)); // 2
        
        // Test 4: Edit Distance
        String str1 = "sunday";
        String str2 = "saturday";
        System.out.println("Edit distance between '" + str1 + "' and '" + str2 + "': " 
            + editDistance(str1, str2)); // 3
        
        // Test 5: Matrix Chain Multiplication
        int[] dims = {10, 30, 5, 60}; // Matrices: 10x30, 30x5, 5x60
        System.out.println("Min cost of matrix chain multiplication: " 
            + matrixChainMultiplication(dims)); // 4500
    }
}
```

### Simple Explanation of Each Method:

1. **0/1 Knapsack Problem:**
   - **What it does:** Finds the max value you can carry in a knapsack with limited capacity, picking each item once or not at all.
   - **How:** 
     - Uses a 2D DP table `dp[i][w]` = max value with i items and w capacity.
     - For each item, decide: include (if fits) or exclude, take max.
   - **Example:** Weights `[1, 3, 4, 5]`, Values `[1, 4, 5, 7]`, Capacity 7 → Take 3 (4) and 4 (5) → 9.
   - **Key Point:** Can’t use fractions of items, classic optimization problem.

2. **Longest Increasing Subsequence (LIS):**
   - **What it does:** Finds the longest sequence in an array where numbers increase (not necessarily consecutive).
   - **How:** 
     - `dp[i]` = length of LIS ending at index i.
     - For each element, check all previous elements, extend if increasing.
   - **Example:** `[10, 9, 2, 5, 3, 7, 101, 18]` → LIS `[2, 5, 7, 101]` → 4.
   - **Key Point:** Looks backward to build forward.

3. **Minimum Coins for Amount:**
   - **What it does:** Finds the fewest coins needed to make an amount (unlimited coin supply).
   - **How:** 
     - `dp[i]` = min coins for amount i.
     - For each amount, try all coins, take min of previous + 1.
   - **Example:** Coins `[1, 4, 6]`, Amount 8 → Use 4 + 4 → 2 coins.
   - **Key Point:** Returns -1 if impossible (e.g., no coins match amount).

4. **Edit Distance:**
   - **What it does:** Finds minimum edits (insert, delete, replace) to turn one string into another.
   - **How:** 
     - `dp[i][j]` = min edits for str1[0..i-1] to str2[0..j-1].
     - If chars match, no edit; else, min of replace, insert, delete + 1.
   - **Example:** "sunday" to "saturday" → Insert 'a', 't', replace 'n' → 3.
   - **Key Point:** Builds solution character by character.

5. **Matrix Chain Multiplication:**
   - **What it does:** Finds the least cost (multiplications) to multiply a chain of matrices.
   - **How:** 
     - `dp[i][j]` = min cost to multiply matrices from i to j.
     - Try all split points k, compute cost as left + right + merge cost.
   - **Example:** Dims `[10, 30, 5, 60]` (10x30, 30x5, 5x60) → Split at 30x5 → 4500.
   - **Key Point:** Order of multiplication matters, DP finds optimal grouping.

### Output from main():
```
Max value in knapsack: 9
Length of LIS: 4
Min coins for 8: 2
Edit distance between 'sunday' and 'saturday': 3
Min cost of matrix chain multiplication: 4500
```

Run this code and modify `main()` inputs to test different scenarios! Let me know if you need more details or examples.
