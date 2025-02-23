### **Lesson 39: Managing Error State for Multi-Page Navigation**

---

## **What You Will Learn:**
1. **Challenges of Error State in Multi-Page Navigation**
   - Why Error State Management is Crucial for Multi-Page Apps
   - Common Pitfalls of Error State in Multi-Page Navigation
   - Core Concepts: Navigation Events, State Consistency, and Error Reset
   - Error State Management Patterns for Multi-Page Applications

2. **Clearing Error State on Navigation Start and End**
   - Using Angular Router Events for State Management
   - Clearing Error State on Navigation Start
   - Resetting Error State on Navigation End
   - Handling Error State for Lazy Loaded Routes

3. **Using Router Guards for Error State Management**
   - Why Use Router Guards for Error State Management?
   - Implementing CanActivate Guard for Error State Reset
   - Clearing Error State with CanDeactivate Guard
   - Managing Error State with CanLoad for Lazy Loading

4. **Error State Reset with Route Resolvers**
   - Why Use Route Resolvers for Error State Reset?
   - Implementing Route Resolvers for State Initialization
   - Resetting Error State Before Route Activation
   - Combining Route Resolvers with Router Guards

5. **Advanced Error State Management Patterns**
   - Error State Persistence with Local Storage
   - Resetting Error State on User Actions and Events
   - Error State Consistency Across Child Routes
   - Handling Asynchronous Errors and State Reset

6. **Error State Testing and Debugging**
   - Unit Testing Error State Reset and Navigation Events
   - Mocking Router Events and Guards in Tests
   - Debugging Error State Flow with Redux DevTools
   - Best Practices for Testing Error State Management

7. **Hands-on Exercises:**
   - Implementing Error State Reset for Multi-Page Navigation
   - Real-World Scenario: Error State Management in Multi-Tenant App

8. **Expert Insights and Best Practices**
9. **Common Mistakes to Avoid**
10. **Recap and Next Steps**

---

## **1. Challenges of Error State in Multi-Page Navigation**

### **1.1 Why Error State Management is Crucial for Multi-Page Apps**
- Multi-Page Angular applications have:
  - **Complex navigation flows** and **multiple routes**.
  - **Lazy loaded modules** and **dynamic routing**.
  - **Shared state** across pages and modules.
- **Error State Management** is crucial for:
  - **Consistent error handling** across routes.
  - **Accurate state transitions** and **error recovery**.
  - **User Experience**: Preventing repeated notifications on navigation.

---

### **1.2 Common Pitfalls of Error State in Multi-Page Navigation**
1. **Stale Error State**:
   - Error state persists when navigating between routes.
   - Displays **outdated error messages** for new actions.

2. **Repeated Error Notifications**:
   - Same error notification is shown on every navigation.
   - Occurs when state is not reset on route change.

3. **Inconsistent Error State**:
   - Error state becomes inconsistent across child routes.
   - Affects **conditional rendering** and **UI consistency**.

4. **State Leakage and Memory Issues**:
   - Accumulation of old errors in state.
   - **Memory leaks** and performance degradation.

---

### **1.3 Core Concepts: Navigation Events, State Consistency, and Error Reset**

1. **Navigation Events**:
   - **Angular Router Events** trigger on route change.
   - Key Events:
     - `NavigationStart`: Triggered when navigation starts.
     - `NavigationEnd`: Triggered when navigation ends.
     - `NavigationError`: Triggered on navigation error.
     - `NavigationCancel`: Triggered when navigation is canceled.

2. **State Consistency**:
   - **Consistent error state** across route changes.
   - **Accurate state transitions** and **UI rendering**.

3. **Error Reset**:
   - **Clears error state** on route change.
   - Prevents repeated error notifications.

---

### **1.4 Error State Management Patterns for Multi-Page Applications**
- **Centralized Error State Management**:
  - Single state slice for managing application-wide errors.
  - Consistent state transitions with **Global Error Actions**.

- **Router Event-Based State Reset**:
  - Reset error state on **NavigationStart** or **NavigationEnd**.
  - Ensures state consistency during navigation.

- **Router Guards and Resolvers**:
  - **CanActivate** and **CanDeactivate** guards for state reset.
  - **Route Resolvers** for state initialization.

- **Error State Consistency**:
  - Error state consistency across **lazy loaded routes**.
  - **Reset error state** before activating new routes.

---

## **2. Clearing Error State on Navigation Start and End**

### **2.1 Using Angular Router Events for State Management**
- **Angular Router** provides **Navigation Events**:
  - **NavigationStart**: Triggered when navigation starts.
  - **NavigationEnd**: Triggered when navigation ends.
  - **NavigationError**: Triggered on navigation error.
  - **NavigationCancel**: Triggered when navigation is canceled.

- **Router Events** can be used to:
  - **Clear error state** on navigation start.
  - **Reset error state** on navigation end.
  - **Consistently manage error state** during navigation.

---

### **2.2 Clearing Error State on Navigation Start**

**app.component.ts**:

```typescript
import { Component, OnInit } from '@angular/core';
import { Router, NavigationStart } from '@angular/router';
import { Store } from '@ngrx/store';
import { AppState } from './app.state';
import * as ErrorActions from './global-error/state/global-error.actions';
import { filter } from 'rxjs/operators';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html'
})
export class AppComponent implements OnInit {
  constructor(private router: Router, private store: Store<AppState>) {}

  ngOnInit(): void {
    this.router.events.pipe(
      filter(event => event instanceof NavigationStart)
    ).subscribe(() => {
      this.store.dispatch(ErrorActions.clearError());
    });
  }
}
```

- **Listens** for `NavigationStart` events.
- **Clears error state** before navigating to a new route.
- Ensures **consistent state transitions** during navigation.

---

### **2.3 Resetting Error State on Navigation End**

**Example: Resetting Error State on Navigation End**

```typescript
this.router.events.pipe(
  filter(event => event instanceof NavigationEnd)
).subscribe(() => {
  this.store.dispatch(ErrorActions.clearError());
});
```

- **Resets error state** after navigation completes.
- Prevents repeated error notifications on page reload.

---

### **2.4 Handling Error State for Lazy Loaded Routes**
- **Lazy loaded routes** are loaded **dynamically**.
- Error state can **leak** into lazy loaded modules.
- **Clear error state** before loading lazy modules.

**Example: Handling Error State for Lazy Loaded Routes**

```typescript
this.router.events.pipe(
  filter(event => event instanceof NavigationStart),
  filter(event => event.url.includes('lazy'))
).subscribe(() => {
  this.store.dispatch(ErrorActions.clearError());
});
```

- Checks if the route contains `lazy` keyword.
- Clears error state before loading **lazy loaded module**.

---

## **3. Using Router Guards for Error State Management**

### **3.1 Why Use Router Guards for Error State Management?**
- **Router Guards** provide **fine-grained control** over route navigation.
- Guards can:
  - **Reset error state** before route activation.
  - **Clear error state** after route deactivation.
  - **Consistently manage state** across child routes.

- **Types of Router Guards**:
  - **CanActivate**: Checks if a route can be activated.
  - **CanDeactivate**: Checks if a route can be deactivated.
  - **CanLoad**: Checks if a module can be lazy loaded.

---

### **3.2 Implementing CanActivate Guard for Error State Reset**

**clear-error.guard.ts**:

```typescript
import { Injectable } from '@angular/core';
import { CanActivate } from '@angular/router';
import { Store } from '@ngrx/store';
import { AppState } from '../app.state';
import * as ErrorActions from './global-error/state/global-error.actions';

@Injectable({ providedIn: 'root' })
export class ClearErrorGuard implements CanActivate {
  constructor(private store: Store<AppState>) {}

  canActivate(): boolean {
    this.store.dispatch(ErrorActions.clearError());
    return true;
  }
}
```

- **Dispatches clearError action** before route activation.
- Ensures **error state is cleared** before navigating to a new route.

---

## **Next Lesson: Using Router Guards for Error State Management**
- **Clearing Error State with CanDeactivate Guard**
- **Managing Error State with CanLoad for Lazy Loading**
- **State Reset with Route Resolvers**
- **Combining Router Guards with Resolvers**
