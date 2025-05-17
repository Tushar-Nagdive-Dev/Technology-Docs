# 🧩💻 **Phase 6: Fullstack AI Integration with Angular + Spring Boot**

---

## 🎯 **Objective**

To build a seamless frontend for:

* Submitting voice/text prompts
* Displaying AI responses
* Visualizing classifications, recommendations, or decisions
* Real-time updates (polling or WebSocket)
* Modern, macOS-inspired UI

---

## 📌 Section 1: Frontend Setup (Angular)

### ✅ Prerequisites:

* Angular v15+ (v19 preferred)
* Angular Material + Custom SCSS Theme (macOS style)
* Install dependencies:

```bash
ng add @angular/material
npm install bootstrap @angular/flex-layout
```

---

## 📌 Section 2: Create Angular Components

### 📁 Structure:

```
src/app/
├── components/
│   ├── chat-box/
│   ├── voice-prompt/
│   ├── classification-view/
│   └── dashboard/
├── services/
│   └── ai.service.ts
└── app.module.ts
```

---

## 📌 Section 3: Build AI Interaction Service

### ✅ `ai.service.ts`

```ts
@Injectable({ providedIn: 'root' })
export class AiService {
  private apiUrl = 'http://localhost:8080/api';

  constructor(private http: HttpClient) {}

  sendPrompt(prompt: string) {
    return this.http.post<{ response: string }>(`${this.apiUrl}/ai/prompt`, { prompt });
  }

  classifyInvoice(invoice: any) {
    return this.http.post<any>(`${this.apiUrl}/invoice/classify`, invoice);
  }

  getConversation(sessionId: string) {
    return this.http.get<string[]>(`${this.apiUrl}/conversation/${sessionId}`);
  }
}
```

---

## 📌 Section 4: Add ChatBox Component

### ✅ `chat-box.component.html`

```html
<div class="chat-window">
  <div *ngFor="let msg of messages" class="message">{{ msg }}</div>

  <mat-form-field>
    <input matInput placeholder="Ask something..." [(ngModel)]="userPrompt" (keyup.enter)="send()">
  </mat-form-field>
</div>
```

### ✅ `chat-box.component.ts`

```ts
export class ChatBoxComponent {
  userPrompt = '';
  messages: string[] = [];

  constructor(private aiService: AiService) {}

  send() {
    this.aiService.sendPrompt(this.userPrompt).subscribe(res => {
      this.messages.push(`You: ${this.userPrompt}`);
      this.messages.push(`AI: ${res.response}`);
      this.userPrompt = '';
    });
  }
}
```

---

## 📌 Section 5: Add Voice Prompt Integration

Use Web Speech API (already covered in Phase 4):

### ✅ `voice-prompt.component.ts`

```ts
startVoiceInput() {
  const recognition = new webkitSpeechRecognition();
  recognition.lang = 'en-US';

  recognition.onresult = (event) => {
    const text = event.results[0][0].transcript;
    this.aiService.sendPrompt(text).subscribe((res) => {
      this.result = res.response;
    });
  };

  recognition.start();
}
```

---

## 📌 Section 6: Add Classification Dashboard

### ✅ `classification-view.component.html`

```html
<mat-card *ngFor="let inv of invoices">
  <div>Invoice: {{ inv.invoiceId }}</div>
  <div>Amount: ₹{{ inv.amount }}</div>
  <div>Category: {{ inv.category }}</div>
</mat-card>
```

### ✅ `classification-view.component.ts`

```ts
ngOnInit() {
  this.kafkaService.getClassifiedInvoices().subscribe((invs) => {
    this.invoices = invs;
  });
}
```

---

## 📌 Section 7: macOS UI Styling (SCSS)

```scss
.chat-window {
  border-radius: 12px;
  box-shadow: 0 8px 24px rgba(0,0,0,0.1);
  background: #f9f9f9;
  padding: 1rem;
  max-width: 600px;
  margin: auto;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
}
```

---

## ✅ Checklist for Phase 6 Completion

| Task                                   | Status     |
| -------------------------------------- | ---------- |
| AI prompt API integrated in Angular    | ✅          |
| Voice input handled via Web Speech API | ✅          |
| Classification results displayed       | ✅          |
| Clean, macOS-style design applied      | ✅          |
| Real-time updates supported            | 🔜 Phase 7 |

---

## 🧠 Optional Enhancements

| Feature                    | Tool                     |
| -------------------------- | ------------------------ |
| ✅ Charts                   | ng2-charts or ApexCharts |
| ✅ Realtime updates         | WebSockets with RxJS     |
| ✅ Form-to-Prompt converter | Chat ➝ Form suggestions  |
| ✅ Light/dark mode toggle   | Angular Material Theming |
| ✅ Response ratings         | User feedback buttons    |
