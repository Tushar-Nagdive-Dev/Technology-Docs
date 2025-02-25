# Regex Mastery Guide: From Basic to Advanced

## 1. Introduction to Regular Expressions (Regex)
- What is Regex?
- History and Origin
- Why Use Regex?
- Applications of Regex

## 2. Basic Concepts
- Characters and Literals
- Metacharacters: ., *, +, ?, ^, $, (), [], {}, |, \
- Escaping Metacharacters

## 3. Character Classes
- What are Character Classes?
- Predefined Character Classes: \d, \w, \s, \D, \W, \S
- Custom Character Classes: [abc], [^abc]
- Ranges and Negation

## 4. Quantifiers
- What are Quantifiers?
- Greedy vs. Lazy Matching
- Common Quantifiers: *, +, ?, {n}, {n,}, {n,m}
- Examples and Use Cases

## 5. Anchors
- Start and End of String: ^ and $
- Word Boundaries: \b and \B
- Line Anchors in Multiline Mode

## 6. Grouping and Capturing
- Grouping Using ( )
- Capturing Groups
- Non-Capturing Groups: (?: ... )
- Named Capturing Groups: (?<name> ... )
- Backreferences

## 7. Lookahead and Lookbehind Assertions
- Positive Lookahead: (?= ... )
- Negative Lookahead: (?! ... )
- Positive Lookbehind: (?<= ... )
- Negative Lookbehind: (?<! ... )
- Examples and Applications

## 8. Regex Flags and Modifiers
- Global (g), Case Insensitive (i), Multiline (m), Dotall (s), Unicode (u), Sticky (y)
- Combining Flags
- Examples of Using Flags

## 9. Advanced Patterns
- Recursive Patterns
- Conditional Expressions
- Atomic Groups: (?> ... )
- Regex Performance Optimization Tips

## 10. Practical Applications
- Input Validation (Email, Phone, Passwords)
- Search and Replace Operations
- Data Extraction
- Log Analysis
- Web Scraping

## 11. Regex in Different Programming Languages
- JavaScript
- Java
- Python
- PHP
- C#
- Ruby
- Differences and Compatibility

## 12. Testing and Debugging Regex
- Online Regex Testers and Tools
- Debugging Complex Patterns
- Best Practices for Writing Maintainable Regex

## 13. Exercises and Challenges
- Beginner Level
- Intermediate Level
- Advanced Level
- Real-World Scenarios

## 14. Resources and References
- Recommended Books
- Online Tutorials
- Cheat Sheets and Guides

## 15. Summary and Conclusion
- Key Takeaways
- Best Practices
- When Not to Use Regex

---

Great! Let's start with the **Introduction to Regular Expressions (Regex)**.

### **1. What is Regex?**
Regex, short for **Regular Expressions**, is a sequence of characters that forms a search pattern. It is primarily used for pattern matching within strings, allowing you to search, replace, extract, and manipulate text efficiently.

### **2. History and Origin**
Regex originated from formal language theory and computer science, developed by American mathematician **Stephen Cole Kleene** in the 1950s. It was initially used in theoretical studies of formal languages and later adapted into practical applications in programming and text processing.

### **3. Why Use Regex?**
- **Text Searching:** Find specific patterns within large texts (e.g., finding email addresses in a document).
- **Validation:** Ensure input data matches expected formats (e.g., phone numbers, postal codes).
- **Text Manipulation:** Replace or reformat strings efficiently.
- **Data Extraction:** Pull useful information from unstructured data (e.g., parsing logs, scraping web content).

### **4. Applications of Regex**
- **Web Development:** Form validations, URL parsing.
- **Data Analysis:** Extracting data from logs, files, and datasets.
- **System Administration:** Searching through configuration files, log files, and automating tasks.
- **Programming:** String manipulation, input validation, and building robust applications.

### **2. Basic Concepts**

Regex is built using a combination of **characters**, **metacharacters**, and **special constructs**. Let's break down the fundamentals to get a solid understanding.

---

### **1. Characters and Literals**

- **Characters** are the building blocks of regex. They include letters (`a-z`, `A-Z`), digits (`0-9`), and special symbols (e.g., `@`, `#`, `$`, `_`).
- **Literals** are characters that match themselves exactly. For example:
  - `a` matches the character 'a'.
  - `cat` matches the word "cat" exactly.

**Example:**
- Regex: `hello`
- Text: `"hello world"`
- Matches: `"hello"`

---

### **2. Metacharacters**
Metacharacters are special characters that control how patterns are matched. They are powerful but need to be understood well:

| Metacharacter | Description                          | Example                 |
|---------------|--------------------------------------|--------------------------|
| `.`           | Matches any character except newline   | `c.t` → `"cat"`, `"cut"` |
| `*`           | Matches 0 or more occurrences          | `ca*t` → `"ct"`, `"cat"`, `"caaat"` |
| `+`           | Matches 1 or more occurrences          | `ca+t` → `"cat"`, `"caaat"` |
| `?`           | Matches 0 or 1 occurrence (optional)   | `ca?t` → `"ct"`, `"cat"` |
| `^`           | Matches the start of a line or string  | `^cat` → `"cat is here"` but not `"I have a cat"` |
| `$`           | Matches the end of a line or string    | `cat$` → `"I have a cat"` but not `"cat is here"` |
| `()`          | Groups patterns together               | `(ab)+` → `"ab"`, `"abab"` |
| `[]`          | Matches any character inside brackets  | `[aeiou]` → Matches any vowel |
| `{}`          | Specifies exact or range occurrences   | `a{2,4}` → `"aa"`, `"aaa"`, `"aaaa"` |
| `|`           | Alternation (OR)                      | `cat|dog` → `"cat"` or `"dog"` |
| `\\`          | Escapes a metacharacter                | `\\.` → Matches the dot character `.` |

---

### **3. Escaping Metacharacters**
Since metacharacters have special meanings, they need to be escaped with a backslash (`\\`) to be matched literally.

**Examples:**
- To match a dot (`.`) literally, use `\\.` → Matches `"3.14"`.
- To match a question mark (`?`) literally, use `\\?` → Matches `"Are you sure?"`.

---

### **Practical Examples:**

1. Match any 3-letter word:
    ```regex
    \b\w{3}\b
    ```
    - Matches: `"cat"`, `"dog"`, but not `"cats"` or `"a"`

2. Match a number with optional decimal:
    ```regex
    \d+(\.\d+)?
    ```
    - Matches: `"42"`, `"3.14"`, `"100.0"`

3. Match an email address:
    ```regex
    [a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}
    ```
    - Matches: `"test.email@example.com"`

---

### **Hands-on Practice:**
- **Beginner:** Find all words starting with the letter 'a'.
- **Intermediate:** Extract all dates in the format `dd/mm/yyyy`.
- **Advanced:** Validate complex passwords (at least 8 characters, 1 uppercase, 1 number, and 1 special character).

---
### **2. Basic Concepts (Detailed Explanations)**

Regex is all about patterns. Understanding the basic building blocks will give you the power to construct complex patterns and master text manipulation.

---

## **1. Characters and Literals**

### **Characters**
- Characters include alphabets (`a-z`, `A-Z`), digits (`0-9`), and symbols (`@`, `#`, `!`, etc.).
- In regex, characters are treated literally unless they are special symbols (metacharacters).

### **Literals**
- A **literal** is a character that matches itself exactly.
- Regex treats alphabets and numbers as literals unless specified otherwise.

### **Examples:**
1. **Regex:** `cat`  
   **Text:** `"I have a cat"`  
   **Matches:** `"cat"`  
   - This matches exactly the word "cat".

2. **Regex:** `42`  
   **Text:** `"The answer is 42"`  
   **Matches:** `"42"`  
   - It matches the number 42 exactly.

---

## **2. Metacharacters (Detailed Explanation)**

Metacharacters are symbols with special meanings in regex. Here are the most commonly used ones:

### **1. Dot (`.`)**
- Matches any **single character** except a newline.
- It is useful for matching variable characters.

**Example:**
- **Regex:** `c.t`
- **Text:** `"cat"`, `"cut"`, `"c@t"`, `"c-t"`
- **Matches:** All of them because the `.` matches any character.

---

### **2. Asterisk (`*`)**
- Matches **zero or more** occurrences of the preceding character or group.
- It is **greedy** by default, meaning it tries to match as many characters as possible.

**Example:**
- **Regex:** `ca*t`
- **Text:** `"ct"`, `"cat"`, `"caaat"`
- **Matches:** All of them because `a*` can be zero or more `a`s.

---

### **3. Plus (`+`)**
- Matches **one or more** occurrences of the preceding character or group.
- It is also **greedy** by default.

**Example:**
- **Regex:** `ca+t`
- **Text:** `"cat"`, `"caaat"`
- **Matches:** `"cat"`, `"caaat"`
- **Does not match:** `"ct"` because `a+` requires at least one `a`.

---

### **4. Question Mark (`?`)**
- Matches **zero or one** occurrence of the preceding character or group.
- Makes the preceding character optional.

**Example:**
- **Regex:** `ca?t`
- **Text:** `"ct"`, `"cat"`
- **Matches:** `"ct"` and `"cat"`
- **Does not match:** `"caaat"` because `?` allows at most one `a`.

---

### **5. Caret (`^`)**
- Matches the **start** of a line or string.
- It is an **anchor** and does not consume characters.

**Example:**
- **Regex:** `^cat`
- **Text:** `"cat is here"`, `"I have a cat"`
- **Matches:** `"cat is here"`
- **Does not match:** `"I have a cat"` because `cat` is not at the start.

---

### **6. Dollar (`$`)**
- Matches the **end** of a line or string.
- It is also an **anchor** and does not consume characters.

**Example:**
- **Regex:** `cat$`
- **Text:** `"I have a cat"`, `"cat is here"`
- **Matches:** `"I have a cat"`
- **Does not match:** `"cat is here"` because `cat` is not at the end.

---

### **7. Parentheses (`()`)**
- **Groups** patterns together.
- **Captures** the matched text for later use.
- Capturing groups are numbered from left to right.

**Example:**
- **Regex:** `(ab)+`
- **Text:** `"ab"`, `"abab"`, `"ababc"`
- **Matches:** `"ab"`, `"abab"`
- **Does not match:** `"ababc"` because `c` is not part of the group.

---

### **8. Square Brackets (`[]`)**
- Matches **any one character** inside the brackets.
- Can also specify a **range** of characters.

**Example:**
- **Regex:** `[aeiou]`
- **Text:** `"cat"`, `"dog"`, `"fish"`
- **Matches:** `"a"` in `"cat"`, `"o"` in `"dog"`, `"i"` in `"fish"`

**Example with Range:**
- **Regex:** `[a-z]`
- **Matches:** Any lowercase letter from `a` to `z`.

---

### **9. Curly Braces (`{}`)**
- Specifies the **number of occurrences** of the preceding character or group.

**Example:**
- **Regex:** `a{2,4}`
- **Text:** `"aa"`, `"aaa"`, `"aaaa"`, `"aaaaa"`
- **Matches:** `"aa"`, `"aaa"`, `"aaaa"`
- **Does not match:** `"aaaaa"` because the maximum is 4.

**Variations:**
- `{n}` → Exactly `n` occurrences.
- `{n,}` → At least `n` occurrences.
- `{n,m}` → Between `n` and `m` occurrences.

---

### **10. Pipe (`|`)**
- Acts as an **OR** operator between patterns.

**Example:**
- **Regex:** `cat|dog`
- **Text:** `"I have a cat"`, `"I have a dog"`, `"I have a bat"`
- **Matches:** `"cat"` and `"dog"`
- **Does not match:** `"bat"`

---

### **11. Backslash (`\\`)**
- **Escapes** a metacharacter to make it literal.

**Example:**
- **Regex:** `\\$`
- **Text:** `"The price is $10"`
- **Matches:** `"$"` as a literal dollar sign.

---

### **Practical Examples:**

1. **Match any 3-letter word:**
    ```regex
    \b\w{3}\b
    ```
    - Matches: `"cat"`, `"dog"`, but not `"cats"` or `"a"`

2. **Match a number with optional decimal:**
    ```regex
    \d+(\.\d+)?
    ```
    - Matches: `"42"`, `"3.14"`, `"100.0"`

3. **Match an email address:**
    ```regex
    [a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}
    ```
    - Matches: `"test.email@example.com"`

---

### **Hands-on Practice:**
- **Beginner:** Find all words starting with the letter 'a'.
- **Intermediate:** Extract all dates in the format `dd/mm/yyyy`.
- **Advanced:** Validate complex passwords (at least 8 characters, 1 uppercase, 1 number, and 1 special character).

---

## **Summary of Key Points:**
1. **Characters and Literals** are the building blocks.
2. **Metacharacters** add special meaning and control over the pattern.
3. **Escaping Metacharacters** is crucial for matching them literally.

---

### **3. Character Classes**

Character classes are used to **define a set of characters** to match at a particular position in a string. They allow you to specify groups of characters, such as digits, letters, or custom sets.

---

## **1. What are Character Classes?**
Character classes let you define a set of characters to match **one character** in the input string.

### **Syntax:**
```regex
[characters]
```
- Matches any **one character** inside the square brackets.
- Matches exactly **one occurrence** unless combined with a quantifier (e.g., `+`, `*`, `{n}`).

### **Examples:**
1. **Regex:** `[aeiou]`
   - Matches **any one vowel** (`a`, `e`, `i`, `o`, `u`).
   - **Text:** `"cat"`, `"dog"`, `"fish"`
   - **Matches:** `"a"` in `"cat"`, `"o"` in `"dog"`, `"i"` in `"fish"`

2. **Regex:** `[0-9]`
   - Matches **any one digit** from `0` to `9`.
   - **Text:** `"Order 123"`
   - **Matches:** `"1"`, `"2"`, `"3"`

---

## **2. Predefined Character Classes**
Regex provides shorthand notations for commonly used character sets:

| Shorthand | Meaning                        | Equivalent | Example         |
|-----------|--------------------------------|------------|-----------------|
| `\d`      | Any digit (0-9)                 | `[0-9]`    | `\d` → `"5"`    |
| `\D`      | Any non-digit                   | `[^0-9]`   | `\D` → `"a"`    |
| `\w`      | Any word character (letters, digits, underscore) | `[a-zA-Z0-9_]` | `\w` → `"A"`, `"9"`, `"_"` |
| `\W`      | Any non-word character           | `[^a-zA-Z0-9_]` | `\W` → `"!"`, `" "` |
| `\s`      | Any whitespace (space, tab, newline) | `[ \t\n\r\f\v]` | `\s` → `" "` |
| `\S`      | Any non-whitespace character      | `[^ \t\n\r\f\v]` | `\S` → `"A"`, `"9"` |

### **Examples:**
1. **Regex:** `\d+`
   - Matches one or more digits.
   - **Text:** `"Year 2023"`
   - **Matches:** `"2023"`

2. **Regex:** `\w+`
   - Matches one or more word characters (alphanumeric + underscore).
   - **Text:** `"Hello_world123"`
   - **Matches:** `"Hello_world123"`

3. **Regex:** `\s`
   - Matches a single whitespace character.
   - **Text:** `"Hello World"`
   - **Matches:** `" "` (space between "Hello" and "World")

---

## **3. Custom Character Classes**

### **1. Simple Character Class**
You can create a custom set of characters by placing them inside square brackets.

**Example:**
- **Regex:** `[abc]`
  - Matches **either `a`, `b`, or `c`**.
  - **Text:** `"cat"`, `"bat"`, `"rat"`
  - **Matches:** `"a"` in `"cat"`, `"b"` in `"bat"`

---

### **2. Ranges**
- Use a hyphen (`-`) to specify a range of characters.
- Ranges are inclusive, meaning the start and end characters are included.

**Examples:**
1. **Regex:** `[a-z]`
   - Matches **any lowercase letter** from `a` to `z`.
   - **Text:** `"apple"`, `"Banana"`, `"Cat"`
   - **Matches:** All lowercase letters.

2. **Regex:** `[A-Z]`
   - Matches **any uppercase letter** from `A` to `Z`.
   - **Text:** `"Apple"`, `"Banana"`, `"Cat"`
   - **Matches:** `"A"`, `"B"`, `"C"`

3. **Regex:** `[0-9]`
   - Matches **any digit** from `0` to `9`.
   - **Text:** `"12345"`
   - **Matches:** `"1"`, `"2"`, `"3"`, `"4"`, `"5"`

---

### **3. Negation**
- Adding a caret (`^`) as the **first character** inside square brackets negates the character class.
- It matches **any character NOT** in the set.

**Examples:**
1. **Regex:** `[^aeiou]`
   - Matches **any character that is NOT a vowel**.
   - **Text:** `"cat"`, `"dog"`, `"fish"`
   - **Matches:** `"c"`, `"t"`, `"d"`, `"g"`, `"f"`, `"s"`, `"h"`

2. **Regex:** `[^0-9]`
   - Matches **any character that is NOT a digit**.
   - **Text:** `"abc123"`
   - **Matches:** `"a"`, `"b"`, `"c"`

---

### **4. Combining Character Classes**
Character classes can be combined to create complex patterns.

**Examples:**
1. **Regex:** `[a-zA-Z]`
   - Matches **any letter**, lowercase or uppercase.
   - **Text:** `"Hello123"`
   - **Matches:** `"H"`, `"e"`, `"l"`, `"l"`, `"o"`

2. **Regex:** `[a-zA-Z0-9]`
   - Matches **any alphanumeric character**.
   - **Text:** `"User_123"`
   - **Matches:** `"U"`, `"s"`, `"e"`, `"r"`, `"1"`, `"2"`, `"3"`

3. **Regex:** `[A-Za-z0-9._%+-]`
   - Matches **characters commonly used in email addresses**.
   - Useful for email validation.

---

### **5. Practical Examples:**

1. **Match any vowel:**
    ```regex
    [aeiouAEIOU]
    ```
    - Matches any uppercase or lowercase vowel.

2. **Match a hexadecimal digit:**
    ```regex
    [0-9a-fA-F]
    ```
    - Matches any character used in hexadecimal numbers.

3. **Match a single character that is NOT a letter:**
    ```regex
    [^a-zA-Z]
    ```
    - Matches numbers, symbols, spaces, etc.

4. **Match any alphanumeric character or underscore:**
    ```regex
    \w
    ```
    - Matches: `"A"`, `"b"`, `"9"`, `"_"`

---

### **Hands-on Practice:**
- **Beginner:** Match all vowels in a given sentence.
- **Intermediate:** Extract all numbers from a string.
- **Advanced:** Match all words that start with a consonant.

---

## **Summary of Key Points:**
1. **Character Classes** allow you to specify a set of characters.
2. **Predefined Character Classes** provide shortcuts for common sets (e.g., `\d`, `\w`, `\s`).
3. **Custom Character Classes** enable flexible and complex pattern matching.
4. **Ranges** and **Negation** add power to custom character classes.

---

### **4. Quantifiers**

Quantifiers are used in regex to specify **how many times** a character, character class, or group should be matched. They allow you to control the repetition of patterns and make your regex more flexible and powerful.

---

## **1. What are Quantifiers?**
Quantifiers define the number of occurrences of a pattern to be matched. They can be **greedy**, **lazy**, or **possessive**, depending on how they consume characters.

### **Types of Quantifiers:**
1. **Greedy Quantifiers:** Try to match as many occurrences as possible.
2. **Lazy Quantifiers:** Try to match as few occurrences as possible.
3. **Possessive Quantifiers:** Match as many occurrences as possible without backtracking (available in some regex engines like Java).

---

## **2. Common Quantifiers**

| Quantifier | Description                        | Example                 | Matches                          |
|------------|------------------------------------|--------------------------|----------------------------------|
| `*`        | Matches **0 or more** occurrences   | `ca*t`                   | `"ct"`, `"cat"`, `"caaaat"`      |
| `+`        | Matches **1 or more** occurrences   | `ca+t`                   | `"cat"`, `"caaaat"`              |
| `?`        | Matches **0 or 1** occurrence (optional) | `ca?t`                   | `"ct"`, `"cat"`                  |
| `{n}`      | Matches **exactly `n`** occurrences | `a{3}`                   | `"aaa"`                          |
| `{n,}`     | Matches **at least `n`** occurrences | `a{2,}`                  | `"aa"`, `"aaa"`, `"aaaa"`        |
| `{n,m}`    | Matches **between `n` and `m`** occurrences | `a{2,4}`               | `"aa"`, `"aaa"`, `"aaaa"`        |

---

## **3. Greedy vs. Lazy Matching**

### **Greedy Quantifiers**
- Try to match **as many characters as possible**.
- They are the default behavior in most regex engines.

### **Lazy Quantifiers**
- Try to match **as few characters as possible**.
- Achieved by adding a `?` after the quantifier.

| Greedy Quantifier | Lazy Version     | Description                               |
|-------------------|------------------|-------------------------------------------|
| `*`               | `*?`             | Matches **0 or more**, as few as possible  |
| `+`               | `+?`             | Matches **1 or more**, as few as possible  |
| `?`               | `??`             | Matches **0 or 1**, as few as possible     |
| `{n,}`            | `{n,}?`          | Matches **at least `n`**, as few as possible|
| `{n,m}`           | `{n,m}?`         | Matches **between `n` and `m`**, as few as possible|

---

### **Examples:**

1. **Greedy Matching Example:**
    ```regex
    <.*>
    ```
    - **Text:** `<b>Bold</b> and <i>Italic</i>`
    - **Matches:** `<b>Bold</b> and <i>Italic</i>`
    - It matches the **longest possible** string between the first `<` and the last `>`.

2. **Lazy Matching Example:**
    ```regex
    <.*?>
    ```
    - **Text:** `<b>Bold</b> and <i>Italic</i>`
    - **Matches:** `<b>` and `<i>`
    - It matches the **shortest possible** string for each pair of tags.

---

## **4. Using Quantifiers in Practical Scenarios**

1. **Zero or More (`*`)**
    - Matches zero or more occurrences of the preceding character or group.

    **Example:**
    ```regex
    ab*c
    ```
    - **Text:** `"ac"`, `"abc"`, `"abbc"`, `"abbbc"`
    - **Matches:** All of them because `b*` matches zero or more `b`s.

---

2. **One or More (`+`)**
    - Matches one or more occurrences of the preceding character or group.

    **Example:**
    ```regex
    ab+c
    ```
    - **Text:** `"abc"`, `"abbc"`, `"abbbc"`
    - **Matches:** All except `"ac"` because `b+` requires at least one `b`.

---

3. **Zero or One (`?`)**
    - Matches zero or one occurrence of the preceding character or group.
    - Makes the character **optional**.

    **Example:**
    ```regex
    colou?r
    ```
    - **Text:** `"color"`, `"colour"`
    - **Matches:** Both because `u?` is optional.

---

4. **Exact Number (`{n}`)**
    - Matches exactly `n` occurrences of the preceding character or group.

    **Example:**
    ```regex
    a{3}
    ```
    - **Text:** `"aaa"`, `"aaaa"`, `"aa"`
    - **Matches:** Only `"aaa"` because exactly three `a`s are required.

---

5. **At Least (`{n,}`)**
    - Matches at least `n` occurrences of the preceding character or group.

    **Example:**
    ```regex
    a{2,}
    ```
    - **Text:** `"aa"`, `"aaa"`, `"aaaa"`
    - **Matches:** All of them because at least two `a`s are present.

---

6. **Range (`{n,m}`)**
    - Matches between `n` and `m` occurrences of the preceding character or group.

    **Example:**
    ```regex
    a{2,4}
    ```
    - **Text:** `"aa"`, `"aaa"`, `"aaaa"`, `"aaaaa"`
    - **Matches:** `"aa"`, `"aaa"`, `"aaaa"`
    - **Does not match:** `"aaaaa"` because the maximum is 4.

---

## **5. Practical Examples:**

1. **Match an optional decimal point:**
    ```regex
    \d+(\.\d+)?  
    ```
    - Matches: `"42"`, `"3.14"`, `"100.0"`

2. **Match a phone number format:**
    ```regex
    \d{3}-\d{3}-\d{4}
    ```
    - Matches: `"123-456-7890"`

3. **Match a word with optional ending:**
    ```regex
    colou?r
    ```
    - Matches: `"color"`, `"colour"`

4. **Match multiple whitespace characters:**
    ```regex
    \s+
    ```
    - Matches: `" "` (spaces), `"\t"` (tabs), and `"\n"` (newlines)

---

## **6. Best Practices:**
1. Use **greedy quantifiers** for maximal matching but be cautious of over-matching.
2. Use **lazy quantifiers** when you need the shortest match.
3. Combine quantifiers with anchors (`^`, `$`) for precise control.
4. Always **test your regex** with sample data to ensure the expected behavior.

---

## **7. Hands-on Practice:**
- **Beginner:** Match all words ending in "ing".
- **Intermediate:** Extract all email addresses from a document.
- **Advanced:** Validate complex passwords (at least 8 characters, 1 uppercase, 1 number, and 1 special character).

---

## **Summary of Key Points:**
1. **Quantifiers** control the number of repetitions for a pattern.
2. **Greedy Quantifiers** match as much as possible.
3. **Lazy Quantifiers** match as little as possible.
4. **Exact and Range Quantifiers** allow for precise control.

---

### **5. Anchors**

Anchors are special characters in regex that **do not match any characters** but assert a position in a string. They are used to specify the position of a pattern within the text, ensuring that the match happens at the start, end, or specific boundary of a word or line.

---

## **1. What are Anchors?**
Anchors help you **pinpoint the location** of a pattern within a string. Unlike other regex components, they don’t consume characters but instead check a position.

### **Types of Anchors:**
1. **Start of String or Line (`^`)**
2. **End of String or Line (`$`)**
3. **Word Boundaries (`\\b` and `\\B`)**
4. **Line Anchors in Multiline Mode**

---

## **2. Start of String or Line (`^`)**

### **Description:**
- `^` asserts that the match must start at the **beginning of a string** or **beginning of a line** (in multiline mode).
- It is an **anchor** and does not consume any characters.

### **Example:**
- **Regex:** `^cat`
- **Text:** 
    ```
    cat is here
    I have a cat
    ```
- **Matches:** `"cat"` in the first line.
- **Does not match:** `"cat"` in the second line because it's not at the start.

---

## **3. End of String or Line (`$`)**

### **Description:**
- `$` asserts that the match must be at the **end of a string** or **end of a line** (in multiline mode).
- It is an **anchor** and does not consume any characters.

### **Example:**
- **Regex:** `cat$`
- **Text:**
    ```
    I have a cat
    cat is here
    ```
- **Matches:** `"cat"` in the first line.
- **Does not match:** `"cat"` in the second line because it's not at the end.

---

### **Combining `^` and `$`**
You can use both anchors to match the **entire string** exactly.

**Example:**
- **Regex:** `^cat$`
- **Text:** `"cat"`, `"cats"`, `"cat is here"`
- **Matches:** Only `"cat"` because it must be the entire string.

---

## **4. Word Boundaries (`\\b` and `\\B`)**

### **Word Boundary (`\\b`)**
- Matches a position **between a word character** (`[a-zA-Z0-9_]`) **and a non-word character**.
- It is useful for matching **whole words** and preventing partial matches.

**Example:**
- **Regex:** `\\bcat\\b`
- **Text:** `"cat"`, `"catapult"`, `"bobcat"`, `"cat!"`
- **Matches:** `"cat"` and `"cat!"`
- **Does not match:** `"catapult"`, `"bobcat"` because they are part of a larger word.

---

### **Non-Word Boundary (`\\B`)**
- Matches a position **not at a word boundary**.
- It is the **inverse** of `\\b`.

**Example:**
- **Regex:** `\\Bcat\\B`
- **Text:** `"cat"`, `"scat"`, `"catapult"`
- **Matches:** `"cat"` in `"scatapult"`
- **Does not match:** `"cat"` as a standalone word.

---

## **5. Line Anchors in Multiline Mode**

### **Multiline Mode (`m` Flag)**
- In **multiline mode**, `^` and `$` match the start and end of **each line**, not just the start and end of the string.
- Useful for processing multi-line text.

### **Example:**
- **Regex:** `^cat`
- **Text:**
    ```
    cat is here
    dog is here
    cat is gone
    ```
- **Matches:**
  - `"cat"` in the first line
  - `"cat"` in the third line

- **Without `m` Flag:** Matches only the first occurrence at the start of the string.
- **With `m` Flag:** Matches at the start of every line.

---

## **6. Practical Examples:**

1. **Match lines that start with a specific word:**
    ```regex
    ^Error
    ```
    - **Text:**
        ```
        Error: File not found
        Warning: Low disk space
        Error: Access denied
        ```
    - **Matches:** Lines starting with `"Error"`

---

2. **Match lines that end with a punctuation:**
    ```regex
    .[.!?]$
    ```
    - **Text:**
        ```
        This is a sentence.
        This is a question?
        This is a statement
        ```
    - **Matches:**
      - `"This is a sentence."`
      - `"This is a question?"`
    - **Does not match:** `"This is a statement"`

---

3. **Match whole words only:**
    ```regex
    \bcat\b
    ```
    - **Text:** `"cat"`, `"catapult"`, `"bobcat"`, `"cat!"`
    - **Matches:** `"cat"` and `"cat!"`
    - **Does not match:** `"catapult"`, `"bobcat"`

---

4. **Match non-words:**
    ```regex
    \Bcat\B
    ```
    - **Text:** `"concatenate"`, `"bobcat"`, `"catapult"`
    - **Matches:** `"cat"` in `"concatenate"`
    - **Does not match:** `"cat"` as a standalone word

---

## **7. Hands-on Practice:**
- **Beginner:** Find all lines that start with a specific word.
- **Intermediate:** Extract sentences ending with a question mark.
- **Advanced:** Match all occurrences of a word, but not as a prefix or suffix.

---

## **8. Summary of Key Points:**
1. **Anchors** specify the position of a pattern within the text.
2. `^` matches the **start** of a line or string.
3. `$` matches the **end** of a line or string.
4. `\\b` matches a **word boundary**, while `\\B` matches a **non-word boundary**.
5. Using the `m` flag enables multiline mode for line-based matching.

---

## **Best Practices:**
1. Use `^` and `$` carefully to avoid unintended matches.
2. Combine anchors with word boundaries (`\\b`) for precise word matching.
3. Test regex with multiline strings when using the `m` flag.

---

### **6. Grouping and Capturing**

Grouping and capturing are powerful techniques in regex that allow you to **group patterns**, **capture matched text**, and **reuse** captured values. They also help organize complex expressions and apply quantifiers to entire groups.

---

## **1. What is Grouping?**
Grouping allows you to **combine multiple characters or expressions** into a single unit. This is useful when you want to:
- Apply a quantifier to the entire group.
- Use alternation (`|`) more effectively.
- Capture matched text for later use.

### **Syntax:**
```regex
(pattern)
```
- Parentheses `()` are used to **group** patterns.
- By default, they also **capture** the matched text.

---

### **Example:**
- **Regex:** `(ab)+`
  - Matches one or more occurrences of `"ab"`.
- **Text:** `"ab"`, `"abab"`, `"ababab"`, `"abc"`
- **Matches:** `"ab"`, `"abab"`, `"ababab"`
- **Does not match:** `"abc"` because `c` is not part of the group.

---

## **2. Capturing Groups**

### **Description:**
- Capturing groups **store matched text** for later use.
- Each group is **numbered** from left to right, starting at 1.
- The **entire match** is stored in group 0.

### **Syntax:**
```regex
(pattern)
```
- Text matched by `pattern` is stored as a captured group.
- Use `\1`, `\2`, etc., to **reference** captured groups.

### **Example:**
- **Regex:** `(\d{3})-(\d{3})-(\d{4})`
  - Matches a phone number in the format `123-456-7890`.
- **Text:** `"Phone: 123-456-7890"`
- **Matches:** `"123-456-7890"`
- **Captured Groups:**
  - Group 0: `"123-456-7890"` (entire match)
  - Group 1: `"123"` (first set of digits)
  - Group 2: `"456"` (second set of digits)
  - Group 3: `"7890"` (third set of digits)

---

## **3. Non-Capturing Groups**

### **Description:**
- Sometimes you want to group patterns **without capturing** the matched text.
- Use `(?: ... )` to create a **non-capturing group**.
- Useful for **structuring** complex patterns without capturing unnecessary data.

### **Syntax:**
```regex
(?:pattern)
```
- Matches the `pattern` but **does not** capture the text.

### **Example:**
- **Regex:** `(?:ab)+`
  - Matches one or more occurrences of `"ab"`.
  - **Does not capture** the matched text.
- **Text:** `"ababab"`
- **Matches:** `"ababab"`
- **No Captured Groups:** No groups are stored because of `?:`

---

## **4. Named Capturing Groups**

### **Description:**
- Named capturing groups allow you to **name** groups instead of using numbered references.
- This makes complex regex **easier to read and maintain**.

### **Syntax:**
```regex
(?<name>pattern)
```
- Matches the `pattern` and stores it in a **named group** called `name`.
- Use `\k<name>` to **reference** the named group.

### **Example:**
- **Regex:** `(?<area>\d{3})-(?<exchange>\d{3})-(?<line>\d{4})`
  - Matches a phone number and stores parts in named groups.
- **Text:** `"Phone: 123-456-7890"`
- **Matches:** `"123-456-7890"`
- **Named Groups:**
  - `area`: `"123"`
  - `exchange`: `"456"`
  - `line`: `"7890"`

---

## **5. Backreferences**

### **Description:**
- Backreferences allow you to **reuse captured groups** later in the same pattern.
- They are useful for **finding repeated text** or ensuring **consistency**.

### **Syntax:**
```regex
\1, \2, ... 
```
- `\1` refers to the **first captured group**, `\2` to the second, and so on.
- Named groups can be referenced using `\k<name>`.

### **Example:**
- **Regex:** `(\w+)\s+\1`
  - Matches a repeated word.
- **Text:** `"hello hello"`, `"test test"`, `"foo bar"`
- **Matches:**
  - `"hello hello"`
  - `"test test"`
- **Does not match:** `"foo bar"` because the words are not identical.

### **Example with Named Backreference:**
- **Regex:** `(?<word>\w+)\s+\k<word>`
  - Matches a repeated word using a named group.
- **Text:** `"hello hello"`, `"test test"`, `"foo bar"`
- **Matches:** Same as above.

---

## **6. Practical Applications**

1. **Extracting Parts of a String:**
    ```regex
    (\d{4})-(\d{2})-(\d{2})
    ```
    - Matches dates in the format `YYYY-MM-DD`.
    - Captures:
      - Group 1: Year
      - Group 2: Month
      - Group 3: Day

2. **Swapping Words Using Backreferences:**
    ```regex
    (\w+)\s+(\w+)
    ```
    - Matches two words separated by whitespace.
    - Replacement: `\2 \1` → Swaps the words.

3. **Validating Consistency:**
    ```regex
    <(\w+)>.*?</\1>
    ```
    - Matches HTML tags and ensures the **opening and closing tags are the same**.
    - Example: Matches `<b>text</b>` but not `<b>text</i>`.

---

## **7. Best Practices:**
1. Use **non-capturing groups** when you don't need to capture text.
2. Name your capturing groups for better readability and maintainability.
3. Use **backreferences** to ensure consistency and repeated patterns.
4. Test your regex with **complex strings** to ensure correct grouping and capturing.

---

## **8. Hands-on Practice:**
- **Beginner:** Extract the area code from a phone number.
- **Intermediate:** Swap first and last names in a list.
- **Advanced:** Validate nested HTML tags.

---

## **9. Summary of Key Points:**
1. **Grouping** allows combining patterns into a single unit.
2. **Capturing Groups** store matched text for later use.
3. **Non-Capturing Groups** structure complex patterns without capturing.
4. **Named Capturing Groups** make regex easier to read and maintain.
5. **Backreferences** reuse captured groups for consistency and repetition.

---

### **7. Lookahead and Lookbehind Assertions**

Lookahead and Lookbehind are advanced regex features that allow you to **assert** the presence or absence of patterns **without consuming characters**. They check for conditions before or after a match, enabling you to create powerful and flexible patterns.

---

## **1. What are Lookaheads and Lookbehinds?**
- Lookaheads and Lookbehinds are **zero-width assertions**, meaning they **do not consume characters** but assert whether a pattern exists.
- They allow you to check for patterns **before** or **after** a match **without including them** in the match.

### **Types of Assertions:**
1. **Positive Lookahead (`?=`)**
2. **Negative Lookahead (`?!`)**
3. **Positive Lookbehind (`?<=`)**
4. **Negative Lookbehind (`?<!`)**

---

## **2. Positive Lookahead (`?=`)**

### **Description:**
- Asserts that a pattern **must be present** **after** the current position.
- The lookahead pattern is checked, but **not included** in the match.

### **Syntax:**
```regex
pattern(?=lookahead)
```
- Matches `pattern` only if it is **followed by** `lookahead`.

### **Example:**
- **Regex:** `\d{3}(?= dollars)`
  - Matches **three digits** only if they are **followed by** the word `"dollars"`.
- **Text:** `"I have 100 dollars"`, `"She paid 200 euros"`
- **Matches:** `"100"` (from `"100 dollars"`)
- **Does not match:** `"200"` (from `"200 euros"`) because it's not followed by `"dollars"`.

---

### **Common Use Cases:**
1. **Match a word only if it's followed by another word:**
    ```regex
    \bcat\b(?= eats)
    ```
    - Matches `"cat"` only if followed by `"eats"`.

2. **Match digits only if followed by a unit:**
    ```regex
    \d+(?= kg)
    ```
    - Matches numbers followed by `"kg"`.

---

## **3. Negative Lookahead (`?!`)**

### **Description:**
- Asserts that a pattern **must NOT be present** **after** the current position.
- The negative lookahead pattern is checked, but **not included** in the match.

### **Syntax:**
```regex
pattern(?!lookahead)
```
- Matches `pattern` only if it is **NOT followed by** `lookahead`.

### **Example:**
- **Regex:** `cat(?! eats)`
  - Matches `"cat"` only if it is **NOT followed by** `"eats"`.
- **Text:** `"cat sleeps"`, `"cat eats"`, `"cat meows"`
- **Matches:** `"cat"` in `"cat sleeps"` and `"cat meows"`
- **Does not match:** `"cat eats"` because `"eats"` follows `"cat"`.

---

### **Common Use Cases:**
1. **Exclude a pattern:**
    ```regex
    \bapple\b(?! pie)
    ```
    - Matches `"apple"` but **not** `"apple pie"`.

2. **Match a word only if it's NOT followed by another word:**
    ```regex
    \bhello\b(?! world)
    ```
    - Matches `"hello"` but **not** `"hello world"`.

---

## **4. Positive Lookbehind (`?<=`)**

### **Description:**
- Asserts that a pattern **must be present** **before** the current position.
- The lookbehind pattern is checked, but **not included** in the match.

### **Syntax:**
```regex
(?<=lookbehind)pattern
```
- Matches `pattern` only if it is **preceded by** `lookbehind`.

### **Example:**
- **Regex:** `(?<=\$)\d+`
  - Matches **digits** only if they are **preceded by** a dollar sign (`$`).
- **Text:** `"Price: $100"`, `"Cost: 100 euros"`
- **Matches:** `"100"` (from `"$100"`)
- **Does not match:** `"100"` (from `"100 euros"`) because it's not preceded by `$`.

---

### **Common Use Cases:**
1. **Match a word only if it is preceded by another word:**
    ```regex
    (?<=Mr\. )\w+
    ```
    - Matches a name only if preceded by `"Mr. "`.

2. **Extract numbers preceded by a currency symbol:**
    ```regex
    (?<=€)\d+
    ```
    - Matches numbers preceded by the euro symbol (`€`).

---

## **5. Negative Lookbehind (`?<!`)**

### **Description:**
- Asserts that a pattern **must NOT be present** **before** the current position.
- The negative lookbehind pattern is checked, but **not included** in the match.

### **Syntax:**
```regex
(?<!lookbehind)pattern
```
- Matches `pattern` only if it is **NOT preceded by** `lookbehind`.

### **Example:**
- **Regex:** `(?<!\$)\d+`
  - Matches **digits** only if they are **NOT preceded by** a dollar sign (`$`).
- **Text:** `"Price: $100"`, `"Cost: 100 euros"`
- **Matches:** `"100"` (from `"100 euros"`)
- **Does not match:** `"100"` (from `"$100"`) because it's preceded by `$`.

---

### **Common Use Cases:**
1. **Exclude a pattern from the match:**
    ```regex
    (?<!@)\busername\b
    ```
    - Matches `"username"` but **not** `"@username"`.

2. **Match numbers not preceded by a currency symbol:**
    ```regex
    (?<!\$)\d+
    ```
    - Matches numbers not preceded by `$`.

---

## **6. Practical Examples:**

1. **Match a word only if followed by a punctuation mark:**
    ```regex
    \bword\b(?=[.,!?])
    ```
    - Matches `"word"` only if followed by `.` `,` `!` or `?`.

2. **Match numbers not followed by a percentage symbol:**
    ```regex
    \d+(?!%)
    ```
    - Matches numbers that are not percentages.

3. **Match a username only if NOT preceded by an @ symbol:**
    ```regex
    (?<!@)\busername\b
    ```
    - Matches `"username"` but **not** `"@username"`.

4. **Match a number only if preceded by a dollar sign:**
    ```regex
    (?<=\$)\d+
    ```
    - Matches: `$100`, `$200`, but **not** `100` or `200`.

---

## **7. Best Practices:**
1. Use **Lookaheads** when you want to check **forward** without consuming characters.
2. Use **Lookbehinds** when you want to check **backward** without consuming characters.
3. Combine positive and negative assertions for **complex conditions**.
4. Not all regex engines support **Lookbehinds** (e.g., JavaScript does not support them natively).

---

## **8. Hands-on Practice:**
- **Beginner:** Match words only if followed by a comma.
- **Intermediate:** Extract prices only in dollars.
- **Advanced:** Validate numbers that are not percentages or negative values.

---

## **9. Summary of Key Points:**
1. **Positive Lookahead (`?=`)** checks for the presence of a pattern **after** the match.
2. **Negative Lookahead (`?!`)** checks for the **absence** of a pattern **after** the match.
3. **Positive Lookbehind (`?<=`)** checks for the presence of a pattern **before** the match.
4. **Negative Lookbehind (`?<!`)** checks for the **absence** of a pattern **before** the match.
5. Lookaheads and Lookbehinds are **zero-width assertions** that **do not consume characters**.

---

### **8. Regex Flags and Modifiers**

Regex flags and modifiers are special options that **change the behavior** of regex patterns. They allow you to:
- Perform **case insensitive** matching.
- Enable **multiline** mode.
- Make `.` match **newline characters**.
- Optimize pattern matching with **sticky** and **global** flags.

---

## **1. What are Regex Flags?**
Flags are **optional parameters** that modify the default behavior of regex patterns. They are usually placed **after** the closing delimiter of the pattern.

### **Syntax:**
```regex
pattern[flags]
```
- In JavaScript, flags are placed at the end:
    ```js
    /pattern/flags
    ```
- In other languages like Python, they are passed as additional arguments:
    ```python
    re.compile(pattern, flags)
    ```

---

## **2. Common Regex Flags**

| Flag | Description                            | Syntax (JavaScript) | Syntax (Python)  | Example Usage           |
|------|----------------------------------------|---------------------|------------------|--------------------------|
| `i`  | **Case Insensitive** matching           | `/pattern/i`        | `re.IGNORECASE`  | `/cat/i` matches `"Cat"`, `"cat"`, `"CAT"` |
| `g`  | **Global** search (find all matches)    | `/pattern/g`        | Not Applicable   | `/cat/g` finds all occurrences of `"cat"` |
| `m`  | **Multiline** mode for `^` and `$`      | `/pattern/m`        | `re.MULTILINE`   | `^cat` matches `"cat"` at the start of each line |
| `s`  | **Dotall** mode (`.` matches newline)   | `/pattern/s`        | `re.DOTALL`      | `/a.b/s` matches `"a\nb"` |
| `u`  | **Unicode** mode (Unicode support)      | `/pattern/u`        | Not Required     | `/\u{1F600}/u` matches 😀 |
| `y`  | **Sticky** mode (exact position match)  | `/pattern/y`        | Not Available    | `/cat/y` only matches `"cat"` at the exact position |

---

## **3. Detailed Explanation of Flags**

### **1. Case Insensitive (`i`)**
- Makes the pattern **case insensitive**.
- Matches letters regardless of their case (`A` = `a`).

### **Example:**
- **Regex:** `/cat/i`
- **Text:** `"Cat"`, `"cat"`, `"CAT"`, `"cAt"`
- **Matches:** All of them because case is ignored.

### **Use Case:**
- Searching for keywords without worrying about capitalization.
- Example: Finding `"error"` regardless of whether it's written as `"Error"`, `"ERROR"`, or `"error"`.

---

### **2. Global (`g`)**
- Enables a **global search**.
- Finds **all matches** rather than stopping at the first match.

### **Example:**
- **Regex:** `/cat/g`
- **Text:** `"cat cat cat"`
- **Matches:** All occurrences of `"cat"`.

### **Use Case:**
- Counting occurrences of a pattern.
- Replacing all instances of a substring.

---

### **3. Multiline (`m`)**
- Changes the behavior of `^` and `$` anchors:
  - `^` matches the **start of each line**.
  - `$` matches the **end of each line**.
- Without `m`, `^` and `$` match the start and end of the **entire string**.

### **Example:**
- **Regex:** `/^cat/m`
- **Text:**
    ```
    cat is here
    dog is here
    cat is gone
    ```
- **Matches:**
  - `"cat"` in the first line
  - `"cat"` in the third line
- **Does not match:** `"cat"` in the middle of the second line.

### **Use Case:**
- Parsing multi-line input (e.g., logs, CSV files).

---

### **4. Dotall (`s`)**
- Changes the behavior of the dot (`.`) metacharacter:
  - `.` matches **any character except a newline** by default.
  - With `s`, `.` matches **any character including newlines**.

### **Example:**
- **Regex:** `/a.b/s`
- **Text:** `"a\nb"`, `"acb"`
- **Matches:**
  - `"a\nb"` (because `.` matches the newline in Dotall mode)
  - `"acb"`

### **Use Case:**
- Matching multi-line patterns, such as parsing paragraphs or HTML tags spanning multiple lines.

---

### **5. Unicode (`u`)**
- Enables **full Unicode support**.
- Allows matching of Unicode characters using escape sequences.

### **Example:**
- **Regex:** `/\u{1F600}/u`
- **Text:** `"😀"`
- **Matches:** The Unicode emoji 😀.

### **Use Case:**
- Matching emojis, special symbols, and international characters.

---

### **6. Sticky (`y`)**
- Matches only at the **exact position** in the string.
- Unlike global (`g`), which searches through the entire string, `y` **starts at the last index**.

### **Example:**
- **Regex:** `/cat/y`
- **Text:** `"cat cat cat"`
- **Matches:** Only the **first occurrence**.
- **Does not match** the second and third `"cat"` because they are not at the exact position.

### **Use Case:**
- When you need to **incrementally** match a pattern at a specific position.
- Useful for tokenizing strings.

---

## **7. Combining Multiple Flags**
You can combine multiple flags by placing them together.

### **Example:**
- **Regex:** `/cat/gi`
  - `g` → Global search (find all matches)
  - `i` → Case insensitive
- **Text:** `"Cat cat CAT"`
- **Matches:** All occurrences regardless of case.

### **Use Case:**
- Searching and replacing multiple instances of a word without case sensitivity.

---

## **8. Practical Examples:**

1. **Case Insensitive Matching:**
    ```regex
    /hello/i
    ```
    - Matches: `"Hello"`, `"HELLO"`, `"hello"`

2. **Global Search:**
    ```regex
    /dog/g
    ```
    - Matches all occurrences of `"dog"` in the text.

3. **Multiline Matching:**
    ```regex
    /^error/m
    ```
    - Matches `"error"` at the start of each line.

4. **Dotall Mode:**
    ```regex
    /a.*b/s
    ```
    - Matches:
      ```
      a
      newline
      b
      ```

5. **Unicode Matching:**
    ```regex
    /\u{1F600}/u
    ```
    - Matches the emoji 😀.

---

## **9. Best Practices:**
1. Use the `i` flag for **case insensitive** searches.
2. Use the `g` flag to **find all matches** instead of just the first.
3. Combine `m` and `s` for **multi-line matching**.
4. Use `u` for **Unicode support**, especially when dealing with international characters or emojis.
5. Test complex patterns with different flags to ensure expected behavior.

---

## **10. Hands-on Practice:**
- **Beginner:** Match all occurrences of a word regardless of case.
- **Intermediate:** Extract email addresses from multi-line text.
- **Advanced:** Match multi-line HTML tags using Dotall mode.

---

## **11. Summary of Key Points:**
1. **Flags** modify the default behavior of regex patterns.
2. **Case Insensitive (`i`)** ignores case differences.
3. **Global (`g`)** finds all matches.
4. **Multiline (`m`)** changes the behavior of `^` and `$`.
5. **Dotall (`s`)** allows `.` to match newlines.
6. **Unicode (`u`)** supports matching Unicode characters.
7. **Sticky (`y`)** matches exactly at the current position.

---

### **9. Advanced Patterns**

Advanced patterns in regex allow you to build more **complex and efficient expressions**. They provide powerful tools to handle recursive structures, conditional logic, and optimize performance, making regex adaptable for sophisticated text processing tasks.

---

## **1. What are Advanced Patterns?**
Advanced patterns enable you to:
- Handle **recursive** and **nested structures**.
- Use **conditional logic** to match different patterns.
- Optimize performance with **atomic groups**.

### **Types of Advanced Patterns:**
1. **Recursive Patterns**
2. **Conditional Expressions**
3. **Atomic Groups**
4. **Performance Optimization Tips**

---

## **2. Recursive Patterns**

### **Description:**
- Recursive patterns allow you to **recurse into the same regex pattern**.
- They are useful for matching **nested structures** like parentheses, brackets, or HTML tags.

### **Syntax (PCRE or Perl Compatible Regex):**
```regex
(?R)
```
- `(?R)` recursively matches the **entire pattern**.

### **Example:**
- **Regex:** `\((?:[^()]+|(?R))*\)`
  - Matches **balanced parentheses**.
  - Handles **nested** parentheses recursively.
- **Text:** 
    ```
    (abc)
    (a(bc)d)
    ((a)(b))
    (a(b(c)d)e)
    ```
- **Matches:**
  - `"(abc)"`
  - `"(a(bc)d)"`
  - `"((a)(b))"`
  - `"(a(b(c)d)e)"`

### **Use Case:**
- Parsing **nested parentheses** or **bracketed expressions**.
- Matching **HTML/XML tags** with nested elements.

### **Important Notes:**
- Not all regex engines support recursive patterns.
- Supported in **PCRE, Perl, and some implementations in PHP and Python**.

---

## **3. Conditional Expressions**

### **Description:**
- Conditional expressions allow you to **conditionally match patterns** based on the presence or value of a **capturing group**.
- Useful for matching **different patterns** depending on context.

### **Syntax:**
```regex
(?(condition)true-pattern|false-pattern)
```
- **Condition**:
  - A backreference (e.g., `\1`) checks if the group is matched.
  - A lookahead or lookbehind condition.
- **True-pattern**: Pattern to match if the condition is **true**.
- **False-pattern**: Pattern to match if the condition is **false** (optional).

---

### **Example 1: Simple Condition**
- **Regex:** `(a)?b(?(1)c|d)`
  - If `a` is matched, then match `c`.
  - If `a` is **not** matched, then match `d`.
- **Text:** `"abc"`, `"bd"`
- **Matches:**
  - `"abc"` (because `a` is matched, followed by `c`)
  - `"bd"` (because `a` is **not** matched, followed by `d`)

### **Example 2: Matching Quotes Consistently**
- **Regex:** 
    ```regex
    (['"])(.*?)\1
    ```
  - Matches a quoted string with **consistent opening and closing quotes**.
  - Uses a backreference to ensure the same quote type is used.
- **Text:** `"hello"`, `'world'`, `"mismatch'`
- **Matches:**
  - `"hello"`
  - `'world'`
- **Does not match:** `"mismatch'` because the quotes are inconsistent.

### **Use Case:**
- Matching **quoted strings** consistently.
- Handling **optional elements** with different conditions.

---

## **4. Atomic Groups**

### **Description:**
- Atomic groups are **non-backtracking** groups.
- Once an atomic group is matched, the regex engine **does not backtrack** inside the group.
- This improves **performance** by preventing unnecessary backtracking.

### **Syntax:**
```regex
(?>pattern)
```
- Matches the `pattern` **without backtracking**.

### **Example:**
- **Regex:** `(?>\d{3})\d{2}`
  - Matches exactly **5 digits** but **without backtracking** in the first 3 digits.
- **Text:** `"12345"`, `"12345"`, `"1234"`
- **Matches:** `"12345"`
- **Does not match:** `"1234"` because it requires exactly 5 digits.

---

### **Why Use Atomic Groups?**
- To **optimize performance** by eliminating unnecessary backtracking.
- To **improve efficiency** in complex patterns with multiple possibilities.
- To **avoid catastrophic backtracking** in patterns with overlapping alternatives.

### **Use Case:**
- Matching patterns with **fixed structures**.
- Optimizing **complex regex** with multiple branches.

---

## **5. Performance Optimization Tips**

### **1. Use Atomic Groups (`?>`)** 
- Prevents unnecessary backtracking.
- Example:
    ```regex
    (?>\d{3})\d{2}
    ```

### **2. Use Possessive Quantifiers (`*+`, `++`, `?+`)**
- Similar to greedy quantifiers but **without backtracking**.
- Example:
    ```regex
    \d++\w+
    ```

### **3. Order Alternations Efficiently**
- Place the **most common patterns first** in alternations.
- Example:
    ```regex
    cat|caterpillar|catch
    ```
    - `"cat"` is checked first, improving performance.

### **4. Minimize Backtracking with Lazy Quantifiers**
- Use lazy quantifiers (`*?`, `+?`, `??`) when you expect the shortest match.
- Example:
    ```regex
    <.*?> 
    ```
    - Matches the **shortest** HTML tag.

### **5. Avoid Catastrophic Backtracking**
- Catastrophic backtracking occurs when a pattern has **multiple overlapping possibilities**.
- Example of problematic pattern:
    ```regex
    (a+)+
    ```
    - Matching `"aaaaaa"` causes excessive backtracking.
- **Solution:**
    - Use **atomic groups** or **possessive quantifiers** to prevent backtracking.

---

## **6. Practical Examples:**

1. **Match Nested Parentheses Recursively:**
    ```regex
    \((?:[^()]+|(?R))*\)
    ```
    - Matches:
      ```
      (abc)
      (a(bc)d)
      ((a)(b))
      (a(b(c)d)e)
      ```

2. **Match Balanced HTML Tags:**
    ```regex
    <(\w+)>.*?</\1>
    ```
    - Matches:
      ```
      <b>text</b>
      <div>content</div>
      ```

3. **Match Conditional Patterns:**
    ```regex
    (a)?b(?(1)c|d)
    ```
    - Matches:
      - `"abc"` (if `a` is present)
      - `"bd"` (if `a` is not present)

4. **Optimize Performance with Atomic Groups:**
    ```regex
    (?>\d{3})\d{2}
    ```
    - Matches **5 digits** without backtracking.

---

## **7. Best Practices:**
1. Use **recursive patterns** for **nested structures**.
2. Use **conditional expressions** for **context-sensitive** matching.
3. Use **atomic groups** and **possessive quantifiers** to **optimize performance**.
4. **Test complex patterns** thoroughly to avoid catastrophic backtracking.

---

## **8. Hands-on Practice:**
- **Beginner:** Match HTML tags with consistent opening and closing tags.
- **Intermediate:** Extract quoted strings while ensuring matching quotes.
- **Advanced:** Match nested parentheses with arbitrary depth.

---

## **9. Summary of Key Points:**
1. **Recursive Patterns** handle nested structures.
2. **Conditional Expressions** provide context-sensitive matching.
3. **Atomic Groups** improve performance by preventing backtracking.
4. **Optimization Tips** help in building efficient and maintainable regex patterns.

---

### **10. Practical Applications**

Regex is a powerful tool with numerous practical applications across different domains. This section covers real-world use cases, demonstrating how regex can be applied to solve complex problems in a simple and efficient way.

---

## **1. Input Validation**

Regex is commonly used for **input validation** to ensure that user inputs are in the expected format. Here are some of the most common scenarios:

### **1. Email Validation**

**Regex:**
```regex
^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$
```

### **Explanation:**
- `^`: Start of the string
- `[a-zA-Z0-9._%+-]+`: One or more letters, digits, dots, underscores, percent signs, pluses, or hyphens
- `@`: Literal `@` symbol
- `[a-zA-Z0-9.-]+`: Domain name (letters, digits, dots, hyphens)
- `\.`: Literal dot
- `[a-zA-Z]{2,}`: Top-level domain with at least two letters
- `$`: End of the string

### **Examples:**
- Matches: `"test.email@example.com"`, `"user.name+tag+sorting@example.co.uk"`
- Does not match: `"plainaddress"`, `"@missingusername.com"`

---

### **2. Phone Number Validation**

**Regex:**
```regex
^\+?[0-9]{1,3}?[-. ]?(\(?[0-9]{1,4}\)?)?[-. ]?[0-9]{1,4}[-. ]?[0-9]{1,4}[-. ]?[0-9]{1,9}$
```

### **Explanation:**
- Supports optional country code (`+` followed by 1 to 3 digits)
- Allows different separators (`-`, `.`, or space)
- Optional parentheses for area code
- Flexible grouping for local numbers

### **Examples:**
- Matches: `"+1-800-555-5555"`, `"123-456-7890"`, `"(123) 456-7890"`
- Does not match: `"123-45-678"`, `"phone123"`

---

### **3. Password Validation**

**Regex:**
```regex
^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$
```

### **Explanation:**
- `^`: Start of the string
- `(?=.*[a-z])`: At least one lowercase letter
- `(?=.*[A-Z])`: At least one uppercase letter
- `(?=.*\d)`: At least one digit
- `(?=.*[@$!%*?&])`: At least one special character
- `[A-Za-z\d@$!%*?&]{8,}`: At least 8 characters long
- `$`: End of the string

### **Examples:**
- Matches: `"Password1!"`, `"Str0ngP@ss"`
- Does not match: `"password"`, `"PASSWORD1"`, `"Passw1"`

---

## **2. Search and Replace Operations**

Regex is extensively used for **search and replace** operations in text editors, programming languages, and data transformation tools.

### **1. Replace All Tabs with Spaces**

**Regex:**
```regex
\t
```

**Replacement:**
```regex
    (four spaces)
```

### **Use Case:**
- Converting tabs to spaces for consistent code indentation.

---

### **2. Reformat Dates**

**Regex:**
```regex
(\d{2})/(\d{2})/(\d{4})
```

**Replacement:**
```regex
$3-$2-$1
```

### **Explanation:**
- Matches dates in `dd/mm/yyyy` format
- Captures day (`\1`), month (`\2`), and year (`\3`)
- Replaces with `yyyy-mm-dd`

### **Examples:**
- Input: `"Date: 25/12/2024"`
- Output: `"Date: 2024-12-25"`

---

### **3. Remove Extra Spaces**

**Regex:**
```regex
\s{2,}
```

**Replacement:**
```regex
 (single space)
```

### **Use Case:**
- Cleaning up multiple spaces between words.
- Example: `"This    is   a   test"` → `"This is a test"`

---

## **3. Data Extraction**

Regex is used to **extract specific pieces of data** from unstructured text, such as logs, files, or web pages.

### **1. Extract Email Addresses**

**Regex:**
```regex
[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}
```

### **Use Case:**
- Extracting all email addresses from a text file or web page.

### **Example:**
- Text: `"Contact us at support@example.com or sales@example.org"`
- Matches: `"support@example.com"`, `"sales@example.org"`

---

### **2. Extract URLs**

**Regex:**
```regex
https?:\/\/[^\s/$.?#].[^\s]*
```

### **Use Case:**
- Extracting all URLs from a web page or log file.

### **Example:**
- Text: `"Visit https://example.com or http://example.org"`
- Matches: `"https://example.com"`, `"http://example.org"`

---

## **4. Log Analysis**

Regex is highly effective for **parsing and analyzing log files**.

### **1. Extract Timestamps**

**Regex:**
```regex
\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}
```

### **Use Case:**
- Extracting timestamps from log entries in `YYYY-MM-DD HH:MM:SS` format.

### **Example:**
- Log: `"2024-12-25 14:30:00 ERROR Connection failed"`
- Matches: `"2024-12-25 14:30:00"`

---

### **2. Extract IP Addresses**

**Regex:**
```regex
\b(?:\d{1,3}\.){3}\d{1,3}\b
```

### **Use Case:**
- Extracting IP addresses from server logs.

### **Example:**
- Log: `"Client IP: 192.168.0.1"`
- Matches: `"192.168.0.1"`

---

## **5. Web Scraping**

Regex is often used in **web scraping** to extract data from HTML pages.

### **1. Extracting Title Tag Content**

**Regex:**
```regex
<title>(.*?)<\/title>
```

### **Use Case:**
- Extracting the content of the `<title>` tag from an HTML page.

### **Example:**
- HTML: `<title>Example Page</title>`
- Matches: `"Example Page"`

---

### **2. Extracting Meta Description**

**Regex:**
```regex
<meta name="description" content="(.*?)">
```

### **Use Case:**
- Extracting the meta description from an HTML page.

### **Example:**
- HTML: `<meta name="description" content="This is an example page.">`
- Matches: `"This is an example page."`

---

## **6. Best Practices:**
1. Use **input validation** to prevent security issues like XSS and SQL injection.
2. Apply **search and replace** for data cleaning and transformation.
3. Use **data extraction** to parse unstructured data efficiently.
4. **Log analysis** with regex helps in monitoring and troubleshooting.
5. For **web scraping**, always be mindful of website terms of service.

---

## **7. Hands-on Practice:**
- **Beginner:** Extract all email addresses from a document.
- **Intermediate:** Reformat dates from `dd/mm/yyyy` to `yyyy-mm-dd`.
- **Advanced:** Extract and organize log entries by severity levels (e.g., ERROR, WARNING, INFO).

---

## **8. Summary of Key Points:**
1. **Input Validation** ensures data consistency and security.
2. **Search and Replace** operations are useful for text manipulation.
3. **Data Extraction** helps retrieve relevant information from unstructured text.
4. **Log Analysis** enables efficient monitoring and troubleshooting.
5. **Web Scraping** allows extraction of data from HTML pages.

---

### **11. Regex in Different Programming Languages**

Regex is widely supported across various programming languages, but each language has **slight variations** in syntax and implementation. Understanding these differences helps you write effective and compatible regex patterns.

---

## **1. Overview of Regex Implementations**

- **JavaScript:** Uses ECMAScript (ES) regex syntax.
- **Java:** Uses the `java.util.regex` package.
- **Python:** Uses the `re` module.
- **PHP:** Uses the `preg_*` functions.
- **C# (.NET):** Uses the `System.Text.RegularExpressions` namespace.
- **Ruby:** Uses built-in regex support with `/pattern/` syntax.

---

## **2. JavaScript**

### **Syntax:**
```js
let regex = /pattern/flags;
```
- Use `/pattern/` for literals.
- Flags are placed after the closing slash (e.g., `/pattern/g`).

### **Common Methods:**
- `.test()`: Checks if the pattern is present.
- `.match()`: Returns all matches.
- `.replace()`: Replaces matched substrings.
- `.split()`: Splits the string using the pattern.

### **Example 1: Test for Match**
```js
let pattern = /cat/i;
console.log(pattern.test("Cat is here")); // true
```

### **Example 2: Find All Matches**
```js
let text = "cat, Cat, CAt, CAT";
let matches = text.match(/cat/gi);
console.log(matches); // ["cat", "Cat", "CAt", "CAT"]
```

### **Example 3: Replace Text**
```js
let text = "I have a cat.";
let replaced = text.replace(/cat/, "dog");
console.log(replaced); // "I have a dog."
```

---

## **3. Java**

### **Syntax:**
```java
Pattern pattern = Pattern.compile("pattern");
Matcher matcher = pattern.matcher("text");
```
- `Pattern` is used to compile the regex.
- `Matcher` is used to find matches.

### **Common Methods:**
- `.matches()`: Checks for a full match.
- `.find()`: Finds the next occurrence.
- `.group()`: Returns the matched text.
- `.replaceAll()`: Replaces all occurrences.

### **Example 1: Check Full Match**
```java
String text = "hello";
boolean isMatch = text.matches("hello");
System.out.println(isMatch); // true
```

### **Example 2: Find and Extract Matches**
```java
Pattern pattern = Pattern.compile("\\d+");
Matcher matcher = pattern.matcher("Order 123, Item 456");
while (matcher.find()) {
    System.out.println(matcher.group()); // 123, 456
}
```

### **Example 3: Replace Text**
```java
String text = "I have a cat.";
String replaced = text.replaceAll("cat", "dog");
System.out.println(replaced); // "I have a dog."
```

---

## **4. Python**

### **Syntax:**
```python
import re
pattern = re.compile(r'pattern')
```
- Use `r''` (raw string) to avoid escaping backslashes.
- `re` module provides regex support.

### **Common Methods:**
- `.match()`: Checks for a match at the start of the string.
- `.search()`: Finds the first occurrence.
- `.findall()`: Returns all matches.
- `.sub()`: Replaces matched substrings.

### **Example 1: Check for Match**
```python
import re
text = "hello"
match = re.match(r"hello", text)
print(bool(match)) # True
```

### **Example 2: Find All Matches**
```python
text = "Order 123, Item 456"
matches = re.findall(r"\d+", text)
print(matches) # ['123', '456']
```

### **Example 3: Replace Text**
```python
text = "I have a cat."
replaced = re.sub(r"cat", "dog", text)
print(replaced) # "I have a dog."
```

---

## **5. PHP**

### **Syntax:**
```php
$pattern = "/pattern/";
```
- Delimiters are mandatory (`/` by default).
- Use `preg_*` functions for regex operations.

### **Common Functions:**
- `preg_match()`: Checks for a match.
- `preg_match_all()`: Finds all matches.
- `preg_replace()`: Replaces matched substrings.
- `preg_split()`: Splits the string using the pattern.

### **Example 1: Check for Match**
```php
$text = "hello";
if (preg_match("/hello/", $text)) {
    echo "Match found!";
}
```

### **Example 2: Find All Matches**
```php
$text = "Order 123, Item 456";
preg_match_all("/\d+/", $text, $matches);
print_r($matches[0]); // Array ( [0] => 123 [1] => 456 )
```

### **Example 3: Replace Text**
```php
$text = "I have a cat.";
$replaced = preg_replace("/cat/", "dog", $text);
echo $replaced; // "I have a dog."
```

---

## **6. C# (.NET)**

### **Syntax:**
```csharp
Regex regex = new Regex("pattern");
```
- Use `System.Text.RegularExpressions`.

### **Common Methods:**
- `.IsMatch()`: Checks for a match.
- `.Matches()`: Returns all matches.
- `.Replace()`: Replaces matched substrings.
- `.Split()`: Splits the string using the pattern.

### **Example 1: Check for Match**
```csharp
string text = "hello";
bool isMatch = Regex.IsMatch(text, "hello");
Console.WriteLine(isMatch); // True
```

### **Example 2: Find All Matches**
```csharp
string text = "Order 123, Item 456";
MatchCollection matches = Regex.Matches(text, @"\d+");
foreach (Match match in matches) {
    Console.WriteLine(match.Value); // 123, 456
}
```

### **Example 3: Replace Text**
```csharp
string text = "I have a cat.";
string replaced = Regex.Replace(text, "cat", "dog");
Console.WriteLine(replaced); // "I have a dog."
```

---

## **7. Differences and Compatibility**

### **1. Delimiters:**
- JavaScript, PHP, Perl use `/pattern/`.
- Python uses `r''` for raw strings.
- Java and C# use string literals.

### **2. Escape Characters:**
- `\` is used as an escape character.
- Use double backslashes (`\\`) in Java, C#, and Python.

### **3. Lookbehind Support:**
- Supported in Python, Java, and C#.
- **Not supported** in JavaScript (until ES2018).

### **4. Unicode Support:**
- JavaScript: Use `/pattern/u`
- Python: Default support with Unicode strings.
- Java: Requires `Pattern.UNICODE_CHARACTER_CLASS`.

---

## **8. Best Practices:**
1. Use language-specific **modifiers** for case insensitivity and multiline mode.
2. Use **raw strings** in Python to avoid escaping backslashes.
3. Test regex with **different inputs** to ensure compatibility.
4. Always **benchmark** performance, especially for complex patterns.

---

## **9. Hands-on Practice:**
- **Beginner:** Match all numbers in a text using different languages.
- **Intermediate:** Extract email addresses using language-specific functions.
- **Advanced:** Reformat dates in multiple languages.

---

## **10. Summary of Key Points:**
1. **JavaScript**: Uses `/pattern/flags` syntax.
2. **Java**: Uses `Pattern` and `Matcher` classes.
3. **Python**: Uses the `re` module with raw strings.
4. **PHP**: Uses `preg_*` functions.
5. **C# (.NET)**: Uses `Regex` class with full-featured support.
6. **Ruby**: Uses built-in regex with `/pattern/` syntax.
7. **Compatibility** and **syntax differences** vary by language.

---

### **12. Testing and Debugging Regex**

Writing complex regex patterns can be challenging, and testing them thoroughly is essential to ensure they work as intended. This section covers tools and techniques for **testing, debugging, and optimizing** regex patterns.

---

## **1. Why Test and Debug Regex?**
- **Avoid Unexpected Matches:** Ensure the pattern matches only the intended text.
- **Performance Optimization:** Prevent excessive backtracking and improve efficiency.
- **Maintainability:** Make complex regex more readable and maintainable.
- **Cross-Platform Compatibility:** Test compatibility across different programming languages.

---

## **2. Online Regex Testers and Tools**

Online regex testers provide a convenient way to **write, test, and debug** regex patterns. They highlight matches, display captured groups, and often provide explanations for complex patterns.

### **Popular Online Tools:**
1. [**Regex101**](https://regex101.com)
   - **Supports:** PCRE (PHP), ECMAScript (JavaScript), Python, and Golang.
   - **Features:**
     - Real-time highlighting of matches.
     - Detailed explanations for each part of the regex.
     - Syntax highlighting and error detection.
     - Capturing group details.

2. [**Regexr**](https://regexr.com)
   - **Supports:** JavaScript (ECMAScript) syntax.
   - **Features:**
     - Real-time regex validation and matching.
     - Explanation of regex syntax.
     - Community examples and pattern sharing.

3. [**RegExPlanet**](https://www.regexplanet.com)
   - **Supports:** Java, JavaScript, Perl, PHP, Python, Ruby, and more.
   - **Features:**
     - Cross-language compatibility testing.
     - Detailed match information and explanations.

4. [**RegExPal**](https://www.regexpal.com)
   - **Supports:** JavaScript syntax.
   - **Features:**
     - Interactive regex testing.
     - Instant highlighting of matches.

---

## **3. Debugging Complex Patterns**

### **1. Break Down Complex Patterns**
- **Deconstruct** complex regex into smaller, manageable parts.
- Test each part **individually** to isolate issues.
- Gradually combine parts, testing at each step.

### **Example:**
- Complex Pattern:
    ```regex
    ^(\d{3})-(\d{2})-(\d{4})$
    ```
- Break Down:
    - `^\d{3}` → Match three digits at the start.
    - `-\d{2}` → Match a hyphen followed by two digits.
    - `-\d{4}$` → Match another hyphen followed by four digits at the end.

---

### **2. Use Comments and Whitespace (Extended Mode)**

Extended mode allows you to add **comments** and **whitespace** to improve readability.

### **Syntax (Python Example):**
```python
pattern = r"""
    ^              # Start of the line
    (\d{3})        # Area code (3 digits)
    -              # Hyphen separator
    (\d{2})        # Prefix (2 digits)
    -              # Hyphen separator
    (\d{4})        # Line number (4 digits)
    $              # End of the line
"""
re.compile(pattern, re.VERBOSE)
```

### **Use Case:**
- Improve readability for complex patterns.
- Make maintenance easier by adding comments.

---

### **3. Test with Diverse Input Data**
- Test with **valid**, **invalid**, **edge cases**, and **boundary values**.
- Include:
  - **Minimum and maximum lengths**
  - **Special characters**
  - **Empty strings**
  - **Unicode characters** (if applicable)

### **Example:**
- Testing Email Pattern:
    ```regex
    ^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$
    ```
- Test Cases:
    - Valid: `"user@example.com"`, `"name.surname@example.co.uk"`
    - Invalid: `"userexample.com"`, `"@example.com"`, `"user@.com"`

---

## **4. Common Pitfalls and Solutions**

### **1. Catastrophic Backtracking**
- Occurs when a regex has **multiple overlapping possibilities**.
- Causes **excessive backtracking** and can lead to performance issues.

### **Example of Problematic Pattern:**
```regex
(a+)+
```
- Matches one or more `a`s, but backtracks excessively with long strings like `"aaaaaa"`.

### **Solution:**
- Use **atomic groups** or **possessive quantifiers** to **prevent backtracking**.

### **Fixed Pattern:**
```regex
(?>a+)+
```
- **Atomic Group** prevents backtracking.

or

```regex
a+++
```
- **Possessive Quantifier** also prevents backtracking.

---

### **2. Greedy vs. Lazy Quantifiers**
- Greedy quantifiers (`*`, `+`, `{n,m}`) **match as many characters as possible**.
- Lazy quantifiers (`*?`, `+?`, `{n,m}?`) **match as few characters as possible**.

### **Example of Greedy Quantifier:**
```regex
<.*>
```
- Matches the **longest** possible string between the first `<` and the last `>`.

### **Solution:**
- Use **lazy quantifiers** for shortest matches.

### **Fixed Pattern:**
```regex
<.*?>
```
- Matches the **shortest** string between each pair of `<` and `>`.

---

### **3. Anchors and Boundaries**
- Ensure that `^` and `$` are used correctly for **start and end** of lines.
- Use `\b` for **word boundaries** and `\B` for **non-word boundaries**.

### **Example:**
```regex
\bcat\b
```
- Matches `"cat"` as a **whole word** but not `"catapult"` or `"bobcat"`.

---

## **5. Best Practices:**
1. **Break down complex patterns** into smaller parts.
2. Use **comments and extended mode** for readability.
3. Test with **diverse input data** to cover all edge cases.
4. Watch out for **catastrophic backtracking** and optimize accordingly.
5. Use **lazy quantifiers** when shortest matches are required.
6. Utilize **online testers** for quick feedback and debugging.

---

## **6. Hands-on Practice:**
- **Beginner:** Test basic patterns using Regex101.
- **Intermediate:** Debug complex patterns using comments and extended mode.
- **Advanced:** Optimize regex with atomic groups and possessive quantifiers.

---

## **7. Summary of Key Points:**
1. **Online Regex Testers** provide real-time feedback and explanations.
2. **Break down complex patterns** to isolate issues.
3. **Extended mode** improves readability with comments and whitespace.
4. **Test with diverse input data** for robust regex validation.
5. **Optimize performance** by avoiding catastrophic backtracking.

---

### **13. Exercises and Challenges**

Practicing regex through exercises and challenges helps reinforce learning and builds confidence in writing complex patterns. This section includes **beginner, intermediate, and advanced level challenges**, along with real-world scenarios to test your skills.

---

## **1. Beginner Level Exercises**

These exercises cover the basics of regex, including character classes, literals, and simple quantifiers.

---

### **1. Match All Words Starting with 'a'**

**Description:**
- Match all words that start with the letter `'a'`.

**Regex:**
```regex
\b[aA]\w*
```

**Example:**
- **Text:** `"apple, ant, Banana, apricot, Avocado"`
- **Matches:** `"apple"`, `"ant"`, `"apricot"`, `"Avocado"`
- **Does not match:** `"Banana"`

---

### **2. Extract All Digits from a Text**

**Description:**
- Extract all sequences of digits.

**Regex:**
```regex
\d+
```

**Example:**
- **Text:** `"I have 2 apples and 10 oranges."`
- **Matches:** `"2"`, `"10"`

---

### **3. Match Valid Email Addresses**

**Description:**
- Match basic email addresses with alphanumeric characters, dots, underscores, and hyphens.

**Regex:**
```regex
[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}
```

**Example:**
- **Text:** `"Contact us at support@example.com or sales@example.org"`
- **Matches:** `"support@example.com"`, `"sales@example.org"`

---

### **4. Validate Simple Dates (dd/mm/yyyy)**

**Description:**
- Match dates in the format `dd/mm/yyyy`.

**Regex:**
```regex
\b\d{2}/\d{2}/\d{4}\b
```

**Example:**
- **Text:** `"Today's date is 25/12/2024"`
- **Matches:** `"25/12/2024"`
- **Does not match:** `"25-12-2024"`, `"2024/12/25"`

---

### **5. Match Words Ending with 'ing'**

**Description:**
- Match all words ending with `'ing'`.

**Regex:**
```regex
\w+ing\b
```

**Example:**
- **Text:** `"I am running, swimming, and jumping."`
- **Matches:** `"running"`, `"swimming"`, `"jumping"`

---

## **2. Intermediate Level Challenges**

These challenges involve grouping, capturing, alternation, and more advanced quantifiers.

---

### **1. Extract All URLs**

**Description:**
- Extract all URLs from a text, supporting both `http` and `https`.

**Regex:**
```regex
https?:\/\/[^\s/$.?#].[^\s]*
```

**Example:**
- **Text:** `"Visit https://example.com or http://example.org"`
- **Matches:** `"https://example.com"`, `"http://example.org"`

---

### **2. Extract Text within Parentheses**

**Description:**
- Extract text that is enclosed within parentheses.

**Regex:**
```regex
\((.*?)\)
```

**Example:**
- **Text:** `"This is a sentence (with some text)"`
- **Matches:** `"with some text"`

---

### **3. Validate Hexadecimal Colors**

**Description:**
- Match hexadecimal color codes (e.g., `#FF5733`, `#fff`).

**Regex:**
```regex
#([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})
```

**Example:**
- **Text:** `"Colors: #FF5733, #fff, #123ABC"`
- **Matches:** `"#FF5733"`, `"#fff"`, `"#123ABC"`

---

### **4. Extract Hashtags from Social Media Text**

**Description:**
- Extract all hashtags from a social media post.

**Regex:**
```regex
#\w+
```

**Example:**
- **Text:** `"Loving the #sunset and #nature!"`
- **Matches:** `"#sunset"`, `"#nature"`

---

### **5. Extract Quoted Text**

**Description:**
- Extract text enclosed within double quotes.

**Regex:**
```regex
"([^"]*)"
```

**Example:**
- **Text:** `"She said, "Hello, world!" and then left."`
- **Matches:** `"Hello, world!"`

---

## **3. Advanced Level Challenges**

These challenges involve lookaheads, lookbehinds, recursion, and conditional expressions.

---

### **1. Validate Complex Passwords**

**Description:**
- Validate passwords that are at least 8 characters long, containing:
  - At least one uppercase letter
  - At least one lowercase letter
  - At least one digit
  - At least one special character (`@$!%*?&`)

**Regex:**
```regex
^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$
```

**Example:**
- Matches: `"Password1!"`, `"Str0ngP@ss"`
- Does not match: `"password"`, `"PASSWORD1"`, `"Passw1"`

---

### **2. Match Balanced Parentheses Recursively**

**Description:**
- Match balanced parentheses, even if nested.

**Regex (PCRE or Perl):**
```regex
\((?:[^()]+|(?R))*\)
```

**Example:**
- **Text:** `"((a)(b))"`, `"(a(b(c)d)e)"`
- **Matches:** 
  - `"((a)(b))"`
  - `"(a(b(c)d)e)"`

---

### **3. Validate Consistent HTML Tags**

**Description:**
- Match HTML tags ensuring the opening and closing tags are the same.

**Regex:**
```regex
<(\w+)>.*?</\1>
```

**Example:**
- **Text:** `"<b>text</b>"`, `"<div>content</div>"`
- **Matches:** 
  - `"<b>text</b>"`
  - `"<div>content</div>"`
- **Does not match:** `"<b>text</i>"`

---

### **4. Extract Nested JSON-like Objects**

**Description:**
- Extract nested JSON-like objects using recursion.

**Regex (PCRE or Perl):**
```regex
\{(?:[^{}]|(?R))*\}
```

**Example:**
- **Text:** `{"key": "value", "nested": {"innerKey": "innerValue"}}`
- **Matches:** The entire JSON object, including nested structures.

---

### **5. Validate Currency Format**

**Description:**
- Match currency formats like:
  - `"$100.00"`
  - `"$1,000.99"`
  - `"$1,000,000.00"`

**Regex:**
```regex
^\$\d{1,3}(,\d{3})*(\.\d{2})?$
```

**Example:**
- **Text:** `"$100.00"`, `"$1,000.99"`, `"$1,000,000.00"`
- **Matches:** All of the above
- **Does not match:** `"100.00"`, `"$100"` (missing decimal places)

---

## **4. Real-World Scenarios**

1. **Log Analysis:**
    - Extract ERROR and WARNING messages from log files.
    - Example:
    ```regex
    (ERROR|WARNING): .*
    ```

2. **Web Scraping:**
    - Extract meta tags from HTML pages.
    - Example:
    ```regex
    <meta name="(.*?)" content="(.*?)">
    ```

3. **Data Cleaning:**
    - Remove all HTML tags from a document.
    - Example:
    ```regex
    <[^>]*>
    ```

---

## **5. Best Practices:**
1. Start with **basic patterns** and **gradually build complexity**.
2. Test patterns with **valid, invalid, and edge cases**.
3. Use **online regex testers** to validate patterns.
4. **Document complex patterns** with comments for maintainability.
5. Practice **real-world scenarios** to enhance problem-solving skills.

---

### **14. Resources and References**

To master regex, it's essential to have access to high-quality learning resources, tools, and references. This section provides a comprehensive list of books, online tutorials, cheat sheets, and tools to reinforce your regex skills.

---

## **1. Recommended Books**

### **1. Mastering Regular Expressions by Jeffrey E. F. Friedl**
- **Description:**
  - Known as the "Regex Bible," this book provides in-depth explanations of regex concepts, patterns, and usage.
  - Covers advanced topics like performance optimization and complex pattern matching.
- **Best For:**
  - Intermediate to advanced users who want to understand the internals of regex engines.

---

### **2. Regular Expressions Cookbook by Jan Goyvaerts and Steven Levithan**
- **Description:**
  - Offers practical solutions to common regex problems, with detailed examples and explanations.
  - Covers regex use cases across multiple programming languages.
- **Best For:**
  - Beginners to advanced users who want a hands-on approach with practical examples.

---

### **3. Learning Regular Expressions by Ben Forta**
- **Description:**
  - A beginner-friendly introduction to regex with a clear and concise writing style.
  - Covers basic to intermediate concepts with practical exercises.
- **Best For:**
  - Beginners looking to get started with regex in a step-by-step manner.

---

### **4. JavaScript Regular Expressions by Loiane Groner**
- **Description:**
  - Focuses on using regex in JavaScript, with practical examples and tips.
  - Covers regex integration with modern JavaScript frameworks.
- **Best For:**
  - Web developers working with JavaScript who want to learn regex in a browser environment.

---

### **5. Regular Expressions: Pocket Primer by Oswald Campesato**
- **Description:**
  - A quick reference guide for regex syntax and patterns.
  - Includes concise explanations and examples for multiple programming languages.
- **Best For:**
  - Intermediate users who need a quick reference and refresher.

---

## **2. Online Tutorials and Courses**

### **1. RegexOne**
- **URL:** [https://regexone.com](https://regexone.com)
- **Description:**
  - An interactive tutorial for learning regex step by step.
  - Covers the basics with practical examples and exercises.
- **Best For:**
  - Absolute beginners who want to learn regex interactively.

---

### **2. FreeCodeCamp Regex Guide**
- **URL:** [https://www.freecodecamp.org/learn](https://www.freecodecamp.org/learn)
- **Description:**
  - FreeCodeCamp offers a regex guide as part of its JavaScript certification course.
  - Covers regex syntax, patterns, and use cases with interactive challenges.
- **Best For:**
  - Beginners to intermediate users who want to practice regex with JavaScript.

---

### **3. Regex101 Tutorial and Explanations**
- **URL:** [https://regex101.com](https://regex101.com)
- **Description:**
  - Offers detailed explanations and breakdowns of regex patterns.
  - Includes syntax highlighting and debugging tools.
- **Best For:**
  - Intermediate users who want to understand how regex patterns work internally.

---

### **4. Educative.io - Regular Expressions for Programmers**
- **URL:** [https://www.educative.io](https://www.educative.io)
- **Description:**
  - A detailed interactive course with quizzes and coding challenges.
  - Covers regex across multiple programming languages.
- **Best For:**
  - Developers who want a comprehensive course with hands-on coding exercises.

---

### **5. Udemy and Coursera Courses**
- **Description:**
  - Both platforms offer beginner to advanced regex courses.
  - Courses are available for different programming languages, including JavaScript, Python, and Java.
- **Best For:**
  - Learners who prefer video tutorials and guided explanations.

---

## **3. Cheat Sheets and Guides**

### **1. Regular Expressions Cheat Sheet by Dave Child**
- **URL:** [https://www.addedbytes.com/cheat-sheets/regular-expressions-cheat-sheet/](https://www.addedbytes.com/cheat-sheets/regular-expressions-cheat-sheet/)
- **Description:**
  - A comprehensive cheat sheet covering regex syntax, patterns, and metacharacters.
  - Suitable for quick reference and learning.

---

### **2. Regex Cheat Sheet on Cheatography**
- **URL:** [https://www.cheatography.com](https://www.cheatography.com)
- **Description:**
  - Includes detailed explanations of character classes, quantifiers, groups, and anchors.
  - Provides examples for different programming languages.

---

### **3. JavaScript Regex Cheatsheet by JavaScript.Info**
- **URL:** [https://javascript.info/regexp](https://javascript.info/regexp)
- **Description:**
  - Focuses on regex patterns in JavaScript with practical examples.
  - Covers modern JavaScript features like Unicode support and named groups.

---

## **4. Online Testing and Debugging Tools**

### **1. Regex101**
- **URL:** [https://regex101.com](https://regex101.com)
- **Description:**
  - Real-time regex testing and debugging.
  - Detailed explanations for each part of the pattern.
  - Supports multiple flavors: PCRE, ECMAScript, Python, and Golang.

---

### **2. Regexr**
- **URL:** [https://regexr.com](https://regexr.com)
- **Description:**
  - Interactive regex testing tool with syntax highlighting.
  - Built-in regex library with community examples and sharing options.

---

### **3. RegExPlanet**
- **URL:** [https://www.regexplanet.com](https://www.regexplanet.com)
- **Description:**
  - Cross-language compatibility testing for Java, JavaScript, Python, and more.
  - Advanced debugging and match information.

---

### **4. RegExPal**
- **URL:** [https://www.regexpal.com](https://www.regexpal.com)
- **Description:**
  - Simple and interactive regex testing with real-time results.
  - Useful for quick pattern testing.

---

### **5. Online String Extractor**
- **URL:** [https://extractor.regexmagic.com](https://extractor.regexmagic.com)
- **Description:**
  - Extracts specific parts of text using regex patterns.
  - Useful for web scraping and data extraction tasks.

---

## **5. Community and Support**

### **1. Stack Overflow**
- **URL:** [https://stackoverflow.com](https://stackoverflow.com)
- **Description:**
  - Active community of developers for regex-related questions and answers.
  - Ideal for troubleshooting and learning best practices.

---

### **2. Reddit - r/regex**
- **URL:** [https://www.reddit.com/r/regex](https://www.reddit.com/r/regex)
- **Description:**
  - Community discussions, questions, and regex challenges.
  - Helpful for learning new patterns and real-world use cases.

---

### **3. GitHub Repositories**
- **Description:**
  - Open-source repositories with regex examples and tools.
  - Great for exploring real-world applications and contributing to regex libraries.

---

## **6. Best Practices:**
1. Use **cheat sheets** for quick reference.
2. Practice with **interactive tutorials** to reinforce learning.
3. Utilize **online testers** for real-time feedback and debugging.
4. Engage in **community forums** for support and knowledge sharing.
5. **Read books** for in-depth understanding and advanced techniques.

---

## **7. Hands-on Practice:**
- **Beginner:** Practice basic patterns on Regex101.
- **Intermediate:** Solve challenges on RegexOne.
- **Advanced:** Contribute to open-source projects on GitHub.

---

## **8. Summary of Key Points:**
1. **Books** provide in-depth knowledge and practical solutions.
2. **Online tutorials** offer interactive learning experiences.
3. **Cheat sheets** are useful for quick reference.
4. **Online testers** aid in real-time debugging and validation.
5. **Communities** like Stack Overflow and Reddit provide support and challenges.

---
### **15. Summary and Conclusion**

This guide has taken you on a comprehensive journey through **Regular Expressions (Regex)**, from the very basics to advanced concepts and practical applications. Here, we summarize the key takeaways, best practices, and when not to use regex, ensuring a holistic understanding of this powerful tool.

---

## **1. Key Takeaways**

### **1. Basics of Regex**
- **What is Regex?**: A pattern-matching language used for text processing.
- **Basic Concepts**:
  - **Literals**: Match exactly as they are.
  - **Metacharacters**: Special characters with symbolic meanings (`.`, `*`, `+`, `?`, `^`, `$`, `()`, `[]`, `{}`, `|`, `\`).
  - **Escaping**: Use `\` to escape metacharacters (e.g., `\.` to match a dot).

---

### **2. Character Classes and Quantifiers**
- **Character Classes**:
  - Predefined: `\d`, `\w`, `\s`, `\D`, `\W`, `\S`
  - Custom: `[abc]`, `[^abc]`, `[a-zA-Z0-9_]`
- **Quantifiers**:
  - Greedy: `*`, `+`, `?`, `{n}`, `{n,}`, `{n,m}`
  - Lazy: `*?`, `+?`, `??`, `{n,}?`, `{n,m}?`
  - Possessive (in some engines): `*+`, `++`, `?+`

---

### **3. Anchors and Boundaries**
- **Anchors**:
  - `^`: Start of line or string.
  - `$`: End of line or string.
- **Word Boundaries**:
  - `\b`: Word boundary.
  - `\B`: Non-word boundary.

---

### **4. Grouping and Capturing**
- **Grouping**:
  - Capturing Groups: `()`
  - Non-Capturing Groups: `(?: ... )`
  - Named Groups: `(?<name> ... )`
- **Backreferences**:
  - Numbered: `\1`, `\2`, ...
  - Named: `\k<name>`

---

### **5. Lookahead and Lookbehind**
- **Lookaheads**:
  - Positive Lookahead: `(?= ... )`
  - Negative Lookahead: `(?! ... )`
- **Lookbehinds**:
  - Positive Lookbehind: `(?<= ... )`
  - Negative Lookbehind: `(?<! ... )`

---

### **6. Flags and Modifiers**
- **Common Flags**:
  - `i`: Case Insensitive
  - `g`: Global Search (all matches)
  - `m`: Multiline mode
  - `s`: Dotall mode (`.` matches newline)
  - `u`: Unicode support
  - `y`: Sticky (exact position match)

---

### **7. Advanced Patterns**
- **Recursive Patterns**: Handle nested structures with `(?R)`.
- **Conditional Expressions**: ` (?(condition)true|false)`.
- **Atomic Groups**: `(?> ... )` to prevent backtracking.
- **Performance Optimization**:
  - Avoid catastrophic backtracking.
  - Use atomic groups and possessive quantifiers for efficiency.

---

### **8. Practical Applications**
- **Input Validation**: Emails, phone numbers, passwords.
- **Search and Replace**: Text manipulation and reformatting.
- **Data Extraction**: Extracting relevant information from unstructured text.
- **Log Analysis**: Monitoring and troubleshooting with pattern matching.
- **Web Scraping**: Extracting data from HTML and web content.

---

### **9. Regex in Different Programming Languages**
- Regex is supported across **JavaScript, Java, Python, PHP, C#, Ruby**, and many more.
- Syntax differences:
  - **Delimiters**: `/pattern/` in JavaScript, `r''` in Python.
  - **Escape Characters**: Double backslashes (`\\`) in Java, C#, and Python.
  - **Lookbehind Support**: Not supported in JavaScript (pre-ES2018).

---

### **10. Testing and Debugging**
- **Online Tools**:
  - **Regex101**: Real-time testing and detailed explanations.
  - **Regexr**: Interactive testing and community examples.
  - **RegExPlanet**: Cross-language compatibility testing.
- **Debugging Techniques**:
  - Break down complex patterns.
  - Use extended mode (`re.VERBOSE`) for comments.
  - Test with diverse inputs, including edge cases.

---

### **11. Exercises and Challenges**
- **Beginner Level**: Simple patterns and character classes.
- **Intermediate Level**: Grouping, capturing, and alternations.
- **Advanced Level**: Lookaheads, lookbehinds, recursion, and conditionals.
- **Real-World Scenarios**: Log analysis, web scraping, data cleaning.

---

### **12. Resources and References**
- **Books**:
  - *Mastering Regular Expressions* by Jeffrey E. F. Friedl
  - *Regular Expressions Cookbook* by Jan Goyvaerts and Steven Levithan
- **Online Tutorials and Courses**:
  - [RegexOne](https://regexone.com)
  - [FreeCodeCamp](https://www.freecodecamp.org)
- **Cheat Sheets and Guides**:
  - [Added Bytes Cheat Sheet](https://www.addedbytes.com/cheat-sheets/regular-expressions-cheat-sheet/)
- **Community and Support**:
  - [Stack Overflow](https://stackoverflow.com)
  - [Reddit - r/regex](https://www.reddit.com/r/regex)

---

## **2. Best Practices**

1. **Start Simple, Then Build**:
   - Start with basic patterns and add complexity gradually.
   - Break down complex patterns into smaller, manageable parts.

2. **Use Descriptive Names**:
   - Use named capturing groups for better readability and maintainability.

3. **Avoid Catastrophic Backtracking**:
   - Use atomic groups and possessive quantifiers to prevent performance issues.

4. **Test with Diverse Inputs**:
   - Include valid, invalid, edge cases, and boundary values.

5. **Document Complex Patterns**:
   - Use comments (extended mode) to explain complex regex for future maintainability.

---

## **3. When Not to Use Regex**

While regex is powerful, there are scenarios where it's **not the best choice**:

1. **Complex Parsing**:
   - Avoid regex for parsing complex nested structures like XML or JSON. Use dedicated parsers instead.

2. **Performance-Critical Applications**:
   - Regex can be slow with complex patterns or large inputs. Optimize or consider alternative solutions.

3. **Readability and Maintainability**:
   - Extremely complex patterns can be hard to read and maintain. Prioritize readability and use comments.

4. **Security Concerns**:
   - Be cautious of regex injection attacks, especially when accepting patterns as user input.

---

## **4. Final Thoughts**

- Regex is a **powerful tool** for text processing, but it requires practice and experience to master.
- Always **test thoroughly** and **optimize** for performance and maintainability.
- **Keep learning** and practicing with real-world scenarios to enhance your problem-solving skills.

---

## **5. Further Learning and Exploration**
- Explore **advanced topics** like recursion, conditional logic, and performance optimization.
- Experiment with **different programming languages** to understand syntax differences.
- Contribute to **open-source projects** or participate in regex challenges on platforms like Stack Overflow and Reddit.

---

Congratulations on completing the **Regex Mastery Guide**! You're now equipped with the knowledge to tackle even the most challenging regex patterns. 
