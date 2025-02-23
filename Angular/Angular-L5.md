### **Lesson 5: Routing and Navigation in Angular**

---

## **What You Will Learn:**
1. **Understanding Routing and Navigation in Angular**
   - What is Routing?
   - Why Use Routing?
   - Configuring Routes in Angular
   - Navigation Techniques
   - Parameterized Routes
   - Child Routes
   - Lazy Loading
   - Route Guards (Authentication and Authorization)

2. **Hands-on Exercises:**
   - Setting up Routing for a Multi-Page Application
   - Real-World Scenario: Building a Product Catalog with Detail Page

3. **Expert Insights and Best Practices**
4. **Common Mistakes to Avoid**
5. **Recap and Next Steps**

---

## **1. Understanding Routing and Navigation in Angular**

### **1.1 What is Routing?**
- **Routing** in Angular enables navigation from one view to another.
- It helps build **Single Page Applications (SPA)** with multiple views.
- Routing maintains the browser history and updates the URL without refreshing the page.

---

### **1.2 Why Use Routing?**
- To **navigate between different views** (pages) in an Angular application.
- To implement **deep linking** for bookmarkable URLs.
- To pass **parameters** between components.
- To control access using **Route Guards** for authentication and authorization.

---

### **1.3 Configuring Routes in Angular**

Angular uses the **RouterModule** to configure and manage application routes.

**Step 1: Import RouterModule**

Open `src/app/app.module.ts` and update:

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { RouterModule, Routes } from '@angular/router';
import { AppComponent } from './app.component';
import { ProductListComponent } from './product-list/product-list.component';
import { ProductDetailComponent } from './product-detail/product-detail.component';

const routes: Routes = [
  { path: '', redirectTo: '/products', pathMatch: 'full' },
  { path: 'products', component: ProductListComponent },
  { path: 'product/:id', component: ProductDetailComponent }
];

@NgModule({
  declarations: [
    AppComponent,
    ProductListComponent,
    ProductDetailComponent
  ],
  imports: [
    BrowserModule,
    RouterModule.forRoot(routes)  // Registering routes
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

**Explanation:**
- `path`: URL path for the route.
- `component`: Component to load for the route.
- `redirectTo`: Redirect to another route.
- `pathMatch`: Specifies how to match the URL (`full` or `prefix`).

---

### **1.4 Navigation Techniques**

Angular provides various ways to navigate between routes:

1. **Using RouterLink Directive (Template Driven)**
   ```html
   <a [routerLink]="['/products']">Products</a>
   <a [routerLink]="['/product', product.id]">Product Details</a>
   ```

2. **Using Router.navigate() (Programmatic Navigation)**
   ```typescript
   import { Router } from '@angular/router';

   constructor(private router: Router) {}

   navigateToProduct(productId: number) {
     this.router.navigate(['/product', productId]);
   }
   ```

---

### **1.5 Parameterized Routes**

Passing parameters to routes enables dynamic routing.

**Step 1: Define Route with Parameter**
```typescript
{ path: 'product/:id', component: ProductDetailComponent }
```

**Step 2: Pass Parameter in RouterLink**
```html
<a [routerLink]="['/product', product.id]">View Details</a>
```

**Step 3: Retrieve Parameter in Component**

Open `src/app/product-detail/product-detail.component.ts` and add:

```typescript
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { ProductService } from '../product.service';

@Component({
  selector: 'app-product-detail',
  templateUrl: './product-detail.component.html',
  styleUrls: ['./product-detail.component.scss']
})
export class ProductDetailComponent implements OnInit {
  product;

  constructor(private route: ActivatedRoute, private productService: ProductService) {}

  ngOnInit(): void {
    const productId = +this.route.snapshot.paramMap.get('id');
    this.product = this.productService.getProductById(productId);
  }
}
```

---

### **1.6 Child Routes**

Child routes enable nesting of routes for complex navigation structures.

**Example:**

```typescript
const routes: Routes = [
  {
    path: 'products',
    component: ProductListComponent,
    children: [
      { path: 'details/:id', component: ProductDetailComponent }
    ]
  }
];
```

**Template Usage:**
```html
<a [routerLink]="['/products/details', product.id]">View Details</a>
<router-outlet></router-outlet>
```

---

### **1.7 Lazy Loading**

Lazy loading improves performance by loading modules only when required.

**Step 1: Create a Feature Module**

```bash
ng generate module product --route products --module app.module
```

**Step 2: Update Routing for Lazy Loading**

```typescript
const routes: Routes = [
  { path: 'products', loadChildren: () => import('./product/product.module').then(m => m.ProductModule) }
];
```

**Explanation:**
- This loads the `ProductModule` lazily when the user navigates to the `/products` route.

---

### **1.8 Route Guards (Authentication and Authorization)**

Route Guards control access to routes based on certain conditions.

**Types of Route Guards:**
1. `CanActivate`: Checks access before activating a route.
2. `CanDeactivate`: Checks access before leaving a route.
3. `CanActivateChild`: Checks access to child routes.
4. `CanLoad`: Checks access before loading lazy-loaded modules.

**Example: CanActivate Guard**

**Step 1: Generate Guard**

```bash
ng generate guard auth
```

**Step 2: Implement Logic in Guard**

Open `src/app/auth.guard.ts` and update:

```typescript
import { Injectable } from '@angular/core';
import { CanActivate, Router } from '@angular/router';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  constructor(private router: Router) {}

  canActivate(): boolean {
    const isAuthenticated = false; // Replace with actual authentication check
    if (!isAuthenticated) {
      this.router.navigate(['/login']);
      return false;
    }
    return true;
  }
}
```

**Step 3: Protect Route**

```typescript
{ path: 'product/:id', component: ProductDetailComponent, canActivate: [AuthGuard] }
```

---

## **2. Hands-on Exercise: Setting up Routing**

Let's build a **Product Catalog** with the following pages:
1. **Home Page** – Displays a welcome message.
2. **Product List Page** – Displays a list of products.
3. **Product Detail Page** – Displays details of a selected product.

**Step 1: Generate Components**
```bash
ng generate component home
ng generate component product-list
ng generate component product-detail
```

**Step 2: Define Routes**

Open `src/app/app.module.ts` and add:
```typescript
const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'products', component: ProductListComponent },
  { path: 'product/:id', component: ProductDetailComponent }
];
```

**Step 3: Create Navigation Links**

Open `src/app/app.component.html` and add:
```html
<nav>
  <a [routerLink]="['/']">Home</a>
  <a [routerLink]="['/products']">Products</a>
</nav>
<router-outlet></router-outlet>
```

**Step 4: Serve the Application**
```bash
ng serve
```

---

## **3. Expert Insights and Best Practices:**
- Use **Lazy Loading** to improve performance and reduce bundle size.
- Organize routes in **feature modules** for better maintainability.
- Avoid deeply nested child routes for simplicity.
- Use **Route Guards** for role-based access control.

---

## **4. Common Mistakes to Avoid:**
- Forgetting to import `RouterModule` or using `forRoot` incorrectly.
- Not placing `<router-outlet>` in the template.
- Misconfiguring relative and absolute paths in `routerLink`.
- Not unsubscribing from route parameters observables, leading to memory leaks.

---

## **5. Recap and Next Steps:**
You have learned:
- **Configuring Routes** and **Navigation Techniques**.
- **Parameterized Routes**, **Child Routes**, and **Lazy Loading**.
- **Route Guards** for Authentication and Authorization.

### **Next Lesson: Forms in Angular**
- **Template-Driven Forms** and **Reactive Forms**
- **Form Validation and Error Handling**
- **Dynamic Forms and FormArray**
- **Real-World Examples and Exercises**

---
