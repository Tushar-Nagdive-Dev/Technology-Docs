### **Lesson 19: Angular Design Patterns and Architecture**

---

## **What You Will Learn:**
1. **Introduction to Design Patterns in Angular**
   - Why Use Design Patterns in Angular?
   - Benefits of Using Design Patterns
   - Overview of Common Design Patterns in Angular

2. **Component Communication Patterns**
   - Input and Output Decorators for Parent-Child Communication
   - ViewChild and ContentChild for DOM Access and Projection
   - Service-Based Communication for Cross-Component Communication
   - EventEmitter vs. RxJS Subjects for Event Handling

3. **State Management Patterns**
   - Smart and Dumb Components
   - Redux Pattern with NgRx
   - Observable Services and State Management with RxJS
   - Using Facade Pattern for State Management

4. **Facade Pattern for Complex Components**
   - What is the Facade Pattern?
   - When to Use Facade Pattern in Angular
   - Implementing Facade Pattern for Complex Components

5. **Advanced Component Design Patterns**
   - Container-Presenter Pattern (Smart and Dumb Components)
   - Dynamic Component Loading with ViewContainerRef
   - Using Angular Portals for Complex UI Components

6. **Architectural Best Practices**
   - Organizing Modules and Folder Structure
   - Dependency Injection Patterns
   - Using Injection Tokens for Scalable Architecture
   - Scalable State Management with NgRx and RxJS

7. **Hands-on Exercises:**
   - Implementing State Management with NgRx and Observable Services
   - Real-World Scenario: Building a Dashboard with Facade Pattern

8. **Expert Insights and Best Practices**
9. **Common Mistakes to Avoid**
10. **Recap and Next Steps**

---

## **1. Introduction to Design Patterns in Angular**

### **1.1 Why Use Design Patterns in Angular?**
- Design patterns **standardize solutions** to common problems.
- They **enhance code readability** and **maintainability**.
- **Improve reusability** and **testability** of components and services.
- **Promote modular architecture** for better scalability and flexibility.

---

### **1.2 Benefits of Using Design Patterns**
- **Consistent Architecture**: Easier to understand and navigate the codebase.
- **Scalable Solutions**: Efficiently scale applications by reusing patterns.
- **Easier Collaboration**: Team members can follow a standardized structure.
- **Enhanced Testability**: Patterns promote loose coupling and better unit testing.

---

### **1.3 Overview of Common Design Patterns in Angular**

1. **Component Communication Patterns**:
   - Input/Output Decorators
   - Service-Based Communication
   - Event Emitters and Subjects

2. **State Management Patterns**:
   - Smart and Dumb Components
   - Redux Pattern with NgRx
   - Observable Services with RxJS
   - Facade Pattern

3. **Advanced Component Patterns**:
   - Container-Presenter Pattern
   - Dynamic Component Loading
   - Angular Portals

---

## **2. Component Communication Patterns**

### **2.1 Input and Output Decorators for Parent-Child Communication**

1. **@Input() Decorator**:
   - Passes data from parent to child component.

**Example: Parent to Child Communication**

**Parent Component Template**:
```html
<app-child [product]="selectedProduct"></app-child>
```

**Child Component Class**:
```typescript
@Component({
  selector: 'app-child',
  template: `<p>{{ product.name }}</p>`
})
export class ChildComponent {
  @Input() product: Product;
}
```

---

2. **@Output() Decorator**:
   - Emits events from child to parent component.

**Example: Child to Parent Communication**

**Child Component Class**:
```typescript
@Output() productSelected = new EventEmitter<Product>();

onSelect(product: Product) {
  this.productSelected.emit(product);
}
```

**Parent Component Template**:
```html
<app-child (productSelected)="onProductSelected($event)"></app-child>
```

**Parent Component Class**:
```typescript
onProductSelected(product: Product) {
  console.log('Selected Product:', product);
}
```

---

### **2.2 ViewChild and ContentChild for DOM Access and Projection**

1. **@ViewChild()**:
   - Accesses child component, directive, or DOM element in the same template.

**Example: Using @ViewChild()**

**Parent Component Template**:
```html
<app-child #childComponent></app-child>
```

**Parent Component Class**:
```typescript
@ViewChild('childComponent') child: ChildComponent;

ngAfterViewInit() {
  console.log(this.child.product);
}
```

- Accesses the **public properties** and **methods** of the child component.

---

2. **@ContentChild()**:
   - Accesses projected content within `ng-content`.

**Example: Using @ContentChild()**

**Parent Component Template**:
```html
<app-child>
  <p #projectedContent>Projected Content</p>
</app-child>
```

**Child Component Class**:
```typescript
@ContentChild('projectedContent') content: ElementRef;

ngAfterContentInit() {
  console.log(this.content.nativeElement.textContent);
}
```

- **@ContentChild()** accesses the **projected content** from the parent.

---

### **2.3 Service-Based Communication for Cross-Component Communication**

- Use **shared services** for communication between unrelated components.

**Example: Using Service for Cross-Component Communication**

**Shared Service**:
```typescript
@Injectable({ providedIn: 'root' })
export class CommunicationService {
  private productSource = new Subject<Product>();
  productSelected$ = this.productSource.asObservable();

  selectProduct(product: Product) {
    this.productSource.next(product);
  }
}
```

**Component A (Sender)**:
```typescript
constructor(private communicationService: CommunicationService) {}

selectProduct(product: Product) {
  this.communicationService.selectProduct(product);
}
```

**Component B (Receiver)**:
```typescript
constructor(private communicationService: CommunicationService) {}

ngOnInit() {
  this.communicationService.productSelected$.subscribe(product => {
    console.log('Received Product:', product);
  });
}
```

- **Service-based communication** decouples components, making them reusable.

---

### **2.4 EventEmitter vs. RxJS Subjects for Event Handling**

1. **EventEmitter**:
   - **Designed for Angular components** for child-to-parent communication.
   - **Used with @Output()** decorator.

2. **RxJS Subject**:
   - **General-purpose event emitter** for cross-component communication.
   - **Preferred for service-based communication**.

**When to Use:**
- **EventEmitter**: Child-to-parent communication.
- **RxJS Subject**: Cross-component or service communication.

---

## **3. State Management Patterns**

### **3.1 Smart and Dumb Components**

1. **Smart Components**:
   - **Container Components** responsible for:
     - Fetching data from services or stores.
     - Managing state and application logic.
     - Passing data to dumb components via @Input().

2. **Dumb Components**:
   - **Presentational Components** responsible for:
     - Displaying data via @Input().
     - Emitting events via @Output().
     - No direct dependency on services or stores.

**Example: Smart and Dumb Components Pattern**

**Smart Component**:
```typescript
@Component({
  selector: 'app-product-container',
  template: `<app-product-list [products]="products"></app-product-list>`
})
export class ProductContainerComponent {
  products: Product[];

  constructor(private productService: ProductService) {}

  ngOnInit() {
    this.productService.getProducts().subscribe(data => {
      this.products = data;
    });
  }
}
```

**Dumb Component**:
```typescript
@Component({
  selector: 'app-product-list',
  template: `
    <ul>
      <li *ngFor="let product of products">{{ product.name }}</li>
    </ul>
  `
})
export class ProductListComponent {
  @Input() products: Product[];
}
```

- **Smart Component** handles **data fetching** and **state management**.
- **Dumb Component** is purely presentational with `@Input()` bindings.

---

## **Next Lesson: Testing Angular Applications**
- **Unit Testing with Jasmine and Karma**
- **End-to-End Testing with Cypress**
- **Mocking Services and HTTP Requests**
- **Test-Driven Development (TDD) with Angular**
- **Best Practices for Testing Angular Applications**
