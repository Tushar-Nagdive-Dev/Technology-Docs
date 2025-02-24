Here's an enhanced version of the comprehensive list of string-related coding questions in Java, categorized from beginner to advanced levels, designed to cover every concept in minute detail. This version includes a **Core Concepts** section to ensure a solid understanding of string fundamentals, crucial for mastering more complex problems. 

Each question and topic is crafted to enhance your understanding and mastery of strings in Java, ensuring a well-rounded learning experience with explanations, examples, and solutions.

---

## 🟡 Core Concepts: Fundamentals of Strings in Java

Before diving into coding questions, it's crucial to understand the **core concepts** of strings in Java. This will provide the foundation needed to solve even the most complex problems.

---

### 1. **String vs StringBuilder vs StringBuffer**
- **Concepts Covered:** 
  - Immutability of `String`
  - Mutability of `StringBuilder` and `StringBuffer`
  - Thread-safety differences
- **Explanation:**
  - `String` is immutable, meaning its value cannot be changed once created.
  - `StringBuilder` is mutable and faster but **not thread-safe**.
  - `StringBuffer` is mutable and **thread-safe** because its methods are synchronized.
- **Example:**
  ```java
  String str = "Hello";
  StringBuilder sb = new StringBuilder("Hello");
  StringBuffer sbf = new StringBuffer("Hello");
  sb.append(" World");
  sbf.append(" World");
  ```

---

### 2. **String Pool and Memory Management**
- **Concepts Covered:** 
  - String literal vs. `new String()`
  - String Interning
- **Explanation:**
  - String literals are stored in a common pool, saving memory.
  - `new String()` creates a new object in the heap, not in the pool.
- **Example:**
  ```java
  String str1 = "Hello";
  String str2 = "Hello";
  String str3 = new String("Hello");
  System.out.println(str1 == str2); // true
  System.out.println(str1 == str3); // false
  ```

---

### 3. **String Immutability**
- **Concepts Covered:** 
  - Why strings are immutable in Java
  - Security, caching, and performance implications
- **Explanation:**
  - Strings are immutable for security (e.g., preventing changes in URLs), caching (string pool), and performance reasons.
- **Example:**
  ```java
  String str = "Hello";
  str.concat(" World");
  System.out.println(str); // Output: Hello (not Hello World)
  ```

---

### 4. **Common String Methods**
- **Concepts Covered:** 
  - `length()`, `charAt()`, `substring()`, `indexOf()`, `lastIndexOf()`, `toUpperCase()`, `toLowerCase()`, `trim()`, `replace()`, `split()`, `join()`, `contains()`, `startsWith()`, `endsWith()`
- **Example:**
  ```java
  String str = "  Java Programming  ";
  System.out.println(str.trim().toUpperCase()); // Output: JAVA PROGRAMMING
  ```

---

### 5. **String Comparison**
- **Concepts Covered:** 
  - `==` vs `equals()` vs `compareTo()`
  - Case-sensitive and case-insensitive comparison
- **Example:**
  ```java
  String str1 = "Hello";
  String str2 = "hello";
  System.out.println(str1.equals(str2)); // false
  System.out.println(str1.equalsIgnoreCase(str2)); // true
  ```

---

### 6. **String Formatting and Parsing**
- **Concepts Covered:** 
  - `String.format()`, `valueOf()`, `parseInt()`, `parseDouble()`
- **Example:**
  ```java
  int num = 10;
  String formatted = String.format("Number: %d", num);
  System.out.println(formatted); // Output: Number: 10
  ```

---

### 7. **StringBuilder and StringBuffer Methods**
- **Concepts Covered:** 
  - `append()`, `insert()`, `delete()`, `reverse()`, `capacity()`, `ensureCapacity()`
- **Example:**
  ```java
  StringBuilder sb = new StringBuilder("Hello");
  sb.append(" World");
  sb.reverse();
  System.out.println(sb); // Output: dlroW olleH
  ```

---

---

## 🟢 Beginner Level: Basic String Operations

### 1. **Reverse a String**
- **Description:** Write a program to reverse a given string.
- **Concepts Covered:** String manipulation, loops, `StringBuilder.reverse()`.
- **Example Input:** `"hello"`
- **Example Output:** `"olleh"`

---

### 2. **Check if a String is Palindrome**
- **Description:** Determine if the given string is a palindrome.
- **Concepts Covered:** String comparison, case sensitivity.
- **Example Input:** `"Madam"`
- **Example Output:** `true`

---

### 3. **Count Vowels and Consonants in a String**
- **Description:** Count the number of vowels and consonants in a string.
- **Concepts Covered:** Loops, conditionals, character operations.
- **Example Input:** `"hello"`
- **Example Output:** `Vowels: 2, Consonants: 3`

---

---

## 🟠 Intermediate Level: String Manipulation and Pattern Matching

### 6. **Longest Palindromic Substring**
- **Description:** Find the longest palindromic substring in a given string.
- **Concepts Covered:** Two-pointer technique, dynamic programming.
- **Example Input:** `"babad"`
- **Example Output:** `"bab" or "aba"`

---

### 7. **Check if Two Strings are Anagrams**
- **Description:** Determine if two strings are anagrams of each other.
- **Concepts Covered:** Sorting, character count, HashMap.
- **Example Input:** `"listen"`, `"silent"`
- **Example Output:** `true`

---

### 8. **Remove Duplicates from String**
- **Description:** Remove duplicate characters from a string while maintaining the order.
- **Concepts Covered:** Set, StringBuilder.
- **Example Input:** `"programming"`
- **Example Output:** `"progamin"`

---

---

## 🔴 Advanced Level: String Algorithms and Optimization

### 11. **Longest Common Prefix**
- **Description:** Find the longest common prefix among a set of strings.
- **Concepts Covered:** String comparison, edge cases.
- **Example Input:** `["flower", "flow", "flight"]`
- **Example Output:** `"fl"`

---

### 12. **Word Break Problem**
- **Description:** Determine if a string can be segmented into words from a dictionary.
- **Concepts Covered:** Dynamic programming, backtracking.
- **Example Input:** `"applepenapple"`, `["apple", "pen"]`
- **Example Output:** `true`

---

### 13. **Regular Expression Matching**
- **Description:** Implement regular expression matching with support for '.' and '*'.
- **Concepts Covered:** Recursion, dynamic programming.
- **Example Input:** `"aab"`, `"c*a*b"`
- **Example Output:** `true`

---

### 14. **Find the First Non-Repeated Character**
- **Description:** Find the first non-repeated character in a string.
- **Concepts Covered:** LinkedHashMap for maintaining order, frequency count.
- **Example Input:** `"swiss"`
- **Example Output:** `"w"`

---

---

## 🎯 What's Next?

Would you like detailed explanations, code solutions, and step-by-step guides for each question? I can also provide:
- **Test Cases:** For comprehensive testing.
- **Complexity Analysis:** To understand the performance.
- **Common Mistakes and Pitfalls:** To avoid common errors.

---
Let's start with the **Core Concepts** first to build a solid foundation. Once you are comfortable with these, we’ll proceed to the coding questions from Beginner to Advanced levels. 

---

## 🟡 Core Concepts: Fundamentals of Strings in Java

Understanding the core concepts is essential for mastering string-related coding problems. We'll cover the following:

1. **String vs StringBuilder vs StringBuffer**
2. **String Pool and Memory Management**
3. **String Immutability**
4. **Common String Methods**
5. **String Comparison**
6. **String Formatting and Parsing**
7. **StringBuilder and StringBuffer Methods**

---

## 🔹 1. String vs StringBuilder vs StringBuffer

### 🔑 Key Differences:
| Feature               | `String`             | `StringBuilder`         | `StringBuffer`           |
|-----------------------|----------------------|--------------------------|--------------------------|
| **Mutability**        | Immutable             | Mutable                  | Mutable                  |
| **Thread Safety**     | Not thread-safe       | Not thread-safe           | Thread-safe               |
| **Performance**       | Slower (due to immutability) | Faster (no synchronization) | Slower (synchronized)     |
| **Use Case**          | Constant strings      | Single-threaded scenarios | Multi-threaded scenarios  |

---

### 🔍 Detailed Explanation:
1. **String**:
   - Immutable: Once created, it cannot be changed.
   - Every modification (like concatenation) creates a new object in memory.
   - Stored in the **String Pool** for memory optimization.
2. **StringBuilder**:
   - Mutable: Can be modified without creating new objects.
   - More efficient in terms of performance for string manipulation.
   - **Not Thread-Safe** - Use when synchronization is not required.
3. **StringBuffer**:
   - Similar to `StringBuilder`, but **Thread-Safe**.
   - Slower due to synchronized methods.
   - Use in **multi-threaded** scenarios where safety is a concern.

---

### 🔎 Example:

```java
public class StringDemo {
    public static void main(String[] args) {
        // String Example (Immutable)
        String str = "Hello";
        str.concat(" World");
        System.out.println("String: " + str); // Output: Hello (unchanged)
        
        // StringBuilder Example (Mutable and Faster)
        StringBuilder sb = new StringBuilder("Hello");
        sb.append(" World");
        System.out.println("StringBuilder: " + sb); // Output: Hello World
        
        // StringBuffer Example (Thread-Safe)
        StringBuffer sbf = new StringBuffer("Hello");
        sbf.append(" World");
        System.out.println("StringBuffer: " + sbf); // Output: Hello World
    }
}
```

---

### 🧠 Key Takeaways:
- Use **String** when the value doesn't change frequently.
- Use **StringBuilder** for faster performance when concatenation is needed.
- Use **StringBuffer** for thread-safe operations in multi-threaded environments.

---

---

## 🔹 2. String Pool and Memory Management

### 🔑 Key Concepts:
- **String Pool**:
  - A special memory area inside the **Heap** that stores string literals.
  - Ensures memory efficiency by reusing common strings.
- **String Interning**:
  - `intern()` method manually adds a string to the pool.
  - If a string already exists, it returns the reference from the pool.

---

### 🔍 Detailed Explanation:
- When you create a string using a **literal** (e.g., `String str1 = "Hello";`), it checks the pool first:
  - If the string exists, it reuses the reference.
  - If not, it creates a new string in the pool.
- When you use **new keyword** (e.g., `String str2 = new String("Hello");`):
  - It creates a new object in the **Heap**, not in the pool.
  - This can lead to duplicate strings and memory overhead.

---

### 🔎 Example:

```java
public class StringPoolDemo {
    public static void main(String[] args) {
        // Using String Literal (from String Pool)
        String str1 = "Hello";
        String str2 = "Hello";
        System.out.println(str1 == str2); // Output: true (same reference)
        
        // Using new Keyword (Heap Memory)
        String str3 = new String("Hello");
        System.out.println(str1 == str3); // Output: false (different reference)
        
        // Using intern() method
        String str4 = new String("Hello").intern();
        System.out.println(str1 == str4); // Output: true (points to pool)
    }
}
```

---

### 🧠 Key Takeaways:
- Always prefer **string literals** over `new String()` for memory efficiency.
- Use `intern()` when you want to add strings to the pool manually.

---

---

## 🔹 3. String Immutability

### 🔑 Key Concepts:
- **Immutable Nature**:
  - Once a `String` object is created, its value **cannot** be changed.
  - Any operation (e.g., concatenation) creates a new object.
- **Why Immutable?**:
  - **Security**: Prevents changes to sensitive strings (e.g., passwords, URLs).
  - **Caching and Performance**: String pool reuses references, optimizing memory usage.
  - **Thread-Safety**: Immutability makes strings inherently thread-safe.

---

### 🔍 Detailed Explanation:
- When you perform an operation like concatenation, a **new** string object is created.
- The original string remains unchanged, maintaining its immutability.

---

### 🔎 Example:

```java
public class StringImmutability {
    public static void main(String[] args) {
        String str = "Hello";
        str.concat(" World");
        System.out.println("After Concatenation: " + str); // Output: Hello
        
        // Proper way to change the value
        String newStr = str.concat(" World");
        System.out.println("New String: " + newStr); // Output: Hello World
    }
}
```

---

### 🧠 Key Takeaways:
- Strings are **immutable** to maintain security, performance, and thread-safety.
- If modification is required, use **StringBuilder** or **StringBuffer**.

---

---

## 🎯 What's Next?

Would you like to:
1. Proceed with more Core Concepts?
2. Start with **Beginner Level** coding questions?
3. Get detailed explanations, code solutions, and best practices?

---
Let's move to the next set of **Core Concepts** before diving into coding questions. Building a strong foundation is essential for mastering string-related problems.

---

## 🟡 Core Concepts: Continued

Next, we'll cover:

4. **Common String Methods**
5. **String Comparison**
6. **String Formatting and Parsing**
7. **StringBuilder and StringBuffer Methods**

These are crucial for efficiently manipulating strings and solving complex problems.

---

---

## 🔹 4. Common String Methods

Java provides a rich set of methods for string manipulation. Let's explore the most commonly used methods with examples.

---

### 🔑 Key Methods:
- **length()**: Returns the length of the string.
- **charAt(index)**: Returns the character at the specified index.
- **substring(beginIndex, endIndex)**: Extracts a substring.
- **indexOf(char/string)**: Returns the first occurrence index.
- **lastIndexOf(char/string)**: Returns the last occurrence index.
- **toUpperCase()** and **toLowerCase()**: Converts the case.
- **trim()**: Removes leading and trailing whitespaces.
- **replace(oldChar, newChar)**: Replaces characters.
- **split(regex)**: Splits the string based on the regex.
- **join(delimiter, elements)**: Joins elements with a delimiter.
- **contains()**: Checks if a substring is present.
- **startsWith()** and **endsWith()**: Checks the beginning or end.

---

### 🔍 Detailed Explanation:

1. **length() and charAt()**:
   - `length()` returns the number of characters in the string.
   - `charAt(index)` gets the character at the specified index (zero-based).

2. **substring()**:
   - Extracts a portion of the string.
   - `substring(beginIndex, endIndex)` returns a new string from `beginIndex` (inclusive) to `endIndex` (exclusive).

3. **indexOf() and lastIndexOf()**:
   - `indexOf()` finds the first occurrence.
   - `lastIndexOf()` finds the last occurrence.

---

### 🔎 Example:

```java
public class StringMethodsDemo {
    public static void main(String[] args) {
        String str = "  Java Programming  ";

        // Length and charAt()
        System.out.println("Length: " + str.length()); // Output: 21
        System.out.println("Character at index 5: " + str.charAt(5)); // Output: P
        
        // Substring
        String sub = str.substring(2, 6);
        System.out.println("Substring: " + sub); // Output: Java
        
        // IndexOf and LastIndexOf
        System.out.println("Index of 'a': " + str.indexOf('a')); // Output: 3
        System.out.println("Last Index of 'a': " + str.lastIndexOf('a')); // Output: 16
        
        // Trim and Case Conversion
        String trimmed = str.trim();
        System.out.println("Trimmed: " + trimmed); // Output: Java Programming
        System.out.println("Uppercase: " + trimmed.toUpperCase()); // Output: JAVA PROGRAMMING
        
        // Replace and Split
        String replaced = str.replace("Java", "Python");
        System.out.println("Replaced: " + replaced); // Output: Python Programming
        String[] words = trimmed.split(" ");
        System.out.println("Words: " + String.join(", ", words)); // Output: Java, Programming
        
        // Join and Contains
        String joined = String.join("-", words);
        System.out.println("Joined: " + joined); // Output: Java-Programming
        System.out.println("Contains 'Java': " + trimmed.contains("Java")); // Output: true
        
        // StartsWith and EndsWith
        System.out.println("Starts with 'Java': " + trimmed.startsWith("Java")); // Output: true
        System.out.println("Ends with 'ing': " + trimmed.endsWith("ing")); // Output: true
    }
}
```

---

### 🧠 Key Takeaways:
- `substring()` and `indexOf()` are widely used in substring extraction and search problems.
- `trim()` is useful for cleaning up input strings.
- `split()` and `join()` are essential for processing CSV-like strings.

---

---

## 🔹 5. String Comparison

### 🔑 Key Methods:
- **==**: Checks reference equality.
- **equals()**: Checks value equality (case-sensitive).
- **equalsIgnoreCase()**: Checks value equality (case-insensitive).
- **compareTo()**: Lexicographical comparison.
- **compareToIgnoreCase()**: Case-insensitive comparison.

---

### 🔍 Detailed Explanation:
- `==` compares references, not content.
- `equals()` compares the content of two strings.
- `compareTo()` returns:
  - `0` if equal
  - Negative if the calling string is lexicographically less
  - Positive if the calling string is greater

---

### 🔎 Example:

```java
public class StringComparison {
    public static void main(String[] args) {
        String str1 = "Hello";
        String str2 = "Hello";
        String str3 = new String("Hello");
        String str4 = "hello";

        // == Reference Comparison
        System.out.println(str1 == str2); // Output: true (same reference)
        System.out.println(str1 == str3); // Output: false (different objects)

        // equals() Value Comparison
        System.out.println(str1.equals(str2)); // Output: true
        System.out.println(str1.equals(str4)); // Output: false

        // equalsIgnoreCase()
        System.out.println(str1.equalsIgnoreCase(str4)); // Output: true

        // compareTo()
        System.out.println(str1.compareTo(str2)); // Output: 0
        System.out.println(str1.compareTo(str4)); // Output: -32

        // compareToIgnoreCase()
        System.out.println(str1.compareToIgnoreCase(str4)); // Output: 0
    }
}
```

---

### 🧠 Key Takeaways:
- Use `equals()` for content comparison.
- Use `==` only for reference checks.
- `compareTo()` is useful for sorting strings.

---

---

## 🔹 6. String Formatting and Parsing

### 🔑 Key Methods:
- **String.format()**: Formats strings using placeholders.
- **valueOf()**: Converts primitive to string.
- **parseInt()**, **parseDouble()**: Parses strings to numbers.

---

### 🔎 Example:

```java
public class StringFormatting {
    public static void main(String[] args) {
        int age = 25;
        double salary = 5432.50;
        
        // String.format()
        String formatted = String.format("Age: %d, Salary: %.2f", age, salary);
        System.out.println(formatted); // Output: Age: 25, Salary: 5432.50
        
        // valueOf()
        String strNum = String.valueOf(age);
        System.out.println("String Value: " + strNum); // Output: 25
        
        // Parsing
        int num = Integer.parseInt("123");
        double decimal = Double.parseDouble("12.34");
        System.out.println("Parsed Integer: " + num); // Output: 123
        System.out.println("Parsed Double: " + decimal); // Output: 12.34
    }
}
```

---

### 🧠 Key Takeaways:
- `String.format()` is powerful for custom outputs.
- `valueOf()` is preferred for primitive-to-string conversion.
- `parseInt()` and `parseDouble()` are essential for input parsing.

---

---

## 🎯 What's Next?

Would you like to:
1. Continue with **StringBuilder and StringBuffer Methods**?
2. Start with **Beginner Level Coding Questions**?
3. Get detailed explanations, code solutions, and best practices?

---

Let's complete the last part of the **Core Concepts** before diving into the coding questions.

---

## 🔹 7. StringBuilder and StringBuffer Methods

### 🔑 Key Concepts:
- **StringBuilder** and **StringBuffer** are used for mutable strings.
- `StringBuilder` is **not thread-safe** but faster.
- `StringBuffer` is **thread-safe** but slower due to synchronized methods.
- Both provide efficient methods for string manipulation without creating new objects.

---

### 🔑 Key Methods:
- **append()**: Adds text at the end.
- **insert(index, text)**: Inserts text at the specified index.
- **delete(startIndex, endIndex)**: Deletes characters from start to end-1.
- **replace(startIndex, endIndex, text)**: Replaces characters from start to end-1.
- **reverse()**: Reverses the character sequence.
- **capacity()**: Returns the current capacity of the builder/buffer.
- **ensureCapacity(minCapacity)**: Ensures minimum capacity.
- **setLength(newLength)**: Sets the length of the sequence.

---

### 🔍 Detailed Explanation:

1. **append()**:
   - Efficiently appends text without creating new objects.
   - Works similarly to `+=` but faster for multiple concatenations.

2. **insert()**:
   - Inserts text at the specified position.
   - Shifts the existing text to the right.

3. **delete()** and **replace()**:
   - `delete()` removes characters from start to end-1.
   - `replace()` replaces the specified range with new text.

4. **reverse()**:
   - Reverses the entire sequence.

5. **capacity() and ensureCapacity()**:
   - `capacity()` shows the current buffer size.
   - `ensureCapacity()` ensures that the buffer can accommodate additional characters.

---

### 🔎 Example:

```java
public class StringBuilderBufferDemo {
    public static void main(String[] args) {
        // StringBuilder Example
        StringBuilder sb = new StringBuilder("Hello");
        sb.append(" World");
        System.out.println("StringBuilder Append: " + sb); // Output: Hello World
        
        sb.insert(6, "Beautiful ");
        System.out.println("StringBuilder Insert: " + sb); // Output: Hello Beautiful World
        
        sb.delete(6, 16);
        System.out.println("StringBuilder Delete: " + sb); // Output: Hello World
        
        sb.replace(6, 11, "Java");
        System.out.println("StringBuilder Replace: " + sb); // Output: Hello Java
        
        sb.reverse();
        System.out.println("StringBuilder Reverse: " + sb); // Output: avaJ olleH
        
        // StringBuffer Example
        StringBuffer sbf = new StringBuffer("Hello");
        sbf.append(" World");
        System.out.println("StringBuffer Append: " + sbf); // Output: Hello World
        
        sbf.setLength(5);
        System.out.println("StringBuffer Set Length: " + sbf); // Output: Hello
        
        // Capacity and Ensure Capacity
        System.out.println("Initial Capacity: " + sb.capacity()); // Output: 16 (or more)
        sb.ensureCapacity(50);
        System.out.println("New Capacity: " + sb.capacity()); // Output: >= 50
    }
}
```

---

### 🧠 Key Takeaways:
- `StringBuilder` is faster but not thread-safe.
- `StringBuffer` is slower but thread-safe.
- Use `append()` and `insert()` for efficient modifications.
- `capacity()` and `ensureCapacity()` optimize performance by reducing memory allocations.

---

---

## 🟢 Moving to Beginner Level: Basic String Operations

Now that we’ve covered all the core concepts, let's start with **Beginner Level Coding Questions**. These questions are designed to reinforce your understanding of the basics while introducing you to practical applications.

---

---

## 🟢 Beginner Level: Basic String Operations

1. **Reverse a String** ✅
2. **Check if a String is Palindrome** ✅
3. **Count Vowels and Consonants in a String** ✅
4. **Remove White Spaces from a String**
5. **Find Duplicate Characters in a String**

Let's start with the first question.

---

---

## 🟢 1. Reverse a String

### 🔑 Problem Description:
Write a program to **reverse** a given string.

- **Input:** A string `str`.
- **Output:** The reversed string.

---

### 🔍 Concepts Covered:
- String manipulation
- Loops and iteration
- Using `StringBuilder` for efficient reversal

---

### 🔔 Example:
```java
Input: "hello"
Output: "olleh"
```

---

### 💡 Approach:

1. **Using a Loop:**
   - Iterate from the end of the string to the beginning.
   - Append each character to a new string.

2. **Using StringBuilder's reverse() Method:**
   - `StringBuilder` provides a built-in `reverse()` method.
   - This is more efficient due to its mutable nature.

---

### 🔎 Solution 1: Using Loop

```java
public class ReverseStringLoop {
    public static void main(String[] args) {
        String str = "hello";
        String reversed = "";
        
        // Loop from the end to the beginning
        for (int i = str.length() - 1; i >= 0; i--) {
            reversed += str.charAt(i);
        }
        
        System.out.println("Reversed String (Loop): " + reversed);
    }
}
```

---

### 🔎 Solution 2: Using StringBuilder

```java
public class ReverseStringBuilder {
    public static void main(String[] args) {
        String str = "hello";
        
        // Using StringBuilder
        StringBuilder sb = new StringBuilder(str);
        String reversed = sb.reverse().toString();
        
        System.out.println("Reversed String (StringBuilder): " + reversed);
    }
}
```

---

### 📊 Complexity Analysis:
- **Time Complexity:** `O(n)` for both solutions (`n` = length of string)
- **Space Complexity:** `O(n)` for storing the reversed string

---

### 🔥 Test Cases:
```java
Input: "hello"
Output: "olleh"

Input: "Java"
Output: "avaJ"

Input: "A"
Output: "A"

Input: ""
Output: ""

Input: "madam"
Output: "madam"
```

---

### 💡 Best Practices:
- Use `StringBuilder` for efficient string manipulation.
- Handle edge cases like empty strings or single-character strings.

---

### ✅ Completed:
- [x] Core Concepts
- [x] Reverse a String

---

---

## 🎯 What's Next?

Would you like to:
1. Move to the next question: **Check if a String is Palindrome**?
2. Get more detailed explanations or variations for this problem?
3. Proceed with more beginner-level questions?

---

Let's move to the next beginner-level question.

---

---

## 🟢 2. Check if a String is Palindrome

### 🔑 Problem Description:
Write a program to **check if a given string is a palindrome**. A palindrome is a word, phrase, number, or other sequences of characters that reads the same forward and backward, ignoring case, spaces, and punctuation.

- **Input:** A string `str`.
- **Output:** `true` if the string is a palindrome, `false` otherwise.

---

### 🔍 Concepts Covered:
- String comparison
- String manipulation (ignoring case and spaces)
- Using `StringBuilder` for reversal
- Two-pointer technique for optimal solution

---

### 🔔 Example:
```java
Input: "madam"
Output: true

Input: "racecar"
Output: true

Input: "Hello"
Output: false

Input: "A man a plan a canal Panama"
Output: true (ignoring spaces and case)
```

---

### 💡 Approach:

1. **Using StringBuilder Reverse Method:**
   - Reverse the string using `StringBuilder`.
   - Compare the reversed string with the original.

2. **Two-Pointer Technique (Optimal Solution):**
   - Use two pointers, one starting from the beginning and one from the end.
   - Compare characters while ignoring case and spaces.
   - Move pointers towards the center.
   - Stop when they meet or cross.

---

---

## 🔎 Solution 1: Using StringBuilder

This solution is simple and leverages `StringBuilder`'s `reverse()` method. However, it consumes more memory because it creates a reversed copy of the string.

```java
public class PalindromeString {
    public static void main(String[] args) {
        String str = "A man a plan a canal Panama";
        
        // Step 1: Clean the input (ignore case and spaces)
        String cleaned = str.replaceAll("[\\W_]", "").toLowerCase();
        
        // Step 2: Reverse the cleaned string
        String reversed = new StringBuilder(cleaned).reverse().toString();
        
        // Step 3: Compare original and reversed strings
        boolean isPalindrome = cleaned.equals(reversed);
        
        System.out.println("Is Palindrome: " + isPalindrome);
    }
}
```

---

### 🧠 Explanation:
1. **Cleaning the String:**
   - `replaceAll("[\\W_]", "")` removes all non-word characters (including spaces and punctuation).
   - `toLowerCase()` ignores case by converting all characters to lower case.

2. **Reversing the String:**
   - `StringBuilder(cleaned).reverse()` efficiently reverses the cleaned string.

3. **Comparing the Strings:**
   - `equals()` checks if the cleaned string is equal to its reversed version.

---

### 📊 Complexity Analysis:
- **Time Complexity:** `O(n)` for reversing and comparing (`n` = length of the cleaned string)
- **Space Complexity:** `O(n)` for storing the reversed string

---

---

## 🔎 Solution 2: Using Two-Pointer Technique (Optimal Solution)

This approach is more memory efficient as it doesn't create a reversed copy. It uses two pointers to compare characters from both ends.

```java
public class PalindromeStringTwoPointer {
    public static void main(String[] args) {
        String str = "A man a plan a canal Panama";
        
        // Step 1: Clean the input (ignore case and spaces)
        String cleaned = str.replaceAll("[\\W_]", "").toLowerCase();
        
        // Step 2: Initialize pointers
        int left = 0;
        int right = cleaned.length() - 1;
        
        // Step 3: Two-pointer comparison
        while (left < right) {
            if (cleaned.charAt(left) != cleaned.charAt(right)) {
                System.out.println("Is Palindrome: false");
                return;
            }
            left++;
            right--;
        }
        
        System.out.println("Is Palindrome: true");
    }
}
```

---

### 🧠 Explanation:
1. **Initialize Two Pointers:**
   - `left` starts from the beginning (`0`).
   - `right` starts from the end (`length - 1`).

2. **Compare and Move Pointers:**
   - Compare characters at the `left` and `right` pointers.
   - If they are not equal, print `false` and exit.
   - If they are equal, move `left` pointer to the right (`++`) and `right` pointer to the left (`--`).

3. **Check Until They Meet:**
   - Continue until `left` meets or crosses `right`.

---

### 📊 Complexity Analysis:
- **Time Complexity:** `O(n)` since we are comparing characters from both ends.
- **Space Complexity:** `O(1)` as no extra space proportional to input size is used.

---

---

## 🔥 Test Cases:
```java
Input: "madam"
Output: true

Input: "racecar"
Output: true

Input: "Hello"
Output: false

Input: "A man a plan a canal Panama"
Output: true

Input: "No lemon, no melon"
Output: true
```

---

### 💡 Best Practices:
- Use **Two-Pointer Technique** for optimal performance.
- Consider edge cases like empty strings and single-character strings.
- Normalize input by ignoring spaces, punctuation, and case.

---

### ✅ Completed:
- [x] Core Concepts
- [x] Reverse a String
- [x] Check if a String is Palindrome

---

---

## 🎯 What's Next?

Would you like to:
1. Move to the next question: **Count Vowels and Consonants in a String**?
2. Get more detailed explanations or variations for this problem?
3. Proceed with more beginner-level questions?

---
Let's move to the next beginner-level question.

---

---

## 🟢 3. Count Vowels and Consonants in a String

### 🔑 Problem Description:
Write a program to **count the number of vowels and consonants** in a given string.

- **Input:** A string `str`.
- **Output:** Number of vowels and consonants in the string.

---

### 🔍 Concepts Covered:
- Character classification (vowels and consonants)
- String manipulation (ignoring case and non-alphabet characters)
- Loops and conditionals

---

### 🔔 Example:
```java
Input: "hello"
Output: Vowels: 2, Consonants: 3

Input: "JAVA"
Output: Vowels: 2, Consonants: 2

Input: "Programming"
Output: Vowels: 3, Consonants: 8

Input: "123 hello !!"
Output: Vowels: 2, Consonants: 3
```

---

### 💡 Approach:

1. **Normalize the String:**
   - Convert the string to lower case for case insensitivity.
   - Remove all non-alphabet characters using regex.

2. **Loop through Characters:**
   - Check each character if it's a vowel or consonant.
   - Use `if-else` or `switch-case` for classification.

3. **Count Vowels and Consonants:**
   - Increment the vowel counter if the character is a vowel.
   - Otherwise, increment the consonant counter.

---

---

## 🔎 Solution: Using Loop and Conditionals

This solution uses a loop to iterate through each character and conditionals to classify them as vowels or consonants.

```java
public class CountVowelsConsonants {
    public static void main(String[] args) {
        String str = "123 hello !!";
        
        // Step 1: Normalize the string
        str = str.toLowerCase().replaceAll("[^a-z]", "");
        
        // Step 2: Initialize counters
        int vowelCount = 0;
        int consonantCount = 0;
        
        // Step 3: Loop through characters
        for (int i = 0; i < str.length(); i++) {
            char ch = str.charAt(i);
            
            // Step 4: Check for vowels
            if (ch == 'a' || ch == 'e' || ch == 'i' || ch == 'o' || ch == 'u') {
                vowelCount++;
            } else {
                consonantCount++;
            }
        }
        
        // Step 5: Print the counts
        System.out.println("Vowels: " + vowelCount);
        System.out.println("Consonants: " + consonantCount);
    }
}
```

---

### 🧠 Explanation:
1. **Normalize the String:**
   - `toLowerCase()` makes the comparison case insensitive.
   - `replaceAll("[^a-z]", "")` removes all non-alphabet characters, including numbers and punctuation.

2. **Loop through Characters:**
   - Use a `for` loop to iterate over each character.
   - `charAt(i)` gets the character at index `i`.

3. **Classify as Vowel or Consonant:**
   - `if` checks if the character is a vowel (`a, e, i, o, u`).
   - If not, it's counted as a consonant.

4. **Count and Display:**
   - Increment the respective counter (`vowelCount` or `consonantCount`).
   - Print the final counts.

---

### 📊 Complexity Analysis:
- **Time Complexity:** `O(n)` where `n` is the length of the string
- **Space Complexity:** `O(1)` since only counters are used

---

---

## 🔥 Test Cases:
```java
Input: "hello"
Output: Vowels: 2, Consonants: 3

Input: "JAVA"
Output: Vowels: 2, Consonants: 2

Input: "Programming"
Output: Vowels: 3, Consonants: 8

Input: "123 hello !!"
Output: Vowels: 2, Consonants: 3

Input: "AEIOUaeiou"
Output: Vowels: 10, Consonants: 0
```

---

### 💡 Best Practices:
- Use `toLowerCase()` to avoid repetitive case checks.
- Use `replaceAll("[^a-z]", "")` to clean the input.
- Optimize by using `switch-case` or `contains()` for checking vowels.

---

### ✅ Completed:
- [x] Core Concepts
- [x] Reverse a String
- [x] Check if a String is Palindrome
- [x] Count Vowels and Consonants in a String

---

---

## 🎯 What's Next?

Would you like to:
1. Move to the next question: **Remove White Spaces from a String**?
2. Get more detailed explanations or variations for this problem?
3. Proceed with more beginner-level questions?

---
Let's move to the next beginner-level question.

---

---

## 🟢 4. Remove White Spaces from a String

### 🔑 Problem Description:
Write a program to **remove all white spaces** from a given string.

- **Input:** A string `str`.
- **Output:** The string without any white spaces.

---

### 🔍 Concepts Covered:
- String manipulation
- Using `replaceAll()` with regular expressions
- Using `StringBuilder` for efficient removal

---

### 🔔 Example:
```java
Input: " h e l l o "
Output: "hello"

Input: " J A V A   P R O G R A M M I N G "
Output: "JAVAPROGRAMMING"

Input: "   Remove   all    spaces   "
Output: "Removeallspaces"
```

---

### 💡 Approach:

1. **Using replaceAll() Method:**
   - Use `replaceAll("\\s+", "")` to remove all white spaces.
   - `\\s` matches any whitespace character (spaces, tabs, newlines).
   - `+` matches one or more occurrences.

2. **Using Loop and StringBuilder (Optimal Solution):**
   - Iterate through each character.
   - Skip spaces and append non-space characters to `StringBuilder`.

---

---

## 🔎 Solution 1: Using replaceAll() Method

This solution uses a regular expression to remove all white spaces in a single line. It is concise and easy to read.

```java
public class RemoveWhiteSpaces {
    public static void main(String[] args) {
        String str = "   Remove   all    spaces   ";
        
        // Step 1: Remove all white spaces using replaceAll()
        String noSpaces = str.replaceAll("\\s+", "");
        
        // Step 2: Print the result
        System.out.println("String without spaces: " + noSpaces);
    }
}
```

---

### 🧠 Explanation:
1. **Using replaceAll() Method:**
   - `\\s+` is a regular expression that matches one or more white spaces.
   - `replaceAll("\\s+", "")` replaces all white spaces with an empty string.

2. **Efficient and Readable:**
   - This solution is both concise and efficient for most use cases.

---

### 📊 Complexity Analysis:
- **Time Complexity:** `O(n)` where `n` is the length of the string
- **Space Complexity:** `O(n)` for storing the result

---

---

## 🔎 Solution 2: Using Loop and StringBuilder (Optimal Solution)

This solution manually iterates through each character and appends non-space characters to a `StringBuilder`. It is more memory-efficient.

```java
public class RemoveWhiteSpacesBuilder {
    public static void main(String[] args) {
        String str = "   Remove   all    spaces   ";
        
        // Step 1: Initialize StringBuilder
        StringBuilder sb = new StringBuilder();
        
        // Step 2: Loop through each character
        for (int i = 0; i < str.length(); i++) {
            char ch = str.charAt(i);
            // Step 3: Append non-space characters
            if (!Character.isWhitespace(ch)) {
                sb.append(ch);
            }
        }
        
        // Step 4: Convert StringBuilder to String
        String noSpaces = sb.toString();
        
        // Step 5: Print the result
        System.out.println("String without spaces: " + noSpaces);
    }
}
```

---

### 🧠 Explanation:
1. **Using StringBuilder:**
   - `StringBuilder` is used for efficient string manipulation.
   - It avoids creating multiple immutable string objects.

2. **Character Checking:**
   - `Character.isWhitespace(ch)` checks if the character is a white space.
   - Non-space characters are appended to the `StringBuilder`.

3. **Convert to String:**
   - `sb.toString()` converts the `StringBuilder` to a `String`.

---

### 📊 Complexity Analysis:
- **Time Complexity:** `O(n)` as it iterates through the string once
- **Space Complexity:** `O(n)` for the `StringBuilder`

---

---

## 🔥 Test Cases:
```java
Input: " h e l l o "
Output: "hello"

Input: " J A V A   P R O G R A M M I N G "
Output: "JAVAPROGRAMMING"

Input: "   Remove   all    spaces   "
Output: "Removeallspaces"

Input: "NoSpacesHere"
Output: "NoSpacesHere"

Input: ""
Output: ""
```

---

### 💡 Best Practices:
- Use `replaceAll()` for quick and concise solutions.
- Use `StringBuilder` for optimal performance when memory efficiency is a concern.
- Handle edge cases like empty strings and strings without spaces.

---

### ✅ Completed:
- [x] Core Concepts
- [x] Reverse a String
- [x] Check if a String is Palindrome
- [x] Count Vowels and Consonants in a String
- [x] Remove White Spaces from a String

---

---

## 🎯 What's Next?

Would you like to:
1. Move to the next question: **Find Duplicate Characters in a String**?
2. Get more detailed explanations or variations for this problem?
3. Proceed with more beginner-level questions?

---
Let's move to the next beginner-level question.

---

---

## 🟢 5. Find Duplicate Characters in a String

### 🔑 Problem Description:
Write a program to **find all duplicate characters** in a given string.

- **Input:** A string `str`.
- **Output:** A list of duplicate characters and their counts.

---

### 🔍 Concepts Covered:
- Character frequency counting
- Using `HashMap` for counting occurrences
- Iteration and conditionals

---

### 🔔 Example:
```java
Input: "programming"
Output: g: 2, r: 2, m: 2

Input: "Java"
Output: a: 2

Input: "hello world"
Output: l: 3, o: 2

Input: "abcdef"
Output: No duplicates
```

---

### 💡 Approach:

1. **Using HashMap:**
   - Use a `HashMap<Character, Integer>` to count occurrences of each character.
   - Characters are keys, and their counts are values.
   - If a character is repeated, increment its count.

2. **Loop through Characters:**
   - Iterate over the string using a loop.
   - Convert to lower case to ignore case sensitivity.
   - Skip spaces and special characters.

3. **Identify Duplicates:**
   - Traverse the HashMap to find characters with counts greater than 1.
   - Print the characters and their counts.

---

---

## 🔎 Solution: Using HashMap

This solution uses a `HashMap` to efficiently count character occurrences. It then checks for duplicates by iterating over the map entries.

```java
import java.util.HashMap;
import java.util.Map;
import java.util.Set;

public class FindDuplicateCharacters {
    public static void main(String[] args) {
        String str = "hello world";
        
        // Step 1: Convert to lowercase and remove non-alphabet characters
        str = str.toLowerCase().replaceAll("[^a-z]", "");
        
        // Step 2: Use HashMap to store character counts
        Map<Character, Integer> charCountMap = new HashMap<>();
        
        // Step 3: Count occurrences of each character
        for (int i = 0; i < str.length(); i++) {
            char ch = str.charAt(i);
            charCountMap.put(ch, charCountMap.getOrDefault(ch, 0) + 1);
        }
        
        // Step 4: Identify and print duplicate characters
        System.out.println("Duplicate characters:");
        boolean hasDuplicates = false;
        
        Set<Map.Entry<Character, Integer>> entrySet = charCountMap.entrySet();
        for (Map.Entry<Character, Integer> entry : entrySet) {
            if (entry.getValue() > 1) {
                System.out.println(entry.getKey() + ": " + entry.getValue());
                hasDuplicates = true;
            }
        }
        
        if (!hasDuplicates) {
            System.out.println("No duplicates found.");
        }
    }
}
```

---

### 🧠 Explanation:
1. **Normalize the String:**
   - `toLowerCase()` ensures case insensitivity.
   - `replaceAll("[^a-z]", "")` removes non-alphabet characters.

2. **Using HashMap for Counting:**
   - `HashMap<Character, Integer>` stores character frequencies.
   - `getOrDefault(ch, 0)` initializes the count if the character is not present.

3. **Finding Duplicates:**
   - Iterate over the `HashMap` entries.
   - Check for values greater than 1 to identify duplicates.

4. **Output Handling:**
   - Print duplicate characters and their counts.
   - If no duplicates are found, print "No duplicates found."

---

### 📊 Complexity Analysis:
- **Time Complexity:** `O(n)` where `n` is the length of the string
- **Space Complexity:** `O(k)` where `k` is the number of unique characters

---

---

## 🔥 Test Cases:
```java
Input: "programming"
Output: g: 2, r: 2, m: 2

Input: "Java"
Output: a: 2

Input: "hello world"
Output: l: 3, o: 2

Input: "abcdef"
Output: No duplicates

Input: "AABBCC"
Output: a: 2, b: 2, c: 2
```

---

### 💡 Best Practices:
- Use `HashMap` for efficient counting and lookups.
- Normalize input by converting to lower case.
- Handle edge cases like empty strings and strings without duplicates.

---

### ✅ Completed:
- [x] Core Concepts
- [x] Reverse a String
- [x] Check if a String is Palindrome
- [x] Count Vowels and Consonants in a String
- [x] Remove White Spaces from a String
- [x] Find Duplicate Characters in a String

---

---

## 🎯 What's Next?

Would you like to:
1. Move to **Intermediate Level** questions?
2. Get more detailed explanations or variations for this problem?
3. Proceed with more string-related questions?

---
Let's move to the **Intermediate Level** questions.

---

---

## 🟠 Intermediate Level: String Manipulation and Pattern Matching

Here are the questions we'll cover at this level:

1. **Longest Palindromic Substring** ✅
2. **Check if Two Strings are Anagrams** ✅
3. **Remove Duplicates from String** ✅
4. **Find All Substrings of a String**
5. **String Rotation Check**

Let's start with the first question.

---

---

## 🟠 1. Longest Palindromic Substring

### 🔑 Problem Description:
Write a program to **find the longest palindromic substring** in a given string.

- **Input:** A string `str`.
- **Output:** The longest palindromic substring.

---

### 🔍 Concepts Covered:
- Two-pointer technique
- Dynamic programming (optional, for optimal solution)
- String manipulation

---

### 🔔 Example:
```java
Input: "babad"
Output: "bab" or "aba"

Input: "cbbd"
Output: "bb"

Input: "a"
Output: "a"

Input: "ac"
Output: "a" or "c"
```

---

### 💡 Approach:

1. **Two-Pointer (Expand Around Center) Method:**
   - Every palindrome is mirrored around its center.
   - There are two types of centers:
     - Single character center (odd length palindrome)
     - Two character center (even length palindrome)
   - Expand outwards from each center to find the longest palindrome.

2. **Dynamic Programming (Optional - Optimal Solution):**
   - Use a 2D boolean array to store palindrome states.
   - `dp[i][j]` is `true` if substring `str[i...j]` is a palindrome.
   - Build the solution from smaller substrings to larger ones.

---

---

## 🔎 Solution: Expand Around Center (Two-Pointer)

This solution expands around each character (and pair of characters) to find the longest palindromic substring. It is more efficient than dynamic programming for this problem.

```java
public class LongestPalindromicSubstring {
    
    // Function to find the longest palindromic substring
    public static String longestPalindrome(String s) {
        if (s == null || s.length() < 1) return "";
        
        int start = 0, end = 0;
        
        // Loop through each character and expand around it
        for (int i = 0; i < s.length(); i++) {
            // Case 1: Single character center
            int len1 = expandAroundCenter(s, i, i);
            // Case 2: Two character center
            int len2 = expandAroundCenter(s, i, i + 1);
            
            // Find the maximum length
            int len = Math.max(len1, len2);
            
            // Update the start and end indices of the longest palindrome
            if (len > end - start) {
                start = i - (len - 1) / 2;
                end = i + len / 2;
            }
        }
        
        return s.substring(start, end + 1);
    }
    
    // Helper function to expand around the center
    private static int expandAroundCenter(String s, int left, int right) {
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }
        return right - left - 1;
    }
    
    public static void main(String[] args) {
        String str = "babad";
        System.out.println("Longest Palindromic Substring: " + longestPalindrome(str));
        
        str = "cbbd";
        System.out.println("Longest Palindromic Substring: " + longestPalindrome(str));
        
        str = "a";
        System.out.println("Longest Palindromic Substring: " + longestPalindrome(str));
        
        str = "ac";
        System.out.println("Longest Palindromic Substring: " + longestPalindrome(str));
    }
}
```

---

### 🧠 Explanation:
1. **Expand Around Center:**
   - For each character and pair of characters, expand outward while the characters are the same.
   - This covers both odd and even length palindromes.

2. **Calculate Length:**
   - The length of the palindrome is calculated as `right - left - 1`.

3. **Update Longest Palindrome:**
   - Update the `start` and `end` indices if a longer palindrome is found.

4. **Return Result:**
   - Return the substring from `start` to `end`.

---

### 📊 Complexity Analysis:
- **Time Complexity:** `O(n^2)` due to the two-pointer expansion
- **Space Complexity:** `O(1)` as no additional space proportional to input size is used

---

---

## 🔥 Test Cases:
```java
Input: "babad"
Output: "bab" or "aba"

Input: "cbbd"
Output: "bb"

Input: "a"
Output: "a"

Input: "ac"
Output: "a" or "c"

Input: "forgeeksskeegfor"
Output: "geeksskeeg"
```

---

### 💡 Best Practices:
- Use the **Expand Around Center** method for optimal performance.
- Consider both odd and even length palindromes.
- Handle edge cases like empty strings and single-character strings.

---

### ✅ Completed:
- [x] Core Concepts
- [x] Reverse a String
- [x] Check if a String is Palindrome
- [x] Count Vowels and Consonants in a String
- [x] Remove White Spaces from a String
- [x] Find Duplicate Characters in a String
- [x] Longest Palindromic Substring

---

---

## 🎯 What's Next?

Would you like to:
1. Move to the next question: **Check if Two Strings are Anagrams**?
2. Get more detailed explanations or variations for this problem?
3. Proceed with more intermediate-level questions?

---

Let's move to the next intermediate-level question.

---

---

## 🟠 2. Check if Two Strings are Anagrams

### 🔑 Problem Description:
Write a program to **check if two strings are anagrams** of each other. Two strings are anagrams if they contain the same characters with the same frequency, but in a different order.

- **Input:** Two strings `str1` and `str2`.
- **Output:** `true` if the strings are anagrams, `false` otherwise.

---

### 🔍 Concepts Covered:
- Sorting and comparison
- Frequency counting using `HashMap`
- Edge case handling (case sensitivity and white spaces)

---

### 🔔 Example:
```java
Input: "listen", "silent"
Output: true

Input: "triangle", "integral"
Output: true

Input: "apple", "pale"
Output: false

Input: "Dormitory", "Dirty room"
Output: true (ignoring case and spaces)
```

---

### 💡 Approach:

1. **Sorting Method:**
   - Sort both strings and compare them.
   - If they are equal after sorting, they are anagrams.

2. **Frequency Counting (Optimal Solution):**
   - Count the frequency of each character using a `HashMap`.
   - Compare the character counts of both strings.

3. **Edge Case Handling:**
   - Ignore case by converting both strings to lower case.
   - Ignore white spaces using `replaceAll()`.

---

---

## 🔎 Solution 1: Using Sorting

This solution sorts both strings and compares them. It is simple to implement but slower due to the sorting operation.

```java
import java.util.Arrays;

public class AnagramCheckSorting {
    public static boolean areAnagrams(String str1, String str2) {
        // Step 1: Normalize strings (lowercase and remove spaces)
        str1 = str1.toLowerCase().replaceAll("\\s+", "");
        str2 = str2.toLowerCase().replaceAll("\\s+", "");
        
        // Step 2: If lengths are not equal, they can't be anagrams
        if (str1.length() != str2.length()) {
            return false;
        }
        
        // Step 3: Convert to character arrays and sort
        char[] charArray1 = str1.toCharArray();
        char[] charArray2 = str2.toCharArray();
        
        Arrays.sort(charArray1);
        Arrays.sort(charArray2);
        
        // Step 4: Compare sorted arrays
        return Arrays.equals(charArray1, charArray2);
    }
    
    public static void main(String[] args) {
        String str1 = "Listen";
        String str2 = "Silent";
        
        System.out.println("Are Anagrams: " + areAnagrams(str1, str2));
        
        str1 = "Triangle";
        str2 = "Integral";
        System.out.println("Are Anagrams: " + areAnagrams(str1, str2));
        
        str1 = "Apple";
        str2 = "Pale";
        System.out.println("Are Anagrams: " + areAnagrams(str1, str2));
        
        str1 = "Dormitory";
        str2 = "Dirty room";
        System.out.println("Are Anagrams: " + areAnagrams(str1, str2));
    }
}
```

---

### 🧠 Explanation:
1. **Normalize Strings:**
   - `toLowerCase()` makes the comparison case insensitive.
   - `replaceAll("\\s+", "")` removes all white spaces.

2. **Length Check:**
   - If lengths are not equal, return `false` immediately.

3. **Sort and Compare:**
   - Convert the strings to character arrays.
   - Sort both arrays and compare them using `Arrays.equals()`.

---

### 📊 Complexity Analysis:
- **Time Complexity:** `O(n log n)` due to sorting (`n` is the length of the strings)
- **Space Complexity:** `O(n)` for the character arrays

---

---

## 🔎 Solution 2: Using Frequency Count (Optimal Solution)

This solution uses a `HashMap` to count the frequency of each character. It is more efficient than sorting.

```java
import java.util.HashMap;
import java.util.Map;

public class AnagramCheckHashMap {
    public static boolean areAnagrams(String str1, String str2) {
        // Step 1: Normalize strings (lowercase and remove spaces)
        str1 = str1.toLowerCase().replaceAll("\\s+", "");
        str2 = str2.toLowerCase().replaceAll("\\s+", "");
        
        // Step 2: If lengths are not equal, they can't be anagrams
        if (str1.length() != str2.length()) {
            return false;
        }
        
        // Step 3: Use HashMap to count character frequencies
        Map<Character, Integer> charCountMap = new HashMap<>();
        
        // Count characters for str1
        for (char ch : str1.toCharArray()) {
            charCountMap.put(ch, charCountMap.getOrDefault(ch, 0) + 1);
        }
        
        // Count characters for str2
        for (char ch : str2.toCharArray()) {
            // If character not present or count is zero, not an anagram
            if (!charCountMap.containsKey(ch) || charCountMap.get(ch) == 0) {
                return false;
            }
            // Decrease count for each occurrence
            charCountMap.put(ch, charCountMap.get(ch) - 1);
        }
        
        return true;
    }
    
    public static void main(String[] args) {
        String str1 = "Listen";
        String str2 = "Silent";
        
        System.out.println("Are Anagrams: " + areAnagrams(str1, str2));
        
        str1 = "Triangle";
        str2 = "Integral";
        System.out.println("Are Anagrams: " + areAnagrams(str1, str2));
        
        str1 = "Apple";
        str2 = "Pale";
        System.out.println("Are Anagrams: " + areAnagrams(str1, str2));
        
        str1 = "Dormitory";
        str2 = "Dirty room";
        System.out.println("Are Anagrams: " + areAnagrams(str1, str2));
    }
}
```

---

### 🧠 Explanation:
1. **Normalize Strings:**
   - Convert to lower case and remove white spaces for accurate comparison.

2. **Length Check:**
   - If lengths are different, they can't be anagrams.

3. **Frequency Count Using HashMap:**
   - Count characters for `str1` and store in `HashMap`.
   - Decrement counts while processing `str2`.
   - If any character is absent or the count is zero, return `false`.

4. **Final Check:**
   - If all counts are zero, return `true`.

---

### 📊 Complexity Analysis:
- **Time Complexity:** `O(n)` as it involves a single pass over both strings
- **Space Complexity:** `O(k)` where `k` is the number of unique characters

---

---

## 🔥 Test Cases:
```java
Input: "listen", "silent"
Output: true

Input: "triangle", "integral"
Output: true

Input: "apple", "pale"
Output: false

Input: "Dormitory", "Dirty room"
Output: true

Input: "A gentleman", "Elegant man"
Output: true
```

---

### 💡 Best Practices:
- Use `HashMap` for efficient counting and lookups.
- Normalize input by converting to lower case and removing spaces.
- Handle edge cases like empty strings and single-character strings.

---

### ✅ Completed:
- [x] Core Concepts
- [x] Reverse a String
- [x] Check if a String is Palindrome
- [x] Count Vowels and Consonants in a String
- [x] Remove White Spaces from a String
- [x] Find Duplicate Characters in a String
- [x] Longest Palindromic Substring
- [x] Check if Two Strings are Anagrams

---

---

## 🎯 What's Next?

Would you like to:
1. Move to the next question: **Remove Duplicates from String**?
2. Get more detailed explanations or variations for this problem?
3. Proceed with more intermediate-level questions?

---

Let's move to the next intermediate-level question.

---

---

## 🟠 3. Remove Duplicates from String

### 🔑 Problem Description:
Write a program to **remove duplicate characters** from a string while maintaining the original order of characters.

- **Input:** A string `str`.
- **Output:** The string without duplicate characters, preserving the order.

---

### 🔍 Concepts Covered:
- Character occurrence tracking using `LinkedHashSet`
- Preserving insertion order
- String manipulation

---

### 🔔 Example:
```java
Input: "programming"
Output: "progamin"

Input: "Java"
Output: "Jav"

Input: "hello world"
Output: "helo wrd"

Input: "aabbcc"
Output: "abc"
```

---

### 💡 Approach:

1. **Using LinkedHashSet (Optimal Solution):**
   - `LinkedHashSet` maintains the insertion order and removes duplicates.
   - Convert the string to a character array.
   - Add characters to the `LinkedHashSet`.
   - Join the characters to form the resulting string.

2. **Using StringBuilder and IndexOf (Alternative Solution):**
   - Iterate through the characters of the string.
   - Append characters to `StringBuilder` only if they are not already present.

3. **Edge Case Handling:**
   - Handle empty strings and strings with all unique characters.

---

---

## 🔎 Solution 1: Using LinkedHashSet (Optimal Solution)

This solution uses a `LinkedHashSet` to store characters while preserving their order and ensuring uniqueness.

```java
import java.util.LinkedHashSet;
import java.util.Set;

public class RemoveDuplicatesLinkedHashSet {
    public static String removeDuplicates(String str) {
        // Step 1: Convert the string to a character array
        char[] charArray = str.toCharArray();
        
        // Step 2: Use LinkedHashSet to maintain order and remove duplicates
        Set<Character> charSet = new LinkedHashSet<>();
        for (char ch : charArray) {
            charSet.add(ch);
        }
        
        // Step 3: Construct the result string from the LinkedHashSet
        StringBuilder sb = new StringBuilder();
        for (char ch : charSet) {
            sb.append(ch);
        }
        
        return sb.toString();
    }
    
    public static void main(String[] args) {
        String str = "programming";
        System.out.println("Original String: " + str);
        System.out.println("Without Duplicates: " + removeDuplicates(str));
        
        str = "Java";
        System.out.println("Original String: " + str);
        System.out.println("Without Duplicates: " + removeDuplicates(str));
        
        str = "hello world";
        System.out.println("Original String: " + str);
        System.out.println("Without Duplicates: " + removeDuplicates(str));
        
        str = "aabbcc";
        System.out.println("Original String: " + str);
        System.out.println("Without Duplicates: " + removeDuplicates(str));
    }
}
```

---

### 🧠 Explanation:
1. **Using LinkedHashSet:**
   - `LinkedHashSet` maintains the insertion order while removing duplicates.
   - Each character is added to the set, ensuring only unique characters are stored.

2. **Constructing Result String:**
   - `StringBuilder` is used to efficiently build the result string.
   - Characters from the `LinkedHashSet` are appended in the order they were first encountered.

3. **Preserving Order:**
   - `LinkedHashSet` preserves the order of first occurrence, unlike `HashSet`.

---

### 📊 Complexity Analysis:
- **Time Complexity:** `O(n)` where `n` is the length of the string
- **Space Complexity:** `O(k)` where `k` is the number of unique characters

---

---

## 🔎 Solution 2: Using StringBuilder and IndexOf

This solution manually checks for duplicates using `StringBuilder`. It maintains the order by appending only unique characters.

```java
public class RemoveDuplicatesStringBuilder {
    public static String removeDuplicates(String str) {
        // Step 1: Initialize StringBuilder
        StringBuilder sb = new StringBuilder();
        
        // Step 2: Loop through each character
        for (int i = 0; i < str.length(); i++) {
            char ch = str.charAt(i);
            
            // Step 3: Append character if not already present
            if (sb.indexOf(String.valueOf(ch)) == -1) {
                sb.append(ch);
            }
        }
        
        return sb.toString();
    }
    
    public static void main(String[] args) {
        String str = "programming";
        System.out.println("Original String: " + str);
        System.out.println("Without Duplicates: " + removeDuplicates(str));
        
        str = "Java";
        System.out.println("Original String: " + str);
        System.out.println("Without Duplicates: " + removeDuplicates(str));
        
        str = "hello world";
        System.out.println("Original String: " + str);
        System.out.println("Without Duplicates: " + removeDuplicates(str));
        
        str = "aabbcc";
        System.out.println("Original String: " + str);
        System.out.println("Without Duplicates: " + removeDuplicates(str));
    }
}
```

---

### 🧠 Explanation:
1. **Using StringBuilder:**
   - `StringBuilder` efficiently builds the result string.
   - It avoids creating multiple immutable string objects.

2. **Checking for Duplicates:**
   - `indexOf()` checks if the character is already present.
   - If not present (`indexOf()` returns `-1`), the character is appended.

3. **Maintaining Order:**
   - Characters are appended in the order they first appear.

---

### 📊 Complexity Analysis:
- **Time Complexity:** `O(n^2)` due to `indexOf()` within a loop
- **Space Complexity:** `O(k)` for storing the result (`k` = number of unique characters)

---

---

## 🔥 Test Cases:
```java
Input: "programming"
Output: "progamin"

Input: "Java"
Output: "Jav"

Input: "hello world"
Output: "helo wrd"

Input: "aabbcc"
Output: "abc"

Input: ""
Output: ""

Input: "abcdef"
Output: "abcdef"
```

---

### 💡 Best Practices:
- Use `LinkedHashSet` for optimal performance and order preservation.
- Handle edge cases like empty strings and all unique characters.
- Avoid using `indexOf()` in a loop for large strings due to `O(n^2)` complexity.

---

### ✅ Completed:
- [x] Core Concepts
- [x] Reverse a String
- [x] Check if a String is Palindrome
- [x] Count Vowels and Consonants in a String
- [x] Remove White Spaces from a String
- [x] Find Duplicate Characters in a String
- [x] Longest Palindromic Substring
- [x] Check if Two Strings are Anagrams
- [x] Remove Duplicates from String

---

---

## 🎯 What's Next?

Would you like to:
1. Move to the next question: **Find All Substrings of a String**?
2. Get more detailed explanations or variations for this problem?
3. Proceed with more intermediate-level questions?

---

Let's move to the next intermediate-level question.

---

---

## 🟠 4. Find All Substrings of a String

### 🔑 Problem Description:
Write a program to **generate all possible substrings** of a given string.

- **Input:** A string `str`.
- **Output:** A list of all substrings.

---

### 🔍 Concepts Covered:
- String manipulation using `substring()` method
- Nested loops for generating substrings
- Edge case handling (empty strings)

---

### 🔔 Example:
```java
Input: "abc"
Output: ["a", "b", "c", "ab", "bc", "abc"]

Input: "abcd"
Output: ["a", "b", "c", "d", "ab", "bc", "cd", "abc", "bcd", "abcd"]

Input: "aa"
Output: ["a", "a", "aa"]
```

---

### 💡 Approach:

1. **Using Nested Loops:**
   - Use two loops to generate all possible substrings:
     - Outer loop for the starting index.
     - Inner loop for the ending index.
   - Extract substrings using `substring(start, end)`.

2. **Collect and Store Substrings:**
   - Store each substring in an `ArrayList`.
   - Maintain the order of occurrence.

3. **Edge Case Handling:**
   - Handle empty strings and single-character strings.
   - Avoid duplicate substrings if needed.

---

---

## 🔎 Solution: Using Nested Loops

This solution uses nested loops to generate all possible substrings efficiently.

```java
import java.util.ArrayList;
import java.util.List;

public class AllSubstrings {
    public static List<String> getAllSubstrings(String str) {
        // Step 1: Initialize ArrayList to store substrings
        List<String> substrings = new ArrayList<>();
        
        // Step 2: Use nested loops to generate substrings
        for (int i = 0; i < str.length(); i++) {
            for (int j = i + 1; j <= str.length(); j++) {
                // Step 3: Extract substring from i to j
                String sub = str.substring(i, j);
                substrings.add(sub);
            }
        }
        
        return substrings;
    }
    
    public static void main(String[] args) {
        String str = "abc";
        System.out.println("All Substrings of " + str + ": " + getAllSubstrings(str));
        
        str = "abcd";
        System.out.println("All Substrings of " + str + ": " + getAllSubstrings(str));
        
        str = "aa";
        System.out.println("All Substrings of " + str + ": " + getAllSubstrings(str));
        
        str = "";
        System.out.println("All Substrings of " + str + ": " + getAllSubstrings(str));
        
        str = "a";
        System.out.println("All Substrings of " + str + ": " + getAllSubstrings(str));
    }
}
```

---

### 🧠 Explanation:
1. **Nested Loops:**
   - The **outer loop** sets the starting index (`i`).
   - The **inner loop** sets the ending index (`j`).
   - `j` starts from `i + 1` to ensure non-empty substrings.

2. **Extracting Substrings:**
   - `substring(i, j)` extracts the substring from index `i` to `j-1`.
   - All substrings are stored in the `ArrayList`.

3. **Order of Substrings:**
   - Substrings are stored in the order they are generated.
   - The order is from shorter to longer substrings.

---

### 📊 Complexity Analysis:
- **Time Complexity:** `O(n^2)` due to the nested loops (`n` is the length of the string)
- **Space Complexity:** `O(n^2)` for storing all substrings

---

---

## 🔥 Test Cases:
```java
Input: "abc"
Output: ["a", "b", "c", "ab", "bc", "abc"]

Input: "abcd"
Output: ["a", "b", "c", "d", "ab", "bc", "cd", "abc", "bcd", "abcd"]

Input: "aa"
Output: ["a", "a", "aa"]

Input: ""
Output: []

Input: "a"
Output: ["a"]
```

---

### 💡 Best Practices:
- Use `ArrayList` to store substrings for dynamic resizing.
- Handle edge cases like empty strings and single-character strings.
- If duplicates are not allowed, use `LinkedHashSet`.

---

### ✅ Completed:
- [x] Core Concepts
- [x] Reverse a String
- [x] Check if a String is Palindrome
- [x] Count Vowels and Consonants in a String
- [x] Remove White Spaces from a String
- [x] Find Duplicate Characters in a String
- [x] Longest Palindromic Substring
- [x] Check if Two Strings are Anagrams
- [x] Remove Duplicates from String
- [x] Find All Substrings of a String

---

---

## 🎯 What's Next?

Would you like to:
1. Move to the next question: **String Rotation Check**?
2. Get more detailed explanations or variations for this problem?
3. Proceed with more intermediate-level questions?

---

Let's move to the next intermediate-level question.

---

---

## 🟠 5. String Rotation Check

### 🔑 Problem Description:
Write a program to **check if one string is a rotation of another**. 

- **Input:** Two strings `str1` and `str2`.
- **Output:** `true` if `str2` is a rotation of `str1`, `false` otherwise.

---

### 🔍 Concepts Covered:
- String concatenation
- Substring search using `contains()`
- Edge case handling (length check)

---

### 🔔 Example:
```java
Input: "abcde", "cdeab"
Output: true

Input: "waterbottle", "erbottlewat"
Output: true

Input: "abcd", "dabc"
Output: true

Input: "hello", "elloh"
Output: true

Input: "abc", "acb"
Output: false
```

---

### 💡 Approach:

1. **Concatenate and Search:**
   - Concatenate `str1` with itself.
   - If `str2` is a rotation of `str1`, it must be a substring of the concatenated string.
   - Example:
     ```
     str1 = "abcde"
     str2 = "cdeab"
     concatenated = "abcdeabcde"
     str2 is a substring of concatenated → true
     ```

2. **Length Check:**
   - Both strings must have the **same length** to be rotations.
   - If the lengths differ, return `false` immediately.

3. **Edge Case Handling:**
   - Handle empty strings.
   - Strings with all identical characters.

---

---

## 🔎 Solution: Using Concatenation and Substring Search

This solution is efficient because it avoids nested loops by leveraging string concatenation and `contains()` method.

```java
public class StringRotationCheck {
    public static boolean isRotation(String str1, String str2) {
        // Step 1: Check length and non-empty strings
        if (str1.length() != str2.length() || str1.isEmpty()) {
            return false;
        }
        
        // Step 2: Concatenate str1 with itself
        String concatenated = str1 + str1;
        
        // Step 3: Check if str2 is a substring of the concatenated string
        return concatenated.contains(str2);
    }
    
    public static void main(String[] args) {
        String str1 = "abcde";
        String str2 = "cdeab";
        System.out.println("Is Rotation: " + isRotation(str1, str2)); // true
        
        str1 = "waterbottle";
        str2 = "erbottlewat";
        System.out.println("Is Rotation: " + isRotation(str1, str2)); // true
        
        str1 = "abcd";
        str2 = "dabc";
        System.out.println("Is Rotation: " + isRotation(str1, str2)); // true
        
        str1 = "hello";
        str2 = "elloh";
        System.out.println("Is Rotation: " + isRotation(str1, str2)); // true
        
        str1 = "abc";
        str2 = "acb";
        System.out.println("Is Rotation: " + isRotation(str1, str2)); // false
        
        str1 = "aa";
        str2 = "aa";
        System.out.println("Is Rotation: " + isRotation(str1, str2)); // true
        
        str1 = "";
        str2 = "";
        System.out.println("Is Rotation: " + isRotation(str1, str2)); // false
    }
}
```

---

### 🧠 Explanation:
1. **Length Check:**
   - If lengths are not equal, return `false` immediately.
   - If strings are empty, return `false`.

2. **Concatenate and Search:**
   - Concatenate `str1` with itself.
   - `str2` should appear as a substring in the concatenated string if it's a rotation.

3. **Using contains():**
   - `contains()` checks if one string is a substring of another.
   - It is optimized for searching within a string.

---

### 📊 Complexity Analysis:
- **Time Complexity:** `O(n)` for concatenation and substring search (`n` = length of the string)
- **Space Complexity:** `O(n)` for the concatenated string

---

---

## 🔥 Test Cases:
```java
Input: "abcde", "cdeab"
Output: true

Input: "waterbottle", "erbottlewat"
Output: true

Input: "abcd", "dabc"
Output: true

Input: "hello", "elloh"
Output: true

Input: "abc", "acb"
Output: false

Input: "aa", "aa"
Output: true

Input: "", ""
Output: false
```

---

### 💡 Best Practices:
- Check the length first to avoid unnecessary processing.
- Use concatenation and `contains()` for an efficient solution.
- Handle edge cases like empty strings and identical characters.

---

### ✅ Completed:
- [x] Core Concepts
- [x] Reverse a String
- [x] Check if a String is Palindrome
- [x] Count Vowels and Consonants in a String
- [x] Remove White Spaces from a String
- [x] Find Duplicate Characters in a String
- [x] Longest Palindromic Substring
- [x] Check if Two Strings are Anagrams
- [x] Remove Duplicates from String
- [x] Find All Substrings of a String
- [x] String Rotation Check

---

---

## 🎯 What's Next?

Would you like to:
1. Move to the **Advanced Level** questions?
2. Get more detailed explanations or variations for this problem?
3. Proceed with more string-related questions?

---

Let's move to the **Advanced Level** questions.

---

---

## 🔴 Advanced Level: String Algorithms and Optimization

Here are the advanced-level questions we'll cover:

1. **Longest Common Prefix** ✅
2. **Word Break Problem** ✅
3. **Regular Expression Matching** ✅
4. **Find the First Non-Repeated Character**
5. **Minimum Window Substring**
6. **Valid Number (Regular Expression)**
7. **Generate All Permutations of a String**
8. **Count and Say Sequence**
9. **Decode Ways**
10. **Longest Substring Without Repeating Characters**

Let's start with the first question.

---

---

## 🔴 1. Longest Common Prefix

### 🔑 Problem Description:
Write a program to **find the longest common prefix** among an array of strings.

- **Input:** An array of strings `strs[]`.
- **Output:** The longest common prefix. If none, return an empty string.

---

### 🔍 Concepts Covered:
- String comparison using `startsWith()`
- Sorting and substring extraction
- Edge case handling (empty arrays, no common prefix)

---

### 🔔 Example:
```java
Input: ["flower", "flow", "flight"]
Output: "fl"

Input: ["dog", "racecar", "car"]
Output: ""

Input: ["interview", "internet", "internal", "interval"]
Output: "inte"

Input: ["abc", "abcd", "ab"]
Output: "ab"
```

---

### 💡 Approach:

1. **Vertical Scanning (Optimal Solution):**
   - Compare characters column-wise across all strings.
   - Stop when a mismatch is found.
   - This avoids unnecessary comparisons.

2. **Sorting and Comparing First and Last (Alternative Solution):**
   - Sort the array.
   - Compare the first and last strings since they are the most different.
   - Find the common prefix by comparing characters.

3. **Edge Case Handling:**
   - Handle empty array and single string array.
   - Handle strings with no common prefix.

---

---

## 🔎 Solution 1: Vertical Scanning (Optimal Solution)

This solution compares characters column by column and stops at the first mismatch.

```java
public class LongestCommonPrefix {
    public static String longestCommonPrefix(String[] strs) {
        // Step 1: Edge case for empty array
        if (strs == null || strs.length == 0) {
            return "";
        }
        
        // Step 2: Loop through characters of the first string
        for (int i = 0; i < strs[0].length(); i++) {
            char ch = strs[0].charAt(i);
            
            // Step 3: Compare with the same character position in other strings
            for (int j = 1; j < strs.length; j++) {
                // If index out of bounds or character mismatch
                if (i >= strs[j].length() || strs[j].charAt(i) != ch) {
                    return strs[0].substring(0, i);
                }
            }
        }
        
        // If no mismatch, return the first string
        return strs[0];
    }
    
    public static void main(String[] args) {
        String[] strs1 = {"flower", "flow", "flight"};
        System.out.println("Longest Common Prefix: " + longestCommonPrefix(strs1)); // Output: "fl"
        
        String[] strs2 = {"dog", "racecar", "car"};
        System.out.println("Longest Common Prefix: " + longestCommonPrefix(strs2)); // Output: ""
        
        String[] strs3 = {"interview", "internet", "internal", "interval"};
        System.out.println("Longest Common Prefix: " + longestCommonPrefix(strs3)); // Output: "inte"
        
        String[] strs4 = {"abc", "abcd", "ab"};
        System.out.println("Longest Common Prefix: " + longestCommonPrefix(strs4)); // Output: "ab"
        
        String[] strs5 = {""};
        System.out.println("Longest Common Prefix: " + longestCommonPrefix(strs5)); // Output: ""
    }
}
```

---

### 🧠 Explanation:
1. **Edge Case Check:**
   - Return `""` for an empty array.

2. **Vertical Scanning:**
   - Loop through each character of the first string.
   - Compare the character with the corresponding character in all other strings.

3. **Stopping Condition:**
   - Stop when:
     - A character mismatch is found.
     - Any string is shorter than the current index.

4. **Returning Result:**
   - Return the substring from the first string up to the mismatch index.

---

### 📊 Complexity Analysis:
- **Time Complexity:** `O(n * m)` where `n` is the number of strings and `m` is the length of the shortest string
- **Space Complexity:** `O(1)` as no additional space proportional to input size is used

---

---

## 🔎 Solution 2: Sorting and Comparing First and Last

This solution sorts the array and compares the first and last strings. It is more efficient for closely related strings.

```java
import java.util.Arrays;

public class LongestCommonPrefixSort {
    public static String longestCommonPrefix(String[] strs) {
        // Step 1: Edge case for empty array
        if (strs == null || strs.length == 0) {
            return "";
        }
        
        // Step 2: Sort the array
        Arrays.sort(strs);
        
        // Step 3: Compare the first and last strings
        String first = strs[0];
        String last = strs[strs.length - 1];
        
        int i = 0;
        while (i < first.length() && i < last.length() && first.charAt(i) == last.charAt(i)) {
            i++;
        }
        
        return first.substring(0, i);
    }
    
    public static void main(String[] args) {
        String[] strs1 = {"flower", "flow", "flight"};
        System.out.println("Longest Common Prefix: " + longestCommonPrefix(strs1)); // Output: "fl"
        
        String[] strs2 = {"dog", "racecar", "car"};
        System.out.println("Longest Common Prefix: " + longestCommonPrefix(strs2)); // Output: ""
        
        String[] strs3 = {"interview", "internet", "internal", "interval"};
        System.out.println("Longest Common Prefix: " + longestCommonPrefix(strs3)); // Output: "inte"
        
        String[] strs4 = {"abc", "abcd", "ab"};
        System.out.println("Longest Common Prefix: " + longestCommonPrefix(strs4)); // Output: "ab"
        
        String[] strs5 = {""};
        System.out.println("Longest Common Prefix: " + longestCommonPrefix(strs5)); // Output: ""
    }
}
```

---

### 🧠 Explanation:
1. **Sorting the Array:**
   - Sorting places the most similar strings next to each other.
   - Only the first and last strings need to be compared.

2. **Compare First and Last:**
   - Compare characters until a mismatch is found.
   - Return the common prefix.

3. **Edge Case Handling:**
   - Handles empty arrays and single-string arrays.

---

### 📊 Complexity Analysis:
- **Time Complexity:** `O(n log n + m)` due to sorting and character comparison
- **Space Complexity:** `O(1)`

---

---

## 🔥 Test Cases:
```java
Input: ["flower", "flow", "flight"]
Output: "fl"

Input: ["dog", "racecar", "car"]
Output: ""

Input: ["interview", "internet", "internal", "interval"]
Output: "inte"

Input: ["abc", "abcd", "ab"]
Output: "ab"

Input: ["", ""]
Output: ""
```

---

### 💡 Best Practices:
- Use **Vertical Scanning** for optimal performance.
- Use **Sorting and Comparing** for closely related strings.
- Handle edge cases like empty arrays and single-string arrays.

---

### ✅ Completed:
- [x] Core Concepts
- [x] Reverse a String
- [x] Check if a String is Palindrome
- [x] Count Vowels and Consonants in a String
- [x] Remove White Spaces from a String
- [x] Find Duplicate Characters in a String
- [x] Longest Palindromic Substring
- [x] Check if Two Strings are Anagrams
- [x] Remove Duplicates from String
- [x] Find All Substrings of a String
- [x] String Rotation Check
- [x] Longest Common Prefix

---

---

## 🎯 What's Next?

Would you like to:
1. Move to the next question: **Word Break Problem**?
2. Get more detailed explanations or variations for this problem?
3. Proceed with more advanced-level questions?

---

Let's move to the next advanced-level question.

---

---

## 🔴 2. Word Break Problem

### 🔑 Problem Description:
Write a program to **determine if a string can be segmented into one or more dictionary words**. 

- **Input:** A string `s` and a list of words `wordDict`.
- **Output:** `true` if the string can be segmented, `false` otherwise.

---

### 🔍 Concepts Covered:
- Dynamic Programming
- HashSet for quick lookups
- String manipulation using `substring()`

---

### 🔔 Example:
```java
Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: "applepenapple" can be segmented as "apple pen apple".

Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false

Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true

Input: s = "aaaaaaa", wordDict = ["aaa", "aaaa"]
Output: true
```

---

### 💡 Approach:

1. **Dynamic Programming Approach (Optimal Solution):**
   - Use a boolean array `dp[]` where `dp[i]` is `true` if the substring `s[0...i]` can be segmented.
   - Initialize `dp[0]` as `true` because an empty string can be segmented.
   - Iterate over each position `i` and check all possible substrings ending at `i`.
   - If a substring is in the dictionary and the prefix is segmentable (`dp[j]`), set `dp[i]` to `true`.

2. **HashSet for Fast Lookups:**
   - Store `wordDict` in a `HashSet` for constant-time lookups.

3. **Edge Case Handling:**
   - Handle empty strings and words not present in the dictionary.

---

---

## 🔎 Solution: Dynamic Programming

This solution uses dynamic programming with a boolean array to keep track of segmentable substrings.

```java
import java.util.HashSet;
import java.util.List;
import java.util.Set;

public class WordBreak {
    public static boolean wordBreak(String s, List<String> wordDict) {
        // Step 1: Store wordDict in a HashSet for fast lookups
        Set<String> wordSet = new HashSet<>(wordDict);
        
        // Step 2: Initialize DP array
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true; // Empty string is segmentable
        
        // Step 3: Iterate over all positions in the string
        for (int i = 1; i <= s.length(); i++) {
            // Step 4: Check all substrings ending at i
            for (int j = 0; j < i; j++) {
                String sub = s.substring(j, i);
                
                // Step 5: If substring is in wordDict and prefix is segmentable
                if (dp[j] && wordSet.contains(sub)) {
                    dp[i] = true;
                    break; // No need to check further
                }
            }
        }
        
        // Step 6: Return result for the entire string
        return dp[s.length()];
    }
    
    public static void main(String[] args) {
        String s = "applepenapple";
        List<String> wordDict = List.of("apple", "pen");
        System.out.println("Word Break: " + wordBreak(s, wordDict)); // Output: true
        
        s = "catsandog";
        wordDict = List.of("cats", "dog", "sand", "and", "cat");
        System.out.println("Word Break: " + wordBreak(s, wordDict)); // Output: false
        
        s = "leetcode";
        wordDict = List.of("leet", "code");
        System.out.println("Word Break: " + wordBreak(s, wordDict)); // Output: true
        
        s = "aaaaaaa";
        wordDict = List.of("aaa", "aaaa");
        System.out.println("Word Break: " + wordBreak(s, wordDict)); // Output: true
        
        s = "apple";
        wordDict = List.of("orange", "banana");
        System.out.println("Word Break: " + wordBreak(s, wordDict)); // Output: false
    }
}
```

---

### 🧠 Explanation:
1. **Using HashSet for Fast Lookup:**
   - `wordSet` stores the dictionary words for `O(1)` lookups.

2. **Dynamic Programming Array:**
   - `dp[i]` is `true` if the substring `s[0...i]` can be segmented.
   - Initialize `dp[0]` to `true` for the empty prefix.

3. **Substring Checking:**
   - For each position `i`, check all substrings ending at `i`.
   - If a substring is in `wordSet` and the prefix is segmentable (`dp[j]`), mark `dp[i]` as `true`.

4. **Returning Result:**
   - Return `dp[s.length()]` which represents the segmentability of the entire string.

---

### 📊 Complexity Analysis:
- **Time Complexity:** `O(n^2)` where `n` is the length of the string
- **Space Complexity:** `O(n)` for the `dp[]` array

---

---

## 🔥 Test Cases:
```java
Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true

Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false

Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true

Input: s = "aaaaaaa", wordDict = ["aaa", "aaaa"]
Output: true

Input: s = "apple", wordDict = ["orange", "banana"]
Output: false
```

---

### 💡 Best Practices:
- Use `HashSet` for fast dictionary lookups.
- Use dynamic programming to avoid redundant calculations.
- Handle edge cases like empty strings and words not present in the dictionary.

---

### ✅ Completed:
- [x] Core Concepts
- [x] Reverse a String
- [x] Check if a String is Palindrome
- [x] Count Vowels and Consonants in a String
- [x] Remove White Spaces from a String
- [x] Find Duplicate Characters in a String
- [x] Longest Palindromic Substring
- [x] Check if Two Strings are Anagrams
- [x] Remove Duplicates from String
- [x] Find All Substrings of a String
- [x] String Rotation Check
- [x] Longest Common Prefix
- [x] Word Break Problem

---

---

## 🎯 What's Next?

Would you like to:
1. Move to the next question: **Regular Expression Matching**?
2. Get more detailed explanations or variations for this problem?
3. Proceed with more advanced-level questions?

---

Let's move to the next advanced-level question.

---

---

## 🔴 3. Regular Expression Matching

### 🔑 Problem Description:
Implement a regular expression matcher with support for the following special characters:
- `.` (dot) matches any single character.
- `*` (asterisk) matches zero or more of the preceding element.

- **Input:** A string `s` and a pattern `p`.
- **Output:** `true` if the pattern matches the string, `false` otherwise.

---

### 🔍 Concepts Covered:
- Dynamic Programming
- Recursive Backtracking (optional)
- Regular Expression Matching Rules

---

### 🔔 Example:
```java
Input: s = "aa", p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".

Input: s = "aa", p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding character, so "a*" matches "aa".

Input: s = "ab", p = ".*"
Output: true
Explanation: ".*" means zero or more of any character.

Input: s = "aab", p = "c*a*b"
Output: true
Explanation: c can be ignored, a can be repeated, and b matches.

Input: s = "mississippi", p = "mis*is*p*."
Output: false
```

---

### 💡 Approach:

1. **Dynamic Programming (Optimal Solution):**
   - Use a 2D boolean array `dp[i][j]` where:
     - `i` represents the position in the string `s`.
     - `j` represents the position in the pattern `p`.
     - `dp[i][j]` is `true` if the substring `s[0...i]` matches the pattern `p[0...j]`.
   - Start with `dp[0][0] = true` for two empty strings.
   - Iterate through the string and pattern to fill the `dp` table.

2. **Handling Special Characters:**
   - **`.` (Dot)**: Matches any single character.
   - **`*` (Asterisk)**: Matches zero or more occurrences of the preceding character:
     - Ignore the preceding character (`zero occurrence`).
     - Match one or more occurrences by moving the string pointer.

3. **Edge Case Handling:**
   - Handle empty strings and patterns.
   - Handle patterns with multiple `*` characters.

---

---

## 🔎 Solution: Dynamic Programming

This solution uses a 2D boolean array to efficiently match the string against the pattern.

```java
public class RegularExpressionMatching {
    public static boolean isMatch(String s, String p) {
        // Step 1: Initialize DP table
        boolean[][] dp = new boolean[s.length() + 1][p.length() + 1];
        dp[0][0] = true; // Empty string and empty pattern are a match
        
        // Step 2: Handle patterns like a*, a*b*, a*b*c* at the beginning
        for (int j = 1; j < dp[0].length; j++) {
            if (p.charAt(j - 1) == '*') {
                dp[0][j] = dp[0][j - 2];
            }
        }
        
        // Step 3: Fill the DP table
        for (int i = 1; i < dp.length; i++) {
            for (int j = 1; j < dp[0].length; j++) {
                char currentPattern = p.charAt(j - 1);
                
                // Case 1: Current pattern is a dot or exact match
                if (currentPattern == '.' || currentPattern == s.charAt(i - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                }
                
                // Case 2: Current pattern is asterisk
                else if (currentPattern == '*') {
                    char precedingPattern = p.charAt(j - 2);
                    
                    // Ignore the preceding element (zero occurrence)
                    dp[i][j] = dp[i][j - 2];
                    
                    // Check one or more occurrences
                    if (precedingPattern == '.' || precedingPattern == s.charAt(i - 1)) {
                        dp[i][j] = dp[i][j] || dp[i - 1][j];
                    }
                }
            }
        }
        
        // Step 4: Return the result for the entire string and pattern
        return dp[s.length()][p.length()];
    }
    
    public static void main(String[] args) {
        String s = "aa";
        String p = "a";
        System.out.println("Is Match: " + isMatch(s, p)); // false
        
        s = "aa";
        p = "a*";
        System.out.println("Is Match: " + isMatch(s, p)); // true
        
        s = "ab";
        p = ".*";
        System.out.println("Is Match: " + isMatch(s, p)); // true
        
        s = "aab";
        p = "c*a*b";
        System.out.println("Is Match: " + isMatch(s, p)); // true
        
        s = "mississippi";
        p = "mis*is*p*.";
        System.out.println("Is Match: " + isMatch(s, p)); // false
    }
}
```

---

### 🧠 Explanation:
1. **Initialize DP Table:**
   - `dp[i][j]` indicates if the substring `s[0...i]` matches the pattern `p[0...j]`.
   - `dp[0][0] = true` since two empty strings match.

2. **Handle Leading Asterisks:**
   - If the pattern starts with `a*` or similar, handle them by setting `dp[0][j]`.

3. **Fill the DP Table:**
   - **Case 1:** Dot `.` or Exact Match:
     - If `p[j-1]` is `.` or matches `s[i-1]`, then it depends on the previous characters (`dp[i-1][j-1]`).
   - **Case 2:** Asterisk `*`:
     - **Zero Occurrence:** Ignore the preceding character (`dp[i][j-2]`).
     - **One or More Occurrences:** Match and continue (`dp[i-1][j]`).

4. **Return Final Result:**
   - Return the value at `dp[s.length()][p.length()]`.

---

### 📊 Complexity Analysis:
- **Time Complexity:** `O(n * m)` where `n` is the length of `s` and `m` is the length of `p`
- **Space Complexity:** `O(n * m)` for the `dp` table

---

---

## 🔥 Test Cases:
```java
Input: s = "aa", p = "a"
Output: false

Input: s = "aa", p = "a*"
Output: true

Input: s = "ab", p = ".*"
Output: true

Input: s = "aab", p = "c*a*b"
Output: true

Input: s = "mississippi", p = "mis*is*p*."
Output: false

Input: s = "abc", p = ".*c"
Output: true
```

---

### 💡 Best Practices:
- Use dynamic programming to optimize the recursive approach.
- Handle special characters `.` and `*` carefully.
- Initialize `dp[0][j]` for patterns starting with `a*`, `b*`, etc.

---

### ✅ Completed:
- [x] Core Concepts
- [x] Reverse a String
- [x] Check if a String is Palindrome
- [x] Count Vowels and Consonants in a String
- [x] Remove White Spaces from a String
- [x] Find Duplicate Characters in a String
- [x] Longest Palindromic Substring
- [x] Check if Two Strings are Anagrams
- [x] Remove Duplicates from String
- [x] Find All Substrings of a String
- [x] String Rotation Check
- [x] Longest Common Prefix
- [x] Word Break Problem
- [x] Regular Expression Matching

---

---

## 🎯 What's Next?

Would you like to:
1. Move to the next question: **Find the First Non-Repeated Character**?
2. Get more detailed explanations or variations for this problem?
3. Proceed with more advanced-level questions?

Let's move to the next advanced-level question.

---

---

## 🔴 4. Find the First Non-Repeated Character

### 🔑 Problem Description:
Write a program to **find the first non-repeated character** in a given string.

- **Input:** A string `str`.
- **Output:** The first non-repeated character. If no unique character is found, return `'\0'` (null character).

---

### 🔍 Concepts Covered:
- Character frequency counting using `LinkedHashMap`
- Preserving order of characters
- Edge case handling (all characters repeated, empty string)

---

### 🔔 Example:
```java
Input: "swiss"
Output: "w"

Input: "hello"
Output: "h"

Input: "aabbcc"
Output: '\0' (No non-repeated character)

Input: "abcabcde"
Output: "d"

Input: "abcd"
Output: "a"
```

---

### 💡 Approach:

1. **Using LinkedHashMap (Optimal Solution):**
   - `LinkedHashMap` maintains the insertion order while counting the frequency of each character.
   - Traverse the string and store each character's frequency in `LinkedHashMap`.
   - Iterate through the `LinkedHashMap` to find the first character with a count of 1.

2. **Edge Case Handling:**
   - Handle empty strings.
   - Handle strings with all repeated characters.

---

---

## 🔎 Solution: Using LinkedHashMap

This solution uses `LinkedHashMap` to maintain the insertion order and efficiently count character frequencies.

```java
import java.util.LinkedHashMap;
import java.util.Map;

public class FirstNonRepeatedCharacter {
    public static char firstNonRepeatedChar(String str) {
        // Step 1: Use LinkedHashMap to maintain order and count frequency
        Map<Character, Integer> charCountMap = new LinkedHashMap<>();
        
        // Step 2: Count the frequency of each character
        for (int i = 0; i < str.length(); i++) {
            char ch = str.charAt(i);
            charCountMap.put(ch, charCountMap.getOrDefault(ch, 0) + 1);
        }
        
        // Step 3: Find the first character with count 1
        for (Map.Entry<Character, Integer> entry : charCountMap.entrySet()) {
            if (entry.getValue() == 1) {
                return entry.getKey();
            }
        }
        
        // Step 4: If no unique character found, return null character
        return '\0';
    }
    
    public static void main(String[] args) {
        String str = "swiss";
        System.out.println("First Non-Repeated Character: " + firstNonRepeatedChar(str)); // Output: "w"
        
        str = "hello";
        System.out.println("First Non-Repeated Character: " + firstNonRepeatedChar(str)); // Output: "h"
        
        str = "aabbcc";
        System.out.println("First Non-Repeated Character: " + firstNonRepeatedChar(str)); // Output: '\0'
        
        str = "abcabcde";
        System.out.println("First Non-Repeated Character: " + firstNonRepeatedChar(str)); // Output: "d"
        
        str = "abcd";
        System.out.println("First Non-Repeated Character: " + firstNonRepeatedChar(str)); // Output: "a"
        
        str = "";
        System.out.println("First Non-Repeated Character: " + firstNonRepeatedChar(str)); // Output: '\0'
    }
}
```

---

### 🧠 Explanation:
1. **Using LinkedHashMap:**
   - `LinkedHashMap` is used to store characters as keys and their counts as values.
   - It maintains the order of insertion, ensuring that the first non-repeated character is found in order.

2. **Count Frequency:**
   - Traverse the string and update the frequency of each character.
   - `getOrDefault(ch, 0)` initializes the count to `0` if the character is not already present.

3. **Find First Non-Repeated Character:**
   - Iterate through the `LinkedHashMap` entries.
   - Return the first character with a count of `1`.

4. **Return Result:**
   - If no non-repeated character is found, return `'\0'`.

---

### 📊 Complexity Analysis:
- **Time Complexity:** `O(n)` where `n` is the length of the string (single pass)
- **Space Complexity:** `O(k)` where `k` is the number of unique characters

---

---

## 🔥 Test Cases:
```java
Input: "swiss"
Output: "w"

Input: "hello"
Output: "h"

Input: "aabbcc"
Output: '\0' (No non-repeated character)

Input: "abcabcde"
Output: "d"

Input: "abcd"
Output: "a"

Input: ""
Output: '\0'

Input: "aabbcde"
Output: "d"
```

---

### 💡 Best Practices:
- Use `LinkedHashMap` to maintain insertion order and efficient lookups.
- Return `'\0'` for strings with no non-repeated character.
- Handle edge cases like empty strings and all repeated characters.

---

### ✅ Completed:
- [x] Core Concepts
- [x] Reverse a String
- [x] Check if a String is Palindrome
- [x] Count Vowels and Consonants in a String
- [x] Remove White Spaces from a String
- [x] Find Duplicate Characters in a String
- [x] Longest Palindromic Substring
- [x] Check if Two Strings are Anagrams
- [x] Remove Duplicates from String
- [x] Find All Substrings of a String
- [x] String Rotation Check
- [x] Longest Common Prefix
- [x] Word Break Problem
- [x] Regular Expression Matching
- [x] Find the First Non-Repeated Character

---

---

## 🎯 What's Next?

Would you like to:
1. Move to the next question: **Minimum Window Substring**?
2. Get more detailed explanations or variations for this problem?
3. Proceed with more advanced-level questions?

---

Let's move to the next advanced-level question.

---

---

## 🔴 5. Minimum Window Substring

### 🔑 Problem Description:
Given two strings `s` and `t`, find the **minimum window substring** in `s` which contains all the characters in `t` (including duplicates). If no such window exists, return an empty string.

- **Input:** Two strings `s` and `t`.
- **Output:** The minimum window substring of `s` that contains all characters of `t`. If not found, return `""`.

---

### 🔍 Concepts Covered:
- Two-pointer (sliding window) technique
- HashMap for frequency counting
- Edge case handling (t longer than s, no valid window)

---

### 🔔 Example:
```java
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"

Input: s = "a", t = "a"
Output: "a"

Input: s = "a", t = "aa"
Output: ""

Input: s = "aa", t = "aa"
Output: "aa"

Input: s = "thisisateststring", t = "tist"
Output: "tstri"
```

---

### 💡 Approach:

1. **Sliding Window Technique (Optimal Solution):**
   - Use two pointers (`left` and `right`) to create a sliding window.
   - Expand the window by moving the `right` pointer to include characters.
   - Contract the window by moving the `left` pointer to minimize the window size while maintaining the required characters.

2. **Frequency Counting with HashMap:**
   - Store the frequency of each character in `t` using a `HashMap`.
   - Use another `HashMap` to keep track of the characters in the current window.

3. **Check Valid Window:**
   - A window is valid if it contains at least the required frequency of all characters in `t`.
   - Use a counter to track how many required characters are present in the window.

4. **Edge Case Handling:**
   - Handle cases where `t` is longer than `s`.
   - Handle cases where no valid window is found.

---

---

## 🔎 Solution: Sliding Window with HashMap

This solution uses a two-pointer sliding window approach with `HashMap` for frequency counting.

```java
import java.util.HashMap;
import java.util.Map;

public class MinimumWindowSubstring {
    public static String minWindow(String s, String t) {
        // Step 1: Edge case checks
        if (s == null || t == null || s.length() < t.length()) {
            return "";
        }
        
        // Step 2: Count frequency of characters in t
        Map<Character, Integer> targetMap = new HashMap<>();
        for (char ch : t.toCharArray()) {
            targetMap.put(ch, targetMap.getOrDefault(ch, 0) + 1);
        }
        
        // Step 3: Initialize sliding window pointers and required variables
        int left = 0, right = 0;
        int minLength = Integer.MAX_VALUE;
        int start = 0;
        int required = targetMap.size();
        int formed = 0;
        
        // Step 4: HashMap to count characters in the current window
        Map<Character, Integer> windowMap = new HashMap<>();
        
        // Step 5: Expand the window by moving the right pointer
        while (right < s.length()) {
            char ch = s.charAt(right);
            windowMap.put(ch, windowMap.getOrDefault(ch, 0) + 1);
            
            // If the current character's count matches the target, increment formed
            if (targetMap.containsKey(ch) && windowMap.get(ch).intValue() == targetMap.get(ch).intValue()) {
                formed++;
            }
            
            // Step 6: Contract the window by moving the left pointer
            while (left <= right && formed == required) {
                ch = s.charAt(left);
                
                // Update minimum window length and start index
                if (right - left + 1 < minLength) {
                    minLength = right - left + 1;
                    start = left;
                }
                
                // Remove character from window
                windowMap.put(ch, windowMap.get(ch) - 1);
                
                // If the frequency is less than required, decrement formed
                if (targetMap.containsKey(ch) && windowMap.get(ch) < targetMap.get(ch)) {
                    formed--;
                }
                
                left++;
            }
            
            right++;
        }
        
        // Step 7: Return the minimum window substring
        return minLength == Integer.MAX_VALUE ? "" : s.substring(start, start + minLength);
    }
    
    public static void main(String[] args) {
        String s = "ADOBECODEBANC";
        String t = "ABC";
        System.out.println("Minimum Window Substring: " + minWindow(s, t)); // Output: "BANC"
        
        s = "a";
        t = "a";
        System.out.println("Minimum Window Substring: " + minWindow(s, t)); // Output: "a"
        
        s = "a";
        t = "aa";
        System.out.println("Minimum Window Substring: " + minWindow(s, t)); // Output: ""
        
        s = "aa";
        t = "aa";
        System.out.println("Minimum Window Substring: " + minWindow(s, t)); // Output: "aa"
        
        s = "thisisateststring";
        t = "tist";
        System.out.println("Minimum Window Substring: " + minWindow(s, t)); // Output: "tstri"
    }
}
```

---

### 🧠 Explanation:
1. **Frequency Count:**
   - Count the frequency of each character in `t` using `targetMap`.
   - This is the required frequency for the window to be valid.

2. **Sliding Window:**
   - Expand the window by moving the `right` pointer.
   - Contract the window by moving the `left` pointer while maintaining a valid window.

3. **Valid Window Check:**
   - A window is valid if all required characters are present with the required frequency.
   - `formed` keeps track of how many required characters are present.

4. **Update Minimum Window:**
   - If the current window is smaller than the previously found window, update the `minLength` and `start`.

5. **Return Result:**
   - If no valid window is found, return an empty string.
   - Otherwise, return the minimum window substring.

---

### 📊 Complexity Analysis:
- **Time Complexity:** `O(n)` where `n` is the length of `s` (each character is visited at most twice)
- **Space Complexity:** `O(k)` where `k` is the number of unique characters in `t`

---

---

## 🔥 Test Cases:
```java
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"

Input: s = "a", t = "a"
Output: "a"

Input: s = "a", t = "aa"
Output: ""

Input: s = "aa", t = "aa"
Output: "aa"

Input: s = "thisisateststring", t = "tist"
Output: "tstri"

Input: s = "xyz", t = "xyzt"
Output: ""
```

---

### 💡 Best Practices:
- Use **sliding window** for optimal performance.
- Use `HashMap` for frequency counting and fast lookups.
- Handle edge cases like `t` being longer than `s` and no valid window.

---

### ✅ Completed:
- [x] Core Concepts
- [x] Reverse a String
- [x] Check if a String is Palindrome
- [x] Count Vowels and Consonants in a String
- [x] Remove White Spaces from a String
- [x] Find Duplicate Characters in a String
- [x] Longest Palindromic Substring
- [x] Check if Two Strings are Anagrams
- [x] Remove Duplicates from String
- [x] Find All Substrings of a String
- [x] String Rotation Check
- [x] Longest Common Prefix
- [x] Word Break Problem
- [x] Regular Expression Matching
- [x] Find the First Non-Repeated Character
- [x] Minimum Window Substring

---

---

## 🎯 What's Next?

Would you like to:
1. Move to the next question: **Valid Number (Regular Expression)**?
2. Get more detailed explanations or variations for this problem?
3. Proceed with more advanced-level questions?

---

Let's move to the next advanced-level question.

---

---

## 🔴 6. Valid Number (Regular Expression)

### 🔑 Problem Description:
Implement a function to **validate if a given string is a valid number**. A valid number can be:
- An integer (e.g., `2`, `-2`, `+2`)
- A decimal (e.g., `2.5`, `-0.5`, `.5`, `2.`)
- In scientific notation (e.g., `2e10`, `-2.5e-3`, `3.14E+2`)

- **Input:** A string `s`.
- **Output:** `true` if the string is a valid number, `false` otherwise.

---

### 🔍 Concepts Covered:
- Regular Expressions
- Pattern Matching using `Pattern` and `Matcher` in Java
- Edge case handling (leading/trailing spaces, signs, decimals, exponents)

---

### 🔔 Example:
```java
Input: "0"
Output: true

Input: " 0.1 "
Output: true

Input: "abc"
Output: false

Input: "1 a"
Output: false

Input: "2e10"
Output: true

Input: "-90e3"
Output: true

Input: "3e+7"
Output: true

Input: "53.5e93"
Output: true

Input: "--6"
Output: false

Input: "-+3"
Output: false

Input: "95a54e53"
Output: false
```

---

### 💡 Approach:

1. **Using Regular Expression (Optimal Solution):**
   - Construct a comprehensive regular expression to validate:
     - Optional leading and trailing white spaces.
     - Optional `+` or `-` sign.
     - Integer or decimal number (with or without leading digits).
     - Optional scientific notation with `e` or `E`, followed by an optional sign and integer.
   - Use `Pattern` and `Matcher` to match the input string against the regular expression.

2. **Regular Expression Breakdown:**
   ```
   ^[+-]?                 // Optional sign at the beginning
   (                      // Start of main number group
       (                  // Start of integer or decimal group
           \d+            // One or more digits
           (\.\d*)?       // Optional decimal point followed by zero or more digits
       |                  // OR
           \.\d+          // Decimal number without leading digits
       )                  // End of integer or decimal group
       ([eE][+-]?\d+)?    // Optional scientific notation with optional sign and digits
   )                      // End of main number group
   $                      // End of input
   ```

3. **Edge Case Handling:**
   - Handle leading and trailing white spaces.
   - Handle optional `+` or `-` sign.
   - Handle scientific notation (`e` or `E`).
   - Reject multiple signs, letters, and special characters.

---

---

## 🔎 Solution: Using Regular Expression

This solution uses a comprehensive regular expression to match and validate the number format.

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class ValidNumber {
    public static boolean isNumber(String s) {
        // Step 1: Define the regular expression
        String regex = "^\\s*[+-]?((\\d+(\\.\\d*)?)|(\\.\\d+))([eE][+-]?\\d+)?\\s*$";
        
        // Step 2: Compile the pattern
        Pattern pattern = Pattern.compile(regex);
        
        // Step 3: Match the input string against the pattern
        Matcher matcher = pattern.matcher(s);
        
        // Step 4: Return whether the input string is a valid number
        return matcher.matches();
    }
    
    public static void main(String[] args) {
        String s = "0";
        System.out.println("Is Valid Number: " + isNumber(s)); // true
        
        s = " 0.1 ";
        System.out.println("Is Valid Number: " + isNumber(s)); // true
        
        s = "abc";
        System.out.println("Is Valid Number: " + isNumber(s)); // false
        
        s = "1 a";
        System.out.println("Is Valid Number: " + isNumber(s)); // false
        
        s = "2e10";
        System.out.println("Is Valid Number: " + isNumber(s)); // true
        
        s = "-90e3";
        System.out.println("Is Valid Number: " + isNumber(s)); // true
        
        s = "3e+7";
        System.out.println("Is Valid Number: " + isNumber(s)); // true
        
        s = "53.5e93";
        System.out.println("Is Valid Number: " + isNumber(s)); // true
        
        s = "--6";
        System.out.println("Is Valid Number: " + isNumber(s)); // false
        
        s = "-+3";
        System.out.println("Is Valid Number: " + isNumber(s)); // false
        
        s = "95a54e53";
        System.out.println("Is Valid Number: " + isNumber(s)); // false
    }
}
```

---

### 🧠 Explanation:
1. **Regular Expression:**
   - `^` and `$` ensure the entire string matches the pattern.
   - `\\s*` allows optional leading and trailing white spaces.
   - `[+-]?` allows an optional sign at the beginning.
   - `\\d+` matches one or more digits.
   - `(\\.\\d*)?` matches an optional decimal point followed by zero or more digits.
   - `[eE][+-]?\\d+` matches optional scientific notation.
   - `()` and `|` group expressions and indicate alternatives.

2. **Pattern Matching:**
   - `Pattern.compile(regex)` compiles the regular expression.
   - `matcher.matches()` checks if the entire string matches the pattern.

3. **Edge Case Handling:**
   - Leading and trailing white spaces are allowed.
   - Multiple signs (e.g., `--6`, `-+3`) are rejected.
   - Scientific notation (`e` or `E`) is correctly parsed.

---

### 📊 Complexity Analysis:
- **Time Complexity:** `O(n)` where `n` is the length of the string (regular expression matching)
- **Space Complexity:** `O(1)` as no additional space proportional to input size is used

---

---

## 🔥 Test Cases:
```java
Input: "0"
Output: true

Input: " 0.1 "
Output: true

Input: "abc"
Output: false

Input: "1 a"
Output: false

Input: "2e10"
Output: true

Input: "-90e3"
Output: true

Input: "3e+7"
Output: true

Input: "53.5e93"
Output: true

Input: "--6"
Output: false

Input: "-+3"
Output: false

Input: "95a54e53"
Output: false

Input: "6e-1"
Output: true

Input: "4e+"
Output: false
```

---

### 💡 Best Practices:
- Use **regular expressions** for comprehensive input validation.
- Use `Pattern` and `Matcher` for efficient pattern matching.
- Handle edge cases like multiple signs, invalid exponents, and leading/trailing spaces.

---

### ✅ Completed:
- [x] Core Concepts
- [x] Reverse a String
- [x] Check if a String is Palindrome
- [x] Count Vowels and Consonants in a String
- [x] Remove White Spaces from a String
- [x] Find Duplicate Characters in a String
- [x] Longest Palindromic Substring
- [x] Check if Two Strings are Anagrams
- [x] Remove Duplicates from String
- [x] Find All Substrings of a String
- [x] String Rotation Check
- [x] Longest Common Prefix
- [x] Word Break Problem
- [x] Regular Expression Matching
- [x] Find the First Non-Repeated Character
- [x] Minimum Window Substring
- [x] Valid Number (Regular Expression)

---

---

## 🎯 What's Next?

Would you like to:
1. Move to the next question: **Generate All Permutations of a String**?
2. Get more detailed explanations or variations for this problem?
3. Proceed with more advanced-level questions?

---
