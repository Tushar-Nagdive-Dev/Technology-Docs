### **Lesson 40: Using Router Guards for Error State Management**

---

## **What You Will Learn:**
1. **Introduction to Router Guards for Error State Management**
   - Why Use Router Guards for Error State Management?
   - Core Concepts: CanActivate, CanDeactivate, and CanLoad
   - Error State Consistency with Router Guards
   - Best Practices for Router Guards in Enterprise Applications

2. **Clearing Error State with CanDeactivate Guard**
   - Why Use CanDeactivate for Error State Management?
   - Implementing CanDeactivate Guard for Error Reset
   - Clearing Error State on Component Exit
   - Ensuring Error State Consistency Across Navigation

3. **Managing Error State with CanLoad for Lazy Loading**
   - Why Use CanLoad for Lazy Loaded Modules?
   - Implementing CanLoad Guard for Error State Management
   - Clearing Error State Before Lazy Loading Modules
   - Ensuring State Consistency for Dynamic Routes

4. **State Reset with Route Resolvers**
   - Why Use Route Resolvers for Error State Reset?
   - Implementing Route Resolvers for State Initialization
   - Resetting Error State Before Route Activation
   - Combining Route Resolvers with Router Guards

5. **Combining Router Guards with Resolvers**
   - Coordinating Guards and Resolvers for State Management
   - Sequential Execution of Guards and Resolvers
   - Ensuring State Consistency with Complex Navigation
   - Best Practices for Combining Guards and Resolvers

6. **Advanced Error State Management Patterns**
   - Dynamic Error State Reset Based on User Actions
   - Error State Consistency Across Child Routes
   - State Persistence and Restoration with Guards
   - Handling Asynchronous Errors with Guards and Resolvers

7. **Hands-on Exercises:**
   - Implementing Error State Reset with Router Guards and Resolvers
   - Real-World Scenario: Multi-Tenant Error State Management

8. **Expert Insights and Best Practices**
9. **Common Mistakes to Avoid**
10. **Recap and Next Steps**

---

## **1. Introduction to Router Guards for Error State Management**

### **1.1 Why Use Router Guards for Error State Management?**
- **Router Guards** provide **fine-grained control** over route navigation.
- Guards can:
  - **Reset error state** before route activation.
  - **Clear error state** after route deactivation.
  - **Consistently manage state** across child routes.
  - **Prevent repeated error notifications** during navigation.

- **Key Benefits**:
  - **Consistent Error State**: Ensures consistent state transitions.
  - **State Isolation**: Isolates error state for specific routes.
  - **Dynamic Error Handling**: Clears state based on route conditions.
  - **Enhanced User Experience**: Prevents repetitive error messages.

---

### **1.2 Core Concepts: CanActivate, CanDeactivate, and CanLoad**

1. **CanActivate**:
   - Checks if a route can be activated.
   - Useful for **pre-activation error state reset**.
   - Example: Reset error state before loading a component.

2. **CanDeactivate**:
   - Checks if a route can be deactivated.
   - Useful for **post-deactivation state reset**.
   - Example: Clear error state when leaving a page.

3. **CanLoad**:
   - Checks if a lazy loaded module can be loaded.
   - Useful for **error state isolation** in lazy loaded modules.
   - Example: Clear error state before loading a dynamic module.

---

### **1.3 Error State Consistency with Router Guards**
- Ensures **error state consistency** across routes and modules.
- **Prevents stale error state** from leaking across components.
- **Avoids repeated notifications** on navigation and reload.
- **Isolates error state** for specific routes and modules.

---

### **1.4 Best Practices for Router Guards in Enterprise Applications**
- **Centralized State Management**:
  - Use **Global Error State** with NgRx for consistent state transitions.
  - Dispatch **Global Error Actions** from guards.

- **Consistent State Reset**:
  - Clear error state before activating new routes.
  - Reset state after deactivating current routes.

- **Lazy Loaded Modules**:
  - Use **CanLoad** to isolate state for lazy loaded modules.
  - Prevent error state leakage across dynamic routes.

- **Combining Guards and Resolvers**:
  - Use **Resolvers** for state initialization.
  - Use **Guards** for state reset and navigation control.

---

## **2. Clearing Error State with CanDeactivate Guard**

### **2.1 Why Use CanDeactivate for Error State Management?**
- **CanDeactivate** is triggered when **leaving a route**.
- Ideal for **post-deactivation state reset**:
  - **Clears error state** when navigating away from a page.
  - **Prevents stale error state** from affecting new routes.
  - **Ensures consistent state transitions** across navigation.

---

### **2.2 Implementing CanDeactivate Guard for Error Reset**

**clear-error-deactivate.guard.ts**:

```typescript
import { Injectable } from '@angular/core';
import { CanDeactivate } from '@angular/router';
import { Store } from '@ngrx/store';
import { AppState } from '../app.state';
import * as ErrorActions from './global-error/state/global-error.actions';

export interface CanComponentDeactivate {
  canDeactivate: () => boolean;
}

@Injectable({ providedIn: 'root' })
export class ClearErrorDeactivateGuard implements CanDeactivate<CanComponentDeactivate> {
  constructor(private store: Store<AppState>) {}

  canDeactivate(component: CanComponentDeactivate): boolean {
    this.store.dispatch(ErrorActions.clearError());
    return component.canDeactivate ? component.canDeactivate() : true;
  }
}
```

- **Dispatches clearError action** before deactivating the route.
- Ensures **error state is cleared** after leaving the page.
- **Prevents stale error state** from leaking to new routes.

---

### **2.3 Clearing Error State on Component Exit**

**example.component.ts**:

```typescript
import { Component } from '@angular/core';
import { CanComponentDeactivate } from './clear-error-deactivate.guard';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html'
})
export class ExampleComponent implements CanComponentDeactivate {
  canDeactivate(): boolean {
    return confirm('Are you sure you want to leave this page?');
  }
}
```

- **Implements CanComponentDeactivate** to control route deactivation.
- **Confirms navigation** before leaving the page.
- **Clears error state** if user confirms navigation.

---

### **2.4 Ensuring Error State Consistency Across Navigation**

**app-routing.module.ts**:

```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { ExampleComponent } from './example/example.component';
import { ClearErrorDeactivateGuard } from './guards/clear-error-deactivate.guard';

const routes: Routes = [
  {
    path: 'example',
    component: ExampleComponent,
    canDeactivate: [ClearErrorDeactivateGuard]
  }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}
```

- **Registers CanDeactivate Guard** for the `example` route.
- **Clears error state** when leaving the `example` component.

---

## **3. Managing Error State with CanLoad for Lazy Loading**

### **3.1 Why Use CanLoad for Lazy Loaded Modules?**
- **Lazy Loaded Modules** are loaded **dynamically**.
- Error state can **leak** into lazy loaded modules.
- **CanLoad** isolates error state for **dynamic routes**:
  - Clears error state before loading a lazy module.
  - Ensures **error state consistency** for dynamic navigation.

---

### **3.2 Implementing CanLoad Guard for Error State Management**

**clear-error-load.guard.ts**:

```typescript
import { Injectable } from '@angular/core';
import { CanLoad, Route, UrlSegment } from '@angular/router';
import { Store } from '@ngrx/store';
import { AppState } from '../app.state';
import * as ErrorActions from './global-error/state/global-error.actions';

@Injectable({ providedIn: 'root' })
export class ClearErrorLoadGuard implements CanLoad {
  constructor(private store: Store<AppState>) {}

  canLoad(route: Route, segments: UrlSegment[]): boolean {
    this.store.dispatch(ErrorActions.clearError());
    return true;
  }
}
```

- **Clears error state** before loading a lazy module.
- **Prevents state leakage** across lazy loaded routes.
- Ensures **consistent state transitions** for dynamic navigation.

---

## **Next Lesson: State Reset with Route Resolvers**
- **Why Use Route Resolvers for Error State Reset?**
- **Implementing Route Resolvers for State Initialization**
- **Resetting Error State Before Route Activation**
- **Combining Route Resolvers with Router Guards**
