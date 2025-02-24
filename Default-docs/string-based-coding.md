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
