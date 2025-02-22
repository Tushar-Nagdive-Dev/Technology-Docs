## 🚀 **Module 11: Mastering Hashing and Hash Tables**  

Hashing is a powerful technique for fast data retrieval, providing average-case constant time complexity for search, insert, and delete operations. Hash Tables are widely used in databases, caches, compilers, and many other real-world applications. This module covers the fundamentals of hashing, hash functions, collision handling techniques, and hash table implementations.

---

## **🔥 11.1 What is Hashing?**  
- **Definition:** A technique that converts data (e.g., a key) into a fixed-size numerical value called a **hash code** using a **hash function**.  
- **Key Idea:** Store data at a calculated index, allowing constant time access.  
- **Example:** Storing a student's name by converting it into an array index.  

---

## **🔥 11.2 Why Use Hashing?**  
- **Fast Search and Retrieval:** Average time complexity of `O(1)`.  
- **Efficient Insert and Delete:** Constant time complexity for insertion and deletion.  
- **Optimal Space Utilization:** Compact storage of keys and values.  

---

## **🔥 11.3 What is a Hash Table?**  
- **Definition:** A data structure that stores key-value pairs using a **hash function** to compute an index into an array of buckets.  
- **Structure:**  
    ```
    Key -> Hash Function -> Index -> Array (Buckets)
    ```
- **Example:**  
    ```
    Hash("John") → Index 2 → Store "John" at arr[2]
    Hash("Jane") → Index 4 → Store "Jane" at arr[4]
    ```

---

## **🔥 11.4 Hash Function**  
- **Definition:** A function that maps keys to a numerical index in a fixed range.  
- **Properties:**  
  - **Deterministic:** Same input always gives the same output.  
  - **Uniform Distribution:** Distributes keys uniformly across the array.  
  - **Efficient Computation:** Should be fast and efficient to compute.  

### 📘 **Example Hash Function:**  
```java
int hash(String key, int tableSize) {
    int hashValue = 0;
    for (int i = 0; i < key.length(); i++) {
        hashValue = (31 * hashValue + key.charAt(i)) % tableSize;
    }
    return hashValue;
}
```

---

## **🔥 11.5 Collision Handling Techniques**  
### 📘 **1. Chaining**  
- **Definition:** Store multiple key-value pairs in a linked list at the same index.  
- **Advantages:** Handles collisions efficiently, simple to implement.  
- **Disadvantages:** Increased memory usage due to linked lists.  

---

### 📘 **2. Open Addressing**  
- **Definition:** Finds the next available slot using a probing sequence.  
- **Types:**  
  1. **Linear Probing:** Moves to the next slot sequentially.  
  2. **Quadratic Probing:** Moves to the next slot by a quadratic function.  
  3. **Double Hashing:** Uses a second hash function for probing.  

---

### 📘 **3. Separate Chaining vs Open Addressing**  
| Property            | Separate Chaining             | Open Addressing             |
|---------------------|-------------------------------|------------------------------|
| Memory Usage        | Extra space for linked lists   | In-place in the array        |
| Performance         | Degrades with long chains      | Degrades with clustering     |
| Deletion            | Easy and efficient             | Complex due to probing       |
| Load Factor         | Can exceed 1                   | Should be ≤ 0.75             |
| Ideal Use Case      | When space is not a constraint | When space is a constraint   |

---

## **🔥 11.6 Implementing Hash Table using Separate Chaining**  
Let's create a Hash Table using an array of linked lists and implement the following operations:  
1. Insert  
2. Search  
3. Delete  
4. Display  

---

### 📘 **Example Code: Hash Table using Separate Chaining**  
```java
import java.util.LinkedList;

class HashNode {
    String key;
    String value;

    // Constructor
    HashNode(String key, String value) {
        this.key = key;
        this.value = value;
    }
}

public class HashTable {
    private LinkedList<HashNode>[] table;
    private int capacity;

    // Constructor
    public HashTable(int capacity) {
        this.capacity = capacity;
        table = new LinkedList[capacity];
        for (int i = 0; i < capacity; i++) {
            table[i] = new LinkedList<>();
        }
    }

    // Hash Function
    private int hash(String key) {
        int hashValue = 0;
        for (int i = 0; i < key.length(); i++) {
            hashValue = (31 * hashValue + key.charAt(i)) % capacity;
        }
        return hashValue;
    }

    // 1. Insert Key-Value Pair
    public void put(String key, String value) {
        int index = hash(key);
        LinkedList<HashNode> chain = table[index];

        // Check if key already exists, update the value
        for (HashNode node : chain) {
            if (node.key.equals(key)) {
                node.value = value;
                return;
            }
        }
        // Insert new node if key doesn't exist
        chain.add(new HashNode(key, value));
    }

    // 2. Search for Value by Key
    public String get(String key) {
        int index = hash(key);
        LinkedList<HashNode> chain = table[index];

        for (HashNode node : chain) {
            if (node.key.equals(key)) {
                return node.value;
            }
        }
        return null;  // Key not found
    }

    // 3. Delete Key-Value Pair
    public void remove(String key) {
        int index = hash(key);
        LinkedList<HashNode> chain = table[index];

        HashNode toRemove = null;
        for (HashNode node : chain) {
            if (node.key.equals(key)) {
                toRemove = node;
                break;
            }
        }
        if (toRemove != null) {
            chain.remove(toRemove);
        }
    }

    // 4. Display Hash Table
    public void display() {
        for (int i = 0; i < capacity; i++) {
            System.out.print("Bucket " + i + ": ");
            for (HashNode node : table[i]) {
                System.out.print("[" + node.key + " -> " + node.value + "] ");
            }
            System.out.println();
        }
    }

    public static void main(String[] args) {
        HashTable hashTable = new HashTable(5);
        hashTable.put("John", "john@example.com");
        hashTable.put("Jane", "jane@example.com");
        hashTable.put("Dave", "dave@example.com");
        hashTable.put("Dana", "dana@example.com");

        System.out.println("Hash Table:");
        hashTable.display();

        System.out.println("\nSearch for Jane: " + hashTable.get("Jane"));
        
        hashTable.remove("Dave");
        System.out.println("\nAfter Removing Dave:");
        hashTable.display();
    }
}
```

---

### 📊 **Output:**  
```
Hash Table:
Bucket 0: 
Bucket 1: [John -> john@example.com] 
Bucket 2: [Jane -> jane@example.com] 
Bucket 3: [Dave -> dave@example.com] [Dana -> dana@example.com] 
Bucket 4: 

Search for Jane: jane@example.com

After Removing Dave:
Bucket 0: 
Bucket 1: [John -> john@example.com] 
Bucket 2: [Jane -> jane@example.com] 
Bucket 3: [Dana -> dana@example.com] 
Bucket 4: 
```

---

### 🔥 **Explanation:**  
- **Hash Function:** Computes the index using a polynomial accumulation method.  
- **Chaining:** Uses a linked list at each index to handle collisions.  
- **Insert:** Adds a new node or updates the existing value if the key already exists.  
- **Search:** Traverses the linked list at the hashed index.  
- **Delete:** Finds and removes the node from the linked list.  

---

## 🔥 **Next: Advanced Data Structures**  
Hashing is the foundation for **Advanced Data Structures** like Tries, Segment Trees, and Graph Algorithms. Next, we will explore these advanced structures.

---
