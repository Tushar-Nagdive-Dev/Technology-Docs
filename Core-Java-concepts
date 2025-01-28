**1. Explain the difference between `==` and `.equals()` in Java.**

* **`==`:** 
    * Compares references (memory addresses) of objects. 
    * For primitive data types, it compares values directly.
    * Returns `true` only if both operands refer to the same object in memory.

* **`.equals()`:** 
    * Compares the content (values) of objects. 
    * By default, it inherits the behavior of `==` from the `Object` class, comparing references.
    * Most classes override this method to compare the actual values of the objects.

**Example:**

```java
String s1 = "hello";
String s2 = "hello"; 
String s3 = new String("hello");

System.out.println(s1 == s2); // Output: true (String pool optimization)
System.out.println(s1 == s3); // Output: false (Different objects in memory)
System.out.println(s1.equals(s2)); // Output: true (Same content)
System.out.println(s1.equals(s3)); // Output: true (Same content)
```

**2. What is the difference between `ArrayList` and `LinkedList` in Java?**

* **ArrayList:**
    * Uses an array to store elements.
    * Provides fast random access (get/set by index) in O(1) time.
    * Slow insertion/deletion operations, especially in the middle of the list, due to potential array shifting.

* **LinkedList:**
    * Uses a doubly linked list to store elements.
    * Efficient insertion/deletion operations at any position in O(1) time.
    * Slow random access (get/set by index) in O(n) time.

**3. Explain the concept of garbage collection in Java.**

* Java has an automatic garbage collector that reclaims memory occupied by objects that are no longer referenced by any part of the program.
* The garbage collector periodically checks for objects that are unreachable and frees up the memory they occupy.
* Developers generally do not have direct control over when garbage collection occurs.

**4. What is the purpose of the `finally` block in Java `try-catch` statements?**

* The `finally` block is always executed, regardless of whether an exception occurs or not.
* It's used to release resources (like file handles, database connections) that must be cleaned up, even if an exception happens.

**5. Explain the concept of polymorphism in Java.**

* Polymorphism allows objects of different classes to be treated as objects of a common type.
* It's achieved through:
    * **Method Overloading:** Defining multiple methods with the same name but different parameters within the same class.
    * **Method Overriding:** Defining a method in a subclass that has the same name, parameters, and return type as a method in its superclass.

**6. What is the difference between an interface and an abstract class in Java?**

* **Interface:**
    * Can only contain abstract methods (methods without implementation) and constant fields.
    * A class can implement multiple interfaces.
    * Represents a contract that a class must adhere to.

* **Abstract Class:**
    * Can contain both abstract and concrete methods.
    * A class can extend only one abstract class.
    * Provides a common base for related classes.

**7. Explain the concept of generics in Java.**

* Generics allow you to create classes and methods that work with different types of objects without compromising type safety.
* They improve code reusability and reduce the risk of type-related errors.

**Example:**

```java
public class Box<T> {
    private T object;

    public void set(T object) { 
        this.object = object;
    }

    public T get() {
        return object;
    }
}
```

**8. What is the purpose of the `volatile` keyword in Java?**

* The `volatile` keyword ensures that a variable is always read from and written to main memory, rather than being cached in a thread's local memory.
* This is crucial for proper synchronization between multiple threads that access the same variable.

**9. Explain the concept of Java Streams.**

* Java Streams provide a declarative and functional way to process collections of data.
* They allow you to perform operations like filtering, mapping, sorting, and reducing data in a concise and efficient manner.

**10. What is the difference between a `HashMap` and a `HashSet` in Java?**

* **HashMap:**
    * Stores data as key-value pairs.
    * Keys must be unique.
    * Allows you to retrieve values based on their keys.

* **HashSet:**
    * Stores only unique elements.
    * Does not maintain any order of elements.
    * Can be used to efficiently check for the presence of an element within a collection.

Certainly, let's delve deeper into some of the core Java concepts with more intricate examples and explanations.

**11. Explain the concept of Serialization in Java.**

* **Serialization** is the process of converting an object's state (its data) into a byte stream. 
* This byte stream can then be stored in a file, transmitted over a network, or stored in a database. 
* The reverse process of converting the byte stream back into an object is called **Deserialization**.
* **Use Cases:**
    * **Persistence:** Storing objects to files or databases for later retrieval.
    * **Network Communication:** Transmitting objects over a network between different systems.
* **Implementation:**
    * Implement the `Serializable` interface in the class you want to serialize.
    * Use `ObjectOutputStream` to serialize the object and `ObjectInputStream` to deserialize it.

**Example:**

```java
import java.io.*;

class Employee implements Serializable {
    String name;
    int id;

    // ... Constructor, getters, setters ...

    public static void main(String[] args) throws IOException {
        Employee emp = new Employee("John Doe", 123);

        // Serialization
        FileOutputStream fos = new FileOutputStream("employee.ser");
        ObjectOutputStream oos = new ObjectOutputStream(fos);
        oos.writeObject(emp);
        oos.close();
        fos.close();

        // Deserialization
        FileInputStream fis = new FileInputStream("employee.ser");
        ObjectInputStream ois = new ObjectInputStream(fis);
        Employee emp2 = (Employee) ois.readObject();
        ois.close();
        fis.close();

        System.out.println("Deserialized Employee: " + emp2.name + ", " + emp2.id); 
    }
}
```

**12. What is the difference between `Checked` and `Unchecked` exceptions in Java?**

* **Checked Exceptions:**
    * Exceptions that are known at compile time.
    * Examples: `IOException`, `SQLException`, `ClassNotFoundException`.
    * Must be handled explicitly (using `try-catch` blocks or by declaring them in the method signature using `throws`).

* **Unchecked Exceptions:**
    * Exceptions that occur at runtime.
    * Examples: `NullPointerException`, `ArrayIndexOutOfBoundsException`, `ArithmeticException`.
    * Not required to be handled explicitly, but it's generally good practice to handle them to prevent unexpected program termination.

**13. Explain the concept of Java Reflection.**

* **Reflection** allows you to inspect and manipulate classes, objects, and their members (methods, fields, constructors) at runtime.
* You can:
    * Get information about a class (its name, interfaces, methods, etc.).
    * Create instances of objects dynamically.
    * Invoke methods on objects dynamically.
    * Access and modify fields of objects.

**Example:**

```java
import java.lang.reflect.*;

public class ReflectionExample {
    public static void main(String[] args) throws Exception {
        Class<?> cls = Class.forName("java.lang.String"); 
        Method[] methods = cls.getMethods();

        for (Method method : methods) {
            System.out.println(method.getName());
        }
    }
}
```

**14. What is the purpose of the `synchronized` keyword in Java?**

* The `synchronized` keyword is used to control access to a shared resource (like a variable or an object) by multiple threads.
* It ensures that only one thread can access the synchronized block or method at a time, preventing race conditions and data corruption.

**Example:**

```java
public class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++; 
    }
}
```

**15. Explain the use of `final` keyword in Java.**

* **For variables:**
    * Declares a constant whose value cannot be changed after initialization.
* **For methods:**
    * Prevents subclassing from overriding the method.
* **For classes:**
    * Prevents the class from being subclassed.

**16. What are Java Annotations?**

* Annotations are meta-data that can be added to Java source code to provide additional information about the code.
* They are used by compilers, build tools, and other programs to process and understand the code better.
* Examples: `@Override`, `@Deprecated`, `@SuppressWarnings`.

**17. Explain the concept of Java Memory Model.**

* The Java Memory Model defines how threads interact with the main memory and their own local memory.
* It specifies rules for how variables are accessed and updated by different threads.
* Understanding the Java Memory Model is crucial for writing correct and thread-safe code.

Here are some advanced and tricky **Core Java interview questions**, along with their answers, for individuals aiming to demonstrate expertise in Java during an interview:

---

### **1. What is the difference between `String`, `StringBuilder`, and `StringBuffer`?**
#### **Answer:**
- `String`: Immutable object; once created, its value cannot be changed. Every modification creates a new object, leading to higher memory usage.
- `StringBuilder`: Mutable object; allows modifications without creating a new object. Faster than `String` as it doesn’t synchronize methods.
- `StringBuffer`: Mutable object; thread-safe because its methods are synchronized. Slightly slower than `StringBuilder`.

---

### **2. How does `HashMap` work internally in Java?**
#### **Answer:**
- **Structure**: `HashMap` is an array of linked lists (or trees in case of Java 8+).
- **Hashing**: It uses a hash function to compute the hash code for a key, which determines the bucket index in the underlying array.
- **Collision Resolution**: When two keys hash to the same bucket, it uses a linked list or a binary tree (in Java 8+ if the bucket size exceeds 8).
- **Load Factor**: Determines when to resize the map. Default is 0.75 (75% full).

---

### **3. Explain the concept of `volatile` keyword.**
#### **Answer:**
- **Purpose**: Ensures visibility of changes to variables across threads.
- **Behavior**: 
  - Prevents threads from caching the value of the variable; all reads/writes go directly to main memory.
  - Does not guarantee atomicity.
- Example:
  ```java
  private volatile boolean flag = true;
  ```

---

### **4. How does the `synchronized` keyword differ from `Lock`?**
#### **Answer:**
- `synchronized`:
  - Implicit locking mechanism.
  - Cannot attempt try-locking or timed locking.
  - Automatically releases the lock.
- `Lock`:
  - Explicit and flexible.
  - Can try to acquire a lock (`tryLock()`), which prevents deadlocks.
  - Requires manual unlocking using `unlock()`.

---

### **5. What is the difference between `Checked` and `Unchecked` exceptions?**
#### **Answer:**
- **Checked Exceptions**:
  - Subclass of `Exception`, excluding `RuntimeException`.
  - Must be handled using `try-catch` or declared in the `throws` clause.
  - Example: `IOException`.
- **Unchecked Exceptions**:
  - Subclass of `RuntimeException`.
  - Does not require handling.
  - Example: `NullPointerException`.

---

### **6. How does the Java Garbage Collector (GC) work?**
#### **Answer:**
- **Purpose**: Reclaims memory by removing unreachable objects.
- **Techniques**:
  - **Mark-and-Sweep**: Marks objects reachable from the root set and removes unmarked objects.
  - **Generational GC**: Divides memory into Young, Old, and Permanent generations.
    - Young Generation: Minor GC for short-lived objects.
    - Old Generation: Major GC for long-lived objects.
    - Permanent Generation (Java 7 and below): Stores metadata.

---

### **7. What are the differences between `wait()`, `sleep()`, and `join()`?**
#### **Answer:**
- `wait()`: Causes the current thread to wait until it is notified. Must be called inside a `synchronized` block.
- `sleep()`: Pauses the thread for a specific time but does not release the lock.
- `join()`: Waits for another thread to finish its execution.

---

### **8. Explain how `ConcurrentHashMap` achieves thread safety.**
#### **Answer:**
- Divides the map into segments to allow multiple threads to access it concurrently.
- Uses locks only at the segment level instead of the entire map.
- In Java 8+, it uses a combination of CAS (Compare-And-Swap), synchronized blocks, and a tree structure for better performance.

---

### **9. What is the `transient` keyword?**
#### **Answer:**
- **Purpose**: Marks a variable to be excluded from serialization.
- **Usage**: When an object is serialized, transient variables are not included in the serialized state.
  ```java
  private transient int sessionID;
  ```

---

### **10. What are functional interfaces? Name a few.**
#### **Answer:**
- **Definition**: An interface with exactly one abstract method.
- **Purpose**: Supports lambda expressions.
- **Examples**:
  - `Runnable` (method: `void run()`)
  - `Callable` (method: `V call()`)
  - `Predicate` (method: `boolean test(T t)`)
  - `Function` (method: `R apply(T t)`)

---

### **11. Explain the difference between `final`, `finally`, and `finalize()`.**
#### **Answer:**
- `final`: Used for declaring constants, immutable variables, or methods that cannot be overridden.
- `finally`: A block in `try-catch` used for cleanup tasks. Always executes.
- `finalize()`: A method invoked by the Garbage Collector before reclaiming memory. Deprecated in Java 9+.

---

### **12. How does Java handle multiple inheritance?**
#### **Answer:**
- **Classes**: Java does not allow multiple inheritance using classes to avoid ambiguity (Diamond Problem).
- **Interfaces**: Allows multiple inheritance using interfaces, as they contain abstract methods by default.
