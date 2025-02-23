---

## **Lesson 73: JWT Authentication and Authorization with Spring Boot and Angular**

---

## **What You Will Learn:**
1. **Introduction to JWT Authentication and Authorization**
   - What is JWT (JSON Web Token)?
   - Why Use JWT for Authentication and Authorization?
   - Understanding JWT Structure and Workflow

2. **Setting up Spring Boot for JWT Authentication**
   - Configuring Spring Security for JWT Authentication
   - Creating UserDetailsService for User Authentication
   - Implementing AuthenticationController for Login and Token Generation
   - Implementing Authorization Filter for Secured Endpoints

3. **Creating JWT Utility Class in Spring Boot**
   - Generating JWT Token
   - Validating JWT Token
   - Extracting User Details from JWT Token

4. **Integrating Angular with JWT Authentication**
   - Creating Authentication Service in Angular
   - Storing JWT Token in Local Storage
   - Sending JWT Token in HTTP Headers
   - Securing Routes with AuthGuard

5. **Hands-on Exercise: Implementing Full Authentication and Authorization Flow**
   - Spring Boot Backend with JWT Authentication and Authorization
   - Angular Frontend with Login, Logout, and Role-Based Authorization
   - Testing Secure Endpoints with Postman and Angular UI

---

## **1. Introduction to JWT Authentication and Authorization**

---

### **1.1 What is JWT (JSON Web Token)?**
- **JWT (JSON Web Token)** is an open standard (RFC 7519) for securely transmitting information between parties as a JSON object.
- It is **digitally signed** using either:
  - **HMAC (symmetric key)** or
  - **RSA/ECDSA (asymmetric key pair)**.
- Ensures **data integrity** and **authenticity**.

---

### **1.2 Why Use JWT for Authentication and Authorization?**
- **Stateless Authentication**:
  - Server does not store session data.
  - Token is sent with every HTTP request, allowing scalability and easier stateless API design.
- **Cross-Domain Authentication**:
  - Works across different domains, making it ideal for **SPA (Single Page Applications)** like Angular.
- **Security and Flexibility**:
  - Digitally signed, preventing data tampering.
  - Customizable payload for user roles and permissions.

---

### **1.3 Understanding JWT Structure and Workflow**
- **JWT Structure**:
  - **Header**: Contains the algorithm used for signing (e.g., HS256, RS256).
  - **Payload**: Contains user claims, roles, and expiration time.
  - **Signature**: Ensures the token's integrity and authenticity.

- **Example of JWT Structure**:
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.          // Header
eyJzdWIiOiJqb2huZG9lIiwicm9sZXMiOlsiQURNSU4iXSwiaWF0IjoxNjE2MjM5MDIyfQ.     // Payload
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c   // Signature
```

---

### **1.4 JWT Authentication and Authorization Workflow**
1. **User Login**:
   - User sends **login credentials** (username and password) to the Spring Boot backend.
2. **Authentication and Token Generation**:
   - Spring Boot **authenticates the user**.
   - If valid, a **JWT token** is generated and sent back to Angular.
3. **Storing JWT Token**:
   - Angular **stores the token** in **Local Storage** or **Session Storage**.
4. **Sending JWT Token in Requests**:
   - Angular **attaches the token** to the **Authorization header** of each request.
5. **Authorization and Secured Endpoints**:
   - Spring Boot **validates the token** and **authorizes** access to secured endpoints.
   - If the token is invalid or expired, a **401 Unauthorized** response is returned.

---

## **2. Setting up Spring Boot for JWT Authentication**

---

### **2.1 Configuring Spring Security for JWT Authentication**

---

#### **Add Dependencies in pom.xml (Maven)**

```xml
<dependencies>
    <!-- Spring Security for authentication and authorization -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>

    <!-- Spring Web for RESTful APIs -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Spring Data JPA for database access -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- JWT Library for token generation and validation -->
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt</artifactId>
        <version>0.9.1</version>
    </dependency>

    <!-- MySQL Driver -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
    </dependency>

    <!-- Lombok for reducing boilerplate code -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>
```

- **spring-boot-starter-security** – For authentication and authorization.
- **spring-boot-starter-web** – For building RESTful APIs.
- **spring-boot-starter-data-jpa** – For database operations with JPA.
- **jjwt** – Java JWT library for token creation and validation.

---

### **2.2 Creating UserDetailsService for User Authentication**

- **UserDetailsService** is a core interface in Spring Security for loading user-specific data.
- We will implement **UserDetailsService** to:
  - Load users from the database.
  - Verify username and password.
  - Fetch user roles for authorization.

---

**Example: CustomUserDetailsService**

```java
package com.example.demo.security;

import com.example.demo.model.User;
import com.example.demo.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

@Service
public class CustomUserDetailsService implements UserDetailsService {

    @Autowired
    private UserRepository userRepository;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        User user = userRepository.findByUsername(username)
            .orElseThrow(() -> new UsernameNotFoundException("User not found with username: " + username));
        
        return new org.springframework.security.core.userdetails.User(
            user.getUsername(),
            user.getPassword(),
            user.getRoles()
        );
    }
}
```

- **UserDetailsService** – Loads user-specific data.
- **loadUserByUsername()** – Fetches user from the database.
- **UsernameNotFoundException** – Thrown if the user is not found.
- **org.springframework.security.core.userdetails.User** – Adapts our `User` entity to Spring Security's UserDetails implementation.

---

## **Next Steps:**
- **2.3 Implementing AuthenticationController for Login and Token Generation**
- **2.4 Implementing Authorization Filter for Secured Endpoints**
- **3. Creating JWT Utility Class in Spring Boot**
  - **Generating JWT Token**
  - **Validating JWT Token**
  - **Extracting User Details from JWT Token**

---

## **Lesson 73 (Continued): Implementing Authentication and Authorization with JWT**

---

## **2.3 Implementing AuthenticationController for Login and Token Generation**

In this section, we will:
- Create an **AuthenticationController** to:
  - **Authenticate users** and **generate JWT tokens**.
  - **Return JWT tokens** to the Angular frontend.
- Use **UsernamePasswordAuthenticationToken** for authentication.
- Integrate with **CustomUserDetailsService** for user verification.
- **Sign JWT tokens** using **HS256 (HMAC with SHA-256)** algorithm.

---

### **2.3.1 Creating AuthenticationController**

- **AuthenticationController** is responsible for:
  - Handling **login requests** from the Angular frontend.
  - Authenticating users using **Spring Security's AuthenticationManager**.
  - Generating **JWT tokens** for authenticated users.
  - Returning the **JWT token** as part of the response.

---

**Example: AuthenticationController**

```java
package com.example.demo.controller;

import com.example.demo.security.CustomUserDetailsService;
import com.example.demo.security.JwtTokenProvider;
import com.example.demo.model.AuthRequest;
import com.example.demo.model.AuthResponse;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/auth")
public class AuthenticationController {

    @Autowired
    private AuthenticationManager authenticationManager;

    @Autowired
    private JwtTokenProvider tokenProvider;

    @PostMapping("/login")
    public AuthResponse authenticateUser(@RequestBody AuthRequest authRequest) {
        Authentication authentication = authenticationManager.authenticate(
                new UsernamePasswordAuthenticationToken(
                        authRequest.getUsername(),
                        authRequest.getPassword()
                )
        );
        SecurityContextHolder.getContext().setAuthentication(authentication);

        String jwt = tokenProvider.generateToken(authentication);
        return new AuthResponse(jwt);
    }
}
```

- **@RestController** – Defines this class as a RESTful web service controller.
- **@RequestMapping("/api/auth")** – Base URI for all authentication-related endpoints.
- **@PostMapping("/login")** – Maps the `POST /api/auth/login` endpoint.
- **AuthenticationManager** – Authenticates users using Spring Security.
- **UsernamePasswordAuthenticationToken** – Represents authentication request.
- **SecurityContextHolder** – Stores authentication information.
- **JwtTokenProvider** – Custom class to **generate JWT tokens**.

---

### **2.3.2 Request and Response Models**

---

#### **AuthRequest Model**

- **AuthRequest** is used to **map login requests** containing username and password.

**Example: AuthRequest**

```java
package com.example.demo.model;

import lombok.Data;

@Data
public class AuthRequest {
    private String username;
    private String password;
}
```

- **@Data (Lombok)** – Generates getters, setters, and other utility methods.
- **username** – Username entered by the user.
- **password** – Password entered by the user.

---

#### **AuthResponse Model**

- **AuthResponse** is used to **return the JWT token** as part of the login response.

**Example: AuthResponse**

```java
package com.example.demo.model;

import lombok.AllArgsConstructor;
import lombok.Data;

@Data
@AllArgsConstructor
public class AuthResponse {
    private String token;
}
```

- **@Data (Lombok)** – Generates getters and setters.
- **@AllArgsConstructor** – Generates a constructor with all fields.
- **token** – Contains the JWT token.

---

## **2.4 Implementing Authorization Filter for Secured Endpoints**

- **Authorization Filter** is responsible for:
  - **Validating the JWT token** sent by the Angular frontend.
  - **Extracting user details** from the JWT token.
  - **Setting the authenticated user** in the **SecurityContext**.
  - **Allowing or denying access** to secured endpoints.

---

### **2.4.1 Creating JwtAuthenticationFilter**

**Example: JwtAuthenticationFilter**

```java
package com.example.demo.security;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
import org.springframework.stereotype.Component;
import org.springframework.web.filter.OncePerRequestFilter;
import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;

@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    @Autowired
    private JwtTokenProvider tokenProvider;

    @Autowired
    private CustomUserDetailsService customUserDetailsService;

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
            throws ServletException, IOException {

        String jwt = getJwtFromRequest(request);

        if (jwt != null && tokenProvider.validateToken(jwt)) {
            String username = tokenProvider.getUsernameFromJWT(jwt);

            var userDetails = customUserDetailsService.loadUserByUsername(username);

            var authentication = new UsernamePasswordAuthenticationToken(
                    userDetails, null, userDetails.getAuthorities());

            authentication.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));

            SecurityContextHolder.getContext().setAuthentication(authentication);
        }

        filterChain.doFilter(request, response);
    }

    private String getJwtFromRequest(HttpServletRequest request) {
        String bearerToken = request.getHeader("Authorization");
        if (bearerToken != null && bearerToken.startsWith("Bearer ")) {
            return bearerToken.substring(7);
        }
        return null;
    }
}
```

- **OncePerRequestFilter** – Ensures the filter is executed once per request.
- **getJwtFromRequest()** – Extracts the token from the **Authorization** header.
- **validateToken(jwt)** – Validates the JWT token.
- **getUsernameFromJWT(jwt)** – Extracts the username from the token.
- **loadUserByUsername(username)** – Loads user details.
- **UsernamePasswordAuthenticationToken** – Sets authentication in the **SecurityContext**.

---

### **2.4.2 Registering JwtAuthenticationFilter**

**Example: SecurityConfig**

```java
package com.example.demo.security;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
public class SecurityConfig {

    @Autowired
    private JwtAuthenticationFilter jwtAuthenticationFilter;

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .authorizeHttpRequests()
            .requestMatchers("/api/auth/**").permitAll()
            .anyRequest().authenticated()
            .and()
            .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);

        http.addFilterBefore(jwtAuthenticationFilter, UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }
}
```

- **SecurityFilterChain** – Configures security rules.
- **csrf().disable()** – Disables CSRF protection for stateless APIs.
- **SessionCreationPolicy.STATELESS** – Ensures no session is created.
- **addFilterBefore()** – Adds `JwtAuthenticationFilter` before `UsernamePasswordAuthenticationFilter`.

---

## **Next Steps:**
- **3. Creating JWT Utility Class in Spring Boot**
  - **Generating JWT Token**
  - **Validating JWT Token**
  - **Extracting User Details from JWT Token**
- **4. Integrating Angular with JWT Authentication**
  - Creating Authentication Service in Angular.
  - Storing JWT Token in Local Storage.
  - Sending JWT Token in HTTP Headers.
  - Securing Routes with AuthGuard.

---

## **Lesson 73 (Continued): Creating JWT Utility Class and Integrating Angular with JWT Authentication**

---

## **3. Creating JWT Utility Class in Spring Boot**

In this section, we will:
- Create a **JWT Utility Class** in Spring Boot to:
  - **Generate JWT tokens** for authenticated users.
  - **Validate JWT tokens** for authorization.
  - **Extract user details** such as username and roles from the JWT token.
- Use **io.jsonwebtoken (jjwt)** library for:
  - **Signing JWT tokens** with **HS256** algorithm.
  - **Parsing and validating** JWT tokens.
- Implement **Token Expiration** to enhance security.

---

### **3.1 Setting up JwtTokenProvider Class**

- **JwtTokenProvider** is responsible for:
  - **Generating JWT tokens** upon successful authentication.
  - **Validating the JWT tokens** in secured endpoints.
  - **Extracting the username** and **roles** from the JWT token.

---

### **3.1.1 Configuring application.properties**

```properties
# JWT Configuration
jwt.secret=YourSecretKeyHere
jwt.expiration=86400000  # 24 hours in milliseconds
```

- **jwt.secret** – Secret key used to **sign the JWT token**.
- **jwt.expiration** – Token expiration time in milliseconds.

---

### **3.1.2 Creating JwtTokenProvider Class**

**Example: JwtTokenProvider**

```java
package com.example.demo.security;

import io.jsonwebtoken.*;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.stereotype.Component;

import java.util.Date;
import java.util.function.Function;

@Component
public class JwtTokenProvider {

    @Value("${jwt.secret}")
    private String jwtSecret;

    @Value("${jwt.expiration}")
    private long jwtExpiration;

    // Generate JWT Token
    public String generateToken(Authentication authentication) {
        UserDetails userDetails = (UserDetails) authentication.getPrincipal();
        return Jwts.builder()
                .setSubject(userDetails.getUsername())
                .setIssuedAt(new Date())
                .setExpiration(new Date(new Date().getTime() + jwtExpiration))
                .signWith(SignatureAlgorithm.HS256, jwtSecret)
                .compact();
    }

    // Extract Username from Token
    public String getUsernameFromJWT(String token) {
        return getClaimFromToken(token, Claims::getSubject);
    }

    // Validate JWT Token
    public boolean validateToken(String token) {
        try {
            Jwts.parser().setSigningKey(jwtSecret).parseClaimsJws(token);
            return true;
        } catch (SignatureException ex) {
            System.out.println("Invalid JWT signature");
        } catch (MalformedJwtException ex) {
            System.out.println("Invalid JWT token");
        } catch (ExpiredJwtException ex) {
            System.out.println("Expired JWT token");
        } catch (UnsupportedJwtException ex) {
            System.out.println("Unsupported JWT token");
        } catch (IllegalArgumentException ex) {
            System.out.println("JWT claims string is empty");
        }
        return false;
    }

    // Extract Claims from Token
    private <T> T getClaimFromToken(String token, Function<Claims, T> claimsResolver) {
        final Claims claims = Jwts.parser()
                .setSigningKey(jwtSecret)
                .parseClaimsJws(token)
                .getBody();
        return claimsResolver.apply(claims);
    }
}
```

---

### **3.1.3 Explanation:**
- **generateToken(Authentication authentication)**:
  - Generates a **JWT token** for the authenticated user.
  - Sets the **subject** as the username.
  - Sets the **issuedAt** and **expiration** time.
  - Signs the token using **HS256** algorithm and the secret key.

- **getUsernameFromJWT(String token)**:
  - Extracts the **username** (subject) from the token.

- **validateToken(String token)**:
  - Validates the JWT token:
    - Checks **signature**, **expiration**, and **claims**.
    - Returns `true` if the token is valid, otherwise `false`.

- **getClaimFromToken()**:
  - Extracts specific claims (e.g., username, roles) from the token.

---

## **4. Integrating Angular with JWT Authentication**

In this section, we will:
- Create an **Authentication Service** in Angular to:
  - **Send login requests** to Spring Boot.
  - **Receive JWT tokens** and store them in **Local Storage**.
  - **Send JWT tokens** in the **Authorization header** of every request.
  - **Check user authentication status**.
- Implement **AuthGuard** to protect Angular routes.

---

### **4.1 Creating Authentication Service**

---

#### **4.1.1 Generating Authentication Service**

```sh
# Generate a new service for authentication
ng generate service services/authentication
```

---

#### **4.1.2 AuthenticationService**

**Example: AuthenticationService**

```typescript
// src/app/services/authentication.service.ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpHeaders } from '@angular/common/http';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';

@Injectable({
  providedIn: 'root'
})
export class AuthenticationService {

  private apiUrl = 'http://localhost:8080/api/auth/login';

  constructor(private http: HttpClient) { }

  // User Login
  login(username: string, password: string): Observable<boolean> {
    return this.http.post<any>(this.apiUrl, { username, password }).pipe(
      map(response => {
        localStorage.setItem('authToken', response.token);
        return true;
      })
    );
  }

  // User Logout
  logout(): void {
    localStorage.removeItem('authToken');
  }

  // Check if User is Logged In
  isLoggedIn(): boolean {
    return localStorage.getItem('authToken') !== null;
  }

  // Get JWT Token
  getToken(): string | null {
    return localStorage.getItem('authToken');
  }
}
```

- **login()**:
  - Sends a **POST request** to `http://localhost:8080/api/auth/login`.
  - Stores the **JWT token** in **Local Storage**.
- **logout()**:
  - Removes the **JWT token** from **Local Storage**.
- **isLoggedIn()**:
  - Checks if the **JWT token** is present in **Local Storage**.
- **getToken()**:
  - Retrieves the **JWT token** from **Local Storage**.

---

### **4.2 Sending JWT Token in HTTP Headers**

- Create an **HTTP Interceptor** to:
  - **Attach JWT token** to the **Authorization header** of every request.

---

#### **4.2.1 Creating JwtInterceptor**

```sh
# Generate a new interceptor
ng generate interceptor interceptors/jwt
```

---

#### **4.2.2 JwtInterceptor**

**Example: JwtInterceptor**

```typescript
// src/app/interceptors/jwt.interceptor.ts
import { Injectable } from '@angular/core';
import { HttpEvent, HttpHandler, HttpInterceptor, HttpRequest } from '@angular/common/http';
import { Observable } from 'rxjs';
import { AuthenticationService } from '../services/authentication.service';

@Injectable()
export class JwtInterceptor implements HttpInterceptor {

  constructor(private authService: AuthenticationService) { }

  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    const token = this.authService.getToken();

    if (token) {
      const clonedRequest = req.clone({
        headers: req.headers.set('Authorization', `Bearer ${token}`)
      });
      return next.handle(clonedRequest);
    }

    return next.handle(req);
  }
}
```

- **HttpInterceptor** – Intercepts HTTP requests.
- **req.clone()** – Clones the request and attaches the **Authorization header**.
- **next.handle()** – Passes the request to the next handler in the chain.

---

## **Next Steps:**
- **4.3 Securing Routes with AuthGuard**
- **5. Hands-on Exercise: Full Authentication and Authorization Flow**

---

## **Lesson 73 (Continued): Securing Routes with AuthGuard and Full Authentication Flow**

---

## **4.3 Securing Routes with AuthGuard in Angular**

In this section, we will:
- Implement **AuthGuard** in Angular to:
  - **Protect routes** by checking user authentication status.
  - **Redirect unauthenticated users** to the login page.
- Use **CanActivate** interface for route guarding.
- Integrate with **AuthenticationService** for authentication checks.

---

### **4.3.1 Why Use AuthGuard?**
- **AuthGuard** is used to **protect routes** in Angular by:
  - Checking if the user is **authenticated** before allowing access.
  - **Redirecting unauthenticated users** to the login page.
  - Ensuring **secured navigation** within the application.

---

### **4.3.2 Creating AuthGuard**

```sh
# Generate a new guard for authentication
ng generate guard guards/auth
```

- **ng generate guard** – Creates a new guard in `src/app/guards/auth.guard.ts`.

---

### **4.3.3 AuthGuard Implementation**

**Example: AuthGuard**

```typescript
// src/app/guards/auth.guard.ts
import { Injectable } from '@angular/core';
import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot, Router } from '@angular/router';
import { Observable } from 'rxjs';
import { AuthenticationService } from '../services/authentication.service';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {

  constructor(private authService: AuthenticationService, private router: Router) { }

  canActivate(
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): Observable<boolean> | Promise<boolean> | boolean {
    if (this.authService.isLoggedIn()) {
      return true;
    }
    this.router.navigate(['/login']);
    return false;
  }
}
```

- **CanActivate** – Checks if the user is authenticated before navigating to a route.
- **isLoggedIn()** – Checks if the **JWT token** is present in **Local Storage**.
- **router.navigate(['/login'])** – Redirects to the login page if the user is not authenticated.

---

### **4.3.4 Protecting Routes with AuthGuard**

**Example: app-routing.module.ts**

```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { UserListComponent } from './components/user-list/user-list.component';
import { UserCreateComponent } from './components/user-create/user-create.component';
import { UserEditComponent } from './components/user-edit/user-edit.component';
import { LoginComponent } from './components/login/login.component';
import { AuthGuard } from './guards/auth.guard';

const routes: Routes = [
  { path: '', redirectTo: '/users', pathMatch: 'full' },
  { path: 'login', component: LoginComponent },
  { path: 'users', component: UserListComponent, canActivate: [AuthGuard] },
  { path: 'users/create', component: UserCreateComponent, canActivate: [AuthGuard] },
  { path: 'users/edit/:id', component: UserEditComponent, canActivate: [AuthGuard] }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

- **canActivate: [AuthGuard]** – Protects routes by applying `AuthGuard`.
- **LoginComponent** – Publicly accessible route for user login.
- **UserListComponent, UserCreateComponent, UserEditComponent** – Secured routes, accessible only to authenticated users.

---

## **4.4 Creating Login Component in Angular**

---

### **4.4.1 Generating Login Component**

```sh
# Generate a new component for user login
ng generate component components/login
```

- **ng generate component** – Creates a new component in `src/app/components/login/`.

---

### **4.4.2 Template: login.component.html**

```html
<h2>Login</h2>
<form (ngSubmit)="onSubmit()">
  <label for="username">Username:</label>
  <input id="username" [(ngModel)]="username" name="username" required>
  
  <label for="password">Password:</label>
  <input type="password" id="password" [(ngModel)]="password" name="password" required>
  
  <button type="submit">Login</button>
  <div *ngIf="errorMessage" class="error">
    {{ errorMessage }}
  </div>
</form>
```

- **ngModel** – Two-way data binding for username and password.
- **name** – Required for **NgModel** in template-driven forms.
- **errorMessage** – Displays error messages if login fails.

---

### **4.4.3 Component Class: login.component.ts**

```typescript
import { Component } from '@angular/core';
import { Router } from '@angular/router';
import { AuthenticationService } from '../../services/authentication.service';

@Component({
  selector: 'app-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css']
})
export class LoginComponent {

  username: string = '';
  password: string = '';
  errorMessage: string = '';

  constructor(private authService: AuthenticationService, private router: Router) { }

  onSubmit(): void {
    this.authService.login(this.username, this.password).subscribe(
      success => {
        if (success) {
          this.router.navigate(['/users']);
        }
      },
      error => {
        this.errorMessage = 'Invalid username or password';
      }
    );
  }
}
```

- **onSubmit()** – Handles form submission and triggers login.
- **login()** – Calls the `login()` method from `AuthenticationService`.
- **router.navigate(['/users'])** – Redirects to the **User List** page upon successful login.
- **errorMessage** – Displays an error message for invalid login attempts.

---

## **5. Hands-on Exercise: Full Authentication and Authorization Flow**

---

### **5.1 Testing the Full Authentication Flow**

1. **Open a Browser and Navigate to `http://localhost:4200`**
   - Try to access **secured routes** (e.g., `/users`, `/users/create`) without logging in.
   - You should be **redirected to the login page**.

2. **Login as a Registered User**
   - Enter **username** and **password**.
   - If credentials are correct:
     - A **JWT token** is received and stored in **Local Storage**.
     - You are redirected to the **User List** page.
     - **Authorization header** is added to all subsequent HTTP requests.

3. **Test Authorization in Spring Boot Backend**
   - Access secured endpoints using **Postman** or **Angular UI**.
   - **Valid JWT tokens** should allow access.
   - **Invalid or expired tokens** should return `401 Unauthorized`.

---

### **5.2 Logout Functionality**

- **Logout**:
  - Clears the **JWT token** from **Local Storage**.
  - Redirects the user to the **Login** page.
  - Secured routes become **inaccessible** after logout.

**Example: Logout Method in AuthenticationService**

```typescript
logout(): void {
  localStorage.removeItem('authToken');
  this.router.navigate(['/login']);
}
```

- **removeItem('authToken')** – Deletes the token from **Local Storage**.
- **router.navigate(['/login'])** – Redirects to the **Login** page.

---

## **Next Steps:**
- **Lesson 74: Role-Based Authorization and Secure API Calls**
  - Implementing Role-Based Authorization in Spring Boot.
  - Securing Angular UI with Role-Based Access Control.
  - Testing Role-Based Authorization using Postman and Angular UI.
  - Enhancing Security with Refresh Tokens.

