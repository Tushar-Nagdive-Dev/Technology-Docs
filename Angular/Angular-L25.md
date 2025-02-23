### **Lesson 25: Angular State Management with NgRx**

---

## **What You Will Learn:**
1. **Introduction to NgRx and Redux Pattern**
   - What is NgRx?
   - Why Use NgRx in Angular Applications?
   - Understanding the Redux Pattern
   - Core Concepts: Actions, Reducers, and Selectors

2. **Setting up NgRx Store in Angular**
   - Installing NgRx Packages
   - Configuring Store Module
   - Defining State and Actions
   - Creating Reducers and Selectors

3. **NgRx Effects for Side Effects Management**
   - What are NgRx Effects?
   - Defining and Dispatching Effects
   - Handling Asynchronous Operations with Effects
   - Using RxJS Operators in Effects

4. **NgRx Entity for Managing Collections**
   - Introduction to NgRx Entity
   - Defining Entity State and Adapter
   - Managing CRUD Operations with NgRx Entity
   - Normalization of Data with Entity Adapter

5. **Advanced NgRx Patterns and Best Practices**
   - Using Feature Modules and Store Isolation
   - Memoized Selectors for Performance Optimization
   - Handling Complex State Changes with Reducer Composition
   - Testing NgRx Store, Effects, and Selectors

6. **Hands-on Exercises:**
   - Setting up NgRx Store and Effects for Product Management
   - Real-World Scenario: Shopping Cart State Management with NgRx Entity

7. **Expert Insights and Best Practices**
8. **Common Mistakes to Avoid**
9. **Recap and Next Steps**

---

## **1. Introduction to NgRx and Redux Pattern**

### **1.1 What is NgRx?**
- **NgRx** is a state management library for Angular applications.
- It is inspired by the **Redux pattern** and uses **RxJS** for reactive state management.
- **Core Features**:
  - **Centralized State Management**: Stores application state in a single place.
  - **Immutable State Changes**: State is immutable and updated using actions and reducers.
  - **Unidirectional Data Flow**: State flows in one direction, ensuring predictability.
  - **RxJS Integration**: Uses Observables for state management and side effects.

---

### **1.2 Why Use NgRx in Angular Applications?**
- **Centralized State**: Manage global state in a single place.
- **Predictable State Changes**: Unidirectional data flow makes state changes predictable.
- **Enhanced Debugging**: Time-travel debugging with NgRx DevTools.
- **Scalable Architecture**: Ideal for large-scale enterprise applications.
- **Reactive State Management**: Leverages RxJS for reactive programming.

---

### **1.3 Understanding the Redux Pattern**
- **Redux Pattern** is based on three core principles:
  1. **Single Source of Truth**:
     - State is stored in a **single store**.
  2. **State is Read-Only**:
     - State is **immutable** and updated using actions.
  3. **Changes are Made with Pure Functions**:
     - **Reducers** are pure functions that take the current state and action as input and return a new state.

---

### **1.4 Core Concepts of NgRx**

1. **State**:
   - Represents the application's global state.
   - Stored in a **single immutable object**.

2. **Actions**:
   - Describe events that **change state**.
   - Dispatched using `store.dispatch(action)`.

3. **Reducers**:
   - Pure functions that **handle state changes**.
   - Take the current state and action as input and return a new state.

4. **Selectors**:
   - Functions that **select slices of state**.
   - Used to **read state** from the store.

5. **Effects**:
   - Handle **side effects** like HTTP requests and routing.
   - Use **RxJS operators** for asynchronous operations.

---

## **2. Setting up NgRx Store in Angular**

### **2.1 Installing NgRx Packages**

Install the required NgRx packages:

```bash
npm install @ngrx/store @ngrx/effects @ngrx/entity @ngrx/store-devtools @ngrx/router-store
```

---

### **2.2 Configuring Store Module**

Add **StoreModule** to the `AppModule`:

```typescript
import { NgModule } from '@angular/core';
import { StoreModule } from '@ngrx/store';
import { StoreDevtoolsModule } from '@ngrx/store-devtools';
import { environment } from '../environments/environment';
import { reducers, metaReducers } from './reducers';

@NgModule({
  imports: [
    StoreModule.forRoot(reducers, { metaReducers }),
    StoreDevtoolsModule.instrument({ maxAge: 25, logOnly: environment.production })
  ]
})
export class AppModule {}
```

- `StoreModule.forRoot()` registers the root state and reducers.
- `StoreDevtoolsModule.instrument()` integrates NgRx DevTools for debugging.

---

### **2.3 Defining State and Actions**

1. **Defining State Interface**

```typescript
export interface ProductState {
  products: Product[];
  loading: boolean;
  error: string;
}
```

2. **Defining Actions**

**actions/product.actions.ts**:

```typescript
import { createAction, props } from '@ngrx/store';
import { Product } from '../models/product.model';

export const loadProducts = createAction('[Product] Load Products');
export const loadProductsSuccess = createAction('[Product] Load Products Success', props<{ products: Product[] }>());
export const loadProductsFailure = createAction('[Product] Load Products Failure', props<{ error: string }>());
```

- **createAction** creates strongly-typed actions.
- **props** defines the payload for actions.

---

### **2.4 Creating Reducers and Selectors**

1. **Creating Reducers**

**reducers/product.reducer.ts**:

```typescript
import { createReducer, on } from '@ngrx/store';
import * as ProductActions from '../actions/product.actions';
import { ProductState } from '../models/product.state';

const initialState: ProductState = {
  products: [],
  loading: false,
  error: ''
};

export const productReducer = createReducer(
  initialState,
  on(ProductActions.loadProducts, state => ({ ...state, loading: true })),
  on(ProductActions.loadProductsSuccess, (state, { products }) => ({ ...state, products, loading: false })),
  on(ProductActions.loadProductsFailure, (state, { error }) => ({ ...state, error, loading: false }))
);
```

- **createReducer** creates a strongly-typed reducer.
- **on** maps actions to state transitions.

---

2. **Creating Selectors**

**selectors/product.selectors.ts**:

```typescript
import { createSelector, createFeatureSelector } from '@ngrx/store';
import { ProductState } from '../models/product.state';

export const selectProductState = createFeatureSelector<ProductState>('products');

export const selectAllProducts = createSelector(
  selectProductState,
  (state: ProductState) => state.products
);

export const selectLoading = createSelector(
  selectProductState,
  (state: ProductState) => state.loading
);
```

- **createFeatureSelector** selects a feature slice of state.
- **createSelector** derives state data for the component.

---

### **2.5 Dispatching Actions and Using Selectors in Components**

**Component Example**:

```typescript
export class ProductListComponent implements OnInit {
  products$: Observable<Product[]> = this.store.select(selectAllProducts);
  loading$: Observable<boolean> = this.store.select(selectLoading);

  constructor(private store: Store) {}

  ngOnInit(): void {
    this.store.dispatch(loadProducts());
  }
}
```

- `store.select()` is used to **select slices of state**.
- `store.dispatch()` is used to **dispatch actions**.

---

## **3. NgRx Effects for Side Effects Management**

### **3.1 Defining and Dispatching Effects**

**effects/product.effects.ts**:

```typescript
import { Injectable } from '@angular/core';
import { Actions, createEffect, ofType } from '@ngrx/effects';
import { ProductService } from '../services/product.service';
import * as ProductActions from '../actions/product.actions';
import { catchError, map, mergeMap, of } from 'rxjs';

@Injectable()
export class ProductEffects {
  loadProducts$ = createEffect(() =>
    this.actions$.pipe(
      ofType(ProductActions.loadProducts),
      mergeMap(() => this.productService.getProducts().pipe(
        map(products => ProductActions.loadProductsSuccess({ products })),
        catchError(error => of(ProductActions.loadProductsFailure({ error })))
      ))
    )
  );

  constructor(private actions$: Actions, private productService: ProductService) {}
}
```

- **ofType** filters the actions.
- **mergeMap** maps to a new Observable (HTTP request).
- **createEffect** registers the effect.

---

## **Next Lesson: Building Angular Micro Frontends**
- **Introduction to Micro Frontends Architecture**
- **Module Federation with Angular**
- **Sharing State Across Micro Frontends**
- **Deploying and Scaling Micro Frontends**
