### **Lesson 9: State Management with NgRx in Angular**

---

## **What You Will Learn:**
1. **Introduction to NgRx and State Management**
   - What is NgRx?
   - Why Use NgRx?
   - Key Concepts: Store, Actions, Reducers, Effects, Selectors

2. **Setting up NgRx in Angular**
   - Installing NgRx Store, Effects, and DevTools
   - Configuring the Store Module
   - Creating Actions and Reducers

3. **State Management Patterns with NgRx**
   - Dispatching Actions
   - Using Selectors to Access State
   - Side Effects with NgRx Effects
   - Entity State Management with NgRx Entity

4. **Hands-on Exercises:**
   - Building a Product Catalog with NgRx
   - Real-World Scenario: Shopping Cart with State Management

5. **Expert Insights and Best Practices**
6. **Common Mistakes to Avoid**
7. **Recap and Next Steps**

---

## **1. Introduction to NgRx and State Management**

### **1.1 What is NgRx?**
- **NgRx** is a state management library for Angular applications.
- It is inspired by **Redux** and uses **RxJS** to manage state reactively.
- It provides a **unidirectional data flow** and centralizes the application's state in a single store.

---

### **1.2 Why Use NgRx?**
- **Predictable State Management**: Centralized state makes debugging easier.
- **Immutability**: State is immutable, ensuring consistency and reliability.
- **Performance Optimization**: Efficient change detection with OnPush strategy.
- **Time-Travel Debugging**: NgRx DevTools allow time-travel debugging.
- **Scalable Architecture**: Ideal for large enterprise-level applications.

---

### **1.3 Key Concepts in NgRx**

1. **Store**:
   - Centralized state container for the entire application.
   - Holds the application state as a single object tree.

2. **Actions**:
   - Describe unique events that trigger state changes.
   - Dispatched by components or services.
   - Example:
     ```typescript
     export const addProduct = createAction(
       '[Product List] Add Product',
       props<{ product: Product }>()
     );
     ```

3. **Reducers**:
   - Functions that handle state changes in response to actions.
   - **Pure functions**: They return a new state object without mutating the existing state.
   - Example:
     ```typescript
     const initialState: Product[] = [];

     const productReducer = createReducer(
       initialState,
       on(addProduct, (state, { product }) => [...state, product])
     );
     ```

4. **Selectors**:
   - Functions to read data from the store.
   - They provide a way to access slices of state.
   - Example:
     ```typescript
     export const selectProducts = createSelector(
       selectProductState,
       (state: ProductState) => state.products
     );
     ```

5. **Effects**:
   - Handle side effects like HTTP requests.
   - Listen to actions and perform asynchronous tasks.
   - Example:
     ```typescript
     loadProducts$ = createEffect(() =>
       this.actions$.pipe(
         ofType(loadProducts),
         mergeMap(() => this.productService.getProducts().pipe(
           map(products => loadProductsSuccess({ products })),
           catchError(error => of(loadProductsFailure({ error })))
         ))
       )
     );
     ```

---

## **2. Setting up NgRx in Angular**

### **2.1 Installing NgRx Store and Effects**

```bash
ng add @ngrx/store @ngrx/effects @ngrx/store-devtools @ngrx/entity
```

- **@ngrx/store** – Centralized state management.
- **@ngrx/effects** – Side effects management for asynchronous tasks.
- **@ngrx/store-devtools** – Debugging tools for time-travel and state inspection.
- **@ngrx/entity** – Entity state adapter for managing collections.

---

### **2.2 Configuring Store Module**

Open `src/app/app.module.ts` and add:

```typescript
import { StoreModule } from '@ngrx/store';
import { EffectsModule } from '@ngrx/effects';
import { StoreDevtoolsModule } from '@ngrx/store-devtools';
import { productReducer } from './store/reducers/product.reducer';
import { ProductEffects } from './store/effects/product.effects';

@NgModule({
  imports: [
    StoreModule.forRoot({ products: productReducer }),
    EffectsModule.forRoot([ProductEffects]),
    StoreDevtoolsModule.instrument({ maxAge: 25 })  // Enables time-travel debugging
  ]
})
export class AppModule {}
```

---

## **3. State Management Patterns with NgRx**

### **3.1 Creating Actions**

Create `src/app/store/actions/product.actions.ts`:

```typescript
import { createAction, props } from '@ngrx/store';
import { Product } from '../../models/product.model';

export const loadProducts = createAction('[Product] Load Products');
export const loadProductsSuccess = createAction(
  '[Product] Load Products Success',
  props<{ products: Product[] }>()
);
export const loadProductsFailure = createAction(
  '[Product] Load Products Failure',
  props<{ error: any }>()
);
```

---

### **3.2 Creating Reducers**

Create `src/app/store/reducers/product.reducer.ts`:

```typescript
import { createReducer, on } from '@ngrx/store';
import { Product } from '../../models/product.model';
import { loadProductsSuccess } from '../actions/product.actions';

export interface ProductState {
  products: Product[];
}

export const initialState: ProductState = {
  products: []
};

export const productReducer = createReducer(
  initialState,
  on(loadProductsSuccess, (state, { products }) => ({
    ...state,
    products: products
  }))
);
```

---

### **3.3 Using Selectors to Access State**

Create `src/app/store/selectors/product.selectors.ts`:

```typescript
import { createFeatureSelector, createSelector } from '@ngrx/store';
import { ProductState } from '../reducers/product.reducer';

export const selectProductState = createFeatureSelector<ProductState>('products');

export const selectAllProducts = createSelector(
  selectProductState,
  (state: ProductState) => state.products
);
```

---

### **3.4 Side Effects with NgRx Effects**

Create `src/app/store/effects/product.effects.ts`:

```typescript
import { Injectable } from '@angular/core';
import { Actions, createEffect, ofType } from '@ngrx/effects';
import { ProductService } from '../../services/product.service';
import { loadProducts, loadProductsSuccess, loadProductsFailure } from '../actions/product.actions';
import { mergeMap, map, catchError, of } from 'rxjs';

@Injectable()
export class ProductEffects {
  constructor(private actions$: Actions, private productService: ProductService) {}

  loadProducts$ = createEffect(() =>
    this.actions$.pipe(
      ofType(loadProducts),
      mergeMap(() => this.productService.getProducts().pipe(
        map(products => loadProductsSuccess({ products })),
        catchError(error => of(loadProductsFailure({ error })))
      ))
    )
  );
}
```

---

## **4. Hands-on Exercise: Product Catalog with NgRx**

1. **Create Product Actions, Reducers, and Selectors**.
2. **Implement ProductEffects** to load products from an API.
3. **Use Store in ProductListComponent** to display products.
4. **Dispatch Actions from Components**.
5. **Use Selectors to Access State**.

**Example: Dispatching Action and Selecting State**
```typescript
constructor(private store: Store) {}

ngOnInit(): void {
  this.store.dispatch(loadProducts());
  this.products$ = this.store.select(selectAllProducts);
}
```

**Template:**
```html
<ul>
  <li *ngFor="let product of products$ | async">
    {{ product.name }} - ${{ product.price }}
  </li>
</ul>
```

---

## **5. Expert Insights and Best Practices:**
- Use **NgRx Entity** for managing collections of entities.
- Organize state management logic in a modular folder structure.
- Use **createFeatureSelector** and **createSelector** for efficient state selection.
- Keep actions, reducers, effects, and selectors in separate files.

---

## **6. Common Mistakes to Avoid:**
- Mutating state inside reducers (always return a new state object).
- Forgetting to register reducers or effects in `AppModule`.
- Not unsubscribing from selectors in components.
- Using heavy computation in selectors (use `memoization`).

---

## **Next Lesson: Angular Material and Bootstrap Integration**
- **Using Angular Material Components**
- **Integrating Bootstrap for Responsive Design**
- **Theming and Customizing Styles**
