### **Lesson 42: Combining Route Resolvers with Router Guards**

---

## **What You Will Learn:**
1. **Why Combine Route Resolvers and Router Guards?**
   - Coordinating Resolvers and Guards for State Management
   - Core Concepts: Sequential Execution and State Consistency
   - Resolvers for Initialization and Guards for Validation
   - Best Practices for Combining Resolvers and Guards

2. **Coordinating Resolvers and Guards for State Management**
   - Role of Resolvers in State Initialization
   - Role of Guards in State Validation and Reset
   - Using Resolvers and Guards Together for Consistency
   - Managing Complex State Transitions with Resolvers and Guards

3. **Sequential Execution of Resolvers and Guards**
   - Understanding Execution Order of Resolvers and Guards
   - Ensuring Sequential State Initialization and Reset
   - Managing Asynchronous Data and Error State
   - Conditional State Transitions with Guards and Resolvers

4. **Ensuring State Consistency with Complex Navigation**
   - Challenges of Complex Navigation Flows
   - State Consistency Across Parent-Child Routes
   - Handling Dynamic and Lazy Loaded Routes
   - Error State Consistency with Guards and Resolvers

5. **Best Practices for Combining Resolvers and Guards**
   - When to Use Resolvers vs. Guards
   - Consistent State Initialization and Reset Patterns
   - Lazy Loading and Dynamic Routing Considerations
   - Performance Optimization with Resolvers and Guards

6. **Advanced State Management Patterns with Resolvers and Guards**
   - State Persistence and Restoration Across Routes
   - Dynamic State Transitions Based on User Actions
   - Error State Isolation and Recovery
   - Handling Asynchronous Errors and State Reset

7. **Hands-on Exercises:**
   - Implementing State Consistency with Resolvers and Guards
   - Real-World Scenario: Multi-Tenant Error State Management

8. **Expert Insights and Best Practices**
9. **Common Mistakes to Avoid**
10. **Recap and Next Steps**

---

## **1. Why Combine Route Resolvers and Router Guards?**

### **1.1 Coordinating Resolvers and Guards for State Management**
- **Resolvers and Guards** play different but complementary roles:
  - **Resolvers**: **Initialize state** before route activation.
  - **Guards**: **Validate and reset state** before or after navigation.
- **Why Combine Them?**:
  - **Consistent State Initialization and Reset**:
    - Resolvers initialize state, Guards reset state on navigation.
  - **Sequential Execution and Coordination**:
    - Ensures **sequential execution** of state initialization and validation.
  - **Complex State Transitions**:
    - Manage complex state transitions in **multi-page applications**.
  - **Error State Consistency**:
    - **Consistent error state** across parent-child routes and lazy loaded modules.

---

### **1.2 Core Concepts: Sequential Execution and State Consistency**

1. **Sequential Execution**:
   - **Resolvers execute before Guards**.
   - Ensures **state initialization** before validation or reset.
   - Ideal for **asynchronous data fetching** and **error state reset**.

2. **State Consistency**:
   - Ensures **consistent state transitions** across navigation.
   - **Avoids stale state** and **repeated notifications**.
   - **Isolates error state** for specific routes and modules.

3. **Dynamic State Transitions**:
   - **Dynamic state transitions** based on user actions and route conditions.
   - Ideal for **conditional navigation** and **multi-tenant applications**.

---

### **1.3 Resolvers for Initialization and Guards for Validation**

1. **Resolvers for State Initialization**:
   - **Initialize state** before route activation.
   - **Pre-fetch asynchronous data** for consistent UI rendering.
   - **Reset error state** for clean transitions.

2. **Guards for State Validation and Reset**:
   - **Validate state** before activating or deactivating a route.
   - **Reset state** after leaving a route.
   - **Prevent navigation** if state validation fails.

---

### **1.4 Best Practices for Combining Resolvers and Guards**
- **Consistent State Initialization and Reset**:
  - Use **Resolvers** for state initialization.
  - Use **Guards** for state reset and validation.
  - Ensures **consistent state transitions** and **error recovery**.

- **Sequential Execution and Coordination**:
  - **Resolvers execute before Guards** for sequential execution.
  - Ideal for **complex state transitions** and **multi-step workflows**.

- **State Isolation and Recovery**:
  - **Isolate state** for specific routes and modules.
  - **Recover error state** for dynamic navigation and lazy loaded modules.

- **Performance Optimization**:
  - **Lazy load state** with Resolvers and Guards.
  - Optimize performance with **conditional navigation**.

---

## **2. Coordinating Resolvers and Guards for State Management**

### **2.1 Role of Resolvers in State Initialization**
- **Resolvers** are executed **before** route activation.
- **Ideal for**:
  - **Pre-activation state initialization**.
  - **Pre-fetching asynchronous data** for UI rendering.
  - **Error state reset** before activating a new route.

**Example: State Initialization with Resolver**

```typescript
import { Injectable } from '@angular/core';
import { Resolve } from '@angular/router';
import { Observable, of } from 'rxjs';
import { Store } from '@ngrx/store';
import { AppState } from '../app.state';
import * as ErrorActions from './global-error/state/global-error.actions';

@Injectable({ providedIn: 'root' })
export class InitializeStateResolver implements Resolve<boolean> {
  constructor(private store: Store<AppState>) {}

  resolve(): Observable<boolean> {
    this.store.dispatch(ErrorActions.clearError());
    return of(true);
  }
}
```

- **Dispatches clearError action** before activating the route.
- **Clears error state** for consistent state transitions.
- **Initializes state** before loading the component.

---

### **2.2 Role of Guards in State Validation and Reset**
- **Guards** are executed **before or after** navigation.
- **Ideal for**:
  - **Pre-navigation state validation**.
  - **Post-deactivation state reset**.
  - **Preventing navigation** if state validation fails.

**Example: State Reset with CanDeactivate Guard**

```typescript
import { Injectable } from '@angular/core';
import { CanDeactivate } from '@angular/router';
import { Store } from '@ngrx/store';
import { AppState } from '../app.state';
import * as ErrorActions from './global-error/state/global-error.actions';

@Injectable({ providedIn: 'root' })
export class ClearErrorDeactivateGuard implements CanDeactivate<unknown> {
  constructor(private store: Store<AppState>) {}

  canDeactivate(): boolean {
    this.store.dispatch(ErrorActions.clearError());
    return true;
  }
}
```

- **Dispatches clearError action** before deactivating the route.
- **Resets error state** after leaving the route.
- **Prevents stale error state** across navigation.

---

### **2.3 Using Resolvers and Guards Together for Consistency**

**app-routing.module.ts**:

```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { ExampleComponent } from './example/example.component';
import { InitializeStateResolver } from './resolvers/initialize-state.resolver';
import { ClearErrorDeactivateGuard } from './guards/clear-error-deactivate.guard';

const routes: Routes = [
  {
    path: 'example',
    component: ExampleComponent,
    resolve: {
      initializeState: InitializeStateResolver
    },
    canDeactivate: [ClearErrorDeactivateGuard]
  }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}
```

- **Resolvers execute before Guards** for sequential execution.
- **Initialize state** before activating the route.
- **Reset error state** after leaving the route.
- **Ensures consistent state transitions** across navigation.

---

## **Next Lesson: Sequential Execution of Resolvers and Guards**
- **Understanding Execution Order of Resolvers and Guards**
- **Ensuring Sequential State Initialization and Reset**
- **Managing Asynchronous Data and Error State**
- **Conditional State Transitions with Guards and Resolvers**
