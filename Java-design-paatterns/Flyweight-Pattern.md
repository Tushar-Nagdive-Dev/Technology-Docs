# **🚀 Lesson 11: Flyweight Pattern – Efficient Memory Optimization**

The **Flyweight Pattern** is a **structural design pattern** that minimizes memory usage by **sharing common object data instead of creating new instances**.

---

## **📌 1. What is the Flyweight Pattern?**
The **Flyweight Pattern**:
✔ **Reduces memory consumption** by sharing objects.  
✔ Stores **intrinsic (shared) state** inside flyweight objects.  
✔ Keeps **extrinsic (unique) state** outside of flyweights.  
✔ Uses a **Factory to manage object reuse**.  

---

## **📌 2. When to Use the Flyweight Pattern?**
✔ When a **large number of similar objects** need to be created.  
✔ When memory optimization is **crucial**.  
✔ When objects have **shared (intrinsic) state** and **unique (extrinsic) state**.  
✔ When following the **Single Responsibility Principle** (object storage is separate from its use).  

---

## **📌 3. Real-World Analogy – Characters in a Text Editor 📝**
- A **text editor** manages **thousands of characters**.
- Each **character (`A`, `B`, `C`) is stored only once** and reused **for all occurrences**.
- Only **position and formatting** (extrinsic state) **change**.

---

## **📌 4. Implementing Flyweight Pattern in Java**
Let’s build a **Character Formatting System** using the **Flyweight Pattern**.

---

### **Step 1: Create the Flyweight Interface**
This defines **common behavior** for shared objects.

```java
// Flyweight Interface
public interface Character {
    void display(int size, String color);
}
```

---

### **Step 2: Implement the Concrete Flyweight Class**
This class **stores shared character data**.

```java
// Concrete Flyweight (Shared Characters)
public class ConcreteCharacter implements Character {
    private final char symbol; // Intrinsic (shared) state

    public ConcreteCharacter(char symbol) {
        this.symbol = symbol;
    }

    @Override
    public void display(int size, String color) {
        System.out.println("Character: " + symbol + " | Size: " + size + " | Color: " + color);
    }
}
```
✔ Stores the **symbol (`A`, `B`, `C`, etc.)** only **once**.  
✔ **Size and color** are **not stored** (extrinsic state).  

---

### **Step 3: Create the Flyweight Factory**
The **Factory ensures** that characters are **reused** instead of creating new instances.

```java
import java.util.HashMap;
import java.util.Map;

// Flyweight Factory (Manages shared Character objects)
public class CharacterFactory {
    private static final Map<Character, Character> characterPool = new HashMap<>();

    public static Character getCharacter(char symbol) {
        characterPool.putIfAbsent(symbol, new ConcreteCharacter(symbol));
        return characterPool.get(symbol);
    }

    public static int getTotalCharactersCreated() {
        return characterPool.size();
    }
}
```
✔ **Creates and stores only one instance per character**.  
✔ Uses a **HashMap** to track shared objects.  

---

### **Step 4: Using the Flyweight Pattern**
```java
public class Main {
    public static void main(String[] args) {
        Character c1 = CharacterFactory.getCharacter('A');
        c1.display(12, "Red");

        Character c2 = CharacterFactory.getCharacter('B');
        c2.display(14, "Blue");

        Character c3 = CharacterFactory.getCharacter('A'); // Reuses existing 'A'
        c3.display(16, "Green");

        System.out.println("Total Character Objects Created: " + CharacterFactory.getTotalCharactersCreated());
    }
}
```

---

## **📌 5. Expected Output**
```
Character: A | Size: 12 | Color: Red
Character: B | Size: 14 | Color: Blue
Character: A | Size: 16 | Color: Green
Total Character Objects Created: 2
```
✔ **Only two objects were created (`A` and `B`), even though `A` was used twice**.  
✔ **Memory is optimized by sharing objects**.  

---

## **📌 6. Intrinsic vs. Extrinsic State**
| **Type** | **Stored in Flyweight?** | **Example** |
|---------|------------------|------------|
| **Intrinsic (Shared State)** | ✅ Yes | Character Symbol (`A`, `B`, `C`) |
| **Extrinsic (Unique State)** | ❌ No | Size, Color, Position |

---

## **📌 7. Key Features of the Flyweight Pattern**
| **Feature** | **Explanation** |
|------------|----------------|
| **Reduces Memory Usage** | Shares objects instead of creating duplicates. |
| **Factory Controls Object Creation** | Ensures only **one instance per character** is created. |
| **Separates Shared & Unique State** | Shared state is inside flyweights; unique state is passed externally. |

---

## **📌 8. Common Mistakes & How to Avoid Them**
❌ **Storing extrinsic state inside Flyweights** – Always pass unique data from outside.  
❌ **Not using a Factory** – Always manage flyweights using a **centralized factory**.  
❌ **Applying it when unnecessary** – Use Flyweight **only when many similar objects exist**.  

---

## **🔥 Hands-On Challenge**
✔ Implement a **Tree Rendering System** where similar **tree objects (Pine, Oak)** share common data.  
✔ Implement a **Car Model Factory** where similar **car models** share data but have unique registration numbers.  

---

## **🚀 Summary**
| **Concept** | **Explanation** |
|------------|----------------|
| **Flyweight Pattern** | Shares objects to **reduce memory usage**. |
| **Flyweight Object** | Stores **shared intrinsic state** (e.g., `Character: A`). |
| **Extrinsic State** | Stored outside Flyweight (`size`, `color`, `position`). |
| **Example** | **Text editor characters with shared symbols**. |

---
