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
