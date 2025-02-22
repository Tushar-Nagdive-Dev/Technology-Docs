## 🚀 **Module 6: Mastering Heaps and Priority Queues**  

Heaps and Priority Queues are advanced tree-based data structures that are widely used in sorting algorithms, scheduling tasks, and graph algorithms. They provide efficient ways to access the minimum or maximum element in constant time.

---

## **🔥 6.1 What is a Heap?**  
- **Definition:** A complete binary tree that satisfies the heap property:  
  - **Max-Heap:** Parent node ≥ Child nodes.  
  - **Min-Heap:** Parent node ≤ Child nodes.  
- **Applications:**  
  - Priority Queues  
  - Heap Sort  
  - Graph algorithms (Dijkstra’s shortest path)  

---

## **🔥 6.2 Types of Heaps**  
1. **Max-Heap:** The maximum element is at the root.  
    - **Example:** `[50, 30, 20, 10, 15, 5, 8]`  
    - **Heap Property:** Every parent node is greater than or equal to its children.  
2. **Min-Heap:** The minimum element is at the root.  
    - **Example:** `[5, 10, 8, 15, 30, 20, 50]`  
    - **Heap Property:** Every parent node is less than or equal to its children.  

---

### 📘 **Array Representation of Heaps**  
- Heaps are typically implemented as arrays to optimize space.  
- For a node at index `i`:  
  - **Parent:** `(i-1) / 2`  
  - **Left Child:** `2 * i + 1`  
  - **Right Child:** `2 * i + 2`  

---

## **🔥 6.3 Operations on Heaps**  
1. **Insert:** Add a new element and adjust the heap property using **Heapify-Up**.  
2. **Delete (Extract Max/Min):** Remove the root and adjust the heap property using **Heapify-Down**.  
3. **Peek:** View the root element (maximum or minimum) without removing it.  

---

## **🔥 6.4 Implementing Max-Heap in Java**  
Let's create a Max-Heap using an array and implement the following operations:  
1. Insert  
2. Delete (Extract Max)  
3. Peek  
4. Display  

---

### 📘 **Example Code: Max-Heap Implementation**  
```java
public class MaxHeap {
    private int[] heap;
    private int size;
    private int capacity;

    // Constructor
    public MaxHeap(int capacity) {
        this.capacity = capacity;
        this.size = 0;
        this.heap = new int[capacity];
    }

    // Get parent, left, and right child indices
    private int parent(int index) { return (index - 1) / 2; }
    private int leftChild(int index) { return 2 * index + 1; }
    private int rightChild(int index) { return 2 * index + 2; }

    // Swap two elements
    private void swap(int index1, int index2) {
        int temp = heap[index1];
        heap[index1] = heap[index2];
        heap[index2] = temp;
    }

    // Insert a new element
    public void insert(int value) {
        if (size == capacity) {
            System.out.println("Heap is full");
            return;
        }
        heap[size] = value;
        int current = size;
        size++;

        // Heapify-Up
        while (current > 0 && heap[current] > heap[parent(current)]) {
            swap(current, parent(current));
            current = parent(current);
        }
    }

    // Delete and return the maximum element
    public int extractMax() {
        if (size == 0) {
            System.out.println("Heap is empty");
            return -1;
        }
        int max = heap[0];
        heap[0] = heap[size - 1];
        size--;

        // Heapify-Down
        heapifyDown(0);

        return max;
    }

    // Heapify-Down
    private void heapifyDown(int index) {
        int largest = index;
        int left = leftChild(index);
        int right = rightChild(index);

        if (left < size && heap[left] > heap[largest]) {
            largest = left;
        }
        if (right < size && heap[right] > heap[largest]) {
            largest = right;
        }
        if (largest != index) {
            swap(index, largest);
            heapifyDown(largest);
        }
    }

    // Peek the maximum element
    public int peek() {
        if (size == 0) {
            System.out.println("Heap is empty");
            return -1;
        }
        return heap[0];
    }

    // Display the heap
    public void display() {
        System.out.print("Heap: ");
        for (int i = 0; i < size; i++) {
            System.out.print(heap[i] + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        MaxHeap maxHeap = new MaxHeap(10);
        maxHeap.insert(50);
        maxHeap.insert(30);
        maxHeap.insert(20);
        maxHeap.insert(15);
        maxHeap.insert(10);
        maxHeap.insert(8);
        maxHeap.insert(60);

        System.out.println("Max-Heap:");
        maxHeap.display();

        System.out.println("\nMaximum Element: " + maxHeap.peek());

        System.out.println("Extracted Max: " + maxHeap.extractMax());
        maxHeap.display();

        System.out.println("Extracted Max: " + maxHeap.extractMax());
        maxHeap.display();
    }
}
```

---

### 📊 **Output:**  
```
Max-Heap:
60 30 50 15 10 8 20 

Maximum Element: 60
Extracted Max: 60
Heap: 50 30 20 15 10 8 

Extracted Max: 50
Heap: 30 15 20 8 10 
```

---

### 🔥 **Explanation:**  
- **Insert:** New elements are added at the end and moved up using **Heapify-Up** to maintain the Max-Heap property.  
- **Extract Max:** The root element is removed, the last element is moved to the root, and then **Heapify-Down** is performed.  
- **Heapify-Up:** Moves the element up until the Max-Heap property is restored.  
- **Heapify-Down:** Moves the element down until the Max-Heap property is restored.  

---

### 🔥 **Time and Space Complexity:**  
- **Insert:** `O(log N)` — Due to Heapify-Up.  
- **Extract Max:** `O(log N)` — Due to Heapify-Down.  
- **Peek:** `O(1)` — Direct access to the root.  
- **Space Complexity:** `O(N)` for storing N elements.  

---

## **🔥 6.5 Priority Queue**  
- **Definition:** A special type of queue where each element has a priority.  
- **Behavior:**  
  - Elements with higher priority are dequeued first.  
  - If priorities are the same, elements are dequeued in the order they were enqueued.  
- **Implementation:** Priority Queues are typically implemented using Heaps for efficient access to the maximum or minimum element.  

---

### 📘 **Example Code: Priority Queue using Java's PriorityQueue Class**  
```java
import java.util.PriorityQueue;

public class PriorityQueueExample {
    public static void main(String[] args) {
        // Min-Heap by default
        PriorityQueue<Integer> pq = new PriorityQueue<>();

        pq.add(30);
        pq.add(10);
        pq.add(50);
        pq.add(20);

        System.out.println("Priority Queue (Min-Heap):");
        while (!pq.isEmpty()) {
            System.out.print(pq.poll() + " ");
        }
    }
}
```

---

### 📊 **Output:**  
```
Priority Queue (Min-Heap):
10 20 30 50 
```

---

## 🔥 **Next: Graphs**  
Heaps and Priority Queues are fundamental for implementing Graph algorithms like Dijkstra’s Shortest Path. Next, we will learn about **Graphs** and their traversal techniques.

---
