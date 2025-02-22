## 🚀 **Module 18: System Design Interviews**  

System design interviews evaluate your ability to design scalable, reliable, and efficient systems. These interviews focus on architectural decisions, design patterns, scalability, database design, and system trade-offs. This module covers the core concepts, methodologies, and best practices needed to excel in system design interviews.

---

## **🔥 18.1 Why Learn System Design?**  
- **Scalable Solutions:** Design systems that can handle millions of users and data points.  
- **High Availability and Reliability:** Ensure minimal downtime and consistent user experience.  
- **Performance Optimization:** Design efficient systems with low latency and high throughput.  
- **Architectural Decisions:** Make informed choices on databases, caching, and communication protocols.  
- **Career Growth:** Essential for senior and architectural roles in top tech companies.  

---

## **🔥 18.2 System Design Interview Format**  
1. **Requirements Gathering:** Clarify functional and non-functional requirements.  
2. **High-Level Design:** Design high-level architecture with system components and interactions.  
3. **Detailed Design:** Deep dive into key components and discuss detailed design patterns.  
4. **Data Modeling:** Design the database schema and data flow.  
5. **Scalability and Performance:** Discuss scaling strategies, load balancing, and caching.  
6. **Security and Reliability:** Address data security, backup, and failover mechanisms.  
7. **Trade-Offs and Alternatives:** Discuss design trade-offs and alternative approaches.  
8. **Q&A and Extensions:** Handle follow-up questions and potential system extensions.  

---

## **🔥 18.3 Key Concepts in System Design**  
1. **Scalability:**  
    - **Vertical Scaling:** Adding more resources (CPU, RAM) to a single server.  
    - **Horizontal Scaling:** Adding more servers to distribute the load.  
2. **Load Balancing:** Distributing incoming requests across multiple servers.  
3. **Caching:** Storing frequently accessed data in memory to reduce latency.  
4. **Database Design:** Choosing between SQL and NoSQL databases.  
5. **Sharding and Partitioning:** Distributing data across multiple databases.  
6. **API Design:** Designing RESTful or GraphQL APIs for efficient communication.  
7. **Asynchronous Processing:** Using message queues for asynchronous communication.  
8. **Microservices Architecture:** Breaking down monolithic applications into independent services.  
9. **Security and Authentication:** Implementing OAuth, JWT, and secure communication.  
10. **Monitoring and Logging:** Tracking system performance and debugging issues.  

---

## **🔥 18.4 System Design Methodology**  
1. **Step 1: Requirements Gathering**  
    - **Functional Requirements:** Core features and functionalities of the system.  
    - **Non-Functional Requirements:** Scalability, availability, latency, and security requirements.  
    - **Constraints and Assumptions:** Traffic volume, storage requirements, and SLA expectations.  

---

### 📘 **Example: Design a URL Shortener (like bit.ly)**  
**Requirements:**  
- Convert long URLs to short URLs.  
- Redirect users to the original URL when short URL is visited.  
- Short URL should be unique and non-guessable.  
- Analytics on click statistics.  
- High availability and scalability.  

---

### 📘 **Step 2: High-Level Design**  
- **Components:**  
    - **Web Server:** Handles user requests and serves the web application.  
    - **Application Server:** Contains the business logic for URL shortening and redirection.  
    - **Database:** Stores long URLs, short codes, and analytics data.  
    - **Cache:** Caches frequently accessed short URLs to reduce database reads.  
- **System Flow:**  
    - User submits a long URL.  
    - Server generates a unique short code.  
    - Short URL is stored in the database.  
    - User is redirected to the original URL when visiting the short URL.  

---

### 📘 **Step 3: Detailed Design**  
1. **Database Design:**  
    - **Table: URL_Mapping**  
        ```
        +-------------+--------------+----------------+
        | Short_Code  | Original_URL | Click_Count    |
        +-------------+--------------+----------------+
        | abc123      | http://...   | 1000           |
        | xyz789      | http://...   | 500            |
        +-------------+--------------+----------------+
        ```
    - **Indexing:** Index on `Short_Code` for fast lookups.  
    - **NoSQL Alternative:** Use Redis for caching and MongoDB for fast reads/writes.  

2. **URL Shortening Algorithm:**  
    - Generate a unique short code using **Base62 Encoding** (characters 0-9, A-Z, a-z).  
    - Example: Convert the auto-increment ID to Base62.  
    - Short Code Length: 6-8 characters for 1 billion unique URLs.  

---

### 📘 **Example Code: Base62 Encoding for URL Shortener**  
```java
public class URLShortener {
    private static final String BASE62 = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    private static final int BASE = BASE62.length();

    // Encode a number to Base62
    public static String encode(long num) {
        StringBuilder sb = new StringBuilder();
        while (num > 0) {
            sb.append(BASE62.charAt((int) (num % BASE)));
            num /= BASE;
        }
        return sb.reverse().toString();
    }

    // Decode a Base62 string to number
    public static long decode(String str) {
        long num = 0;
        for (int i = 0; i < str.length(); i++) {
            num = num * BASE + BASE62.indexOf(str.charAt(i));
        }
        return num;
    }

    public static void main(String[] args) {
        long id = 12345;
        String shortCode = encode(id);
        System.out.println("Short Code: " + shortCode);

        long decodedId = decode(shortCode);
        System.out.println("Decoded ID: " + decodedId);
    }
}
```

---

### 📊 **Output:**  
```
Short Code: dnh
Decoded ID: 12345
```

---

### 🔥 **Explanation:**  
- **Base62 Encoding:** Converts a number to a Base62 string.  
- **Unique Short Code:** Ensures a unique and non-guessable short code.  
- **Time Complexity:** `O(log N)` — Logarithmic time for encoding and decoding.  
- **Space Complexity:** `O(1)` — Constant space.  

---

### 📘 **Step 4: Scaling and Caching**  
1. **Scaling:**  
    - **Horizontal Scaling:** Multiple application servers behind a Load Balancer.  
    - **Database Sharding:** Partition the database by Short Code hash.  
    - **CDN:** Distribute static assets globally.  

2. **Caching:**  
    - **Redis Cache:** Cache short URL mappings for faster redirection.  
    - **Cache Expiration:** Set expiration times to remove stale data.  
    - **Cache Invalidation:** Invalidate cache on updates.  

---

### 📘 **Step 5: Security and Reliability**  
- **Security:**  
    - **Input Validation:** Sanitize URLs to prevent XSS and SQL injection.  
    - **HTTPS:** Ensure secure communication.  
- **Reliability:**  
    - **Data Replication:** Master-slave replication for high availability.  
    - **Failover Mechanisms:** Automatic failover for database and cache.  
    - **Backup and Restore:** Regular backups for disaster recovery.  

---

## **🔥 18.5 System Design Best Practices**  
1. **Start with High-Level Design:** Focus on components and interactions first.  
2. **Scalable Architecture:** Design for scalability and future growth.  
3. **Redundancy and Failover:** Ensure high availability and reliability.  
4. **Data Partitioning:** Use sharding and partitioning for large datasets.  
5. **Load Balancing and Caching:** Optimize performance and reduce latency.  
6. **Security and Compliance:** Ensure data security and regulatory compliance.  
7. **Trade-Off Analysis:** Discuss trade-offs and alternative solutions.  
8. **Practice Mock Interviews:** Practice with peers or online platforms like Exponent.  

---

## 🔥 **Next: Design Patterns**  
System design interviews often require knowledge of **Design Patterns**. Next, we will explore Creational, Structural, and Behavioral Design Patterns.

---
