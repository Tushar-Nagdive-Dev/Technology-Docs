---

## **Lesson 76: Latest Angular Features (Angular 19 and Beyond)**

---

## **What You Will Learn:**
1. **Introduction to Angular 19 and Beyond**
   - What's New in Angular 19?
   - Key Changes and Improvements
   - Why Upgrade to Angular 19?

2. **Signals in Angular**
   - What are Signals?
   - Why Use Signals for State Management?
   - Implementing Signals in Angular Components

3. **Enhanced Router Features**
   - Router Hooks and Observability Improvements
   - Typed Router Events for Type Safety
   - Route Data Preloading and Lazy Loading Enhancements

4. **Performance Improvements in Angular 19**
   - Improved Change Detection with Signals
   - Faster Builds and Smaller Bundle Sizes
   - Enhanced RxJS Integration

5. **Typed Forms and Improved Form Validation**
   - Introduction to Typed Forms
   - Strong Type Checking for Reactive Forms
   - Improved Form Validation with Type Safety

6. **Developer Experience Enhancements**
   - Angular CLI Improvements
   - New Angular DevTools Features
   - Enhanced Error Messages and Debugging

7. **Real-World Project with Angular 19 and Spring Boot**
   - Building a Real-World Application using Angular 19
   - Integrating with Spring Boot Microservices
   - Deploying Angular 19 + Spring Boot on Cloud Platforms

---

## **1. Introduction to Angular 19 and Beyond**

---

### **1.1 What's New in Angular 19?**
Angular 19 introduces **exciting new features** and **performance improvements** aimed at:
- **Enhanced developer productivity**.
- **Improved application performance**.
- **Better state management** with **Signals**.
- **Type safety** improvements in routing and forms.

---

### **1.2 Key Changes and Improvements**

1. **Signals for Reactive State Management**:
   - **Signals** provide a **reactive programming model** for state management.
   - Enables **fine-grained reactivity** and **efficient change detection**.

2. **Enhanced Router Features**:
   - **Typed Router Events** for **type-safe routing**.
   - **Router Hooks** for better control over navigation events.
   - **Route Data Preloading** and **Lazy Loading Enhancements**.

3. **Performance Improvements**:
   - Faster builds with **improved Angular CLI**.
   - Smaller bundle sizes and better **tree-shaking**.
   - Improved **Change Detection** with **Signals**.

4. **Typed Forms**:
   - **Typed Reactive Forms** with **strong type checking**.
   - Enhanced **Form Validation** with type safety.

5. **Developer Experience Enhancements**:
   - Improved **Angular CLI commands** and **DevTools**.
   - **Better error messages** and **debugging tools**.

---

### **1.3 Why Upgrade to Angular 19?**
- **Performance Gains**:
  - Faster change detection with **Signals**.
  - Smaller bundle sizes for **faster loading times**.
- **Developer Productivity**:
  - **Typed Forms** for enhanced type safety and better IDE support.
  - **Typed Router Events** for safer navigation.
- **State Management**:
  - **Signals** provide a more **reactive and predictable state management** solution.
- **Future-Proofing**:
  - Angular 19 lays the groundwork for **future improvements** and **new paradigms**.

---

## **2. Signals in Angular**

---

### **2.1 What are Signals?**
- **Signals** are a new **reactive state management** feature in Angular 19.
- They provide:
  - **Fine-grained reactivity** by tracking state changes at the property level.
  - **Efficient change detection** by updating only affected components.
- Signals are **inspired by reactive programming paradigms** seen in frameworks like **Solid.js** and **Vue.js**.

---

### **2.2 Why Use Signals for State Management?**
- **Improved Performance**:
  - Signals trigger **change detection only for components** affected by state changes.
  - Eliminates unnecessary change detection cycles.
- **Predictable State Flow**:
  - Signals are **declarative and predictable**, leading to fewer bugs.
- **Fine-Grained Reactivity**:
  - Only the components that depend on a particular state are re-rendered.

---

### **2.3 How Signals Work in Angular 19**
- Signals are **observable** by nature.
- **Signal Hooks** are used to **subscribe to state changes**.
- **No need for RxJS** – Signals provide a simpler, more predictable reactive model.

---

### **2.4 Implementing Signals in Angular Components**

---

#### **2.4.1 Example: Using Signals for State Management**

**Step 1: Install Angular 19**

```sh
# Install the latest version of Angular CLI
npm install -g @angular/cli@next

# Create a new Angular 19 project
ng new angular-signals-demo --routing --style=scss

# Navigate to the project directory
cd angular-signals-demo

# Start the development server
ng serve
```

---

**Step 2: Create a Counter Component**

```sh
# Generate a new Counter component
ng generate component components/counter
```

---

**Step 3: Implementing Signals in Counter Component**

**Example: counter.component.ts**

```typescript
import { Component, signal } from '@angular/core';

@Component({
  selector: 'app-counter',
  templateUrl: './counter.component.html',
  styleUrls: ['./counter.component.scss']
})
export class CounterComponent {
  // Create a Signal for Counter State
  counter = signal(0);

  // Increment Counter
  increment() {
    this.counter.update(value => value + 1);
  }

  // Decrement Counter
  decrement() {
    this.counter.update(value => value - 1);
  }
}
```

---

**Example: counter.component.html**

```html
<h2>Counter Example using Signals</h2>
<p>Current Count: {{ counter() }}</p>
<button (click)="increment()">Increment</button>
<button (click)="decrement()">Decrement</button>
```

- **signal(0)** – Initializes a **Signal** with an initial value of `0`.
- **counter()** – Retrieves the **current value** of the Signal.
- **counter.update()** – Updates the state and triggers **reactive change detection**.
- **Only the affected part of the DOM is re-rendered**, improving performance.

---

#### **2.4.2 Signals with Derived State**

**Example: Using Derived Signals**

```typescript
// Derived Signal for Double Counter
doubleCounter = this.counter.derive(value => value * 2);
```

- **derive()** – Creates a **derived Signal** that automatically updates when the source Signal changes.

---

#### **2.4.3 Signals with Side Effects**

**Example: Using Signal Effects**

```typescript
import { Component, signal, effect } from '@angular/core';

@Component({
  selector: 'app-counter',
  templateUrl: './counter.component.html',
  styleUrls: ['./counter.component.scss']
})
export class CounterComponent {
  counter = signal(0);

  constructor() {
    // Effect triggered whenever counter changes
    effect(() => {
      console.log(`Counter changed to: ${this.counter()}`);
    });
  }

  increment() {
    this.counter.update(value => value + 1);
  }
}
```

- **effect()** – Creates a **side effect** that executes whenever the Signal value changes.
- **console.log** – Logs the current value of `counter()` whenever it changes.

---

## **Next Steps:**
- **3. Enhanced Router Features**
  - Router Hooks and Observability Improvements
  - Typed Router Events for Type Safety
  - Route Data Preloading and Lazy Loading Enhancements

- **4. Performance Improvements in Angular 19**
  - Improved Change Detection with Signals
  - Faster Builds and Smaller Bundle Sizes

- **5. Real-World Project with Angular 19 and Spring Boot**
  - Building a Real-World Application using Angular 19
  - Integrating with Spring Boot Microservices
  - Deploying Angular 19 + Spring Boot on Cloud Platforms

---

## **2.5 Deep Dive into Signals in Angular 19**

---

## **2.5.1 Signals: The Next Generation State Management**

### **What Makes Signals Different from Observables?**
- **Signals** are a **reactive primitive** designed for **fine-grained reactivity**.
- Unlike **RxJS Observables**, Signals:
  - Are **synchronous** by nature.
  - Maintain a **single state** over time.
  - Trigger updates **only for components that depend** on the state.
- They provide a **simpler alternative** to RxJS for state management without **complex subscriptions** and **unsubscriptions**.

---

### **Why Use Signals Over RxJS Observables?**
- **Simpler Syntax**:
  - No need for `.subscribe()` and `.unsubscribe()`.
  - **Synchronous** and **predictable** state flow.
- **Efficient Change Detection**:
  - Angular **automatically tracks dependencies** on Signals.
  - Only **affected components are re-rendered**.
- **Better Performance**:
  - Signals **reduce change detection cycles**.
  - Provide **fine-grained reactivity** compared to `ChangeDetectionStrategy.OnPush`.

---

## **2.5.2 How Signals Work Under the Hood**

### **Core Concepts of Signals:**
1. **Signal**:
   - Holds a **single reactive state**.
   - Can be **read synchronously** using `signal()`.
   - **Triggers change detection** when updated.

2. **Derived Signal**:
   - **Depends on other Signals**.
   - **Automatically updates** when the source Signal changes.
   - Computed using `.derive()` method.

3. **Signal Effect**:
   - **Runs side effects** when a Signal changes.
   - Similar to **Angular Lifecycle Hooks** but **reactive**.
   - Executed **synchronously** when the dependency changes.

---

### **Signals vs. Observables: Key Differences**

| Feature             | Signals                            | Observables (RxJS)                  |
|---------------------|------------------------------------|-------------------------------------|
| **Reactivity**      | Fine-grained reactivity             | Push-based reactivity               |
| **Execution**       | Synchronous                        | Asynchronous                        |
| **Subscriptions**   | Not required                       | Requires `.subscribe()`              |
| **Unsubscribing**   | Not required                       | Required to prevent memory leaks     |
| **Change Detection**| Automatic, fine-grained             | Manual with `.subscribe()`           |
| **State Management**| Built-in with `.update()` and `.set()`| Requires external state management libraries|

---

### **2.5.3 Creating Signals and Updating State**

#### **Basic Usage of Signals**

```typescript
import { Component, signal } from '@angular/core';

@Component({
  selector: 'app-signal-example',
  template: `
    <h2>Signal Example</h2>
    <p>Current Count: {{ counter() }}</p>
    <button (click)="increment()">Increment</button>
    <button (click)="decrement()">Decrement</button>
  `,
  styleUrls: ['./signal-example.component.scss']
})
export class SignalExampleComponent {
  // Create a Signal with an initial value of 0
  counter = signal(0);

  // Increment Counter
  increment() {
    this.counter.update(value => value + 1);
  }

  // Decrement Counter
  decrement() {
    this.counter.update(value => value - 1);
  }
}
```

- **signal(0)**:
  - Initializes a **Signal** with the value `0`.
- **counter()**:
  - **Reads** the current value of the Signal.
- **counter.update()**:
  - **Updates** the Signal and triggers **change detection**.

---

#### **Using `.set()` and `.update()` Methods**

- **.set(newValue)**:
  - **Directly sets** a new value.
  - Triggers change detection.

- **.update(callback)**:
  - **Updates the value** using a callback function.
  - Passes the **current value** to the callback.

**Example: Using `.set()` and `.update()`**

```typescript
reset() {
  // Set counter to 0 directly
  this.counter.set(0);
}

double() {
  // Double the counter value using update
  this.counter.update(value => value * 2);
}
```

- **.set(0)** – Resets the counter to `0`.
- **.update(value => value * 2)** – Doubles the current counter value.

---

## **2.5.4 Using Derived Signals**

### **What are Derived Signals?**
- **Derived Signals** are:
  - **Read-only Signals** that depend on other Signals.
  - **Automatically updated** when the source Signals change.
  - Similar to **computed properties** in Vue.js or **selectors** in NgRx.

### **When to Use Derived Signals?**
- **Computed State**:
  - When state depends on other Signals.
  - Example: `doubleCounter` depends on `counter`.

- **Performance Optimization**:
  - Avoids unnecessary calculations.
  - Only recalculates when dependencies change.

---

### **Example: Using Derived Signals**

```typescript
import { Component, signal } from '@angular/core';

@Component({
  selector: 'app-derived-signal',
  template: `
    <h2>Derived Signal Example</h2>
    <p>Counter: {{ counter() }}</p>
    <p>Double Counter: {{ doubleCounter() }}</p>
    <button (click)="increment()">Increment</button>
    <button (click)="decrement()">Decrement</button>
  `,
  styleUrls: ['./derived-signal.component.scss']
})
export class DerivedSignalComponent {
  // Base Signal
  counter = signal(0);

  // Derived Signal
  doubleCounter = this.counter.derive(value => value * 2);

  // Increment Counter
  increment() {
    this.counter.update(value => value + 1);
  }

  // Decrement Counter
  decrement() {
    this.counter.update(value => value - 1);
  }
}
```

- **derive()**:
  - Creates a **Derived Signal** that doubles the `counter` value.
  - Updates automatically when `counter` changes.
- **doubleCounter()**:
  - Reads the value of the **Derived Signal**.
- **No manual subscriptions** or recalculations are needed.

---

## **2.5.5 Using Signal Effects**

### **What are Signal Effects?**
- **Signal Effects**:
  - **Run side effects** when a Signal changes.
  - Used for:
    - **Logging state changes**.
    - **Triggering HTTP requests**.
    - **Performing calculations** or **updating DOM**.

- Unlike `useEffect` in React or `Effect` in NgRx, **Signal Effects**:
  - Are **synchronous**.
  - Are **automatically cleaned up**.

---

### **Example: Using Signal Effects**

```typescript
import { Component, signal, effect } from '@angular/core';

@Component({
  selector: 'app-signal-effect',
  template: `
    <h2>Signal Effect Example</h2>
    <p>Counter: {{ counter() }}</p>
    <button (click)="increment()">Increment</button>
  `,
  styleUrls: ['./signal-effect.component.scss']
})
export class SignalEffectComponent {
  counter = signal(0);

  constructor() {
    // Effect triggered whenever counter changes
    effect(() => {
      console.log(`Counter changed to: ${this.counter()}`);
    });
  }

  increment() {
    this.counter.update(value => value + 1);
  }
}
```

- **effect()**:
  - Creates a **Signal Effect** that logs the counter value.
  - Runs **synchronously** whenever the Signal changes.
- **No manual subscriptions** or cleanup needed.

---

## **Next Steps:**
- **3. Enhanced Router Features**
  - Router Hooks and Observability Improvements
  - Typed Router Events for Type Safety
  - Route Data Preloading and Lazy Loading Enhancements

---

## **Lesson 76 (Continued): Enhanced Router Features in Angular 19**

---

## **3. Enhanced Router Features**

In this section, we will explore the **new routing features** introduced in Angular 19, including:
- **Router Hooks** for fine-grained navigation control.
- **Typed Router Events** for enhanced type safety.
- **Route Data Preloading and Lazy Loading Enhancements** for improved performance.
- **Real-World Examples** of how to use these features effectively.

---

### **3.1 Router Hooks and Observability Improvements**

Angular 19 introduces **Router Hooks** for:
- **Fine-grained control** over navigation events.
- **Observing and modifying navigation flows**.
- **Replacing deprecated lifecycle hooks** like `ngOnInit` and `ngOnDestroy`.

---

### **3.1.1 What are Router Hooks?**
- **Router Hooks** allow you to:
  - **Listen to router events** more precisely.
  - **Modify navigation flows** before route activation.
  - **Handle navigation errors and redirects** more elegantly.
- They provide a **more flexible way** to handle routing logic compared to traditional guards like `CanActivate` and `CanDeactivate`.

---

### **3.1.2 Types of Router Hooks in Angular 19**

1. **BeforeRouteEnter** – Executed before the route is entered.
2. **AfterRouteEnter** – Executed after the route is fully loaded.
3. **BeforeRouteLeave** – Executed before leaving the current route.
4. **AfterRouteLeave** – Executed after leaving the current route.

---

### **3.1.3 Implementing Router Hooks**

**Example: Using BeforeRouteEnter and BeforeRouteLeave**

**Step 1: Import Router Hooks**

```typescript
import { Component } from '@angular/core';
import { Router, NavigationStart, NavigationEnd } from '@angular/router';
import { filter } from 'rxjs/operators';
```

---

**Step 2: Implementing BeforeRouteEnter Hook**

**Example: before-route-enter.component.ts**

```typescript
@Component({
  selector: 'app-before-route-enter',
  template: `
    <h2>Before Route Enter Example</h2>
    <p>Navigation status: {{ navigationStatus }}</p>
  `,
  styleUrls: ['./before-route-enter.component.scss']
})
export class BeforeRouteEnterComponent {
  navigationStatus = 'Not started';

  constructor(private router: Router) {
    // BeforeRouteEnter Hook
    this.router.events
      .pipe(filter(event => event instanceof NavigationStart))
      .subscribe(() => {
        console.log('Navigation started');
        this.navigationStatus = 'Navigation started';
      });
  }
}
```

- **NavigationStart**:
  - Triggers when navigation **starts**.
  - Used to **execute logic before entering the route**.

---

**Step 3: Implementing BeforeRouteLeave Hook**

**Example: before-route-leave.component.ts**

```typescript
@Component({
  selector: 'app-before-route-leave',
  template: `
    <h2>Before Route Leave Example</h2>
    <p>Navigation status: {{ navigationStatus }}</p>
  `,
  styleUrls: ['./before-route-leave.component.scss']
})
export class BeforeRouteLeaveComponent {
  navigationStatus = 'Active';

  constructor(private router: Router) {
    // BeforeRouteLeave Hook
    this.router.events
      .pipe(filter(event => event instanceof NavigationEnd))
      .subscribe(() => {
        console.log('Navigation ended');
        this.navigationStatus = 'Navigation ended';
      });
  }
}
```

- **NavigationEnd**:
  - Triggers when navigation **ends**.
  - Used to **execute logic before leaving the route**.

---

### **3.1.4 Registering Routes for Router Hooks**

**Example: app-routing.module.ts**

```typescript
const routes: Routes = [
  { path: 'before-enter', component: BeforeRouteEnterComponent },
  { path: 'before-leave', component: BeforeRouteLeaveComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}
```

- **before-enter** – Executes `BeforeRouteEnter` hook.
- **before-leave** – Executes `BeforeRouteLeave` hook.

---

### **3.1.5 Real-World Use Cases for Router Hooks**
1. **Authentication and Authorization**:
   - Use **BeforeRouteEnter** to check user roles and permissions.
   - Redirect unauthorized users before entering the route.

2. **Data Loading and Pre-Fetching**:
   - Use **BeforeRouteEnter** to pre-load route data.
   - Optimize performance by fetching data before route activation.

3. **Analytics and Tracking**:
   - Use **AfterRouteEnter** to log page views for analytics.
   - Trigger custom events for tracking user navigation.

4. **Confirmation Dialogs**:
   - Use **BeforeRouteLeave** to show confirmation dialogs.
   - Prevent navigation if the user has unsaved changes.

---

## **3.2 Typed Router Events for Type Safety**

---

### **3.2.1 What are Typed Router Events?**
- **Typed Router Events** provide **type safety** for navigation events.
- Ensures **type-safe access** to event properties.
- **Improves maintainability and IDE support**.

---

### **3.2.2 Using Typed Router Events**

**Example: Using Typed NavigationStart and NavigationEnd**

```typescript
import { Component } from '@angular/core';
import { Router, NavigationStart, NavigationEnd } from '@angular/router';
import { filter } from 'rxjs/operators';

@Component({
  selector: 'app-typed-router-events',
  template: `
    <h2>Typed Router Events</h2>
    <p>Current URL: {{ currentUrl }}</p>
  `,
  styleUrls: ['./typed-router-events.component.scss']
})
export class TypedRouterEventsComponent {
  currentUrl: string = '';

  constructor(private router: Router) {
    // Typed NavigationStart
    this.router.events
      .pipe(filter((event): event is NavigationStart => event instanceof NavigationStart))
      .subscribe((event: NavigationStart) => {
        console.log('NavigationStart:', event.url);
      });

    // Typed NavigationEnd
    this.router.events
      .pipe(filter((event): event is NavigationEnd => event instanceof NavigationEnd))
      .subscribe((event: NavigationEnd) => {
        console.log('NavigationEnd:', event.urlAfterRedirects);
        this.currentUrl = event.urlAfterRedirects;
      });
  }
}
```

- **Typed NavigationStart and NavigationEnd**:
  - Provide **type-safe access** to event properties:
    - `NavigationStart.url`
    - `NavigationEnd.urlAfterRedirects`
- **event is NavigationStart**:
  - **Type guard** to ensure the event is of type `NavigationStart`.

---

### **3.2.3 Why Use Typed Router Events?**
- **Type Safety**:
  - Ensures compile-time type checking.
  - Reduces runtime errors due to incorrect property access.
- **Enhanced IDE Support**:
  - Provides **intelligent autocompletion** and **type hints**.
- **Maintainable Code**:
  - Improves readability and maintainability with **explicit typing**.

---

## **3.3 Route Data Preloading and Lazy Loading Enhancements**

---

### **3.3.1 Route Data Preloading**

- **Route Data Preloading** allows:
  - **Preloading route data** before activation.
  - **Improved performance** by loading data in the background.
- Useful for:
  - **SEO optimization** with Angular Universal.
  - **Enhanced user experience** with faster route transitions.

---

### **3.3.2 Lazy Loading Enhancements**

- Angular 19 introduces:
  - **Improved Lazy Loading** with **Named Chunks**.
  - **Faster route transitions** with enhanced preloading strategies.
- **Dynamic Imports** with `import()`:
  - Enables **on-demand loading** of Angular modules.
  - Reduces **initial bundle size** for faster loading times.

---

## **Next Steps:**
- **4. Performance Improvements in Angular 19**
  - Improved Change Detection with Signals
  - Faster Builds and Smaller Bundle Sizes
  - Enhanced RxJS Integration

- **5. Real-World Project with Angular 19 and Spring Boot**
  - Building a Real-World Application using Angular 19
  - Integrating with Spring Boot Microservices
  - Deploying Angular 19 + Spring Boot on Cloud Platforms

---

## **Lesson 76 (Continued): Performance Improvements in Angular 19**

---

## **4. Performance Improvements in Angular 19**

In this section, we will explore the **performance enhancements** introduced in Angular 19, including:
- **Improved Change Detection with Signals** for fine-grained reactivity.
- **Faster Builds and Smaller Bundle Sizes** using advanced tree-shaking.
- **Enhanced RxJS Integration** for more efficient state management.
- **Real-World Examples** demonstrating how to use these optimizations.

---

### **4.1 Improved Change Detection with Signals**

---

### **4.1.1 How Change Detection Works in Angular**

- **Change Detection** is the process of:
  - **Tracking state changes** and updating the DOM accordingly.
  - **Detecting data changes** in components and services.
- In previous versions, Angular used:
  - **Zone.js** to detect changes automatically.
  - **ChangeDetectionStrategy.Default** which checks **every component** in the tree.
  - **ChangeDetectionStrategy.OnPush** to **optimize performance** by checking components only when inputs change.

---

### **4.1.2 Limitations of Default Change Detection**

- **Default Change Detection**:
  - **Traverses the entire component tree**, leading to **performance bottlenecks**.
  - Triggers change detection **even when unrelated state changes**.
  - Relies heavily on **Zone.js**, which wraps browser APIs like `setTimeout`, `setInterval`, and `XHR`.

---

### **4.1.3 How Signals Improve Change Detection**

- **Signals** in Angular 19:
  - **Track dependencies** at a granular level.
  - **Trigger change detection** only for components that depend on the changed state.
  - Eliminate the need for **Zone.js** in most cases.
  - Provide a **reactive programming model** that is **synchronous** and **predictable**.

### **Why Use Signals for Change Detection?**
- **Fine-grained Reactivity**:
  - Only **affected components are updated**.
  - Reduces unnecessary DOM updates and improves performance.
- **Predictable State Flow**:
  - Changes are **synchronous** and **explicit**.
  - Eliminates **async bugs** caused by Zone.js.
- **Better Performance**:
  - Eliminates **global change detection cycles**.
  - **No need for manual change detection** with `ChangeDetectorRef`.

---

### **4.1.4 Using Signals for Fine-Grained Change Detection**

---

#### **Example: Fine-Grained Change Detection with Signals**

**Step 1: Create a Signal-Based Counter Component**

**Example: counter-signals.component.ts**

```typescript
import { Component, signal } from '@angular/core';

@Component({
  selector: 'app-counter-signals',
  template: `
    <h2>Counter with Signals</h2>
    <p>Current Count: {{ counter() }}</p>
    <button (click)="increment()">Increment</button>
    <button (click)="decrement()">Decrement</button>
  `,
  styleUrls: ['./counter-signals.component.scss']
})
export class CounterSignalsComponent {
  // Signal with Initial Value
  counter = signal(0);

  // Increment Counter
  increment() {
    this.counter.update(value => value + 1);
  }

  // Decrement Counter
  decrement() {
    this.counter.update(value => value - 1);
  }
}
```

- **signal(0)**:
  - Creates a **Signal** with an initial value of `0`.
- **counter()**:
  - **Reads the current value** of the Signal.
- **counter.update()**:
  - **Updates the value** and triggers **change detection** only for this component.

---

### **4.1.5 Fine-Grained Dependency Tracking**

- **Angular 19** automatically tracks **component dependencies** on Signals.
- **Only components that depend** on the changed Signal are re-rendered.
- **No need for manual change detection** using `ChangeDetectorRef`.

**Example: Fine-Grained Dependency Tracking**

```typescript
<p *ngIf="counter() > 0">Positive Count: {{ counter() }}</p>
<p *ngIf="counter() < 0">Negative Count: {{ counter() }}</p>
```

- **Positive Count** is only updated when `counter()` becomes positive.
- **Negative Count** is only updated when `counter()` becomes negative.

---

### **4.1.6 Using Signals with OnPush Strategy**

- **ChangeDetectionStrategy.OnPush** combined with Signals:
  - Ensures that the component is **only checked** when:
    - **Input properties** change.
    - **Signal values** change.
- This provides **maximum performance optimization**.

**Example: Using OnPush with Signals**

```typescript
@Component({
  selector: 'app-counter-signals-onpush',
  templateUrl: './counter-signals-onpush.component.html',
  styleUrls: ['./counter-signals-onpush.component.scss'],
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class CounterSignalsOnPushComponent {
  counter = signal(0);

  increment() {
    this.counter.update(value => value + 1);
  }
}
```

- **ChangeDetectionStrategy.OnPush**:
  - Combined with **Signals** for **fine-grained change detection**.
  - The component is **only checked** when `counter()` changes.

---

## **4.2 Faster Builds and Smaller Bundle Sizes**

---

### **4.2.1 Advanced Tree Shaking and Dead Code Elimination**

- **Tree Shaking**:
  - Removes **unused code** during the build process.
  - **Angular 19** improves tree shaking by:
    - Analyzing **import paths** more efficiently.
    - **Eliminating unused RxJS operators**.
- **Dead Code Elimination**:
  - Removes **unreachable code paths**.
  - Reduces **bundle sizes** and improves **loading times**.

---

### **4.2.2 How Angular 19 Optimizes Bundle Sizes**

1. **Better ES Module Analysis**:
   - **Angular CLI** now analyzes ES modules more effectively.
   - **Unused imports** are eliminated during build time.
2. **Enhanced RxJS Tree Shaking**:
   - Only includes **used RxJS operators** in the final bundle.
   - Reduces bundle sizes significantly in apps using RxJS.
3. **Smaller Polyfills**:
   - Only necessary polyfills are included.
   - Reduces bundle size for modern browsers.

---

### **4.2.3 Configuring Angular CLI for Optimized Builds**

**Example: angular.json Configuration**

```json
"configurations": {
  "production": {
    "optimization": {
      "scripts": true,
      "styles": true
    },
    "sourceMap": false,
    "extractLicenses": true,
    "outputHashing": "all",
    "namedChunks": false
  }
}
```

- **optimization.scripts** – Minifies and tree-shakes JavaScript bundles.
- **outputHashing: "all"** – Enables **cache busting** by hashing output filenames.
- **namedChunks: false** – Reduces bundle size by using short chunk names.

---

### **4.2.4 Faster Builds with Angular CLI Improvements**

- **Faster Incremental Builds**:
  - Angular 19 improves **incremental builds** with **faster recompilation**.
- **Improved Build Caching**:
  - Caches **unchanged modules** for faster rebuilds.
- **Reduced Compilation Time**:
  - Optimizes the **Angular Compiler (Ivy)** for faster compilation.

---

### **4.2.5 Build Performance Best Practices**

1. **Use Production Mode**:
   ```sh
   ng build --prod
   ```
2. **Enable AOT Compilation**:
   ```sh
   ng build --aot
   ```
   - **Ahead-of-Time (AOT)** compilation reduces runtime overhead.
3. **Lazy Load Modules**:
   - Use `loadChildren` for **Lazy Loading** modules.
4. **Analyze Bundle Sizes**:
   ```sh
   ng build --prod --stats-json
   npx source-map-explorer dist/main.*.js
   ```
   - Use **source-map-explorer** to analyze bundle sizes.

---

## **Next Steps:**
- **5. Typed Forms and Improved Form Validation**
  - Strong Type Checking for Reactive Forms
  - Improved Form Validation with Type Safety
  - Real-World Examples with Advanced Validation

---

## **Lesson 76 (Continued): Typed Forms and Improved Form Validation**

---

## **5. Typed Forms and Improved Form Validation**

In this section, we will explore the **new Typed Forms** introduced in Angular 19, including:
- **Strong Type Checking** for Reactive Forms.
- **Improved Form Validation** with Type Safety.
- **Advanced Validation Techniques** for complex scenarios.
- **Real-World Examples** demonstrating how to use Typed Forms effectively.

---

### **5.1 Introduction to Typed Forms**

---

### **5.1.1 What are Typed Forms?**
- **Typed Forms** in Angular 19 provide:
  - **Strong type checking** for Reactive Forms.
  - **Compile-time safety** with improved autocompletion in IDEs.
  - **Type-safe access** to form control values and errors.
- They enable:
  - **Type inference** for form values.
  - **Type-safe validation** and error handling.
  - **Better maintainability** and **fewer runtime errors**.

---

### **5.1.2 Why Use Typed Forms?**
- **Type Safety**:
  - Ensures **compile-time checks** for form values.
  - Prevents **type-related runtime errors**.
- **Enhanced IDE Support**:
  - **Intelligent autocompletion** and **type hints** in IDEs.
  - Improves **developer productivity** and **code maintainability**.
- **Improved Validation**:
  - **Type-safe validators** with better error handling.
  - **Type inference** for form errors.

---

### **5.1.3 Typed Forms vs. Traditional Forms**

| Feature              | Typed Forms (Angular 19)           | Traditional Forms                   |
|----------------------|-----------------------------------|-------------------------------------|
| **Type Checking**    | Strong type checking at compile-time| No type checking (implicit any type)|
| **Type Safety**      | Type-safe access to values and errors| Prone to type-related runtime errors|
| **IDE Support**      | Enhanced autocompletion and hints  | Limited type hints and autocompletion|
| **Validation**       | Type-safe validation and error handling| Manual type checks and validations  |
| **Maintainability**  | Easier to maintain and refactor    | Harder to maintain with complex forms|

---

## **5.2 Setting Up Typed Reactive Forms**

---

### **5.2.1 Enabling Typed Forms in Angular 19**

- Typed Forms are **enabled by default** in Angular 19 for **Reactive Forms**.
- Ensure that you have Angular 19 installed:
```sh
# Install the latest version of Angular CLI
npm install -g @angular/cli@next

# Create a new Angular 19 project
ng new angular-typed-forms-demo --routing --style=scss

# Navigate to the project directory
cd angular-typed-forms-demo

# Install Reactive Forms Module
npm install @angular/forms

# Start the development server
ng serve
```

---

### **5.2.2 Importing Reactive Forms Module**

**Example: app.module.ts**

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { ReactiveFormsModule } from '@angular/forms';
import { AppComponent } from './app.component';
import { UserFormComponent } from './components/user-form/user-form.component';

@NgModule({
  declarations: [
    AppComponent,
    UserFormComponent
  ],
  imports: [
    BrowserModule,
    ReactiveFormsModule  // Import ReactiveFormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

- **ReactiveFormsModule**:
  - Enables **Reactive Forms** with **Typed Forms** support.

---

## **5.3 Building a Typed Reactive Form**

---

### **5.3.1 Defining Typed Form Model**

**Example: User Model (TypeScript Interface)**

```typescript
// src/app/models/user.model.ts
export interface User {
  firstName: string;
  lastName: string;
  email: string;
  age: number;
}
```

- Defines the **User model** with:
  - `firstName`: **string**
  - `lastName`: **string**
  - `email`: **string**
  - `age`: **number**

---

### **5.3.2 Creating Typed Form Group**

**Example: user-form.component.ts**

```typescript
import { Component } from '@angular/core';
import { FormBuilder, Validators, FormGroup } from '@angular/forms';
import { User } from '../../models/user.model';

@Component({
  selector: 'app-user-form',
  templateUrl: './user-form.component.html',
  styleUrls: ['./user-form.component.scss']
})
export class UserFormComponent {
  // Typed Form Group
  userForm: FormGroup<User> = this.fb.group<User>({
    firstName: this.fb.control('', { nonNullable: true, validators: [Validators.required] }),
    lastName: this.fb.control('', { nonNullable: true, validators: [Validators.required] }),
    email: this.fb.control('', { nonNullable: true, validators: [Validators.required, Validators.email] }),
    age: this.fb.control(0, { nonNullable: true, validators: [Validators.required, Validators.min(18)] })
  });

  constructor(private fb: FormBuilder) {}

  // Submit Form
  onSubmit() {
    if (this.userForm.valid) {
      const user: User = this.userForm.getRawValue();
      console.log('User:', user);
    } else {
      console.log('Form is invalid');
    }
  }
}
```

- **FormGroup<User>**:
  - **Typed FormGroup** with type `User`.
  - Provides **type-safe access** to form values.
- **this.fb.control('', { nonNullable: true })**:
  - **Non-nullable control** for strict type checking.
- **getRawValue()**:
  - Returns the **typed form value** as `User`.

---

### **5.3.3 Creating User Form Template**

**Example: user-form.component.html**

```html
<h2>Typed User Form</h2>
<form [formGroup]="userForm" (ngSubmit)="onSubmit()">
  <div>
    <label>First Name:</label>
    <input formControlName="firstName">
    <span *ngIf="userForm.controls.firstName.errors?.required">First Name is required</span>
  </div>

  <div>
    <label>Last Name:</label>
    <input formControlName="lastName">
    <span *ngIf="userForm.controls.lastName.errors?.required">Last Name is required</span>
  </div>

  <div>
    <label>Email:</label>
    <input formControlName="email" type="email">
    <span *ngIf="userForm.controls.email.errors?.required">Email is required</span>
    <span *ngIf="userForm.controls.email.errors?.email">Invalid email format</span>
  </div>

  <div>
    <label>Age:</label>
    <input formControlName="age" type="number">
    <span *ngIf="userForm.controls.age.errors?.required">Age is required</span>
    <span *ngIf="userForm.controls.age.errors?.min">Age must be at least 18</span>
  </div>

  <button type="submit" [disabled]="userForm.invalid">Submit</button>
</form>
```

- **formControlName**:
  - Binds input elements to **typed form controls**.
- **userForm.controls.firstName.errors?.required**:
  - Provides **type-safe access** to validation errors.

---

### **5.3.4 Strong Type Checking for Form Controls**

- **Typed Form Controls** provide:
  - **Compile-time type safety** for form values and errors.
  - **Intelligent autocompletion** and **type hints** in IDEs.
- **No type assertions** (`as`) or type guards required.

**Example: Typed Form Control Access**

```typescript
const firstName: string = this.userForm.controls.firstName.value;
const age: number = this.userForm.controls.age.value;
```

- `firstName` is inferred as `string`.
- `age` is inferred as `number`.

---

## **Next Steps:**
- **6. Developer Experience Enhancements**
  - Angular CLI Improvements
  - New Angular DevTools Features
  - Enhanced Error Messages and Debugging

---

## **Lesson 76 (Continued): Developer Experience Enhancements in Angular 19**

---

## **6. Developer Experience Enhancements**

In this section, we will explore the **developer experience improvements** introduced in Angular 19, including:
- **Angular CLI Improvements** for faster development workflows.
- **New Angular DevTools Features** for enhanced debugging and profiling.
- **Enhanced Error Messages and Debugging** for better developer productivity.
- **Real-World Examples** demonstrating how to use these enhancements effectively.

---

### **6.1 Angular CLI Improvements**

---

### **6.1.1 Faster Development Workflows with Angular CLI**

- **Angular CLI** in Angular 19 introduces:
  - **Faster builds** with incremental compilation.
  - **Enhanced watch mode** for faster live reloads.
  - **Better cache management** for faster rebuilds.
  - **New CLI commands** for improved productivity.

---

### **6.1.2 Key Improvements in Angular CLI 19**

1. **Faster Incremental Builds**:
   - Uses **incremental compilation** to recompile only changed modules.
   - **Reduces build times** significantly for large projects.

2. **Improved Watch Mode**:
   - **Faster live reloads** with optimized file watching.
   - Automatically detects changes in:
     - **Angular templates** (`.html` files).
     - **Stylesheets** (`.scss`, `.css` files).
     - **TypeScript files**.

3. **Optimized Caching**:
   - Caches **unchanged modules** for faster rebuilds.
   - Uses **content-hash-based caching** for cache invalidation.

4. **Enhanced Configuration Options**:
   - **Fine-grained control** over build optimizations.
   - Customizable **source maps** and **output hashing**.

---

### **6.1.3 New Angular CLI Commands**

---

#### **ng serve Improvements**

- **ng serve** now supports:
  - **Faster hot module reloading** (HMR) with `--hmr` flag.
  - **Selective rebuilds** using **incremental compilation**.
  - **Live reloading** for Angular templates and styles.

```sh
# Start the development server with HMR enabled
ng serve --hmr
```

- **--hmr**:
  - Enables **Hot Module Replacement (HMR)** for faster live updates.
  - Only the **changed module is reloaded**, preserving component state.

---

#### **ng cache Commands**

- **New caching commands** for **better cache management**:

```sh
# View Angular CLI cache details
ng cache

# Clear Angular CLI cache
ng cache clear
```

- **ng cache**:
  - Displays **cache statistics** and **cache location**.
- **ng cache clear**:
  - Clears the **Angular CLI cache**.
  - Useful for **resolving build inconsistencies**.

---

#### **ng analytics Commands**

- **New analytics commands** for **better privacy control**:

```sh
# Enable Angular CLI analytics
ng analytics on

# Disable Angular CLI analytics
ng analytics off

# View Angular CLI analytics settings
ng analytics info
```

- **ng analytics on** / **off**:
  - Enables or disables **usage analytics**.
- **ng analytics info**:
  - Displays the **current analytics configuration**.

---

#### **ng generate Enhancements**

- **ng generate** now supports:
  - **Typed Forms** with `--typed` flag.
  - **Standalone Components** with `--standalone` flag.

```sh
# Generate a new Typed Form Component
ng generate component user-form --typed

# Generate a new Standalone Component
ng generate component standalone-example --standalone
```

- **--typed**:
  - Generates a **Typed Form Component** with type-safe controls.
- **--standalone**:
  - Generates a **Standalone Component** with its own module and dependencies.

---

### **6.1.4 Using Angular CLI Best Practices**

1. **Enable HMR for Faster Development**:
```sh
ng serve --hmr
```

2. **Optimize Builds for Production**:
```sh
ng build --prod --aot --source-map=false
```

3. **Analyze Bundle Size for Optimization**:
```sh
ng build --prod --stats-json
npx source-map-explorer dist/main.*.js
```

4. **Use Typed Forms and Standalone Components**:
```sh
ng generate component user-form --typed
ng generate component standalone-example --standalone
```

---

## **6.2 New Angular DevTools Features**

---

### **6.2.1 Overview of Angular DevTools**

- **Angular DevTools** is a **browser extension** for:
  - **Debugging Angular applications**.
  - **Profiling performance** and **change detection cycles**.
  - **Inspecting component hierarchies** and **state changes**.
- Available for:
  - **Google Chrome** and **Microsoft Edge** as a browser extension.

---

### **6.2.2 Key Features of Angular DevTools 19**

1. **Change Detection Profiler**:
   - **Visualizes change detection cycles** in real-time.
   - **Identifies performance bottlenecks** caused by change detection.
   - Shows **component-wise change detection timing**.

2. **Signals Inspector**:
   - **New in Angular 19**.
   - Inspects **Signal dependencies** and **reactive state changes**.
   - **Tracks Signal changes** in real-time.
   - **Visualizes Signal dependencies** and change propagation.

3. **Component State Viewer**:
   - **Inspects component state** and input properties.
   - **Tracks state changes** with time travel debugging.
   - **Shows dependency tree** for state management.

4. **Routing Inspector**:
   - Visualizes **Angular Router state**.
   - Shows **active routes**, **lazy-loaded modules**, and **guards**.
   - **Debugs navigation flows** and **route data**.

---

### **6.2.3 Installing Angular DevTools**

- Install the **Angular DevTools extension** for:
  - [Google Chrome](https://chrome.google.com/webstore/detail/angular-devtools)
  - [Microsoft Edge](https://microsoftedge.microsoft.com/addons/detail/angular-devtools)

---

### **6.2.4 Using Change Detection Profiler**

- Open **Angular DevTools** in **Chrome DevTools**:
  - Go to **Profiler** tab.
  - Click on **Start Profiling**.
  - Interact with the Angular app.
  - Click on **Stop Profiling** to visualize change detection cycles.

- **Change Detection Profiler** shows:
  - **Change detection cycles** for each component.
  - **Time taken** for each change detection cycle.
  - **Frequency of change detection**.

---

### **6.2.5 Using Signals Inspector**

- Open **Angular DevTools** in **Chrome DevTools**:
  - Go to **Signals** tab.
  - Interact with the Angular app using Signals.
  - **Visualizes Signal dependencies** and **change propagation**.
  - **Tracks Signal changes** and **reactive state updates**.

---

## **6.3 Enhanced Error Messages and Debugging**

---

### **6.3.1 Improved Error Messages**

- **Angular 19** introduces:
  - **More descriptive error messages** for easier debugging.
  - **Actionable suggestions** with links to Angular documentation.
  - **Stack traces** with improved readability.

---

### **6.3.2 Debugging with Angular DevTools**

1. **Inspect Component Hierarchy**:
   - **Components** tab shows:
     - **Component tree** with input properties.
     - **Component state** and change detection cycles.

2. **Track State Changes**:
   - **Signals** tab tracks:
     - **Signal changes** and **dependencies**.
     - **Reactive state propagation**.

3. **Debugging Routing Issues**:
   - **Router** tab shows:
     - **Active routes** and **route parameters**.
     - **Lazy-loaded modules** and **guards**.

---

## **Next Steps:**
- **7. Real-World Project with Angular 19 and Spring Boot**
  - Building a Real-World Application using Angular 19
  - Integrating with Spring Boot Microservices
  - Deploying Angular 19 + Spring Boot on Cloud Platforms

---

## **Lesson 76 (Continued): Real-World Project with Angular 19 and Spring Boot**

---

## **7. Real-World Project with Angular 19 and Spring Boot**

In this section, we will build a **Real-World Full-Stack Application** using:
- **Angular 19** for the frontend with **Signals** and **Typed Forms**.
- **Spring Boot** for the backend with **RESTful APIs** and **JWT Authentication**.
- **Containerization and Deployment** using **Docker** and **Kubernetes**.
- **CI/CD Integration** using **GitHub Actions**.

---

### **7.1 Project Overview: Expense Management System**

---

### **7.1.1 Project Description**

- We will build a **Full-Stack Expense Management System** that:
  - **Manages expenses, categories, and users**.
  - **Authenticates users** with **JWT-based authentication**.
  - **Authorizes users** with **role-based access control** (`USER` and `ADMIN` roles).
  - **Visualizes expenses** with **interactive charts** using Angular Material.
  - **Persists data** using **MySQL** database.

---

### **7.1.2 Key Features**

1. **User Authentication and Authorization**:
   - **JWT Authentication** with **access and refresh tokens**.
   - **Role-based Authorization**:
     - `USER`: Manage personal expenses.
     - `ADMIN`: Manage all expenses and categories.

2. **Expense Management**:
   - **Create, Read, Update, and Delete (CRUD)** expenses.
   - **Categorize expenses** with dynamic categories.

3. **Interactive Dashboard**:
   - **Visualize expenses** with charts.
   - **Filter expenses** by category and date range.

4. **Real-Time State Management**:
   - **Reactive State Management** using **Signals**.
   - **Fine-grained change detection** for better performance.

5. **Containerization and Deployment**:
   - **Containerized deployment** using **Docker** and **Kubernetes**.
   - **CI/CD pipeline** using **GitHub Actions**.

---

### **7.1.3 Technology Stack**

- **Frontend (Angular 19)**:
  - **Signals** for reactive state management.
  - **Typed Forms** for type-safe validation.
  - **Angular Material** for UI components.
  - **NgRx Store** for state management.
  - **RxJS** for asynchronous data handling.

- **Backend (Spring Boot)**:
  - **Spring Security** for JWT authentication and role-based authorization.
  - **Spring Data JPA** for database interaction.
  - **MySQL** as the relational database.
  - **RESTful APIs** for frontend-backend communication.

- **Deployment and CI/CD**:
  - **Docker** for containerization.
  - **Kubernetes** for orchestration.
  - **GitHub Actions** for CI/CD automation.

---

## **7.2 Project Architecture**

---

### **7.2.1 Microservices Architecture Overview**

- The project follows a **Microservices Architecture** with:
  - **Angular Micro Frontend** for the user interface.
  - **Spring Boot Microservices** for:
    - **Auth Service** – Handles authentication and authorization.
    - **Expense Service** – Manages expenses and categories.
  - **API Gateway** for centralized routing and CORS management.
  - **MySQL Database** for data persistence.

---

### **7.2.2 Microservices and Micro Frontend Diagram**

```
                  +---------------+
                  |  Angular App  |
                  | (Micro Front) |
                  +---------------+
                          |
                  +---------------+
                  |  API Gateway  |
                  +---------------+
                          |
           +--------------+--------------+
           |                             |
+------------------+          +------------------+
| Auth Service     |          | Expense Service   |
| (Spring Boot)    |          | (Spring Boot)     |
+------------------+          +------------------+
           |                             |
    +-------------+                 +-------------+
    |  MySQL DB   |                 |  MySQL DB   |
    +-------------+                 +-------------+
```

- **Angular Micro Frontend**:
  - **Single Page Application** with:
    - **Routing** for navigation.
    - **Typed Forms** for user input.
    - **Signals** for state management.
- **Spring Boot Microservices**:
  - **Auth Service**:
    - **JWT Authentication** and **Role-based Authorization**.
    - **Access and Refresh Tokens** for session management.
  - **Expense Service**:
    - **CRUD operations** for expenses and categories.
    - **User-specific expense management**.
- **API Gateway**:
  - **Centralized routing and CORS management**.
  - **Load balancing** and **rate limiting**.
- **MySQL Database**:
  - **Separate database** for each microservice.
  - **Database-per-service pattern** for data isolation.

---

## **7.3 Angular Frontend Implementation**

---

### **7.3.1 Project Setup**

**Step 1: Create Angular Project**

```sh
# Create a new Angular project
ng new expense-management-frontend --routing --style=scss

# Navigate to the project directory
cd expense-management-frontend

# Install Angular Material and NgRx Store
ng add @angular/material
npm install @ngrx/store @ngrx/effects

# Install Angular JWT Helper and Signals
npm install @auth0/angular-jwt
```

- **@ngrx/store** and **@ngrx/effects**:
  - Used for **state management**.
- **@auth0/angular-jwt**:
  - Handles **JWT Authentication** in Angular.
- **Angular Material**:
  - Provides **UI components** for the user interface.

---

### **7.3.2 Folder Structure**

```
src/
│
├── app/
│   ├── components/              # UI components
│   ├── pages/                   # Page components
│   ├── services/                # API and Auth services
│   ├── store/                   # NgRx Store
│   ├── models/                  # TypeScript interfaces and models
│   ├── interceptors/            # HTTP Interceptors
│   ├── guards/                  # Route Guards
│   ├── app-routing.module.ts    # App Routing
│   └── app.module.ts            # App Module
│
└── assets/                      # Static assets
```

---

### **7.3.3 Implementing JWT Authentication**

---

#### **Step 1: Create Auth Service**

**Example: auth.service.ts**

```typescript
import { Injectable } from '@angular/core';
import { HttpClient, HttpHeaders } from '@angular/common/http';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';
import { JwtHelperService } from '@auth0/angular-jwt';

@Injectable({
  providedIn: 'root'
})
export class AuthService {
  private apiUrl = 'http://localhost:8080/auth';
  private jwtHelper = new JwtHelperService();

  constructor(private http: HttpClient) {}

  // Login Method
  login(username: string, password: string): Observable<any> {
    return this.http.post<any>(`${this.apiUrl}/login`, { username, password }).pipe(
      tap(response => {
        localStorage.setItem('accessToken', response.accessToken);
        localStorage.setItem('refreshToken', response.refreshToken);
      })
    );
  }

  // Get Access Token
  getAccessToken(): string | null {
    return localStorage.getItem('accessToken');
  }

  // Check if User is Logged In
  isLoggedIn(): boolean {
    const token = this.getAccessToken();
    return token && !this.jwtHelper.isTokenExpired(token);
  }

  // Logout
  logout(): void {
    localStorage.removeItem('accessToken');
    localStorage.removeItem('refreshToken');
  }
}
```

- **login()**:
  - Sends a **POST request** to Spring Boot for authentication.
  - Stores **accessToken** and **refreshToken** in **Local Storage**.
- **isLoggedIn()**:
  - Checks if the user is **authenticated** and the token is **not expired**.

---

## **Next Steps:**
- **7.4 Spring Boot Backend Implementation**
  - Setting up Spring Boot Microservices
  - Implementing JWT Authentication
  - Integrating with Angular 19 Frontend

---

## **7.4 Spring Boot Backend Implementation**

---

## **7.4.1 Setting up Spring Boot Microservices**

We will set up **Spring Boot Microservices** for:
- **Auth Service** – Handles JWT Authentication and Role-based Authorization.
- **Expense Service** – Manages Expenses and Categories with CRUD operations.
- **API Gateway** – Centralized routing and CORS management.

---

### **7.4.1.1 Creating Spring Boot Projects**

We will use **Spring Initializr** to create the microservices.

---

#### **Step 1: Create Auth Service**

- **Project Name**: `auth-service`
- **Dependencies**:
  - **Spring Web** – For building RESTful APIs.
  - **Spring Security** – For JWT Authentication.
  - **Spring Data JPA** – For database interaction.
  - **MySQL Driver** – For MySQL database connectivity.
  - **Lombok** – For reducing boilerplate code.

**URL for Spring Initializr**:
- https://start.spring.io/

**Example: application.properties (Auth Service)**

```properties
# Server Port
server.port=8081

# JWT Configuration
jwt.secret=YourSecretKeyHere
jwt.access.expiration=900000
jwt.refresh.expiration=604800000

# MySQL Database Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/auth_db
spring.datasource.username=root
spring.datasource.password=root
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
spring.jpa.show-sql=true
```

- **server.port=8081** – Sets the **Auth Service** port.
- **JWT Configuration**:
  - `jwt.secret` – Secret key for signing tokens.
  - `jwt.access.expiration` – Access token expiration time.
  - `jwt.refresh.expiration` – Refresh token expiration time.
- **MySQL Database Configuration**:
  - **auth_db** is the **database name** for Auth Service.
  - **spring.jpa.hibernate.ddl-auto=update** – Auto updates the database schema.

---

#### **Step 2: Create Expense Service**

- **Project Name**: `expense-service`
- **Dependencies**:
  - **Spring Web** – For building RESTful APIs.
  - **Spring Security** – For securing APIs.
  - **Spring Data JPA** – For database interaction.
  - **MySQL Driver** – For MySQL database connectivity.
  - **Lombok** – For reducing boilerplate code.

**Example: application.properties (Expense Service)**

```properties
# Server Port
server.port=8082

# MySQL Database Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/expense_db
spring.datasource.username=root
spring.datasource.password=root
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
spring.jpa.show-sql=true
```

- **server.port=8082** – Sets the **Expense Service** port.
- **expense_db** is the **database name** for Expense Service.

---

### **7.4.1.2 Creating API Gateway**

- **Project Name**: `api-gateway`
- **Dependencies**:
  - **Spring Cloud Gateway** – For centralized routing and CORS management.
  - **Spring Security** – For securing API Gateway routes.
  - **Eureka Discovery Client** – For service discovery.

**Example: application.properties (API Gateway)**

```properties
# Server Port
server.port=8080

# Enable Eureka Discovery Client
spring.application.name=api-gateway
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/

# Security Configuration
spring.security.oauth2.resourceserver.jwt.jwk-set-uri=http://localhost:8081/oauth2/jwks

# CORS Configuration
spring.cloud.gateway.globalcors.corsConfigurations.[/**].allowedOrigins=*
spring.cloud.gateway.globalcors.corsConfigurations.[/**].allowedMethods=GET,POST,PUT,DELETE
```

- **server.port=8080** – Sets the **API Gateway** port.
- **spring.cloud.gateway.globalcors.corsConfigurations**:
  - Configures **CORS** to allow requests from all origins.
- **spring.security.oauth2.resourceserver.jwt.jwk-set-uri**:
  - Validates **JWT tokens** using **Auth Service**.

---

### **7.4.1.3 Setting up Eureka Discovery Server**

- **Project Name**: `eureka-server`
- **Dependencies**:
  - **Eureka Server** – For service discovery and registration.

**Example: application.properties (Eureka Server)**

```properties
# Server Port
server.port=8761

# Enable Eureka Server
spring.application.name=eureka-server
eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false
```

- **server.port=8761** – Sets the **Eureka Server** port.
- **eureka.client.register-with-eureka=false**:
  - **Eureka Server** does not register itself.
- **eureka.client.fetch-registry=false**:
  - **Eureka Server** does not fetch the service registry.

---

## **7.4.2 Implementing JWT Authentication in Auth Service**

---

### **7.4.2.1 Creating User Entity**

**Example: User Entity (User.java)**

```java
package com.example.authservice.entity;

import jakarta.persistence.*;
import lombok.Data;

import java.util.Set;

@Entity
@Data
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true)
    private String username;

    @Column(nullable = false)
    private String password;

    @ManyToMany(fetch = FetchType.EAGER)
    @JoinTable(
        name = "user_roles",
        joinColumns = @JoinColumn(name = "user_id"),
        inverseJoinColumns = @JoinColumn(name = "role_id")
    )
    private Set<Role> roles;
}
```

- **User Entity**:
  - **username** and **password** for authentication.
  - **roles** for role-based authorization.

---

### **7.4.2.2 Creating Role Entity**

**Example: Role Entity (Role.java)**

```java
package com.example.authservice.entity;

import jakarta.persistence.*;
import lombok.Data;

@Entity
@Data
public class Role {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true)
    private String name;
}
```

- **Role Entity**:
  - **name**: Role name (e.g., `ROLE_USER`, `ROLE_ADMIN`).

---

### **7.4.2.3 JWT Token Generation and Validation**

**Example: JwtTokenProvider.java**

```java
package com.example.authservice.security;

import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

import java.util.Date;

@Component
public class JwtTokenProvider {

    @Value("${jwt.secret}")
    private String secret;

    @Value("${jwt.access.expiration}")
    private long accessTokenExpiration;

    // Generate JWT Token
    public String generateToken(String username) {
        Date now = new Date();
        Date expiryDate = new Date(now.getTime() + accessTokenExpiration);

        return Jwts.builder()
                .setSubject(username)
                .setIssuedAt(now)
                .setExpiration(expiryDate)
                .signWith(SignatureAlgorithm.HS512, secret)
                .compact();
    }

    // Validate JWT Token
    public boolean validateToken(String token) {
        try {
            Jwts.parser().setSigningKey(secret).parseClaimsJws(token);
            return true;
        } catch (Exception ex) {
            return false;
        }
    }
}
```

- **generateToken()**:
  - Generates a **JWT Access Token** with **HS512** signing algorithm.
- **validateToken()**:
  - Validates the **JWT Token** using the secret key.

---

## **Next Steps:**
- **7.5 Integrating with Angular 19 Frontend**
  - Connecting Angular with Spring Boot APIs
  - Implementing Role-based Authorization
  - Real-World Deployment with Docker and Kubernetes

---

## **7.5 Integrating Angular 19 Frontend with Spring Boot APIs**

---

## **7.5.1 Overview of Frontend-Backend Integration**

We will **integrate Angular 19 frontend** with **Spring Boot Microservices** using:
- **HTTP Client** for making **RESTful API calls**.
- **JWT Authentication** for securing API requests.
- **Role-based Authorization** for controlling access to routes and components.
- **Reactive State Management** using **Signals** and **NgRx Store**.

---

### **7.5.2 Connecting Angular with Spring Boot APIs**

---

### **7.5.2.1 Setting up Environment Variables**

- Angular 19 uses **environment.ts** for configuration.

**Example: src/environments/environment.ts**

```typescript
export const environment = {
  production: false,
  apiBaseUrl: 'http://localhost:8080/api',
  authUrl: 'http://localhost:8081/auth',
  expenseUrl: 'http://localhost:8082/expense'
};
```

- **apiBaseUrl**: Base URL for **API Gateway**.
- **authUrl**: Base URL for **Auth Service**.
- **expenseUrl**: Base URL for **Expense Service**.

---

### **7.5.2.2 Configuring CORS in Spring Boot**

To allow Angular to communicate with Spring Boot APIs, **CORS** needs to be configured.

**Example: Global CORS Configuration (WebConfig.java)**

```java
package com.example.demo.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig {

    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/**")
                        .allowedOrigins("http://localhost:4200")
                        .allowedMethods("GET", "POST", "PUT", "DELETE")
                        .allowedHeaders("*")
                        .allowCredentials(true);
            }
        };
    }
}
```

- **allowedOrigins("http://localhost:4200")**:
  - Allows **Angular development server** to communicate with Spring Boot.
- **allowedMethods**:
  - Specifies allowed HTTP methods (`GET`, `POST`, `PUT`, `DELETE`).

---

### **7.5.2.3 Setting up HTTP Interceptors in Angular**

---

#### **Step 1: Create HTTP Interceptor**

- We will create a **JWT Interceptor** to:
  - **Attach the access token** to all HTTP requests.
  - **Handle expired tokens** and **refresh tokens**.

**Example: jwt.interceptor.ts**

```typescript
import { Injectable } from '@angular/core';
import { HttpEvent, HttpInterceptor, HttpHandler, HttpRequest, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';
import { AuthService } from '../services/auth.service';
import { Router } from '@angular/router';

@Injectable()
export class JwtInterceptor implements HttpInterceptor {

  constructor(private authService: AuthService, private router: Router) {}

  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    const token = this.authService.getAccessToken();
    if (token) {
      req = req.clone({
        setHeaders: {
          Authorization: `Bearer ${token}`
        }
      });
    }
    return next.handle(req).pipe(
      catchError((error: HttpErrorResponse) => {
        if (error.status === 401) {
          this.authService.logout();
          this.router.navigate(['/login']);
        }
        return throwError(error);
      })
    );
  }
}
```

- **Authorization: Bearer ${token}**:
  - Attaches the **JWT access token** to every HTTP request.
- **catchError()**:
  - Checks for **401 Unauthorized** response.
  - **Logs out** the user and **redirects to login page**.

---

#### **Step 2: Registering HTTP Interceptor**

**Example: app.module.ts**

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClientModule, HTTP_INTERCEPTORS } from '@angular/common/http';
import { JwtInterceptor } from './interceptors/jwt.interceptor';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    HttpClientModule
  ],
  providers: [
    { provide: HTTP_INTERCEPTORS, useClass: JwtInterceptor, multi: true }
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

- **HTTP_INTERCEPTORS**:
  - Registers **JwtInterceptor** as an **HTTP Interceptor**.
- **multi: true**:
  - Allows multiple interceptors to be registered.

---

### **7.5.2.4 Making API Calls with Angular HTTP Client**

---

#### **Step 1: Create Expense Service**

- We will create an **Expense Service** to:
  - **Make CRUD API calls** to **Expense Service** in Spring Boot.

**Example: expense.service.ts**

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { environment } from 'src/environments/environment';
import { Expense } from '../models/expense.model';

@Injectable({
  providedIn: 'root'
})
export class ExpenseService {
  private apiUrl = `${environment.expenseUrl}/expenses`;

  constructor(private http: HttpClient) {}

  // Get All Expenses
  getExpenses(): Observable<Expense[]> {
    return this.http.get<Expense[]>(this.apiUrl);
  }

  // Create Expense
  createExpense(expense: Expense): Observable<Expense> {
    return this.http.post<Expense>(this.apiUrl, expense);
  }

  // Update Expense
  updateExpense(expense: Expense): Observable<Expense> {
    return this.http.put<Expense>(`${this.apiUrl}/${expense.id}`, expense);
  }

  // Delete Expense
  deleteExpense(expenseId: number): Observable<void> {
    return this.http.delete<void>(`${this.apiUrl}/${expenseId}`);
  }
}
```

- **getExpenses()**:
  - **GET request** to retrieve all expenses.
- **createExpense()**:
  - **POST request** to create a new expense.
- **updateExpense()**:
  - **PUT request** to update an existing expense.
- **deleteExpense()**:
  - **DELETE request** to delete an expense by ID.

---

### **7.5.2.5 Consuming APIs in Angular Components**

**Example: expense-list.component.ts**

```typescript
import { Component, OnInit } from '@angular/core';
import { ExpenseService } from '../../services/expense.service';
import { Expense } from '../../models/expense.model';

@Component({
  selector: 'app-expense-list',
  templateUrl: './expense-list.component.html',
  styleUrls: ['./expense-list.component.scss']
})
export class ExpenseListComponent implements OnInit {
  expenses: Expense[] = [];

  constructor(private expenseService: ExpenseService) {}

  ngOnInit(): void {
    this.loadExpenses();
  }

  loadExpenses(): void {
    this.expenseService.getExpenses().subscribe(
      (data) => {
        this.expenses = data;
      },
      (error) => {
        console.error('Error loading expenses', error);
      }
    );
  }
}
```

- **ngOnInit()**:
  - Calls **loadExpenses()** to **fetch expenses** on component initialization.
- **loadExpenses()**:
  - **Subscribes** to the `getExpenses()` method in **Expense Service**.

---

## **Next Steps:**
- **7.6 Implementing Role-based Authorization**
  - Protecting Routes with Angular Guards
  - Role-based Authorization in Angular and Spring Boot
  - Real-World Examples with Admin and User Roles

---

## **7.6 Implementing Role-based Authorization**

---

## **7.6.1 Overview of Role-based Authorization**

We will implement **Role-based Authorization** using:
- **Angular Guards** for protecting routes and components.
- **Spring Security** for securing backend APIs.
- **JWT Role Claims** for authorizing users based on roles.
- **Admin and User Roles**:
  - `ADMIN`: Full access to manage users, expenses, and categories.
  - `USER`: Access to manage personal expenses only.

---

### **7.6.2 Role-based Authorization in Spring Boot**

---

### **7.6.2.1 Configuring Roles and Permissions**

We will define two roles:
1. **ROLE_USER**:
   - Can manage **own expenses**.
   - Cannot access admin-specific endpoints.
2. **ROLE_ADMIN**:
   - Full access to **manage all users, expenses, and categories**.

---

### **7.6.2.2 Defining User Roles in Spring Boot**

---

#### **Step 1: Define Role Entity**

**Example: Role.java**

```java
package com.example.authservice.entity;

import jakarta.persistence.*;
import lombok.Data;

@Entity
@Data
public class Role {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true)
    private String name;
}
```

- **Role Entity**:
  - Represents a **user role** in the database.
  - Contains:
    - `id`: Unique identifier.
    - `name`: Role name (`ROLE_USER` or `ROLE_ADMIN`).

---

#### **Step 2: Define User Entity with Roles**

**Example: User.java**

```java
package com.example.authservice.entity;

import jakarta.persistence.*;
import lombok.Data;
import java.util.Set;

@Entity
@Data
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true)
    private String username;

    @Column(nullable = false)
    private String password;

    @ManyToMany(fetch = FetchType.EAGER)
    @JoinTable(
        name = "user_roles",
        joinColumns = @JoinColumn(name = "user_id"),
        inverseJoinColumns = @JoinColumn(name = "role_id")
    )
    private Set<Role> roles;
}
```

- **User Entity**:
  - Contains:
    - `username` and `password` for authentication.
    - **roles** for role-based authorization.
- **ManyToMany**:
  - Establishes a **many-to-many relationship** between `User` and `Role`.
  - Creates a **join table** `user_roles`.

---

### **7.6.2.3 Assigning Roles in Database**

---

#### **Step 1: Define Role Repository**

**Example: RoleRepository.java**

```java
package com.example.authservice.repository;

import com.example.authservice.entity.Role;
import org.springframework.data.jpa.repository.JpaRepository;

public interface RoleRepository extends JpaRepository<Role, Long> {
    Role findByName(String name);
}
```

- **Role Repository**:
  - Provides methods to **find roles by name**.
  - **findByName()**: Finds role by name (e.g., `ROLE_USER`, `ROLE_ADMIN`).

---

#### **Step 2: Initialize Roles in Database**

**Example: DataLoader.java**

```java
package com.example.authservice.config;

import com.example.authservice.entity.Role;
import com.example.authservice.repository.RoleRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;

@Component
public class DataLoader implements CommandLineRunner {

    @Autowired
    private RoleRepository roleRepository;

    @Override
    public void run(String... args) {
        if (roleRepository.findByName("ROLE_USER") == null) {
            Role userRole = new Role();
            userRole.setName("ROLE_USER");
            roleRepository.save(userRole);
        }

        if (roleRepository.findByName("ROLE_ADMIN") == null) {
            Role adminRole = new Role();
            adminRole.setName("ROLE_ADMIN");
            roleRepository.save(adminRole);
        }
    }
}
```

- **DataLoader**:
  - **Initializes roles** in the database if they don't exist.
  - Creates:
    - `ROLE_USER`
    - `ROLE_ADMIN`

---

### **7.6.2.4 Securing Endpoints with Role-based Access**

---

#### **Step 1: Define Role-based Security Configuration**

**Example: SecurityConfig.java**

```java
package com.example.authservice.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .authorizeHttpRequests()
            .requestMatchers("/auth/login", "/auth/register").permitAll()
            .requestMatchers("/admin/**").hasRole("ADMIN")
            .requestMatchers("/user/**").hasAnyRole("USER", "ADMIN")
            .anyRequest().authenticated()
            .and()
            .httpBasic();
        return http.build();
    }
}
```

- **requestMatchers("/admin/**").hasRole("ADMIN")**:
  - Only `ADMIN` can access endpoints under `/admin/`.
- **requestMatchers("/user/**").hasAnyRole("USER", "ADMIN")**:
  - Both `USER` and `ADMIN` can access endpoints under `/user/`.
- **httpBasic()**:
  - Uses **Basic Authentication** for simplicity (replace with JWT in production).

---

### **7.6.2.5 Example: Securing Admin Endpoint**

**Example: AdminController.java**

```java
package com.example.authservice.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/admin")
public class AdminController {

    @GetMapping("/dashboard")
    public String getAdminDashboard() {
        return "Welcome to Admin Dashboard!";
    }
}
```

- **@RequestMapping("/admin")**:
  - Base URL for **admin-specific endpoints**.
- **@GetMapping("/dashboard")**:
  - Secured with **ROLE_ADMIN** using `SecurityConfig`.

---

## **7.6.3 Role-based Authorization in Angular**

---

### **7.6.3.1 Protecting Routes with Angular Guards**

---

#### **Step 1: Create Role Guard**

**Example: role.guard.ts**

```typescript
import { Injectable } from '@angular/core';
import { CanActivate, Router } from '@angular/router';
import { AuthService } from '../services/auth.service';

@Injectable({
  providedIn: 'root'
})
export class RoleGuard implements CanActivate {

  constructor(private authService: AuthService, private router: Router) {}

  canActivate(): boolean {
    if (this.authService.hasRole('ADMIN')) {
      return true;
    } else {
      this.router.navigate(['/unauthorized']);
      return false;
    }
  }
}
```

- **hasRole('ADMIN')**:
  - Checks if the user has the `ADMIN` role.
- **canActivate()**:
  - Allows navigation if the user is an `ADMIN`.
  - Redirects to **Unauthorized Page** otherwise.

---

#### **Step 2: Applying Role Guard to Routes**

**Example: app-routing.module.ts**

```typescript
const routes: Routes = [
  { path: 'admin', component: AdminDashboardComponent, canActivate: [RoleGuard] },
  { path: 'unauthorized', component: UnauthorizedComponent }
];
```

- **canActivate: [RoleGuard]**:
  - Protects the **Admin Dashboard** route with **RoleGuard**.
  - Only accessible to **ADMIN** users.

---

## **Next Steps:**
- **7.7 Real-World Deployment with Docker and Kubernetes**
  - Containerizing Angular and Spring Boot Microservices
  - Deploying on Kubernetes with CI/CD Pipeline
  - Real-World Cloud Deployment Examples

---

## **7.7 Real-World Deployment with Docker and Kubernetes**

---

## **7.7.1 Overview of Containerization and Deployment**

In this section, we will **containerize and deploy** our Angular 19 and Spring Boot Microservices using:
- **Docker** for containerization.
- **Kubernetes (K8s)** for orchestration and scaling.
- **CI/CD Pipeline** using **GitHub Actions**.
- **Real-World Deployment** on **Cloud Platforms** (AWS, Azure, GCP).

---

### **7.7.2 Why Use Docker and Kubernetes?**

- **Docker**:
  - **Containerizes Angular and Spring Boot Microservices** for consistent deployment.
  - Ensures **environment consistency** across development, testing, and production.

- **Kubernetes (K8s)**:
  - **Orchestrates containers** for scaling and resilience.
  - Provides:
    - **Service Discovery** for microservices.
    - **Load Balancing** and **Auto-scaling**.
    - **Zero-downtime deployments** with rolling updates.

- **CI/CD Pipeline**:
  - Automates **build, test, and deployment** processes.
  - Ensures **continuous integration and continuous delivery**.

---

## **7.7.3 Containerizing Angular and Spring Boot Microservices**

---

### **7.7.3.1 Dockerizing Angular Frontend**

---

#### **Step 1: Create Dockerfile for Angular**

**Example: Dockerfile (Angular Frontend)**

```Dockerfile
# Step 1: Build Angular App
FROM node:18 AS build

WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build --prod

# Step 2: Serve Angular App with Nginx
FROM nginx:alpine
COPY --from=build /app/dist/expense-management-frontend /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

- **Multi-stage Build**:
  - **Step 1 (Build Stage)**:
    - Builds the Angular app using **Node.js**.
  - **Step 2 (Production Stage)**:
    - **Serves Angular static files** with **Nginx**.
- **EXPOSE 80**:
  - Exposes **Nginx** on port `80`.

---

#### **Step 2: Build and Run Angular Docker Image**

```sh
# Build Docker Image
docker build -t expense-frontend:latest .

# Run Docker Container
docker run -d -p 4200:80 expense-frontend:latest
```

- **docker build**:
  - Builds a **Docker image** named `expense-frontend`.
- **docker run**:
  - Runs a **Docker container** on port `4200`.

---

### **7.7.3.2 Dockerizing Spring Boot Microservices**

---

#### **Step 1: Create Dockerfile for Auth Service**

**Example: Dockerfile (Auth Service)**

```Dockerfile
# Use Maven to Build Spring Boot Application
FROM maven:3.8.6-openjdk-17 AS build
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn clean package -DskipTests

# Use OpenJDK to Run Spring Boot Application
FROM openjdk:17-jdk-slim
COPY --from=build /app/target/auth-service.jar /auth-service.jar
EXPOSE 8081
ENTRYPOINT ["java", "-jar", "/auth-service.jar"]
```

- **Multi-stage Build**:
  - **Step 1 (Build Stage)**:
    - **Builds Spring Boot JAR** using **Maven**.
  - **Step 2 (Production Stage)**:
    - **Runs Spring Boot application** with **OpenJDK 17**.
- **EXPOSE 8081**:
  - Exposes **Auth Service** on port `8081`.

---

#### **Step 2: Build and Run Auth Service Docker Image**

```sh
# Build Docker Image
docker build -t auth-service:latest .

# Run Docker Container
docker run -d -p 8081:8081 auth-service:latest
```

- **docker build**:
  - Builds a **Docker image** named `auth-service`.
- **docker run**:
  - Runs a **Docker container** on port `8081`.

---

#### **Step 3: Create Dockerfile for Expense Service**

- Repeat the above steps for the **Expense Service**:
  - **EXPOSE 8082** – Exposes on port `8082`.
  - **docker build -t expense-service:latest .**
  - **docker run -d -p 8082:8082 expense-service:latest**

---

## **7.7.4 Using Docker Compose for Multi-Container Setup**

---

### **7.7.4.1 Docker Compose Configuration**

**Example: docker-compose.yml**

```yaml
version: '3.8'
services:
  frontend:
    image: expense-frontend:latest
    container_name: expense-frontend
    ports:
      - "4200:80"
    networks:
      - expense-network

  auth-service:
    image: auth-service:latest
    container_name: auth-service
    ports:
      - "8081:8081"
    networks:
      - expense-network
    environment:
      - SPRING_PROFILES_ACTIVE=prod

  expense-service:
    image: expense-service:latest
    container_name: expense-service
    ports:
      - "8082:8082"
    networks:
      - expense-network
    environment:
      - SPRING_PROFILES_ACTIVE=prod

  mysql:
    image: mysql:8.0
    container_name: expense-mysql
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: expense_db
    ports:
      - "3306:3306"
    networks:
      - expense-network
    volumes:
      - db_data:/var/lib/mysql

networks:
  expense-network:
    driver: bridge

volumes:
  db_data:
```

- **Docker Compose**:
  - **Orchestrates multiple containers** for:
    - **Angular Frontend** (`expense-frontend`)
    - **Auth Service** (`auth-service`)
    - **Expense Service** (`expense-service`)
    - **MySQL Database** (`expense-mysql`)
- **Networks and Volumes**:
  - **expense-network**: Creates a **bridge network** for container communication.
  - **db_data**: Persistent storage for **MySQL database**.

---

### **7.7.4.2 Starting and Stopping Containers**

```sh
# Start Containers
docker-compose up -d

# Stop Containers
docker-compose down
```

- **docker-compose up -d**:
  - **Starts all containers** in detached mode.
- **docker-compose down**:
  - **Stops and removes** all containers.

---

## **7.7.5 Deploying on Kubernetes (K8s)**

---

### **7.7.5.1 Creating Kubernetes Manifests**

**Example: deployment.yaml**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: expense-frontend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: expense-frontend
  template:
    metadata:
      labels:
        app: expense-frontend
    spec:
      containers:
      - name: expense-frontend
        image: expense-frontend:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: expense-frontend-service
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30001
  selector:
    app: expense-frontend
```

- **Deployment**:
  - **2 replicas** of `expense-frontend` for high availability.
- **Service (NodePort)**:
  - Exposes the frontend on **NodePort 30001**.

---

### **7.7.5.2 Deploying on Kubernetes**

```sh
# Apply Kubernetes Manifests
kubectl apply -f deployment.yaml
```

- **kubectl apply -f**:
  - Deploys the **Angular frontend** on **Kubernetes**.

---

## **Next Steps:**
- **7.8 CI/CD Pipeline with GitHub Actions**
  - Automating Build, Test, and Deployment
  - Real-World CI/CD Workflow for Microservices

---

## **7.8 CI/CD Pipeline with GitHub Actions**

---

## **7.8.1 Overview of CI/CD Pipeline**

We will implement a **CI/CD Pipeline** using:
- **GitHub Actions** for automating:
  - **Build** and **Test** of Angular and Spring Boot Microservices.
  - **Docker Image Creation** and **Push to Docker Hub**.
  - **Kubernetes Deployment** on **Cloud Platforms**.
- **Multi-Stage Pipeline** with:
  - **Build Stage** – Compiles and tests the code.
  - **Docker Stage** – Builds and pushes Docker images.
  - **Deploy Stage** – Deploys the application to **Kubernetes**.

---

### **7.8.2 Why Use GitHub Actions?**

- **GitHub Actions** provides:
  - **Continuous Integration** with automated testing.
  - **Continuous Delivery** with automated deployment.
  - **Built-in integration with GitHub** for easy repository management.
  - **YAML-based workflows** for customizable pipelines.

---

### **7.8.3 Setting Up GitHub Actions for CI/CD**

---

### **7.8.3.1 Creating GitHub Secrets**

Before configuring the pipeline, we need to **create GitHub Secrets** for:
- **DOCKER_HUB_USERNAME** – Docker Hub username.
- **DOCKER_HUB_TOKEN** – Docker Hub access token.
- **KUBE_CONFIG** – Base64 encoded **Kubernetes configuration file**.

---

#### **Step 1: Create Docker Hub Token**

- Log in to **Docker Hub**.
- Go to **Account Settings > Security > New Access Token**.
- Create a **read/write token**.
- **Copy the token** and save it as **DOCKER_HUB_TOKEN**.

---

#### **Step 2: Base64 Encode Kubernetes Config**

```sh
cat ~/.kube/config | base64
```

- **Base64 encode** the Kubernetes configuration file.
- **Copy the output** and save it as **KUBE_CONFIG** in **GitHub Secrets**.

---

### **7.8.3.2 Defining GitHub Actions Workflow**

---

#### **Step 1: Create Workflow File**

**Example: .github/workflows/ci-cd.yml**

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build-test:
    name: Build and Test
    runs-on: ubuntu-latest

    services:
      mysql:
        image: mysql:8.0
        env:
          MYSQL_ROOT_PASSWORD: root
          MYSQL_DATABASE: expense_db
        ports:
          - 3306:3306
        options: --health-cmd='mysqladmin ping --silent' --health-interval=10s --health-timeout=5s --health-retries=3

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Set up JDK 17
        uses: actions/setup-java@v3
        with:
          java-version: '17'

      - name: Cache Maven Packages
        uses: actions/cache@v3
        with:
          path: ~/.m2
          key: ${{ runner.os }}-maven-${{ hashFiles('**/pom.xml') }}

      - name: Build and Test Spring Boot
        run: mvn clean test

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install Dependencies
        run: npm install --prefix expense-management-frontend

      - name: Build Angular App
        run: npm run build --prefix expense-management-frontend

      - name: Run Angular Unit Tests
        run: npm test --prefix expense-management-frontend
```

---

### **7.8.3.3 Explanation of Build and Test Job**

- **Build and Test Job**:
  - **Runs on**: `ubuntu-latest`
  - **Uses Services**:
    - **MySQL 8.0** for integration tests.
  - **Steps**:
    - **Checkout Code** – Clones the repository.
    - **Set up JDK 17** – For building Spring Boot microservices.
    - **Cache Maven Packages** – Speeds up Maven builds.
    - **Build and Test Spring Boot** – Compiles and runs tests.
    - **Set up Node.js 18** – For building Angular frontend.
    - **Install Dependencies** – Installs Angular dependencies.
    - **Build Angular App** – Compiles the Angular frontend.
    - **Run Angular Unit Tests** – Runs Jasmine/Karma tests.

---

### **7.8.3.4 Docker Image Build and Push Stage**

---

#### **Step 1: Define Docker Stage**

**Example: ci-cd.yml (Docker Stage)**

```yaml
  docker:
    name: Docker Build and Push
    runs-on: ubuntu-latest
    needs: build-test

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2

      - name: Log in to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_HUB_USERNAME }}
          password: ${{ secrets.DOCKER_HUB_TOKEN }}

      - name: Build and Push Angular Image
        uses: docker/build-push-action@v4
        with:
          context: ./expense-management-frontend
          push: true
          tags: ${{ secrets.DOCKER_HUB_USERNAME }}/expense-frontend:latest

      - name: Build and Push Auth Service Image
        uses: docker/build-push-action@v4
        with:
          context: ./auth-service
          push: true
          tags: ${{ secrets.DOCKER_HUB_USERNAME }}/auth-service:latest

      - name: Build and Push Expense Service Image
        uses: docker/build-push-action@v4
        with:
          context: ./expense-service
          push: true
          tags: ${{ secrets.DOCKER_HUB_USERNAME }}/expense-service:latest
```

---

### **7.8.3.5 Explanation of Docker Stage**

- **needs: build-test**:
  - **Depends on Build and Test Job** completion.
- **Uses**:
  - **docker/setup-buildx-action** – Sets up Docker Buildx for multi-platform builds.
  - **docker/login-action** – Logs into **Docker Hub** using **GitHub Secrets**.
  - **docker/build-push-action** – Builds and pushes Docker images to Docker Hub.

- **Images Pushed**:
  - `expense-frontend:latest`
  - `auth-service:latest`
  - `expense-service:latest`

---

### **7.8.3.6 Deploying on Kubernetes (Deploy Stage)**

---

#### **Step 1: Define Deploy Stage**

**Example: ci-cd.yml (Deploy Stage)**

```yaml
  deploy:
    name: Deploy to Kubernetes
    runs-on: ubuntu-latest
    needs: docker

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Set up Kubectl
        uses: azure/setup-kubectl@v3
        with:
          version: 'latest'

      - name: Configure Kubeconfig
        run: echo "${{ secrets.KUBE_CONFIG }}" | base64 --decode > ~/.kube/config

      - name: Deploy to Kubernetes
        run: |
          kubectl apply -f k8s/deployment.yaml
          kubectl rollout status deployment expense-frontend
```

---

### **7.8.3.7 Explanation of Deploy Stage**

- **needs: docker**:
  - **Depends on Docker Stage** completion.
- **Uses**:
  - **azure/setup-kubectl** – Sets up **kubectl** for Kubernetes deployment.
- **Deploys on Kubernetes**:
  - **expense-frontend** and other microservices.
  - **Rolls out updates** with zero-downtime deployment.

---

## **Next Steps:**
- **7.9 Real-World Cloud Deployment Examples**
  - Deploying on **AWS EKS**, **Azure AKS**, and **Google GKE**
  - **CI/CD Integration** with **Cloud Provider's Managed Kubernetes Services**

---

## **7.9 Real-World Cloud Deployment Examples**

---

## **7.9.1 Overview of Cloud Deployment**

We will **deploy the Angular 19 and Spring Boot Microservices** on:
- **AWS EKS** – Amazon Elastic Kubernetes Service.
- **Azure AKS** – Azure Kubernetes Service.
- **Google GKE** – Google Kubernetes Engine.
- **CI/CD Integration** with **GitHub Actions** for automated deployments.

---

### **7.9.2 Why Use Managed Kubernetes Services?**

- **Managed Kubernetes Services** provide:
  - **Automated Scaling** – Automatically scales pods and nodes.
  - **High Availability** – Ensures application availability with multi-region clusters.
  - **Integrated Monitoring and Logging** – With **CloudWatch (AWS)**, **Azure Monitor**, and **Stackdriver (GCP)**.
  - **Security and Compliance** – Built-in security policies and compliance with industry standards.

---

## **7.9.3 Deploying on AWS EKS (Amazon Elastic Kubernetes Service)**

---

### **7.9.3.1 Prerequisites**

- **AWS Account** with:
  - **IAM Role** with `AdministratorAccess`.
  - **EKS Cluster** created in the desired region.
- **AWS CLI** and **kubectl** installed:
  - [AWS CLI Installation](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
  - [kubectl Installation](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

---

### **7.9.3.2 Configuring AWS CLI and EKS Cluster**

```sh
# Configure AWS CLI with IAM User
aws configure

# Verify EKS Clusters
aws eks list-clusters

# Update Kubeconfig for EKS Cluster
aws eks update-kubeconfig --name <your-cluster-name> --region <your-region>
```

- **aws configure**:
  - Configures **AWS CLI** with **IAM User credentials**.
- **aws eks update-kubeconfig**:
  - Updates the local **Kubeconfig** file to connect to **EKS Cluster**.

---

### **7.9.3.3 Creating EKS Cluster using eksctl**

**Example: eksctl command**

```sh
# Create EKS Cluster
eksctl create cluster \
  --name expense-cluster \
  --region us-east-1 \
  --nodegroup-name expense-nodes \
  --node-type t3.medium \
  --nodes 3 \
  --nodes-min 2 \
  --nodes-max 4 \
  --managed
```

- **eksctl create cluster**:
  - Creates an **EKS Cluster** named `expense-cluster`.
- **--nodes 3**:
  - Starts with **3 nodes** and **scales between 2 and 4 nodes**.
- **--managed**:
  - **Managed Node Group** for automatic scaling and updates.

---

### **7.9.3.4 Deploying on AWS EKS**

---

#### **Step 1: Define Kubernetes Manifests**

**Example: deployment-eks.yaml**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: expense-frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: expense-frontend
  template:
    metadata:
      labels:
        app: expense-frontend
    spec:
      containers:
      - name: expense-frontend
        image: <AWS_ACCOUNT_ID>.dkr.ecr.<REGION>.amazonaws.com/expense-frontend:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: expense-frontend-service
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 80
  selector:
    app: expense-frontend
```

- **Deployment**:
  - **3 replicas** of `expense-frontend` for high availability.
- **Service (LoadBalancer)**:
  - Exposes the frontend using **AWS Elastic Load Balancer**.

---

#### **Step 2: Deploying on AWS EKS**

```sh
# Apply Kubernetes Manifests
kubectl apply -f deployment-eks.yaml

# Get Service Details
kubectl get svc expense-frontend-service
```

- **kubectl apply -f**:
  - Deploys the **Angular frontend** on **AWS EKS**.
- **kubectl get svc**:
  - Retrieves the **public URL** of the **LoadBalancer**.

---

#### **Step 3: Accessing the Application**

- Access the application using the **public URL** provided by the **LoadBalancer**.
- Example: `http://<LoadBalancer-DNS-Name>`

---

## **7.9.4 Deploying on Azure AKS (Azure Kubernetes Service)**

---

### **7.9.4.1 Prerequisites**

- **Azure Account** with:
  - **Owner Role** on **Subscription**.
- **Azure CLI** and **kubectl** installed:
  - [Azure CLI Installation](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)
  - [kubectl Installation](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

---

### **7.9.4.2 Creating AKS Cluster**

```sh
# Login to Azure Account
az login

# Create Resource Group
az group create --name expense-rg --location eastus

# Create AKS Cluster
az aks create --resource-group expense-rg \
  --name expense-aks-cluster \
  --node-count 3 \
  --enable-addons monitoring \
  --generate-ssh-keys

# Get AKS Cluster Credentials
az aks get-credentials --resource-group expense-rg --name expense-aks-cluster
```

- **az aks create**:
  - Creates an **AKS Cluster** with:
    - **3 nodes**.
    - **Azure Monitor** enabled for logging and monitoring.
- **az aks get-credentials**:
  - Updates **Kubeconfig** to connect to the **AKS Cluster**.

---

### **7.9.4.3 Deploying on Azure AKS**

```sh
# Apply Kubernetes Manifests
kubectl apply -f deployment-eks.yaml

# Get Service Details
kubectl get svc expense-frontend-service
```

- **kubectl apply -f**:
  - Deploys the **Angular frontend** on **Azure AKS**.
- **kubectl get svc**:
  - Retrieves the **public IP** of the **Azure Load Balancer**.

---

### **7.9.4.4 Accessing the Application**

- Access the application using the **public IP** provided by the **Azure Load Balancer**.
- Example: `http://<Public-IP-Address>`

---

## **7.9.5 Deploying on Google GKE (Google Kubernetes Engine)**

---

### **7.9.5.1 Prerequisites**

- **Google Cloud Account** with:
  - **Owner Role** on **Project**.
- **gcloud CLI** and **kubectl** installed:
  - [gcloud CLI Installation](https://cloud.google.com/sdk/docs/install)
  - [kubectl Installation](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

---

### **7.9.5.2 Creating GKE Cluster**

```sh
# Authenticate with Google Cloud
gcloud auth login

# Set Project
gcloud config set project <your-project-id>

# Create GKE Cluster
gcloud container clusters create expense-cluster \
  --zone us-central1-a \
  --num-nodes 3 \
  --enable-autoscaling \
  --min-nodes 2 \
  --max-nodes 4

# Get GKE Cluster Credentials
gcloud container clusters get-credentials expense-cluster --zone us-central1-a
```

- **gcloud container clusters create**:
  - Creates a **GKE Cluster** with:
    - **3 nodes**.
    - **Auto-scaling** enabled between **2 and 4 nodes**.
- **gcloud container clusters get-credentials**:
  - Updates **Kubeconfig** to connect to the **GKE Cluster**.

---

### **7.9.5.3 Deploying on Google GKE**

```sh
# Apply Kubernetes Manifests
kubectl apply -f deployment-eks.yaml

# Get Service Details
kubectl get svc expense-frontend-service
```

- **kubectl get svc**:
  - Retrieves the **external IP** of the **GKE LoadBalancer**.

---

## **Next Steps:**
- **7.10 Advanced Monitoring and Logging**
  - Monitoring with **Prometheus and Grafana**
  - Centralized Logging with **ELK Stack**

---

## **7.10 Advanced Monitoring and Logging**

---

## **7.10.1 Overview of Monitoring and Logging**

We will set up **Advanced Monitoring and Centralized Logging** for:
- **Angular 19 Frontend** and **Spring Boot Microservices** deployed on **Kubernetes**.
- **Monitoring** with:
  - **Prometheus** – Metrics collection and alerting.
  - **Grafana** – Interactive dashboards and visualizations.
- **Centralized Logging** with:
  - **ELK Stack (Elasticsearch, Logstash, Kibana)** – For log aggregation, search, and visualization.
- **Cloud-Native Observability** using:
  - **CloudWatch (AWS)**, **Azure Monitor**, and **Google Stackdriver**.

---

### **7.10.2 Why Use Advanced Monitoring and Logging?**

- **Proactive Monitoring**:
  - **Identify performance bottlenecks** and **detect anomalies**.
  - **Real-time alerts** for critical issues.
- **Centralized Logging**:
  - **Aggregates logs** from Angular, Spring Boot, and Kubernetes Pods.
  - Enables **search and correlation** for troubleshooting.
- **Cloud-Native Observability**:
  - Integrates with **Cloud Provider Monitoring** for end-to-end visibility.

---

## **7.10.3 Monitoring with Prometheus and Grafana**

---

### **7.10.3.1 Overview of Prometheus and Grafana**

- **Prometheus**:
  - **Metrics collection** and **alerting system**.
  - Uses a **pull-based model** to scrape metrics from **Kubernetes Pods**.
- **Grafana**:
  - **Interactive dashboards** for visualizing Prometheus metrics.
  - **Customizable alerts** for proactive monitoring.

---

### **7.10.3.2 Setting up Prometheus on Kubernetes**

---

#### **Step 1: Install Prometheus using Helm**

- **Helm** is a **package manager** for Kubernetes.

```sh
# Add Prometheus Helm Repository
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

# Update Helm Repositories
helm repo update

# Install Prometheus using Helm
helm install prometheus prometheus-community/kube-prometheus-stack --namespace monitoring --create-namespace
```

- **prometheus-community/kube-prometheus-stack**:
  - **Kube-Prometheus-Stack** provides:
    - **Prometheus** for metrics collection.
    - **Alertmanager** for alert notifications.
    - **Node Exporter** for node-level metrics.

---

#### **Step 2: Accessing Prometheus Dashboard**

```sh
# Port-forward Prometheus UI to localhost
kubectl port-forward svc/prometheus-kube-prometheus-prometheus -n monitoring 9090:9090
```

- **http://localhost:9090**:
  - Access **Prometheus Dashboard**.

---

### **7.10.3.3 Setting up Grafana on Kubernetes**

---

#### **Step 1: Accessing Grafana Dashboard**

```sh
# Port-forward Grafana UI to localhost
kubectl port-forward svc/prometheus-grafana -n monitoring 3000:80
```

- **http://localhost:3000**:
  - Access **Grafana Dashboard**.
- **Default Credentials**:
  - **Username**: `admin`
  - **Password**: `prom-operator`

---

#### **Step 2: Importing Pre-built Dashboards**

- **Grafana** provides **pre-built dashboards** for:
  - **Kubernetes Cluster Monitoring**.
  - **Node Exporter Metrics**.
  - **Spring Boot Metrics**.

- **Import Dashboard**:
  - Go to **Grafana UI > Dashboards > Import**.
  - Use Dashboard IDs from **Grafana's Dashboard Directory**:
    - **Kubernetes Cluster Monitoring** – `315`
    - **Node Exporter Full** – `1860`
    - **Spring Boot Micrometer** – `6756`

---

### **7.10.3.4 Monitoring Spring Boot Microservices**

---

#### **Step 1: Enable Actuator Metrics in Spring Boot**

**Example: application.properties**

```properties
# Enable Spring Boot Actuator Metrics
management.endpoints.web.exposure.include=prometheus
management.metrics.export.prometheus.enabled=true
management.metrics.export.prometheus.step=5s
management.metrics.tags.application=expense-service
```

- **management.endpoints.web.exposure.include=prometheus**:
  - Exposes **Prometheus metrics endpoint**.
- **management.metrics.export.prometheus.enabled=true**:
  - Enables **Prometheus metrics export**.
- **management.metrics.tags.application=expense-service**:
  - Adds a **custom application tag** for identification.

---

#### **Step 2: Expose Prometheus Metrics in Spring Boot**

**Example: pom.xml**

```xml
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```

- **Micrometer Prometheus Registry**:
  - Exposes metrics at `/actuator/prometheus`.

---

#### **Step 3: Configuring Prometheus to Scrape Metrics**

**Example: prometheus.yaml**

```yaml
scrape_configs:
  - job_name: 'spring-boot'
    metrics_path: '/actuator/prometheus'
    static_configs:
      - targets: ['expense-service:8082']
```

- **scrape_configs**:
  - **Job Name**: `spring-boot`
  - **Metrics Path**: `/actuator/prometheus`
  - **Targets**:
    - **expense-service** running on port `8082`.

---

#### **Step 4: Visualizing Metrics in Grafana**

- **Import Spring Boot Dashboard**:
  - **Dashboard ID**: `6756`
- **Visualize Metrics**:
  - **JVM Memory Usage**
  - **HTTP Request Metrics**
  - **Database Connection Pool Metrics**

---

## **7.10.4 Centralized Logging with ELK Stack**

---

### **7.10.4.1 Overview of ELK Stack**

- **ELK Stack**:
  - **Elasticsearch** – Full-text search engine for log storage.
  - **Logstash** – Data processing pipeline for ingesting logs.
  - **Kibana** – Visualization and analytics dashboard for Elasticsearch data.
- **Centralized Logging**:
  - **Aggregates logs** from Angular, Spring Boot, and Kubernetes Pods.
  - Enables **search and correlation** for faster troubleshooting.

---

### **7.10.4.2 Setting up ELK Stack on Kubernetes**

---

#### **Step 1: Install ELK Stack using Helm**

```sh
# Add Elastic Helm Repository
helm repo add elastic https://helm.elastic.co

# Update Helm Repositories
helm repo update

# Install Elasticsearch
helm install elasticsearch elastic/elasticsearch --namespace logging --create-namespace

# Install Kibana
helm install kibana elastic/kibana --namespace logging

# Install Filebeat for Log Ingestion
helm install filebeat elastic/filebeat --namespace logging
```

- **Helm Charts Used**:
  - **elasticsearch** – For log storage.
  - **kibana** – For visualization.
  - **filebeat** – Log forwarder for Kubernetes logs.

---

#### **Step 2: Accessing Kibana Dashboard**

```sh
# Port-forward Kibana UI to localhost
kubectl port-forward svc/kibana-kibana -n logging 5601:5601
```

- **http://localhost:5601**:
  - Access **Kibana Dashboard**.

---

#### **Step 3: Configuring Filebeat for Kubernetes Logs**

**Example: filebeat-values.yaml**

```yaml
filebeatConfig:
  filebeat.yml: |
    filebeat.inputs:
      - type: container
        paths:
          - /var/log/containers/*.log
        processors:
          - add_kubernetes_metadata:
              host: ${NODE_NAME}
              matchers:
                - logs_path:
                    logs_path: "/var/log/containers/"
    output.elasticsearch:
      hosts: ["http://elasticsearch:9200"]
```

- **filebeat.inputs**:
  - Collects logs from **Kubernetes containers**.
- **output.elasticsearch**:
  - Sends logs to **Elasticsearch** for storage.

---

### **7.10.4.3 Visualizing Logs in Kibana**

- **Create Index Pattern**:
  - **Index Pattern**: `filebeat-*`
- **Visualizations**:
  - **Log Search and Filtering**.
  - **Error Rate Analysis**.
  - **Correlation Analysis** for distributed tracing.

---

## **Next Steps:**
- **7.11 Advanced Security and Compliance**
  - Implementing **Zero Trust Security Model**
  - **OWASP Security Best Practices**

---

## **7.11 Advanced Security and Compliance**

---

## **7.11.1 Overview of Security and Compliance**

In this section, we will **enhance the security** of our Angular 19 and Spring Boot Microservices using:
- **Zero Trust Security Model** – Verify every request and enforce least privilege.
- **OWASP Security Best Practices** – Protect against common web vulnerabilities.
- **Security Compliance** – Achieving **SOC 2**, **ISO 27001**, and **GDPR Compliance**.
- **Cloud-Native Security** using:
  - **AWS Security Hub**, **Azure Security Center**, and **Google Cloud Security Command Center**.

---

### **7.11.2 Why Implement Advanced Security?**

- **Zero Trust Security Model**:
  - **Never trust, always verify** – Every request is authenticated and authorized.
  - Enforces **least privilege** and **micro-segmentation**.
- **OWASP Security Best Practices**:
  - Protects against:
    - **SQL Injection**, **XSS**, **CSRF**, and **Insecure Deserialization**.
- **Security Compliance**:
  - Ensures compliance with:
    - **SOC 2** – Data security, availability, and integrity.
    - **ISO 27001** – Information security management.
    - **GDPR** – Data protection and privacy.

---

## **7.11.3 Zero Trust Security Model**

---

### **7.11.3.1 Overview of Zero Trust Security**

- **Zero Trust Security Model**:
  - Assumes **no implicit trust** for any request, even within the network.
  - **Every request is verified** using:
    - **Strong authentication** with MFA.
    - **Contextual authorization** based on user roles, device, location, and behavior.
- **Key Principles**:
  - **Verify Explicitly** – Authenticate and authorize every request.
  - **Use Least Privilege Access** – Limit access to only necessary resources.
  - **Assume Breach** – Minimize blast radius and impact of breaches.

---

### **7.11.3.2 Implementing Zero Trust in Spring Boot**

---

#### **Step 1: Enforcing Strong Authentication with MFA**

- **Multi-Factor Authentication (MFA)** requires:
  - **Password** and **One-Time Password (OTP)** or **Biometrics**.

**Example: application.properties**

```properties
# Enable MFA
security.mfa.enabled=true
security.mfa.otp.expiration=300
security.mfa.otp.issuer=ExpenseApp
```

- **security.mfa.enabled=true**:
  - Enables **Multi-Factor Authentication**.
- **security.mfa.otp.expiration=300**:
  - **OTP expires** in **300 seconds (5 minutes)**.

---

#### **Step 2: Implementing OTP-based MFA in Spring Boot**

**Example: OtpService.java**

```java
package com.example.authservice.security;

import org.springframework.stereotype.Service;
import de.taimos.totp.TOTP;
import java.time.Instant;
import java.util.concurrent.ConcurrentHashMap;

@Service
public class OtpService {

    private static final ConcurrentHashMap<String, String> otpCache = new ConcurrentHashMap<>();

    // Generate OTP
    public String generateOtp(String username) {
        String otp = TOTP.getOTP(username);
        otpCache.put(username, otp);
        return otp;
    }

    // Validate OTP
    public boolean validateOtp(String username, String otp) {
        String cachedOtp = otpCache.get(username);
        return otp.equals(cachedOtp);
    }
}
```

- **generateOtp()**:
  - Generates **Time-based OTP** using **TOTP**.
- **validateOtp()**:
  - Validates **OTP** against the cached value.

---

#### **Step 3: Securing Endpoints with MFA**

**Example: SecurityConfig.java**

```java
package com.example.authservice.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .authorizeHttpRequests()
            .requestMatchers("/auth/login", "/auth/register").permitAll()
            .requestMatchers("/auth/otp-verify").authenticated()
            .anyRequest().authenticated()
            .and()
            .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);

        return http.build();
    }
}
```

- **/auth/otp-verify**:
  - Protected endpoint for **OTP verification**.
- **SessionCreationPolicy.STATELESS**:
  - **Stateless session management** for **JWT Authentication**.

---

### **7.11.3.3 Implementing Zero Trust in Angular**

---

#### **Step 1: Implementing Role-Based Access Control (RBAC)**

- **Angular Guards** enforce **role-based access** at:
  - **Route level** – Protects Angular routes.
  - **Component level** – Shows or hides components based on roles.

**Example: role.guard.ts**

```typescript
import { Injectable } from '@angular/core';
import { CanActivate, Router } from '@angular/router';
import { AuthService } from '../services/auth.service';

@Injectable({
  providedIn: 'root'
})
export class RoleGuard implements CanActivate {

  constructor(private authService: AuthService, private router: Router) {}

  canActivate(): boolean {
    if (this.authService.hasRole('ADMIN')) {
      return true;
    } else {
      this.router.navigate(['/unauthorized']);
      return false;
    }
  }
}
```

- **hasRole('ADMIN')**:
  - Checks if the user has the `ADMIN` role.
- **canActivate()**:
  - Allows navigation if the user is an `ADMIN`.
  - Redirects to **Unauthorized Page** otherwise.

---

#### **Step 2: Applying Role Guard to Routes**

**Example: app-routing.module.ts**

```typescript
const routes: Routes = [
  { path: 'admin', component: AdminDashboardComponent, canActivate: [RoleGuard] },
  { path: 'unauthorized', component: UnauthorizedComponent }
];
```

- **canActivate: [RoleGuard]**:
  - Protects the **Admin Dashboard** route with **RoleGuard**.
  - Only accessible to **ADMIN** users.

---

### **7.11.3.4 Contextual Authorization and Device Trust**

- **Contextual Authorization**:
  - **Dynamic authorization** based on:
    - **User role**.
    - **Device trust level**.
    - **Geolocation** and **IP Address**.
- **Device Trust**:
  - **Registers trusted devices** and enforces **device-based MFA**.
  - **Blocks unknown devices** for high-risk actions.

---

### **7.11.3.5 Implementing Device Trust in Angular and Spring Boot**

---

#### **Step 1: Device Registration and Trust Level**

- **Angular** collects **device fingerprint** using:
  - **User Agent**, **IP Address**, **Browser Type**, and **Operating System**.
- **Spring Boot** stores **device information** in the database.
- **Trusted Device Flow**:
  - **New Device**: Requires **MFA verification**.
  - **Trusted Device**: Allows access without additional verification.

---

## **7.11.4 OWASP Security Best Practices**

---

### **7.11.4.1 SQL Injection Protection**

- **Spring Data JPA**:
  - **Prepared Statements** for parameterized queries.
  - **QueryDSL** for dynamic queries.

**Example: Using QueryDSL**

```java
public List<User> findByUsername(String username) {
    QUser qUser = QUser.user;
    return (List<User>) userRepository.findAll(qUser.username.eq(username));
}
```

- **QueryDSL**:
  - Generates **type-safe queries** to prevent **SQL Injection**.

---

### **7.11.4.2 Cross-Site Scripting (XSS) Protection**

- **Angular**:
  - **Sanitizes HTML** automatically.
  - Use **DomSanitizer** for trusted HTML.

**Example: Using DomSanitizer**

```typescript
constructor(private sanitizer: DomSanitizer) {}

safeHtml(html: string) {
  return this.sanitizer.bypassSecurityTrustHtml(html);
}
```

---

## **Next Steps:**
- **7.12 Compliance and Data Privacy**
  - Implementing **GDPR Compliance** and **Data Privacy** Best Practices

---

## **7.12 Compliance and Data Privacy**

---

## **7.12.1 Overview of Compliance and Data Privacy**

In this section, we will ensure our **Angular 19 and Spring Boot Microservices** are compliant with:
- **GDPR (General Data Protection Regulation)** – Protects user data privacy.
- **SOC 2 (Service Organization Control 2)** – Security, availability, and integrity.
- **ISO 27001** – Information security management.
- **Data Privacy Best Practices** including:
  - **Data Encryption** – In transit and at rest.
  - **Data Anonymization and Pseudonymization**.
  - **Consent Management** and **Right to be Forgotten**.
- **Cloud-Native Security and Compliance** using:
  - **AWS Security Hub**, **Azure Security Center**, and **Google Cloud Security Command Center**.

---

### **7.12.2 Why Ensure Compliance and Data Privacy?**

- **Legal Compliance**:
  - **GDPR** imposes heavy penalties for non-compliance.
  - **SOC 2** and **ISO 27001** are mandatory for enterprise applications.
- **Data Privacy and Security**:
  - Protects **user data** from **unauthorized access** and **breaches**.
  - Builds **trust** and **credibility** with users.

---

## **7.12.3 GDPR Compliance in Angular and Spring Boot**

---

### **7.12.3.1 Overview of GDPR Requirements**

- **GDPR** mandates:
  - **Data Minimization** – Collect only necessary data.
  - **Purpose Limitation** – Data usage limited to the purpose for which it was collected.
  - **Consent Management** – Explicit consent for data collection and processing.
  - **Right to be Forgotten** – Users can request deletion of their personal data.
  - **Data Portability** – Users can request a copy of their data in a machine-readable format.
  - **Data Security** – Encryption of data in transit and at rest.

---

### **7.12.3.2 Implementing Consent Management in Angular**

---

#### **Step 1: Creating Consent Service**

**Example: consent.service.ts**

```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class ConsentService {
  private consentKey = 'user-consent';

  // Check if Consent is Given
  isConsentGiven(): boolean {
    return localStorage.getItem(this.consentKey) === 'true';
  }

  // Save Consent
  giveConsent(): void {
    localStorage.setItem(this.consentKey, 'true');
  }

  // Revoke Consent
  revokeConsent(): void {
    localStorage.removeItem(this.consentKey);
  }
}
```

- **isConsentGiven()**:
  - Checks if **user consent** is given.
- **giveConsent()**:
  - Saves consent in **Local Storage**.
- **revokeConsent()**:
  - **Removes consent** and disables data tracking.

---

#### **Step 2: Creating Consent Banner Component**

**Example: consent-banner.component.ts**

```typescript
import { Component } from '@angular/core';
import { ConsentService } from '../../services/consent.service';

@Component({
  selector: 'app-consent-banner',
  templateUrl: './consent-banner.component.html',
  styleUrls: ['./consent-banner.component.scss']
})
export class ConsentBannerComponent {
  constructor(private consentService: ConsentService) {}

  acceptConsent(): void {
    this.consentService.giveConsent();
  }

  declineConsent(): void {
    this.consentService.revokeConsent();
  }
}
```

**Example: consent-banner.component.html**

```html
<div class="consent-banner" *ngIf="!consentService.isConsentGiven()">
  <p>We use cookies to enhance your experience. Do you accept?</p>
  <button (click)="acceptConsent()">Accept</button>
  <button (click)="declineConsent()">Decline</button>
</div>
```

- **acceptConsent()**:
  - **Gives consent** and **enables tracking**.
- **declineConsent()**:
  - **Revokes consent** and **disables tracking**.

---

#### **Step 3: Showing Consent Banner Globally**

**Example: app.component.html**

```html
<app-consent-banner></app-consent-banner>
<router-outlet></router-outlet>
```

- **Consent Banner** is shown **globally** in the application.

---

### **7.12.3.3 Implementing Right to be Forgotten in Spring Boot**

---

#### **Step 1: Creating User Controller Endpoint**

**Example: UserController.java**

```java
package com.example.authservice.controller;

import com.example.authservice.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/user")
public class UserController {

    @Autowired
    private UserService userService;

    // Right to be Forgotten Endpoint
    @DeleteMapping("/delete")
    public String deleteUser(@RequestParam String username) {
        userService.deleteUser(username);
        return "User data deleted successfully";
    }
}
```

- **@DeleteMapping("/delete")**:
  - Endpoint for **Right to be Forgotten**.
- **deleteUser()**:
  - Deletes **user data** by **username**.

---

#### **Step 2: Implementing Data Deletion in UserService**

**Example: UserService.java**

```java
package com.example.authservice.service;

import com.example.authservice.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    // Delete User Data
    public void deleteUser(String username) {
        userRepository.deleteByUsername(username);
    }
}
```

- **deleteByUsername(username)**:
  - **Deletes user data** by **username**.

---

### **7.12.3.4 Data Portability in Spring Boot**

---

#### **Step 1: Exporting User Data as JSON**

**Example: UserController.java**

```java
// Data Portability Endpoint
@GetMapping("/export")
public User exportUserData(@RequestParam String username) {
    return userService.getUserByUsername(username);
}
```

- **@GetMapping("/export")**:
  - Endpoint for **Data Portability**.
- **getUserByUsername(username)**:
  - **Exports user data** as **JSON**.

---

#### **Step 2: Downloading User Data in Angular**

**Example: user.service.ts**

```typescript
exportUserData(username: string): Observable<any> {
  return this.http.get<any>(`${environment.authUrl}/user/export?username=${username}`, {
    responseType: 'json'
  });
}
```

**Example: user-profile.component.ts**

```typescript
downloadUserData(): void {
  this.userService.exportUserData(this.username).subscribe(data => {
    const blob = new Blob([JSON.stringify(data)], { type: 'application/json' });
    const url = window.URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = 'user-data.json';
    a.click();
  });
}
```

- **downloadUserData()**:
  - **Exports user data** as a **JSON file**.
  - Prompts the user to **download the file**.

---

### **7.12.3.5 Data Encryption in Spring Boot**

---

#### **Step 1: Enabling HTTPS**

**Example: application.properties**

```properties
server.port=8443
server.ssl.key-store=classpath:keystore.p12
server.ssl.key-store-password=changeit
server.ssl.key-store-type=PKCS12
server.ssl.key-alias=expenseapp
```

- **server.ssl.key-store**:
  - **Path to the keystore** containing the SSL certificate.
- **server.ssl.key-store-password**:
  - **Password** for the keystore.
- **server.ssl.key-store-type=PKCS12**:
  - **Keystore type** (`PKCS12` recommended for Spring Boot).
- **server.ssl.key-alias**:
  - **Alias** of the SSL certificate.

---

### **7.12.3.6 Data Encryption at Rest**

- **Encrypt sensitive data** using:
  - **JPA Attribute Converters**.
  - **AES Encryption** with **Jasypt** or **Vault**.

---

## **Next Steps:**
- **7.13 Final Wrap-Up and Real-World Project Completion**
  - Recap of **Angular 19 + Spring Boot** Integration
  - Real-World Project Completion and Best Practices

---

## **7.13 Final Wrap-Up and Real-World Project Completion**

---

## **7.13.1 Recap of Angular 19 + Spring Boot Integration**

Congratulations! You have successfully navigated through the **end-to-end journey** of building a **Real-World Full-Stack Application** using:
- **Angular 19** with **Signals**, **Typed Forms**, and **Advanced Performance Optimization**.
- **Spring Boot Microservices** with **JWT Authentication** and **Role-based Authorization**.
- **Microservices Architecture** with:
  - **Angular Micro Frontend**.
  - **Spring Boot Microservices** for **Auth** and **Expense Management**.
- **Containerization and Orchestration** using:
  - **Docker** and **Kubernetes**.
- **CI/CD Pipeline** with **GitHub Actions**.
- **Advanced Monitoring and Logging** with **Prometheus**, **Grafana**, and **ELK Stack**.
- **Zero Trust Security Model** and **GDPR Compliance** for enhanced security and data privacy.

---

## **7.13.2 Key Learnings and Best Practices**

### **Angular 19 Frontend**

- **Component Architecture**:
  - **Reusable Components** with **Typed Inputs** and **Typed Outputs**.
- **State Management**:
  - **Signals** for reactive state management.
  - **NgRx Store** for global state consistency.
- **Performance Optimization**:
  - **Change Detection Optimization** with **OnPush Strategy**.
  - **RxJS Best Practices** for efficient data handling.
- **Typed Forms and Advanced Routing**:
  - **Typed Reactive Forms** for type-safe validation.
  - **Advanced Routing Patterns** with **Lazy Loading** and **Route Guards**.

---

### **Spring Boot Microservices**

- **Microservices Architecture**:
  - **Auth Service** for **JWT Authentication** and **Role-based Authorization**.
  - **Expense Service** for **Expense Management** with **CRUD Operations**.
- **Spring Security**:
  - **JWT Authentication Flow** with **Access and Refresh Tokens**.
  - **Role-based Access Control (RBAC)** for secure endpoints.
- **Spring Data JPA and MySQL**:
  - **Repository Pattern** for database interactions.
  - **QueryDSL** for type-safe dynamic queries.
- **Zero Trust Security Model**:
  - **MFA (Multi-Factor Authentication)** and **Device Trust**.
  - **Contextual Authorization** for dynamic access control.

---

### **Microservices and Micro Frontend Architecture**

- **Micro Frontend Architecture**:
  - Angular application is **modularized by feature** for scalability.
- **Microservices Communication**:
  - **API Gateway** for centralized routing and load balancing.
  - **Service Discovery** with **Eureka Server**.
- **Containerization and Orchestration**:
  - **Docker** for consistent environments.
  - **Kubernetes (K8s)** for:
    - **Auto-scaling** and **Zero-downtime deployments**.
    - **Load Balancing** with **Ingress Controllers**.

---

### **CI/CD Pipeline and Cloud Deployment**

- **GitHub Actions** for CI/CD:
  - **Automated Build, Test, and Deployment** workflow.
  - **Docker Image Build and Push** to **Docker Hub**.
- **Cloud Deployment**:
  - **AWS EKS**, **Azure AKS**, and **Google GKE** for cloud-native deployment.
  - **Infrastructure as Code** with **Helm Charts** and **Kubernetes Manifests**.
- **Cloud-Native Security and Compliance**:
  - **AWS Security Hub**, **Azure Security Center**, and **Google Cloud Security Command Center**.

---

### **Monitoring, Logging, and Security**

- **Advanced Monitoring**:
  - **Prometheus** for metrics collection.
  - **Grafana** for interactive dashboards.
- **Centralized Logging**:
  - **ELK Stack (Elasticsearch, Logstash, Kibana)** for log aggregation and visualization.
- **Zero Trust Security Model**:
  - **Least Privilege Access** and **Dynamic Authorization**.
- **Compliance and Data Privacy**:
  - **GDPR Compliance** with **Consent Management** and **Right to be Forgotten**.
  - **Data Encryption** in transit and at rest.

---

## **7.13.3 Real-World Project Completion**

### **End-to-End Application Features**

- **Expense Management System** with:
  - **JWT Authentication** and **Role-based Authorization**.
  - **Real-Time State Management** with **Signals**.
  - **Interactive Dashboard** with **Angular Material Charts**.
  - **Responsive UI** with **Angular Material** and **SCSS**.
  - **Containerized Microservices Architecture** with:
    - **Auth Service** and **Expense Service**.
    - **API Gateway** for centralized routing.
- **Secure Deployment**:
  - **Dockerized Microservices** and **Angular Micro Frontend**.
  - **Kubernetes Deployment** on **AWS EKS**, **Azure AKS**, and **Google GKE**.
- **Compliance and Security**:
  - **Zero Trust Security Model** and **GDPR Compliance**.

---

### **Final Checklist for Real-World Deployment**

- [x] **Angular Frontend**:
  - [x] Responsive UI and Typed Forms.
  - [x] State Management with Signals and NgRx.
  - [x] Role-based Authorization with Angular Guards.
- [x] **Spring Boot Microservices**:
  - [x] JWT Authentication and Role-based Access Control.
  - [x] Secure APIs with Zero Trust Security Model.
- [x] **Containerization and Orchestration**:
  - [x] Dockerized Angular and Spring Boot Microservices.
  - [x] Kubernetes Deployment with Auto-scaling.
- [x] **CI/CD Pipeline**:
  - [x] Automated Build, Test, and Deployment using GitHub Actions.
- [x] **Advanced Monitoring and Logging**:
  - [x] Prometheus and Grafana for Monitoring.
  - [x] ELK Stack for Centralized Logging.
- [x] **Security and Compliance**:
  - [x] Zero Trust Security Model.
  - [x] GDPR Compliance and Data Privacy.

---

### **7.13.4 Real-World Best Practices**

- **Angular Best Practices**:
  - **Immutability** for **OnPush Strategy**.
  - **Typed Forms** and **Type-safe State Management**.
- **Spring Boot Best Practices**:
  - **JWT Expiration and Refresh Flow**.
  - **Role-based Access Control (RBAC)** with **@PreAuthorize**.
- **Microservices Best Practices**:
  - **API Gateway for Centralized Security**.
  - **Service Mesh** for inter-microservice communication.
- **CI/CD and DevOps Best Practices**:
  - **Blue-Green Deployment** and **Canary Releases**.
  - **Helm Charts** for Kubernetes Configuration Management.
- **Security Best Practices**:
  - **OWASP Security Best Practices** for API Security.
  - **Zero Trust Security Model** for dynamic access control.
- **Cloud-Native Observability**:
  - **Centralized Logging** and **Distributed Tracing**.
  - **Proactive Monitoring and Alerting**.

---

### **7.13.5 Final Words and Future Roadmap**

- **Congratulations!** You have successfully:
  - **Mastered Angular 19** with **Advanced Performance Optimization**.
  - **Integrated Angular with Spring Boot Microservices**.
  - **Containerized and Deployed** on **Kubernetes**.
  - **Secured and Complied** with **GDPR** and **Zero Trust Security**.
- **Future Roadmap**:
  - **Micro Frontend Architecture** with **Module Federation**.
  - **GraphQL Integration** with **Apollo Angular** and **Spring Boot**.
  - **Serverless Architecture** with **AWS Lambda** and **Azure Functions**.
  - **Event-Driven Microservices** with **Kafka** and **Event Sourcing**.
  - **Progressive Web App (PWA)** with **Angular Universal**.

---

### **Congratulations! You are now an Angular 19 + Spring Boot Expert!** 🚀
