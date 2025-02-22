## 🚀 **Module 4: Mastering Stacks and Queues**  

Stacks and Queues are fundamental linear data structures used in many algorithms and real-world applications. They provide controlled ways of accessing and manipulating data, making them crucial for mastering Data Structures and Algorithms (DSA).

---

# **🔥 4.1 What is a Stack?**  
- **Definition:** A linear data structure that follows the **Last In, First Out (LIFO)** principle.  
- **Example:** Imagine a stack of plates. You can only take the top plate off first.  
- **Operations:**  
  1. **Push:** Add an element to the top of the stack.  
  2. **Pop:** Remove and return the top element.  
  3. **Peek:** View the top element without removing it.  
  4. **isEmpty:** Check if the stack is empty.  

---

## **🔥 4.2 Implementing Stack in Java**  
Java provides a built-in `Stack` class, but let's learn how it works by implementing it manually using a linked list.

---

### 📘 **Example Code: Stack Implementation using Linked List**  
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

public class MyStack {
    Node top;

    // 1. Push Operation
    public void push(int data) {
        Node newNode = new Node(data);
        newNode.next = top;
        top = newNode;
        System.out.println(data + " pushed to stack");
    }

    // 2. Pop Operation
    public int pop() {
        if (top == null) {
            System.out.println("Stack Underflow");
            return -1;
        }
        int popped = top.data;
        top = top.next;
        return popped;
    }

    // 3. Peek Operation
    public int peek() {
        if (top == null) {
            System.out.println("Stack is Empty");
            return -1;
        }
        return top.data;
    }

    // 4. Check if Stack is Empty
    public boolean isEmpty() {
        return top == null;
    }

    // Display Stack
    public void display() {
        Node temp = top;
        System.out.print("Stack: ");
        while (temp != null) {
            System.out.print(temp.data + " -> ");
            temp = temp.next;
        }
        System.out.println("null");
    }

    public static void main(String[] args) {
        MyStack stack = new MyStack();
        stack.push(10);
        stack.push(20);
        stack.push(30);
        stack.display();

        System.out.println("\nTop element is: " + stack.peek());

        System.out.println("Popped element: " + stack.pop());
        stack.display();

        System.out.println("Stack is empty? " + stack.isEmpty());
    }
}
```

---

### 📊 **Output:**  
```
10 pushed to stack
20 pushed to stack
30 pushed to stack
Stack: 30 -> 20 -> 10 -> null

Top element is: 30
Popped element: 30
Stack: 20 -> 10 -> null
Stack is empty? false
```

---

### 🔥 **Explanation:**  
- **Push:** Adds a new node at the top of the stack.  
- **Pop:** Removes and returns the top node.  
- **Peek:** Returns the data of the top node without removing it.  
- **isEmpty:** Checks if `top` is `null`.  

---

### 🔥 **Time and Space Complexity:**  
- **Push:** `O(1)` — Constant time.  
- **Pop:** `O(1)` — Constant time.  
- **Peek:** `O(1)` — Constant time.  
- **Space Complexity:** `O(N)` for storing `N` nodes.  

---

## **🔥 4.3 Real-World Applications of Stacks**  
1. **Function Call Stack:** Manages function calls and returns in programming.  
2. **Undo Mechanism:** Stores previous states for undo actions in text editors.  
3. **Expression Evaluation:** Used in parsing expressions (e.g., Infix to Postfix conversion).  

---

## **🔥 4.4 Example: Balanced Parentheses using Stack**  
Check if parentheses in an expression are balanced.  
- **Example:**  
    - `"(())"` → Balanced  
    - `"(()"` → Not Balanced  

---

### 📘 **Example Code: Balanced Parentheses**  
```java
import java.util.Stack;

public class BalancedParentheses {
    public static boolean isBalanced(String expr) {
        Stack<Character> stack = new Stack<>();

        for (char ch : expr.toCharArray()) {
            if (ch == '(') {
                stack.push(ch);
            } else if (ch == ')') {
                if (stack.isEmpty()) {
                    return false;
                }
                stack.pop();
            }
        }
        return stack.isEmpty();
    }

    public static void main(String[] args) {
        String expr1 = "(())";
        String expr2 = "(()";

        System.out.println(expr1 + " is balanced? " + isBalanced(expr1));
        System.out.println(expr2 + " is balanced? " + isBalanced(expr2));
    }
}
```

---

### 📊 **Output:**  
```
(()) is balanced? true
(() is balanced? false
```

---

## **🔥 4.5 What is a Queue?**  
- **Definition:** A linear data structure that follows the **First In, First Out (FIFO)** principle.  
- **Example:** A queue of people at a ticket counter.  
- **Operations:**  
  1. **Enqueue:** Add an element to the end of the queue.  
  2. **Dequeue:** Remove and return the front element.  
  3. **Peek:** View the front element without removing it.  
  4. **isEmpty:** Check if the queue is empty.  

---

## **🔥 4.6 Implementing Queue in Java**  
Let's implement a Queue using a linked list.

---

### 📘 **Example Code: Queue Implementation using Linked List**  
```java
class QNode {
    int data;
    QNode next;

    QNode(int data) {
        this.data = data;
        this.next = null;
    }
}

public class MyQueue {
    QNode front, rear;

    // 1. Enqueue Operation
    public void enqueue(int data) {
        QNode newNode = new QNode(data);
        if (rear == null) {
            front = rear = newNode;
            return;
        }
        rear.next = newNode;
        rear = newNode;
        System.out.println(data + " enqueued to queue");
    }

    // 2. Dequeue Operation
    public int dequeue() {
        if (front == null) {
            System.out.println("Queue Underflow");
            return -1;
        }
        int dequeued = front.data;
        front = front.next;
        if (front == null) {
            rear = null;
        }
        return dequeued;
    }

    // 3. Peek Operation
    public int peek() {
        if (front == null) {
            System.out.println("Queue is Empty");
            return -1;
        }
        return front.data;
    }

    // 4. Check if Queue is Empty
    public boolean isEmpty() {
        return front == null;
    }

    public static void main(String[] args) {
        MyQueue queue = new MyQueue();
        queue.enqueue(10);
        queue.enqueue(20);
        queue.enqueue(30);

        System.out.println("Front element is: " + queue.peek());

        System.out.println("Dequeued element: " + queue.dequeue());
        System.out.println("Front element is: " + queue.peek());

        System.out.println("Queue is empty? " + queue.isEmpty());
    }
}
```

---

### 📊 **Output:**  
```
10 enqueued to queue
20 enqueued to queue
30 enqueued to queue
Front element is: 10
Dequeued element: 10
Front element is: 20
Queue is empty? false
```

---

## 🔥 **Next: Trees**  
Next, we will learn **Trees**, including Binary Trees and Binary Search Trees, which are essential for hierarchical data representation and fast searching.  

---

## 🔥 **Ready to Proceed?**  
Let me know when you're ready to move on to **Trees**! 🚀
