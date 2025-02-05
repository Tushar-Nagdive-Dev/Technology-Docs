# **🚀 Lesson 10: Composite Pattern – Structuring Hierarchical Objects**

The **Composite Pattern** is a **structural design pattern** used to handle **hierarchical structures** where **individual objects and groups of objects** should be treated the same way.

---

## **📌 1. What is the Composite Pattern?**
The **Composite Pattern**:
✔ **Combines objects into tree structures** to represent hierarchies.  
✔ **Allows treating individual objects and groups uniformly**.  
✔ **Follows the "part-whole" hierarchy** (e.g., files inside folders).  

---

## **📌 2. When to Use the Composite Pattern?**
✔ When you need to **represent hierarchical structures** like a **file system, menu structure, or company hierarchy**.  
✔ When you want to **treat individual objects and collections of objects the same way**.  
✔ When following the **Open/Closed Principle** (extending functionality without modifying existing code).  

---

## **📌 3. Real-World Analogy – File System 📁**
A **file system** consists of:
- **Files** (individual objects).
- **Folders** (which contain files or other folders).

A **folder can contain both files and other folders**, but **both should have common behaviors like `showDetails()`**.

---

## **📌 4. Implementing Composite Pattern in Java**
Let’s build a **File System Simulation** where:
- **File** is a leaf node.
- **Folder** is a composite node that can contain files and other folders.

---

### **Step 1: Create the Component Interface**
This interface represents both **Files and Folders**.

```java
// Component Interface
public interface FileSystemComponent {
    void showDetails(); // Common operation for both Files and Folders
}
```

---

### **Step 2: Implement the Leaf Node (File)**
A **File** is an **individual component** with no children.

```java
// Leaf Node (Individual File)
public class File implements FileSystemComponent {
    private String name;

    public File(String name) {
        this.name = name;
    }

    @Override
    public void showDetails() {
        System.out.println("File: " + name);
    }
}
```
✔ Implements `showDetails()`.  
✔ Represents **a single file**.  

---

### **Step 3: Implement the Composite Node (Folder)**
A **Folder** can contain **Files or other Folders**.

```java
import java.util.ArrayList;
import java.util.List;

// Composite Node (Folder containing Files & Folders)
public class Folder implements FileSystemComponent {
    private String name;
    private List<FileSystemComponent> components = new ArrayList<>();

    public Folder(String name) {
        this.name = name;
    }

    public void addComponent(FileSystemComponent component) {
        components.add(component);
    }

    public void removeComponent(FileSystemComponent component) {
        components.remove(component);
    }

    @Override
    public void showDetails() {
        System.out.println("Folder: " + name);
        for (FileSystemComponent component : components) {
            component.showDetails(); // Recursively call showDetails()
        }
    }
}
```
✔ **Holds a list of `FileSystemComponent` (both Files & Folders)**.  
✔ **Uses recursion** to display nested structures.  

---

### **Step 4: Using the Composite Pattern**
```java
public class Main {
    public static void main(String[] args) {
        // Creating Files
        File file1 = new File("document.txt");
        File file2 = new File("photo.jpg");
        File file3 = new File("music.mp3");

        // Creating Folders
        Folder folder1 = new Folder("Downloads");
        Folder folder2 = new Folder("Music");

        // Adding Files to Folders
        folder1.addComponent(file1);
        folder1.addComponent(file2);

        folder2.addComponent(file3);

        // Creating Root Folder
        Folder root = new Folder("Root");
        root.addComponent(folder1);
        root.addComponent(folder2);

        // Display Structure
        root.showDetails();
    }
}
```

---

## **📌 5. Expected Output**
```
Folder: Root
Folder: Downloads
File: document.txt
File: photo.jpg
Folder: Music
File: music.mp3
```
✔ **Files and folders are treated the same way** (`showDetails()`).  
✔ **Folders can contain other folders** recursively.  

---

## **📌 6. Key Features of Composite Pattern**
| **Feature** | **Explanation** |
|------------|----------------|
| **Hierarchy Representation** | Represents **part-whole relationships** like trees. |
| **Uniformity** | Treats individual objects and collections **the same way**. |
| **Recursive Behavior** | Calls `showDetails()` recursively on child components. |
| **Flexibility** | Can add new **Files or Folders dynamically**. |

---

## **📌 7. Advantages of the Composite Pattern**
✅ **Simplifies client code** – No need to differentiate between **Files and Folders**.  
✅ **Encapsulation** – Hides tree structure from the client.  
✅ **Extensibility** – Easily **add new types of components**.  
✅ **Supports Open/Closed Principle** – Extend functionality **without modifying existing code**.  

---

## **📌 8. Common Mistakes & How to Avoid Them**
❌ **Forgetting to use a common interface** – Both **Files & Folders must implement the same interface**.  
❌ **Not handling recursion properly** – Ensure `showDetails()` is **called recursively**.  
❌ **Using Inheritance Instead of Composition** – Always use **composition (List of components inside Folder)**.  

---

## **🔥 Hands-On Challenge**
✔ Implement a **Company Hierarchy System** where:
   - **Employee** is a leaf node.
   - **Department** is a composite node that can contain Employees or other Departments.  
✔ Implement a **Menu System** where:
   - **Menu Items (Burger, Fries) are leaf nodes**.
   - **Categories (Fast Food, Desserts) are composite nodes**.

---

## **🚀 Summary**
| **Concept** | **Explanation** |
|------------|----------------|
| **Composite Pattern** | Handles tree-like structures where objects can contain other objects. |
| **Component Interface** | Defines common operations (e.g., `FileSystemComponent`). |
| **Leaf Node** | Represents individual elements (e.g., `File`). |
| **Composite Node** | Represents groups of elements (e.g., `Folder`). |
| **Example** | A **File System where folders contain files and other folders**. |

---
