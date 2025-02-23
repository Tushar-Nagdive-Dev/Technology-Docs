### **Lesson 35: Global Error Handling with Meta Reducers in NgRx**

---

## **What You Will Learn:**
1. **Introduction to Global Error Handling in NgRx**
   - Why Use Global Error Handling?
   - Centralized Error Management in Enterprise Applications
   - Core Concepts: Meta Reducers, Global State, and Notifications
   - Error Handling Best Practices for Scalable Apps

2. **Centralized Error Handling with Meta Reducers**
   - What are Meta Reducers in NgRx?
   - Creating Global Error Meta Reducer
   - Implementing Global Error Logging and Notifications
   - Using Meta Reducers for State Reset on Logout

3. **Managing Global Error State**
   - Designing a Global Error State Slice
   - Error State Normalization and Structure
   - Updating Error State from Meta Reducers
   - Accessing Global Error State with Selectors

4. **Displaying Global Error Notifications**
   - Dispatching Notification Actions on Global Errors
   - Integrating Angular Material Snackbar for Notifications
   - Centralized Error Handling in App Component
   - User-Friendly Error Messages and UI Feedback

5. **Logging Errors with NgRx Effects and Services**
   - Centralized Error Logging with Effects
   - Logging Errors with External Services (Sentry, LogRocket)
   - Error Reporting and Monitoring Best Practices
   - Debugging and Tracking Error Patterns

6. **Advanced Error Handling Patterns**
   - Handling Multiple Error Streams with combineLatest
   - Using finalize for Cleanup in Effects
   - Error Recovery and Retry Strategies
   - Integrating Global Error State with Router Guards

7. **Hands-on Exercises:**
   - Implementing Global Error Handling with Meta Reducers
   - Real-World Scenario: Centralized Error Logging and Notification System

8. **Expert Insights and Best Practices**
9. **Common Mistakes to Avoid**
10. **Recap and Next Steps**

---

## **1. Introduction to Global Error Handling in NgRx**

### **1.1 Why Use Global Error Handling?**
- **Global Error Handling** provides **centralized management** of errors in Angular applications.
- Key Benefits:
  - **Consistent Error Handling**: Consistent error messages and notifications across the application.
  - **Centralized Logging**: Centralized error logging and tracking for better debugging.
  - **Scalable Architecture**: Suitable for **large-scale enterprise applications** with complex error flows.
  - **User Experience**: Improved user experience with **user-friendly error messages**.

---

### **1.2 Centralized Error Management in Enterprise Applications**
- **Enterprise Applications** have complex error flows involving:
  - Multiple **HTTP requests** and **asynchronous operations**.
  - Complex **state dependencies** and **nested Observables**.
  - **Cross-cutting concerns** like authentication, authorization, and state persistence.
- Centralized Error Handling:
  - **Prevents duplication** of error handling logic.
  - **Improves maintainability** by separating error handling from components and services.
  - **Provides a single source of truth** for application errors.

---

### **1.3 Core Concepts: Meta Reducers, Global State, and Notifications**

1. **Meta Reducers**:
   - **Global reducers** that wrap existing reducers.
   - Handle **cross-cutting concerns** like error logging, state reset, and analytics.
   - Applied **globally** to all state changes.

2. **Global Error State**:
   - Centralized state slice for managing application-wide errors.
   - Stores error messages, status codes, and notifications.

3. **Notifications and User Feedback**:
   - **Consistent error notifications** using Angular Material Snackbar.
   - User-friendly error messages for better **user experience**.

---

### **1.4 Error Handling Best Practices for Scalable Apps**
- **Centralized Error State**:
  - Manage all errors in a **single state slice**.
  - Provides a **global view** of application errors.

- **Consistent Notification Mechanism**:
  - Use a consistent **notification mechanism** (e.g., Angular Material Snackbar).

- **Separation of Concerns**:
  - Separate **error handling logic** from components and services.
  - Use **Meta Reducers** and **Global Effects**.

- **Logging and Monitoring**:
  - Use external logging services like **Sentry**, **LogRocket**, or **New Relic**.
  - Centralized logging and monitoring for **better debugging and analytics**.

---

## **2. Centralized Error Handling with Meta Reducers**

### **2.1 What are Meta Reducers in NgRx?**
- **Meta Reducers** are **global reducers** that **wrap existing reducers**.
- They **intercept** every state transition.
- Useful for **cross-cutting concerns** like:
  - Global error handling.
  - Logging actions and state changes.
  - State persistence and restoration.
  - State reset on logout.

---

### **2.2 Creating Global Error Meta Reducer**

**app.reducer.ts**:

```typescript
import { ActionReducer, MetaReducer } from '@ngrx/store';
import { AppState } from './app.state';

export function globalErrorMetaReducer(reducer: ActionReducer<AppState>): ActionReducer<AppState> {
  return (state, action) => {
    try {
      return reducer(state, action);
    } catch (error) {
      console.error('Error occurred:', error);
      return {
        ...state,
        globalError: {
          message: error.message,
          stack: error.stack
        }
      };
    }
  };
}

export const metaReducers: MetaReducer<AppState>[] = [globalErrorMetaReducer];
```

- **globalErrorMetaReducer** wraps existing reducers and **catches errors**.
- Updates the **global error state** when an error occurs.

---

### **2.3 Implementing Global Error Logging and Notifications**

**Example: Global Error Logging with Meta Reducer**

```typescript
export function globalErrorMetaReducer(reducer: ActionReducer<AppState>): ActionReducer<AppState> {
  return (state, action) => {
    try {
      return reducer(state, action);
    } catch (error) {
      console.error('Global Error:', error);
      // Dispatch a global error notification
      // Use Angular Material Snackbar for notifications
      return {
        ...state,
        globalError: {
          message: error.message,
          stack: error.stack
        }
      };
    }
  };
}
```

- Logs **global errors** in the console.
- **Dispatches a global error notification**.

---

### **2.4 Using Meta Reducers for State Reset on Logout**

**Example: State Reset Meta Reducer**

```typescript
export function stateResetMetaReducer(reducer: ActionReducer<AppState>): ActionReducer<AppState> {
  return (state, action) => {
    if (action.type === '[Auth] Logout Success') {
      state = undefined;
    }
    return reducer(state, action);
  };
}

export const metaReducers: MetaReducer<AppState>[] = [stateResetMetaReducer];
```

- **Resets state** when `Logout Success` action is dispatched.
- Ensures **application state is cleared** on logout.

---

## **3. Managing Global Error State**

### **3.1 Designing a Global Error State Slice**

**global-error.state.ts**:

```typescript
export interface GlobalErrorState {
  message: string;
  statusCode: number;
  stack: string;
}
```

- **GlobalErrorState** stores:
  - Error **message**
  - **Status code** for HTTP errors
  - **Stack trace** for debugging

---

### **3.2 Error State Normalization and Structure**

**app.state.ts**:

```typescript
import { GlobalErrorState } from './global-error/state/global-error.state';

export interface AppState {
  globalError: GlobalErrorState;
}
```

- Centralized **AppState** with **GlobalErrorState** slice.

---

### **3.3 Updating Error State from Meta Reducers**

**global-error.reducer.ts**:

```typescript
import { createReducer, on } from '@ngrx/store';
import { GlobalErrorState } from './global-error.state';
import * as ErrorActions from './global-error.actions';

const initialState: GlobalErrorState = {
  message: '',
  statusCode: 0,
  stack: ''
};

export const globalErrorReducer = createReducer(
  initialState,
  on(ErrorActions.setError, (state, { error }) => ({
    ...state,
    message: error.message,
    statusCode: error.statusCode,
    stack: error.stack
  })),
  on(ErrorActions.clearError, state => ({
    ...state,
    message: '',
    statusCode: 0,
    stack: ''
  }))
);
```

- **setError** action updates the global error state.
- **clearError** action **resets the error state**.

---

## **Next Lesson: Displaying Global Error Notifications**
- **Dispatching Notification Actions on Global Errors**
- **Integrating Angular Material Snackbar for Notifications**
- **Centralized Error Handling in App Component**
- **User-Friendly Error Messages and UI Feedback**
