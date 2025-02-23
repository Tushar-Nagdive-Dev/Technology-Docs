### **Lesson 20: Testing Angular Applications**

---

## **What You Will Learn:**
1. **Introduction to Testing in Angular**
   - Why Testing is Important in Angular Applications
   - Types of Testing:
     - Unit Testing
     - Integration Testing
     - End-to-End (E2E) Testing
   - Testing Frameworks: Jasmine, Karma, and Cypress

2. **Unit Testing with Jasmine and Karma**
   - Setting up Jasmine and Karma in Angular
   - Writing Unit Test Cases for:
     - Components
     - Services
     - Directives
     - Pipes
   - Mocking Dependencies with TestBed
   - Testing Asynchronous Code

3. **End-to-End Testing with Cypress**
   - Why Use Cypress for E2E Testing?
   - Setting up Cypress in Angular
   - Writing E2E Tests for User Journeys
   - Testing Forms, Navigation, and API Requests

4. **Advanced Testing Techniques**
   - Mocking HTTP Requests with HttpClientTestingModule
   - Using Spies and Mocks for Dependency Injection
   - Code Coverage and Best Practices

5. **Hands-on Exercises:**
   - Writing Unit Tests for ProductService
   - Real-World Scenario: E2E Testing for Shopping Cart Flow

6. **Expert Insights and Best Practices**
7. **Common Mistakes to Avoid**
8. **Recap and Next Steps**

---

## **1. Introduction to Testing in Angular**

### **1.1 Why Testing is Important in Angular Applications**
- Ensures **code reliability** and **maintainability**.
- Detects bugs and issues early in the development cycle.
- Facilitates **refactoring** without breaking existing functionality.
- Enhances **code quality** and **developer confidence**.
- Promotes **continuous integration** and **delivery**.

---

### **1.2 Types of Testing**

1. **Unit Testing**:
   - Tests individual components, services, pipes, and directives in isolation.
   - Fast execution with no external dependencies.
   - Frameworks: **Jasmine** and **Karma**.

2. **Integration Testing**:
   - Tests the interaction between multiple components or modules.
   - Ensures that different parts of the application work together seamlessly.

3. **End-to-End (E2E) Testing**:
   - Tests the entire application workflow from start to finish.
   - Simulates real user scenarios and interactions.
   - Frameworks: **Cypress** and **Protractor**.

---

### **1.3 Testing Frameworks**

1. **Jasmine**:
   - Behavior-driven development (BDD) framework for writing unit tests.
   - Provides `describe`, `it`, `beforeEach`, `afterEach`, `expect`, and `spyOn` methods.

2. **Karma**:
   - Test runner for executing unit tests in real browsers.
   - Integrates with Jasmine to provide test reports and code coverage.

3. **Cypress**:
   - Fast, reliable, and easy-to-use E2E testing framework.
   - Provides real-time reloading and interactive debugging.

---

## **2. Unit Testing with Jasmine and Karma**

### **2.1 Setting up Jasmine and Karma in Angular**

Angular CLI comes with **Jasmine** and **Karma** pre-configured.

**Configuration Files:**
- `karma.conf.js`: Karma configuration file.
- `tsconfig.spec.json`: TypeScript configuration for unit tests.

**Running Unit Tests:**
```bash
ng test
```

This command:
- Compiles the test files.
- Launches **Karma** in a browser.
- Executes all test cases and generates a report.

---

### **2.2 Writing Unit Test Cases for Components**

**Example: Unit Testing ProductListComponent**

**Step 1: Create ProductListComponent**

```bash
ng generate component product-list
```

**Step 2: Add Component Logic**

Open `src/app/product-list/product-list.component.ts` and add:

```typescript
import { Component, OnInit } from '@angular/core';
import { ProductService } from '../product.service';
import { Product } from '../models/product.model';

@Component({
  selector: 'app-product-list',
  templateUrl: './product-list.component.html',
  styleUrls: ['./product-list.component.scss']
})
export class ProductListComponent implements OnInit {
  products: Product[] = [];

  constructor(private productService: ProductService) {}

  ngOnInit(): void {
    this.getProducts();
  }

  getProducts(): void {
    this.productService.getProducts().subscribe(products => {
      this.products = products;
    });
  }
}
```

---

**Step 3: Write Unit Test for ProductListComponent**

Open `src/app/product-list/product-list.component.spec.ts` and add:

```typescript
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { ProductListComponent } from './product-list.component';
import { ProductService } from '../product.service';
import { of } from 'rxjs';

describe('ProductListComponent', () => {
  let component: ProductListComponent;
  let fixture: ComponentFixture<ProductListComponent>;
  let mockProductService;

  beforeEach(async () => {
    mockProductService = jasmine.createSpyObj(['getProducts']);

    await TestBed.configureTestingModule({
      declarations: [ ProductListComponent ],
      providers: [
        { provide: ProductService, useValue: mockProductService }
      ]
    }).compileComponents();
  });

  beforeEach(() => {
    fixture = TestBed.createComponent(ProductListComponent);
    component = fixture.componentInstance;
  });

  it('should create the component', () => {
    expect(component).toBeTruthy();
  });

  it('should fetch products on initialization', () => {
    const products = [
      { id: 1, name: 'Product 1', price: 10 },
      { id: 2, name: 'Product 2', price: 20 }
    ];

    mockProductService.getProducts.and.returnValue(of(products));
    fixture.detectChanges();

    expect(component.products.length).toBe(2);
    expect(component.products).toEqual(products);
  });
});
```

**Explanation:**
- **TestBed** is used to create a test module.
- **jasmine.createSpyObj** is used to create a mock service.
- **of()** from RxJS is used to create an Observable.
- **fixture.detectChanges()** triggers Angular's change detection.

---

### **2.3 Writing Unit Test for Services**

**Example: Unit Testing ProductService**

Open `src/app/product.service.spec.ts` and add:

```typescript
import { TestBed } from '@angular/core/testing';
import { HttpClientTestingModule, HttpTestingController } from '@angular/common/http/testing';
import { ProductService } from './product.service';
import { Product } from './models/product.model';

describe('ProductService', () => {
  let service: ProductService;
  let httpMock: HttpTestingController;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule],
      providers: [ProductService]
    });

    service = TestBed.inject(ProductService);
    httpMock = TestBed.inject(HttpTestingController);
  });

  afterEach(() => {
    httpMock.verify();
  });

  it('should fetch products via GET', () => {
    const products: Product[] = [
      { id: 1, name: 'Product 1', price: 10 },
      { id: 2, name: 'Product 2', price: 20 }
    ];

    service.getProducts().subscribe(data => {
      expect(data.length).toBe(2);
      expect(data).toEqual(products);
    });

    const req = httpMock.expectOne('https://fakestoreapi.com/products');
    expect(req.request.method).toBe('GET');
    req.flush(products);
  });
});
```

**Explanation:**
- **HttpClientTestingModule** is used to mock HTTP requests.
- **HttpTestingController** verifies that no unexpected requests are made.
- **req.flush(products)** simulates an HTTP response.

---

## **Next Lesson: Angular Deployment and CI/CD**
- **Deploying Angular on Firebase, Netlify, and Vercel**
- **CI/CD Pipelines with GitHub Actions and GitLab CI**
- **Dockerizing Angular Applications**
- **Best Practices for Angular Deployment**
