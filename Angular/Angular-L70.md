### **Lesson 70: Advanced Patterns for Change Detection Isolation**

---

## **What You Will Learn:**
1. **Why Use Advanced Patterns for Change Detection Isolation?**
   - Understanding Advanced Patterns for Change Detection Isolation
   - Why Use Advanced Patterns for Performance Optimization?
   - Core Concepts: runOutsideAngular(), OnPush Strategy, markForCheck(), detectChanges(), trackBy, and async Pipe
   - Performance Impact of Advanced Change Detection Isolation

2. **Combining runOutsideAngular(), OnPush Strategy, and markForCheck()**
   - Why Combine runOutsideAngular(), OnPush Strategy, and markForCheck()?
   - Efficient Change Detection Isolation with runOutsideAngular()
   - Conditional Change Detection with OnPush and markForCheck()
   - Advanced Pattern for Real-Time Data Streams and Asynchronous Updates

3. **Conditional Change Detection with ngIf, async Pipe, and OnPush**
   - Why Use Conditional Change Detection with ngIf and async Pipe?
   - Efficient Conditional Rendering with ngIf and async Pipe
   - Combining OnPush Strategy with Conditional Change Detection
   - Performance Gains with Conditional Change Detection

4. **Change Detection Optimization with trackBy and async Pipe**
   - Why Use trackBy with Lazy Loaded Data?
   - Efficient Change Detection with trackBy and ngFor
   - Using async Pipe for Efficient State Management
   - Performance Optimization with trackBy and async Pipe

5. **Advanced Patterns for Real-Time Data Streams and Asynchronous Updates**
   - Why Use Advanced Patterns for Real-Time Data Streams?
   - Combining runOutsideAngular(), async Pipe, and ChangeDetectorRef
   - Efficient Change Detection for Nested Components and Asynchronous Data
   - Advanced Patterns for Real-Time Data Streams and WebSocket Subscriptions

6. **Hands-on Exercises:**
   - Implementing Advanced Change Detection Isolation Patterns
   - Combining runOutsideAngular(), OnPush Strategy, and async Pipe
   - Real-World Scenario: Change Detection Isolation for Complex Asynchronous Data

7. **Expert Insights and Best Practices**
8. **Common Mistakes to Avoid**
9. **Recap and Next Steps**

---

## **1. Why Use Advanced Patterns for Change Detection Isolation?**

### **1.1 Understanding Advanced Patterns for Change Detection Isolation**
- **Advanced Change Detection Isolation**:
  - **Combines multiple change detection strategies** for **maximum performance**.
  - **Efficiently manages asynchronous data streams** with **lazy loading**.
  - **Isolates change detection** to **specific components or parts of the DOM**.
  - **Prevents unnecessary change detection** for **hidden or off-screen data**.

- **Why Use Advanced Patterns for Performance Optimization?**:
  - **Prevents change detection** for **static or unchanging data**.
  - **Improves rendering speed** by **reducing change detection cycles**.
  - **Efficiently manages complex data structures** and **nested components**.
  - **Reduces memory usage** by **destroying unused components**.

---

### **1.2 Core Concepts: runOutsideAngular(), OnPush Strategy, markForCheck(), detectChanges(), trackBy, and async Pipe**
- **runOutsideAngular()**:
  - **Executes code outside Angular's zone**.
  - **Prevents change detection** for **asynchronous tasks**.
  - **Isolates change detection** to **specific components**.

- **OnPush Strategy**:
  - **Change detection is triggered** only when:
    - **Input properties change**.
    - **Events are triggered**.
    - **Manually marked for check** with `markForCheck()` or `detectChanges()`.

- **markForCheck()**:
  - **Requests change detection** for the **current component and its children**.
  - **Triggers change detection** during the **next change detection cycle**.
  - **Efficient state management** for **OnPush Strategy**.

- **detectChanges()**:
  - **Triggers immediate change detection** for the **current component and its children**.
  - **Efficient state management** with **isolated change detection**.
  - **Conditional change detection** for **asynchronous updates**.

- **trackBy**:
  - **Tracks list items** by **unique identifiers**.
  - **Efficient change detection** with **ngFor**.
  - **Re-renders only changed items**.

- **async Pipe**:
  - **Subscribes to Observables or Promises**.
  - **Automatically triggers change detection** when data is received.
  - **Automatically unsubscribes** to **prevent memory leaks**.

- **Performance Impact of Advanced Change Detection Isolation**:
  - **Improves rendering speed** by **isolating change detection**.
  - **Reduces memory usage** by **destroying unused components**.
  - **Efficient state management** with **RxJS and async Pipe**.
  - **Prevents unnecessary change detection** for **hidden data**.

---

## **2. Combining runOutsideAngular(), OnPush Strategy, and markForCheck()**

### **2.1 Why Combine runOutsideAngular(), OnPush Strategy, and markForCheck()?**
- **Combining runOutsideAngular(), OnPush Strategy, and markForCheck()**:
  - **Maximizes performance** by **isolating change detection**.
  - **runOutsideAngular()** **prevents change detection** for **asynchronous tasks**.
  - **OnPush Strategy** **prevents unnecessary checks**.
  - **markForCheck()** **requests change detection** during the **next cycle**.

- **Performance Gains**:
  - **Improves rendering speed** by **isolating change detection**.
  - **Reduces memory usage** by **destroying unused components**.
  - **Efficient state management** with **RxJS and async Pipe**.
  - **Prevents change detection** for **hidden or off-screen data**.

---

### **2.2 Efficient Change Detection Isolation with runOutsideAngular() and OnPush**

**Example: Combining runOutsideAngular(), OnPush Strategy, and markForCheck()**

```typescript
// app.component.ts
import { Component, NgZone, ChangeDetectorRef, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <h2>Advanced Change Detection Isolation</h2>
    <button (click)="startTimer()">Start Timer</button>
    <p>Timer: {{ timer }}</p>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class AppComponent {
  timer = 0;

  constructor(private ngZone: NgZone, private cdRef: ChangeDetectorRef) {}

  startTimer() {
    this.ngZone.runOutsideAngular(() => {
      setInterval(() => {
        this.timer++;
        this.cdRef.markForCheck();
      }, 1000);
    });
  }
}
```

- **Explanation**:
  - **runOutsideAngular()** is used to **execute the timer outside Angular's zone**.
  - **OnPush Strategy** is used to **prevent unnecessary change detection**.
  - **markForCheck()** is used to **request change detection** during the **next cycle**.
  - **Change detection is triggered** only when `timer` is updated.
  - **Improves performance** by **isolating change detection**.

---

## **Next Steps:**
- **Angular + Spring Boot Integration**
  - Using Angular with Spring Boot
  - Containerization and Deployment with Docker and Kubernetes
  - Advanced Patterns for Angular and Spring Boot Integration
