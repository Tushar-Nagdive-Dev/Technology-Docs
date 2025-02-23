### **Lesson 65: Advanced Change Detection Patterns with Lazy Loading**

---

## **What You Will Learn:**
1. **Why Use Advanced Patterns for Change Detection?**
   - Understanding Advanced Change Detection Patterns
   - Why Use Advanced Patterns for Performance Optimization?
   - Core Concepts: OnPush Strategy, trackBy, async Pipe, and NgZone
   - Performance Impact of Advanced Change Detection Patterns

2. **Combining trackBy, async Pipe, and OnPush Strategy**
   - Why Combine trackBy, async Pipe, and OnPush Strategy?
   - Efficient Change Detection with trackBy and async Pipe
   - Isolating Change Detection with OnPush Strategy
   - Advanced Pattern for Complex Lists and Asynchronous Data

3. **Conditional Change Detection with ngIf and async Pipe**
   - Why Use Conditional Change Detection with ngIf and async Pipe?
   - Efficient Conditional Rendering with ngIf and async Pipe
   - Preventing Unnecessary Change Detection with Conditional Loading
   - Performance Gains with Conditional Change Detection

4. **Change Detection Optimization with NgZone**
   - What is NgZone in Angular?
   - Why Use NgZone for Change Detection Optimization?
   - Using runOutsideAngular() for Performance Optimization
   - Manually Triggering Change Detection with NgZone

5. **Advanced Change Detection Patterns for Complex Asynchronous Data**
   - Why Use Advanced Patterns for Complex Data?
   - Combining Lazy Loading with trackBy and async Pipe
   - Efficient Change Detection for Nested Components
   - Advanced Patterns for Real-Time Data Streams

6. **Hands-on Exercises:**
   - Implementing Advanced Change Detection Patterns
   - Combining trackBy, async Pipe, and OnPush Strategy
   - Real-World Scenario: Change Detection Optimization for Complex UI

7. **Expert Insights and Best Practices**
8. **Common Mistakes to Avoid**
9. **Recap and Next Steps**

---

## **1. Why Use Advanced Patterns for Change Detection?**

### **1.1 Understanding Advanced Change Detection Patterns**
- **Advanced Change Detection Patterns**:
  - **Combine multiple change detection strategies** for **maximum performance**.
  - **Efficiently manage asynchronous data streams** with **lazy loading**.
  - **Isolate change detection** to **specific components or parts of the DOM**.
  - **Optimize rendering speed** with **conditional change detection**.

- **Why Use Advanced Patterns for Performance Optimization?**:
  - **Prevents unnecessary change detection** for **hidden or off-screen data**.
  - **Efficiently manages complex data structures** and **nested components**.
  - **Improves rendering speed** by **isolating change detection**.
  - **Reduces memory usage** by **destroying unused components**.

---

### **1.2 Core Concepts: OnPush Strategy, trackBy, async Pipe, and NgZone**
- **OnPush Strategy**:
  - **Change detection is triggered** only when:
    - **Input properties change**.
    - **Events are triggered**.
    - **Manually marked for check** with `markForCheck()`.
  - **Prevents unnecessary change detection** for **static data**.
  - **Isolates change detection** to **specific components**.

- **trackBy**:
  - **Tracks list items** by **unique identifiers**.
  - **Efficient change detection** with **ngFor**.
  - **Re-renders only changed items**.

- **async Pipe**:
  - **Subscribes to Observables or Promises**.
  - **Automatically triggers change detection** when data is received.
  - **Automatically unsubscribes** to **prevent memory leaks**.

- **NgZone**:
  - **Angular service** that **controls change detection cycles**.
  - **runOutsideAngular()** is used to **prevent change detection** for **asynchronous tasks**.
  - **Manually triggers change detection** with `run()` or `markForCheck()`.

- **Performance Impact of Advanced Change Detection Patterns**:
  - **Improves rendering speed** by **isolating change detection**.
  - **Reduces memory usage** by **destroying unused components**.
  - **Efficient state management** with **RxJS and async Pipe**.
  - **Prevents unnecessary change detection** for **hidden data**.

---

## **2. Combining trackBy, async Pipe, and OnPush Strategy**

### **2.1 Why Combine trackBy, async Pipe, and OnPush Strategy?**
- **Combining trackBy, async Pipe, and OnPush Strategy**:
  - **Maximizes performance** by **isolating change detection**.
  - **trackBy** optimizes **ngFor rendering**.
  - **async Pipe** **subscribes to Observables** and **automatically triggers change detection**.
  - **OnPush Strategy** **prevents unnecessary checks**.

- **Performance Gains**:
  - **Improves rendering speed** by **isolating change detection**.
  - **Reduces memory usage** by **destroying unused components**.
  - **Efficient state management** with **RxJS and async Pipe**.
  - **Prevents change detection** for **hidden or off-screen data**.

---

### **2.2 Efficient Change Detection with trackBy and async Pipe**

**Example: Combining trackBy, async Pipe, and OnPush Strategy**

```typescript
// advanced-list.component.ts
import { Component, Input, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-advanced-list',
  template: `
    <h3>Advanced List with trackBy, async Pipe, and OnPush</h3>
    <ul>
      <li *ngFor="let item of data$ | async; trackBy: trackById">{{ item.name }}</li>
    </ul>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class AdvancedListComponent {
  @Input() data$: Observable<{ id: number, name: string }[]>;

  trackById(index: number, item: any): number {
    return item.id;
  }
}
```

```typescript
// app.component.ts
import { Component } from '@angular/core';
import { Observable, of } from 'rxjs';
import { delay } from 'rxjs/operators';

@Component({
  selector: 'app-root',
  template: `
    <h2>Advanced Change Detection Example</h2>
    <button (click)="loadData()">Load Data</button>
    <app-advanced-list *ngIf="data$" [data$]="data$"></app-advanced-list>
  `
})
export class AppComponent {
  data$: Observable<{ id: number, name: string }[]>;

  loadData() {
    this.data$ = of([
      { id: 1, name: 'Item 1' },
      { id: 2, name: 'Item 2' },
      { id: 3, name: 'Item 3' }
    ]).pipe(delay(2000));
  }
}
```

- **Explanation**:
  - **trackBy** is used to **track items by their unique `id`**.
  - **async Pipe** **subscribes to the Observable** and **automatically triggers change detection**.
  - **OnPush Strategy** **prevents unnecessary change detection** for **static data**.
  - **Efficient change detection** by **isolating change detection**.

---

### **2.3 Isolating Change Detection with OnPush Strategy**
- **OnPush Strategy**:
  - **Change detection is triggered** only when:
    - **Input properties change**.
    - **Events are triggered**.
    - **Manually marked for check** with `markForCheck()`.

- **Performance Gains**:
  - **Reduces unnecessary checks** for **static data**.
  - **Improves rendering speed** by **isolating change detection**.
  - **Efficient state management** with **RxJS and async Pipe**.

---

## **Next Steps:**
- **Change Detection Optimization with NgZone**
  - Using runOutsideAngular() for Performance Optimization
  - Manually Triggering Change Detection with NgZone
