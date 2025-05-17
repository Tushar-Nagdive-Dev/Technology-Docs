# 🗣️🚦 **Phase 4: Real-Time AI with Voice Input, Redis Sessions, and Event Streaming**

---

## 🎯 **Objective**

To build a **real-time, voice-enabled AI assistant** using:

* 🎤 Voice Input (Speech-to-Text)
* 🧠 Conversational context (Redis)
* 🔁 Async AI processing (Kafka)

---

## 📌 Section 1: Architecture Overview

```
[User Voice Input]
     ↓
[Angular (Web Speech API)]
     ↓
[Spring Boot API (Text Prompt)]
     ↓
[Kafka Topic - ai_prompts]
     ↓
[AI Consumer → OpenAI/DJL]
     ↓
[Kafka Topic - ai_responses]
     ↓
[Redis/Database Store Response]
     ↓
[Angular UI → Poll/Subscribe]
```

---

## 📌 Section 2: Voice Input in Angular (Front-End)

Use **Web Speech API** for capturing voice:

```ts
const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
recognition.lang = 'en-US';

recognition.onresult = (event) => {
  const voiceText = event.results[0][0].transcript;
  this.sendPrompt(voiceText);
};

recognition.start();
```

📝 This sends the voice prompt as text to your Spring Boot backend.

---

## 📌 Section 3: Kafka Setup in Spring Boot

### ✅ Add Kafka dependencies:

```xml
<dependency>
  <groupId>org.springframework.kafka</groupId>
  <artifactId>spring-kafka</artifactId>
</dependency>
```

### ✅ application.properties:

```properties
spring.kafka.bootstrap-servers=localhost:9092
spring.kafka.consumer.group-id=ai-group
```

---

## 📌 Section 4: Kafka Producer – Send Prompt

```java
@Service
public class PromptProducer {
    private final KafkaTemplate<String, String> kafkaTemplate;

    public PromptProducer(KafkaTemplate<String, String> kafkaTemplate) {
        this.kafkaTemplate = kafkaTemplate;
    }

    public void sendPrompt(String prompt) {
        kafkaTemplate.send("ai_prompts", prompt);
    }
}
```

---

## 📌 Section 5: Kafka Consumer – Process AI

```java
@Service
public class AiPromptConsumer {

    private final OpenAiService aiService;
    private final KafkaTemplate<String, String> kafkaTemplate;

    public AiPromptConsumer(OpenAiService aiService, KafkaTemplate<String, String> kafkaTemplate) {
        this.aiService = aiService;
        this.kafkaTemplate = kafkaTemplate;
    }

    @KafkaListener(topics = "ai_prompts", groupId = "ai-group")
    public void consumePrompt(String prompt) {
        String reply = aiService.getCompletion(prompt);
        kafkaTemplate.send("ai_responses", reply);
    }
}
```

---

## 📌 Section 6: Store Conversation Context (Redis)

### ✅ Add Redis dependency:

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

### ✅ Store/retrieve conversation:

```java
@Service
public class ConversationService {

    private final StringRedisTemplate redisTemplate;

    public ConversationService(StringRedisTemplate redisTemplate) {
        this.redisTemplate = redisTemplate;
    }

    public void saveResponse(String sessionId, String reply) {
        redisTemplate.opsForList().rightPush("chat:" + sessionId, reply);
    }

    public List<String> getConversation(String sessionId) {
        return redisTemplate.opsForList().range("chat:" + sessionId, 0, -1);
    }
}
```

---

## 📌 Section 7: UI: Poll or Subscribe to Responses

You can either:

* Poll `/api/conversation/{sessionId}`
* Or use WebSocket for push

---

## 📌 Optional Add-on: Speech-to-Text with Google STT or Whisper API

You can use:

* 🎙️ [Google Cloud STT](https://cloud.google.com/speech-to-text)
* 🧠 [Whisper API](https://platform.openai.com/docs/guides/speech-to-text)

To enhance voice recognition quality.

---

## ✅ Checklist for Phase 4 Completion

| Task                               | Status     |
| ---------------------------------- | ---------- |
| Captured voice input from Angular  | ✅          |
| Built prompt queue using Kafka     | ✅          |
| AI processed prompt asynchronously | ✅          |
| Stored response in Redis           | ✅          |
| Enabled real-time reply fetch      | ✅          |
| Ready to orchestrate pipelines     | 🔜 Phase 5 |
