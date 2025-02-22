## 🚀 **2.9 Mastering Strings in Java**  

Strings are one of the most commonly used data structures in programming. In Java, strings are immutable, which means once a string is created, it cannot be changed. Let's explore how strings work, common operations, and how to solve real-world problems using strings.

---

## **🔥 2.9.1 What is a String?**  
- **Definition:** A sequence of characters.  
- **Example:** `"Hello, World!"`  
- In Java, strings are objects of the `String` class.  
- Strings are **immutable**, meaning any modification creates a new string.

---

## **🔥 2.9.2 Declaring and Initializing Strings**  
```java
// Using String Literal (Stored in String Pool)
String str1 = "Hello";

// Using new Keyword (Stored in Heap Memory)
String str2 = new String("World");
```

- **String Pool:** A special memory region for storing string literals to optimize memory usage.  
- If a string literal already exists in the pool, Java reuses it instead of creating a new one.

---

## **🔥 2.9.3 Common String Operations**  

### 📘 **1. Length of a String**  
```java
String str = "Hello, World!";
int length = str.length();
System.out.println("Length: " + length);
```

### 📘 **2. Accessing Characters**  
```java
char firstChar = str.charAt(0);  // 'H'
char lastChar = str.charAt(str.length() - 1);  // '!'
```

### 📘 **3. Concatenation**  
```java
String firstName = "John";
String lastName = "Doe";
String fullName = firstName + " " + lastName;
System.out.println(fullName);  // "John Doe"
```

### 📘 **4. Substring**  
```java
String sentence = "Hello, World!";
String subStr = sentence.substring(7, 12);
System.out.println(subStr);  // "World"
```

### 📘 **5. Comparison**  
```java
String strA = "Hello";
String strB = "hello";

// Case-sensitive comparison
boolean isEqual = strA.equals(strB);  // false

// Case-insensitive comparison
boolean isEqualIgnoreCase = strA.equalsIgnoreCase(strB);  // true
```

### 📘 **6. Search**  
```java
String sentence = "Hello, World!";
int index = sentence.indexOf("World");  // 7
int lastIndex = sentence.lastIndexOf("o");  // 8
```

### 📘 **7. Replace**  
```java
String greeting = "Hello, John!";
String newGreeting = greeting.replace("John", "Jane");
System.out.println(newGreeting);  // "Hello, Jane!"
```

### 📘 **8. Convert to Uppercase and Lowercase**  
```java
String text = "Java Programming";
String upper = text.toUpperCase();
String lower = text.toLowerCase();
```

### 📘 **9. Split**  
```java
String sentence = "apple,banana,orange";
String[] fruits = sentence.split(",");
for (String fruit : fruits) {
    System.out.println(fruit);
}
```

---

## **🔥 2.9.4 Example 1: String Operations**  
Let's see an example that demonstrates the most common string operations.  

### 📘 **Code: StringOperations.java**  
```java
public class StringOperations {
    public static void main(String[] args) {
        String sentence = "Hello, Java World!";
        
        // 1. Length of String
        System.out.println("Length: " + sentence.length());

        // 2. Accessing Characters
        System.out.println("First Character: " + sentence.charAt(0));
        System.out.println("Last Character: " + sentence.charAt(sentence.length() - 1));

        // 3. Substring
        System.out.println("Substring (7, 11): " + sentence.substring(7, 11));

        // 4. Concatenation
        String greeting = "Hello";
        String name = "Maya";
        String message = greeting + ", " + name + "!";
        System.out.println("Concatenation: " + message);

        // 5. Comparison
        String str1 = "hello";
        String str2 = "Hello";
        System.out.println("Equals: " + str1.equals(str2));
        System.out.println("Equals Ignore Case: " + str1.equalsIgnoreCase(str2));

        // 6. Search
        System.out.println("Index of 'Java': " + sentence.indexOf("Java"));

        // 7. Replace
        String replaced = sentence.replace("Java", "DSA");
        System.out.println("Replaced: " + replaced);

        // 8. Uppercase and Lowercase
        System.out.println("Uppercase: " + sentence.toUpperCase());
        System.out.println("Lowercase: " + sentence.toLowerCase());

        // 9. Split
        String fruits = "apple,banana,orange";
        String[] fruitArray = fruits.split(",");
        System.out.println("Fruits:");
        for (String fruit : fruitArray) {
            System.out.println(fruit);
        }
    }
}
```

---

### 📊 **Output:**  
```
Length: 17
First Character: H
Last Character: !
Substring (7, 11): Java
Concatenation: Hello, Maya!
Equals: false
Equals Ignore Case: true
Index of 'Java': 7
Replaced: Hello, DSA World!
Uppercase: HELLO, JAVA WORLD!
Lowercase: hello, java world!
Fruits:
apple
banana
orange
```

---

## **🔥 2.9.5 StringBuilder and StringBuffer**  
- **StringBuilder:** Mutable, not thread-safe, faster for single-threaded programs.  
- **StringBuffer:** Mutable, thread-safe, slightly slower due to synchronization.  

### 📘 **Example: Using StringBuilder**  
```java
StringBuilder sb = new StringBuilder("Hello");
sb.append(", World!");
System.out.println(sb.toString());  // "Hello, World!"
```

---

## **🔥 2.9.6 Common Mistakes to Avoid**  
- **Using `==` for String Comparison:** Use `.equals()` instead.  
- **Ignoring Immutability:** Remember, `String` is immutable, so concatenation creates new objects.  
- **Using `String` instead of `StringBuilder` in loops:** It leads to performance issues due to repeated object creation.  

---

## **📝 Exercise Set:**  
1. Write a function to check if a string is a palindrome.  
2. Implement a method to count the occurrence of each character in a string.  
3. Write a function to reverse each word in a given sentence.  
4. Implement a function to find the longest palindrome substring in a string.  
5. Write a function to check if two strings are anagrams.  

---

## 🔥 **Next: Advanced String Manipulation**  
In the next section, we will explore advanced string manipulation techniques, including pattern matching, regular expressions, and solving real-world interview problems.

---

## 🔥 **Ready to Proceed?**  
- Do you feel comfortable with Strings so far?  
- Would you like more examples or explanations on any topic?  
- Were you able to run the example programs successfully?
