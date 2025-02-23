### **Lesson 6: Forms in Angular**

---

## **What You Will Learn:**
1. **Understanding Forms in Angular**
   - Types of Forms in Angular
     - Template-Driven Forms
     - Reactive Forms
   - When to Use Which Form Type

2. **Template-Driven Forms**
   - Creating a Simple Template-Driven Form
   - Two-Way Data Binding with `ngModel`
   - Form Validation and Error Handling
   - Hands-on Exercise: Creating a Contact Form

3. **Reactive Forms**
   - Creating a Reactive Form
   - FormGroup and FormControl
   - FormBuilder for Efficient Form Creation
   - Form Validation and Custom Validators
   - Dynamic Forms with FormArray
   - Hands-on Exercise: Creating a Registration Form

4. **Real-World Scenario: Building a Complete Checkout Form**

5. **Expert Insights and Best Practices**
6. **Common Mistakes to Avoid**
7. **Recap and Next Steps**

---

## **1. Understanding Forms in Angular**

Forms are fundamental in web applications for data collection and user interactions. Angular provides two ways to work with forms:

1. **Template-Driven Forms:**
   - **Declarative Approach**: Uses Angular directives in the template.
   - Best for **simple forms** with minimal logic.
   - Uses `ngModel` for two-way data binding.

2. **Reactive Forms:**
   - **Programmatic Approach**: Form is created and managed in the component class.
   - Best for **complex forms** with dynamic validation logic.
   - Uses `FormGroup`, `FormControl`, and `FormBuilder`.

---

## **2. Template-Driven Forms**

### **2.1 Creating a Simple Template-Driven Form**

**Step 1: Import FormsModule**

Open `src/app/app.module.ts` and add:

```typescript
import { FormsModule } from '@angular/forms';

@NgModule({
  imports: [FormsModule]
})
export class AppModule {}
```

**Step 2: Create Contact Component**

```bash
ng generate component contact
```

**Step 3: Define Contact Model**

Open `src/app/contact/contact.component.ts` and add:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-contact',
  templateUrl: './contact.component.html',
  styleUrls: ['./contact.component.scss']
})
export class ContactComponent {
  contact = {
    name: '',
    email: '',
    message: ''
  };

  submitForm() {
    console.log('Form Submitted', this.contact);
  }
}
```

**Step 4: Create Template for Form**

Open `src/app/contact/contact.component.html` and add:

```html
<form (ngSubmit)="submitForm()" #contactForm="ngForm">
  <div>
    <label>Name:</label>
    <input type="text" name="name" [(ngModel)]="contact.name" required>
  </div>
  <div>
    <label>Email:</label>
    <input type="email" name="email" [(ngModel)]="contact.email" required>
  </div>
  <div>
    <label>Message:</label>
    <textarea name="message" [(ngModel)]="contact.message" required></textarea>
  </div>
  <button type="submit" [disabled]="contactForm.invalid">Submit</button>
</form>
```

---

### **2.2 Two-Way Data Binding with `ngModel`**

- `[(ngModel)]` binds input values to the component's model.
- It provides two-way data binding for template-driven forms.

**Example:**
```html
<input type="text" name="name" [(ngModel)]="contact.name" required>
```

- This binds `contact.name` from the component class to the input field.
- Any change in the input is reflected in the component model and vice versa.

---

### **2.3 Form Validation and Error Handling**

Angular provides built-in validators like `required`, `minlength`, `maxlength`, `pattern`, etc.

**Example: Email Validation**
```html
<input type="email" name="email" [(ngModel)]="contact.email" required email>
<span *ngIf="contactForm.controls.email?.invalid && contactForm.controls.email?.touched">
  Invalid Email
</span>
```

**Adding Error Messages:**
```html
<div *ngIf="contactForm.controls.name?.invalid && contactForm.controls.name?.touched">
  <small class="error" *ngIf="contactForm.controls.name?.errors?.required">Name is required</small>
</div>
```

---

### **2.4 Hands-on Exercise: Creating a Contact Form**

1. **Create a Contact Form Component**
2. **Implement Two-Way Data Binding using `ngModel`**
3. **Add Form Validation and Error Messages**
4. **Submit Form and Display Data in Console**

---

## **3. Reactive Forms**

### **3.1 Creating a Reactive Form**

**Step 1: Import ReactiveFormsModule**

Open `src/app/app.module.ts` and add:

```typescript
import { ReactiveFormsModule } from '@angular/forms';

@NgModule({
  imports: [ReactiveFormsModule]
})
export class AppModule {}
```

**Step 2: Create Registration Component**

```bash
ng generate component registration
```

**Step 3: Create Form in Component Class**

Open `src/app/registration/registration.component.ts` and add:

```typescript
import { Component, OnInit } from '@angular/core';
import { FormGroup, FormControl, Validators } from '@angular/forms';

@Component({
  selector: 'app-registration',
  templateUrl: './registration.component.html',
  styleUrls: ['./registration.component.scss']
})
export class RegistrationComponent implements OnInit {
  registrationForm: FormGroup;

  ngOnInit(): void {
    this.registrationForm = new FormGroup({
      name: new FormControl('', [Validators.required, Validators.minLength(3)]),
      email: new FormControl('', [Validators.required, Validators.email]),
      password: new FormControl('', [Validators.required, Validators.minLength(6)])
    });
  }

  onSubmit() {
    console.log('Form Submitted', this.registrationForm.value);
  }
}
```

---

### **3.2 FormGroup and FormControl**

- **FormGroup**: Tracks the value and validity state of a group of FormControl instances.
- **FormControl**: Tracks the value and validation status of an individual form control.

---

### **3.3 Template for Reactive Form**

Open `src/app/registration/registration.component.html` and add:

```html
<form [formGroup]="registrationForm" (ngSubmit)="onSubmit()">
  <div>
    <label>Name:</label>
    <input type="text" formControlName="name">
    <div *ngIf="registrationForm.controls.name.invalid && registrationForm.controls.name.touched">
      <small class="error" *ngIf="registrationForm.controls.name.errors.required">Name is required</small>
    </div>
  </div>
  <div>
    <label>Email:</label>
    <input type="email" formControlName="email">
  </div>
  <div>
    <label>Password:</label>
    <input type="password" formControlName="password">
  </div>
  <button type="submit" [disabled]="registrationForm.invalid">Register</button>
</form>
```

---

### **3.4 FormBuilder for Efficient Form Creation**

Using **FormBuilder** simplifies form creation:

```typescript
import { FormBuilder } from '@angular/forms';

constructor(private fb: FormBuilder) {}

ngOnInit(): void {
  this.registrationForm = this.fb.group({
    name: ['', [Validators.required, Validators.minLength(3)]],
    email: ['', [Validators.required, Validators.email]],
    password: ['', [Validators.required, Validators.minLength(6)]]
  });
}
```

---

### **3.5 Dynamic Forms with FormArray**

**FormArray** is used to dynamically add or remove form controls.

**Example:**
```typescript
skills = this.fb.array([this.fb.control('')]);

addSkill() {
  this.skills.push(this.fb.control(''));
}
```

**Template:**
```html
<div formArrayName="skills">
  <div *ngFor="let skill of skills.controls; let i=index">
    <input [formControlName]="i">
    <button (click)="removeSkill(i)">Remove</button>
  </div>
  <button (click)="addSkill()">Add Skill</button>
</div>
```

---

## **4. Expert Insights and Best Practices:**
- Use **Template-Driven Forms** for simple forms and **Reactive Forms** for complex scenarios.
- Prefer **Reactive Forms** for dynamic form validation and model-driven form control.
- Keep **HTML templates clean** by handling complex validation logic in the component class.

---

## **5. Common Mistakes to Avoid:**
- Not importing `FormsModule` or `ReactiveFormsModule`.
- Using `ngModel` with Reactive Forms (not compatible).
- Not unsubscribing from value changes in Reactive Forms.

---

## **Next Lesson: HTTP Client and RESTful Services**
- **Making HTTP Requests**
- **GET, POST, PUT, DELETE Methods**
- **Handling Observables and Promises**
- **Interceptors for Request and Response Handling**
