# ⚙️ **Phase 3: Run AI/ML Models Inside Spring Boot (Java Native Inference)**

---

## 🎯 **Objective**

Use a **pre-trained AI model** locally in a Spring Boot app using Java libraries like **DJL (Deep Java Library)** and **ONNX Runtime**.

---

## 📌 Section 1: Choose the Right Tool

| Tool                        | When to Use                  | Notes                                     |
| --------------------------- | ---------------------------- | ----------------------------------------- |
| **DJL (Deep Java Library)** | Easiest for Java + Spring    | Supports PyTorch, TensorFlow, MXNet, ONNX |
| **ONNX Runtime Java**       | When you have ONNX models    | Very fast, cross-platform                 |
| **TensorFlow Java API**     | For TF models only           | Heavy and lower-level                     |
| **JEP (Java Embed Python)** | When model is only in Python | Needs Python installed, less portable     |

➡️ For this phase, we'll use **DJL**, the most production-ready and Spring Boot–friendly AI engine for Java.

---

## 📌 Section 2: Setup Project for DJL

### ✅ Add Maven dependencies:

```xml
<!-- DJL Core -->
<dependency>
    <groupId>ai.djl</groupId>
    <artifactId>api</artifactId>
    <version>0.26.0</version>
</dependency>

<!-- PyTorch Engine -->
<dependency>
    <groupId>ai.djl.pytorch</groupId>
    <artifactId>pytorch-engine</artifactId>
    <version>0.26.0</version>
</dependency>

<!-- Model Zoo -->
<dependency>
    <groupId>ai.djl.pytorch</groupId>
    <artifactId>pytorch-model-zoo</artifactId>
    <version>0.26.0</version>
</dependency>
```

> You can also use `tensorflow-engine` or `onnxruntime-engine` instead.

---

## 📌 Section 3: Load a Pre-trained Model (Sentiment Analysis Example)

We’ll use a built-in sentiment analysis model from DJL’s model zoo.

### ✅ `SentimentService.java`

```java
@Service
public class SentimentService {

    public String predictSentiment(String inputText) {
        try (ZooModel<String, Classifications> model = loadModel();
             Predictor<String, Classifications> predictor = model.newPredictor()) {

            Classifications result = predictor.predict(inputText);
            return result.best().getClassName(); // e.g., "Positive"
        } catch (Exception e) {
            throw new RuntimeException("Failed to predict sentiment: " + e.getMessage());
        }
    }

    private ZooModel<String, Classifications> loadModel() throws IOException, ModelException {
        Criteria<String, Classifications> criteria = Criteria.builder()
                .setTypes(String.class, Classifications.class)
                .optApplication(Application.NLP.SENTIMENT_ANALYSIS)
                .build();

        return ModelZoo.loadModel(criteria);
    }
}
```

---

### ✅ `SentimentController.java`

```java
@RestController
@RequestMapping("/api/sentiment")
public class SentimentController {

    private final SentimentService sentimentService;

    public SentimentController(SentimentService sentimentService) {
        this.sentimentService = sentimentService;
    }

    @PostMapping
    public ResponseEntity<Map<String, String>> analyze(@RequestBody Map<String, String> request) {
        String sentiment = sentimentService.predictSentiment(request.get("text"));
        return ResponseEntity.ok(Map.of("sentiment", sentiment));
    }
}
```

---

## 📌 Section 4: Run and Test It

```http
POST http://localhost:8080/api/sentiment
Content-Type: application/json

{
  "text": "I love how easy Spring Boot makes development!"
}
```

🟢 **Response**:

```json
{
  "sentiment": "Positive"
}
```

---

## 📌 Section 5: Where to Get More AI Models?

* DJL Model Zoo: [https://github.com/deepjavalibrary/djl-model-zoo](https://github.com/deepjavalibrary/djl-model-zoo)
* Convert Hugging Face models to ONNX and use them in DJL
* Use your own fine-tuned model

---

## 🔐 Advanced Ideas for You

| Feature               | How                           |
| --------------------- | ----------------------------- |
| ✅ Offline AI          | Use DJL model cache           |
| ✅ GPU support         | Add GPU engine for DJL        |
| ✅ Upload custom model | Load `.pt` or `.onnx` file    |
| ✅ Voice or Vision     | Use DJL image/audio pipelines |

---

## ✅ Checklist for Phase 3 Completion

| Task                           | Status     |
| ------------------------------ | ---------- |
| DJL setup in Spring Boot       | ✅          |
| Local model inference working  | ✅          |
| Used Sentiment Analysis model  | ✅          |
| Ready for custom model loading | 🔜 Phase 4 |
