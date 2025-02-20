### **Phase 2: Setting Up Kafka Locally** 🚀  
Now that you understand the fundamentals, it's time to get your hands dirty! In this phase, we'll set up **Apache Kafka** on your local machine and perform basic operations like creating topics, producing messages, and consuming messages.

---

## **1. System Requirements**  
Before installing Kafka, ensure your system meets the following requirements:
- **Operating System**: Windows, macOS, or Linux
- **Java**: JDK 8 or later (Kafka runs on the JVM)
- **Memory**: Minimum 4 GB RAM (8 GB recommended)
- **Disk Space**: At least 2 GB free space
- **Network**: Loopback (localhost) connectivity

---

## **2. Installing Java (If Not Already Installed)**
1. **Check Java Installation**  
   Open a terminal or command prompt and run:
   ```bash
   java -version
   ```
   You should see a version number. If not, proceed with installation.

2. **Install Java (JDK 11 Recommended)**
   - Download JDK 11 from [AdoptOpenJDK](https://adoptopenjdk.net/) or [Oracle](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html).
   - Install it and set the `JAVA_HOME` environment variable.
   - Add `JAVA_HOME/bin` to your system's PATH.

3. **Verify Installation**:
   ```bash
   java -version
   javac -version
   ```

---

## **3. Downloading and Installing Apache Kafka**  
1. **Download Kafka**:  
   - Go to [Apache Kafka Downloads](https://kafka.apache.org/downloads)
   - Download the latest **binary** version (e.g., `kafka_2.13-3.0.0.tgz`).

2. **Extract the Archive**:
   ```bash
   tar -xvf kafka_2.13-3.0.0.tgz
   cd kafka_2.13-3.0.0
   ```

3. **Directory Structure**:
   - `bin/`: Scripts to run Kafka and Zookeeper.
   - `config/`: Configuration files for Kafka and Zookeeper.
   - `logs/`: Log files.
   - `libs/`: Kafka and Zookeeper libraries.

---

## **4. Configuring Zookeeper and Kafka**  
### **1. Zookeeper Configuration**
- Zookeeper is required to manage Kafka brokers.
- The default configuration file is located at: `config/zookeeper.properties`

**Key Settings**:
```properties
dataDir=/tmp/zookeeper
clientPort=2181
maxClientCnxns=60
```

### **2. Kafka Broker Configuration**
- Located at: `config/server.properties`

**Key Settings**:
```properties
broker.id=0                    # Unique ID for the broker
listeners=PLAINTEXT://localhost:9092
log.dirs=/tmp/kafka-logs        # Directory to store logs
num.partitions=1               # Default number of partitions per topic
zookeeper.connect=localhost:2181
```

---

## **5. Starting Zookeeper and Kafka**  
### **1. Start Zookeeper**:
```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
```

- You should see logs indicating that Zookeeper is running on `port 2181`.

### **2. Start Kafka Broker**:
Open a new terminal and run:
```bash
bin/kafka-server-start.sh config/server.properties
```

- You should see logs confirming Kafka is running on `port 9092`.

---

## **6. Basic Kafka Operations**  
### **1. Creating a Topic**  
```bash
bin/kafka-topics.sh --create --topic my-first-topic --bootstrap-server localhost:9092 --partitions 3 --replication-factor 1
```

**Explanation**:
- `--topic`: Name of the topic (`my-first-topic`)
- `--partitions`: Number of partitions (3 in this case)
- `--replication-factor`: Number of copies of data (1 for local setup)
- `--bootstrap-server`: Kafka broker address (`localhost:9092`)

### **2. List All Topics**  
```bash
bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```

### **3. Describe Topic Details**  
```bash
bin/kafka-topics.sh --describe --topic my-first-topic --bootstrap-server localhost:9092
```

---

## **7. Producing and Consuming Messages**  
### **1. Producing Messages**  
```bash
bin/kafka-console-producer.sh --topic my-first-topic --bootstrap-server localhost:9092
```
- Type messages and press **Enter** to produce them.
- Example:
```
Hello Kafka!
This is my first message!
```

### **2. Consuming Messages**  
Open another terminal and run:
```bash
bin/kafka-console-consumer.sh --topic my-first-topic --from-beginning --bootstrap-server localhost:9092
```
- This will display all messages from the beginning of the topic.

---

## **8. Viewing Topic Offsets**  
```bash
bin/kafka-run-class.sh kafka.tools.GetOffsetShell --topic my-first-topic --broker-list localhost:9092
```
- This shows the latest offset (message position) for each partition.

---

## **9. Stopping Zookeeper and Kafka**  
- **Stop Kafka Broker**:
```bash
bin/kafka-server-stop.sh
```

- **Stop Zookeeper**:
```bash
bin/zookeeper-server-stop.sh
```

---

## **10. Common Installation Issues and Fixes**  
- **Port Already in Use**:
  - Check for processes using port `9092` or `2181`:
    ```bash
    netstat -tuln | grep 9092
    ```
  - Kill the process or change the port in the config file.

- **Broker Not Connecting to Zookeeper**:
  - Ensure Zookeeper is running before starting Kafka.
  - Check `zookeeper.connect` property in `server.properties`.

- **Java Not Found Error**:
  - Ensure `JAVA_HOME` is set correctly.
  - Add `JAVA_HOME/bin` to the system PATH.

---

## **11. Exercises and Resources**  
### **Hands-On Lab**  
1. Install and configure Kafka on your local machine.
2. Create a topic with 3 partitions.
3. Produce and consume messages from the topic.
4. View topic details and offsets.

### **Watch**:
- [Kafka Installation Guide](https://www.youtube.com/watch?v=UtHG-YK_f_A)
- [Kafka Basics](https://www.youtube.com/watch?v=bL_Tps8kZ5c)

### **Read**:
- [Kafka Quickstart Guide](https://kafka.apache.org/quickstart)

### **Quiz**:
- What is the role of Zookeeper in Kafka?
- How do partitions affect scalability and performance?
- What is a bootstrap server in Kafka?

---

## **Next Step: Phase 3 - Deep Dive into Kafka Architecture**  
In the next phase, we will:
- Explore Kafka's architecture in depth.
- Understand how partitions, brokers, producers, and consumers work.
- Learn about consumer groups, offset management, and replication for high availability.

---
