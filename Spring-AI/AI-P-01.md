# 🧠 **Phase 1: Core Foundations**

---

## 🎯 **Objective**

To understand the *what, why, and how* of integrating AI into Spring Boot applications—step by step and practically.

---

## 📌 Section 1: What is AI, ML, and DL?

| Term                             | Description                      | Example                                |
| -------------------------------- | -------------------------------- | -------------------------------------- |
| **AI (Artificial Intelligence)** | Simulates human intelligence     | ChatGPT, voice assistants              |
| **ML (Machine Learning)**        | Algorithms that learn from data  | Fraud detection, price prediction      |
| **DL (Deep Learning)**           | Neural networks with many layers | Face recognition, language translation |

---

## 📌 Section 2: Where AI Fits in a Spring Boot App

Spring Boot typically handles:

* Web requests (REST APIs)
* Business logic
* Integration with databases and other systems

**AI Integration Points:**

1. **REST Layer** → Accept prompts or data
2. **Service Layer** → Call AI APIs or run AI models locally
3. **Persistence Layer** (optional) → Store AI responses or history
4. **Frontend** (Angular/React) → Interacts with AI via APIs

---

## 📌 Section 3: Types of AI Integration in Spring Boot

| Type                             | Description                                 | Example                                                  |
| -------------------------------- | ------------------------------------------- | -------------------------------------------------------- |
| 🔹 **Prompt-based (API)**        | Send a prompt to an external AI like OpenAI | "Summarize this paragraph"                               |
| 🔹 **Model Inference (On-Prem)** | Load and run AI model inside Spring Boot    | Image classifier using DJL                               |
| 🔹 **AI Middleware**             | Separate microservice for AI                | Microservice running PyTorch/TensorFlow, called via REST |
| 🔹 **Hybrid**                    | Combo of local models and external APIs     | Local spellchecker + OpenAI GPT                          |

---

## 📌 Section 4: Technologies Overview

| Tech                                     | Use                              |
| ---------------------------------------- | -------------------------------- |
| **Spring Boot**                          | API layer, service orchestration |
| **OpenAI/HuggingFace APIs**              | Cloud-based AI                   |
| **DJL / ONNX Runtime / TensorFlow Java** | Run models in Java               |
| **Apache Camel / Kafka**                 | AI pipelines                     |
| **Angular**                              | UI integration                   |
| **WebClient**                            | API calling from Spring          |
| **Redis**                                | Store AI conversation/session    |
| **PostgreSQL**                           | Store AI responses and metadata  |

---

## 📌 Section 5: Setup Development Environment

### ✅ Requirements:

* JDK 17+
* Spring Boot (v3+ recommended)
* IntelliJ or VS Code with Spring extensions
* Maven or Gradle
* Postman / Swagger for API testing
* OpenAI account (for API key)

### ✅ Starter Project:

```bash
spring init --dependencies=web,data-jpa,lombok --build=maven ai-integration
```

### ✅ Project Structure:

```
src/main/java/com/example/ai
├── controller/
│   └── AiController.java
├── service/
│   └── AiService.java
├── dto/
│   └── AiRequest.java
│   └── AiResponse.java
└── AiIntegrationApplication.java
```

---

## ✅ Step-by-Step Practice Task

### 🎯 Create Your First AI Prompt API (Dummy for now)

1. **Create DTOs**:

```java
// AiRequest.java
public record AiRequest(String prompt) {}
```

```java
// AiResponse.java
public record AiResponse(String response) {}
```

2. **Controller Layer**:

```java
@RestController
@RequestMapping("/api/ai")
public class AiController {

    private final AiService aiService;

    public AiController(AiService aiService) {
        this.aiService = aiService;
    }

    @PostMapping("/prompt")
    public ResponseEntity<AiResponse> handlePrompt(@RequestBody AiRequest request) {
        String result = aiService.processPrompt(request.prompt());
        return ResponseEntity.ok(new AiResponse(result));
    }
}
```

3. **Service Layer**:

```java
@Service
public class AiService {
    public String processPrompt(String prompt) {
        return "You said: " + prompt; // Next: Replace with OpenAI API call
    }
}
```

4. **Test with Postman**:

```json
POST http://localhost:8080/api/ai/prompt
{
  "prompt": "Hello, what is AI?"
}
```

🟢 Response:

```json
{
  "response": "You said: Hello, what is AI?"
}
```

---

## ✅ Checklist for Phase 1 Completion

| Item                                      | Status     |
| ----------------------------------------- | ---------- |
| Understood AI vs ML vs DL                 | ✅          |
| Understood AI entry points in Spring Boot | ✅          |
| Installed Spring Boot                     | ✅          |
| Built prompt-processing API               | ✅          |
| Ready for OpenAI integration              | 🔜 Phase 2 |

---

### 📘 Optional Reading Before Phase 2

* [OpenAI API Docs](https://platform.openai.com/docs)
* [Spring Boot WebClient Guide](https://www.baeldung.com/spring-webclient-resttemplate)
