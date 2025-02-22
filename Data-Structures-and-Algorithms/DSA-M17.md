## 🚀 **Module 12: Mastering Advanced Data Structures**  

Advanced data structures provide efficient ways to store, retrieve, and manipulate data, optimizing time and space complexity. They are essential for competitive programming, complex algorithms, and real-world applications like search engines, databases, and compilers.

---

## **🔥 12.1 Why Learn Advanced Data Structures?**  
- **Optimize Time Complexity:** Faster search, insert, and delete operations.  
- **Efficient Memory Usage:** Compact storage for large datasets.  
- **Enable Complex Algorithms:** Efficient implementation of graph algorithms, range queries, and text processing.  
- **Competitive Programming:** Solve complex problems efficiently.  

---

## **🔥 12.2 Overview of Advanced Data Structures**  
1. **Trie (Prefix Tree)** — Efficient retrieval of strings or prefixes.  
2. **Segment Tree** — Efficient range queries and updates.  
3. **Fenwick Tree (Binary Indexed Tree)** — Efficient cumulative frequency queries.  
4. **AVL Tree and Red-Black Tree** — Self-balancing binary search trees.  
5. **Disjoint Set (Union-Find)** — Efficient union and find operations for disjoint sets.  

---

## **🔥 12.3 Trie (Prefix Tree)**  
- **Definition:** A tree-like data structure used to store strings by breaking them into individual characters.  
- **Applications:**  
  - Autocomplete and spell check.  
  - Word search in a dictionary.  
  - Longest prefix matching (e.g., IP routing).  
- **Time Complexity:**  
  - **Insert:** `O(L)` — L is the length of the word.  
  - **Search:** `O(L)`  
  - **Space Complexity:** `O(N * L)` — N is the number of words.  

---

### 📘 **Structure of Trie Node**  
```java
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    boolean isEndOfWord;

    TrieNode() {
        isEndOfWord = false;
        for (int i = 0; i < 26; i++) {
            children[i] = null;
        }
    }
}
```

---

### 📘 **Example Code: Trie Implementation**  
Let's implement a Trie with the following operations:  
1. Insert a word.  
2. Search for a word.  
3. Check if a prefix exists.  

```java
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    boolean isEndOfWord;

    TrieNode() {
        isEndOfWord = false;
        for (int i = 0; i < 26; i++) {
            children[i] = null;
        }
    }
}

public class Trie {
    private TrieNode root;

    // Constructor
    public Trie() {
        root = new TrieNode();
    }

    // 1. Insert a word
    public void insert(String word) {
        TrieNode current = root;
        for (int i = 0; i < word.length(); i++) {
            int index = word.charAt(i) - 'a';
            if (current.children[index] == null) {
                current.children[index] = new TrieNode();
            }
            current = current.children[index];
        }
        current.isEndOfWord = true;
    }

    // 2. Search for a word
    public boolean search(String word) {
        TrieNode current = root;
        for (int i = 0; i < word.length(); i++) {
            int index = word.charAt(i) - 'a';
            if (current.children[index] == null) {
                return false;
            }
            current = current.children[index];
        }
        return current.isEndOfWord;
    }

    // 3. Check if a prefix exists
    public boolean startsWith(String prefix) {
        TrieNode current = root;
        for (int i = 0; i < prefix.length(); i++) {
            int index = prefix.charAt(i) - 'a';
            if (current.children[index] == null) {
                return false;
            }
            current = current.children[index];
        }
        return true;
    }

    public static void main(String[] args) {
        Trie trie = new Trie();
        trie.insert("apple");
        trie.insert("app");
        trie.insert("bat");

        System.out.println("Search 'apple': " + trie.search("apple")); // true
        System.out.println("Search 'app': " + trie.search("app"));     // true
        System.out.println("Search 'bat': " + trie.search("bat"));     // true
        System.out.println("Search 'batman': " + trie.search("batman"));// false

        System.out.println("Starts with 'app': " + trie.startsWith("app")); // true
        System.out.println("Starts with 'bat': " + trie.startsWith("bat")); // true
        System.out.println("Starts with 'cat': " + trie.startsWith("cat")); // false
    }
}
```

---

### 📊 **Output:**  
```
Search 'apple': true
Search 'app': true
Search 'bat': true
Search 'batman': false
Starts with 'app': true
Starts with 'bat': true
Starts with 'cat': false
```

---

### 🔥 **Explanation:**  
- **Insert:** Navigates each character and creates a new node if it doesn’t exist. Marks the end of the word.  
- **Search:** Checks each character in the Trie. Returns true only if the last character is marked as end of the word.  
- **StartsWith:** Similar to search but returns true even if it’s not the end of a word.  

---

## **🔥 12.4 Segment Tree**  
- **Definition:** A tree-like data structure for efficient range queries and updates.  
- **Applications:**  
  - Range Sum Queries.  
  - Range Minimum/Maximum Queries.  
  - Range GCD or LCM Queries.  
- **Time Complexity:**  
  - **Build:** `O(N log N)`  
  - **Query and Update:** `O(log N)`  
- **Space Complexity:** `O(2 * N)`  

---

### 📘 **Example Code: Segment Tree (Range Sum Query)**  
```java
public class SegmentTree {
    private int[] tree;
    private int n;

    // Constructor to build the Segment Tree
    public SegmentTree(int[] arr) {
        this.n = arr.length;
        tree = new int[2 * n];
        buildTree(arr);
    }

    // Build Segment Tree
    private void buildTree(int[] arr) {
        // Insert leaf nodes
        for (int i = 0; i < n; i++) {
            tree[n + i] = arr[i];
        }
        // Build the tree by calculating parents
        for (int i = n - 1; i > 0; i--) {
            tree[i] = tree[2 * i] + tree[2 * i + 1];
        }
    }

    // Range Sum Query
    public int rangeSum(int left, int right) {
        left += n;  // Shift index to leaf
        right += n;
        int sum = 0;

        while (left <= right) {
            if (left % 2 == 1) {
                sum += tree[left];
                left++;
            }
            if (right % 2 == 0) {
                sum += tree[right];
                right--;
            }
            left /= 2;
            right /= 2;
        }
        return sum;
    }

    // Update value at index
    public void update(int index, int value) {
        index += n;
        tree[index] = value;

        for (int i = index; i > 1; i /= 2) {
            tree[i / 2] = tree[i] + tree[i ^ 1];
        }
    }

    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5};
        SegmentTree segmentTree = new SegmentTree(arr);

        System.out.println("Range Sum (1, 3): " + segmentTree.rangeSum(1, 3));  // 9
        segmentTree.update(2, 10);
        System.out.println("Range Sum (1, 3) after update: " + segmentTree.rangeSum(1, 3));  // 16
    }
}
```

---

### 📊 **Output:**  
```
Range Sum (1, 3): 9
Range Sum (1, 3) after update: 16
```

---

## 🔥 **Next: Algorithm Techniques**  
Next, we will explore **Algorithm Techniques** like Divide and Conquer, Greedy Algorithms, and Backtracking.  

---
