### **Lesson 13: Performance Optimization and Lazy Loading in Angular**

---

## **What You Will Learn:**
1. **Introduction to Performance Optimization in Angular**
   - Why Performance Optimization is Important
   - Common Performance Bottlenecks in Angular Applications
   - Overview of Angular's Change Detection Mechanism

2. **Lazy Loading in Angular**
   - What is Lazy Loading?
   - Why Use Lazy Loading?
   - Implementing Lazy Loading for Modules and Components

3. **Change Detection Strategies**
   - Default vs OnPush Change Detection
   - When to Use OnPush Strategy
   - Optimizing Change Detection with Immutable Data

4. **Performance Optimization Techniques**
   - Using trackBy with *ngFor
   - Caching and Memoization
   - Debouncing and Throttling for Event Handlers
   - Avoiding Unnecessary Renders

5. **Advanced Performance Optimization**
   - Preloading and QuickLink Strategy
   - Lazy Loading Images and Assets
   - Angular's Built-in Optimizations (Ivy Compiler, AOT Compilation)
   - Code Splitting and Bundle Analysis

6. **Hands-on Exercises:**
   - Implementing Lazy Loading for Feature Modules
   - Real-World Scenario: Optimizing a Product Catalog for Performance

7. **Expert Insights and Best Practices**
8. **Common Mistakes to Avoid**
9. **Recap and Next Steps**

---

## **1. Introduction to Performance Optimization in Angular**

### **1.1 Why Performance Optimization is Important**
- Improves **user experience** by reducing load times and enhancing UI responsiveness.
- Enhances **SEO** and **accessibility** by optimizing page rendering.
- Reduces **memory consumption** and **CPU usage**, leading to better battery life on mobile devices.
- Increases application scalability and maintainability.

---

### **1.2 Common Performance Bottlenecks in Angular Applications**
- **Large Initial Bundle Size**: Slows down page load times.
- **Frequent Change Detection Cycles**: Impacts rendering performance.
- **Unoptimized Loops in Templates**: Inefficient rendering with `*ngFor`.
- **Memory Leaks**: Not unsubscribing from Observables.
- **Unnecessary HTTP Requests**: Redundant API calls without caching.

---

### **1.3 Overview of Angular's Change Detection Mechanism**
- Angular uses the **Zone.js** library for change detection.
- **Change Detection** is triggered by:
  - User events (click, input, etc.)
  - Asynchronous operations (HTTP requests, Promises)
  - Timers (setTimeout, setInterval)
- **Default Change Detection** checks every component in the component tree, which can be inefficient.
- **OnPush Change Detection** optimizes performance by checking only when input properties change or events occur.

---

## **2. Lazy Loading in Angular**

### **2.1 What is Lazy Loading?**
- **Lazy Loading** is a design pattern that delays loading of resources until they are needed.
- It **reduces initial bundle size** and **improves loading time**.
- In Angular, lazy loading is typically applied to **feature modules**.

---

### **2.2 Why Use Lazy Loading?**
- **Improves Performance**: Loads only the necessary modules for the initial view.
- **Reduces Bundle Size**: Smaller initial bundles lead to faster download and parsing.
- **Better User Experience**: Improves perceived performance and responsiveness.

---

### **2.3 Implementing Lazy Loading for Modules**

**Step 1: Create a Feature Module**

```bash
ng generate module products --route products --module app.module
```

This will:
- Create a `products` module with routing configured.
- Add a lazy-loaded route in `AppRoutingModule`.

---

**Step 2: Configure Lazy Loading Route**

Open `src/app/app-routing.module.ts` and add:

```typescript
const routes: Routes = [
  { path: 'products', loadChildren: () => import('./products/products.module').then(m => m.ProductsModule) }
];
```

- `loadChildren` dynamically loads the `ProductsModule`.
- The module is loaded **only when** the user navigates to `/products`.

---

**Step 3: Organize Feature Module**

Open `src/app/products/products-routing.module.ts` and add:

```typescript
const routes: Routes = [
  { path: '', component: ProductListComponent },
  { path: ':id', component: ProductDetailComponent }
];
```

- `ProductListComponent` and `ProductDetailComponent` are loaded **only when** needed.

---

### **2.4 Lazy Loading Components**

Angular supports **Component-level Lazy Loading** using dynamic imports.

**Example: Lazy Loading a Dialog Component**

```typescript
async openDialog() {
  const { DialogComponent } = await import('./dialog/dialog.component');
  this.dialog.open(DialogComponent);
}
```

- The `DialogComponent` is imported **only when** `openDialog()` is called.

---

## **3. Change Detection Strategies**

### **3.1 Default vs OnPush Change Detection**

1. **Default Change Detection**:
   - Checks the entire component tree on every event or data change.
   - **Performance Impact**: Can lead to unnecessary checks and slow rendering.

2. **OnPush Change Detection**:
   - Checks only when:
     - @Input() properties change.
     - An event is triggered in the component.
     - An Observable emits a new value.
   - **Performance Gain**: Reduces change detection cycles for better performance.

**Example: Using OnPush Strategy**

```typescript
@Component({
  selector: 'app-product',
  templateUrl: './product.component.html',
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ProductComponent {
  @Input() product: Product;
}
```

- `ProductComponent` is checked **only when** the `product` input changes.

---

### **3.2 Optimizing Change Detection with Immutable Data**

- Use **immutable data structures** to optimize change detection.
- Instead of modifying an object directly, create a new instance.

**Example: Immutable Update**

```typescript
// Inefficient - Modifies existing object
this.product.name = 'New Product';

// Efficient - Creates a new object
this.product = { ...this.product, name: 'New Product' };
```

---

## **4. Performance Optimization Techniques**

### **4.1 Using trackBy with *ngFor**

- `trackBy` helps Angular identify which items have changed, added, or removed.

**Example: Using trackBy**

```html
<li *ngFor="let product of products; trackBy: trackByProductId">
  {{ product.name }}
</li>
```

```typescript
trackByProductId(index: number, product: Product): number {
  return product.id;
}
```

- Improves rendering performance for long lists.

---

### **4.2 Caching and Memoization**

- Cache HTTP requests to avoid redundant API calls.
- Use **memoization** for expensive computations.

**Example: Caching HTTP Request with RxJS shareReplay**

```typescript
getProducts(): Observable<Product[]> {
  return this.http.get<Product[]>(this.apiUrl).pipe(
    shareReplay(1)
  );
}
```

- `shareReplay(1)` caches the response and serves it to multiple subscribers.

---

### **4.3 Debouncing and Throttling for Event Handlers**

- Use **debounceTime** and **throttleTime** to limit event frequency.

**Example: Debouncing Input Search**

```typescript
search$ = new Subject<string>();

ngOnInit(): void {
  this.search$.pipe(
    debounceTime(300),
    switchMap(query => this.productService.searchProducts(query))
  ).subscribe();
}
```

- Reduces the number of API calls during user input.

---

### **4.4 Avoiding Unnecessary Renders**

- Use **ngIf** and **ngSwitch** instead of binding to `[hidden]`.
- Use **OnPush Change Detection** wherever possible.
- Avoid binding complex expressions directly in templates.

---

## **5. Advanced Performance Optimization**

### **5.1 Preloading and QuickLink Strategy**

- Preload lazy-loaded modules **after** the initial load.

**Example: Angular Preloading Strategy**

```typescript
RouterModule.forRoot(routes, { preloadingStrategy: PreloadAllModules })
```

### **5.2 Lazy Loading Images and Assets**

- Use **Lazy Loading** for images with `loading="lazy"` attribute.

**Example: Lazy Loading Image**

```html
<img src="product.jpg" loading="lazy" alt="Product Image">
```

---

## **Next Lesson: Security Best Practices in Angular**
- **Cross-Site Scripting (XSS) Prevention**
- **Cross-Site Request Forgery (CSRF) Protection**
- **HTTP Security Headers and Content Security Policy (CSP)**
- **JWT Authentication and Authorization**
- **Angular Security Features and Best Practices**
