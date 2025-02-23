### **Lesson 3: Directives and Pipes in Angular**

---

## **What You Will Learn:**
1. **Understanding Directives in Angular**
   - Structural Directives
     - `*ngIf`
     - `*ngFor`
     - `*ngSwitch`
   - Attribute Directives
     - `[ngStyle]`
     - `[ngClass]`
   - Custom Directives

2. **Understanding Pipes in Angular**
   - Built-in Pipes
     - `DatePipe`
     - `CurrencyPipe`
     - `UpperCasePipe`, `LowerCasePipe`, and `TitleCasePipe`
   - Custom Pipes

3. **Hands-on Exercises:**
   - Using Directives and Pipes in a `ProductListComponent`
   - Real-World Scenario: Displaying a List of Products

4. **Expert Insights and Best Practices**
5. **Common Mistakes to Avoid**
6. **Recap and Next Steps**

---

## **1. Understanding Directives in Angular**

### **What are Directives?**
- Directives are special classes that allow you to manipulate the DOM.
- They add behavior to elements in your Angular templates.

### **Types of Directives:**
1. **Structural Directives**: Change the structure of the DOM.
2. **Attribute Directives**: Change the appearance or behavior of an element.
3. **Custom Directives**: Custom behavior defined by the developer.

---

### **1.1 Structural Directives**

Structural directives modify the layout by adding or removing DOM elements. They are prefixed with an asterisk (`*`).

---

#### **1.1.1 `*ngIf`**
- Condition-based rendering of DOM elements.
- **Syntax**:
  ```html
  <div *ngIf="isVisible">This is visible only if isVisible is true.</div>
  ```
- **Example**:
  ```html
  <button (click)="toggleVisibility()">Toggle</button>
  <p *ngIf="isVisible">Now you see me!</p>
  <p *ngIf="!isVisible">Now you don't!</p>
  ```
- **Component Class**:
  ```typescript
  isVisible = true;

  toggleVisibility() {
    this.isVisible = !this.isVisible;
  }
  ```

---

#### **1.1.2 `*ngFor`**
- Looping mechanism to display a list of items.
- **Syntax**:
  ```html
  <li *ngFor="let item of items">{{ item }}</li>
  ```
- **Example**:
  ```html
  <ul>
    <li *ngFor="let product of products">
      {{ product.name }} - ${{ product.price }}
    </li>
  </ul>
  ```
- **Component Class**:
  ```typescript
  products = [
    { name: 'Angular Book', price: 29.99 },
    { name: 'TypeScript Guide', price: 19.99 },
    { name: 'RxJS Manual', price: 24.99 }
  ];
  ```

---

#### **1.1.3 `*ngSwitch`**
- Display DOM elements conditionally based on matching a value.
- **Syntax**:
  ```html
  <div [ngSwitch]="color">
    <p *ngSwitchCase="'red'">You selected Red!</p>
    <p *ngSwitchCase="'green'">You selected Green!</p>
    <p *ngSwitchDefault>Select a color.</p>
  </div>
  ```
- **Component Class**:
  ```typescript
  color = 'red';
  ```

---

### **1.2 Attribute Directives**

Attribute directives change the appearance or behavior of an element.

---

#### **1.2.1 `[ngStyle]`**
- Apply dynamic inline styles to an element.
- **Syntax**:
  ```html
  <div [ngStyle]="{ 'color': textColor, 'font-size': fontSize + 'px' }">
    Dynamic Styling Example
  </div>
  ```
- **Component Class**:
  ```typescript
  textColor = 'blue';
  fontSize = 20;
  ```

---

#### **1.2.2 `[ngClass]`**
- Apply dynamic classes to an element.
- **Syntax**:
  ```html
  <div [ngClass]="{ 'active': isActive, 'disabled': !isActive }">
    Conditional Class Example
  </div>
  ```
- **Component Class**:
  ```typescript
  isActive = true;
  ```

---

### **1.3 Custom Directives**

Custom directives are used to create reusable functionalities.

---

### **Example: Highlight Directive**

**Step 1: Generate Directive**
```bash
ng generate directive highlight
```

**Step 2: Define Custom Behavior**

Open `src/app/highlight.directive.ts` and update:

```typescript
import { Directive, ElementRef, HostListener, Renderer2 } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  constructor(private el: ElementRef, private renderer: Renderer2) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.renderer.setStyle(this.el.nativeElement, 'color', 'red');
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.renderer.setStyle(this.el.nativeElement, 'color', 'black');
  }
}
```

**Step 3: Use in Template**

Open any component template:
```html
<p appHighlight>Hover over this text to see the highlight effect.</p>
```

---

## **2. Understanding Pipes in Angular**

Pipes are used to transform data before displaying it in the view.

### **Built-in Pipes:**
1. **DatePipe**: Formats dates.
   ```html
   <p>{{ today | date:'fullDate' }}</p>
   ```
2. **CurrencyPipe**: Formats currency values.
   ```html
   <p>{{ price | currency:'USD' }}</p>
   ```
3. **UpperCasePipe**, **LowerCasePipe**, **TitleCasePipe**: Change text case.
   ```html
   <p>{{ text | uppercase }}</p>
   <p>{{ text | lowercase }}</p>
   <p>{{ text | titlecase }}</p>
   ```

---

### **Custom Pipe Example**

**Step 1: Generate Pipe**
```bash
ng generate pipe discount
```

**Step 2: Define Pipe Logic**

Open `src/app/discount.pipe.ts` and update:

```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'discount'
})
export class DiscountPipe implements PipeTransform {
  transform(price: number, discount: number): number {
    return price - (price * (discount / 100));
  }
}
```

**Step 3: Use in Template**

```html
<p>Original Price: ${{ price }}</p>
<p>Discounted Price: ${{ price | discount:10 }}</p>
```

**Component Class**:
```typescript
price = 100;
```

---

## **3. Real-World Scenario: Displaying a List of Products**

Let's create a `ProductListComponent` to display a list of products with directives and pipes.

### **Step 1: Generate Component**

```bash
ng generate component product-list
```

### **Step 2: Define Product Data**

Open `src/app/product-list/product-list.component.ts` and add:

```typescript
products = [
  { name: 'Angular Book', price: 29.99, date: new Date() },
  { name: 'TypeScript Guide', price: 19.99, date: new Date() },
  { name: 'RxJS Manual', price: 24.99, date: new Date() }
];
```

### **Step 3: Use Directives and Pipes**

Open `src/app/product-list/product-list.component.html` and add:

```html
<ul>
  <li *ngFor="let product of products">
    <h3>{{ product.name }}</h3>
    <p>Price: {{ product.price | currency:'USD' }}</p>
    <p>Release Date: {{ product.date | date:'fullDate' }}</p>
  </li>
</ul>
```

---

## **4. Expert Insights and Best Practices:**
- Use `*ngIf` and `*ngFor` judiciously to optimize performance.
- Prefer `[ngStyle]` and `[ngClass]` over inline styling.
- Avoid complex logic in templates. Use custom pipes for transformations.

---

## **5. Common Mistakes to Avoid:**
- Forgetting to declare custom directives and pipes in the module.
- Using structural directives without understanding their impact on DOM rendering.
- Not using the `async` pipe with Observables, leading to memory leaks.

---

## **6. Next Lesson: Services and Dependency Injection**
- **Creating and Using Services**
- **Injecting Services using DI**
- **Sharing Data Between Components**
- **Real-World Examples and Exercises**

---
