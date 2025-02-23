### **Lesson 37: Centralized Error Handling in App Component**

---

## **What You Will Learn:**
1. **Introduction to Centralized Error Handling in App Component**
   - Why Use Centralized Error Handling in App Component?
   - Centralized vs. Distributed Error Handling
   - Core Concepts: Global Error State, Notification Service, and App Component
   - Centralized Error Handling Patterns for Enterprise Apps

2. **Using Global Error State in App Component**
   - Selecting Global Error State with NgRx Selectors
   - Subscribing to Error State in App Component
   - Reactive Error Handling with RxJS Operators
   - Displaying Error Notifications Based on State Changes

3. **Displaying Notifications from App Component**
   - Triggering Notifications from App Component
   - Using Notification Service for Centralized Notifications
   - Displaying Snackbar Notifications with Angular Material
   - Customizing Snackbar Position, Duration, and Styling

4. **Clearing Error State after Notification Display**
   - Why Clear Error State after Displaying Notifications?
   - Dispatching clearError Action from App Component
   - Ensuring Error State Consistency Across Application
   - Managing Error State for Multi-Page Navigation

5. **Handling Multiple Notifications with Queue System**
   - Challenges of Multiple Concurrent Notifications
   - Implementing Queue System for Notifications
   - Sequential Display of Notifications using RxJS
   - Advanced Notification Patterns with Buffering and Debouncing

6. **Advanced Centralized Error Handling Patterns**
   - Using Angular ErrorHandler for Global Error Handling
   - Integrating Global Error State with Router Guards
   - Error Boundary Pattern for Component-Level Error Handling
   - Dynamic Error Messages and Contextual Feedback

7. **Hands-on Exercises:**
   - Implementing Centralized Error Handling in App Component
   - Real-World Scenario: Multi-Tenant Error Management and Notifications

8. **Expert Insights and Best Practices**
9. **Common Mistakes to Avoid**
10. **Recap and Next Steps**

---

## **1. Introduction to Centralized Error Handling in App Component**

### **1.1 Why Use Centralized Error Handling in App Component?**
- **Centralized Error Handling** in **App Component** provides:
  - **Consistent error notifications** across the application.
  - **Centralized control** over error handling logic.
  - Improved **maintainability** and **scalability**.
- **Key Benefits**:
  - **Consistent User Experience**: Uniform error messages and notifications.
  - **Centralized State Management**: Single source of truth for error state.
  - **Separation of Concerns**: Decouples error handling from components and services.

---

### **1.2 Centralized vs. Distributed Error Handling**
- **Centralized Error Handling**:
  - Error handling logic is managed in a **central location**.
  - **App Component** or **Global Error Service** manages notifications.
  - **Consistent notifications** and **state management**.

- **Distributed Error Handling**:
  - Error handling is managed in **individual components** or **services**.
  - Inconsistent error messages and **redundant logic**.
  - Difficult to maintain and scale.

---

### **1.3 Core Concepts: Global Error State, Notification Service, and App Component**

1. **Global Error State**:
   - Centralized state slice for managing application-wide errors.
   - Stores **error messages**, **status codes**, and **notifications**.

2. **Notification Service**:
   - Centralized service for displaying notifications.
   - Uses **Angular Material Snackbar** for non-intrusive alerts.

3. **App Component**:
   - Acts as a **centralized container** for error handling.
   - Subscribes to **Global Error State** and displays notifications.
   - Clears error state after displaying notifications.

---

### **1.4 Centralized Error Handling Patterns for Enterprise Apps**
- **Centralized Error State**:
  - Single state slice for managing application-wide errors.
  - Consistent and predictable state transitions.

- **Notification Queue System**:
  - Queue system for managing **multiple notifications**.
  - Displays notifications in **FIFO (First-In-First-Out)** order.

- **Reactive Error Handling**:
  - **RxJS Operators** for reactive error handling.
  - **Dynamic notifications** based on state changes.

- **Error Logging and Monitoring**:
  - Centralized error logging with external services.
  - **Sentry**, **LogRocket**, or **New Relic** for monitoring.

---

## **2. Using Global Error State in App Component**

### **2.1 Selecting Global Error State with NgRx Selectors**

**global-error.selectors.ts**:

```typescript
import { createSelector, createFeatureSelector } from '@ngrx/store';
import { GlobalErrorState } from './global-error.state';

export const selectGlobalErrorState = createFeatureSelector<GlobalErrorState>('globalError');

export const selectErrorMessage = createSelector(
  selectGlobalErrorState,
  (state: GlobalErrorState) => state.message
);

export const selectErrorStatusCode = createSelector(
  selectGlobalErrorState,
  (state: GlobalErrorState) => state.statusCode
);
```

- **createFeatureSelector** selects the **Global Error State**.
- **createSelector** derives the **error message** and **status code**.

---

### **2.2 Subscribing to Error State in App Component**

**app.component.ts**:

```typescript
import { Component, OnInit } from '@angular/core';
import { Store } from '@ngrx/store';
import { Observable } from 'rxjs';
import { AppState } from './app.state';
import * as ErrorSelectors from './global-error/state/global-error.selectors';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html'
})
export class AppComponent implements OnInit {
  errorMessage$: Observable<string>;

  constructor(private store: Store<AppState>) {}

  ngOnInit(): void {
    this.errorMessage$ = this.store.select(ErrorSelectors.selectErrorMessage);
  }
}
```

- **Subscribes** to `selectErrorMessage` using `store.select()`.
- **Reactive subscription** to error state changes.

---

### **2.3 Reactive Error Handling with RxJS Operators**

**Example: Reactive Error Handling with RxJS**

```typescript
ngOnInit(): void {
  this.errorMessage$ = this.store.select(ErrorSelectors.selectErrorMessage).pipe(
    filter(message => !!message), // Only if message is not empty
    debounceTime(300),            // Debounce for better UX
    distinctUntilChanged()         // Avoid duplicate notifications
  );
}
```

- **filter** to ignore empty error messages.
- **debounceTime** for better UX and reduced notification frequency.
- **distinctUntilChanged** to avoid duplicate notifications.

---

### **2.4 Displaying Error Notifications Based on State Changes**

**app.component.ts**:

```typescript
import { NotificationService } from './notification.service';

constructor(private store: Store<AppState>, private notificationService: NotificationService) {}

ngOnInit(): void {
  this.errorMessage$.subscribe(message => {
    if (message) {
      this.notificationService.showNotification(message, 'error');
      this.store.dispatch(ErrorActions.clearError());
    }
  });
}
```

- **Displays error notifications** using `NotificationService`.
- **Clears error state** after displaying the notification.

---

## **3. Displaying Notifications from App Component**

### **3.1 Triggering Notifications from App Component**

- **App Component** acts as a **centralized container** for notifications.
- **Subscribes** to Global Error State and **triggers notifications**.

**app.component.html**:

```html
<router-outlet></router-outlet>
```

- Notifications are **independent of routing**.
- Displayed across all routes and pages.

---

### **3.2 Using Notification Service for Centralized Notifications**

**notification.service.ts**:

```typescript
showNotification(message: string, type: 'error' | 'success' | 'info') {
  this.snackBar.open(message, 'Close', {
    duration: 3000,
    panelClass: [type],
    verticalPosition: 'top',
    horizontalPosition: 'right'
  });
}
```

- **Centralized notification logic** decoupled from components.
- **Consistent styling** and **positioning** for all notifications.

---

## **Next Lesson: Clearing Error State after Notification Display**
- **Why Clear Error State after Displaying Notifications?**
- **Dispatching clearError Action from App Component**
- **Ensuring Error State Consistency Across Application**
- **Managing Error State for Multi-Page Navigation**
