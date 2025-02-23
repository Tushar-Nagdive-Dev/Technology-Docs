### **Lesson 7: HTTP Client and RESTful Services in Angular**

---

## **What You Will Learn:**
1. **Understanding HTTP Client in Angular**
   - What is Angular HTTP Client?
   - Why Use HTTP Client?
   - Setting up HttpClientModule
   - Making HTTP Requests
     - GET, POST, PUT, DELETE
     - Handling Observables and Promises

2. **Working with RESTful Services**
   - Consuming RESTful APIs in Angular
   - CRUD Operations (Create, Read, Update, Delete)
   - Error Handling with HTTP Client

3. **Advanced HTTP Client Features**
   - Using Interceptors for Request and Response Handling
   - HTTP Headers, Params, and Authentication
   - Caching and Retry Mechanisms

4. **Hands-on Exercises:**
   - Integrating a Public REST API
   - Building a Product Management Module with CRUD Operations

5. **Expert Insights and Best Practices**
6. **Common Mistakes to Avoid**
7. **Recap and Next Steps**

---

## **1. Understanding HTTP Client in Angular**

### **1.1 What is Angular HTTP Client?**
- **HttpClient** is a built-in Angular module that facilitates communication with external RESTful APIs.
- It is used to make **HTTP requests** (GET, POST, PUT, DELETE) and handle responses using **Observables**.
- It is part of the **@angular/common/http** package.

---

### **1.2 Why Use HTTP Client?**
- **Asynchronous Communication**: Handles asynchronous requests using RxJS Observables.
- **Type Safety**: Supports TypeScript interfaces for type-safe API responses.
- **Interceptors**: Middleware for modifying requests or responses (e.g., adding headers, error handling).
- **Built-in Support for Error Handling**: Easily manage HTTP errors.

---

### **1.3 Setting up HttpClientModule**

**Step 1: Import HttpClientModule**

Open `src/app/app.module.ts` and add:

```typescript
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  imports: [HttpClientModule]
})
export class AppModule {}
```

- This enables Angular to make HTTP requests using the `HttpClient` service.

---

## **2. Making HTTP Requests**

### **2.1 GET Request**
- **GET** is used to retrieve data from the server.
- Returns an **Observable** of the response body as a JSON object.

**Example:**
```typescript
import { HttpClient } from '@angular/common/http';

constructor(private http: HttpClient) {}

getProducts() {
  return this.http.get('https://api.example.com/products');
}
```

**With Type Safety:**
```typescript
interface Product {
  id: number;
  name: string;
  price: number;
}

getProducts() {
  return this.http.get<Product[]>('https://api.example.com/products');
}
```

**Consuming the Observable:**
```typescript
this.getProducts().subscribe(products => {
  console.log(products);
});
```

---

### **2.2 POST Request**
- **POST** is used to send data to the server.
- It is commonly used for creating new resources.

**Example:**
```typescript
addProduct(product: Product) {
  return this.http.post('https://api.example.com/products', product);
}
```

**Consuming the Observable:**
```typescript
const newProduct = { name: 'New Product', price: 99.99 };
this.addProduct(newProduct).subscribe(response => {
  console.log('Product Added:', response);
});
```

---

### **2.3 PUT Request**
- **PUT** is used to update an existing resource on the server.

**Example:**
```typescript
updateProduct(product: Product) {
  return this.http.put(`https://api.example.com/products/${product.id}`, product);
}
```

**Consuming the Observable:**
```typescript
const updatedProduct = { id: 1, name: 'Updated Product', price: 79.99 };
this.updateProduct(updatedProduct).subscribe(response => {
  console.log('Product Updated:', response);
});
```

---

### **2.4 DELETE Request**
- **DELETE** is used to delete a resource from the server.

**Example:**
```typescript
deleteProduct(id: number) {
  return this.http.delete(`https://api.example.com/products/${id}`);
}
```

**Consuming the Observable:**
```typescript
this.deleteProduct(1).subscribe(response => {
  console.log('Product Deleted:', response);
});
```

---

### **2.5 Handling Observables and Promises**

- **HttpClient** methods return **Observables**, which are lazy and won't execute until subscribed.
- Use `.subscribe()` to consume the Observable.
- Use RxJS operators like `.map()`, `.filter()`, and `.catchError()` for data transformation and error handling.
- Convert Observables to Promises using `.toPromise()` if needed:
  ```typescript
  async getProductsAsync() {
    const products = await this.getProducts().toPromise();
    console.log(products);
  }
  ```

---

## **3. Working with RESTful Services**

Let's create a **ProductService** to perform CRUD operations with a public REST API.

### **Step 1: Generate Product Service**
```bash
ng generate service product
```

### **Step 2: Define Product Model**

Create `src/app/models/product.model.ts`:
```typescript
export interface Product {
  id?: number;
  name: string;
  price: number;
}
```

---

### **Step 3: Implement ProductService**

Open `src/app/product.service.ts` and add:

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { Product } from './models/product.model';

@Injectable({
  providedIn: 'root'
})
export class ProductService {
  private apiUrl = 'https://fakestoreapi.com/products';

  constructor(private http: HttpClient) {}

  getProducts(): Observable<Product[]> {
    return this.http.get<Product[]>(this.apiUrl);
  }

  getProductById(id: number): Observable<Product> {
    return this.http.get<Product>(`${this.apiUrl}/${id}`);
  }

  addProduct(product: Product): Observable<Product> {
    return this.http.post<Product>(this.apiUrl, product);
  }

  updateProduct(product: Product): Observable<Product> {
    return this.http.put<Product>(`${this.apiUrl}/${product.id}`, product);
  }

  deleteProduct(id: number): Observable<void> {
    return this.http.delete<void>(`${this.apiUrl}/${id}`);
  }
}
```

---

### **Step 4: Consuming ProductService in Component**

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
    this.productService.getProducts().subscribe(products => {
      this.products = products;
    });
  }
}
```

**Template:**
```html
<ul>
  <li *ngFor="let product of products">
    <h3>{{ product.name }}</h3>
    <p>Price: {{ product.price | currency }}</p>
  </li>
</ul>
```

---

## **4. Expert Insights and Best Practices:**
- Use **environment.ts** for storing API URLs.
- Handle errors using `.catchError()` and `HttpErrorResponse`.
- Use **Interceptors** for common logic like authentication headers and error handling.
- Prefer **Observables** over Promises for better control over asynchronous operations.

---

## **5. Common Mistakes to Avoid:**
- Not importing `HttpClientModule`.
- Not subscribing to Observables, causing requests not to be sent.
- Using `.subscribe()` multiple times, leading to duplicate requests.
- Exposing sensitive information like API keys directly in the code.

---

## **Next Lesson: Observables and RxJS Essentials**
- **Understanding Observables and Operators**
- **Subject, BehaviorSubject, ReplaySubject**
- **RxJS Operators (map, filter, switchMap, mergeMap, concatMap)**
- **Error Handling with RxJS**
- **Real-World Examples and Exercises**
