### **Lesson 2: Data Binding and Event Binding in Angular**

---

## **What You Will Learn:**
1. **Understanding Data Binding in Angular**
   - Interpolation
   - Property Binding
   - Event Binding
   - Two-Way Data Binding

2. **Hands-on Exercises:**
   - Implementing Data Binding in a `ProductComponent`
   - Real-World Scenario: Building a Simple Shopping Cart UI

3. **Expert Insights and Best Practices**
4. **Common Mistakes to Avoid**
5. **Recap and Next Steps**

---

## **1. Understanding Data Binding in Angular**

Data Binding in Angular connects the template (view) with the component’s class (model). This communication flow is essential for dynamic and interactive user interfaces.

Angular supports **four types of data binding**:

1. **Interpolation (One-Way Binding from Component to View)**
2. **Property Binding (One-Way Binding from Component to View)**
3. **Event Binding (One-Way Binding from View to Component)**
4. **Two-Way Data Binding (Two-Way Binding between Component and View)**

---

### **1.1 Interpolation**

- **Syntax**: `{{ expression }}`
- **Purpose**: Display data from the component class in the template.
- **Example**:
  ```html
  <h2>{{ productName }}</h2>
  <p>Price: ${{ price }}</p>
  ```
- **Component Class**:
  ```typescript
  export class ProductComponent {
    productName = 'Angular Book';
    price = 25;
  }
  ```

---

### **1.2 Property Binding**

- **Syntax**: `[property]="expression"`
- **Purpose**: Bind the value of an HTML element property to a data property in the component class.
- **Example**:
  ```html
  <img [src]="productImage" alt="Product Image">
  <button [disabled]="isOutOfStock">Buy Now</button>
  ```
- **Component Class**:
  ```typescript
  export class ProductComponent {
    productImage = 'assets/images/angular-book.jpg';
    isOutOfStock = false;
  }
  ```

---

### **1.3 Event Binding**

- **Syntax**: `(event)="expression"`
- **Purpose**: Bind an event from the template (view) to a method in the component class.
- **Example**:
  ```html
  <button (click)="addToCart()">Add to Cart</button>
  ```
- **Component Class**:
  ```typescript
  export class ProductComponent {
    addToCart() {
      console.log('Product added to cart');
    }
  }
  ```

---

### **1.4 Two-Way Data Binding**

- **Syntax**: `[(ngModel)]="dataProperty"`
- **Purpose**: Bind the value of an input element to a data property in the component class, updating both the view and the model simultaneously.
- **Example**:
  ```html
  <input [(ngModel)]="quantity" type="number">
  <p>Selected Quantity: {{ quantity }}</p>
  ```
- **Component Class**:
  ```typescript
  export class ProductComponent {
    quantity = 1;
  }
  ```
- **Important Note**: To use `ngModel`, import `FormsModule` in your module:
  ```typescript
  import { FormsModule } from '@angular/forms';

  @NgModule({
    imports: [FormsModule]
  })
  export class AppModule {}
  ```

---

## **2. Hands-on Exercise: Implementing Data Binding**

Let's create a `ProductComponent` that demonstrates all types of data binding in a real-world scenario.

### **Step 1: Generate Product Component**

```bash
ng generate component product
```

This creates:
- `product.component.ts`
- `product.component.html`
- `product.component.scss`

---

### **Step 2: Define Product Data in Component Class**

Open `src/app/product/product.component.ts` and update it as follows:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-product',
  templateUrl: './product.component.html',
  styleUrls: ['./product.component.scss']
})
export class ProductComponent {
  productName = 'Angular Mastery Book';
  productPrice = 29.99;
  productImage = 'assets/images/angular-book.jpg';
  isOutOfStock = false;
  quantity = 1;
  cartMessage = '';

  addToCart() {
    this.cartMessage = `${this.productName} added to cart. Quantity: ${this.quantity}`;
  }
}
```

---

### **Step 3: Create Product Template**

Open `src/app/product/product.component.html` and add the following:

```html
<div class="product-card">
  <img [src]="productImage" alt="{{ productName }}">
  <h2>{{ productName }}</h2>
  <p>Price: ${{ productPrice }}</p>

  <input [(ngModel)]="quantity" type="number" min="1">
  <button (click)="addToCart()" [disabled]="isOutOfStock">Add to Cart</button>

  <p class="cart-message">{{ cartMessage }}</p>
</div>
```

---

### **Step 4: Style Product Card**

Open `src/app/product/product.component.scss` and add the following:

```scss
.product-card {
  border: 1px solid #ccc;
  padding: 20px;
  border-radius: 8px;
  text-align: center;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  width: 300px;
  margin: auto;
}

.product-card img {
  max-width: 100%;
  border-radius: 4px;
}

.product-card button {
  background-color: #3f51b5;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
}

.product-card button:disabled {
  background-color: #ccc;
}

.cart-message {
  color: green;
  margin-top: 10px;
}
```

---

### **Step 5: Display Product Component**

Open `src/app/app.component.html` and add:
```html
<app-product></app-product>
```

---

### **Step 6: Serve the Application**

```bash
ng serve
```

Open the browser at `http://localhost:4200` to see the **Product Component** in action.

- Adjust the quantity using the input box.
- Click "Add to Cart" to see the dynamic message.
- Observe how the cart message updates using **Two-Way Data Binding**.

---

## **3. Real-World Scenario: Shopping Cart UI**

You have just created a simple shopping cart UI demonstrating:
- **Interpolation**: Displaying product name and price.
- **Property Binding**: Image source and button state.
- **Event Binding**: Add to cart button click event.
- **Two-Way Data Binding**: Quantity input.

This exercise showcases how Angular's data binding features are used to create dynamic and interactive user interfaces.

---

## **4. Expert Insights and Best Practices:**
- **Use Interpolation** for text content and Property Binding for DOM properties.
- **Event Binding** should be used for user interactions like clicks, hover, keypress, etc.
- **Two-Way Data Binding** is ideal for form inputs and requires `FormsModule`.
- Keep data binding expressions simple and avoid complex logic in templates.

---

## **5. Common Mistakes to Avoid:**
- Using `{{}}` inside property bindings (`[property]`) — they are not allowed.
- Not importing `FormsModule` for `[(ngModel)]`.
- Using two-way binding unnecessarily — use it only when the data flow is bi-directional.
- Using **Event Binding** without a corresponding method in the component class.

---

## **6. Recap and Next Steps:**
You have learned:
- **Four types of data binding**: Interpolation, Property Binding, Event Binding, and Two-Way Data Binding.
- How to implement data binding using a **real-world Product Component**.
- Best practices and common mistakes to avoid.

### **Next Lesson: Directives and Pipes**
- **Structural Directives** (`*ngIf`, `*ngFor`)
- **Attribute Directives** (`[ngStyle]`, `[ngClass]`)
- **Custom Directives and Pipes**
- Real-world examples and exercises

---
