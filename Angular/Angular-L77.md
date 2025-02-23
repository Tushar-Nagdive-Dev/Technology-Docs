---

## **XI. Angular Lifecycle Hooks** 

---

## **XI.1 Overview of Angular Lifecycle Hooks**

Angular **Lifecycle Hooks** allow developers to:
- **Hook into key moments** in a component's lifecycle.
- **Initialize**, **detect changes**, and **clean up** components.
- **Optimize performance** by managing when certain code executes.

---

### **XI.1.1 Why Use Lifecycle Hooks?**

- **Efficient Change Detection** – Manage change detection cycles.
- **Component Initialization** – Load data or initialize variables.
- **Performance Optimization** – Avoid unnecessary DOM manipulation.
- **Cleanup Operations** – Unsubscribe from Observables and detach event listeners.

---

### **XI.1.2 List of Angular Lifecycle Hooks**

1. **ngOnChanges()** – Called when data-bound input properties change.
2. **ngOnInit()** – Initializes the component after Angular displays the data-bound properties.
3. **ngDoCheck()** – Custom change detection.
4. **ngAfterContentInit()** – Called after Angular projects external content.
5. **ngAfterContentChecked()** – Called after Angular checks the projected content.
6. **ngAfterViewInit()** – Called after Angular initializes component views and child views.
7. **ngAfterViewChecked()** – Called after Angular checks the component views and child views.
8. **ngOnDestroy()** – Cleanup just before Angular destroys the component.

---

---

## **XI.2 ngOnChanges()**

---

### **XI.2.1 What is ngOnChanges()?**

- **ngOnChanges()** is called **before ngOnInit()** and **every time** an input property changes.
- It is triggered by changes to:
  - **@Input() properties** bound to template variables.

---

### **XI.2.2 When to Use ngOnChanges()?**

- **Reacting to Input Changes** – Execute logic when an input property changes.
- **Validating Input Data** – Check input values before processing them.
- **Triggering Side Effects** – Re-calculate dependent values when input changes.

---

### **XI.2.3 How to Use ngOnChanges()?**

- **ngOnChanges(changes: SimpleChanges)**:
  - **SimpleChanges** object contains:
    - `currentValue`: New value of the input property.
    - `previousValue`: Previous value of the input property.
    - `firstChange`: Boolean indicating if this is the first change.

---

### **XI.2.4 Example of ngOnChanges()**

**Example: app.component.ts**

```typescript
import { Component, Input, OnChanges, SimpleChanges } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `<p>Child Component: {{ message }}</p>`
})
export class ChildComponent implements OnChanges {
  @Input() message: string = '';

  ngOnChanges(changes: SimpleChanges): void {
    console.log('ngOnChanges called');
    for (const propName in changes) {
      const change = changes[propName];
      const currentValue = change.currentValue;
      const previousValue = change.previousValue;
      console.log(`Property: ${propName}, Previous: ${previousValue}, Current: ${currentValue}`);
    }
  }
}
```

**Example: parent.component.html**

```html
<h2>Parent Component</h2>
<input [(ngModel)]="parentMessage" placeholder="Enter Message">
<app-child [message]="parentMessage"></app-child>
```

**Example: parent.component.ts**

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-parent',
  templateUrl: './parent.component.html'
})
export class ParentComponent {
  parentMessage: string = 'Hello Child!';
}
```

- **When the input changes**, Angular:
  - Calls **ngOnChanges()** in **ChildComponent**.
  - Logs the **previous and current values** to the console.

---

---

## **XI.3 ngOnInit()**

---

### **XI.3.1 What is ngOnInit()?**

- **ngOnInit()** is called **once** after the first `ngOnChanges()` and **before the view is initialized**.
- It is **only called once** during the component’s lifecycle.

---

### **XI.3.2 When to Use ngOnInit()?**

- **Initialize Properties** – Set up properties required for the template.
- **Fetch Data** – Retrieve data from a service (e.g., HTTP requests).
- **Subscribe to Observables** – Start listening to Observables.
- **Avoid Complex Logic in Constructor** – Use `ngOnInit()` instead of the constructor.

---

### **XI.3.3 How to Use ngOnInit()?**

- **ngOnInit()**:
  - It is part of the **OnInit** lifecycle hook.
  - **No parameters** and **void return type**.

---

### **XI.3.4 Example of ngOnInit()**

**Example: data-fetch.component.ts**

```typescript
import { Component, OnInit } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Component({
  selector: 'app-data-fetch',
  template: `
    <h2>Data Fetch Component</h2>
    <ul>
      <li *ngFor="let user of users">{{ user.name }}</li>
    </ul>
  `
})
export class DataFetchComponent implements OnInit {
  users: any[] = [];

  constructor(private http: HttpClient) {}

  ngOnInit(): void {
    console.log('ngOnInit called');
    this.http.get<any[]>('https://jsonplaceholder.typicode.com/users')
      .subscribe(data => {
        this.users = data;
      });
  }
}
```

- **ngOnInit()** is used to:
  - **Fetch data** from an API.
  - **Initialize the `users` array** with the fetched data.
- **Logs "ngOnInit called"** to the console.

---

---

## **XI.4 ngDoCheck()**

---

### **XI.4.1 What is ngDoCheck()?**

- **ngDoCheck()** is called **during every change detection cycle**.
- It is used for **custom change detection**.
- **Called more frequently** than `ngOnChanges()`.

---

### **XI.4.2 When to Use ngDoCheck()?**

- **Detect Changes Manually** – When Angular’s default change detection doesn’t detect changes.
- **Performance Optimization** – Avoid unnecessary change detection.
- **Custom Change Tracking** – Implement custom change detection logic.

---

### **XI.4.3 How to Use ngDoCheck()?**

- **ngDoCheck()**:
  - Part of the **DoCheck** lifecycle hook.
  - **No parameters** and **void return type**.
  - **Manually detects changes** using custom logic.

---

### **XI.4.4 Example of ngDoCheck()**

**Example: docheck-example.component.ts**

```typescript
import { Component, Input, DoCheck } from '@angular/core';

@Component({
  selector: 'app-docheck-example',
  template: `
    <h2>DoCheck Example</h2>
    <p>Input Property: {{ inputData }}</p>
    <button (click)="updateData()">Change Data</button>
  `
})
export class DoCheckExampleComponent implements DoCheck {
  @Input() inputData: string = '';
  private previousValue: string = '';

  ngDoCheck(): void {
    if (this.inputData !== this.previousValue) {
      console.log('ngDoCheck called - Input changed');
      console.log('Previous:', this.previousValue, 'Current:', this.inputData);
      this.previousValue = this.inputData;
    }
  }

  updateData(): void {
    this.inputData = 'New Value';
  }
}
```

- **ngDoCheck()**:
  - **Manually tracks changes** to `inputData`.
  - Logs **previous and current values** to the console.
- **updateData()**:
  - **Changes the input property** and **triggers ngDoCheck()**.

---

## **Next Steps:**
- **XI.5 ngAfterContentInit() and ngAfterContentChecked()**
- **XI.6 ngAfterViewInit() and ngAfterViewChecked()**
- **XI.7 ngOnDestroy()**

---

## **XI.5 ngAfterContentInit() and ngAfterContentChecked()**

---

## **XI.5.1 ngAfterContentInit()**

---

### **XI.5.1.1 What is ngAfterContentInit()?**

- **ngAfterContentInit()** is called **once** after Angular projects external content into the component’s view.
- **Called only once** during the component’s lifecycle.
- Occurs **after content is projected** using **ng-content**.

---

### **XI.5.1.2 When to Use ngAfterContentInit()?**

- **Access Projected Content** – Interact with content projected using **ng-content**.
- **Initialize Projected Content** – Perform operations after the projected content is initialized.
- **Manipulate DOM Elements** – Access DOM elements of projected content.

---

### **XI.5.1.3 How to Use ngAfterContentInit()?**

- **ngAfterContentInit()**:
  - Part of the **AfterContentInit** lifecycle hook.
  - **No parameters** and **void return type**.
  - Called **only once**.

---

### **XI.5.1.4 Example of ngAfterContentInit()**

**Example: parent.component.html**

```html
<h2>Parent Component</h2>
<app-content-projection>
  <p>This is projected content from the parent component.</p>
</app-content-projection>
```

**Example: content-projection.component.html**

```html
<h2>Content Projection Example</h2>
<ng-content></ng-content>
```

**Example: content-projection.component.ts**

```typescript
import { Component, AfterContentInit, ContentChild, ElementRef } from '@angular/core';

@Component({
  selector: 'app-content-projection',
  templateUrl: './content-projection.component.html'
})
export class ContentProjectionComponent implements AfterContentInit {
  @ContentChild('projectedContent', { static: true }) content: ElementRef;

  ngAfterContentInit(): void {
    console.log('ngAfterContentInit called');
    console.log('Projected Content:', this.content.nativeElement.textContent);
  }
}
```

- **ngAfterContentInit()**:
  - **Accesses and logs the projected content** after initialization.
- **@ContentChild()**:
  - Accesses the **projected content** within the component.

---

---

## **XI.5.2 ngAfterContentChecked()**

---

### **XI.5.2.1 What is ngAfterContentChecked()?**

- **ngAfterContentChecked()** is called **after every check** of the projected content.
- Called **after ngAfterContentInit()** and **every time** Angular checks for changes.
- Occurs **more frequently** than `ngAfterContentInit()`.

---

### **XI.5.2.2 When to Use ngAfterContentChecked()?**

- **Custom Change Detection** – Track changes in the projected content.
- **Performance Optimization** – Avoid unnecessary change detection.
- **Side Effects** – Trigger side effects when content changes.

---

### **XI.5.2.3 How to Use ngAfterContentChecked()?**

- **ngAfterContentChecked()**:
  - Part of the **AfterContentChecked** lifecycle hook.
  - **No parameters** and **void return type**.
  - **Called multiple times** during change detection cycles.

---

### **XI.5.2.4 Example of ngAfterContentChecked()**

**Example: content-checked.component.html**

```html
<h2>Content Checked Example</h2>
<ng-content></ng-content>
<button (click)="changeContent()">Change Content</button>
```

**Example: content-checked.component.ts**

```typescript
import { Component, AfterContentChecked, ContentChild, ElementRef } from '@angular/core';

@Component({
  selector: 'app-content-checked',
  templateUrl: './content-checked.component.html'
})
export class ContentCheckedComponent implements AfterContentChecked {
  @ContentChild('projectedContent', { static: true }) content: ElementRef;
  previousContent: string = '';

  ngAfterContentChecked(): void {
    if (this.content && this.content.nativeElement.textContent !== this.previousContent) {
      console.log('ngAfterContentChecked called - Projected content changed');
      this.previousContent = this.content.nativeElement.textContent;
    }
  }

  changeContent(): void {
    this.content.nativeElement.textContent = 'Updated Projected Content';
  }
}
```

- **ngAfterContentChecked()**:
  - **Detects changes** in the projected content.
  - **Logs changes** to the console.
- **changeContent()**:
  - **Updates the projected content** and triggers `ngAfterContentChecked()`.

---

---

## **XI.6 ngAfterViewInit() and ngAfterViewChecked()**

---

## **XI.6.1 ngAfterViewInit()**

---

### **XI.6.1.1 What is ngAfterViewInit()?**

- **ngAfterViewInit()** is called **once** after Angular initializes the component’s views and child views.
- Occurs **after the first change detection cycle**.
- **Called only once** during the component’s lifecycle.

---

### **XI.6.1.2 When to Use ngAfterViewInit()?**

- **DOM Manipulation** – Access and manipulate DOM elements after view initialization.
- **ViewChild Initialization** – Access child components and directives.
- **Initialize Third-Party Libraries** – Initialize libraries that depend on the DOM.

---

### **XI.6.1.3 How to Use ngAfterViewInit()?**

- **ngAfterViewInit()**:
  - Part of the **AfterViewInit** lifecycle hook.
  - **No parameters** and **void return type**.
  - **Called only once**.

---

### **XI.6.1.4 Example of ngAfterViewInit()**

**Example: view-init.component.html**

```html
<h2>View Init Example</h2>
<p #paragraph>ViewChild Paragraph</p>
```

**Example: view-init.component.ts**

```typescript
import { Component, AfterViewInit, ViewChild, ElementRef } from '@angular/core';

@Component({
  selector: 'app-view-init',
  templateUrl: './view-init.component.html'
})
export class ViewInitComponent implements AfterViewInit {
  @ViewChild('paragraph', { static: false }) paragraph: ElementRef;

  ngAfterViewInit(): void {
    console.log('ngAfterViewInit called');
    this.paragraph.nativeElement.style.color = 'blue';
    console.log('Paragraph Text:', this.paragraph.nativeElement.textContent);
  }
}
```

- **ngAfterViewInit()**:
  - **Accesses and manipulates the DOM element**.
  - **Changes the color** of the paragraph to blue.
- **@ViewChild()**:
  - Accesses the **paragraph element** in the view.

---

---

## **XI.6.2 ngAfterViewChecked()**

---

### **XI.6.2.1 What is ngAfterViewChecked()?**

- **ngAfterViewChecked()** is called **after every check** of the component's views and child views.
- Called **after ngAfterViewInit()** and **every time** Angular checks the view.
- Occurs **more frequently** than `ngAfterViewInit()`.

---

### **XI.6.2.2 When to Use ngAfterViewChecked()?**

- **Custom Change Detection** – Detect and respond to view changes.
- **Performance Optimization** – Avoid unnecessary change detection.
- **Side Effects** – Trigger side effects when view changes.

---

### **XI.6.2.3 How to Use ngAfterViewChecked()?**

- **ngAfterViewChecked()**:
  - Part of the **AfterViewChecked** lifecycle hook.
  - **No parameters** and **void return type**.
  - **Called multiple times** during change detection cycles.

---

## **Next Steps:**
- **XI.7 ngOnDestroy()** – **Cleanup and Memory Management**
- **XI.8 Summary and Best Practices for Lifecycle Hooks**

---

## **XI.7 ngOnDestroy() – Cleanup and Memory Management**

---

## **XI.7.1 What is ngOnDestroy()?**

- **ngOnDestroy()** is called **just before Angular destroys the component or directive**.
- It is triggered when:
  - The **component is removed from the DOM**.
  - **Navigating away** from the route where the component is active.
  - **Conditional rendering** (e.g., using `*ngIf`) removes the component.

---

### **XI.7.2 When to Use ngOnDestroy()?**

- **Unsubscribe from Observables** – To avoid memory leaks.
- **Detach Event Listeners** – Remove event listeners added manually.
- **Clean Up Resources** – Free up resources like intervals or timeouts.
- **Remove References** – Clear references to services or components to allow garbage collection.

---

### **XI.7.3 How to Use ngOnDestroy()?**

- **ngOnDestroy()**:
  - Part of the **OnDestroy** lifecycle hook.
  - **No parameters** and **void return type**.
  - **Called only once** before component destruction.

---

### **XI.7.4 Example of ngOnDestroy()**

**Example: unsubscribe.component.ts**

```typescript
import { Component, OnInit, OnDestroy } from '@angular/core';
import { Subscription, interval } from 'rxjs';

@Component({
  selector: 'app-unsubscribe',
  template: `<h2>Unsubscribe Example</h2>`
})
export class UnsubscribeComponent implements OnInit, OnDestroy {
  private intervalSubscription: Subscription;

  ngOnInit(): void {
    console.log('ngOnInit called');
    this.intervalSubscription = interval(1000).subscribe(count => {
      console.log('Interval Count:', count);
    });
  }

  ngOnDestroy(): void {
    console.log('ngOnDestroy called');
    this.intervalSubscription.unsubscribe();
    console.log('Unsubscribed from Interval');
  }
}
```

- **ngOnInit()**:
  - Starts an **interval Observable** that emits a value every second.
- **ngOnDestroy()**:
  - **Unsubscribes** from the interval Observable.
  - **Prevents memory leaks** by ensuring the interval is cleared.

---

### **XI.7.5 Example of Detaching Event Listeners**

**Example: event-listener.component.ts**

```typescript
import { Component, OnInit, OnDestroy, Renderer2, ElementRef } from '@angular/core';

@Component({
  selector: 'app-event-listener',
  template: `<button #myButton>Click Me</button>`
})
export class EventListenerComponent implements OnInit, OnDestroy {
  private listener: () => void;

  constructor(private renderer: Renderer2, private el: ElementRef) {}

  ngOnInit(): void {
    const button = this.el.nativeElement.querySelector('button');
    this.listener = this.renderer.listen(button, 'click', () => {
      console.log('Button Clicked!');
    });
  }

  ngOnDestroy(): void {
    console.log('ngOnDestroy called');
    this.listener();
    console.log('Event Listener Removed');
  }
}
```

- **ngOnInit()**:
  - **Adds a click event listener** to the button.
- **ngOnDestroy()**:
  - **Removes the event listener** to prevent memory leaks.

---

---

## **XI.8 Summary and Best Practices for Lifecycle Hooks**

---

### **XI.8.1 Summary of Angular Lifecycle Hooks**

| Lifecycle Hook           | Description                                                     | Called |
|---------------------------|-----------------------------------------------------------------|--------|
| **ngOnChanges()**         | Responds to changes in data-bound input properties.              | Multiple times |
| **ngOnInit()**            | Initializes the component after Angular first displays the data. | Once   |
| **ngDoCheck()**           | Custom change detection logic.                                  | Multiple times |
| **ngAfterContentInit()**  | Called once after projecting content into the component's view.  | Once   |
| **ngAfterContentChecked()** | Called after every check of projected content.                | Multiple times |
| **ngAfterViewInit()**     | Called once after initializing the component's views.            | Once   |
| **ngAfterViewChecked()**  | Called after every check of the component's views.               | Multiple times |
| **ngOnDestroy()**         | Cleanup just before the component is destroyed.                  | Once   |

---

### **XI.8.2 Best Practices for Using Lifecycle Hooks**

1. **Avoid Heavy Logic in ngOnInit()**:
   - Use `ngOnInit()` for **initialization logic only**.
   - Avoid heavy calculations or long-running tasks.

2. **Optimize Change Detection with ngDoCheck()**:
   - Use **ngDoCheck()** for **custom change detection logic**.
   - Avoid overuse as it is called **frequently**.

3. **Avoid DOM Manipulation in ngOnInit()**:
   - Use **ngAfterViewInit()** for **DOM manipulation** as the view is fully initialized.
   - **Avoid manipulating the DOM** in `ngOnInit()`.

4. **Unsubscribe from Observables in ngOnDestroy()**:
   - Always **unsubscribe from Observables** in `ngOnDestroy()` to prevent memory leaks.
   - Use the **takeUntil** pattern with a **Subject** for complex subscriptions.

5. **Remove Event Listeners in ngOnDestroy()**:
   - Always **remove event listeners** added manually to prevent memory leaks.

6. **Use ngOnChanges() for Input Changes**:
   - Use **ngOnChanges()** to **respond to input property changes**.
   - Avoid using `ngDoCheck()` for input changes as it is called more frequently.

7. **Use ngAfterContentInit() for Projected Content**:
   - Use **ngAfterContentInit()** to **initialize projected content** using **ng-content**.

8. **Use ngAfterViewInit() for ViewChild Initialization**:
   - Access **@ViewChild** and **@ViewChildren** in `ngAfterViewInit()`.

---

---

### **XI.8.3 Common Mistakes to Avoid**

1. **Using ngOnChanges() without @Input()**:
   - `ngOnChanges()` is only triggered by changes to **@Input() properties**.
   - It won't be called if the property is not decorated with **@Input()**.

2. **DOM Manipulation in ngOnInit()**:
   - The DOM is **not fully initialized** in `ngOnInit()`.
   - Use `ngAfterViewInit()` for **DOM manipulation**.

3. **Forgetting to Unsubscribe in ngOnDestroy()**:
   - **Unsubscribing from Observables** is crucial to avoid memory leaks.
   - Always **unsubscribe in ngOnDestroy()** or use **AsyncPipe**.

4. **Infinite Loops in ngDoCheck()**:
   - Avoid **complex change detection logic** that triggers Angular’s change detection repeatedly.
   - **Optimize and minimize** checks in `ngDoCheck()`.

---

---

## **XI.9 Real-World Use Cases for Lifecycle Hooks**

1. **ngOnChanges()** – Validating and responding to changes in input properties.
2. **ngOnInit()** – Fetching data from APIs on component initialization.
3. **ngDoCheck()** – Custom change detection for complex data models.
4. **ngAfterContentInit()** – Initializing projected content from parent components.
5. **ngAfterViewInit()** – Initializing third-party libraries that require the DOM.
6. **ngOnDestroy()** – Unsubscribing from Observables and detaching event listeners.

---

## **XI.10 Conclusion and Next Steps**

- **Lifecycle Hooks** provide **fine-grained control** over component behavior.
- They enable **efficient change detection**, **data fetching**, and **resource cleanup**.
- Proper usage ensures **high performance**, **maintainability**, and **memory management**.

---

## **Next Steps:**
- ✅ **Completed: Angular Lifecycle Hooks** 🎉
- 🚧 **Next: Testing and Debugging in Angular**
  - **Unit Testing with Jasmine and Karma**
  - **End-to-End (E2E) Testing with Cypress**
  - **Mocking Services and HTTP Calls in Tests**
  - **Debugging Angular Applications with Augury**
