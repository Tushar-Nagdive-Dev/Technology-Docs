### **Lesson 55 (Continued): Using OnPush Strategy for Performance Optimization**

---

## **3. Using OnPush Strategy for Performance Optimization**

### **3.1 Why OnPush Strategy Boosts Performance**
- **OnPush Strategy**:
  - **Change detection is triggered only** when:
    - **Input properties** change (using `@Input()`).
    - **Events** occur within the component (e.g., click, input).
    - **Change detection is manually triggered** using `ChangeDetectorRef`.
  - **Reduces unnecessary change detection cycles**.
  - **Improves performance** by preventing change detection in **unchanged components**.

- **Performance Gains**:
  - **Prevents unnecessary checks** in the **Change Detection Tree**.
  - **Change detection propagates** only when necessary.
  - **Reduces the performance impact** on **large applications** with **deep component trees**.
  - **Ideal for components** with **immutable inputs**.

---

### **3.2 Implementing OnPush Strategy in Angular Components**

**Example 1: Default Strategy (Without OnPush)**

```typescript
// parent.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-parent',
  template: `
    <h2>Parent Component</h2>
    <button (click)="updateMessage()">Update Message</button>
    <app-child [message]="message"></app-child>
  `
})
export class ParentComponent {
  message = 'Hello from Parent';

  updateMessage() {
    this.message = 'Message Updated';
  }
}
```

```typescript
// child.component.ts
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
    <h3>Child Component</h3>
    <p>{{ message }}</p>
  `
})
export class ChildComponent {
  @Input() message: string;
}
```

- **Default Strategy**:
  - **Change detection** runs on **both Parent and Child components**.
  - **Child component** is checked even if `message` is not changed.
  - **Unnecessary change detection cycles** reduce performance.

---

**Example 2: OnPush Strategy (Optimized)**

```typescript
// child.component.ts (Using OnPush Strategy)
import { Component, Input, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
    <h3>Child Component</h3>
    <p>{{ message }}</p>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ChildComponent {
  @Input() message: string;
}
```

- **OnPush Strategy**:
  - **Change detection** runs **only** when `message` changes.
  - **Child component** is not checked if `message` is the same.
  - **Improves performance** by avoiding unnecessary checks.

---

### **3.3 Input Immutability and OnPush Efficiency**
- **OnPush Strategy** relies on **Input Immutability**:
  - **Change detection** is triggered **only when the reference changes**.
  - **Primitive types**: Trigger change detection on value change.
  - **Objects and Arrays**: Trigger change detection on reference change.

**Example: Input Immutability with OnPush**

```typescript
// parent.component.ts (With OnPush Optimization)
import { Component } from '@angular/core';

@Component({
  selector: 'app-parent',
  template: `
    <h2>Parent Component</h2>
    <button (click)="updateObject()">Update Object</button>
    <app-child [data]="data"></app-child>
  `
})
export class ParentComponent {
  data = { message: 'Hello from Parent' };

  updateObject() {
    // OnPush will not trigger because the reference is not changed
    this.data.message = 'Updated Message';
  }

  updateObjectImmutable() {
    // OnPush will trigger because the reference is changed
    this.data = { message: 'Updated Message' };
  }
}
```

```typescript
// child.component.ts (Using OnPush Strategy)
import { Component, Input, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
    <h3>Child Component</h3>
    <p>{{ data.message }}</p>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ChildComponent {
  @Input() data: { message: string };
}
```

- **Immutability with Objects**:
  - **Change detection is not triggered** if the **reference is not changed**.
  - **Change detection is triggered** when a **new object is assigned**.
  - **Improves performance** by avoiding unnecessary checks.

---

### **3.4 Best Practices for Using OnPush Strategy**
1. **Use OnPush with Immutable Inputs**:
   - Ensure **Input properties are immutable**.
   - **Change detection** is triggered **only when the reference changes**.

2. **Trigger Change Detection Manually if Needed**:
   - Use `ChangeDetectorRef` to **manually trigger change detection**.
   - Example:

```typescript
import { Component, ChangeDetectionStrategy, ChangeDetectorRef } from '@angular/core';

@Component({
  selector: 'app-manual-detection',
  template: `
    <h3>Manual Change Detection</h3>
    <p>{{ message }}</p>
    <button (click)="updateMessage()">Update Message</button>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ManualDetectionComponent {
  message = 'Hello';

  constructor(private cdr: ChangeDetectorRef) {}

  updateMessage() {
    this.message = 'Updated Message';
    this.cdr.detectChanges(); // Manually trigger change detection
  }
}
```

3. **Use markForCheck() to Propagate Changes**:
   - Use `markForCheck()` to **propagate change detection** in **child components**.
   - Example:

```typescript
this.cdr.markForCheck();
```

4. **Combine OnPush with RxJS and async Pipe**:
   - Use **RxJS Observables** with `async` pipe for efficient change detection.
   - `async` pipe **automatically triggers change detection** when data is updated.
   - Example:

```html
<p>{{ observableData$ | async }}</p>
```

---

### **3.5 When Not to Use OnPush Strategy**
- **Avoid OnPush Strategy** if:
  - **Input properties are mutable** and **change frequently**.
  - **Two-way data binding** is used (`[(ngModel)]` or `[(value)]`).
  - **Change detection needs to propagate** to child components.
  - **Dynamic content or template references** are used.

---

## **Next Steps:**
- **Change Detection Optimization Techniques**
  - Avoiding Unnecessary Change Detection Cycles
  - Change Detection Optimization with Angular Directives
  - Manual Change Detection with ChangeDetectorRef
  - Using markForCheck() and detectChanges()

- **Optimizing Change Detection with RxJS and async Pipe**
