### **Lesson 18: Advanced Angular Concepts and Best Practices**

---

## **What You Will Learn:**
1. **Dependency Injection and Hierarchical Injectors**
   - Understanding Dependency Injection (DI) in Angular
   - Angular's Hierarchical Injector System
   - Multi-Providers and Injection Tokens
   - Using `providedIn` for Tree-shakable Providers

2. **Angular Module Architecture and Lazy Loading Patterns**
   - Organizing Angular Modules for Scalability
   - Core Module and Shared Module Pattern
   - Feature Modules and Lazy Loading Strategies
   - Lazy Loading with Preloading Strategies

3. **Advanced Change Detection and Zone.js**
   - How Change Detection Works in Angular
   - Default Change Detection vs OnPush Strategy
   - Optimizing Change Detection with `markForCheck` and `detach`
   - Understanding Zone.js and Its Impact on Performance

4. **Reactive Programming with RxJS**
   - Higher-Order Mapping Operators (`switchMap`, `mergeMap`, `concatMap`, `exhaustMap`)
   - Error Handling and Retry Strategies with RxJS
   - State Management with RxJS and Observable Services
   - Avoiding Memory Leaks with `takeUntil` and `unsubscribe`

5. **Performance Profiling and Optimization**
   - Profiling Angular Applications with Chrome DevTools
   - Angular Profiler and Performance Testing with Web Vitals
   - Reducing Bundle Size with Lazy Loading and Code Splitting
   - AOT Compilation and Ivy Renderer Optimization

6. **Hands-on Exercises:**
   - Implementing Advanced Dependency Injection Patterns
   - Real-World Scenario: State Management with RxJS and Observable Store

7. **Expert Insights and Best Practices**
8. **Common Mistakes to Avoid**
9. **Recap and Next Steps**

---

## **1. Dependency Injection and Hierarchical Injectors**

### **1.1 Understanding Dependency Injection (DI) in Angular**
- **Dependency Injection (DI)** is a design pattern in which a class receives its dependencies from an external source.
- In Angular, DI is used to provide:
  - **Services** to Components, Directives, and Pipes.
  - **Singleton Instances** for application-wide state management.
- Angular's DI system uses:
  - **Constructor Injection**: Dependencies are injected via the constructor.
  - **Provider Configuration**: Configures how services are created and shared.

---

### **1.2 Angular's Hierarchical Injector System**
- Angular uses a **Hierarchical Injector System**:
  - **Root Injector**: Singleton instance across the entire application.
  - **Module Injector**: Shared within a module and its children.
  - **Component Injector**: Specific to a component and its children.

**Example: Component-Level Injector**

```typescript
@Component({
  selector: 'app-product',
  templateUrl: './product.component.html',
  providers: [ProductService]
})
export class ProductComponent {
  constructor(private productService: ProductService) {}
}
```

- `ProductService` is **re-created** for each instance of `ProductComponent`.

---

### **1.3 Multi-Providers and Injection Tokens**

1. **Multi-Providers**:
   - Use multi-providers to provide multiple values for a single injection token.

```typescript
const MULTI_PROVIDERS = new InjectionToken<string[]>('MULTI_PROVIDERS');

@NgModule({
  providers: [
    { provide: MULTI_PROVIDERS, useValue: 'Value 1', multi: true },
    { provide: MULTI_PROVIDERS, useValue: 'Value 2', multi: true }
  ]
})
export class AppModule {}
```

**Injecting Multi-Providers**:
```typescript
constructor(@Inject(MULTI_PROVIDERS) public values: string[]) {
  console.log(values); // Output: ['Value 1', 'Value 2']
}
```

---

2. **Injection Tokens**:
   - Use `InjectionToken` for providing non-class dependencies.

```typescript
export const API_URL = new InjectionToken<string>('API_URL');

@NgModule({
  providers: [
    { provide: API_URL, useValue: 'https://api.example.com' }
  ]
})
export class AppModule {}
```

**Injecting Injection Token**:
```typescript
constructor(@Inject(API_URL) private apiUrl: string) {
  console.log(this.apiUrl); // Output: 'https://api.example.com'
}
```

---

### **1.4 Using `providedIn` for Tree-shakable Providers**

- Use `providedIn` for **Tree-shakable Providers**.
- Services provided with `providedIn: 'root'` are automatically tree-shaken if not used.

```typescript
@Injectable({
  providedIn: 'root'
})
export class ProductService {
  constructor(private http: HttpClient) {}
}
```

- **`providedIn: 'root'`** makes `ProductService` a singleton across the entire application.
- **`providedIn: 'any'`** creates a new instance for each lazy-loaded module.
- **`providedIn: 'platform'`** makes the service available across multiple Angular applications.

---

## **2. Angular Module Architecture and Lazy Loading Patterns**

### **2.1 Organizing Angular Modules for Scalability**

1. **Core Module**:
   - Singleton services and application-wide providers.
   - Imported only once in the `AppModule`.

**Example: Core Module Configuration**

```typescript
@NgModule({
  providers: [AuthService, LoggerService],
  exports: [NavbarComponent, FooterComponent]
})
export class CoreModule {}
```

---

2. **Shared Module**:
   - Reusable components, directives, and pipes.
   - **NO services** to avoid multiple instances.

**Example: Shared Module Configuration**

```typescript
@NgModule({
  declarations: [CustomButtonComponent, CustomPipe],
  exports: [CustomButtonComponent, CustomPipe]
})
export class SharedModule {}
```

- Imported in feature modules to reuse common components and pipes.

---

### **2.2 Feature Modules and Lazy Loading Strategies**

1. **Feature Modules**:
   - Organized by feature or domain (e.g., ProductModule, UserModule).
   - Contains **components, services, and routing** related to a specific feature.

2. **Lazy Loading with Dynamic Imports**:
   - Load feature modules **on demand** for better performance.

```typescript
const routes: Routes = [
  { path: 'products', loadChildren: () => import('./products/products.module').then(m => m.ProductsModule) }
];
```

- The `ProductsModule` is loaded **only when** the user navigates to `/products`.

---

### **2.3 Lazy Loading with Preloading Strategies**

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

- This conditionally preloads the `ProductsModule` based on the `preload` property.

---

## **3. Advanced Change Detection and Zone.js**

### **3.1 How Change Detection Works in Angular**
- Angular uses **Zone.js** to track asynchronous operations and trigger change detection.
- Change detection is **triggered** by:
  - User events (e.g., click, input)
  - Asynchronous operations (e.g., HTTP requests)
  - Timers (e.g., setTimeout, setInterval)

### **3.2 Default Change Detection vs OnPush Strategy**
- **Default Change Detection**: Checks the entire component tree.
- **OnPush Change Detection**: Checks only when:
  - `@Input()` properties change.
  - An event is triggered in the component.
  - An Observable emits a new value.

**Example: OnPush Strategy**

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

## **Next Lesson: Angular Design Patterns and Architecture**
- **Component Communication Patterns**
- **State Management Patterns (Redux, RxJS Store)**
- **Facade Pattern for Complex Components**
- **Smart and Dumb Components**
- **Best Practices for Scalable Angular Architecture**
