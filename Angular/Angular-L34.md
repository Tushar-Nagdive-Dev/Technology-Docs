### **Lesson 34: Error Handling and Retry Strategies in NgRx Effects**

---

## **What You Will Learn:**
1. **Introduction to Error Handling in NgRx Effects**
   - Why Handle Errors in Effects?
   - Common Error Scenarios in NgRx
   - Core Concepts: catchError, throwError, and retry
   - Error Handling Best Practices in NgRx

2. **Handling Errors with catchError and throwError**
   - Using catchError to Handle Errors Gracefully
   - Creating Error Actions for State Management
   - Re-throwing Errors with throwError
   - Error Notification and Logging in Effects

3. **Implementing Retry Strategies in Effects**
   - When and Why to Use Retry in Effects
   - Implementing Retry with retry and retryWhen
   - Conditional Retry Logic for Specific Errors
   - Exponential Backoff and Delay Strategies

4. **Global Error Handling with Meta Reducers**
   - Centralized Error Handling with Meta Reducers
   - Managing Global Error State
   - Displaying Global Error Notifications
   - Logging Errors with NgRx Effects

5. **Error Notifications and User Feedback**
   - Dispatching Notification Actions on Errors
   - Integrating Angular Material Snackbar for Notifications
   - User-Friendly Error Messages and UI Feedback
   - Advanced Error Handling with Sentry and Logging Services

6. **Advanced Error Handling Patterns**
   - Handling Multiple Error Streams with forkJoin and zip
   - Recovering from Errors with catchError and continueWith
   - Using finalize for Cleanup in Effects
   - Error Handling in Complex Workflows and Transactions

7. **Hands-on Exercises:**
   - Implementing Retry Strategies and Exponential Backoff
   - Real-World Scenario: Error Handling in Payment Gateway Integration

8. **Expert Insights and Best Practices**
9. **Common Mistakes to Avoid**
10. **Recap and Next Steps**

---

## **1. Introduction to Error Handling in NgRx Effects**

### **1.1 Why Handle Errors in Effects?**
- **NgRx Effects** are responsible for handling **side effects** like:
  - HTTP requests and API calls.
  - State persistence in local storage.
  - Navigation and routing.
- **Error Handling** is crucial to:
  - Ensure **application stability** and **user experience**.
  - Prevent **application crashes** and **unhandled errors**.
  - Provide **meaningful error messages** and **UI feedback**.
  - Maintain **consistent state** and **error recovery**.

---

### **1.2 Common Error Scenarios in NgRx**
1. **HTTP Request Failures**:
   - Network issues, server downtime, or API errors.
   - Example: `404 Not Found`, `500 Internal Server Error`.

2. **Invalid User Input**:
   - Validation errors and form submission failures.

3. **Authentication and Authorization Errors**:
   - Unauthorized access (`401`) or forbidden actions (`403`).

4. **State Persistence Failures**:
   - Errors in saving or retrieving state from local storage or session storage.

5. **Unexpected Runtime Errors**:
   - Unhandled exceptions or null reference errors.

---

### **1.3 Core Concepts: catchError, throwError, and retry**

1. **catchError**:
   - Catches errors and **returns a new Observable**.
   - **Gracefully handles** errors without breaking the Observable stream.

2. **throwError**:
   - Re-throws an error to be handled by a higher-level catch block.
   - Used to **bubble up errors**.

3. **retry**:
   - Retries the Observable on error.
   - Useful for **transient errors** like network issues.

4. **retryWhen**:
   - Conditionally retries based on custom logic.
   - Can include **delays** and **exponential backoff**.

---

### **1.4 Error Handling Best Practices in NgRx**
- **Consistent Error Actions**:
  - Dispatch **error actions** to maintain a consistent state.

- **User-Friendly Error Messages**:
  - Display **user-friendly** error messages.
  - Avoid technical jargon.

- **Centralized Error Handling**:
  - Use **Meta Reducers** for centralized error handling.

- **Logging and Monitoring**:
  - Log errors with **logging services** like Sentry.

---

## **2. Handling Errors with catchError and throwError**

### **2.1 Using catchError to Handle Errors Gracefully**

**Example: Using catchError in Effects**

```typescript
loadProducts$ = createEffect(() =>
  this.actions$.pipe(
    ofType(ProductActions.loadProducts),
    switchMap(() => this.productService.getProducts().pipe(
      map(products => ProductActions.loadProductsSuccess({ products })),
      catchError(error => of(ProductActions.loadProductsFailure({ error })))
    ))
  )
);
```

- **catchError** catches errors and returns a new action (`loadProductsFailure`).
- Uses `of()` to **emit an Observable** with the error action.

---

### **2.2 Creating Error Actions for State Management**

**Example: Error Actions**

```typescript
export const loadProductsFailure = createAction(
  '[Product] Load Products Failure',
  props<{ error: string }>()
);
```

**Example: Handling Error Actions in Reducer**

```typescript
export const productReducer = createReducer(
  initialState,
  on(ProductActions.loadProductsFailure, (state, { error }) => ({
    ...state,
    loading: false,
    error
  }))
);
```

- **Error Actions** are dispatched when errors occur.
- The **error state** is updated in the reducer.

---

### **2.3 Re-throwing Errors with throwError**

**Example: Using throwError**

```typescript
loadProducts$ = createEffect(() =>
  this.actions$.pipe(
    ofType(ProductActions.loadProducts),
    switchMap(() => this.productService.getProducts().pipe(
      map(products => ProductActions.loadProductsSuccess({ products })),
      catchError(error => {
        console.error('Error loading products:', error);
        return throwError(() => new Error('Failed to load products.'));
      })
    ))
  )
);
```

- **throwError** re-throws the error.
- Used when errors need to be handled at a **higher level**.

---

### **2.4 Error Notification and Logging in Effects**

1. **Error Notification**

**Example: Dispatching Notification Action**

```typescript
catchError(error => of(NotificationActions.showError({ message: 'Failed to load products' })))
```

- Dispatches a **notification action** to show an error message.

2. **Logging Errors**

```typescript
catchError(error => {
  console.error('Error:', error);
  this.loggingService.logError(error);
  return of(ProductActions.loadProductsFailure({ error }));
})
```

- Logs errors with a **logging service** for debugging and monitoring.

---

## **3. Implementing Retry Strategies in Effects**

### **3.1 When and Why to Use Retry in Effects**
- **Retry** is useful for **transient errors** like:
  - Network connectivity issues.
  - Temporary server downtime.
  - Rate limiting or throttling errors (`429 Too Many Requests`).

- **Avoid Retrying** for:
  - **Client-side validation errors**.
  - **Authorization errors** (`401 Unauthorized`, `403 Forbidden`).

---

### **3.2 Implementing Retry with retry and retryWhen**

1. **Using retry**

```typescript
loadProducts$ = createEffect(() =>
  this.actions$.pipe(
    ofType(ProductActions.loadProducts),
    switchMap(() => this.productService.getProducts().pipe(
      retry(3),  // Retry up to 3 times
      map(products => ProductActions.loadProductsSuccess({ products })),
      catchError(error => of(ProductActions.loadProductsFailure({ error })))
    ))
  )
);
```

- Retries the request **3 times** before failing.

---

2. **Using retryWhen with Conditional Logic**

```typescript
loadProducts$ = createEffect(() =>
  this.actions$.pipe(
    ofType(ProductActions.loadProducts),
    switchMap(() => this.productService.getProducts().pipe(
      retryWhen(errors => errors.pipe(
        delay(2000),
        take(3),
        catchError(error => of(ProductActions.loadProductsFailure({ error })))
      )),
      map(products => ProductActions.loadProductsSuccess({ products }))
    ))
  )
);
```

- Retries with a **delay** of 2 seconds.
- **Retries up to 3 times**.

---

## **Next Lesson: Global Error Handling with Meta Reducers**
- **Centralized Error Handling with Meta Reducers**
- **Managing Global Error State**
- **Displaying Global Error Notifications**
- **Logging Errors with NgRx Effects**
