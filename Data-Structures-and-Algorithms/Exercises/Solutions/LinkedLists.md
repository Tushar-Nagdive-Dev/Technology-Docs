Below is a Java program implementing five operations on a singly linked list with clear explanations and well-documented code. We'll define a `Node` class for the linked list structure and a `LinkedListOperations` class containing all methods, along with a `main` method to test them.

```java
// Node class representing each element in the linked list
class Node {
    int data;
    Node next;
    
    Node(int data) {
        this.data = data;
        this.next = null;
    }
}

public class LinkedListOperations {
    // 1. Insert a node at a specific position
    /**
     * Inserts a new node with given data at the specified position.
     * Position is 0-based (0 is head). If position is invalid, does nothing.
     * Time Complexity: O(n), Space Complexity: O(1)
     */
    public static Node insertAtPosition(Node head, int data, int position) {
        Node newNode = new Node(data);
        
        // Insert at beginning (position 0)
        if (position == 0) {
            newNode.next = head;
            return newNode;
        }
        
        Node current = head;
        // Traverse to the node before the desired position
        for (int i = 0; i < position - 1 && current != null; i++) {
            current = current.next;
        }
        
        // If position is out of bounds, do nothing
        if (current == null) return head;
        
        // Insert new node
        newNode.next = current.next;
        current.next = newNode;
        return head;
    }
    
    // 2. Reverse a singly linked list
    /**
     * Reverses the linked list by changing the direction of links.
     * Time Complexity: O(n), Space Complexity: O(1)
     */
    public static Node reverseList(Node head) {
        Node prev = null;    // Previous node starts as null
        Node current = head; // Current node to process
        Node next = null;    // Next node to save before changing link
        
        while (current != null) {
            next = current.next;    // Save next node
            current.next = prev;    // Reverse the link
            prev = current;         // Move prev forward
            current = next;         // Move current forward
        }
        return prev; // New head is the last node
    }
    
    // 3. Check if linked list has a cycle (Floyd’s Cycle-Finding Algorithm)
    /**
     * Uses two pointers (slow and fast) to detect a cycle.
     * If fast meets slow, there's a cycle. Time Complexity: O(n), Space Complexity: O(1)
     */
    public static boolean hasCycle(Node head) {
        if (head == null || head.next == null) return false;
        
        Node slow = head;    // Moves one step
        Node fast = head;    // Moves two steps
        
        while (fast != null && fast.next != null) {
            slow = slow.next;         // Slow moves 1 step
            fast = fast.next.next;    // Fast moves 2 steps
            if (slow == fast) return true; // Meeting point indicates cycle
        }
        return false;
    }
    
    // 4. Merge two sorted linked lists
    /**
     * Merges two sorted lists into one sorted list.
     * Time Complexity: O(n + m), Space Complexity: O(1) excluding recursion stack
     */
    public static Node mergeSortedLists(Node list1, Node list2) {
        // Base cases
        if (list1 == null) return list2;
        if (list2 == null) return list1;
        
        // Choose head of merged list and recurse
        if (list1.data <= list2.data) {
            list1.next = mergeSortedLists(list1.next, list2);
            return list1;
        } else {
            list2.next = mergeSortedLists(list1, list2.next);
            return list2;
        }
    }
    
    // 5. Find middle element in one traversal
    /**
     * Uses two pointers: slow (1 step) and fast (2 steps) to find middle.
     * For even length, returns second middle. Time Complexity: O(n), Space Complexity: O(1)
     */
    public static Node findMiddle(Node head) {
        if (head == null) return null;
        
        Node slow = head;
        Node fast = head;
        
        // Fast moves twice as fast, so when it reaches end, slow is at middle
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }
    
    // Helper method to print list
    public static void printList(Node head) {
        Node current = head;
        while (current != null) {
            System.out.print(current.data + " -> ");
            current = current.next;
        }
        System.out.println("null");
    }
    
    // Main method to test all functions
    public static void main(String[] args) {
        // Test 1: Insert at position
        Node list1 = new Node(1);
        list1.next = new Node(3);
        list1.next.next = new Node(5);
        System.out.println("Original list:");
        printList(list1); // 1 -> 3 -> 5 -> null
        list1 = insertAtPosition(list1, 4, 2);
        System.out.println("After inserting 4 at position 2:");
        printList(list1); // 1 -> 3 -> 4 -> 5 -> null
        
        // Test 2: Reverse list
        System.out.println("Reversed list:");
        list1 = reverseList(list1);
        printList(list1); // 5 -> 4 -> 3 -> 1 -> null
        
        // Test 3: Check for cycle
        Node list2 = new Node(1);
        list2.next = new Node(2);
        list2.next.next = new Node(3);
        list2.next.next.next = list2.next; // Create cycle: 1 -> 2 -> 3 -> 2
        System.out.println("Has cycle? " + hasCycle(list2)); // true
        
        // Test 4: Merge two sorted lists
        Node sorted1 = new Node(1);
        sorted1.next = new Node(3);
        sorted1.next.next = new Node(5);
        Node sorted2 = new Node(2);
        sorted2.next = new Node(4);
        sorted2.next.next = new Node(6);
        System.out.println("Merging sorted lists:");
        printList(sorted1); // 1 -> 3 -> 5 -> null
        printList(sorted2); // 2 -> 4 -> 6 -> null
        Node merged = mergeSortedLists(sorted1, sorted2);
        printList(merged); // 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> null
        
        // Test 5: Find middle
        Node list3 = new Node(1);
        list3.next = new Node(2);
        list3.next.next = new Node(3);
        list3.next.next.next = new Node(4);
        list3.next.next.next.next = new Node(5);
        System.out.println("List for middle:");
        printList(list3); // 1 -> 2 -> 3 -> 4 -> 5 -> null
        Node middle = findMiddle(list3);
        System.out.println("Middle element: " + middle.data); // 3
    }
}
```

### Simple Explanation of Each Method:

1. **Insert at Specific Position:**
   - **What it does:** Adds a new node with given data at a specific position (0-based index).
   - **How:** 
     - If position is 0, make new node the head.
     - Otherwise, traverse to the node before the position, adjust links to insert new node.
   - **Example:** List `1 -> 3 -> 5`, insert 4 at position 2 → `1 -> 3 -> 4 -> 5`.
   - **Key Point:** Checks for invalid positions (beyond list length) and handles them gracefully.

2. **Reverse a Linked List:**
   - **What it does:** Reverses the direction of all links (e.g., `1 -> 3 -> 5` becomes `5 -> 3 -> 1`).
   - **How:** Uses three pointers (prev, current, next):
     - Save next node, point current to prev, move forward.
   - **Example:** `1 -> 3 -> 5` → step-by-step: `null <- 1  3 -> 5`, `null <- 1 <- 3  5`, `null <- 1 <- 3 <- 5`.
   - **Key Point:** In-place reversal, no extra space needed.

3. **Check for Cycle (Floyd’s Algorithm):**
   - **What it does:** Detects if the list loops back on itself (e.g., `1 -> 2 -> 3 -> 2`).
   - **How:** Uses two pointers:
     - Slow moves 1 step, fast moves 2 steps.
     - If they meet, there’s a cycle; if fast reaches end, no cycle.
   - **Example:** `1 -> 2 -> 3 -> 2` → slow at 2, fast at 3 → slow at 3, fast loops to 2 → meet at some point.
   - **Key Point:** “Tortoise and Hare” method, efficient with no extra space.

4. **Merge Two Sorted Lists:**
   - **What it does:** Combines two sorted lists into one sorted list (e.g., `1 -> 3 -> 5` and `2 -> 4 -> 6` → `1 -> 2 -> 3 -> 4 -> 5 -> 6`).
   - **How:** Recursively:
     - Compare heads, pick smaller, link to merged result of rest.
   - **Example:** `1 -> 3` vs `2 -> 4` → 1 < 2, so `1 -> merge(3, 2)` → `1 -> 2 -> 3 -> 4`.
   - **Key Point:** Maintains sorted order, uses existing nodes.

5. **Find Middle Element:**
   - **What it does:** Finds the middle node in one pass (e.g., `1 -> 2 -> 3 -> 4 -> 5` → 3).
   - **How:** Uses two pointers:
     - Slow moves 1 step, fast moves 2 steps.
     - When fast reaches end, slow is at middle.
   - **Example:** `1 -> 2 -> 3 -> 4 -> 5` → slow: 1, fast: 1 → slow: 2, fast: 3 → slow: 3, fast: 5 → done.
   - **Key Point:** For even length, picks second middle (e.g., `1 -> 2 -> 3 -> 4` → 3).

### Output from main():
```
Original list:
1 -> 3 -> 5 -> null
After inserting 4 at position 2:
1 -> 3 -> 4 -> 5 -> null
Reversed list:
5 -> 4 -> 3 -> 1 -> null
Has cycle? true
Merging sorted lists:
1 -> 3 -> 5 -> null
2 -> 4 -> 6 -> null
1 -> 2 -> 3 -> 4 -> 5 -> 6 -> null
List for middle:
1 -> 2 -> 3 -> 4 -> 5 -> null
Middle element: 3
```

You can run this code and tweak the `main()` inputs to explore different scenarios! Let me know if you need more clarification or examples.
