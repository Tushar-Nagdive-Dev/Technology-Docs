### **Lesson 60: Advanced Lazy Loading Patterns**

---

## **What You Will Learn:**
1. **Lazy Loading with Intersection Observer API**
   - What is Intersection Observer API?
   - Why Use Intersection Observer for Lazy Loading?
   - Implementing Lazy Loading with Intersection Observer
   - Performance Optimization with Intersection Observer

2. **Lazy Loading Images and Media Content**
   - Why Lazy Load Images and Media?
   - Using Intersection Observer for Lazy Loading Images
   - Lazy Loading Videos and Background Images
   - Performance Gains with Media Lazy Loading

3. **Lazy Loading Child Components with ViewContainerRef**
   - What is ViewContainerRef in Angular?
   - Dynamically Loading Child Components with ViewContainerRef
   - Destroying Dynamically Loaded Components
   - Advanced Component Lazy Loading Patterns

4. **Dynamic Component Loading with ngTemplateOutlet**
   - What is ngTemplateOutlet?
   - Dynamic Template Rendering with ngTemplateOutlet
   - Conditional Component Loading with ngTemplateOutlet
   - Performance Optimization with Dynamic Templates

5. **Lazy Loading and Asynchronous Data**
   - Why Use Lazy Loading with Asynchronous Data?
   - Optimizing Asynchronous Data with ngIf and async Pipe
   - Combining Lazy Loading with RxJS and Observables
   - Efficient Change Detection with Lazy Loaded Data

6. **Hands-on Exercises:**
   - Implementing Lazy Loading with Intersection Observer
   - Dynamic Component Loading with ViewContainerRef
   - Real-World Scenario: Lazy Loading Complex Templates

7. **Expert Insights and Best Practices**
8. **Common Mistakes to Avoid**
9. **Recap and Next Steps**

---

## **1. Lazy Loading with Intersection Observer API**

### **1.1 What is Intersection Observer API?**
- **Intersection Observer API** is a **browser API** that **tracks the visibility of an element** within the viewport.
- **Why Use Intersection Observer?**:
  - **Efficiently detects when an element is visible** on the screen.
  - **Triggers lazy loading** only when the element **enters the viewport**.
  - **Optimizes performance** by **loading content only when needed**.

- **Use Cases**:
  - **Lazy loading images, videos, or components**.
  - **Infinite scrolling** and **virtualized lists**.
  - **Performance optimization** for **large lists and complex templates**.

---

### **1.2 Why Use Intersection Observer for Lazy Loading?**
- **Intersection Observer** is **ideal for lazy loading** because:
  - **Detects visibility** without affecting **main thread performance**.
  - **Efficiently tracks multiple elements** without **event listeners**.
  - **Works asynchronously**, **improving rendering performance**.

- **Performance Gains**:
  - **Reduces initial load time** by **loading content on demand**.
  - **Prevents unnecessary change detection** for **offscreen elements**.
  - **Improves scrolling performance** by **lazy loading content**.

---

### **1.3 Implementing Lazy Loading with Intersection Observer**

**Example: Lazy Loading Images with Intersection Observer**

```typescript
// lazy-image.directive.ts
import { Directive, ElementRef, Input, OnInit } from '@angular/core';

@Directive({
  selector: '[appLazyImage]'
})
export class LazyImageDirective implements OnInit {
  @Input('appLazyImage') src: string;

  constructor(private el: ElementRef) {}

  ngOnInit() {
    const observer = new IntersectionObserver((entries, observer) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          const img = this.el.nativeElement;
          img.src = this.src;
          observer.unobserve(img);
        }
      });
    });

    observer.observe(this.el.nativeElement);
  }
}
```

```html
<!-- app.component.html -->
<h2>Lazy Loading Images with Intersection Observer</h2>
<img appLazyImage="https://example.com/image1.jpg" alt="Image 1">
<img appLazyImage="https://example.com/image2.jpg" alt="Image 2">
<img appLazyImage="https://example.com/image3.jpg" alt="Image 3">
```

- **Explanation**:
  - **Custom Directive (`appLazyImage`)** is created for **lazy loading images**.
  - **Intersection Observer** **loads the image** when it **enters the viewport**.
  - **Image is not loaded** until it is **visible on the screen**.
  - **Performance optimization** by **reducing initial load time**.

---

### **1.4 Performance Optimization with Intersection Observer**
- **Intersection Observer** optimizes performance by:
  - **Loading content only when visible**.
  - **Improving initial load time** by **delaying offscreen content**.
  - **Reducing memory usage** with **lazy-loaded media**.
  - **Enhancing user experience** with **smooth scrolling**.

---

## **2. Lazy Loading Images and Media Content**

### **2.1 Why Lazy Load Images and Media?**
- **Images and media content** are **resource-intensive** and **impact performance**.
- **Lazy loading images and media**:
  - **Reduces initial load time** by **loading only visible media**.
  - **Improves rendering performance** with **on-demand loading**.
  - **Enhances user experience** with **faster interactions**.

---

### **2.2 Using Intersection Observer for Lazy Loading Images**

**Example: Lazy Loading Background Images**

```typescript
// lazy-bg-image.directive.ts
import { Directive, ElementRef, Input, OnInit } from '@angular/core';

@Directive({
  selector: '[appLazyBgImage]'
})
export class LazyBgImageDirective implements OnInit {
  @Input('appLazyBgImage') imageUrl: string;

  constructor(private el: ElementRef) {}

  ngOnInit() {
    const observer = new IntersectionObserver((entries, observer) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          this.el.nativeElement.style.backgroundImage = `url(${this.imageUrl})`;
          observer.unobserve(this.el.nativeElement);
        }
      });
    });

    observer.observe(this.el.nativeElement);
  }
}
```

```html
<!-- app.component.html -->
<h2>Lazy Loading Background Images</h2>
<div appLazyBgImage="https://example.com/bg-image1.jpg" class="bg-container">
  Content over Background Image 1
</div>
<div appLazyBgImage="https://example.com/bg-image2.jpg" class="bg-container">
  Content over Background Image 2
</div>
```

```css
/* styles.css */
.bg-container {
  height: 300px;
  background-size: cover;
  background-position: center;
  margin-bottom: 20px;
}
```

- **Explanation**:
  - **Custom Directive (`appLazyBgImage`)** is created for **lazy loading background images**.
  - **Intersection Observer** **sets background image** when the element **enters the viewport**.
  - **Background image is not loaded** until it is **visible on the screen**.
  - **Performance optimization** by **reducing initial load time**.

---

### **2.3 Lazy Loading Videos and Media Content**
- **Lazy Loading Videos**:
  - **Reduces initial load time** by **loading video content on demand**.
  - **Improves rendering speed** with **conditional rendering**.

**Example: Lazy Loading Videos with Intersection Observer**

```html
<video controls appLazyVideo="https://example.com/video1.mp4" width="600">
  Your browser does not support the video tag.
</video>
```

- **Explanation**:
  - **Intersection Observer** is used to **load videos on demand**.
  - **Video file is not loaded** until the **video element is visible**.
  - **Improves performance** by **lazy loading media content**.

---

## **Next Steps:**
- **Lazy Loading Child Components with ViewContainerRef**
  - Dynamically Loading Child Components
  - Destroying Dynamically Loaded Components

- **Dynamic Component Loading with ngTemplateOutlet**
  - Dynamic Template Rendering
  - Conditional Component Loading
