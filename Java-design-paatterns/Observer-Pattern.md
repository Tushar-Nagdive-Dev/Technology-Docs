# **🚀 Lesson 13: Observer Pattern – Managing Event-Driven Systems**

The **Observer Pattern** is a **behavioral design pattern** where **one object (Subject) notifies multiple dependent objects (Observers) about changes** in its state.

---

## **📌 1. What is the Observer Pattern?**
The **Observer Pattern**:
✔ **Allows multiple objects (Observers) to listen to changes in a Subject**.  
✔ **Ensures automatic updates** when the Subject changes.  
✔ **Decouples the Subject from its Observers**.  

---

## **📌 2. When to Use the Observer Pattern?**
✔ When **one object’s state change should automatically notify others**.  
✔ When implementing **Event-driven architectures**.  
✔ When following the **Open/Closed Principle** (Observers can be added dynamically).  
✔ When developing **real-time applications (e.g., Stock Market, Notifications, Weather Updates, Messaging Systems, etc.)**.  

---

## **📌 3. Real-World Analogy – YouTube Channel Subscription 📺**
- A **YouTube Channel (Subject)** publishes videos.  
- **Subscribers (Observers)** receive notifications when a new video is uploaded.  
- **New subscribers can be added dynamically** without modifying the channel.  

---

## **📌 4. Implementing Observer Pattern in Java**
Let’s build a **News Agency System** where **Subscribers receive News Updates**.

---

### **Step 1: Create the Observer Interface**
This interface defines the **update()** method for all subscribers.

```java
// Observer Interface (Subscribers)
public interface Subscriber {
    void update(String news);
}
```

---

### **Step 2: Implement Concrete Observers (Subscribers)**
These classes **receive news updates**.

```java
// Concrete Observer 1
public class EmailSubscriber implements Subscriber {
    private String email;

    public EmailSubscriber(String email) {
        this.email = email;
    }

    @Override
    public void update(String news) {
        System.out.println("Email sent to " + email + ": " + news);
    }
}

// Concrete Observer 2
public class SMSSubscriber implements Subscriber {
    private String phoneNumber;

    public SMSSubscriber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
    }

    @Override
    public void update(String news) {
        System.out.println("SMS sent to " + phoneNumber + ": " + news);
    }
}
```
✔ **Email and SMS subscribers receive notifications separately**.  
✔ New observer types (e.g., `PushNotificationSubscriber`) **can be added without modifying existing code**.  

---

### **Step 3: Create the Subject Interface**
This interface **manages subscribers**.

```java
import java.util.ArrayList;
import java.util.List;

// Subject Interface
public interface NewsAgency {
    void subscribe(Subscriber subscriber);
    void unsubscribe(Subscriber subscriber);
    void notifySubscribers(String news);
}
```

---

### **Step 4: Implement Concrete Subject (News Agency)**
This class **stores and notifies subscribers**.

```java
// Concrete Subject (News Agency)
public class NewsChannel implements NewsAgency {
    private List<Subscriber> subscribers = new ArrayList<>();

    @Override
    public void subscribe(Subscriber subscriber) {
        subscribers.add(subscriber);
    }

    @Override
    public void unsubscribe(Subscriber subscriber) {
        subscribers.remove(subscriber);
    }

    @Override
    public void notifySubscribers(String news) {
        for (Subscriber subscriber : subscribers) {
            subscriber.update(news);
        }
    }
}
```
✔ **Stores subscribers dynamically** in a `List`.  
✔ **Notifies all subscribers** whenever a new update is published.  

---

### **Step 5: Using the Observer Pattern**
```java
public class Main {
    public static void main(String[] args) {
        // Create News Channel (Subject)
        NewsChannel newsChannel = new NewsChannel();

        // Create Subscribers
        Subscriber emailSubscriber = new EmailSubscriber("tushar@example.com");
        Subscriber smsSubscriber = new SMSSubscriber("+1234567890");

        // Subscribe to News Channel
        newsChannel.subscribe(emailSubscriber);
        newsChannel.subscribe(smsSubscriber);

        // Publish News
        newsChannel.notifySubscribers("Breaking News: Observer Pattern Implemented in Java!");

        // Unsubscribe SMS Subscriber
        newsChannel.unsubscribe(smsSubscriber);

        // Publish Another News
        newsChannel.notifySubscribers("Update: Design Patterns Learning Continues!");
    }
}
```

---

## **📌 5. Expected Output**
```
Email sent to tushar@example.com: Breaking News: Observer Pattern Implemented in Java!
SMS sent to +1234567890: Breaking News: Observer Pattern Implemented in Java!

Email sent to tushar@example.com: Update: Design Patterns Learning Continues!
```
✔ **Both Email & SMS subscribers receive notifications initially**.  
✔ **SMS Subscriber is removed**, so only Email Subscriber gets the second update.  

---

## **📌 6. Advantages of the Observer Pattern**
| **Feature** | **Explanation** |
|------------|----------------|
| **Loosely Coupled** | Observers can be added/removed dynamically. |
| **Event-Driven** | Automatic updates when Subject changes. |
| **Scalable** | New observer types can be added without modifying existing code. |
| **Encapsulation** | Observers do not need direct access to Subject data. |

---

## **📌 7. Common Mistakes & How to Avoid Them**
❌ **Not removing unneeded observers** – Always **unsubscribe inactive observers** to avoid memory leaks.  
❌ **Directly calling observer methods** – Always use **notifySubscribers()** instead of calling `update()` manually.  
❌ **Using tight coupling** – Subject **should not depend on specific observer implementations**.  

---

## **🔥 Hands-On Challenge**
✔ Implement a **Stock Market Notification System** where investors receive **real-time stock updates**.  
✔ Implement a **Weather Monitoring System** where different devices (Mobile, TV, Website) get weather updates.  

---

## **🚀 Summary**
| **Concept** | **Explanation** |
|------------|----------------|
| **Observer Pattern** | Allows objects to subscribe and receive updates automatically. |
| **Subject (Publisher)** | Maintains a list of **observers (subscribers)** and notifies them on changes. |
| **Observers (Subscribers)** | Receive updates when the **subject state changes**. |
| **Example** | **News Agency with Email & SMS Subscribers**. |

---
