# 🚀 **Phase 3 - Lesson 27: Building AI-Powered Spring Boot Applications (ChatGPT, ML Models, TensorFlow)**  

## **📌 Lesson Objective**  
By the end of this lesson, you will:  
✅ Integrate **AI & Machine Learning with Spring Boot**  
✅ Use **OpenAI's ChatGPT API in a Spring Boot application**  
✅ Implement **AI-based text generation & summarization**  
✅ Deploy a **Machine Learning (ML) Model in Spring Boot using TensorFlow**  
✅ Build a **real-world AI-driven API for predictions**  

---

# 🧠 **Part 1: Integrating OpenAI's ChatGPT API in Spring Boot**  

## **1️⃣ Why Use AI in Spring Boot?**  
📌 **AI-powered applications** enhance user experience by:  
✔ **Generating intelligent responses** (Chatbots, AI Assistants)  
✔ **Summarizing content automatically**  
✔ **Providing smart recommendations**  

✅ **Use AI in Spring Boot when:**  
- You need **natural language processing (NLP) capabilities**.  
- You want to **automate content generation**.  

---

## **2️⃣ Using OpenAI's ChatGPT API in Spring Boot**  

📌 **Step 1: Get OpenAI API Key**  
1️⃣ Sign up at **[OpenAI](https://openai.com/)**  
2️⃣ Get your **API Key** from OpenAI's dashboard.  

📌 **Step 2: Add Dependencies in `pom.xml`**  
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webflux</artifactId>
</dependency>
```

📌 **Step 3: Create `OpenAIService.java` to Call ChatGPT API**  
```java
package com.example.demo.service;

import org.springframework.http.HttpHeaders;
import org.springframework.http.MediaType;
import org.springframework.stereotype.Service;
import org.springframework.web.reactive.function.client.WebClient;
import java.util.Map;

@Service
public class OpenAIService {
    
    private final WebClient webClient;
    private static final String OPENAI_API_KEY = "your-openai-api-key";

    public OpenAIService() {
        this.webClient = WebClient.builder()
                .baseUrl("https://api.openai.com/v1")
                .defaultHeader(HttpHeaders.AUTHORIZATION, "Bearer " + OPENAI_API_KEY)
                .defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE)
                .build();
    }

    public String getChatResponse(String prompt) {
        Map<String, Object> requestBody = Map.of(
            "model", "gpt-3.5-turbo",
            "messages", new Object[]{Map.of("role", "user", "content", prompt)}
        );

        return webClient.post()
                .uri("/chat/completions")
                .bodyValue(requestBody)
                .retrieve()
                .bodyToMono(String.class)
                .block();
    }
}
```

📌 **Step 4: Create API Controller (`ChatController.java`)**  
```java
package com.example.demo.controller;

import com.example.demo.service.OpenAIService;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/chat")
public class ChatController {

    private final OpenAIService openAIService;

    public ChatController(OpenAIService openAIService) {
        this.openAIService = openAIService;
    }

    @PostMapping
    public String chat(@RequestParam String prompt) {
        return openAIService.getChatResponse(prompt);
    }
}
```

📌 **Step 5: Test the AI-Powered Chat API**  
```bash
curl -X POST "http://localhost:8080/api/chat" -d "prompt=Hello, AI! How are you?"
```
✔ **ChatGPT now responds to your API calls!** 🎉  

---

# 🔢 **Part 2: Deploying a Machine Learning Model in Spring Boot (TensorFlow)**  

## **3️⃣ Why Use Machine Learning in Spring Boot?**  
📌 **ML models** enable applications to **predict outcomes, classify data, and automate decision-making**.  

✅ **Use ML in Spring Boot when:**  
- You need **image/text classification**.  
- You want **predictive analytics**.  

---

## **4️⃣ Deploying a TensorFlow Model in Spring Boot**  

📌 **Step 1: Add TensorFlow Dependency in `pom.xml`**  
```xml
<dependency>
    <groupId>org.tensorflow</groupId>
    <artifactId>tensorflow</artifactId>
    <version>2.9.0</version>
</dependency>
```

📌 **Step 2: Load a Pre-Trained TensorFlow Model (`MLService.java`)**  
```java
package com.example.demo.service;

import org.springframework.stereotype.Service;
import org.tensorflow.*;

import java.nio.FloatBuffer;
import java.util.List;

@Service
public class MLService {

    private final SavedModelBundle model;

    public MLService() {
        this.model = SavedModelBundle.load("model_directory", "serve");
    }

    public float predict(float[] input) {
        try (Session session = model.session()) {
            Tensor<Float> inputTensor = Tensors.create(FloatBuffer.wrap(input));
            List<Tensor<?>> outputs = session.runner().feed("input_tensor", inputTensor).fetch("output_tensor").run();
            float result = outputs.get(0).copyTo(new float[1])[0];
            return result;
        }
    }
}
```

📌 **Step 3: Create an API Endpoint (`MLController.java`)**  
```java
package com.example.demo.controller;

import com.example.demo.service.MLService;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/ml")
public class MLController {

    private final MLService mlService;

    public MLController(MLService mlService) {
        this.mlService = mlService;
    }

    @PostMapping("/predict")
    public float predict(@RequestBody float[] input) {
        return mlService.predict(input);
    }
}
```

📌 **Step 4: Test ML Model Deployment**  
```bash
curl -X POST "http://localhost:8080/api/ml/predict" -H "Content-Type: application/json" -d "[1.0, 2.0, 3.0]"
```
✔ The **ML model now makes real-time predictions!** 🚀  

---

# 🏆 **Part 3: Building a Real-World AI-Powered API**  

## **5️⃣ Implementing AI-Based Sentiment Analysis**  
📌 **Step 1: Add NLP Dependency (Stanford NLP)**  
```xml
<dependency>
    <groupId>edu.stanford.nlp</groupId>
    <artifactId>stanford-corenlp</artifactId>
    <version>4.5.0</version>
</dependency>
```

📌 **Step 2: Create `SentimentAnalysisService.java`**  
```java
package com.example.demo.service;

import edu.stanford.nlp.pipeline.*;
import org.springframework.stereotype.Service;

import java.util.Properties;

@Service
public class SentimentAnalysisService {

    private final StanfordCoreNLP pipeline;

    public SentimentAnalysisService() {
        Properties props = new Properties();
        props.setProperty("annotators", "tokenize, ssplit, pos, lemma, parse, sentiment");
        this.pipeline = new StanfordCoreNLP(props);
    }

    public String analyzeSentiment(String text) {
        CoreDocument document = new CoreDocument(text);
        pipeline.annotate(document);
        return document.sentences().get(0).sentiment();
    }
}
```

📌 **Step 3: Create API Endpoint (`SentimentController.java`)**  
```java
package com.example.demo.controller;

import com.example.demo.service.SentimentAnalysisService;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/sentiment")
public class SentimentController {

    private final SentimentAnalysisService sentimentService;

    public SentimentController(SentimentAnalysisService sentimentService) {
        this.sentimentService = sentimentService;
    }

    @PostMapping
    public String analyze(@RequestParam String text) {
        return sentimentService.analyzeSentiment(text);
    }
}
```

📌 **Step 4: Test AI Sentiment Analysis API**  
```bash
curl -X POST "http://localhost:8080/api/sentiment" -d "text=I love Spring Boot!"
```
✔ **Response:** `"Positive"`  

🎉 **AI-based sentiment analysis is now integrated into Spring Boot!** 🚀  

---

## 🎯 **Lesson 27 - Summary**  
✅ Integrated **ChatGPT API into Spring Boot**  
✅ Deployed a **TensorFlow ML model in Spring Boot**  
✅ Built **AI-powered Sentiment Analysis using NLP**  

---
