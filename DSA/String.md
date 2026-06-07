### **1. The Absolute Basics: What is a String?**

In Java, a String is **not a primitive data type** (like `int` or `char`). It is an **Object** that represents a sequence of characters. Under the hood (since Java 9), it is backed by a `byte[]` array to save memory (previously a `char[]` array).

### **2. How to Create a String (And Why it Matters)**

There are two primary ways to create a string, and interviewers care deeply about the difference because of how Java manages memory.

**A. String Literal (The most common way)**

```java
String s1 = "hello";

```

* **What happens:** Java checks a special memory area called the **String Constant Pool (SCP)**. If "hello" already exists, `s1` just points to the existing one. If it doesn't, Java creates it in the pool. This saves memory.

**B. Using the `new` Keyword**

```java
String s2 = new String("hello");

```

* **What happens:** This forces Java to create a **brand new object in the standard Heap memory**, regardless of whether "hello" already exists in the String Pool.

> **Interview Trap:** `s1 == s2` will evaluate to `false`. The `==` operator checks if they point to the exact same location in memory. Always use `s1.equals(s2)` to compare the actual text!

### **3. The Golden Rule: Immutability**

In Java, **Strings are immutable**. Once a String object is created, its value can **never** be changed.

```java
String str = "MAANG";
str.concat(" Bound"); // This creates a NEW string "MAANG Bound", but 'str' doesn't change!
System.out.println(str); // Still prints "MAANG"

// To actually update it, you must reassign the reference:
str = str.concat(" Bound"); 

```

**Why are they immutable?** 1.  **Security:** Strings are used for database URLs, passwords, and network connections. If they could be changed, a hacker could alter the string after security checks pass.
2.  **Thread Safety:** Because they can't change, multiple threads can safely read the same String at the same time without locking.
3.  **String Pool:** The SCP only works *because* strings don't change. If two variables point to "hello" and one modifies it, the other would unexpectedly change too.

### **4. StringBuilder vs. StringBuffer (The Solution to Immutability)**

Because Strings are immutable, concatenating strings in a `for` loop is a massive red flag in interviews. It creates a new object every single loop iteration, turning an $O(N)$ operation into an $O(N^2)$ memory disaster.

**The Fix:** Use `StringBuilder` (or `StringBuffer`). These represent *mutable* (changeable) sequences of characters.

```java
// Interview Best Practice for String Manipulation
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 5; i++) {
    sb.append(i); // Modifies the existing object, doesn't create a new one
}
String result = sb.toString(); // "01234"

```

* **StringBuilder:** Fast, but not thread-safe. **(Use this in 99% of your interview algorithms)**
* **StringBuffer:** Slower, but thread-safe (synchronized).

### **5. Essential String Methods (Your Interview Toolkit)**

Memorize these. You will use them constantly.

```java
String s = "Interview";

// 1. Length
int len = s.length(); // 9 (Note: Unlike arrays, length is a method() with parentheses)

// 2. Accessing Characters
char c = s.charAt(0); // 'I'

// 3. Substrings (Extremely common)
// substring(startIndex, endIndex) -> inclusive of start, EXCLUSIVE of end
String sub = s.substring(0, 5); // "Inter"

// 4. Searching
int index = s.indexOf("view"); // 5 (Returns starting index, or -1 if not found)

// 5. Conversion to Array (Crucial for sorting or modifying)
char[] chars = s.toCharArray(); // ['I', 'n', 't', 'e', 'r', 'v', 'i', 'e', 'w']

// 6. Splitting
String sentence = "a b c";
String[] words = sentence.split(" "); // ["a", "b", "c"]

```

---

### **6. Common MAANG String Patterns**

To ace the interview, you need to recognize the "patterns" behind String questions. Here are the two most critical ones:

#### **Pattern A: Two Pointers (e.g., Valid Palindrome)**

Used when you need to compare characters from opposite ends of the string.

```java
public boolean isPalindrome(String s) {
    int left = 0;
    int right = s.length() - 1;
    
    while (left < right) {
        if (s.charAt(left) != s.charAt(right)) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}

```

#### **Pattern B: Character Frequency Counting (e.g., Valid Anagram)**

Since there are only 26 lowercase English letters, you can use a fixed-size array of length 26 to count frequencies instead of a bulky `HashMap`. This is a classic $O(N)$ time, $O(1)$ space optimization.

```java
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) return false;
    
    int[] charCounts = new int[26];
    
    for (int i = 0; i < s.length(); i++) {
        // 'a' - 'a' = 0, 'b' - 'a' = 1. This maps letters to indices 0-25.
        charCounts[s.charAt(i) - 'a']++; 
        charCounts[t.charAt(i) - 'a']--; 
    }
    
    for (int count : charCounts) {
        if (count != 0) return false;
    }
    
    return true;
}

```

---

Here is the complete blueprint of built-in Java String methods and advanced interview patterns, broken down into clear, organized categories with simple examples.

---

## **Part 1: The Complete Java String Method Cheat Sheet**

Instead of memorizing a random list, group these methods by their actual functional purpose during an interview.

### **1. Inspection & Boundary Checks**

These methods help you analyze a string's properties without modifying it.

* **`length()`**: Returns the total number of characters.
```java
"MAANG".length(); // Returns 5

```


* **`charAt(int index)`**: Returns the character at a specific position ($0$-indexed).
```java
"Apple".charAt(1); // Returns 'p'

```


* **`isEmpty()`**: Returns `true` if `length()` is $0$.
```java
"".isEmpty(); // true

```


* **`isBlank()`** *(Java 11+)*: Returns `true` if the string is empty or contains only whitespace characters.
```java
"   ".isBlank(); // true ("".isEmpty() would be false here!)

```



### **2. Search & Verification**

Crucial for locating substrings or checking formatting rules.

* **`contains(CharSequence s)`**: Checks if a sequence of characters exists inside the string.
```java
"LeetCode".contains("Code"); // true

```


* **`indexOf(String str)` / `indexOf(char ch)**`: Returns the index of the **first** occurrence of the character/substring, or `-1` if not found.
```java
"banana".indexOf('a'); // 1
"banana".indexOf("na"); // 2

```


* **`lastIndexOf(String str)`**: Returns the index of the **last** occurrence.
```java
"banana".lastIndexOf('a'); // 5

```


* **`startsWith(String prefix)`** / **`endsWith(String suffix)`**: Checks if the string begins or ends with a specific sequence.
```java
"Driver.java".endsWith(".java"); // true

```



### **3. Transformation & Extraction**

These return a **new** String because strings are immutable.

* **`substring(int beginIndex)`** / **`substring(int beginIndex, int endIndex)`**: Extracts a portion of the string. **Remember:** The end index is exclusive.
```java
"Alphabet".substring(4);     // "abet"
"Alphabet".substring(0, 4);  // "Alpha" (Indices 0, 1, 2, 3)

```


* **`toLowerCase()`** / **`toUpperCase()`**: Converts the casing of the entire string.
```java
"Java".toUpperCase(); // "JAVA"

```


* **`trim()`** / **`strip()`**: Removes leading and trailing whitespaces. (`strip()` is Unicode-aware and preferred in newer Java versions).
```java
"  hello  ".trim(); // "hello"

```


* **`replace(char oldChar, char newChar)`** / **`replace(CharSequence target, CharSequence replacement)`**: Replaces occurrences of a character or sequence.
```java
"cat".replace('c', 'b'); // "bat"

```


* **`replaceAll(String regex, String replacement)`**: Replaces matching substrings using regular expressions. Great for cleaning up inputs.
```java
"a1b2c3".replaceAll("[0-9]", ""); // "abc" (Removes all digits)

```



### **4. Splitting & Joining**

Essential for tokenization problems (e.g., parsing sentences or file paths).

* **`split(String regex)`**: Breaks the string into an array based on a delimiter.
```java
String[] words = "one,two,three".split(","); // ["one", "two", "three"]

```


* **`String.join(CharSequence delimiter, CharSequence... elements)`**: Merges multiple strings together with a delimiter between them.
```java
String joined = String.join("-", "06", "07", "2026"); // "06-07-2026"

```


* **`toCharArray()`**: Converts the string into a mutable primitive character array.
```java
char[] arr = "hi".toCharArray(); // ['h', 'i']

```



---

## **Part 2: The Core Algorithmic String Patterns**

MAANG interviewers look for pattern recognition. Almost every advanced String question maps directly to one of these four algorithmic categories.

### **Pattern 1: Two Pointers (Inward or Outward Tracking)**

Used to compare elements from opposite directions or when reversing sequences without consuming extra space ($O(1)$ auxiliary space).

* **Example Problem:** Reverse a character array in-place.

```java
public void reverseString(char[] s) {
    int left = 0;
    int right = s.length - 1;
    
    while (left < right) {
        // Swap elements
        char temp = s[left];
        s[left] = s[right];
        s[right] = temp;
        
        // Move pointers closer together
        left++;
        right--;
    }
}

```

### **Pattern 2: Sliding Window (Substring Exploration)**

Used when a problem asks for a contiguous block of characters that satisfies a condition (e.g., "longest substring without repeating characters").

* **Example Problem:** Find the maximum sum of a fixed window size $K$ (represented here as a core sliding framework).

```java
public int findMaxWindowUnique(String s) {
    int maxLen = 0;
    int left = 0;
    HashSet<Character> seen = new HashSet<>();
    
    for (int right = 0; right < s.length(); right++) {
        // Dynamic contraction: shrink window from the left if a duplicate is found
        while (seen.contains(s.charAt(right))) {
            seen.remove(s.charAt(left));
            left++;
        }
        seen.add(s.charAt(right));
        // Window size formula: (right - left + 1)
        maxLen = Math.max(maxLen, right - left + 1);
    }
    return maxLen;
}

```

### **Pattern 3: Frequency Bucket Arrays (Hashing Optimization)**

When a problem specifies that strings contain only lower-case English characters, avoid using a heavy `HashMap`. Use a primitive integer array of size 26 instead to optimize space to $O(1)$.

* **Example Problem:** Find the first unique character in a string.

```java
public int firstUniqChar(String s) {
    int[] count = new int[26];
    
    // Step 1: Count frequencies
    for (int i = 0; i < s.length(); i++) {
        count[s.charAt(i) - 'a']++;
    }
    
    // Step 2: Find the first index with a count of 1
    for (int i = 0; i < s.length(); i++) {
        if (count[s.charAt(i) - 'a'] == 1) {
            return i;
        }
    }
    return -1;
}

```

[![VIEW Topic](https://img.shields.io/badge/🚀-Preview-blue?style=for-the-badge)](https://htmlpreview.github.io/?https://github.com/Tushar-Nagdive/StackBlueprint/blob/StackTech/DSA/views/string-view.html)