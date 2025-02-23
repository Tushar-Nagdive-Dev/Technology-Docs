### **Lesson 28: Angular + NestJS Full-Stack Development**

---

## **What You Will Learn:**
1. **Introduction to Angular and NestJS Integration**
   - Why Use Angular with NestJS?
   - Full-Stack Architecture Overview
   - REST vs GraphQL with Angular and NestJS
   - Core Concepts: Modular Architecture, Dependency Injection

2. **Setting up Angular and NestJS**
   - Creating an Angular Frontend Project
   - Creating a NestJS Backend Project
   - Configuring CORS and Environment Variables
   - Connecting Angular with NestJS API

3. **Building RESTful APIs with NestJS**
   - Creating Controllers, Services, and Modules
   - Using Data Transfer Objects (DTOs) and Validation
   - Implementing CRUD Operations
   - Integrating MongoDB with Mongoose

4. **GraphQL API Development with NestJS**
   - Setting up GraphQL with Apollo Server
   - Defining GraphQL Schemas and Resolvers
   - Query, Mutation, and Subscription with GraphQL
   - Using GraphQL Code Generator with Angular

5. **Authentication and Authorization with JWT**
   - Implementing JWT Authentication with Passport
   - Role-Based Authorization in NestJS
   - Securing Angular Routes with AuthGuard
   - Refresh Tokens and Token Expiration Handling

6. **Microservices Architecture with Angular and NestJS**
   - Introduction to Microservices with NestJS
   - Using Redis and RabbitMQ for Messaging
   - Communication Between Microservices
   - API Gateway and Load Balancing with NestJS

7. **Deploying Angular and NestJS Full-Stack Application**
   - Dockerizing Angular and NestJS Applications
   - CI/CD Pipelines with GitHub Actions and GitLab CI
   - Deploying on AWS, GCP, and Azure
   - Versioning and Scaling Full-Stack Applications

8. **Hands-on Exercises:**
   - Building a Full-Stack E-commerce Application
   - Real-World Scenario: User Authentication and Authorization with JWT

9. **Expert Insights and Best Practices**
10. **Common Mistakes to Avoid**
11. **Recap and Next Steps**

---

## **1. Introduction to Angular and NestJS Integration**

### **1.1 Why Use Angular with NestJS?**
- **Angular** is a **front-end framework** for building dynamic, single-page web applications.
- **NestJS** is a **back-end framework** built on **Node.js** and **Express**, using **TypeScript**.
- **Full-Stack Integration**:
  - **Angular** handles the client-side rendering and user interface.
  - **NestJS** manages server-side logic, API endpoints, and data processing.
- **Modular Architecture**:
  - Both Angular and NestJS follow a modular architecture for **scalable applications**.

---

### **1.2 Full-Stack Architecture Overview**
- **Angular Frontend**:
  - **Service Layer**: Manages HTTP requests.
  - **Component Layer**: Manages UI rendering and user interactions.
  - **State Management**: Uses **NgRx** or **RxJS** for global state.

- **NestJS Backend**:
  - **Controller Layer**: Handles HTTP requests and routes.
  - **Service Layer**: Contains business logic.
  - **Repository Layer**: Manages data access (MongoDB, PostgreSQL, etc.).
  - **Middleware and Guards**: Handles authentication and authorization.

---

### **1.3 REST vs GraphQL with Angular and NestJS**
- **REST**:
  - **URL-based routing**.
  - Multiple endpoints for different data needs.
  - Suitable for **simple CRUD operations**.

- **GraphQL**:
  - **Single endpoint** for all queries.
  - Client specifies the data shape.
  - Suitable for **complex data queries** and **real-time updates**.

---

### **1.4 Core Concepts**
1. **Modular Architecture**:
   - Angular and NestJS organize features as **modules**.
   - Modules provide **scalable and maintainable** architecture.

2. **Dependency Injection**:
   - Both frameworks use **Dependency Injection** for managing services and dependencies.

---

## **2. Setting up Angular and NestJS**

### **2.1 Creating an Angular Frontend Project**

```bash
ng new frontend --routing --style=scss
cd frontend
ng serve
```

- **--routing**: Adds routing module for navigation.
- **--style=scss**: Uses SCSS for styling.

---

### **2.2 Creating a NestJS Backend Project**

```bash
npm i -g @nestjs/cli
nest new backend
cd backend
npm run start:dev
```

- **nestjs/cli**: CLI tool for creating and managing NestJS projects.
- **start:dev**: Starts the development server with **hot reload**.

---

### **2.3 Configuring CORS and Environment Variables**

**Enabling CORS in NestJS**:

**main.ts**:

```typescript
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.enableCors({
    origin: 'http://localhost:4200',
    methods: 'GET,HEAD,PUT,PATCH,POST,DELETE',
    credentials: true
  });
  await app.listen(3000);
}
bootstrap();
```

- **origin**: Specifies the allowed origin for CORS.
- **credentials**: Allows cookies to be sent with requests.

---

### **2.4 Connecting Angular with NestJS API**

1. **Create Angular Service for API Communication**

**product.service.ts**:

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { Product } from '../models/product.model';

@Injectable({ providedIn: 'root' })
export class ProductService {
  private apiUrl = 'http://localhost:3000/api/products';

  constructor(private http: HttpClient) {}

  getProducts(): Observable<Product[]> {
    return this.http.get<Product[]>(this.apiUrl);
  }
}
```

2. **Use the Service in Angular Component**

**product.component.ts**:

```typescript
import { Component, OnInit } from '@angular/core';
import { ProductService } from './product.service';
import { Product } from '../models/product.model';

@Component({
  selector: 'app-product',
  templateUrl: './product.component.html'
})
export class ProductComponent implements OnInit {
  products: Product[] = [];

  constructor(private productService: ProductService) {}

  ngOnInit(): void {
    this.productService.getProducts().subscribe(data => {
      this.products = data;
    });
  }
}
```

---

## **3. Building RESTful APIs with NestJS**

### **3.1 Creating Controllers, Services, and Modules**

1. **Generate Product Module, Service, and Controller**

```bash
nest g module products
nest g service products
nest g controller products
```

2. **Define Product Model**

**product.model.ts**:

```typescript
export class Product {
  id: string;
  name: string;
  price: number;
}
```

3. **Implement Product Service**

**product.service.ts**:

```typescript
import { Injectable } from '@nestjs/common';
import { Product } from './product.model';

@Injectable()
export class ProductService {
  private products: Product[] = [];

  getAllProducts(): Product[] {
    return this.products;
  }

  addProduct(name: string, price: number): Product {
    const product: Product = {
      id: Date.now().toString(),
      name,
      price
    };
    this.products.push(product);
    return product;
  }
}
```

4. **Implement Product Controller**

**product.controller.ts**:

```typescript
import { Controller, Get, Post, Body } from '@nestjs/common';
import { ProductService } from './product.service';
import { Product } from './product.model';

@Controller('products')
export class ProductController {
  constructor(private productService: ProductService) {}

  @Get()
  getAllProducts(): Product[] {
    return this.productService.getAllProducts();
  }

  @Post()
  addProduct(@Body('name') name: string, @Body('price') price: number): Product {
    return this.productService.addProduct(name, price);
  }
}
```

5. **Register Module in App Module**

**app.module.ts**:

```typescript
import { Module } from '@nestjs/common';
import { ProductsModule } from './products/products.module';

@Module({
  imports: [ProductsModule]
})
export class AppModule {}
```

- `@Get()` and `@Post()` define HTTP endpoints.
- `@Body()` retrieves data from the request body.

---

## **Next Lesson: Advanced Angular and NestJS Integration**
- **GraphQL Integration with Angular and NestJS**
- **Authentication and Authorization with JWT and Passport**
- **Role-Based Access Control and Guards**
- **Microservices Architecture with NestJS and Angular**
