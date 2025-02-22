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

Below is a Java program implementing five string-related functions with simple and clear explanations for each. We'll create a single class `StringOperations` containing all methods and include a `main` method to test them.

```java
import java.util.*;

public class StringOperations {
    // 1. Check if a string is a palindrome
    public static boolean isPalindrome(String str) {
        // Remove spaces and convert to lowercase for fair comparison
        str = str.replaceAll("\\s", "").toLowerCase();
        
        int left = 0;
        int right = str.length() - 1;
        
        // Compare characters from both ends moving inward
        while (left < right) {
            if (str.charAt(left) != str.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
    
    // 2. Count occurrence of each character in a string
    public static Map<Character, Integer> countCharacters(String str) {
        Map<Character, Integer> charCount = new HashMap<>();
        
        // Iterate through each character and count
        for (char c : str.toCharArray()) {
            charCount.put(c, charCount.getOrDefault(c, 0) + 1);
        }
        return charCount;
    }
    
    // 3. Reverse each word in a sentence
    public static String reverseWords(String sentence) {
        // Split sentence into words
        String[] words = sentence.split("\\s+");
        StringBuilder result = new StringBuilder();
        
        // Reverse each word and build result
        for (String word : words) {
            StringBuilder reversedWord = new StringBuilder(word).reverse();
            result.append(reversedWord).append(" ");
        }
        
        // Remove extra space at the end and return
        return result.toString().trim();
    }
    
    // 4. Find longest palindromic substring
    public static String longestPalindrome(String str) {
        if (str == null || str.length() < 1) return "";
        
        int start = 0, maxLength = 1;  // Start index and length of longest palindrome
        
        // Check each position as potential center of palindrome
        for (int i = 0; i < str.length(); i++) {
            // Check for odd length palindromes (center at i)
            int len1 = expandAroundCenter(str, i, i);
            // Check for even length palindromes (center between i and i+1)
            int len2 = expandAroundCenter(str, i, i + 1);
            
            int len = Math.max(len1, len2);
            if (len > maxLength) {
                start = i - (len - 1) / 2;  // Calculate start of palindrome
                maxLength = len;
            }
        }
        
        return str.substring(start, start + maxLength);
    }
    
    private static int expandAroundCenter(String str, int left, int right) {
        // Expand around center while characters match and within bounds
        while (left >= 0 && right < str.length() && str.charAt(left) == str.charAt(right)) {
            left--;
            right++;
        }
        return right - left - 1;  // Length of palindrome
    }
    
    // 5. Check if two strings are anagrams
    public static boolean areAnagrams(String str1, String str2) {
        // Remove spaces and convert to lowercase
        str1 = str1.replaceAll("\\s", "").toLowerCase();
        str2 = str2.replaceAll("\\s", "").toLowerCase();
        
        // If lengths differ after cleanup, not anagrams
        if (str1.length() != str2.length()) return false;
        
        // Count characters in first string
        int[] charCount = new int[26];  // For a-z
        for (char c : str1.toCharArray()) {
            charCount[c - 'a']++;
        }
        
        // Decrease count for second string
        for (char c : str2.toCharArray()) {
            charCount[c - 'a']--;
        }
        
        // Check if all counts are zero
        for (int count : charCount) {
            if (count != 0) return false;
        }
        return true;
    }

    // Main method to test all functions
    public static void main(String[] args) {
        // Test 1: Palindrome
        String str1 = "Racecar";
        System.out.println("Is '" + str1 + "' a palindrome? " + isPalindrome(str1));  // true
        
        // Test 2: Character count
        String str2 = "hello";
        Map<Character, Integer> counts = countCharacters(str2);
        System.out.println("Character counts in '" + str2 + "': " + counts);  // {h=1, e=1, l=2, o=1}
        
        // Test 3: Reverse words
        String sentence = "Hello World Java";
        String reversed = reverseWords(sentence);
        System.out.println("Reversed words: '" + reversed + "'");  // "olleH dlroW avaJ"
        
        // Test 4: Longest palindrome
        String str3 = "babad";
        String longestPal = longestPalindrome(str3);
        System.out.println("Longest palindrome in '" + str3 + "': '" + longestPal + "'");  // "bab" or "aba"
        
        // Test 5: Anagrams
        String str4 = "Listen";
        String str5 = "Silent";
        System.out.println("'" + str4 + "' and '" + str5 + "' are anagrams? " + areAnagrams(str4, str5));  // true
    }
}
```

### Simple Explanation of Each Method:

1. **Check if String is a Palindrome:**
   - **What it does:** Checks if a string reads the same forwards and backwards (e.g., "racecar").
   - **How:** Remove spaces, make lowercase, then compare characters from both ends moving inward.
   - **Example:** "Racecar" → "racecar" → check 'r' vs 'r', 'a' vs 'a', etc. → true.
   - **Key Point:** Ignores spaces and case (e.g., "A man a plan" → true).

2. **Count Characters:**
   - **What it does:** Counts how many times each character appears in the string.
   - **How:** Uses a HashMap to store each character and its count, incrementing as we go.
   - **Example:** "hello" → h:1, e:1, l:2, o:1.
   - **Key Point:** Simple and shows frequency of all characters, including spaces if present.

3. **Reverse Each Word:**
   - **What it does:** Takes a sentence and reverses each word individually (e.g., "Hello World" → "olleH dlroW").
   - **How:** Split into words, reverse each word using StringBuilder, then join back with spaces.
   - **Example:** "Hello World" → ["Hello", "World"] → ["olleH", "dlroW"] → "olleH dlroW".
   - **Key Point:** Keeps word order, only reverses letters within each word.

4. **Find Longest Palindromic Substring:**
   - **What it does:** Finds the longest substring that’s a palindrome (e.g., "babad" → "bab").
   - **How:** Checks each character as a possible center of a palindrome, expands outward to find longest match.
   - **Example:** "babad" → check around 'b', 'a', etc. → "bab" or "aba" (length 3).
   - **Key Point:** Handles both odd-length ("aba") and even-length ("aa") palindromes.

5. **Check if Two Strings are Anagrams:**
   - **What it does:** Checks if two strings have the same characters with the same frequencies (e.g., "listen" and "silent").
   - **How:** Clean up strings, use an array to count character frequencies in first string, decrease for second, check if all zero.
   - **Example:** "Listen" → "listen", "Silent" → "silent" → same letters, same counts → true.
   - **Key Point:** Ignores case and spaces, focuses on character composition.

### Output from main():
```
Is 'Racecar' a palindrome? true
Character counts in 'hello': {e=1, h=1, l=2, o=1}
Reversed words: 'olleH dlroW avaJ'
Longest palindrome in 'babad': 'bab'
'Listen' and 'Silent' are anagrams? true
```

### Why It’s Easy to Understand:
- **Comments:** Each step is explained in the code.
- **Simple Logic:** Uses basic loops, conditionals, and common Java tools (HashMap, StringBuilder).
- **Examples:** Main method shows how each function works with clear outputs.
- **Edge Cases:** Handles empty strings, spaces, and case sensitivity where appropriate.

Feel free to run this code and tweak the inputs in `main()` to see how it behaves with different strings!
