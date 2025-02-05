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

### **🚀 Flyweight Pattern: Car Model Factory Implementation**
  
We will build a **Car Model Factory** where:  
✅ **Car Models** (`Tesla Model S`, `Toyota Corolla`) are **shared** objects.  
✅ **Registration Numbers** are **unique for each car instance**.  
✅ The **Flyweight Pattern** will ensure we don’t create duplicate `CarModel` objects.  

---

## **📌 Step 1: Create the Flyweight Interface**
This interface represents the **common behavior** for shared car models.

```java
// Flyweight Interface (Defines shared Car Model properties)
public interface CarModel {
    void displayCar(String registrationNumber);
}
```

---

## **📌 Step 2: Implement the Concrete Flyweight Class**
This class **stores shared car model data**.

```java
// Concrete Flyweight (Shared Car Model)
public class ConcreteCarModel implements CarModel {
    private final String brand;
    private final String model;
    private final String engineType;
    private final String color;

    public ConcreteCarModel(String brand, String model, String engineType, String color) {
        this.brand = brand;
        this.model = model;
        this.engineType = engineType;
        this.color = color;
    }

    @Override
    public void displayCar(String registrationNumber) {
        System.out.println("Car: " + brand + " " + model +
                " | Engine: " + engineType +
                " | Color: " + color +
                " | Registration: " + registrationNumber);
    }
}
```
✔ Stores **brand, model, engine type, and color** (**intrinsic state**).  
✔ **Registration number is passed dynamically** (**extrinsic state**).  

---

## **📌 Step 3: Create the Flyweight Factory**
The **Factory ensures** that car models are **reused** instead of creating new instances.

```java
import java.util.HashMap;
import java.util.Map;

// Flyweight Factory (Manages shared CarModel objects)
public class CarModelFactory {
    private static final Map<String, CarModel> carModelPool = new HashMap<>();

    public static CarModel getCarModel(String brand, String model, String engineType, String color) {
        String key = brand + "-" + model + "-" + engineType + "-" + color;

        carModelPool.putIfAbsent(key, new ConcreteCarModel(brand, model, engineType, color));
        return carModelPool.get(key);
    }

    public static int getTotalCarModelsCreated() {
        return carModelPool.size();
    }
}
```
✔ Uses a **HashMap** to track shared car models.  
✔ Ensures **only one instance per unique model** is created.  

---

## **📌 Step 4: Using the Flyweight Pattern**
```java
public class Main {
    public static void main(String[] args) {
        // Shared Car Models
        CarModel model1 = CarModelFactory.getCarModel("Tesla", "Model S", "Electric", "Red");
        CarModel model2 = CarModelFactory.getCarModel("Toyota", "Corolla", "Petrol", "White");

        // Unique Cars with Different Registration Numbers
        model1.displayCar("TS-1234");
        model1.displayCar("TS-5678");

        model2.displayCar("TO-9876");
        model2.displayCar("TO-4321");

        // Checking Memory Optimization
        System.out.println("Total Unique Car Models Created: " + CarModelFactory.getTotalCarModelsCreated());
    }
}
```

---

## **📌 Expected Output**
```
Car: Tesla Model S | Engine: Electric | Color: Red | Registration: TS-1234
Car: Tesla Model S | Engine: Electric | Color: Red | Registration: TS-5678
Car: Toyota Corolla | Engine: Petrol | Color: White | Registration: TO-9876
Car: Toyota Corolla | Engine: Petrol | Color: White | Registration: TO-4321
Total Unique Car Models Created: 2
```
✔ **Only two unique car models were created** (`Tesla Model S` & `Toyota Corolla`).  
✔ **Registration numbers remain unique for each car instance**.  
✔ **Memory optimization achieved by reusing car models**.  

---

## **📌 Flyweight Pattern Breakdown**
| **Type** | **Stored in Flyweight?** | **Example** |
|---------|------------------|------------|
| **Intrinsic (Shared State)** | ✅ Yes | Car Brand, Model, Engine Type, Color |
| **Extrinsic (Unique State)** | ❌ No | Registration Number |

---

## **📌 Advantages of Using Flyweight in Car Factory**
✅ **Memory-efficient** – Only **one object per car model is stored**.  
✅ **Fast object creation** – Instead of creating new objects, we **reuse existing ones**.  
✅ **Supports high scalability** – Ideal for **large-scale car rental, manufacturing, or inventory systems**.  

---

## **📌 Common Mistakes & How to Avoid Them**
❌ **Storing unique data inside Flyweights** – Always pass unique state externally.  
❌ **Not using a Factory** – Use a **centralized factory to manage flyweight objects**.  
❌ **Applying Flyweight in small-scale systems** – Use only when **many similar objects exist**.  

---

## **🔥 Hands-On Challenge**
✔ Modify the **Car Factory** to support **different car dealerships** while still reusing car models.  
✔ Implement a **Tree Rendering System** where similar **tree types** share model data but have unique coordinates.  

---

## **🚀 Summary**
| **Concept** | **Explanation** |
|------------|----------------|
| **Flyweight Pattern** | Shares objects to **reduce memory usage**. |
| **Flyweight Object** | Stores **shared intrinsic state** (Car Model Data). |
| **Extrinsic State** | Stored outside Flyweight (Registration Number). |
| **Example** | **Car Factory where models are shared, but registration is unique**. |

---
