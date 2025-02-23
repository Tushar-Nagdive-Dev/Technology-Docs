### **Lesson 4: Services and Dependency Injection in Angular**

---

## **What You Will Learn:**
1. **Understanding Services in Angular**
   - What are Services?
   - Why Use Services?
   - Creating and Using Services
   - Sharing Data Between Components

2. **Dependency Injection (DI) in Angular**
   - What is Dependency Injection?
   - How Angular's DI System Works
   - Injecting Services into Components

3. **Hands-on Exercises:**
   - Creating a `ProductService` for Data Management
   - Real-World Scenario: Sharing Data Between Components

4. **Expert Insights and Best Practices**
5. **Common Mistakes to Avoid**
6. **Recap and Next Steps**

---

## **1. Understanding Services in Angular**

### **1.1 What are Services?**
- **Services** are TypeScript classes with a specific purpose to provide data, logic, or utility functions.
- They are responsible for **business logic**, **data management**, and **communication with external APIs**.
- Services in Angular are typically **singleton**, meaning they are created once and shared across components.

---

### **1.2 Why Use Services?**
- **Separation of Concerns**: Business logic is separated from UI logic.
- **Reusability**: Services can be reused across multiple components.
- **Maintainability**: Centralized logic makes it easier to maintain and update.
- **Testability**: Services are easier to test in isolation.

---

### **1.3 Creating and Using Services**

Services are typically created using the Angular CLI:
```bash
ng generate service product
```

This creates two files:
- `product.service.ts` – The service implementation.
- `product.service.spec.ts` – The test file.

---

### **1.4 Example: ProductService**

Let's create a `ProductService` to manage product data for a shopping application.

**Step 1: Generate Service**
```bash
ng generate service product
```

**Step 2: Define Product Service**

Open `src/app/product.service.ts` and update:

```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'  // Makes this service a singleton
})
export class ProductService {
  products = [
    { id: 1, name: 'Angular Book', price: 29.99 },
    { id: 2, name: 'TypeScript Guide', price: 19.99 },
    { id: 3, name: 'RxJS Manual', price: 24.99 }
  ];

  getProducts() {
    return this.products;
  }

  getProductById(id: number) {
    return this.products.find(product => product.id === id);
  }
}
```

---

### **1.5 Using ProductService in a Component**

Let's use `ProductService` in the `ProductListComponent`.

**Step 1: Inject Service into Component**

Open `src/app/product-list/product-list.component.ts` and update:

```typescript
import { Component, OnInit } from '@angular/core';
import { ProductService } from '../product.service';

@Component({
  selector: 'app-product-list',
  templateUrl: './product-list.component.html',
  styleUrls: ['./product-list.component.scss']
})
export class ProductListComponent implements OnInit {
  products = [];

  constructor(private productService: ProductService) {}

  ngOnInit(): void {
    this.products = this.productService.getProducts();
  }
}
```

**Step 2: Display Products in Template**

Open `src/app/product-list/product-list.component.html` and add:

```html
<ul>
  <li *ngFor="let product of products">
    <h3>{{ product.name }}</h3>
    <p>Price: {{ product.price | currency:'USD' }}</p>
  </li>
</ul>
```

**Step 3: Serve the Application**

```bash
ng serve
```

Open the browser at `http://localhost:4200` to see the **Product List** in action.

---

## **2. Dependency Injection (DI) in Angular**

### **2.1 What is Dependency Injection?**
- **Dependency Injection (DI)** is a design pattern where a class receives its dependencies from an external source rather than creating them itself.
- Angular has a powerful **DI system** that provides dependencies to components and services.

---

### **2.2 How Angular's DI System Works**
- Angular creates a **Dependency Injection Tree** that manages the lifecycle of services.
- **@Injectable** decorator is used to define a service as a dependency.
- Dependencies are registered in the **providers** array or using **providedIn** syntax.

---

### **2.3 Injecting Services into Components**
- **Injecting a Service** is done through the **constructor** of a component.
- Angular uses **TypeScript's Type Annotations** to resolve dependencies.

**Example**:
```typescript
constructor(private productService: ProductService) {}
```

---

### **2.4 Real-World Scenario: Sharing Data Between Components**

We will create two components:
- `ProductListComponent`: Displays a list of products.
- `ProductDetailComponent`: Displays the details of a selected product.

### **Step 1: Generate Components**

```bash
ng generate component product-detail
```

### **Step 2: Modify ProductService**

Open `src/app/product.service.ts` and add a method to get product by ID:
```typescript
getProductById(id: number) {
  return this.products.find(product => product.id === id);
}
```

### **Step 3: Display Product Details**

**ProductListComponent:**
```html
<ul>
  <li *ngFor="let product of products" (click)="selectProduct(product.id)">
    <h3>{{ product.name }}</h3>
    <p>Price: {{ product.price | currency:'USD' }}</p>
  </li>
</ul>
```

**Component Class:**
```typescript
selectProduct(id: number) {
  this.selectedProduct = this.productService.getProductById(id);
}
```

---

**ProductDetailComponent:**
```html
<div *ngIf="product">
  <h2>{{ product.name }} Details</h2>
  <p>Price: {{ product.price | currency:'USD' }}</p>
  <button (click)="goBack()">Back to List</button>
</div>
```

**Component Class:**
```typescript
import { Component, Input } from '@angular/core';
import { ProductService } from '../product.service';

@Component({
  selector: 'app-product-detail',
  templateUrl: './product-detail.component.html',
  styleUrls: ['./product-detail.component.scss']
})
export class ProductDetailComponent {
  @Input() product;
}
```

---

### **Step 4: Sharing Data Using Service**

To **share data** between the two components:
1. Store the selected product in the `ProductService`.
2. Retrieve the selected product in `ProductDetailComponent`.

**ProductService:**
```typescript
selectedProduct;

setSelectedProduct(product) {
  this.selectedProduct = product;
}

getSelectedProduct() {
  return this.selectedProduct;
}
```

**ProductListComponent:**
```typescript
selectProduct(id: number) {
  const product = this.productService.getProductById(id);
  this.productService.setSelectedProduct(product);
}
```

**ProductDetailComponent:**
```typescript
ngOnInit(): void {
  this.product = this.productService.getSelectedProduct();
}
```

---

## **3. Expert Insights and Best Practices:**
- **Singleton Pattern**: Services are singleton by default when provided in root.
- **Shared State Management**: Use services to manage shared state across components.
- **Avoid Circular Dependencies**: Inject dependencies wisely to avoid circular references.
- **Lazy Loading**: Register services in a lazy-loaded module if they're used only in that module.

---

## **4. Common Mistakes to Avoid:**
- Not using **providedIn: 'root'** which results in multiple instances of a service.
- Injecting services in the wrong module, leading to unexpected behavior.
- Directly modifying service state without using a method.
- Not unsubscribing from Observables in services, causing memory leaks.

---

## **5. Recap and Next Steps:**
You have learned:
- **What are Services and Why Use Them?**
- **Dependency Injection** in Angular.
- **Sharing Data Between Components** using Services.
- **Real-World Scenario** of a Shopping Cart with Product List and Detail.

### **Next Lesson: Routing and Navigation**
- **Configuring Angular Routes**
- **Navigation and Parameterized Routes**
- **Child Routes and Lazy Loading**
- **Route Guards for Authentication and Authorization**

---
