# 🚀 **Phase 2: Prompt-based AI Integration (Spring Boot + OpenAI)**

---

## 🎯 **Objective**

To build a real AI integration using Spring Boot that sends prompts and receives responses from the **OpenAI ChatGPT API**.

---

## 📌 Section 1: Prerequisites

### ✅ Get Your OpenAI API Key

1. Go to [https://platform.openai.com/account/api-keys](https://platform.openai.com/account/api-keys)
2. Generate a new key
3. Copy and save it securely

---

### ✅ Add Dependencies

#### In `pom.xml`:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-json</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

Optional (for Lombok):

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <scope>provided</scope>
</dependency>
```

---

## 📌 Section 2: Configuration

### ✅ Store your API key securely

#### In `application.properties`:

```properties
openai.api.key=sk-xxxxxxxxxxxxxxxxxxxx
openai.api.url=https://api.openai.com/v1/chat/completions
```

---

## 📌 Section 3: Build OpenAI Integration

### ✅ `AiRequest` DTO:

```java
public record AiRequest(String prompt) {}
```

### ✅ `AiResponse` DTO:

```java
public record AiResponse(String response) {}
```

---

### ✅ `OpenAiService.java`:

```java
@Service
public class OpenAiService {

    @Value("${openai.api.key}")
    private String apiKey;

    @Value("${openai.api.url}")
    private String apiUrl;

    private final WebClient webClient = WebClient.builder().build();

    public String getCompletion(String prompt) {
        String requestBody = """
            {
              "model": "gpt-3.5-turbo",
              "messages": [{"role": "user", "content": "%s"}]
            }
            """.formatted(prompt);

        return webClient.post()
                .uri(apiUrl)
                .header(HttpHeaders.AUTHORIZATION, "Bearer " + apiKey)
                .header(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE)
                .bodyValue(requestBody)
                .retrieve()
                .bodyToMono(String.class)
                .map(response -> extractContent(response))
                .block();
    }

    private String extractContent(String json) {
        try {
            ObjectMapper mapper = new ObjectMapper();
            JsonNode root = mapper.readTree(json);
            return root.path("choices").get(0).path("message").path("content").asText();
        } catch (Exception e) {
            return "Error parsing response: " + e.getMessage();
        }
    }
}
```

---

### ✅ `AiController.java`:

```java
@RestController
@RequestMapping("/api/ai")
public class AiController {

    private final OpenAiService aiService;

    public AiController(OpenAiService aiService) {
        this.aiService = aiService;
    }

    @PostMapping("/prompt")
    public ResponseEntity<AiResponse> prompt(@RequestBody AiRequest request) {
        String reply = aiService.getCompletion(request.prompt());
        return ResponseEntity.ok(new AiResponse(reply));
    }
}
```

---

## 📌 Section 4: Test Your AI API

Use **Postman** or **curl**:

```http
POST http://localhost:8080/api/ai/prompt
Content-Type: application/json

{
  "prompt": "Explain Spring Boot in one sentence."
}
```

🟢 **Sample Response**:

```json
{
  "response": "Spring Boot simplifies Java application development by offering production-ready defaults and auto-configuration."
}
```

---

## 📌 Section 5: Error Handling & Validation (Optional Advanced)

### Add @Valid on Request:

```java
public record AiRequest(@NotBlank String prompt) {}
```

### Global Error Handler:

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    public ResponseEntity<?> handle(Exception e) {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
                .body(Map.of("error", e.getMessage()));
    }
}
```

---

## ✅ Checklist for Phase 2 Completion

| Task                                   | Status     |
| -------------------------------------- | ---------- |
| Connected Spring Boot with OpenAI      | ✅          |
| Sent a prompt and received AI response | ✅          |
| Used WebClient with headers and JSON   | ✅          |
| Parsed JSON and returned clean message | ✅          |
| Ready for real AI-powered features     | 🔜 Phase 3 |
