### **Lesson 55 (Continued): More Examples on Using OnPush Strategy for Performance Optimization**

---

## **3.6 Advanced Examples of OnPush Strategy in Angular**

### **Example 1: Using OnPush with Nested Components**

**Scenario**:
- **Parent component** passes data to **Child component**.
- **Child component** uses `ChangeDetectionStrategy.OnPush`.
- **Change detection** is triggered **only when Input changes**.

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

- **Explanation**:
  - **OnPush Strategy** is used in **Child Component**.
  - **Change detection** is triggered **only when Input changes**.
  - **No change detection** occurs if `message` is the same.
  - **Performance optimization** by avoiding unnecessary checks.

---

### **Example 2: OnPush with Immutable Inputs**

**Scenario**:
- **Parent component** passes an **immutable object** as Input.
- **OnPush Strategy** is used in **Child component**.
- **Change detection** is triggered **only when reference changes**.

```typescript
// parent.component.ts
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
    // Change detection will NOT trigger because the reference is the same
    this.data.message = 'Updated Message';
  }

  updateObjectImmutable() {
    // Change detection WILL trigger because the reference is changed
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

- **Explanation**:
  - **OnPush Strategy** is used in **Child Component**.
  - **Change detection** is triggered **only when reference changes**.
  - If `data.message` is updated but the **reference is not changed**, no change detection occurs.
  - **Performance optimization** with **immutable inputs**.

---

### **Example 3: OnPush with RxJS and async Pipe**

**Scenario**:
- **Observable data** is used with `async` pipe.
- **OnPush Strategy** is used in the component.
- **Change detection** is triggered **automatically** by `async` pipe.

```typescript
// observable-example.component.ts
import { Component, ChangeDetectionStrategy } from '@angular/core';
import { Observable, interval } from 'rxjs';
import { map } from 'rxjs/operators';

@Component({
  selector: 'app-observable-example',
  template: `
    <h2>Observable Example with OnPush</h2>
    <p>Counter: {{ counter$ | async }}</p>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ObservableExampleComponent {
  counter$: Observable<number>;

  constructor() {
    // Observable emitting counter every second
    this.counter$ = interval(1000).pipe(
      map(value => value + 1)
    );
  }
}
```

- **Explanation**:
  - **OnPush Strategy** is used in **ObservableExampleComponent**.
  - `async` pipe **automatically triggers change detection** when the observable emits new data.
  - **No manual change detection** is required.
  - **Performance optimization** with **RxJS and async pipe**.

---

### **Example 4: Manual Change Detection with ChangeDetectorRef**

**Scenario**:
- **OnPush Strategy** is used.
- **Change detection** is triggered **manually** using `ChangeDetectorRef`.

```typescript
// manual-detection.component.ts
import { Component, ChangeDetectionStrategy, ChangeDetectorRef } from '@angular/core';

@Component({
  selector: 'app-manual-detection',
  template: `
    <h2>Manual Change Detection</h2>
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

- **Explanation**:
  - **OnPush Strategy** is used in **ManualDetectionComponent**.
  - **Change detection** is triggered **manually** using `ChangeDetectorRef.detectChanges()`.
  - **Improves performance** by avoiding automatic checks.
  - **Manual control** over change detection cycles.

---

### **Example 5: Using markForCheck() to Propagate Changes**

**Scenario**:
- **OnPush Strategy** is used.
- **Change detection** is triggered **manually** using `markForCheck()` to **propagate changes**.

```typescript
// mark-for-check.component.ts
import { Component, ChangeDetectionStrategy, ChangeDetectorRef } from '@angular/core';

@Component({
  selector: 'app-mark-for-check',
  template: `
    <h2>markForCheck Example</h2>
    <p>{{ message }}</p>
    <button (click)="triggerChange()">Trigger Change</button>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class MarkForCheckComponent {
  message = 'Hello from markForCheck';

  constructor(private cdr: ChangeDetectorRef) {}

  triggerChange() {
    this.message = 'Message Updated with markForCheck';
    this.cdr.markForCheck(); // Mark the component for check
  }
}
```

- **Explanation**:
  - **OnPush Strategy** is used in **MarkForCheckComponent**.
  - `markForCheck()` **marks the component for change detection**.
  - **Change detection propagates** through child components.
  - **Efficient change detection propagation** with **manual control**.

---

## **Summary and Key Takeaways:**
- **OnPush Strategy** significantly **improves performance** by:
  - **Reducing unnecessary change detection cycles**.
  - **Triggering change detection** only on **Input changes**, **events**, or **manual triggers**.
- **Best Practices**:
  - Use **immutable inputs** for better performance.
  - Use `async` pipe with RxJS for **automatic change detection**.
  - Use `ChangeDetectorRef.detectChanges()` for **manual control**.
  - Use `markForCheck()` to **propagate changes** through child components.

---

## **Next Steps:**
- **Change Detection Optimization Techniques**:
  - Avoiding Unnecessary Change Detection Cycles
  - Change Detection Optimization with Angular Directives
  - Manual Change Detection with ChangeDetectorRef
  - Using markForCheck() and detectChanges()
