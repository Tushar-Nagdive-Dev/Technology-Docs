# 🧠📊 **Phase 5: AI + Data Pipelines + Smart Decisioning (Kafka + Apache Camel)**

---

## 🎯 **Objective**

To integrate AI into business workflows by:

* Consuming real-time data streams
* Running AI models for classification/scoring
* Making intelligent routing decisions using Kafka + Camel

---

## 📌 Section 1: Use Case Example – Smart Invoice Classification

### Real-world flow:

```
[Invoice Data Stream]
     ↓
[Kafka Topic: invoice_events]
     ↓
[Apache Camel Route]
     ↓
[AI Decision Engine (DJL/OpenAI)]
     ↓
[Kafka Topic: classified_invoices]
     ↓
[Dashboard / Email Alerts / Storage]
```

---

## 📌 Section 2: Apache Camel Setup in Spring Boot

### ✅ Add dependencies:

```xml
<dependency>
    <groupId>org.apache.camel.springboot</groupId>
    <artifactId>camel-spring-boot-starter</artifactId>
    <version>4.0.0</version>
</dependency>
<dependency>
    <groupId>org.apache.camel.springboot</groupId>
    <artifactId>camel-kafka-starter</artifactId>
    <version>4.0.0</version>
</dependency>
```

---

## 📌 Section 3: Sample Kafka Invoice Event

```json
{
  "invoiceId": "INV-2025-001",
  "amount": 4500,
  "description": "Payment for SaaS services",
  "vendor": "TCS"
}
```

---

## 📌 Section 4: Create Camel Route for Invoice Stream

### ✅ `InvoiceRouteBuilder.java`

```java
@Component
public class InvoiceRouteBuilder extends RouteBuilder {

    @Override
    public void configure() throws Exception {
        from("kafka:invoice_events?brokers=localhost:9092")
            .routeId("invoice-stream-route")
            .unmarshal().json(JsonLibrary.Jackson, InvoiceEvent.class)
            .process(new InvoiceProcessor())
            .marshal().json(JsonLibrary.Jackson)
            .to("kafka:classified_invoices?brokers=localhost:9092");
    }
}
```

---

## 📌 Section 5: Create AI Processor Logic

### ✅ `InvoiceProcessor.java`

```java
public class InvoiceProcessor implements Processor {

    private final AiModelService aiModelService = new AiModelService(); // use DI if needed

    @Override
    public void process(Exchange exchange) throws Exception {
        InvoiceEvent invoice = exchange.getIn().getBody(InvoiceEvent.class);

        String decision = aiModelService.classify(invoice);
        invoice.setCategory(decision); // e.g., "Software", "Hardware", "Travel"

        exchange.getIn().setBody(invoice);
    }
}
```

---

## 📌 Section 6: Build AI Model Classifier

### ✅ `AiModelService.java`

```java
public class AiModelService {
    public String classify(InvoiceEvent invoice) {
        // Simple logic for demo. Replace with DJL/OpenAI-based classifier
        if (invoice.getDescription().toLowerCase().contains("software")) {
            return "Software";
        } else if (invoice.getDescription().toLowerCase().contains("travel")) {
            return "Travel";
        } else {
            return "General";
        }
    }
}
```

---

## 📌 Section 7: Result – Smart Output to Dashboard

You can now:

* Consume `classified_invoices` from Kafka
* Store in DB or display in Angular UI
* Trigger email/SMS notifications
* Use decision scores for reconciliation/approvals

---

## ✅ Checklist for Phase 5 Completion

| Task                                | Status     |
| ----------------------------------- | ---------- |
| Kafka topic setup for business data | ✅          |
| Apache Camel route configured       | ✅          |
| AI classification integrated        | ✅          |
| Output sent to new Kafka topic      | ✅          |
| Ready for real business automation  | 🔜 Phase 6 |

---

## 🛠️ Optional Enhancements

| Feature                 | Tool                          |
| ----------------------- | ----------------------------- |
| ✅ AI scoring thresholds | OpenAI fine-tuned model       |
| ✅ Rule-based fallbacks  | Drools or custom Spring logic |
| ✅ Stream persistence    | Kafka Connect + PostgreSQL    |
| ✅ Dashboard             | Angular app or Grafana        |
| ✅ Logging/metrics       | ELK Stack or Prometheus       |
