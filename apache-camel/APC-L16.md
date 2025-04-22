
## ☁️ **Stage 3 – Lesson 17: Camel K & Cloud-Native Apache Camel**

> *“Write Camel routes, run them on Kubernetes with a single command.”*

---

## 🌪️ What is **Camel K**?

> **Camel K** is a lightweight, cloud-native integration runtime built on **Apache Camel**, designed to run **serverless** and **fast** on **Kubernetes/OpenShift**.

| Feature | Benefit |
|--------|---------|
| 🧾 Write DSLs in Java, XML, Groovy, YAML, etc. |
| ⚡ Super fast startup time (great for serverless) |
| 🔁 Automatic deployment via `kamel` CLI |
| 🧠 Operator-based lifecycle management |
| 📦 No need to build Docker images manually |

---

## ✅ 1. Camel K Architecture

```
You ➝ kamel CLI ➝ Kubernetes (via Camel K Operator) ➝ Integration Pod
```

✅ You just write the route, and `kamel` does:
- Build
- Package
- Deploy
- Monitor

---

## 🚀 2. Installation Prerequisites

### Tools:
- Kubernetes cluster (Minikube / Kind / EKS / OpenShift)
- `kubectl`
- `kamel` CLI (Camel K tool)

### Install Camel K CLI:
```bash
brew install camel-k  # Mac
choco install camel-k # Windows
```

Or manually: [https://camel.apache.org/camel-k/latest/installation/](https://camel.apache.org/camel-k/latest/installation/)

---

## 📦 3. Install Camel K on Kubernetes

```bash
kamel install
```

✅ Installs the **Camel K Operator** in your cluster.

---

## ✏️ 4. Your First Camel K Route (Java DSL)

Create a file called `HelloRoute.java`:

```java
import org.apache.camel.builder.RouteBuilder;

public class HelloRoute extends RouteBuilder {
  public void configure() {
    from("timer:hello?period=2000")
      .setBody().simple("Hello from Camel K!")
      .log("${body}");
  }
}
```

Deploy it to Kubernetes:

```bash
kamel run HelloRoute.java
```

🎉 That’s it! Your Camel route is now running in a pod.

---

## 📁 5. Supported DSLs in Camel K

| DSL | File Extension |
|-----|----------------|
| Java | `.java` |
| XML | `.xml` |
| YAML | `.yaml` |
| Groovy | `.groovy` |
| Kotlin | `.kts` |

You can run a YAML route:

```bash
kamel run myRoute.yaml
```

---

## 🌐 6. Integration with REST/Kafka

Example – REST in Camel K (YAML):
```yaml
- from:
    uri: "rest:get:/hello"
    steps:
      - set-body:
          simple: "Hello from Camel K REST!"
```

Kafka:
```yaml
- from:
    uri: "kafka:orders?brokers=my-kafka:9092"
    steps:
      - log: "Order received: ${body}"
```

---

## 🧠 Real-World Use Case

> Deploy a **payment validation service** that:
- Listens to Kafka
- Validates message
- Sends to HTTP or JMS
- Auto-redeploys when code changes

Camel K allows you to do this live with minimal setup!

---

## ⚠️ Common Pitfalls

| Problem | Fix |
|--------|-----|
| `kamel` not found | Install CLI from official site |
| Integration not starting | Ensure operator is running (`kubectl get pods`) |
| No logs | Use `kamel logs <integration-name>` |

---

## 🧪 Mini Challenge

Write a simple `GoodbyeRoute.java`:
- Runs every 3 seconds
- Logs `"Goodbye from Camel K!"`

Run it on your local Kubernetes with:
```bash
kamel run GoodbyeRoute.java
```
---
```javaimport org.apache.camel.builder.RouteBuilder;

public class GoodbyeRoute extends RouteBuilder {

    @Override
    public void configure() throws Exception {
        from("timer:goodbye?period=3000")
            .log("Goodbye from Camel K!");
    }
}
```

To run this on your local Kubernetes cluster using Camel K, execute the following command in your terminal:

```bash
kamel run GoodbyeRoute.java
```

**Prerequisites**:
- Ensure you have a Kubernetes cluster running (e.g., Minikube, Kind, or a local setup).
- Install the Camel K CLI (`kamel`) and configure it to connect to your cluster.
- Have Apache Camel K operator installed in your Kubernetes cluster.

This command will deploy the `GoodbyeRoute` integration to your Kubernetes cluster, and it will log "Goodbye from Camel K!" every 3 seconds. You can check the logs using `kubectl logs` or the Camel K CLI.
