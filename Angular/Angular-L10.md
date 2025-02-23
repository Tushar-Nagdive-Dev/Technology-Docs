### **Lesson 10: Angular Material and Bootstrap Integration**

---

## **What You Will Learn:**
1. **Introduction to Angular Material and Bootstrap**
   - Why Use Angular Material and Bootstrap?
   - Key Features and Differences
   - Setting up Angular Material and Bootstrap

2. **Using Angular Material Components**
   - Angular Material Layout and Design
   - Commonly Used Angular Material Components:
     - Navigation (Toolbar, Sidenav, Menu)
     - Buttons and Icons
     - Forms and Inputs
     - Cards and Dialogs
     - Tables and DataTables

3. **Integrating Bootstrap with Angular**
   - Responsive Grid System
   - Bootstrap Utilities and Components
   - Customizing Bootstrap Styles with SCSS

4. **Theming and Customization**
   - Custom Themes in Angular Material
   - Using Angular Material Typography and Color Schemes
   - Overriding Bootstrap Variables

5. **Hands-on Exercises:**
   - Building a Responsive Dashboard Layout
   - Real-World Scenario: Product Catalog with Material and Bootstrap Design

6. **Expert Insights and Best Practices**
7. **Common Mistakes to Avoid**
8. **Recap and Next Steps**

---

## **1. Introduction to Angular Material and Bootstrap**

### **1.1 Why Use Angular Material and Bootstrap?**

**Angular Material**:
- Designed specifically for Angular applications.
- Follows **Google's Material Design guidelines**.
- **Component-based** architecture with built-in accessibility and animations.
- Rich set of UI components like `Toolbar`, `Sidenav`, `Table`, `Dialog`, etc.
- Integrated theming and customization.

**Bootstrap**:
- **Popular CSS framework** for responsive and mobile-first design.
- **Utility-based styling** with a powerful grid system.
- Easy to use with consistent design components like buttons, cards, and modals.
- Extensive community support and third-party themes.

---

### **1.2 Key Features and Differences**

| Feature           | Angular Material                              | Bootstrap                              |
|------------------|------------------------------------------------|----------------------------------------|
| Design Language   | Material Design                               | Customizable but Bootstrap-styled       |
| Components        | Angular-specific components with animations   | Basic UI components                     |
| Flexibility       | Highly customizable with Angular themes        | Utility classes for rapid styling       |
| Grid System       | Flex Layout for responsive design              | Powerful 12-column grid system           |
| Performance       | Optimized for Angular change detection         | Lightweight and fast                    |

---

## **2. Setting up Angular Material and Bootstrap**

### **2.1 Installing Angular Material**

```bash
ng add @angular/material
```

- Prompts:
  - Choose a theme: **Indigo/Pink** (or your choice)
  - Enable global typography and animations: **Yes**

This will:
- Install `@angular/material` and `@angular/cdk`
- Add Material CSS in `angular.json`
- Configure global typography and animations

---

### **2.2 Installing Bootstrap**

```bash
npm install bootstrap
```

Add Bootstrap CSS in `angular.json`:

```json
"styles": [
  "src/styles.scss",
  "node_modules/bootstrap/dist/css/bootstrap.min.css"
]
```

---

## **3. Using Angular Material Components**

Angular Material provides a wide range of components for building rich UIs.

### **3.1 Angular Material Layout and Design**

Angular Material uses **Angular Flex Layout** for responsive design:

**Example: Responsive Layout with Flex Layout**

```html
<div fxLayout="row" fxLayoutAlign="space-between center">
  <div fxFlex="20">Left</div>
  <div fxFlex="60">Center</div>
  <div fxFlex="20">Right</div>
</div>
```

Install Angular Flex Layout:
```bash
npm install @angular/flex-layout
```

Import in `AppModule`:
```typescript
import { FlexLayoutModule } from '@angular/flex-layout';

@NgModule({
  imports: [FlexLayoutModule]
})
export class AppModule {}
```

---

### **3.2 Commonly Used Angular Material Components**

1. **Toolbar and Sidenav**

**App Module Configuration:**
```typescript
import { MatToolbarModule } from '@angular/material/toolbar';
import { MatSidenavModule } from '@angular/material/sidenav';
import { MatIconModule } from '@angular/material/icon';
import { MatButtonModule } from '@angular/material/button';

@NgModule({
  imports: [
    MatToolbarModule,
    MatSidenavModule,
    MatIconModule,
    MatButtonModule
  ]
})
export class AppModule {}
```

**Example: Toolbar and Sidenav Layout**

```html
<mat-sidenav-container>
  <mat-sidenav mode="side" opened>
    <mat-nav-list>
      <a mat-list-item href="#">Home</a>
      <a mat-list-item href="#">Products</a>
      <a mat-list-item href="#">Contact</a>
    </mat-nav-list>
  </mat-sidenav>

  <mat-sidenav-content>
    <mat-toolbar color="primary">
      <button mat-icon-button (click)="sidenav.toggle()">
        <mat-icon>menu</mat-icon>
      </button>
      <span>My Angular App</span>
    </mat-toolbar>

    <div class="content">
      <router-outlet></router-outlet>
    </div>
  </mat-sidenav-content>
</mat-sidenav-container>
```

---

2. **Forms and Inputs**

**App Module Configuration:**
```typescript
import { MatInputModule } from '@angular/material/input';
import { MatFormFieldModule } from '@angular/material/form-field';
import { MatButtonModule } from '@angular/material/button';

@NgModule({
  imports: [
    MatInputModule,
    MatFormFieldModule,
    MatButtonModule
  ]
})
export class AppModule {}
```

**Example: Material Form**

```html
<form>
  <mat-form-field>
    <mat-label>Username</mat-label>
    <input matInput placeholder="Enter your username" required>
  </mat-form-field>

  <mat-form-field>
    <mat-label>Password</mat-label>
    <input matInput placeholder="Enter your password" type="password" required>
  </mat-form-field>

  <button mat-raised-button color="primary">Login</button>
</form>
```

---

3. **Cards and Dialogs**

**App Module Configuration:**
```typescript
import { MatCardModule } from '@angular/material/card';
import { MatDialogModule } from '@angular/material/dialog';

@NgModule({
  imports: [
    MatCardModule,
    MatDialogModule
  ]
})
export class AppModule {}
```

**Example: Product Card**

```html
<mat-card>
  <mat-card-header>
    <mat-card-title>Product Name</mat-card-title>
    <mat-card-subtitle>$29.99</mat-card-subtitle>
  </mat-card-header>
  <img mat-card-image src="assets/product.jpg" alt="Product Image">
  <mat-card-content>
    <p>Product Description goes here.</p>
  </mat-card-content>
  <mat-card-actions>
    <button mat-button>BUY NOW</button>
    <button mat-button>ADD TO CART</button>
  </mat-card-actions>
</mat-card>
```

---

## **4. Integrating Bootstrap with Angular**

### **4.1 Responsive Grid System**

Bootstrap uses a 12-column grid system for responsive layouts.

**Example: Bootstrap Grid Layout**

```html
<div class="container">
  <div class="row">
    <div class="col-md-4">Column 1</div>
    <div class="col-md-4">Column 2</div>
    <div class="col-md-4">Column 3</div>
  </div>
</div>
```

### **4.2 Bootstrap Utilities and Components**

**Example: Buttons and Alerts**

```html
<button class="btn btn-primary">Primary Button</button>
<div class="alert alert-success" role="alert">
  This is a success alert—check it out!
</div>
```

---

## **5. Theming and Customization**

### **5.1 Custom Themes in Angular Material**

Create a custom theme in `src/styles.scss`:

```scss
@use '@angular/material' as mat;

$custom-primary: mat.define-palette(mat.$indigo-palette);
$custom-accent: mat.define-palette(mat.$pink-palette, A200, A100, A400);
$custom-theme: mat.define-light-theme((
  color: (
    primary: $custom-primary,
    accent: $custom-accent
  )
));

@include mat.all-component-typographies($custom-theme);
@include mat.all-component-themes($custom-theme);
```

### **5.2 Overriding Bootstrap Variables**

Create `_variables.scss`:
```scss
$primary: #3f51b5;
$secondary: #f50057;
$body-bg: #f4f4f9;
```

Import in `styles.scss`:
```scss
@import 'variables';
```

---

## **Next Lesson: Angular Animations**
- **Introduction to Angular Animations**
- **Triggering Animations**
- **Transition States and Keyframes**
- **Complex Animations and Sequencing**
