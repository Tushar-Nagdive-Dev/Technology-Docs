## 🚀 **Module 3: Mastering Linked Lists**  

Linked Lists are fundamental data structures in computer science. Unlike arrays, linked lists are dynamic, allowing efficient insertion and deletion. They are the foundation for many advanced data structures like stacks, queues, and graphs.

---

## **🔥 3.1 What is a Linked List?**  
- **Definition:** A linear data structure where elements are stored as nodes. Each node contains:  
  1. **Data:** The value stored in the node.  
  2. **Pointer (Next):** A reference to the next node in the list.  

---

### 📘 **Types of Linked Lists**  
1. **Singly Linked List:** Each node points to the next node.  
2. **Doubly Linked List:** Each node points to both the next and previous nodes.  
3. **Circular Linked List:** Last node points back to the first node, forming a loop.  

---

### 🔥 **Why Use Linked Lists?**  
- **Dynamic Size:** Linked lists grow or shrink as needed.  
- **Efficient Insertion/Deletion:** Insertion or deletion at the beginning or end is `O(1)`.  
- **Memory Utilization:** No need to pre-allocate memory like arrays.  

---

## **🔥 3.2 Singly Linked List**  

### 📘 **Structure of a Node**  
Each node contains:  
- **Data**: Value stored in the node.  
- **Next**: Pointer to the next node.

```java
class Node {
    int data;
    Node next;

    // Constructor
    Node(int data) {
        this.data = data;
        this.next = null;
    }
}
```

---

### 📘 **Example Code: Singly Linked List**  
Let's create a Singly Linked List and perform the following operations:  
1. Insert a node at the beginning.  
2. Insert a node at the end.  
3. Delete a node by value.  
4. Search for a node.  
5. Display all nodes.  

```java
class Node {
    int data;
    Node next;

    // Constructor
    Node(int data) {
        this.data = data;
        this.next = null;
    }
}

public class SinglyLinkedList {
    Node head;

    // 1. Insert at Beginning
    public void insertAtBeginning(int data) {
        Node newNode = new Node(data);
        newNode.next = head;
        head = newNode;
    }

    // 2. Insert at End
    public void insertAtEnd(int data) {
        Node newNode = new Node(data);
        if (head == null) {
            head = newNode;
            return;
        }
        Node temp = head;
        while (temp.next != null) {
            temp = temp.next;
        }
        temp.next = newNode;
    }

    // 3. Delete by Value
    public void deleteByValue(int data) {
        if (head == null) {
            System.out.println("List is empty");
            return;
        }
        if (head.data == data) {
            head = head.next;
            return;
        }
        Node temp = head;
        while (temp.next != null) {
            if (temp.next.data == data) {
                temp.next = temp.next.next;
                return;
            }
            temp = temp.next;
        }
        System.out.println("Node with value " + data + " not found");
    }

    // 4. Search for a Node
    public boolean search(int data) {
        Node temp = head;
        while (temp != null) {
            if (temp.data == data) {
                return true;
            }
            temp = temp.next;
        }
        return false;
    }

    // 5. Display all Nodes
    public void display() {
        Node temp = head;
        while (temp != null) {
            System.out.print(temp.data + " -> ");
            temp = temp.next;
        }
        System.out.println("null");
    }

    public static void main(String[] args) {
        SinglyLinkedList list = new SinglyLinkedList();
        list.insertAtEnd(10);
        list.insertAtEnd(20);
        list.insertAtEnd(30);
        list.insertAtBeginning(5);
        list.insertAtBeginning(1);

        System.out.println("Linked List:");
        list.display();

        System.out.println("\nSearching for 20: " + list.search(20));
        System.out.println("Searching for 50: " + list.search(50));

        list.deleteByValue(20);
        System.out.println("\nAfter Deleting 20:");
        list.display();
    }
}
```

---

### 📊 **Output:**  
```
Linked List:
1 -> 5 -> 10 -> 20 -> 30 -> null

Searching for 20: true
Searching for 50: false

After Deleting 20:
1 -> 5 -> 10 -> 30 -> null
```

---

### 🔥 **Explanation:**  
- **Insert at Beginning:** Adds a new node at the start and points it to the old head.  
- **Insert at End:** Traverses to the end and links the new node.  
- **Delete by Value:** Searches for the node and adjusts the pointer of the previous node.  
- **Search:** Traverses the list to find the value.  
- **Display:** Traverses and prints each node’s data.  

---

### 🔥 **Time and Space Complexity:**  
- **Insert at Beginning:** `O(1)`  
- **Insert at End:** `O(N)`  
- **Delete by Value:** `O(N)`  
- **Search:** `O(N)`  
- **Space Complexity:** `O(N)` for storing N nodes.  

---

## **🔥 3.3 Doubly Linked List**  
- Each node points to both the next and previous nodes.  
- This allows bidirectional traversal but requires extra memory for the previous pointer.

---

### 📘 **Example Code: Doubly Linked List**  
```java
class DNode {
    int data;
    DNode prev;
    DNode next;

    DNode(int data) {
        this.data = data;
        this.prev = null;
        this.next = null;
    }
}

public class DoublyLinkedList {
    DNode head;

    // Insert at End
    public void insertAtEnd(int data) {
        DNode newNode = new DNode(data);
        if (head == null) {
            head = newNode;
            return;
        }
        DNode temp = head;
        while (temp.next != null) {
            temp = temp.next;
        }
        temp.next = newNode;
        newNode.prev = temp;
    }

    // Display in Forward Direction
    public void displayForward() {
        DNode temp = head;
        while (temp != null) {
            System.out.print(temp.data + " <-> ");
            temp = temp.next;
        }
        System.out.println("null");
    }

    // Display in Reverse Direction
    public void displayReverse() {
        DNode temp = head;
        if (temp == null) return;
        while (temp.next != null) {
            temp = temp.next;
        }
        while (temp != null) {
            System.out.print(temp.data + " <-> ");
            temp = temp.prev;
        }
        System.out.println("null");
    }

    public static void main(String[] args) {
        DoublyLinkedList list = new DoublyLinkedList();
        list.insertAtEnd(10);
        list.insertAtEnd(20);
        list.insertAtEnd(30);

        System.out.println("Forward:");
        list.displayForward();
        
        System.out.println("Reverse:");
        list.displayReverse();
    }
}
```

---

### 📊 **Output:**  
```
Forward:
10 <-> 20 <-> 30 <-> null

Reverse:
30 <-> 20 <-> 10 <-> null
```

---

## **📝 Exercise Set:**  
1. Implement insertion at a specific position in a singly linked list.  
2. Reverse a singly linked list.  
3. Check if a linked list has a cycle (using Floyd’s cycle-finding algorithm).  
4. Merge two sorted linked lists into one sorted linked list.  
5. Find the middle element of a linked list in one traversal.  

---

## 🔥 **Next: Stacks and Queues**  
Linked Lists are the foundation of **Stacks and Queues**, which are widely used in algorithms and real-world applications.  

---

## 🔥 **Ready to Proceed?**  
Let me know when you're ready to move on to **Stacks and Queues**! 🚀
