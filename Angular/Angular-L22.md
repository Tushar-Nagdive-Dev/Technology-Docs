### **Lesson 22: Angular Advanced Routing and Navigation**

---

## **What You Will Learn:**
1. **Introduction to Angular Routing and Navigation**
   - Overview of Angular Router
   - Why Use Advanced Routing Techniques?
   - Key Concepts: Routes, RouterOutlet, RouterLink, ActivatedRoute

2. **Nested Routes and Lazy Loading**
   - Defining Nested Routes for Child Components
   - Implementing Nested RouterOutlets
   - Lazy Loading Feature Modules
   - Preloading Strategies for Better Performance

3. **Dynamic Route Parameters and Query Params**
   - Passing and Retrieving Route Parameters
   - Using Route Parameters in Components
   - Query Parameters and Preserving Query Params
   - Optional Parameters and Default Values

4. **Route Guards and Role-Based Authorization**
   - Types of Route Guards:
     - CanActivate
     - CanDeactivate
     - CanActivateChild
     - CanLoad
   - Implementing Role-Based Authorization
   - Protecting Routes with AuthGuard

5. **Advanced Navigation Techniques**
   - Imperative Navigation using Router
   - Conditional Navigation and Route Reuse
   - Redirects, Wildcard Routes, and Fallbacks
   - Angular Router Events and Observables

6. **Hands-on Exercises:**
   - Implementing Nested Routes and Lazy Loading
   - Real-World Scenario: Role-Based Access Control with Route Guards

7. **Expert Insights and Best Practices**
8. **Common Mistakes to Avoid**
9. **Recap and Next Steps**

---

## **1. Introduction to Angular Routing and Navigation**

### **1.1 Overview of Angular Router**
- **Angular Router** is a powerful module for:
  - Navigating between different views and components.
  - Managing URL parameters and query strings.
  - Supporting deep linking and SEO-friendly URLs.
  - Lazy loading modules for better performance.

### **1.2 Why Use Advanced Routing Techniques?**
- Organize complex navigation flows with **Nested Routes**.
- Improve application performance with **Lazy Loading**.
- Secure routes using **Role-Based Authorization**.
- Enhance user experience with **Dynamic Route Parameters** and **Query Params**.
- Control navigation flow with **Router Events** and **Guards**.

---

### **1.3 Key Concepts**

1. **Routes**:
   - Define paths and their corresponding components.
   - Example:
     ```typescript
     const routes: Routes = [
       { path: 'home', component: HomeComponent },
       { path: 'products', component: ProductListComponent },
       { path: '**', redirectTo: 'home' }
     ];
     ```

2. **RouterOutlet**:
   - Acts as a placeholder for rendering routed components.
   - Example:
     ```html
     <router-outlet></router-outlet>
     ```

3. **RouterLink**:
   - Directive for navigation.
   - Example:
     ```html
     <a routerLink="/products">View Products</a>
     ```

4. **ActivatedRoute**:
   - Provides access to route parameters and query parameters.
   - Example:
     ```typescript
     this.route.params.subscribe(params => {
       console.log(params['id']);
     });
     ```

---

## **2. Nested Routes and Lazy Loading**

### **2.1 Defining Nested Routes for Child Components**

- Nested routes allow hierarchical routing within a feature module.

**Example: Defining Nested Routes**

```typescript
const routes: Routes = [
  {
    path: 'products',
    component: ProductComponent,
    children: [
      { path: '', component: ProductListComponent },
      { path: ':id', component: ProductDetailComponent }
    ]
  }
];
```

**Example: Using Nested RouterOutlet**

```html
<router-outlet></router-outlet>
```

- The `ProductListComponent` or `ProductDetailComponent` is rendered within the `ProductComponent`.

---

### **2.2 Implementing Nested RouterOutlets**

- Nested `router-outlet` tags are used for child routes.
- Example:
  ```html
  <router-outlet></router-outlet>
  ```

- The parent component should include `<router-outlet>` where child components should be rendered.

---

### **2.3 Lazy Loading Feature Modules**

- Lazy loading delays the loading of feature modules until needed.
- This reduces the **initial bundle size** and **improves performance**.

**Example: Lazy Loading a Feature Module**

**App Routing Module**:
```typescript
const routes: Routes = [
  {
    path: 'products',
    loadChildren: () => import('./products/products.module').then(m => m.ProductsModule)
  }
];
```

- `ProductsModule` is loaded **only when** the user navigates to `/products`.

---

### **2.4 Preloading Strategies for Better Performance**

1. **NoPreloading**: Default behavior, no modules are preloaded.
2. **PreloadAllModules**: Preloads all lazy-loaded modules **after** the initial load.
3. **Custom Preloading Strategy**: Conditionally preload specific modules.

**Example: PreloadAllModules Strategy**

```typescript
RouterModule.forRoot(routes, { preloadingStrategy: PreloadAllModules })
```

**Example: Custom Preloading Strategy**

```typescript
export class CustomPreloadingStrategy implements PreloadingStrategy {
  preload(route: Route, load: () => Observable<unknown>): Observable<unknown> {
    return route.data && route.data.preload ? load() : of(null);
  }
}
```

**Usage in Routing Module**:
```typescript
{ path: 'products', loadChildren: () => import('./products/products.module').then(m => m.ProductsModule), data: { preload: true } }
```

- Conditionally preloads the `ProductsModule` based on the `preload` property.

---

## **3. Dynamic Route Parameters and Query Params**

### **3.1 Passing and Retrieving Route Parameters**

- Route parameters are dynamic values in the URL.

**Example: Defining Route Parameters**

```typescript
const routes: Routes = [
  { path: 'products/:id', component: ProductDetailComponent }
];
```

**Example: Navigating with Parameters**

```html
<a [routerLink]="['/products', product.id]">View Details</a>
```

**Example: Retrieving Route Parameters**

```typescript
this.route.params.subscribe(params => {
  console.log(params['id']);
});
```

- **ActivatedRoute** is used to access route parameters.

---

### **3.2 Query Parameters and Preserving Query Params**

**Example: Adding Query Parameters**

```html
<a [routerLink]="['/products']" [queryParams]="{ category: 'electronics' }">Electronics</a>
```

**Example: Retrieving Query Parameters**

```typescript
this.route.queryParams.subscribe(params => {
  console.log(params['category']);
});
```

**Example: Preserving Query Params on Navigation**

```typescript
this.router.navigate(['/products'], { queryParamsHandling: 'preserve' });
```

---

### **3.3 Optional Parameters and Default Values**

**Example: Using Optional Parameters**

```typescript
const routes: Routes = [
  { path: 'search', component: SearchComponent }
];
```

**Example: Navigating with Optional Parameters**

```typescript
this.router.navigate(['/search'], { queryParams: { q: 'angular', page: 1 } });
```

**Example: Retrieving Optional Parameters with Default Values**

```typescript
this.route.queryParams.subscribe(params => {
  const query = params['q'] || 'default';
});
```

---

## **4. Route Guards and Role-Based Authorization**

### **4.1 Types of Route Guards**

1. **CanActivate**:
   - Controls navigation to a route.
2. **CanDeactivate**:
   - Controls navigation away from a route.
3. **CanActivateChild**:
   - Controls access to child routes.
4. **CanLoad**:
   - Prevents lazy-loaded modules from being loaded.

---

### **4.2 Implementing Role-Based Authorization**

**Example: AuthGuard for Role-Based Access**

```typescript
@Injectable({ providedIn: 'root' })
export class AuthGuard implements CanActivate {
  constructor(private authService: AuthService, private router: Router) {}

  canActivate(): boolean {
    if (this.authService.isAuthenticated() && this.authService.hasRole('admin')) {
      return true;
    }
    this.router.navigate(['/login']);
    return false;
  }
}
```

**Protecting Routes with AuthGuard**:

```typescript
{ path: 'admin', component: AdminComponent, canActivate: [AuthGuard] }
```

---

## **Next Lesson: Angular Best Practices and Advanced Tips**
- **Performance Optimization and Lazy Loading**
- **Security Best Practices (XSS, CSRF, JWT)**
- **Reusable Component Libraries and Shared Modules**
- **Angular Architecture Patterns and Scalable Folder Structure**
