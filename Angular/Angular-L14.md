### **Lesson 14: Security Best Practices in Angular**

---

## **What You Will Learn:**
1. **Introduction to Security in Angular**
   - Why Security is Critical in Angular Applications
   - Common Security Threats:
     - Cross-Site Scripting (XSS)
     - Cross-Site Request Forgery (CSRF)
     - Insecure API Calls
     - Authentication and Authorization Issues

2. **Cross-Site Scripting (XSS) Prevention**
   - What is XSS?
   - Types of XSS Attacks
   - Angular's Built-in XSS Protection
   - Best Practices to Prevent XSS

3. **Cross-Site Request Forgery (CSRF) Protection**
   - What is CSRF?
   - How CSRF Attacks Work
   - Implementing CSRF Protection in Angular

4. **HTTP Security Headers and Content Security Policy (CSP)**
   - Securing HTTP Headers
   - Implementing Content Security Policy (CSP)

5. **Authentication and Authorization**
   - Implementing JWT Authentication
   - Role-Based Authorization with Guards
   - Securing Routes with Angular Route Guards

6. **Advanced Security Techniques**
   - Sanitizing User Input with DomSanitizer
   - Using Angular's Bypass Security API Safely
   - Preventing Clickjacking Attacks

7. **Hands-on Exercises:**
   - Implementing JWT Authentication and Role-Based Authorization
   - Real-World Scenario: Securing an Admin Dashboard

8. **Expert Insights and Best Practices**
9. **Common Mistakes to Avoid**
10. **Recap and Next Steps**

---

## **1. Introduction to Security in Angular**

### **1.1 Why Security is Critical in Angular Applications**
- Protects user data and ensures data integrity.
- Maintains application trust and credibility.
- Prevents unauthorized access and data breaches.
- Complies with security standards (e.g., OWASP Top 10).

---

### **1.2 Common Security Threats**

1. **Cross-Site Scripting (XSS)**:
   - Occurs when malicious scripts are injected into trusted websites.
   - Impact: Stealing cookies, session tokens, or redirecting users to malicious sites.

2. **Cross-Site Request Forgery (CSRF)**:
   - Occurs when unauthorized commands are transmitted from a user trusted by the web application.
   - Impact: Performing actions on behalf of authenticated users without their consent.

3. **Insecure API Calls**:
   - Occurs when sensitive data is transmitted without encryption.
   - Impact: Data interception and manipulation.

4. **Authentication and Authorization Issues**:
   - Inadequate authentication or authorization checks.
   - Impact: Unauthorized access to protected resources.

---

## **2. Cross-Site Scripting (XSS) Prevention**

### **2.1 What is XSS?**
- **Cross-Site Scripting (XSS)** is a security vulnerability that allows attackers to inject malicious scripts into web applications.
- **Types of XSS Attacks**:
  - **Stored XSS**: Malicious script is stored on the server (e.g., in a database).
  - **Reflected XSS**: Script is reflected off a web server (e.g., via URL parameters).
  - **DOM-Based XSS**: Manipulates the DOM environment on the client side.

---

### **2.2 Angular's Built-in XSS Protection**
- **Angular uses Contextual Escaping** by default:
  - Automatically escapes HTML content.
  - Protects against most XSS attacks.
- **Interpolation and Property Binding** are safe:
  ```html
  <div>{{ userInput }}</div>
  <div [innerHTML]="userInput"></div>
  ```
- Angular sanitizes `innerHTML` and other risky properties.

---

### **2.3 Best Practices to Prevent XSS**

1. **Avoid Directly Binding to `innerHTML`**:
   - Avoid using `[innerHTML]` unless absolutely necessary.
   - Use Angular templates and bindings instead.

2. **Use Angular's DomSanitizer for Dynamic HTML**:
   ```typescript
   import { DomSanitizer, SafeHtml } from '@angular/platform-browser';

   constructor(private sanitizer: DomSanitizer) {}

   getSafeHtml(html: string): SafeHtml {
     return this.sanitizer.bypassSecurityTrustHtml(html);
   }
   ```

   **Usage in Template**:
   ```html
   <div [innerHTML]="getSafeHtml(userInput)"></div>
   ```

3. **Avoid Using `eval()` and `setTimeout()` with Strings**:
   - These functions can execute arbitrary scripts and are vulnerable to XSS.

4. **Sanitize User Input**:
   - Use libraries like **DOMPurify** to sanitize HTML input.

---

## **3. Cross-Site Request Forgery (CSRF) Protection**

### **3.1 What is CSRF?**
- **Cross-Site Request Forgery (CSRF)** is an attack that forces authenticated users to execute unwanted actions on a web application.
- Example:
  - A malicious website tricks users into submitting a request to another site where they are authenticated.

---

### **3.2 How CSRF Attacks Work**
1. **Victim Authentication**:
   - User logs into a trusted site and a session cookie is set.
2. **Malicious Request**:
   - User visits a malicious site while still authenticated.
   - The malicious site sends a request to the trusted site using the stored session cookie.
3. **Unauthorized Action**:
   - The trusted site executes the request, thinking it's from the authenticated user.

---

### **3.3 Implementing CSRF Protection in Angular**

1. **Use `XSRF-TOKEN` with HttpClient**

- Angular automatically adds the `X-XSRF-TOKEN` header to HTTP requests.

**Step 1: Configure `HttpClientModule`**

```typescript
import { HttpClientModule, HTTP_INTERCEPTORS } from '@angular/common/http';
import { HttpXsrfInterceptor, HttpXsrfTokenExtractor } from '@angular/common/http';

@NgModule({
  imports: [HttpClientModule],
  providers: [
    {
      provide: HTTP_INTERCEPTORS,
      useClass: HttpXsrfInterceptor,
      multi: true
    }
  ]
})
export class AppModule {}
```

**Step 2: Set XSRF Token in Backend**
- Ensure the backend sets a cookie named `XSRF-TOKEN`.

**Example: In Spring Boot Backend**
```java
http.csrf().csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse());
```

2. **Use SameSite Cookies**

- Set `SameSite` attribute for cookies to `Strict` or `Lax` to prevent cross-site requests.

**Example: In Spring Boot Backend**
```java
http
  .csrf().csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse())
  .and()
  .headers().frameOptions().sameOrigin();
```

---

## **4. HTTP Security Headers and Content Security Policy (CSP)**

### **4.1 Securing HTTP Headers**

1. **Content Security Policy (CSP)**:
   - Controls sources of content (scripts, styles, images).
   - Prevents XSS attacks by restricting external scripts.

**Example: CSP Header in Angular**

```html
<meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self'">
```

2. **HTTP Strict Transport Security (HSTS)**:
   - Forces HTTPS connections.
   - Example:
     ```http
     Strict-Transport-Security: max-age=31536000; includeSubDomains
     ```

3. **X-Content-Type-Options**:
   - Prevents MIME type sniffing.
   - Example:
     ```http
     X-Content-Type-Options: nosniff
     ```

---

## **5. Authentication and Authorization**

### **5.1 Implementing JWT Authentication**

1. **Store JWT Securely**
   - **DO NOT** store JWT in Local Storage (vulnerable to XSS).
   - Store in **HttpOnly Cookies** or **Memory Storage**.

2. **Send JWT in HTTP Headers**

```typescript
const headers = new HttpHeaders().set('Authorization', `Bearer ${token}`);
this.http.get('https://api.example.com/protected', { headers });
```

---

### **5.2 Role-Based Authorization with Guards**

1. **Create AuthGuard**

```typescript
import { Injectable } from '@angular/core';
import { CanActivate, Router } from '@angular/router';
import { AuthService } from './auth.service';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  constructor(private authService: AuthService, private router: Router) {}

  canActivate(): boolean {
    if (this.authService.isAuthenticated()) {
      return true;
    }
    this.router.navigate(['/login']);
    return false;
  }
}
```

2. **Protect Routes**

```typescript
{ path: 'admin', component: AdminComponent, canActivate: [AuthGuard] }
```

---

## **Next Lesson: Internationalization (i18n) in Angular**
- **Introduction to Internationalization (i18n) and Localization (l10n)**
- **Setting up Angular i18n**
- **Translating Static and Dynamic Content**
- **Handling Date, Currency, and Number Formats**
- **Lazy Loading Translations**
