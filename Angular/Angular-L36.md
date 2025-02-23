### **Lesson 36: Displaying Global Error Notifications in NgRx**

---

## **What You Will Learn:**
1. **Introduction to Global Error Notifications**
   - Why Use Global Error Notifications?
   - User-Friendly Error Messages and UI Feedback
   - Core Concepts: Error Actions, Notifications, and Snackbar
   - Notification Patterns for Enterprise Applications

2. **Dispatching Notification Actions on Global Errors**
   - Creating Global Error Actions for Notifications
   - Dispatching Notification Actions from Meta Reducers
   - Using Effects to Listen for Global Error Actions
   - Triggering Notifications Based on Error State

3. **Integrating Angular Material Snackbar for Notifications**
   - Why Use Angular Material Snackbar for Notifications?
   - Setting up Angular Material Snackbar in Angular
   - Displaying Notifications with MatSnackBar
   - Customizing Snackbar Appearance and Duration

4. **Centralized Error Handling in App Component**
   - Using Global Error State in App Component
   - Displaying Notifications from App Component
   - Clearing Error State after Notification Display
   - Handling Multiple Notifications with Queue System

5. **User-Friendly Error Messages and UI Feedback**
   - Designing User-Friendly Error Messages
   - Mapping Error Codes to User-Friendly Messages
   - Dynamic Error Messages Based on User Context
   - Best Practices for Error Notifications and Feedback

6. **Advanced Error Notification Patterns**
   - Handling Multiple Error Streams with combineLatest
   - Notification Throttling and Debouncing
   - Displaying Success and Warning Notifications
   - Using Notification Service for Centralized Notifications

7. **Hands-on Exercises:**
   - Implementing Global Error Notifications with Snackbar
   - Real-World Scenario: Centralized Notification System for Multi-Tenant App

8. **Expert Insights and Best Practices**
9. **Common Mistakes to Avoid**
10. **Recap and Next Steps**

---

## **1. Introduction to Global Error Notifications**

### **1.1 Why Use Global Error Notifications?**
- **Global Error Notifications** provide **consistent error feedback** across the application.
- **Key Benefits**:
  - **Consistent User Experience**: Uniform error messages and notifications.
  - **User-Friendly Error Messages**: User-friendly and contextual error descriptions.
  - **Centralized Management**: Centralized notification logic for scalability and maintainability.
  - **Improved Debugging**: Detailed error logs for debugging and monitoring.

---

### **1.2 User-Friendly Error Messages and UI Feedback**
- **User-Friendly Error Messages**:
  - Avoid technical jargon and show meaningful messages.
  - Provide **contextual feedback** based on the user's action.

- **UI Feedback and Notifications**:
  - **Snackbar Notifications** for non-intrusive alerts.
  - **Dialog Boxes** for critical errors or confirmation.
  - **Inline Error Messages** for form validation.

---

### **1.3 Core Concepts: Error Actions, Notifications, and Snackbar**

1. **Error Actions**:
   - Dispatched when an error occurs.
   - Contains error information such as message and status code.
   - Example: `setError`, `clearError`, `showNotification`.

2. **Global Error State**:
   - Stores error messages, status codes, and stack traces.
   - Centralized state slice for application-wide error management.

3. **Snackbar Notifications**:
   - Non-intrusive notifications displayed at the bottom of the screen.
   - Suitable for **error alerts**, **success messages**, and **info notifications**.

---

### **1.4 Notification Patterns for Enterprise Applications**
- **Centralized Notification System**:
  - Centralized logic for displaying notifications.
  - Decoupled from components for maintainability.

- **Queue System for Multiple Notifications**:
  - Queue system for managing multiple notifications.
  - Displays notifications in **FIFO (First-In-First-Out)** order.

- **Notification Throttling and Debouncing**:
  - Throttling and debouncing to **limit the frequency** of notifications.

- **Dynamic Error Messages**:
  - Dynamic error messages based on **user context** and **error codes**.

---

## **2. Dispatching Notification Actions on Global Errors**

### **2.1 Creating Global Error Actions for Notifications**

**global-error.actions.ts**:

```typescript
import { createAction, props } from '@ngrx/store';

export const setError = createAction(
  '[Global Error] Set Error',
  props<{ message: string; statusCode: number }>()
);

export const clearError = createAction('[Global Error] Clear Error');

export const showNotification = createAction(
  '[Notification] Show Notification',
  props<{ message: string; notificationType: 'error' | 'success' | 'info' }>()
);
```

- **setError**: Sets the global error state.
- **clearError**: Clears the global error state.
- **showNotification**: Dispatches a notification action.

---

### **2.2 Dispatching Notification Actions from Meta Reducers**

**Example: Dispatching Notifications from Meta Reducer**

```typescript
import { ActionReducer, MetaReducer } from '@ngrx/store';
import { AppState } from './app.state';
import * as ErrorActions from './global-error/state/global-error.actions';

export function globalErrorMetaReducer(reducer: ActionReducer<AppState>): ActionReducer<AppState> {
  return (state, action) => {
    try {
      return reducer(state, action);
    } catch (error) {
      console.error('Global Error:', error);

      // Dispatch a notification action
      return {
        ...state,
        globalError: {
          message: error.message,
          statusCode: 500,
          stack: error.stack
        }
      };
    }
  };
}

export const metaReducers: MetaReducer<AppState>[] = [globalErrorMetaReducer];
```

- **Dispatches a notification action** on global error.
- **Updates the global error state** for consistent error management.

---

### **2.3 Using Effects to Listen for Global Error Actions**

**global-error.effects.ts**:

```typescript
import { Injectable } from '@angular/core';
import { Actions, createEffect, ofType } from '@ngrx/effects';
import * as ErrorActions from './global-error.actions';
import { map } from 'rxjs/operators';

@Injectable()
export class GlobalErrorEffects {
  showErrorNotification$ = createEffect(() =>
    this.actions$.pipe(
      ofType(ErrorActions.setError),
      map(action => ErrorActions.showNotification({ message: action.message, notificationType: 'error' }))
    )
  );

  constructor(private actions$: Actions) {}
}
```

- **Listens** for `setError` action.
- **Dispatches** `showNotification` action to trigger the snackbar.

---

### **2.4 Triggering Notifications Based on Error State**

**app.module.ts**:

```typescript
@NgModule({
  imports: [
    StoreModule.forRoot(reducers, { metaReducers }),
    EffectsModule.forRoot([GlobalErrorEffects])
  ]
})
export class AppModule {}
```

- Registers **GlobalErrorEffects** to **trigger notifications**.

---

## **3. Integrating Angular Material Snackbar for Notifications**

### **3.1 Why Use Angular Material Snackbar for Notifications?**
- **Angular Material Snackbar** provides:
  - **Non-intrusive notifications** displayed at the bottom.
  - **Consistent UI** and **customizable appearance**.
  - **Built-in animations** and **accessibility support**.

---

### **3.2 Setting up Angular Material Snackbar in Angular**

```bash
ng add @angular/material
```

- Adds **Angular Material** to the project.
- Installs **Angular Material Snackbar** for notifications.

---

### **3.3 Displaying Notifications with MatSnackBar**

**notification.service.ts**:

```typescript
import { Injectable } from '@angular/core';
import { MatSnackBar } from '@angular/material/snack-bar';

@Injectable({ providedIn: 'root' })
export class NotificationService {
  constructor(private snackBar: MatSnackBar) {}

  showNotification(message: string, type: 'error' | 'success' | 'info') {
    this.snackBar.open(message, 'Close', {
      duration: 3000,
      panelClass: [type]
    });
  }
}
```

- **showNotification** method displays snackbar with a **dynamic type**.
- **panelClass** sets the CSS class for custom styling.

---

### **3.4 Customizing Snackbar Appearance and Duration**

- **panelClass** allows **custom styling** for error, success, and info notifications.
- **duration** controls the **display duration** of the snackbar.

**Example: Custom Styles in styles.scss**:

```scss
.error {
  background-color: #f44336;
}

.success {
  background-color: #4caf50;
}

.info {
  background-color: #2196f3;
}
```

---

## **Next Lesson: Centralized Error Handling in App Component**
- **Using Global Error State in App Component**
- **Displaying Notifications from App Component**
- **Clearing Error State after Notification Display**
- **Handling Multiple Notifications with Queue System**

Ready to proceed to **Centralized Error Handling in App Component**, or do you need more examples on Global Error Notifications?
