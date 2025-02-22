## 🚀 **Module 5: Mastering Trees**  

Trees are hierarchical data structures that store elements in a parent-child relationship. They are used in databases, file systems, search engines, and many other applications. This module covers the fundamental concepts of trees, different types, and practical implementations.

---

## **🔥 5.1 What is a Tree?**  
- **Definition:** A hierarchical data structure with a root node and child nodes forming a parent-child relationship.  
- **Example:** A family tree, organizational hierarchy, or a file system.  

---

### 📘 **Tree Terminology**  
- **Root:** The top node in a tree.  
- **Parent:** A node with one or more child nodes.  
- **Child:** A node that has a parent node.  
- **Leaf:** A node with no children.  
- **Subtree:** A tree consisting of a node and its descendants.  
- **Height:** The longest path from the root to a leaf.  
- **Depth:** The distance from the root to a node.  

---

### 📘 **Types of Trees**  
1. **Binary Tree:** Each node has at most two children (left and right).  
2. **Binary Search Tree (BST):** A binary tree with the property:
    - Left child < Parent < Right child  
3. **AVL Tree:** A self-balancing binary search tree.  
4. **Heap:** A complete binary tree with a heap property:
    - **Max-Heap:** Parent ≥ Child  
    - **Min-Heap:** Parent ≤ Child  
5. **Trie:** A tree used for storing strings or words efficiently.  

---

## **🔥 5.2 Binary Tree**  
- **Definition:** A tree where each node has at most two children.  
- **Applications:** Expression trees, Huffman coding trees, and binary heaps.  

---

### 📘 **Structure of a Binary Tree Node**  
```java
class TreeNode {
    int data;
    TreeNode left;
    TreeNode right;

    // Constructor
    TreeNode(int data) {
        this.data = data;
        this.left = null;
        this.right = null;
    }
}
```

---

## **🔥 5.3 Implementing Binary Tree in Java**  
Let's create a binary tree and perform the following operations:  
1. Insertion  
2. Preorder Traversal  
3. Inorder Traversal  
4. Postorder Traversal  
5. Level Order (Breadth-First) Traversal  

---

### 📘 **Example Code: Binary Tree Implementation**  
```java
class TreeNode {
    int data;
    TreeNode left;
    TreeNode right;

    TreeNode(int data) {
        this.data = data;
        this.left = null;
        this.right = null;
    }
}

public class BinaryTree {
    TreeNode root;

    // 1. Insertion
    public TreeNode insert(TreeNode root, int data) {
        if (root == null) {
            root = new TreeNode(data);
            return root;
        }
        if (data < root.data) {
            root.left = insert(root.left, data);
        } else if (data > root.data) {
            root.right = insert(root.right, data);
        }
        return root;
    }

    // 2. Preorder Traversal (Root -> Left -> Right)
    public void preorder(TreeNode node) {
        if (node != null) {
            System.out.print(node.data + " ");
            preorder(node.left);
            preorder(node.right);
        }
    }

    // 3. Inorder Traversal (Left -> Root -> Right)
    public void inorder(TreeNode node) {
        if (node != null) {
            inorder(node.left);
            System.out.print(node.data + " ");
            inorder(node.right);
        }
    }

    // 4. Postorder Traversal (Left -> Right -> Root)
    public void postorder(TreeNode node) {
        if (node != null) {
            postorder(node.left);
            postorder(node.right);
            System.out.print(node.data + " ");
        }
    }

    // 5. Level Order Traversal (Breadth-First)
    public void levelOrder(TreeNode root) {
        if (root == null) return;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);

        while (!queue.isEmpty()) {
            TreeNode temp = queue.poll();
            System.out.print(temp.data + " ");
            if (temp.left != null) queue.add(temp.left);
            if (temp.right != null) queue.add(temp.right);
        }
    }

    public static void main(String[] args) {
        BinaryTree tree = new BinaryTree();
        tree.root = tree.insert(tree.root, 50);
        tree.insert(tree.root, 30);
        tree.insert(tree.root, 20);
        tree.insert(tree.root, 40);
        tree.insert(tree.root, 70);
        tree.insert(tree.root, 60);
        tree.insert(tree.root, 80);

        System.out.println("Preorder Traversal:");
        tree.preorder(tree.root);

        System.out.println("\nInorder Traversal:");
        tree.inorder(tree.root);

        System.out.println("\nPostorder Traversal:");
        tree.postorder(tree.root);

        System.out.println("\nLevel Order Traversal:");
        tree.levelOrder(tree.root);
    }
}
```

---

### 📊 **Output:**  
```
Preorder Traversal:
50 30 20 40 70 60 80 

Inorder Traversal:
20 30 40 50 60 70 80 

Postorder Traversal:
20 40 30 60 80 70 50 

Level Order Traversal:
50 30 70 20 40 60 80 
```

---

### 🔥 **Explanation:**  
- **Preorder Traversal:** Visits the root first, then left subtree, and finally the right subtree.  
- **Inorder Traversal:** Visits the left subtree, root, and then the right subtree (Gives sorted order for BST).  
- **Postorder Traversal:** Visits the left subtree, right subtree, and then the root.  
- **Level Order Traversal:** Visits nodes level by level using a queue.  

---

### 🔥 **Time and Space Complexity:**  
- **Insertion:** `O(N)` in the worst case for unbalanced trees, `O(log N)` for balanced trees.  
- **Traversal (All Types):** `O(N)` — Each node is visited once.  
- **Space Complexity:** `O(N)` for the call stack in recursive traversal or queue in level order.  

---

## **🔥 5.4 Binary Search Tree (BST)**  
- **Definition:** A binary tree where for each node:
    - Left child < Parent < Right child  
- **Properties:**  
    - Inorder traversal gives nodes in ascending order.  
    - Efficient search, insertion, and deletion (`O(log N)` for balanced BST).  

---

### 📘 **Example Code: Binary Search Tree Operations**  
```java
// Check if a value is present in the BST
public boolean search(TreeNode root, int key) {
    if (root == null) return false;
    if (root.data == key) return true;
    return key < root.data ? search(root.left, key) : search(root.right, key);
}

// Delete a node in the BST
public TreeNode delete(TreeNode root, int key) {
    if (root == null) return root;
    if (key < root.data) {
        root.left = delete(root.left, key);
    } else if (key > root.data) {
        root.right = delete(root.right, key);
    } else {
        // Node with one child or no child
        if (root.left == null) return root.right;
        if (root.right == null) return root.left;
        // Node with two children
        root.data = minValue(root.right);
        root.right = delete(root.right, root.data);
    }
    return root;
}

// Find minimum value in BST
public int minValue(TreeNode node) {
    int minVal = node.data;
    while (node.left != null) {
        minVal = node.left.data;
        node = node.left;
    }
    return minVal;
}
```

---

## 🔥 **Next: Heaps and Priority Queues**  
Trees are the foundation for **Heaps and Priority Queues**, which are used in sorting algorithms, scheduling, and graph algorithms.  

---
