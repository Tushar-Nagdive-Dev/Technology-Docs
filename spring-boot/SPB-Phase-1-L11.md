# 🚀 **Phase 1 - Lesson 11: Real-Time Notifications with Spring Boot WebSockets** 🚀  

## **📌 Lesson Objective**  
By the end of this lesson, you will:  
✅ Understand **WebSockets and why they are used**  
✅ Learn how to **set up WebSockets in Spring Boot**  
✅ Implement **real-time communication between clients**  
✅ Use **STOMP (Simple Text Oriented Messaging Protocol)** for message handling  
✅ Build a **real-time notification system**  

---

## **1️⃣ What is WebSocket & Why Use It?**  
📌 **WebSocket** is a protocol that enables **real-time, bidirectional communication** between the server and the client.  

### **🔥 WebSocket vs REST API**
| Feature | REST API (HTTP) | WebSockets |
|---------|----------------|------------|
| Communication | **Request-Response** | **Real-Time, Event-Driven** |
| Connection Type | **Stateless** | **Persistent** |
| Use Case | **Fetching Data, CRUD APIs** | **Live Notifications, Chat, Stock Updates** |

### **🔥 Real-World Use Cases**
✔ **Live Chat Applications** 💬  
✔ **Stock Price Updates** 📈  
✔ **Push Notifications** 🔔  
✔ **Online Multiplayer Games** 🎮  

---

## **2️⃣ Setting Up WebSockets in Spring Boot**  

📌 **Step 1: Add WebSocket Dependency in `pom.xml`**  
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-websocket</artifactId>
</dependency>
```

📌 **Step 2: Enable WebSockets in Spring Boot**  
Create a new package **`com.example.demo.config`** and add `WebSocketConfig.java`:

```java
package com.example.demo.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.messaging.simp.config.MessageBrokerRegistry;
import org.springframework.web.socket.config.annotation.EnableWebSocketMessageBroker;
import org.springframework.web.socket.config.annotation.StompEndpointRegistry;
import org.springframework.web.socket.config.annotation.WebSocketMessageBrokerConfigurer;

@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {

    @Override
    public void configureMessageBroker(MessageBrokerRegistry config) {
        config.enableSimpleBroker("/topic"); // Client subscribes to this prefix
        config.setApplicationDestinationPrefixes("/app"); // Client sends messages here
    }

    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        registry.addEndpoint("/ws") // WebSocket endpoint
                .setAllowedOrigins("*")
                .withSockJS(); // Enables fallback options for older browsers
    }
}
```

✅ **How It Works?**
- `@EnableWebSocketMessageBroker` → Enables WebSockets in Spring Boot.
- `configureMessageBroker()`:
  - `enableSimpleBroker("/topic")` → Messages sent to `/topic` will be received by subscribers.
  - `setApplicationDestinationPrefixes("/app")` → Clients send messages to `/app`.
- `registerStompEndpoints()`:
  - `/ws` is the **WebSocket connection endpoint**.
  - `SockJS` enables fallback for browsers that don’t support WebSockets.

---

## **3️⃣ Creating a WebSocket Controller**  

📌 **Step 3: Create `WebSocketController.java`**
Create a new package **`com.example.demo.controller`** and add:

```java
package com.example.demo.controller;

import com.example.demo.model.Message;
import org.springframework.messaging.handler.annotation.MessageMapping;
import org.springframework.messaging.handler.annotation.SendTo;
import org.springframework.stereotype.Controller;

@Controller
public class WebSocketController {

    @MessageMapping("/sendMessage") // Clients send messages here
    @SendTo("/topic/public") // Broadcasts to all subscribed clients
    public Message broadcastMessage(Message message) {
        return message;
    }
}
```

✅ **How It Works?**
- `@MessageMapping("/sendMessage")` → Listens for messages from clients.
- `@SendTo("/topic/public")` → Broadcasts messages to all **subscribed clients**.

---

## **4️⃣ Defining the Message Model**
📌 **Step 4: Create `Message.java` in `com.example.demo.model`**  

```java
package com.example.demo.model;

public class Message {
    private String sender;
    private String content;

    public Message() {}

    public Message(String sender, String content) {
        this.sender = sender;
        this.content = content;
    }

    public String getSender() { return sender; }
    public void setSender(String sender) { this.sender = sender; }

    public String getContent() { return content; }
    public void setContent(String content) { this.content = content; }
}
```

✅ **This represents a chat message with `sender` and `content`.**  

---

## **5️⃣ Creating a Simple Frontend to Test WebSocket**
📌 **Step 5: Create `index.html` in `src/main/resources/static`**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>WebSocket Chat</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/sockjs-client/1.5.1/sockjs.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/stomp.js/2.3.3/stomp.min.js"></script>
</head>
<body>
    <h1>Spring Boot WebSocket Chat</h1>
    <input type="text" id="name" placeholder="Enter your name">
    <input type="text" id="message" placeholder="Enter your message">
    <button onclick="sendMessage()">Send</button>
    <ul id="messages"></ul>

    <script>
        var socket = new SockJS('/ws');
        var stompClient = Stomp.over(socket);
        
        stompClient.connect({}, function (frame) {
            console.log('Connected: ' + frame);
            stompClient.subscribe('/topic/public', function (message) {
                showMessage(JSON.parse(message.body));
            });
        });

        function sendMessage() {
            var sender = document.getElementById("name").value;
            var content = document.getElementById("message").value;
            stompClient.send("/app/sendMessage", {}, JSON.stringify({sender: sender, content: content}));
        }

        function showMessage(message) {
            var list = document.getElementById("messages");
            var item = document.createElement("li");
            item.textContent = message.sender + ": " + message.content;
            list.appendChild(item);
        }
    </script>
</body>
</html>
```

✅ **How It Works?**
- Establishes a **WebSocket connection** with `/ws`.
- Subscribes to **`/topic/public`** to receive messages.
- Sends messages to **`/app/sendMessage`**.

---

## **6️⃣ Running and Testing the WebSocket Chat**
### **Step 1: Start Spring Boot Application**
```bash
mvn spring-boot:run
```
### **Step 2: Open the Frontend**
Go to **`http://localhost:8080/index.html`**.

### **Step 3: Send Messages**
- Open **multiple browser tabs**.
- Enter a **name & message** and click **Send**.
- Messages appear in **real-time** on all open pages! 🎉

---

## 🎯 **Lesson 11 - Summary**
✅ Implemented **real-time communication using WebSockets**  
✅ Used **STOMP protocol** for message handling  
✅ Created **WebSocket API for real-time messaging**  
✅ Built a **simple WebSocket-based chat app**  

---
