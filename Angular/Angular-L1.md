### **Lesson 1: Angular Architecture and Component-Based Design**

---

## **What You Will Learn:**
1. **Understanding Angular Architecture**
   - Modules
   - Components
   - Templates
   - Metadata
   - Data Binding
   - Directives
   - Services and Dependency Injection

2. **Component-Based Design Philosophy**
   - Importance of components
   - Reusability and Maintainability
   - Modular Architecture

3. **Hands-on Exercise: Building a Basic Component Structure**

---

## **1. Understanding Angular Architecture**

Angular is a **component-based framework** designed to create dynamic web applications. It follows a modular architecture that makes the codebase easy to maintain and scalable. Let's break down its fundamental building blocks.

---

### **1.1 Angular Building Blocks:**

1. **Modules (NgModules):**
   - Angular applications are modular and are defined using **NgModules**.
   - Every Angular app has at least one module called the **Root Module** (usually `AppModule`).
   - Modules group related components, services, directives, and pipes.
   - Example:
     ```typescript
     @NgModule({
       declarations: [
         AppComponent,
         HeaderComponent,
         FooterComponent
       ],
       imports: [
         BrowserModule,
         AppRoutingModule
       ],
       providers: [],
       bootstrap: [AppComponent]
     })
     export class AppModule { }
     ```

2. **Components:**
   - **Components** are the building blocks of Angular applications.
   - They control a portion of the view (UI) called a **View Component**.
   - A component consists of:
     - **HTML Template**: Defines the view layout.
     - **CSS Styles**: Styles specific to the component.
     - **TypeScript Class**: Contains data and logic.
     - **Metadata**: Configures the component.
   - Example:
     ```typescript
     @Component({
       selector: 'app-header',
       templateUrl: './header.component.html',
       styleUrls: ['./header.component.scss']
     })
     export class HeaderComponent {
       title = 'Angular Mastery';
     }
     ```

3. **Templates:**
   - Define the layout and structure of a view.
   - Can contain Angular-specific syntax like **Interpolation**, **Directives**, and **Data Binding**.
   - Example:
     ```html
     <h1>{{ title }}</h1>
     <p>Welcome to {{ title }} course!</p>
     ```

4. **Metadata:**
   - Metadata tells Angular how to process a class.
   - It is defined using **Decorators** such as `@Component`, `@NgModule`, `@Injectable`, etc.

5. **Data Binding:**
   - Communication between the component’s class and its template.
   - Four types of data binding:
     - **Interpolation**: `{{ data }}`
     - **Property Binding**: `[property]="data"`
     - **Event Binding**: `(event)="handler()"`
     - **Two-Way Binding**: `[(ngModel)]="data"`

6. **Directives:**
   - Special classes that manipulate the DOM.
   - **Structural Directives**: Change the structure of the DOM (`*ngIf`, `*ngFor`).
   - **Attribute Directives**: Change the appearance or behavior of an element (`[ngStyle]`, `[ngClass]`).

7. **Services and Dependency Injection:**
   - **Services** provide data and business logic to components.
   - Angular uses **Dependency Injection (DI)** to supply dependencies to classes.
   - Example:
     ```typescript
     @Injectable({
       providedIn: 'root'
     })
     export class DataService {
       getData() {
         return ['Angular', 'TypeScript', 'RxJS'];
       }
     }
     ```

---

## **2. Component-Based Design Philosophy**

Angular embraces the component-based design approach, which promotes:
- **Modularity:** Breaking down the application into smaller, self-contained components.
- **Reusability:** Components can be reused across different parts of the application.
- **Maintainability:** Easier to maintain and manage a modular codebase.
- **Testability:** Components can be individually tested.

---

### **2.1 Why Components?**
- **Encapsulation:** Each component encapsulates its structure, style, and behavior.
- **Separation of Concerns:** Separation between UI (HTML), styles (CSS), and logic (TypeScript).
- **Reusability:** Easily reusable across multiple views.

---

### **2.2 Component Structure:**
Each component consists of:
- **Selector:** Custom HTML tag that represents the component.
- **Template:** The HTML layout.
- **Style:** Component-specific CSS/SCSS styles.
- **Class:** Contains data and behavior.

Example:
```typescript
@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  styleUrls: ['./example.component.scss']
})
export class ExampleComponent {
  title: string = 'Hello Angular!';
}
```

---

## **3. Hands-on Exercise: Building a Basic Component Structure**

Let's create a **Header Component** and a **Footer Component** to understand the component structure better.

### **Step 1: Generate Components**
Open the terminal and navigate to your Angular project directory. Then, run:
```bash
ng generate component header
ng generate component footer
```

This will create:
- `header.component.ts`
- `header.component.html`
- `header.component.scss`
- `footer.component.ts`
- `footer.component.html`
- `footer.component.scss`

---

### **Step 2: Edit Header Component**

Open `src/app/header/header.component.html` and add:
```html
<header>
  <h1>Welcome to Angular Mastery!</h1>
  <nav>
    <a href="#">Home</a>
    <a href="#">About</a>
    <a href="#">Contact</a>
  </nav>
</header>
```

Open `src/app/header/header.component.scss` and add:
```scss
header {
  background-color: #3f51b5;
  color: white;
  padding: 1em;
  text-align: center;
}

nav a {
  color: white;
  margin: 0 10px;
  text-decoration: none;
}
```

---

### **Step 3: Edit Footer Component**

Open `src/app/footer/footer.component.html` and add:
```html
<footer>
  <p>&copy; 2025 Angular Mastery. All rights reserved.</p>
</footer>
```

Open `src/app/footer/footer.component.scss` and add:
```scss
footer {
  background-color: #303f9f;
  color: white;
  text-align: center;
  padding: 1em;
  position: fixed;
  width: 100%;
  bottom: 0;
}
```

---

### **Step 4: Integrate in App Component**

Open `src/app/app.component.html` and modify it as follows:
```html
<app-header></app-header>
<router-outlet></router-outlet>
<app-footer></app-footer>
```

---

### **Step 5: Serve the Application**

```bash
ng serve
```
Open the browser at `http://localhost:4200` to see your new Header and Footer components in action.

---

## **Expert Insights:**
- Keep components small and focused on a single task.
- Follow consistent naming conventions (`ComponentNameComponent`).
- Maintain a clean folder structure for scalability.

---

## **Common Mistakes to Avoid:**
- Not declaring the component in the appropriate NgModule.
- Forgetting to add the component selector in the template.
- Writing complex logic in the component template instead of the class.

---

## **Next Steps:**
In the next lesson, we will dive into **Data Binding and Event Binding**:
- **Interpolation** and **Property Binding**.
- **Event Binding** and **Two-Way Data Binding**.
- Practical examples to reinforce understanding.

---
