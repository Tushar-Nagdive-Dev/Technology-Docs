### **Lesson 17: Angular Universal (Server-Side Rendering)**

---

## **What You Will Learn:**
1. **Introduction to Angular Universal**
   - What is Angular Universal?
   - Why Use Server-Side Rendering (SSR)?
   - How Angular Universal Works

2. **Setting up Angular Universal**
   - Installing Angular Universal
   - Configuring Server-Side Rendering
   - Building and Running Angular Universal

3. **SEO Benefits and Optimization**
   - Improving SEO with Server-Side Rendering
   - Meta Tags and Open Graph Integration
   - Dynamic Meta Tag Rendering

4. **Performance Optimization with Angular Universal**
   - Faster First Contentful Paint (FCP)
   - Improving Time to Interactive (TTI)
   - Lazy Loading and Pre-rendering

5. **Deploying Angular Universal**
   - Deploying on Firebase Hosting
   - Deploying on AWS (Lambda and S3)
   - Deploying on VPS (Node.js and Nginx)

6. **Hands-on Exercises:**
   - Converting an Angular App to Angular Universal
   - Real-World Scenario: SEO Optimization for a Blog Website

7. **Expert Insights and Best Practices**
8. **Common Mistakes to Avoid**
9. **Recap and Next Steps**

---

## **1. Introduction to Angular Universal**

### **1.1 What is Angular Universal?**
- **Angular Universal** is a **Server-Side Rendering (SSR)** solution for Angular applications.
- It renders Angular applications on the server and sends **pre-rendered HTML** to the client.
- Angular Universal provides:
  - **Faster Page Load**: Initial content is rendered on the server.
  - **SEO Optimization**: Search engines can crawl pre-rendered content.
  - **Social Media Sharing**: Meta tags and Open Graph data are rendered for better sharing.

---

### **1.2 Why Use Server-Side Rendering (SSR)?**
- **Improved SEO**: Search engines can index server-rendered pages more effectively.
- **Faster First Paint**: Users see the content faster, even on slow networks.
- **Reduced Time to Interactive (TTI)**: Faster rendering of static content.
- **Social Media Preview**: Accurate previews on platforms like Facebook, Twitter, and LinkedIn.
- **Better Performance on Low-Powered Devices**: Offloads rendering from client to server.

---

### **1.3 How Angular Universal Works**
1. **Request**: User requests a page from the server.
2. **Server Rendering**:
   - Angular Universal **pre-renders** the HTML on the server.
   - The **Angular application state** is serialized and embedded in the HTML.
3. **Response**:
   - The **pre-rendered HTML** is sent to the client.
   - The **Angular application bootstraps** on the client side.
   - Angular takes over and makes the app interactive.

---

## **2. Setting up Angular Universal**

### **2.1 Installing Angular Universal**

Angular provides the `@nguniversal/express-engine` package for setting up Angular Universal with **Express.js**.

```bash
ng add @nguniversal/express-engine
```

This will:
- Install `@nguniversal/express-engine` and its dependencies.
- Configure **server-side rendering** with **Express.js**.
- Generate a `server.ts` file for the Node.js server.
- Update `angular.json` to include server builds.

---

### **2.2 Configuring Server-Side Rendering**

1. **server.ts**:
   - This file acts as the entry point for the Node.js server.
   - It uses **Express.js** to serve the Angular application.

**Example: server.ts**

```typescript
import 'zone.js/node';
import { ngExpressEngine } from '@nguniversal/express-engine';
import express from 'express';
import { join } from 'path';
import { AppServerModule } from './src/main.server';

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

- **ngExpressEngine** is used to render Angular templates with Express.js.
- **AppServerModule** is the server-side entry point for the Angular app.

---

### **2.3 Building and Running Angular Universal**

1. **Build the Angular Universal Application**

```bash
npm run build:ssr
```

This command builds two versions:
- **Browser** version in `dist/your-app-name/browser`
- **Server** version in `dist/your-app-name/server`

---

2. **Serve the Angular Universal Application**

```bash
npm run serve:ssr
```

- This starts the Node.js server on `http://localhost:4000`.
- The application is now **server-side rendered** and SEO-friendly.

---

## **3. SEO Benefits and Optimization**

### **3.1 Improving SEO with Server-Side Rendering**
- Angular Universal provides **pre-rendered HTML**, which search engines can crawl and index.
- This improves the **SEO ranking** of the application.

---

### **3.2 Meta Tags and Open Graph Integration**

1. **Meta Tags**:
   - Meta tags provide SEO-related information like titles, descriptions, and keywords.

**Example: Using Meta Tags in Angular Universal**

```typescript
import { Component, OnInit } from '@angular/core';
import { Meta, Title } from '@angular/platform-browser';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html'
})
export class HomeComponent implements OnInit {
  constructor(private title: Title, private meta: Meta) {}

  ngOnInit(): void {
    this.title.setTitle('Angular Universal Example');
    this.meta.addTags([
      { name: 'description', content: 'Learn Angular Universal for SEO' },
      { name: 'keywords', content: 'Angular, Universal, SEO, SSR' }
    ]);
  }
}
```

2. **Open Graph Tags**:
   - Open Graph tags improve sharing on social media platforms.

**Example: Open Graph Meta Tags**

```html
<meta property="og:title" content="Angular Universal Example">
<meta property="og:description" content="Learn Angular Universal for SEO">
<meta property="og:image" content="https://example.com/image.png">
<meta property="og:url" content="https://example.com">
```

- Add Open Graph tags in the `index.html` or dynamically in components.

---

### **3.3 Dynamic Meta Tag Rendering**

Use Angular's **Meta** and **Title** services to update meta tags dynamically based on route parameters or state.

---

## **4. Performance Optimization with Angular Universal**

### **4.1 Faster First Contentful Paint (FCP)**
- Server-side rendering provides a **pre-rendered HTML shell**.
- Users see the initial content faster, improving **First Contentful Paint (FCP)**.

---

### **4.2 Lazy Loading and Pre-rendering**

1. **Lazy Loading**:
   - Load feature modules on demand to **reduce initial bundle size**.

```typescript
const routes: Routes = [
  { path: 'products', loadChildren: () => import('./products/products.module').then(m => m.ProductsModule) }
];
```

2. **Pre-rendering with Angular Universal**:
   - Use **Scully** or **Guess.js** for pre-rendering static pages.

---

## **5. Deploying Angular Universal**

### **5.1 Deploying on Firebase Hosting**

1. **Install Firebase Tools**:

```bash
npm install -g firebase-tools
```

2. **Initialize Firebase Hosting**:

```bash
firebase init
```

3. **Deploy to Firebase**:

```bash
firebase deploy
```

---

### **5.2 Deploying on AWS (Lambda and S3)**
- Deploy the **browser build** to **S3** as a static website.
- Deploy the **server build** to **AWS Lambda** using **Serverless Framework**.

---

### **5.3 Deploying on VPS (Node.js and Nginx)**
- Serve the **browser build** with **Nginx**.
- Serve the **server build** using **Node.js** with **PM2**.

---

## **Next Lesson: Advanced Angular Concepts and Best Practices**
- **Dependency Injection and Hierarchical Injectors**
- **Angular Module Architecture and Lazy Loading Patterns**
- **Advanced Change Detection and Zone.js**
- **Performance Profiling and Optimization**
