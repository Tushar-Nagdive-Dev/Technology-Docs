### **Lesson 30: Angular Universal and SSR with NestJS**

---

## **What You Will Learn:**
1. **Introduction to Angular Universal and SSR**
   - What is Angular Universal?
   - Why Use Server-Side Rendering (SSR)?
   - Benefits of Angular Universal with NestJS
   - Key Concepts: Pre-rendering, Hydration, and SEO Optimization

2. **Setting up Angular Universal with NestJS**
   - Installing Angular Universal and Express Engine
   - Integrating Angular Universal with NestJS
   - Configuring Server-Side Rendering (SSR)
   - Building and Running Angular Universal with NestJS

3. **SEO Optimization with Angular Universal**
   - Improving SEO with Server-Side Rendering
   - Dynamic Meta Tags and Open Graph Protocol
   - Integrating Angular Meta Service for SEO
   - Pre-rendering Static Pages for Better SEO

4. **Performance Optimization with Angular Universal**
   - Faster First Contentful Paint (FCP)
   - Reducing Time to Interactive (TTI)
   - Lazy Loading and Code Splitting
   - Caching Strategies and CDN Integration

5. **Dynamic Content and API Integration**
   - Server-Side Data Fetching with Angular Universal
   - Integrating RESTful and GraphQL APIs with SSR
   - Handling Asynchronous Data in Angular Universal
   - Managing State and Data Hydration

6. **Deployment and Hosting of SSR Applications**
   - Dockerizing Angular Universal and NestJS Applications
   - Deploying SSR Applications on:
     - Firebase Hosting
     - AWS EC2 and Lambda
     - Vercel and Netlify
   - CI/CD Pipelines for SSR with GitHub Actions

7. **Hands-on Exercises:**
   - Building an SEO-Optimized Blog with Angular Universal
   - Real-World Scenario: Dynamic E-commerce Store with Angular Universal and NestJS

8. **Expert Insights and Best Practices**
9. **Common Mistakes to Avoid**
10. **Recap and Next Steps**

---

## **1. Introduction to Angular Universal and SSR**

### **1.1 What is Angular Universal?**
- **Angular Universal** is a **Server-Side Rendering (SSR)** solution for Angular applications.
- It pre-renders Angular applications on the **server** and sends **static HTML** to the client.
- **Angular Universal** uses **Express Engine** to:
  - Render Angular applications on the server.
  - Send fully rendered HTML to the browser.
  - Boot Angular client-side to enable interactivity.

---

### **1.2 Why Use Server-Side Rendering (SSR)?**
- **Improved SEO**:
  - Pre-rendered HTML is **crawlable** by search engines.
  - Improves SEO rankings for Angular applications.

- **Faster Page Load**:
  - Users see content faster due to server-rendered HTML.
  - Reduces **Time to First Byte (TTFB)** and **First Contentful Paint (FCP)**.

- **Social Media Sharing**:
  - Open Graph tags and meta descriptions are pre-rendered.
  - Accurate previews on Facebook, Twitter, and LinkedIn.

- **Better Performance on Low-Powered Devices**:
  - Offloads rendering from client to server.

---

### **1.3 Benefits of Angular Universal with NestJS**
- **Unified Architecture**:
  - Integrate Angular Universal with NestJS for **full-stack SSR**.
  - Single codebase for client-side and server-side logic.

- **SEO and Social Sharing**:
  - Dynamic meta tags and Open Graph integration.

- **Performance Optimization**:
  - Faster initial load with pre-rendered HTML.
  - Better **First Contentful Paint (FCP)** and **Time to Interactive (TTI)**.

---

### **1.4 Key Concepts**

1. **Pre-rendering**:
   - Generates static HTML on the server and sends it to the client.

2. **Hydration**:
   - Angular bootstraps on the client-side and takes over the pre-rendered HTML.

3. **Dynamic Meta Tags**:
   - Meta tags and Open Graph tags are dynamically rendered on the server.

4. **Lazy Loading and Code Splitting**:
   - Load modules and components **on demand** to optimize performance.

---

## **2. Setting up Angular Universal with NestJS**

### **2.1 Installing Angular Universal and Express Engine**

```bash
ng add @nguniversal/express-engine
```

This command:
- Installs **@nguniversal/express-engine** and dependencies.
- Configures Angular for **Server-Side Rendering**.
- Generates:
  - **server.ts**: Entry point for the server.
  - **main.server.ts**: Server-side bootstrap file.
  - **tsconfig.server.json**: TypeScript configuration for server-side rendering.

---

### **2.2 Integrating Angular Universal with NestJS**

1. **Install Dependencies for NestJS Integration**

```bash
npm install @nestjs/ng-universal express
```

2. **Configure Angular Universal with NestJS**

**main.server.ts**:

```typescript
import 'zone.js/node';
import { enableProdMode } from '@angular/core';
import { ngExpressEngine } from '@nguniversal/express-engine';
import * as express from 'express';
import { join } from 'path';
import { AppServerModule } from './src/main.server';

enableProdMode();

export function app(): express.Express {
  const server = express();
  const distFolder = join(process.cwd(), 'dist/your-app-name/browser');
  const indexHtml = 'index.html';

  server.engine('html', ngExpressEngine({
    bootstrap: AppServerModule,
  }));

  server.set('view engine', 'html');
  server.set('views', distFolder);

  server.get('*.*', express.static(distFolder, {
    maxAge: '1y'
  }));

  server.get('*', (req, res) => {
    res.render(indexHtml, { req });
  });

  return server;
}

function run(): void {
  const port = process.env.PORT || 4000;

  const server = app();
  server.listen(port, () => {
    console.log(`Node Express server listening on http://localhost:${port}`);
  });
}

declare const __non_webpack_require__: NodeRequire;

if (require.main === module) {
  run();
}
```

- **ngExpressEngine**: Angular Express engine for SSR.
- **AppServerModule**: The Angular module bootstrapped on the server.
- **distFolder**: Directory of pre-rendered HTML files.
- **indexHtml**: Entry point for server-side rendering.

---

### **2.3 Configuring Server-Side Rendering (SSR)**

1. **Add Server-Side Module**

**app.server.module.ts**:

```typescript
import { NgModule } from '@angular/core';
import { ServerModule } from '@angular/platform-server';
import { AppModule } from './app.module';
import { AppComponent } from './app.component';

@NgModule({
  imports: [
    AppModule,
    ServerModule
  ],
  bootstrap: [AppComponent],
})
export class AppServerModule {}
```

- **ServerModule**: Angular module for server-side rendering.

2. **Update Angular Configuration**

**angular.json**:

```json
"server": {
  "builder": "@angular-devkit/build-angular:server",
  "options": {
    "outputPath": "dist/your-app-name/server",
    "main": "src/main.server.ts",
    "tsConfig": "tsconfig.server.json"
  }
}
```

- Configures the **server build** for Angular Universal.

---

### **2.4 Building and Running Angular Universal with NestJS**

1. **Build Angular Universal Application**

```bash
npm run build:ssr
```

- Builds both **browser** and **server** versions.

2. **Serve Angular Universal Application**

```bash
npm run serve:ssr
```

- Starts the **Node.js server** for SSR.
- URL: `http://localhost:4000`

- The application is now **server-side rendered** and **SEO-friendly**.

---

## **3. SEO Optimization with Angular Universal**

### **3.1 Dynamic Meta Tags and Open Graph Protocol**

**Using Angular Meta Service**:

```typescript
constructor(private meta: Meta, private title: Title) {}

ngOnInit() {
  this.title.setTitle('Angular Universal SEO Example');
  this.meta.addTags([
    { name: 'description', content: 'Learn Angular Universal SEO' },
    { name: 'keywords', content: 'Angular, Universal, SEO, SSR' }
  ]);
}
```

- **Meta Service** dynamically updates meta tags on the server.

---

## **Next Lesson: Angular Enterprise Architecture and Scalable Patterns**
- **Scalable Folder Structures and Module Architecture**
- **Enterprise State Management with NgRx and RxJS**
- **Performance Optimization and Caching**
- **Security and Role-Based Authorization in Enterprise Apps**
