### **Lesson 11: Angular Animations**

---

## **What You Will Learn:**
1. **Introduction to Angular Animations**
   - What are Angular Animations?
   - Why Use Angular Animations?
   - Animation Architecture in Angular

2. **Setting up Angular Animations**
   - Installing and Importing Angular Animations Module
   - Enabling Animations in Angular Application

3. **Triggering Animations in Angular**
   - Using the `@trigger` and `@animate` Decorators
   - Animation States and Transitions
   - Animating Styles and Keyframes

4. **Advanced Animation Techniques**
   - Sequence and Group Animations
   - Animation Callbacks and Events
   - Animation Querying and Staggering

5. **Hands-on Exercises:**
   - Creating a Fade-In/Fade-Out Animation
   - Real-World Scenario: Slide-In Navigation Menu

6. **Expert Insights and Best Practices**
7. **Common Mistakes to Avoid**
8. **Recap and Next Steps**

---

## **1. Introduction to Angular Animations**

### **1.1 What are Angular Animations?**
- **Angular Animations** is a module that allows you to add dynamic transitions and animations to your Angular applications.
- Built on top of the **Web Animations API**, it provides a powerful way to animate components, views, and elements.

### **1.2 Why Use Angular Animations?**
- **Enhance User Experience**: Smooth transitions improve user interactions.
- **Guide User Attention**: Direct users' attention to key elements.
- **Visual Feedback**: Provide feedback for actions like button clicks or form submissions.
- **Professional and Modern UI**: Create polished and engaging user interfaces.

---

### **1.3 Animation Architecture in Angular**

Angular Animations are built using:
- **Triggers**: Named animations that can be bound to a component property.
- **States**: Named conditions defined in a trigger.
- **Transitions**: Rules for switching between states.
- **Animations**: Define the style changes and timing for transitions.

---

## **2. Setting up Angular Animations**

### **2.1 Installing Angular Animations**

Angular Animations are part of the `@angular/animations` package.

```bash
ng add @angular/animations
```

### **2.2 Importing BrowserAnimationsModule**

Open `src/app/app.module.ts` and add:

```typescript
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';

@NgModule({
  imports: [BrowserAnimationsModule]
})
export class AppModule {}
```

This enables Angular's animation capabilities throughout the application.

---

## **3. Triggering Animations in Angular**

### **3.1 Defining Animation Triggers**

Animation triggers are defined using the `trigger()` function.

**Example: Fade In/Out Animation**

```typescript
import { Component } from '@angular/core';
import { trigger, state, style, transition, animate } from '@angular/animations';

@Component({
  selector: 'app-fade',
  template: `
    <button (click)="toggle()">Toggle Fade</button>
    <div *ngIf="isVisible" @fadeAnimation class="fade-box">
      Fading Box
    </div>
  `,
  styles: [`
    .fade-box {
      width: 200px;
      height: 100px;
      background-color: #3f51b5;
      color: white;
      text-align: center;
      line-height: 100px;
      margin-top: 20px;
    }
  `],
  animations: [
    trigger('fadeAnimation', [
      state('void', style({ opacity: 0 })),
      state('*', style({ opacity: 1 })),
      transition(':enter', [ animate('500ms ease-in') ]),
      transition(':leave', [ animate('500ms ease-out') ])
    ])
  ]
})
export class FadeComponent {
  isVisible = false;

  toggle() {
    this.isVisible = !this.isVisible;
  }
}
```

**Explanation:**
- **state('void')**: Represents the element when it is not in the DOM.
- **:enter** and **:leave**: Special states for entering and leaving the DOM.
- **transition**: Defines the timing and style of the animation.
- **animate**: Specifies the duration and easing of the animation.

---

### **3.2 Animation States and Transitions**

Animation states allow you to define different visual states for a component.

**Example: Open/Close Animation**

```typescript
trigger('openClose', [
  state('open', style({
    height: '200px',
    opacity: 1,
    backgroundColor: 'lightgreen'
  })),
  state('closed', style({
    height: '100px',
    opacity: 0.5,
    backgroundColor: 'lightcoral'
  })),
  transition('open <=> closed', [
    animate('0.5s ease-in-out')
  ])
])
```

**Usage in Template:**
```html
<div [@openClose]="isOpen ? 'open' : 'closed'" class="box">
  Toggle Box
</div>
```

**Component Class:**
```typescript
isOpen = true;

toggle() {
  this.isOpen = !this.isOpen;
}
```

---

### **3.3 Animating Styles and Keyframes**

**Example: Keyframe Animation**

```typescript
trigger('bounce', [
  transition('* => *', [
    animate('1s', keyframes([
      style({ transform: 'translateY(0)', offset: 0 }),
      style({ transform: 'translateY(-30px)', offset: 0.5 }),
      style({ transform: 'translateY(0)', offset: 1.0 })
    ]))
  ])
])
```

**Usage in Template:**
```html
<div @bounce class="bounce-box">
  Bounce Animation
</div>
```

---

## **4. Advanced Animation Techniques**

### **4.1 Sequence and Group Animations**

- **sequence()**: Executes animations one after another.
- **group()**: Executes animations simultaneously.

**Example: Sequence Animation**

```typescript
trigger('sequenceAnimation', [
  transition('* => *', [
    sequence([
      animate('1s', style({ opacity: 0 })),
      animate('1s', style({ opacity: 1 }))
    ])
  ])
])
```

**Example: Group Animation**

```typescript
trigger('groupAnimation', [
  transition('* => *', [
    group([
      animate('1s', style({ opacity: 0 })),
      animate('2s', style({ transform: 'scale(1.2)' }))
    ])
  ])
])
```

---

### **4.2 Animation Callbacks and Events**

Angular animations provide hooks for animation lifecycle events:
- `(@triggerName.start)` – Fired when the animation starts.
- `(@triggerName.done)` – Fired when the animation completes.

**Example: Using Animation Callbacks**

```html
<div @fadeAnimation (@fadeAnimation.start)="onStart()" (@fadeAnimation.done)="onDone()">
  Animated Box
</div>
```

**Component Class:**
```typescript
onStart() {
  console.log('Animation Started');
}

onDone() {
  console.log('Animation Completed');
}
```

---

### **4.3 Animation Querying and Staggering**

- **query()**: Targets specific elements within a container.
- **stagger()**: Adds delays to animations for multiple elements.

**Example: Staggered List Animation**

```typescript
trigger('listAnimation', [
  transition('* => *', [
    query(':enter', [
      style({ opacity: 0, transform: 'translateY(-50px)' }),
      stagger(100, [
        animate('500ms', style({ opacity: 1, transform: 'translateY(0)' }))
      ])
    ], { optional: true })
  ])
])
```

**Usage in Template:**
```html
<ul [@listAnimation]>
  <li *ngFor="let item of items">{{ item }}</li>
</ul>
```

---

## **5. Expert Insights and Best Practices:**
- Use **animations** sparingly for better performance.
- Prefer **CSS animations** for simple transitions (e.g., hover effects).
- Use **Angular Animations** for complex sequences and dynamic transitions.
- Optimize performance by using **OnPush change detection**.

---

## **6. Common Mistakes to Avoid:**
- Not importing `BrowserAnimationsModule`.
- Applying animations directly in CSS instead of using Angular triggers.
- Not handling animation callbacks, leading to unexpected UI behavior.
- Using long durations or complex easing, impacting performance.

---

## **Next Lesson: Unit Testing and End-to-End Testing in Angular**
- **Introduction to Unit Testing (Jasmine and Karma)**
- **Writing Test Cases for Components, Services, and Directives**
- **End-to-End Testing with Protractor and Cypress**
- **Mocking HTTP Requests and Dependency Injection in Tests**
- **Code Coverage and Best Practices**
