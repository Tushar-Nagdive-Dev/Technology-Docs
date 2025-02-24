## How do you reverse a string in Java?
Alright, let’s reverse a string efficiently in Java! Since Java strings are immutable, we can’t modify them directly, but we can work with a `char` array to achieve the same two-pointer swapping trick I mentioned earlier. This keeps it fast—O(n) time—and lean on memory—O(1) extra space beyond the array we create. Here’s how it looks:

```java
public class ReverseString {
    public static String reverseString(String s) {
        // Convert string to char array since strings are immutable in Java
        char[] chars = s.toCharArray();
        int left = 0;
        int right = chars.length - 1;
        
        // Swap characters from outside in
        while (left < right) {
            char temp = chars[left];
            chars[left] = chars[right];
            chars[right] = temp;
            left++;
            right--;
        }
        
        // Convert back to string
        return new String(chars);
    }

    public static void main(String[] args) {
        String s = "hello";
        System.out.println(reverseString(s));  // Output: "olleh"
    }
}
```

Why’s this efficient? We’re only traversing the array once, swapping characters in place with a single temporary variable. The `toCharArray()` and `new String()` steps are O(n), but they’re just the cost of working around Java’s immutability—there’s no way to avoid that if we want a string output. The core swapping logic itself is as tight as it gets.

If you’re using Java 8+, you could also lean on `StringBuilder`’s built-in `reverse()` method, like this:

```java
public static String reverseString(String s) {
    return new StringBuilder(s).reverse().toString();
}
```

This is concise and still O(n), but under the hood, it’s doing similar work—possibly with a bit more overhead from the `StringBuilder` object. For raw efficiency, the `char` array approach wins because it’s more direct.

## How do you check if a string is a palindrome?
```java
public class PalindromeCheck {
    public static boolean isPalindrome(String s) {
        // Handle null or empty string
        if (s == null || s.isEmpty()) {
            return true;  // Often considered palindromes by convention
        }

        int left = 0;
        int right = s.length() - 1;

        // Compare characters from both ends moving inward
        while (left < right) {
            if (s.charAt(left) != s.charAt(right)) {
                return false;  // Mismatch found, not a palindrome
            }
            left++;
            right--;
        }
        return true;  // All characters matched
    }

    public static void main(String[] args) {
        String s1 = "racecar";
        String s2 = "hello";
        System.out.println(isPalindrome(s1));  // Output: true
        System.out.println(isPalindrome(s2));  // Output: false
    }
}
```

## How can you determine if two strings are anagrams of each other?
```java
public class AnagramCheck {
    public static boolean areAnagrams(String s1, String s2) {
        s1 = s1.toLowerCase();
        s2 = s2.toLowerCase();
        
        // Quick checks: null handling and length mismatch
        if (s1 == null || s2 == null) {
            return s1 == s2;  // Both null = true, one null = false
        }
        if (s1.length() != s2.length()) {
            return false;  // Different lengths can’t be anagrams
        }

        // Use an array to count character frequencies (ASCII size = 128)
        int[] charCount = new int[128];  // Covers basic ASCII

        // Increment counts for s1, decrement for s2
        for (int i = 0; i < s1.length(); i++) {
            charCount[s1.charAt(i)]++;  // Add s1’s char
            charCount[s2.charAt(i)]--;  // Subtract s2’s char
        }

        // If they’re anagrams, all counts should be zero
        for (int count : charCount) {
            if (count != 0) {
                return false;  // Mismatch in frequency
            }
        }
        return true;
    }

    public static void main(String[] args) {
        String s1 = "listen";
        String s2 = "silent";
        String s3 = "hello";
        System.out.println(areAnagrams(s1, s2));  // Output: true
        System.out.println(areAnagrams(s1, s3));  // Output: false
    }
}
```

## How do you find the first non-repeating character in a string?

Let’s tackle finding the first non-repeating character in a string in Java, aiming for efficiency! The goal is to identify the first character that appears exactly once, like 'l' in "leetcode" or 'w' in "world". A fast and clean way to do this is to use a frequency counter—specifically, an array or map to track how often each character appears—then scan the string again to find the first one with a count of 1. This gives us O(n) time and O(1) space if we assume a fixed character set (like ASCII).

Here’s an efficient solution using an array:

```java
public class FirstNonRepeatingChar {
    public static char findFirstNonRepeating(String s) {
        // Handle null or empty string
        if (s == null || s.isEmpty()) {
            return '\0';  // Null char as a convention for "not found"
        }

        // Array for ASCII character counts (128 covers basic ASCII)
        int[] charCount = new int[128];

        // Count frequency of each character
        for (int i = 0; i < s.length(); i++) {
            charCount[s.charAt(i)]++;
        }

        // Find first character with count of 1
        for (int i = 0; i < s.length(); i++) {
            if (charCount[s.charAt(i)] == 1) {
                return s.charAt(i);
            }
        }

        return '\0';  // No non-repeating character found
    }

    public static void main(String[] args) {
        String s1 = "leetcode";
        String s2 = "loveleetcode";
        String s3 = "aabb";
        System.out.println(findFirstNonRepeating(s1));  // Output: 'l'
        System.out.println(findFirstNonRepeating(s2));  // Output: 'v'
        System.out.println(findFirstNonRepeating(s3));  // Output: '\0' (none)
    }
}
```

### Why’s this efficient?
- **Time Complexity**: O(n) — One pass to count frequencies, one pass to find the first non-repeater. Total is 2n, which is still O(n).
- **Space Complexity**: O(1) — The `charCount` array is fixed at 128 (or 256 for extended ASCII), not growing with input size.
- **No Extra Data Structures**: Unlike a map or set, the array keeps it lightweight and fast for a limited character set.

### How it works
1. Build a frequency table: Loop through the string, incrementing the count for each character.
2. Check in order: Loop again, returning the first character whose count is 1.
3. If nothing’s found, return a sentinel like `\0`.

### Tweaks and Alternatives
- **Unicode Support**: If the string might have more than ASCII (e.g., "héllo"), use a `HashMap` instead. Here’s that version:

```java
import java.util.HashMap;

public static char findFirstNonRepeating(String s) {
    if (s == null || s.isEmpty()) return '\0';

    HashMap<Character, Integer> charCount = new HashMap<>();
    // Count frequencies
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        charCount.merge(c, 1, Integer::sum);
    }
    // Find first non-repeater
    for (int i = 0; i < s.length(); i++) {
        if (charCount.get(s.charAt(i)) == 1) {
            return s.charAt(i);
        }
    }
    return '\0';
}
```

This is still O(n) time but O(k) space, where k is the number of unique characters. It’s more flexible but less space-efficient than the array.

- **Single Pass with LinkedHashMap**: You could track order and counts in one go with a `LinkedHashMap`, but it’s overkill—still O(n) time and O(n) space, and slower due to hash operations.

## How can you remove duplicate characters from a string?
Let’s dive into removing duplicate characters from a string in Java efficiently! The goal is to take a string like "hello" and turn it into "helo", keeping only the first occurrence of each character. Since Java strings are immutable, we’ll need to build a new result, and the trick is to balance speed and space. One of the most efficient ways is to use a boolean array (or set) to track seen characters while preserving order, giving us O(n) time and O(1) space for a fixed character set like ASCII.

Here’s a solid solution:

```java
public class RemoveDuplicates {
    public static String removeDuplicates(String s) {
        // Handle null or empty string
        if (s == null || s.isEmpty()) {
            return s;
        }

        // Boolean array to mark seen characters (ASCII size = 128)
        boolean[] seen = new boolean[128];
        StringBuilder result = new StringBuilder();

        // Iterate through string, keeping first occurrence
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (!seen[c]) {  // If not seen yet
                seen[c] = true;
                result.append(c);
            }
        }

        return result.toString();
    }

    public static void main(String[] args) {
        String s1 = "hello";
        String s2 = "aabbcc";
        System.out.println(removeDuplicates(s1));  // Output: "helo"
        System.out.println(removeDuplicates(s2));  // Output: "abc"
    }
}
```

### Why’s this efficient?
- **Time Complexity**: O(n) — One pass through the string, with constant-time array lookups and appends (amortized for `StringBuilder`).
- **Space Complexity**: O(1) — The `seen` array is fixed at 128 (or 256 for extended ASCII), and `StringBuilder` scales with unique characters, but that’s part of the output.
- **Preserves Order**: We keep the first occurrence of each character naturally, no sorting needed.

### How it works
1. Use a `boolean` array to flag characters we’ve seen.
2. Build the result with `StringBuilder` (faster than string concatenation).
3. Only append a character the first time we see it.

### Tweaks and Alternatives
- **Unicode Support**: If your string has more than ASCII (e.g., "hééllo"), switch to a `HashSet`. Here’s that version:

```java
import java.util.HashSet;

public static String removeDuplicates(String s) {
    if (s == null || s.isEmpty()) return s;

    HashSet<Character> seen = new HashSet<>();
    StringBuilder result = new StringBuilder();

    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        if (seen.add(c)) {  // add() returns true if c wasn’t already present
            result.append(c);
        }
    }
    return result.toString();
}
```

This is still O(n) time, but space is O(k) where k is the number of unique characters. It’s a tad slower due to hash operations but handles any character set.

- **In-Place (Sort of)**: If you’re given a `char[]` instead of a `String`, you could mark duplicates in-place with a similar tracking array and shift characters, but Java’s string immutability makes `StringBuilder` the practical choice here.

- **LinkedHashSet**: If order matters (it does here), a `LinkedHashSet` works too, but it’s O(n) space and slower than a plain array or `HashSet`.

The boolean array version is the efficiency champ for basic strings—fast lookups, minimal memory. If your strings are wild with Unicode, the `HashSet` version’s got you covered. What’s your string like? Need a specific twist on this?

## How do you check if a string contains only digits?
Let’s figure out an efficient way to check if a string contains only digits in Java! The goal is to verify that every character is a number (0-9), like "12345", and reject strings like "12a34" or "abc". The most efficient approach is to scan the string once, checking each character against a digit condition, which gives us O(n) time and O(1) space—no extra data structures needed.

Here’s a straightforward and fast solution:

```java
public class DigitCheck {
    public static boolean containsOnlyDigits(String s) {
        // Handle null or empty string
        if (s == null || s.isEmpty()) {
            return false;  // Convention: null/empty isn’t "only digits"
        }

        // Check each character
        for (int i = 0; i < s.length(); i++) {
            if (!Character.isDigit(s.charAt(i))) {
                return false;  // Found a non-digit
            }
        }
        return true;  // All characters are digits
    }

    public static void main(String[] args) {
        String s1 = "12345";
        String s2 = "12a34";
        String s3 = "";
        System.out.println(containsOnlyDigits(s1));  // Output: true
        System.out.println(containsOnlyDigits(s2));  // Output: false
        System.out.println(containsOnlyDigits(s3));  // Output: false
    }
}
```

### Why’s this efficient?
- **Time Complexity**: O(n) — Single pass through the string, with `charAt()` and `isDigit()` being O(1) per character.
- **Space Complexity**: O(1) — No extra memory beyond a loop variable.
- **Early Exit**: If we hit a non-digit, we stop immediately—no need to check the rest.

### How it works
- `Character.isDigit()` checks if a character is a decimal digit (0-9 in Unicode). It’s built into Java and super fast.
- Loop through each character; if any fail the digit test, return `false`.
- If we make it through, it’s all digits—return `true`.

### Tweaks and Alternatives
- **Manual Check**: Instead of `isDigit()`, you could check `c >= '0' && c <= '9'`. It’s slightly faster (avoids method call overhead) and works for ASCII digits:

```java
public static boolean containsOnlyDigits(String s) {
    if (s == null || s.isEmpty()) return false;

    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        if (c < '0' || c > '9') {
            return false;
        }
    }
    return true;
}
```

This is still O(n) and O(1), just a bit leaner if you’re sure your input is ASCII.

- **Regex**: You could use `s.matches("\\d+")`, but it’s overkill—O(n) time with worse constants and extra object creation. Not as efficient.
- **Empty String Handling**: I return `false` for empty strings or null, but if you want `true` (e.g., "empty is valid"), just tweak the initial check.

## How do you find the longest substring without repeating characters?

Let’s crack the problem of finding the longest substring without repeating characters in Java, aiming for efficiency! We want to take a string like "abcabcbb" and find "abc" (length 3) or "pwwkew" and get "wke" (length 3). The key is to track characters and their positions while sliding through the string. The most efficient approach uses a sliding window with a map or array to mark seen characters, achieving O(n) time and O(min(m, n)) space, where n is the string length and m is the character set size.

Here’s an optimized solution using a `HashMap`:

```java
import java.util.HashMap;

public class LongestSubstring {
    public static String longestSubstringWithoutRepeating(String s) {
        if (s == null || s.isEmpty()) {
            return "";
        }

        // Map to store character -> last seen index
        HashMap<Character, Integer> seen = new HashMap<>();
        int start = 0;  // Start of current window
        int maxLength = 0;  // Length of longest substring
        int maxStart = 0;  // Start index of longest substring

        // Slide through the string
        for (int end = 0; end < s.length(); end++) {
            char current = s.charAt(end);
            
            // If character is seen and within current window, shrink window
            if (seen.containsKey(current) && seen.get(current) >= start) {
                start = seen.get(current) + 1;
            } else {
                // Update max if current window is longer
                if (end - start + 1 > maxLength) {
                    maxLength = end - start + 1;
                    maxStart = start;
                }
            }
            seen.put(current, end);  // Update last seen index
        }

        // Extract the substring
        return s.substring(maxStart, maxStart + maxLength);
    }

    public static void main(String[] args) {
        String s1 = "abcabcbb";
        String s2 = "bbbbb";
        String s3 = "pwwkew";
        System.out.println(longestSubstringWithoutRepeating(s1));  // Output: "abc"
        System.out.println(longestSubstringWithoutRepeating(s2));  // Output: "b"
        System.out.println(longestSubstringWithoutRepeating(s3));  // Output: "wke"
    }
}
```

### Why’s this efficient?
- **Time Complexity**: O(n) — Each character is processed once, and `HashMap` operations (get/put) are O(1) on average.
- **Space Complexity**: O(min(m, n)) — The map stores at most `min(m, n)` entries, where m is the character set size (e.g., 128 for ASCII) and n is the string length.
- **Sliding Window**: We expand the window until we hit a repeat, then slide the start pointer just past the last occurrence—minimal backtracking.

### How it works
1. Use `start` and `end` pointers to define the current window.
2. Track each character’s last index in the `HashMap`.
3. When a repeat is found within the window, move `start` to skip the earlier occurrence.
4. Update the max length and starting point whenever the current window grows longer.
5. Finally, extract the substring using the stored indices.

### Alternative: Array for ASCII
If you’re sure the string is ASCII-only, use an array instead of a map for even better performance (O(1) space, no hash overhead):

```java
public static String longestSubstringWithoutRepeating(String s) {
    if (s == null || s.isEmpty()) return "";

    int[] lastIndex = new int[128];  // ASCII size
    for (int i = 0; i < 128; i++) {
        lastIndex[i] = -1;  // Initialize as not seen
    }

    int start = 0;
    int maxLength = 0;
    int maxStart = 0;

    for (int end = 0; end < s.length(); end++) {
        char current = s.charAt(end);
        if (lastIndex[current] >= start) {
            start = lastIndex[current] + 1;
        } else if (end - start + 1 > maxLength) {
            maxLength = end - start + 1;
            maxStart = start;
        }
        lastIndex[current] = end;
    }

    return s.substring(maxStart, maxStart + maxLength);
}
```

This drops space to O(1) for fixed-size character sets and skips hash overhead, making it slightly faster for simple strings.

### Why not brute force?
Checking every substring would be O(n²) or worse—way too slow. The sliding window cuts that down by only adjusting the window boundaries as needed.

The `HashMap` version handles any character set (Unicode included), while the array version is leaner for ASCII. Which suits your string? Want just the length instead of the substring? I can tweak it!

## How can you check if two strings are rotations of each other?
Let’s figure out how to efficiently check if two strings are rotations of each other in Java! Two strings are rotations if one can be shifted circularly to match the other—like "abcde" and "cdeab". The trick is to avoid manually checking every rotation, which would be slow. Instead, a clever and efficient solution leverages the fact that if `s2` is a rotation of `s1`, then `s2` must be a substring of `s1 + s1`. This runs in O(n) time and O(n) space, where n is the string length.

Here’s the solution:

```java
public class StringRotation {
    public static boolean areRotations(String s1, String s2) {
        // Check nulls and length mismatch
        if (s1 == null || s2 == null) {
            return s1 == s2;  // Both null = true, one null = false
        }
        if (s1.length() != s2.length()) {
            return false;  // Different lengths can’t be rotations
        }

        // Concatenate s1 with itself
        String doubleS1 = s1 + s1;
        
        // Check if s2 is a substring of doubleS1
        return doubleS1.contains(s2);
    }

    public static void main(String[] args) {
        String s1 = "abcde";
        String s2 = "cdeab";
        String s3 = "abcd";
        System.out.println(areRotations(s1, s2));  // Output: true
        System.out.println(areRotations(s1, s3));  // Output: false
    }
}
```

### Why’s this efficient?
- **Time Complexity**: O(n) — String concatenation is O(n), and `contains()` uses an efficient substring search (like KMP or Boyer-Moore under the hood in Java, typically O(n+m), but here it’s O(n) since the pattern is the same length).
- **Space Complexity**: O(n) — We create a new string `doubleS1` of length 2n.
- **Simple Logic**: No need to manually rotate and compare every possibility, which would be O(n²).

### How it works
- If `s1 = "abcde"`, then `s1 + s1 = "abcdeabcde"`.
- Any rotation of `s1` (e.g., "cdeab") will appear as a contiguous chunk in `doubleS1`.
- `contains()` checks this in one shot.
- Early checks for null or length mismatch avoid wasted effort.

### Why does it work?
Imagine rotating "abcde":
- Shift 0: "abcde"
- Shift 1: "bcdea"
- Shift 2: "cdeab"
- All these are substrings of "abcdeabcde". The duplication ensures every possible rotation is covered.

### Alternative: Manual Rotation Check
You could rotate `s1` and compare it to `s2` at each position, but that’s O(n²)—way less efficient. Another option is to find the rotation point using character matching, but it’s trickier and rarely faster than the concatenation trick.

### Tweaks
- **Case Sensitivity**: Add `s1 = s1.toLowerCase(); s2 = s2.toLowerCase();` if "ABCde" and "cdeAB" should match.
- **Space Tradeoff**: If you’re desperate to avoid the extra string, you could use modular indexing to simulate rotation, but it’s more complex and still O(n²) unless paired with a smart search.

This method’s the sweet spot—fast, readable, and reliable. Does it fit your needs? Want to tweak it for something specific?

## How do you find the most frequent character in a string?
Let’s find the most frequent character in a string efficiently in Java! The goal is to identify which character appears the most, like 'l' in "hello" (appears 2 times). The most efficient approach is to use a frequency counter—either an array or a map—to tally occurrences in one pass, then find the max. For a fixed character set like ASCII, this gives us O(n) time and O(1) space, where n is the string length.

Here’s a lean solution using an array:

```java
public class MostFrequentChar {
    public static char mostFrequentChar(String s) {
        // Handle null or empty string
        if (s == null || s.isEmpty()) {
            return '\0';  // Sentinel for "not found"
        }

        // Array for ASCII character counts (128 for basic ASCII)
        int[] charCount = new int[128];
        int maxCount = 0;
        char maxChar = '\0';

        // Count frequencies
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            charCount[c]++;
            // Update max if this char’s count beats the current max
            if (charCount[c] > maxCount) {
                maxCount = charCount[c];
                maxChar = c;
            }
        }

        return maxChar;
    }

    public static void main(String[] args) {
        String s1 = "hello";
        String s2 = "aabbcc";
        System.out.println(mostFrequentChar(s1));  // Output: 'l' (2 times)
        System.out.println(mostFrequentChar(s2));  // Output: 'a' (2 times, first max)
    }
}
```

### Why’s this efficient?
- **Time Complexity**: O(n) — One pass through the string, with constant-time array updates and comparisons.
- **Space Complexity**: O(1) — The `charCount` array is fixed at 128 (or 256 for extended ASCII), regardless of input size.
- **Single Pass**: We track the max while counting, avoiding a separate scan.

### How it works
1. Use an array to count each character’s occurrences.
2. During the count, update the max count and corresponding character on the fly.
3. Return the character with the highest frequency (first one if tied).

### Alternative: HashMap for Unicode
If your string might have non-ASCII characters (e.g., "héllo"), use a `HashMap`:

```java
import java.util.HashMap;

public static char mostFrequentChar(String s) {
    if (s == null || s.isEmpty()) return '\0';

    HashMap<Character, Integer> charCount = new HashMap<>();
    int maxCount = 0;
    char maxChar = '\0';

    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        int count = charCount.merge(c, 1, Integer::sum);  // Increment count
        if (count > maxCount) {
            maxCount = count;
            maxChar = c;
        }
    }

    return maxChar;
}
```

- **Time**: Still O(n) — Hash operations are O(1) on average.
- **Space**: O(k) — Where k is the number of unique characters.
- **Flexibility**: Handles any character set, like emojis or accents.

### Tweaks
- **Ties**: This returns the first max encountered. Want the last? Move the max check to a second loop.
- **Count Too**: Return both char and frequency? Use a custom class or array.
- **Case Insensitivity**: Add `c = Character.toLowerCase(c)` if 'A' and 'a' should count together.

The array version is the efficiency king for ASCII strings—super fast and light. The `HashMap` version scales to any character set. What’s your string like? Need a specific twist?
