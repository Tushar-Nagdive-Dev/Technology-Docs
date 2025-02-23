### **Lesson 23: Angular Best Practices and Advanced Tips**

---

## **What You Will Learn:**
1. **Introduction to Angular Best Practices**
   - Why Follow Best Practices in Angular?
   - Benefits of Using Best Practices
   - Overview of Advanced Tips for Angular Development

2. **Performance Optimization and Lazy Loading**
   - Optimizing Angular Performance
   - Lazy Loading Modules and Components
   - OnPush Change Detection Strategy
   - Using trackBy with *ngFor
   - Caching, Debouncing, and Throttling

3. **Security Best Practices in Angular**
   - Cross-Site Scripting (XSS) Prevention
   - Cross-Site Request Forgery (CSRF) Protection
   - Using Angular’s DomSanitizer Safely
   - JWT Authentication and Authorization
   - Secure Storage of Tokens

4. **Reusable Component Libraries and Shared Modules**
   - Creating Reusable Angular Libraries
   - Using Shared Modules for Common Components
   - Custom Angular UI Component Libraries
   - Publishing Angular Libraries to npm

5. **Angular Architecture Patterns and Scalable Folder Structure**
   - Organizing Angular Modules for Scalability
   - Feature Module and Core Module Pattern
   - Smart and Dumb Components Pattern
   - Facade Pattern for State Management
   - Scalable Folder Structure for Large Applications

6. **Hands-on Exercises:**
   - Implementing OnPush Change Detection and trackBy
   - Creating a Reusable Angular Library
   - Real-World Scenario: Securing an Admin Dashboard with Role-Based Authorization

7. **Expert Insights and Best Practices**
8. **Common Mistakes to Avoid**
9. **Recap and Next Steps**

---

## **1. Introduction to Angular Best Practices**

### **1.1 Why Follow Best Practices in Angular?**
- Ensures **maintainability** and **scalability** of Angular applications.
- Improves **performance** and **security**.
- Enhances **code readability** and **consistency**.
- Facilitates **team collaboration** by following standardized patterns.

---

### **1.2 Benefits of Using Best Practices**
- **Performance Optimization**: Faster loading and improved responsiveness.
- **Security**: Protects against common vulnerabilities like XSS and CSRF.
- **Reusability and Modularization**: Efficient reuse of components and modules.
- **Maintainability**: Easier to maintain and extend the codebase.
- **Scalability**: Smooth scaling of applications as they grow.

---

### **1.3 Overview of Advanced Tips for Angular Development**
- Performance optimization with **Lazy Loading** and **Change Detection**.
- Security best practices for **XSS** and **JWT Authentication**.
- Creating **Reusable Libraries** and **Component Libraries**.
- Scalable **Angular Architecture Patterns** and **Folder Structures**.
- **Advanced Routing Techniques** and **State Management Patterns**.

---

## **2. Performance Optimization and Lazy Loading**

### **2.1 Optimizing Angular Performance**

1. **Ahead-of-Time (AOT) Compilation**:
   - Compiles Angular templates **before** running in the browser.
   - Improves performance and reduces bundle size.
   - Default in production builds:
     ```bash
     ng build --prod
     ```

2. **Ivy Renderer**:
   - Smaller bundle size and faster rendering.
   - Enables **Tree-Shaking** to remove unused code.

---

### **2.2 Lazy Loading Modules and Components**

1. **Lazy Loading Feature Modules**:
   - Load modules **on demand** to reduce the initial bundle size.

**Example: Lazy Loading a Module**

```typescript
const routes: Routes = [
  { path: 'products', loadChildren: () => import('./products/products.module').then(m => m.ProductsModule) }
];
```

- The `ProductsModule` is loaded **only when** the user navigates to `/products`.

---

2. **Lazy Loading Components**:
   - Load components dynamically using **Angular’s Dynamic Component Loader**.

**Example: Lazy Loading a Dialog Component**

```typescript
async openDialog() {
  const { DialogComponent } = await import('./dialog/dialog.component');
  this.dialog.open(DialogComponent);
}
```

- The `DialogComponent` is imported **only when** `openDialog()` is called.

---

### **2.3 OnPush Change Detection Strategy**

- **OnPush Change Detection** checks only when:
  - `@Input()` properties change.
  - An event is triggered in the component.
  - An Observable emits a new value.

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

- **OnPush** improves performance by reducing change detection cycles.

---

### **2.4 Using trackBy with *ngFor**

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

### **2.5 Caching, Debouncing, and Throttling**

1. **Caching HTTP Requests**:
   - Cache HTTP requests to avoid redundant API calls.
   - Use **RxJS shareReplay** to cache responses.

**Example: Caching HTTP Request with shareReplay**

```typescript
getProducts(): Observable<Product[]> {
  return this.http.get<Product[]>(this.apiUrl).pipe(
    shareReplay(1)
  );
}
```

2. **Debouncing and Throttling**:
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

## **3. Security Best Practices in Angular**

### **3.1 Cross-Site Scripting (XSS) Prevention**

- Angular uses **Contextual Escaping** by default.
- Avoid using **[innerHTML]** directly.
- Use **DomSanitizer** for dynamic HTML content.

**Example: Using DomSanitizer**

```typescript
import { DomSanitizer, SafeHtml } from '@angular/platform-browser';

constructor(private sanitizer: DomSanitizer) {}

getSafeHtml(html: string): SafeHtml {
  return this.sanitizer.bypassSecurityTrustHtml(html);
}
```

---

### **3.2 JWT Authentication and Authorization**

1. **Store JWT Securely**:
   - **DO NOT** store JWT in Local Storage (vulnerable to XSS).
   - Store in **HttpOnly Cookies** or **Memory Storage**.

2. **Send JWT in HTTP Headers**

```typescript
const headers = new HttpHeaders().set('Authorization', `Bearer ${token}`);
this.http.get('https://api.example.com/protected', { headers });
```

---

### **3.3 Secure Storage of Tokens**

- Store tokens in **Memory Storage** or **HttpOnly Cookies**.
- **DO NOT** store tokens in Local Storage or Session Storage.

---

## **4. Reusable Component Libraries and Shared Modules**

### **4.1 Creating Reusable Angular Libraries**

- Use Angular CLI to create a library:

```bash
ng generate library shared-components
```

- The library is created in the `projects` folder.

### **4.2 Publishing Angular Libraries to npm**

- Build the library:

```bash
ng build shared-components
```

- Publish to npm:

```bash
cd dist/shared-components
npm publish
```

- Use the published library in other projects.

---

## **5. Angular Architecture Patterns and Scalable Folder Structure**

### **5.1 Feature Module and Core Module Pattern**

1. **Feature Modules**:
   - Organized by feature or domain (e.g., ProductModule, UserModule).
   - Contains **components, services, and routing** related to a specific feature.

2. **Core Module**:
   - Singleton services and application-wide providers.
   - Imported only once in the `AppModule`.

---

## **Next Steps: Mastering Angular**
- Advanced RxJS Patterns and Operators
- Angular State Management with NgRx
- Building Angular Micro Frontends
- Angular + GraphQL Integration
