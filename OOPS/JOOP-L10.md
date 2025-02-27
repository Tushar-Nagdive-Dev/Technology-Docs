### **Level 2: Intermediate - Interface Segregation Principle (ISP)**  

---

## **1. What is Interface Segregation Principle (ISP)?**
**Interface Segregation Principle (ISP)** states:
> *"Clients should not be forced to implement interfaces they don't use."*

### **Key Points:**
- A class should not be forced to implement methods it does not need.
- Interfaces should be **specific and cohesive**, containing only the methods that are relevant to the implementing class.
- Promotes **decoupling** and **high cohesion**.

---

## **2. Why is ISP Important?**
1. **High Cohesion and Low Coupling:**
   - ISP ensures that interfaces are focused on specific tasks, leading to high cohesion.
   - This reduces coupling between classes, making the system more maintainable.

2. **Maintainability and Flexibility:**
   - Changes in one part of the system have minimal impact on other parts.
   - Clients are not burdened with unnecessary dependencies.

3. **Scalability and Extensibility:**
   - Smaller, specific interfaces are easier to extend and maintain.
   - New functionalities can be added without affecting existing classes.

---

## **3. Violation of ISP: Example and Problem Analysis**
### **Scenario: Document Management System**
- You need to support multiple document types like `PDF`, `Word`, and `Excel`.
- Each document type has different capabilities:
  - `PDF`: Can print and view but cannot edit.
  - `Word`: Can view, edit, and print.
  - `Excel`: Can view and edit but does not support printing.

### **Incorrect Design: Fat Interface**
```java
public interface Document {
    void view();
    void edit();
    void print();
}
```

### **Problem:**
- **Fat Interface:** The `Document` interface has methods that are not relevant for all implementing classes.
- **Interface Pollution:** All classes are forced to implement methods they don't need, leading to:
  - `PDF` class implementing `edit()` with an empty body.
  - `Excel` class implementing `print()` with an empty body.
- **Violation of ISP:** Clients are forced to depend on methods they do not use.

---

### **Example: Violating ISP**
```java
public class PDFDocument implements Document {
    @Override
    public void view() {
        System.out.println("Viewing PDF document.");
    }

    @Override
    public void edit() {
        // PDF cannot be edited
        System.out.println("Edit not supported for PDF.");
    }

    @Override
    public void print() {
        System.out.println("Printing PDF document.");
    }
}
```

```java
public class ExcelDocument implements Document {
    @Override
    public void view() {
        System.out.println("Viewing Excel document.");
    }

    @Override
    public void edit() {
        System.out.println("Editing Excel document.");
    }

    @Override
    public void print() {
        // Excel does not support printing
        System.out.println("Print not supported for Excel.");
    }
}
```

### **Usage:**
```java
public class Main {
    public static void main(String[] args) {
        Document pdf = new PDFDocument();
        pdf.view();
        pdf.edit();   // Unsupported but implemented
        pdf.print();

        Document excel = new ExcelDocument();
        excel.view();
        excel.edit();
        excel.print(); // Unsupported but implemented
    }
}
```

### **Output:**
```
Viewing PDF document.
Edit not supported for PDF.
Printing PDF document.
Viewing Excel document.
Editing Excel document.
Print not supported for Excel.
```

### **What's Wrong and Why ISP is Violated?**
- **PDFDocument** and **ExcelDocument** are forced to implement methods they don't need.
- This leads to empty method bodies or misleading messages.
- The design is **rigid** and **hard to maintain**.

---

## **4. Solution: Refactor to Respect ISP**
### **Break Down Fat Interface into Multiple Specific Interfaces**
- Instead of one big interface, create multiple smaller, cohesive interfaces.

### **Step 1: Create Specific Interfaces**
```java
public interface Viewable {
    void view();
}
```

```java
public interface Editable {
    void edit();
}
```

```java
public interface Printable {
    void print();
}
```

### **Step 2: Implement Specific Interfaces in Classes**
```java
public class PDFDocument implements Viewable, Printable {
    @Override
    public void view() {
        System.out.println("Viewing PDF document.");
    }

    @Override
    public void print() {
        System.out.println("Printing PDF document.");
    }
}
```

```java
public class WordDocument implements Viewable, Editable, Printable {
    @Override
    public void view() {
        System.out.println("Viewing Word document.");
    }

    @Override
    public void edit() {
        System.out.println("Editing Word document.");
    }

    @Override
    public void print() {
        System.out.println("Printing Word document.");
    }
}
```

```java
public class ExcelDocument implements Viewable, Editable {
    @Override
    public void view() {
        System.out.println("Viewing Excel document.");
    }

    @Override
    public void edit() {
        System.out.println("Editing Excel document.");
    }
}
```

### **Step 3: Usage with Polymorphism**
```java
public class Main {
    public static void main(String[] args) {
        Viewable pdf = new PDFDocument();
        pdf.view();

        Viewable word = new WordDocument();
        Editable editableWord = (Editable) word;
        Printable printableWord = (Printable) word;
        word.view();
        editableWord.edit();
        printableWord.print();

        Viewable excel = new ExcelDocument();
        Editable editableExcel = (Editable) excel;
        excel.view();
        editableExcel.edit();
    }
}
```

### **Output:**
```
Viewing PDF document.
Viewing Word document.
Editing Word document.
Printing Word document.
Viewing Excel document.
Editing Excel document.
```

---

### **What's Improved and Why ISP is Preserved?**
- **Smaller, specific interfaces**:
  - `Viewable`, `Editable`, and `Printable` are cohesive and focused.
- **Classes implement only relevant interfaces**:
  - `PDFDocument` implements `Viewable` and `Printable` only.
  - `ExcelDocument` implements `Viewable` and `Editable` only.
- **No empty method bodies**:
  - Each class implements only the methods it needs.
- **Better Maintainability and Extensibility**:
  - New functionalities can be added by creating new interfaces without modifying existing classes.

---

## **5. Guidelines for Following ISP**
1. **Keep Interfaces Small and Focused:**
   - Interfaces should be specific and focused on one responsibility.
2. **Avoid Fat Interfaces:**
   - Break down large interfaces into smaller, cohesive ones.
3. **Client-Specific Interfaces:**
   - Design interfaces tailored to client needs, avoiding unnecessary dependencies.
4. **Use Composition over Inheritance:**
   - Prefer composition to avoid forcing subclasses to implement unnecessary methods.

---

## **6. Common Mistakes to Avoid:**
1. **Fat Interfaces:**
   - Avoid designing interfaces with too many unrelated methods.
2. **Overusing Marker Interfaces:**
   - Marker interfaces (without methods) should be used sparingly.
3. **Incorrect Client Requirements:**
   - Design interfaces based on client requirements, not implementation convenience.

---

## **7. Exercise: Mastering ISP**
### **Task: Create an `Appliance` System**
1. **Interfaces:**
   - `Switchable`: `turnOn()` and `turnOff()`.
   - `RemoteControllable`: `connectToRemote()` and `disconnectFromRemote()`.
   - `Adjustable`: `increaseLevel()` and `decreaseLevel()`.
2. **Classes:**
   - `Fan`: Implements `Switchable` and `Adjustable`.
   - `Light`: Implements `Switchable` and `RemoteControllable`.
   - `SmartTV`: Implements `Switchable`, `RemoteControllable`, and `Adjustable`.
3. **Requirements:**
   - Use **ISP** by creating small, specific interfaces.
   - Test the implementation using polymorphism.

---

## **8. What's Next?**
We’ve now mastered:
- **Interface Segregation Principle (ISP)**:
  - Promotes small, specific, and cohesive interfaces.
  - Avoids forcing classes to implement unused methods.

### Next Up: **Dependency Inversion Principle (DIP)**
- High-level modules should not depend on low-level modules.
- Implementing DIP using Dependency Injection and Inversion of Control.

---

Here's a Java implementation of the Appliance system adhering to the **Interface Segregation Principle (ISP)** by using small, specific interfaces, followed by polymorphic testing:

```java
// Interface: Switchable (for basic on/off functionality)
interface Switchable {
    void turnOn();
    void turnOff();
}

// Interface: RemoteControllable (for remote control functionality)
interface RemoteControllable {
    void connectToRemote();
    void disconnectFromRemote();
}

// Interface: Adjustable (for level adjustment functionality)
interface Adjustable {
    void increaseLevel();
    void decreaseLevel();
}

// Class: Fan (Implements Switchable and Adjustable)
class Fan implements Switchable, Adjustable {
    private boolean isOn;
    private int speed; // 0 to 5

    public Fan() {
        this.isOn = false;
        this.speed = 0;
    }

    @Override
    public void turnOn() {
        if (!isOn) {
            isOn = true;
            speed = 1; // Start at minimum speed
            System.out.println("Fan is turned ON. Speed: " + speed);
        } else {
            System.out.println("Fan is already ON.");
        }
    }

    @Override
    public void turnOff() {
        if (isOn) {
            isOn = false;
            speed = 0;
            System.out.println("Fan is turned OFF.");
        } else {
            System.out.println("Fan is already OFF.");
        }
    }

    @Override
    public void increaseLevel() {
        if (isOn && speed < 5) {
            speed++;
            System.out.println("Fan speed increased to " + speed);
        } else if (!isOn) {
            System.out.println("Cannot adjust speed: Fan is OFF.");
        } else {
            System.out.println("Fan is already at maximum speed.");
        }
    }

    @Override
    public void decreaseLevel() {
        if (isOn && speed > 1) {
            speed--;
            System.out.println("Fan speed decreased to " + speed);
        } else if (!isOn) {
            System.out.println("Cannot adjust speed: Fan is OFF.");
        } else {
            System.out.println("Fan is already at minimum speed.");
        }
    }
}

// Class: Light (Implements Switchable and RemoteControllable)
class Light implements Switchable, RemoteControllable {
    private boolean isOn;
    private boolean isRemoteConnected;

    public Light() {
        this.isOn = false;
        this.isRemoteConnected = false;
    }

    @Override
    public void turnOn() {
        isOn = true;
        System.out.println("Light is turned ON.");
    }

    @Override
    public void turnOff() {
        isOn = false;
        System.out.println("Light is turned OFF.");
    }

    @Override
    public void connectToRemote() {
        if (!isRemoteConnected) {
            isRemoteConnected = true;
            System.out.println("Light is connected to remote.");
        } else {
            System.out.println("Light is already connected to remote.");
        }
    }

    @Override
    public void disconnectFromRemote() {
        if (isRemoteConnected) {
            isRemoteConnected = false;
            System.out.println("Light is disconnected from remote.");
        } else {
            System.out.println("Light is not connected to remote.");
        }
    }
}

// Class: SmartTV (Implements Switchable, RemoteControllable, and Adjustable)
class SmartTV implements Switchable, RemoteControllable, Adjustable {
    private boolean isOn;
    private boolean isRemoteConnected;
    private int volume; // 0 to 50

    public SmartTV() {
        this.isOn = false;
        this.isRemoteConnected = false;
        this.volume = 0;
    }

    @Override
    public void turnOn() {
        if (!isOn) {
            isOn = true;
            volume = 10; // Default volume
            System.out.println("SmartTV is turned ON. Volume: " + volume);
        } else {
            System.out.println("SmartTV is already ON.");
        }
    }

    @Override
    public void turnOff() {
        if (isOn) {
            isOn = false;
            volume = 0;
            System.out.println("SmartTV is turned OFF.");
        } else {
            System.out.println("SmartTV is already OFF.");
        }
    }

    @Override
    public void connectToRemote() {
        if (!isRemoteConnected) {
            isRemoteConnected = true;
            System.out.println("SmartTV is connected to remote.");
        } else {
            System.out.println("SmartTV is already connected to remote.");
        }
    }

    @Override
    public void disconnectFromRemote() {
        if (isRemoteConnected) {
            isRemoteConnected = false;
            System.out.println("SmartTV is disconnected from remote.");
        } else {
            System.out.println("SmartTV is not connected to remote.");
        }
    }

    @Override
    public void increaseLevel() {
        if (isOn && volume < 50) {
            volume += 5;
            System.out.println("SmartTV volume increased to " + volume);
        } else if (!isOn) {
            System.out.println("Cannot adjust volume: SmartTV is OFF.");
        } else {
            System.out.println("SmartTV is already at maximum volume.");
        }
    }

    @Override
    public void decreaseLevel() {
        if (isOn && volume > 0) {
            volume -= 5;
            System.out.println("SmartTV volume decreased to " + volume);
        } else if (!isOn) {
            System.out.println("Cannot adjust volume: SmartTV is OFF.");
        } else {
            System.out.println("SmartTV is already at minimum volume.");
        }
    }
}

// Test Class
class Main {
    public static void main(String[] args) {
        // Create appliance objects
        Fan fan = new Fan();
        Light light = new Light();
        SmartTV smartTV = new SmartTV();

        // Store in Switchable array for polymorphic testing
        Switchable[] switchables = {fan, light, smartTV};

        System.out.println("Testing Switchable Behavior:");
        for (Switchable device : switchables) {
            device.turnOn();
            device.turnOff();
            System.out.println();
        }

        // Test Adjustable appliances
        Adjustable[] adjustables = {fan, smartTV};

        System.out.println("Testing Adjustable Behavior:");
        for (Adjustable device : adjustables) {
            if (device instanceof Fan) ((Fan) device).turnOn();
            if (device instanceof SmartTV) ((SmartTV) device).turnOn();
            device.increaseLevel();
            device.increaseLevel();
            device.decreaseLevel();
            System.out.println();
        }

        // Test RemoteControllable appliances
        RemoteControllable[] remotes = {light, smartTV};

        System.out.println("Testing RemoteControllable Behavior:");
        for (RemoteControllable device : remotes) {
            device.connectToRemote();
            device.disconnectFromRemote();
            System.out.println();
        }
    }
}
```

### Explanation:

1. **Interfaces (ISP)**:
   - **`Switchable`**: Defines `turnOn()` and `turnOff()` for basic power control.
   - **`RemoteControllable`**: Defines `connectToRemote()` and `disconnectFromRemote()` for remote functionality.
   - **`Adjustable`**: Defines `increaseLevel()` and `decreaseLevel()` for adjusting settings (e.g., speed or volume).
   - Each interface is small and specific, ensuring classes only implement what they need (ISP).

2. **Classes**:
   - **`Fan`**: Implements `Switchable` (on/off) and `Adjustable` (speed control, 0-5).
   - **`Light`**: Implements `Switchable` (on/off) and `RemoteControllable` (remote connection).
   - **`SmartTV`**: Implements all three: `Switchable` (on/off), `RemoteControllable` (remote), and `Adjustable` (volume, 0-50).

3. **Polymorphism**:
   - Arrays of interface types (`Switchable[]`, `Adjustable[]`, `RemoteControllable[]`) hold objects of different classes.
   - Methods are called polymorphically, with the correct implementation executed based on the actual object type.

4. **Testing**:
   - `Switchable` test: Turns on and off all appliances.
   - `Adjustable` test: Adjusts levels for `Fan` and `SmartTV` after turning them on.
   - `RemoteControllable` test: Connects and disconnects `Light` and `SmartTV` from remote.

### Sample Output:
```
Testing Switchable Behavior:
Fan is turned ON. Speed: 1
Fan is turned OFF.

Light is turned ON.
Light is turned OFF.

SmartTV is turned ON. Volume: 10
SmartTV is turned OFF.

Testing Adjustable Behavior:
Fan is turned ON. Speed: 1
Fan speed increased to 2
Fan speed increased to 3
Fan speed decreased to 2

SmartTV is turned ON. Volume: 10
SmartTV volume increased to 15
SmartTV volume increased to 20
SmartTV volume decreased to 15

Testing RemoteControllable Behavior:
Light is connected to remote.
Light is disconnected from remote.

SmartTV is connected to remote.
SmartTV is disconnected from remote.
```

### Key Points:
- **ISP**: Interfaces are segregated so classes like `Fan` don’t need to implement `RemoteControllable`, and `Light` doesn’t need `Adjustable`.
- **Polymorphism**: Demonstrated by treating objects through their interface types in arrays and loops.
- **Behavior**: Each class implements only the interfaces relevant to its functionality, with realistic state management (e.g., speed, volume, on/off status).
