# **🚀 Lesson 16: Command Pattern – Encapsulating Requests as Objects**

The **Command Pattern** is a **behavioral design pattern** that encapsulates **a request as an object**, allowing clients to parameterize methods, queue requests, and support undo operations.

---

## **📌 1. What is the Command Pattern?**
The **Command Pattern**:
✔ **Encapsulates a request as an object**.  
✔ **Decouples sender (Invoker) from receiver (Executor)**.  
✔ **Supports undo and redo operations**.  
✔ **Allows command queuing and logging**.  

---

## **📌 2. When to Use the Command Pattern?**
✔ When you need to **queue requests** and execute them later.  
✔ When implementing **undo/redo operations** (e.g., text editors, transactions).  
✔ When following the **Open/Closed Principle** (new commands can be added without modifying existing code).  
✔ When implementing **GUI buttons, remote controls, or job scheduling systems**.  

---

## **📌 3. Real-World Analogy – Remote Control for Home Devices 🎛️**
- A **TV Remote** has buttons for **ON, OFF, Volume Up, Volume Down**.  
- Each button **sends a command to the TV**.  
- The **Remote (Invoker) does not know the TV’s internal logic**; it just sends commands.  
- The **TV (Receiver) executes the command** when received.

---

## **📌 4. Implementing Command Pattern in Java**
Let’s build a **Remote Control System** where buttons send **commands to a TV**.

---

### **Step 1: Create the Command Interface**
This interface defines the `execute()` method for all commands.

```java
// Command Interface
public interface Command {
    void execute();
}
```

---

### **Step 2: Create the Receiver (TV)**
The **TV is the actual device that executes commands**.

```java
// Receiver (TV)
public class TV {
    public void turnOn() {
        System.out.println("TV is turned ON");
    }

    public void turnOff() {
        System.out.println("TV is turned OFF");
    }
}
```
✔ **Defines real actions** (`turnOn()`, `turnOff()`).  
✔ **Commands will call these methods**.  

---

### **Step 3: Implement Concrete Commands**
Each command **encapsulates an action** and calls the receiver.

#### **Turn ON Command**
```java
// Concrete Command 1: Turns TV ON
public class TurnOnCommand implements Command {
    private TV tv;

    public TurnOnCommand(TV tv) {
        this.tv = tv;
    }

    @Override
    public void execute() {
        tv.turnOn();
    }
}
```

#### **Turn OFF Command**
```java
// Concrete Command 2: Turns TV OFF
public class TurnOffCommand implements Command {
    private TV tv;

    public TurnOffCommand(TV tv) {
        this.tv = tv;
    }

    @Override
    public void execute() {
        tv.turnOff();
    }
}
```
✔ **Each command has a reference to the TV** (Receiver).  
✔ **Calls appropriate TV methods** when executed.  

---

### **Step 4: Create the Invoker (Remote Control)**
The **Remote Control sends commands** when buttons are pressed.

```java
// Invoker (Remote Control)
public class RemoteControl {
    private Command command;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void pressButton() {
        if (command != null) {
            command.execute();
        }
    }
}
```
✔ **Holds a reference to a `Command`**.  
✔ **Executes the assigned command when a button is pressed**.  

---

### **Step 5: Using the Command Pattern**
```java
public class Main {
    public static void main(String[] args) {
        TV tv = new TV(); // Receiver

        // Create Commands
        Command turnOn = new TurnOnCommand(tv);
        Command turnOff = new TurnOffCommand(tv);

        // Remote Control
        RemoteControl remote = new RemoteControl();

        // Press ON button
        remote.setCommand(turnOn);
        remote.pressButton();

        // Press OFF button
        remote.setCommand(turnOff);
        remote.pressButton();
    }
}
```

---

## **📌 5. Expected Output**
```
TV is turned ON
TV is turned OFF
```
✔ **The Remote (Invoker) triggers the commands dynamically**.  
✔ **The TV (Receiver) executes commands without knowing the Remote’s details**.  

---

## **📌 6. Key Features of the Command Pattern**
| **Feature** | **Explanation** |
|------------|----------------|
| **Encapsulates Requests** | Requests are stored as objects (Commands). |
| **Decouples Sender & Receiver** | The Invoker (Remote) does not know the Receiver (TV) internals. |
| **Supports Undo/Redo** | Commands can store history for undo/redo operations. |
| **Allows Command Queuing** | Commands can be stored and executed later. |

---

## **📌 7. Adding Undo Support**
We modify `Command` to include `undo()`:

```java
// Extended Command Interface with Undo
public interface Command {
    void execute();
    void undo();
}
```

Then, update **TurnOnCommand** and **TurnOffCommand**:

#### **Turn ON Command with Undo**
```java
public class TurnOnCommand implements Command {
    private TV tv;

    public TurnOnCommand(TV tv) {
        this.tv = tv;
    }

    @Override
    public void execute() {
        tv.turnOn();
    }

    @Override
    public void undo() {
        tv.turnOff();
    }
}
```

✔ `undo()` reverses the `execute()` action.  
✔ **Now we can implement an undoable remote control!**  

---

## **📌 8. Common Mistakes & How to Avoid Them**
❌ **Directly calling receiver methods** – Always **use command objects** instead.  
❌ **Not supporting undo when needed** – Include `undo()` if undo functionality is required.  
❌ **Tightly coupling the invoker with the receiver** – Always **use interfaces** to keep them separate.  

---

## **🔥 Hands-On Challenge**
✔ Implement a **Text Editor Undo System**, where `WriteTextCommand` and `DeleteTextCommand` can be undone.  
✔ Implement a **Job Scheduling System** where tasks (`BackupTask`, `EmailTask`) are stored in a queue and executed later.  

---

## **🚀 Summary**
| **Concept** | **Explanation** |
|------------|----------------|
| **Command Pattern** | Encapsulates requests as objects and allows execution later. |
| **Command Interface** | Defines common methods (`execute()`, `undo()`). |
| **Receiver** | The actual object performing the action (`TV`). |
| **Invoker** | The client that triggers commands (`RemoteControl`). |
| **Example** | **Remote Control that turns TV ON/OFF using commands**. |

---
