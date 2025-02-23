### **Lesson 26: Building Angular Micro Frontends**

---

## **What You Will Learn:**
1. **Introduction to Micro Frontends Architecture**
   - What are Micro Frontends?
   - Why Use Micro Frontends in Angular?
   - Advantages and Challenges of Micro Frontends
   - Core Concepts: Module Federation, Independent Deployments

2. **Module Federation with Angular**
   - What is Module Federation?
   - How Module Federation Works in Angular
   - Setting up Module Federation with Webpack 5
   - Sharing Components, Modules, and State

3. **Building Micro Frontends with Angular**
   - Creating Multiple Angular Applications as Micro Frontends
   - Configuring Module Federation for Shell and Remote Apps
   - Exposing and Consuming Modules Dynamically
   - Routing and Navigation Between Micro Frontends

4. **State Sharing and Communication**
   - Sharing Global State Across Micro Frontends
   - Using NgRx Store for State Management
   - Event Bus Pattern for Cross-App Communication
   - Data Passing with Input, Output, and Services

5. **Deploying and Scaling Micro Frontends**
   - Independent Deployment of Micro Frontends
   - Versioning and Compatibility Management
   - Continuous Integration and Delivery (CI/CD) for Micro Frontends
   - Scaling and Performance Optimization

6. **Hands-on Exercises:**
   - Building a Shell App with Multiple Micro Frontends
   - Real-World Scenario: E-commerce Application with Modular Frontends

7. **Expert Insights and Best Practices**
8. **Common Mistakes to Avoid**
9. **Recap and Next Steps**

---

## **1. Introduction to Micro Frontends Architecture**

### **1.1 What are Micro Frontends?**
- **Micro Frontends** is an architectural style where a frontend application is **split into multiple independent modules**.
- Each module is a **self-contained frontend application** with its own codebase, dependencies, and deployment pipeline.
- Micro Frontends are inspired by **Microservices Architecture** but for frontend development.

---

### **1.2 Why Use Micro Frontends in Angular?**
- **Scalable Architecture**: Allows independent development, deployment, and scaling of each module.
- **Technology Agnostic**: Different modules can use different frontend frameworks.
- **Team Autonomy**: Enables cross-functional teams to work on separate modules.
- **Incremental Upgrades**: Gradual adoption of new technologies without a complete rewrite.

---

### **1.3 Advantages and Challenges of Micro Frontends**

1. **Advantages**:
   - **Independent Deployment**: Each micro frontend can be deployed separately.
   - **Maintainability**: Smaller, focused codebases are easier to maintain.
   - **Resilience**: Isolated failures do not impact the entire application.

2. **Challenges**:
   - **Complex Routing**: Routing across multiple modules requires coordination.
   - **Shared State Management**: Managing global state across modules is challenging.
   - **Cross-App Communication**: Communication between micro frontends requires consistent patterns.

---

### **1.4 Core Concepts**

1. **Module Federation**:
   - **Webpack 5 Module Federation** is the key technology enabling Micro Frontends in Angular.
   - Allows sharing of modules, components, and state **dynamically at runtime**.

2. **Shell and Remote Architecture**:
   - **Shell Application**:
     - Main container application.
     - Manages routing and navigation.
     - Loads micro frontends as remote modules.
   - **Remote Applications**:
     - Independent micro frontends exposed as modules.
     - Loaded by the shell application dynamically.

---

## **2. Module Federation with Angular**

### **2.1 What is Module Federation?**
- **Module Federation** is a feature of **Webpack 5** that allows applications to:
  - **Share modules** between independent applications.
  - **Dynamically load remote modules** at runtime.
  - **Expose components and modules** for consumption by other applications.

---

### **2.2 How Module Federation Works in Angular**
- **Webpack Module Federation** uses two key roles:
  1. **Host**: Application that loads remote modules dynamically.
  2. **Remote**: Application that exposes modules for consumption.

- Angular uses **custom builders** from **@angular-architects/module-federation** for integrating Module Federation.

---

### **2.3 Setting up Module Federation with Webpack 5**

1. **Install Module Federation Plugin**

```bash
npm install @angular-architects/module-federation --save-dev
```

2. **Initialize Module Federation**

```bash
ng add @angular-architects/module-federation --project shell
ng add @angular-architects/module-federation --project remote
```

- The command:
  - Configures **Webpack 5** in `webpack.config.js`.
  - Adds `ModuleFederationPlugin` for exposing and consuming modules.
  - Updates **Angular Builder** for custom Webpack configuration.

---

### **2.4 Configuring Module Federation**

**Shell Application (Host)**:

**webpack.config.js**:

```js
const ModuleFederationPlugin = require('webpack/lib/container/ModuleFederationPlugin');
const deps = require('./package.json').dependencies;

module.exports = {
  output: {
    publicPath: 'http://localhost:4200/'
  },
  plugins: [
    new ModuleFederationPlugin({
      remotes: {
        remoteApp: 'remoteApp@http://localhost:4201/remoteEntry.js'
      },
      shared: {
        ...deps,
        '@angular/core': { singleton: true, strictVersion: true, requiredVersion: deps['@angular/core'] },
        '@angular/common': { singleton: true, strictVersion: true, requiredVersion: deps['@angular/common'] }
      }
    })
  ]
};
```

**Remote Application (Remote)**:

**webpack.config.js**:

```js
const ModuleFederationPlugin = require('webpack/lib/container/ModuleFederationPlugin');

module.exports = {
  output: {
    publicPath: 'http://localhost:4201/'
  },
  plugins: [
    new ModuleFederationPlugin({
      name: 'remoteApp',
      filename: 'remoteEntry.js',
      exposes: {
        './ProductModule': './src/app/product/product.module.ts'
      },
      shared: {
        '@angular/core': { singleton: true },
        '@angular/common': { singleton: true }
      }
    })
  ]
};
```

- **Exposes**: `ProductModule` is exposed for consumption.
- **Remotes**: The shell application loads the remote module from `remoteApp`.

---

### **2.5 Sharing Components, Modules, and State**

1. **Exposing Components and Modules**

**Remote Application**:

```js
exposes: {
  './ProductComponent': './src/app/product/product.component.ts'
}
```

2. **Consuming Remote Modules in Shell**

**App Routing Module (Shell)**:

```typescript
const routes: Routes = [
  {
    path: 'products',
    loadChildren: () =>
      import('remoteApp/ProductModule').then((m) => m.ProductModule)
  }
];
```

- `ProductModule` is **dynamically loaded** from the remote application.

---

## **3. Building Micro Frontends with Angular**

### **3.1 Creating Multiple Angular Applications**

1. **Create Shell Application**

```bash
ng new shell --routing --style=scss
cd shell
ng serve
```

2. **Create Remote Application**

```bash
ng new remote --routing --style=scss
cd remote
ng serve --port 4201
```

- **Shell** is the main container application.
- **Remote** is the micro frontend exposed to the shell.

---

### **3.2 Routing and Navigation Between Micro Frontends**

- The **Shell Application** handles **global routing**.
- Each **Remote Application** manages its own routing.

**Shell Routing**:

```typescript
const routes: Routes = [
  { path: 'home', component: HomeComponent },
  {
    path: 'products',
    loadChildren: () => import('remoteApp/ProductModule').then(m => m.ProductModule)
  },
  { path: '**', redirectTo: 'home' }
];
```

**Remote Routing**:

```typescript
const routes: Routes = [
  { path: '', component: ProductListComponent },
  { path: ':id', component: ProductDetailComponent }
];
```

- The shell routes to `remoteApp` for products.
- `remoteApp` manages its own nested routes.

---

## **Next Lesson: Angular + GraphQL Integration**
- **Introduction to GraphQL and Apollo Client**
- **Setting up GraphQL in Angular**
- **Query, Mutation, and Subscription with Apollo**
- **GraphQL Caching and State Management**
- **Best Practices and Performance Optimization**
