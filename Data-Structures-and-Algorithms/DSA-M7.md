## 🚀 **2.10 Advanced String Manipulation in Java**  

Strings are powerful in programming, and mastering advanced string manipulation is essential for solving complex problems. This module covers advanced techniques, including pattern matching, regular expressions, and solving real-world interview questions using strings.

---

## **🔥 2.10.1 Pattern Matching and Searching**  
### 📘 **Finding Substring Occurrences**  
Let's find all the occurrences of a substring within a string.  

### 📘 **Example Code: Find Substring Occurrences**  
```java
public class AdvancedStringManipulation {
    public static void findOccurrences(String text, String pattern) {
        int index = text.indexOf(pattern);
        while (index != -1) {
            System.out.println("Found at index: " + index);
            index = text.indexOf(pattern, index + 1);
        }
    }

    public static void main(String[] args) {
        String text = "abracadabra";
        String pattern = "abra";
        findOccurrences(text, pattern);
    }
}
```

---

### 📊 **Output:**  
```
Found at index: 0
Found at index: 7
```

---

### 🔥 **Explanation:**  
- The `indexOf()` method finds the first occurrence of the substring.  
- By providing the starting index as `index + 1`, we find subsequent occurrences.  
- This continues until no more occurrences are found (`index = -1`).  

---

## **🔥 2.10.2 Using Regular Expressions (Regex)**  
### 📘 **What is Regex?**  
- A regular expression (regex) is a sequence of characters that form a search pattern.  
- It is used for pattern matching, searching, and replacing text.  

### 📘 **Common Regex Patterns**  
| Pattern | Description                  | Example Match           |
|---------|-------------------------------|--------------------------|
| `.`     | Any character                 | a.b → `acb`, `aab`        |
| `\d`    | Any digit (0-9)                | \d+ → `123`, `4567`       |
| `\w`    | Any word character (a-z, A-Z, 0-9, _) | \w+ → `word123`, `var_1`  |
| `\s`    | Any whitespace                 | \s+ → space, tab, newline |
| `^`     | Start of string                | ^abc → `abc`, `abcdef`    |
| `$`     | End of string                  | xyz$ → `xyz`, `abcxyz`    |
| `*`     | Zero or more repetitions       | ab*c → `ac`, `abc`, `abbc`|
| `+`     | One or more repetitions        | ab+c → `abc`, `abbc`      |
| `?`     | Zero or one occurrence         | colou?r → `color`, `colour` |
| `|`     | OR condition                   | cat|dog → `cat`, `dog`    |

---

### 📘 **Example Code: Regex Pattern Matching**  
```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class RegexExample {
    public static void main(String[] args) {
        String text = "My email is example@mail.com and his email is test@mail.org";
        String regex = "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}";

        Pattern pattern = Pattern.compile(regex);
        Matcher matcher = pattern.matcher(text);

        while (matcher.find()) {
            System.out.println("Found email: " + matcher.group());
        }
    }
}
```

---

### 📊 **Output:**  
```
Found email: example@mail.com
Found email: test@mail.org
```

---

### 🔥 **Explanation:**  
- **Pattern.compile()** compiles the regular expression into a pattern.  
- **Matcher** is used to find matching substrings.  
- **matcher.find()** finds the next occurrence, and **matcher.group()** returns the matched text.  

---

## **🔥 2.10.3 String Replacement using Regex**  
```java
public class StringReplacement {
    public static void main(String[] args) {
        String text = "The price is $100 and the discount is $20";
        String updatedText = text.replaceAll("\\$\\d+", "CONFIDENTIAL");
        System.out.println("Updated Text: " + updatedText);
    }
}
```

---

### 📊 **Output:**  
```
Updated Text: The price is CONFIDENTIAL and the discount is CONFIDENTIAL
```

---

### 🔥 **Explanation:**  
- `\\$\\d+` matches any dollar sign followed by one or more digits.  
- `replaceAll()` replaces all occurrences of the pattern with the given string.  

---

## **🔥 2.10.4 String Palindrome Check**  
Let's check if a string is a palindrome (reads the same forward and backward).  

### 📘 **Example Code: Palindrome Check**  
```java
public class PalindromeCheck {
    public static boolean isPalindrome(String str) {
        int left = 0;
        int right = str.length() - 1;

        while (left < right) {
            if (str.charAt(left) != str.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }

    public static void main(String[] args) {
        String input = "madam";
        System.out.println("Is Palindrome? " + isPalindrome(input));
    }
}
```

---

### 📊 **Output:**  
```
Is Palindrome? true
```

---

### 🔥 **Explanation:**  
- Two pointers are used to compare characters from both ends.  
- If characters at any position don’t match, it's not a palindrome.  
- The time complexity is `O(N)`, where `N` is the length of the string.  

---

## **🔥 2.10.5 Anagram Check**  
An anagram is a word or phrase formed by rearranging the letters of another word or phrase.  

### 📘 **Example Code: Anagram Check**  
```java
import java.util.Arrays;

public class AnagramCheck {
    public static boolean areAnagrams(String str1, String str2) {
        // If lengths are different, they can't be anagrams
        if (str1.length() != str2.length()) {
            return false;
        }
        
        // Convert strings to character arrays
        char[] arr1 = str1.toCharArray();
        char[] arr2 = str2.toCharArray();

        // Sort both arrays
        Arrays.sort(arr1);
        Arrays.sort(arr2);

        // Compare sorted arrays
        return Arrays.equals(arr1, arr2);
    }

    public static void main(String[] args) {
        String str1 = "listen";
        String str2 = "silent";
        System.out.println("Are Anagrams? " + areAnagrams(str1, str2));
    }
}
```

---

### 📊 **Output:**  
```
Are Anagrams? true
```

---

### 🔥 **Explanation:**  
- Both strings are converted to character arrays and sorted.  
- If the sorted arrays are equal, then the strings are anagrams.  
- The time complexity is `O(N log N)` due to the sorting operation.  

---

## **🔥 2.10.6 Common Mistakes to Avoid**  
- **Using `==` for String Comparison:** Always use `.equals()` for content comparison.  
- **Ignoring Case Sensitivity:** Use `.toLowerCase()` or `.equalsIgnoreCase()` for case-insensitive comparison.  
- **Not Escaping Special Characters in Regex:** Special characters like `.` or `+` should be escaped with `\\`.  

---

## **📝 Exercise Set:**  
1. Write a function to find the longest substring without repeating characters.  
2. Implement a method to find the most frequent character in a string.  
3. Write a function to find all permutations of a string.  
4. Implement a pattern matching algorithm without using regex.  
5. Write a function to count the number of words in a sentence.  

---

## 🔥 **Next: Linked Lists**  
After mastering Strings, the next step is to learn **Linked Lists**. Linked Lists are dynamic data structures used for efficient memory management and flexible insertion/deletion.

---

## 🔥 **Ready to Proceed?**  
- Do you feel comfortable with Advanced String Manipulation so far?  
- Would you like more examples or explanations on any topic?  
- Were you able to run the example programs successfully?
