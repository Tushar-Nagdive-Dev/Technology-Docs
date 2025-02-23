---

## **Lesson 74: Role-Based Authorization and Secure API Calls**

---

## **What You Will Learn:**
1. **Introduction to Role-Based Authorization**
   - Why Use Role-Based Authorization?
   - Understanding Roles and Permissions
   - Role-Based Access Control (RBAC) in Spring Security

2. **Implementing Role-Based Authorization in Spring Boot**
   - Setting Up User Roles and Permissions
   - Configuring Role-Based Access Control in Spring Security
   - Securing Endpoints with @PreAuthorize and @Secured
   - Testing Role-Based Authorization using Postman

3. **Securing Angular UI with Role-Based Access Control**
   - Role-Based Navigation and UI Components
   - Securing Angular Routes with Role-Based AuthGuard
   - Displaying UI Elements Based on Roles
   - Handling Unauthorized Access

4. **Hands-on Exercise: Implementing Role-Based Authorization Flow**
   - Creating Admin and User Roles in Spring Boot
   - Integrating Role-Based Authorization in Angular
   - Testing Role-Based Authorization using Postman and Angular UI

---

## **1. Introduction to Role-Based Authorization**

---

### **1.1 Why Use Role-Based Authorization?**
- **Role-Based Authorization** ensures that users:
  - Can only **access resources** they are permitted to.
  - Can only **perform actions** they are authorized to.
- It enhances **security** by:
  - Restricting **sensitive data** and **admin functionalities**.
  - Enforcing **least privilege access**.

---

### **1.2 Understanding Roles and Permissions**
- **Roles**:
  - A collection of **permissions** assigned to a user.
  - Example roles: **ADMIN**, **USER**, **MODERATOR**.
- **Permissions**:
  - Actions that a role can perform.
  - Example permissions: **READ**, **WRITE**, **UPDATE**, **DELETE**.

---

### **1.3 Role-Based Access Control (RBAC) in Spring Security**
- **Spring Security** supports **Role-Based Access Control (RBAC)** using:
  - **@PreAuthorize** – Checks user roles before entering a method.
  - **@Secured** – Restricts method access based on roles.
  - **hasRole('ROLE_NAME')** – Checks if the user has a specific role.
  - **hasAuthority('PERMISSION')** – Checks if the user has a specific permission.

---

## **2. Implementing Role-Based Authorization in Spring Boot**

---

### **2.1 Setting Up User Roles and Permissions**

---

#### **2.1.1 User Entity with Roles**

- We will **extend the User entity** to include **roles**.

**Example: User Entity with Roles**

```java
package com.example.demo.model;

import jakarta.persistence.*;
import lombok.Data;
import java.util.HashSet;
import java.util.Set;

@Entity
@Table(name = "users")
@Data
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String username;
    private String password;
    private boolean enabled;

    @ManyToMany(fetch = FetchType.EAGER, cascade = CascadeType.ALL)
    @JoinTable(
            name = "user_roles",
            joinColumns = @JoinColumn(name = "user_id"),
            inverseJoinColumns = @JoinColumn(name = "role_id")
    )
    private Set<Role> roles = new HashSet<>();
}
```

- **@ManyToMany** – Establishes a **many-to-many relationship** between `User` and `Role`.
- **@JoinTable** – Maps the relationship to a **join table** named `user_roles`.

---

#### **2.1.2 Role Entity**

- **Role Entity** represents the **role** assigned to the user.
- Roles are stored in a separate table named `roles`.

**Example: Role Entity**

```java
package com.example.demo.model;

import jakarta.persistence.*;
import lombok.Data;
import org.springframework.security.core.GrantedAuthority;

@Entity
@Table(name = "roles")
@Data
public class Role implements GrantedAuthority {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(unique = true)
    private String name;

    @Override
    public String getAuthority() {
        return name;
    }
}
```

- **@Entity** – Marks the class as a JPA entity.
- **@Table(name = "roles")** – Maps the entity to the `roles` table.
- **name** – Stores the role name (e.g., `ROLE_ADMIN`, `ROLE_USER`).
- **implements GrantedAuthority** – Allows Spring Security to use `Role` as a **GrantedAuthority**.

---

### **2.2 Configuring Role-Based Access Control in Spring Security**

---

#### **2.2.1 Extending CustomUserDetailsService**

- We will **extend the `CustomUserDetailsService`** to load **user roles** and **permissions**.

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
import java.util.stream.Collectors;

@Service
public class CustomUserDetailsService implements UserDetailsService {

    @Autowired
    private UserRepository userRepository;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        User user = userRepository.findByUsername(username)
                .orElseThrow(() -> new UsernameNotFoundException("User not found with username: " + username));
        
        var authorities = user.getRoles().stream()
                .map(role -> "ROLE_" + role.getName())
                .collect(Collectors.toList());

        return org.springframework.security.core.userdetails.User.builder()
                .username(user.getUsername())
                .password(user.getPassword())
                .authorities(authorities.toArray(new String[0]))
                .build();
    }
}
```

- **user.getRoles().stream()** – Streams the user's roles.
- **map(role -> "ROLE_" + role.getName())** – Converts role names to Spring Security's required format (e.g., `ROLE_ADMIN`).
- **authorities.toArray(new String[0])** – Converts the list of roles to an array.

---

### **2.3 Securing Endpoints with @PreAuthorize and @Secured**

- Use **@PreAuthorize** and **@Secured** annotations to **secure endpoints**.

---

#### **2.3.1 Securing Controllers with @PreAuthorize**

**Example: UserController**

```java
package com.example.demo.controller;

import com.example.demo.model.User;
import com.example.demo.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/users")
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping
    @PreAuthorize("hasRole('ADMIN')")
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }

    @PostMapping
    @PreAuthorize("hasRole('ADMIN')")
    public User createUser(@RequestBody User user) {
        return userService.createUser(user);
    }

    @PutMapping("/{id}")
    @PreAuthorize("hasRole('ADMIN') or hasRole('USER')")
    public User updateUser(@PathVariable Long id, @RequestBody User userDetails) {
        return userService.updateUser(id, userDetails);
    }

    @DeleteMapping("/{id}")
    @PreAuthorize("hasRole('ADMIN')")
    public void deleteUser(@PathVariable Long id) {
        userService.deleteUser(id);
    }
}
```

- **@PreAuthorize** – Checks roles **before** entering the method:
  - `hasRole('ADMIN')` – Only **ADMIN** can access.
  - `hasRole('ADMIN') or hasRole('USER')` – Either **ADMIN** or **USER** can access.

---

## **Next Steps:**
- **3. Securing Angular UI with Role-Based Access Control**
  - Role-Based Navigation and UI Components.
  - Securing Angular Routes with Role-Based AuthGuard.
  - Displaying UI Elements Based on Roles.
  - Handling Unauthorized Access.

---

## **Lesson 74 (Continued): Securing Angular UI with Role-Based Access Control**

---

## **3. Securing Angular UI with Role-Based Access Control**

In this section, we will:
- Implement **Role-Based Navigation and UI Components** in Angular.
- Secure Angular routes with **Role-Based AuthGuard**.
- **Display UI elements** based on user roles.
- **Handle Unauthorized Access** and provide user feedback.

---

### **3.1 Role-Based Navigation and UI Components**

- We will enhance the Angular UI to:
  - Display **different navigation menus** based on user roles.
  - **Restrict access** to certain components and features.
  - Provide a **seamless user experience** with role-specific content.

---

### **3.1.1 Updating AuthenticationService to Extract Roles**

---

#### **3.1.1.1 Extracting Roles from JWT Token**

- JWT tokens contain roles in their **payload**.
- We will **decode the token** and extract user roles.

---

**Example: Updated AuthenticationService**

```typescript
// src/app/services/authentication.service.ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpHeaders } from '@angular/common/http';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';
import jwt_decode from 'jwt-decode';

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

  // Get JWT Token
  getToken(): string | null {
    return localStorage.getItem('authToken');
  }

  // Decode Token and Extract Roles
  getUserRoles(): string[] {
    const token = this.getToken();
    if (token) {
      const decodedToken: any = jwt_decode(token);
      return decodedToken.roles || [];
    }
    return [];
  }

  // Check if User has a Specific Role
  hasRole(role: string): boolean {
    return this.getUserRoles().includes(role);
  }

  // User Logout
  logout(): void {
    localStorage.removeItem('authToken');
  }

  // Check if User is Logged In
  isLoggedIn(): boolean {
    return this.getToken() !== null;
  }
}
```

- **jwt_decode** – Decodes the JWT token and extracts the payload.
- **getUserRoles()**:
  - Extracts the **roles** from the decoded token.
  - Returns an array of roles (e.g., `['ROLE_ADMIN', 'ROLE_USER']`).
- **hasRole(role: string)**:
  - Checks if the user has a specific role.

---

#### **3.1.1.2 Installing jwt-decode Library**

```sh
# Install jwt-decode for decoding JWT tokens
npm install jwt-decode
```

- **jwt-decode** is used to **decode JWT tokens** without validating the signature.

---

### **3.2 Securing Angular Routes with Role-Based AuthGuard**

- We will create **RoleGuard** to:
  - **Protect routes** based on user roles.
  - **Restrict navigation** for unauthorized users.

---

#### **3.2.1 Creating RoleGuard**

```sh
# Generate a new guard for role-based authorization
ng generate guard guards/role
```

- **ng generate guard** – Creates a new guard in `src/app/guards/role.guard.ts`.

---

#### **3.2.2 RoleGuard Implementation**

**Example: RoleGuard**

```typescript
// src/app/guards/role.guard.ts
import { Injectable } from '@angular/core';
import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot, Router } from '@angular/router';
import { Observable } from 'rxjs';
import { AuthenticationService } from '../services/authentication.service';

@Injectable({
  providedIn: 'root'
})
export class RoleGuard implements CanActivate {

  constructor(private authService: AuthenticationService, private router: Router) { }

  canActivate(
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): Observable<boolean> | Promise<boolean> | boolean {
    const expectedRole = route.data['role'];
    if (this.authService.isLoggedIn() && this.authService.hasRole(expectedRole)) {
      return true;
    }
    this.router.navigate(['/unauthorized']);
    return false;
  }
}
```

- **CanActivate** – Checks if the user has the required role before navigating to a route.
- **route.data['role']** – Extracts the **expected role** from the route configuration.
- **hasRole(expectedRole)** – Checks if the user has the required role.
- **router.navigate(['/unauthorized'])** – Redirects to the **Unauthorized** page if the user lacks the required role.

---

### **3.3 Protecting Routes with RoleGuard**

**Example: app-routing.module.ts**

```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { UserListComponent } from './components/user-list/user-list.component';
import { UserCreateComponent } from './components/user-create/user-create.component';
import { UserEditComponent } from './components/user-edit/user-edit.component';
import { LoginComponent } from './components/login/login.component';
import { UnauthorizedComponent } from './components/unauthorized/unauthorized.component';
import { AuthGuard } from './guards/auth.guard';
import { RoleGuard } from './guards/role.guard';

const routes: Routes = [
  { path: '', redirectTo: '/users', pathMatch: 'full' },
  { path: 'login', component: LoginComponent },
  { path: 'unauthorized', component: UnauthorizedComponent },
  { path: 'users', component: UserListComponent, canActivate: [AuthGuard] },
  { path: 'users/create', component: UserCreateComponent, canActivate: [RoleGuard], data: { role: 'ROLE_ADMIN' } },
  { path: 'users/edit/:id', component: UserEditComponent, canActivate: [RoleGuard], data: { role: 'ROLE_ADMIN' } }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

- **canActivate: [RoleGuard]** – Protects routes with `RoleGuard`.
- **data: { role: 'ROLE_ADMIN' }** – Specifies the **required role** for the route.
- **UnauthorizedComponent** – Displays an error message for unauthorized access.

---

### **3.4 Displaying UI Elements Based on Roles**

**Example: Conditional UI Rendering**

```html
<!-- Navigation Menu in app.component.html -->
<nav>
  <a routerLink="/users">User List</a>
  <a *ngIf="authService.hasRole('ROLE_ADMIN')" routerLink="/users/create">Add User</a>
  <a (click)="authService.logout()">Logout</a>
</nav>
```

- **ngIf="authService.hasRole('ROLE_ADMIN')"**:
  - Conditionally displays the **Add User** link **only for admins**.
- **routerLink** – Navigates to different routes.

---

### **3.5 Handling Unauthorized Access**

**Example: UnauthorizedComponent**

```html
<!-- src/app/components/unauthorized/unauthorized.component.html -->
<h2>Unauthorized</h2>
<p>You do not have permission to access this page.</p>
<a routerLink="/login">Go to Login</a>
```

- Displays an **error message** for unauthorized access.
- Provides a **link to the login page**.

---

## **Next Steps:**
- **4. Hands-on Exercise: Full Role-Based Authorization Flow**
  - Creating Admin and User Roles in Spring Boot.
  - Integrating Role-Based Authorization in Angular.
  - Testing Role-Based Authorization using Postman and Angular UI.
  - Enhancing Security with Refresh Tokens.

---

## **Lesson 74 (Continued): Full Role-Based Authorization Flow and Testing**

---

## **4. Hands-on Exercise: Full Role-Based Authorization Flow**

In this section, we will:
- **Create Admin and User Roles** in Spring Boot.
- **Integrate Role-Based Authorization** in Angular.
- **Test Role-Based Authorization** using:
  - **Postman** for API testing.
  - **Angular UI** for end-to-end testing.
- **Enhance Security with Refresh Tokens** for token renewal.

---

### **4.1 Creating Admin and User Roles in Spring Boot**

- We will create **Admin** and **User** roles:
  - **ROLE_ADMIN** – Has full access to all resources.
  - **ROLE_USER** – Has limited access (e.g., read-only).
- These roles will be **stored in the database** and **assigned to users**.

---

### **4.1.1 Creating Role and User Entities**

**Example: Role Entity**

```java
package com.example.demo.model;

import jakarta.persistence.*;
import lombok.Data;
import org.springframework.security.core.GrantedAuthority;

@Entity
@Table(name = "roles")
@Data
public class Role implements GrantedAuthority {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(unique = true)
    private String name;

    @Override
    public String getAuthority() {
        return name;
    }
}
```

- **@Entity** – Marks the class as a JPA entity.
- **@Table(name = "roles")** – Maps the entity to the `roles` table.
- **name** – Stores the role name (e.g., `ROLE_ADMIN`, `ROLE_USER`).
- **implements GrantedAuthority** – Allows Spring Security to use `Role` as a **GrantedAuthority**.

---

**Example: User Entity with Roles**

```java
package com.example.demo.model;

import jakarta.persistence.*;
import lombok.Data;
import java.util.HashSet;
import java.util.Set;

@Entity
@Table(name = "users")
@Data
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String username;
    private String password;
    private boolean enabled;

    @ManyToMany(fetch = FetchType.EAGER, cascade = CascadeType.ALL)
    @JoinTable(
            name = "user_roles",
            joinColumns = @JoinColumn(name = "user_id"),
            inverseJoinColumns = @JoinColumn(name = "role_id")
    )
    private Set<Role> roles = new HashSet<>();
}
```

- **@ManyToMany** – Establishes a **many-to-many relationship** between `User` and `Role`.
- **@JoinTable** – Maps the relationship to a **join table** named `user_roles`.

---

### **4.1.2 Creating Role and User Repositories**

**Example: RoleRepository**

```java
package com.example.demo.repository;

import com.example.demo.model.Role;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface RoleRepository extends JpaRepository<Role, Long> {
    Role findByName(String name);
}
```

- **JpaRepository<Role, Long>** – Manages `Role` entity with `Long` as the primary key.
- **findByName(String name)** – Finds a role by its name.

---

**Example: UserRepository**

```java
package com.example.demo.repository;

import com.example.demo.model.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import java.util.Optional;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByUsername(String username);
}
```

- **JpaRepository<User, Long>** – Manages `User` entity with `Long` as the primary key.
- **findByUsername(String username)** – Finds a user by their username.

---

### **4.1.3 Initializing Admin and User Roles**

- We will **initialize roles** and **create an admin user** during application startup.

**Example: DataLoader Class**

```java
package com.example.demo.config;

import com.example.demo.model.Role;
import com.example.demo.model.User;
import com.example.demo.repository.RoleRepository;
import com.example.demo.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.stereotype.Component;

import java.util.HashSet;
import java.util.Set;

@Component
public class DataLoader implements CommandLineRunner {

    @Autowired
    private RoleRepository roleRepository;

    @Autowired
    private UserRepository userRepository;

    @Autowired
    private PasswordEncoder passwordEncoder;

    @Override
    public void run(String... args) {
        // Create Admin Role
        if (roleRepository.findByName("ROLE_ADMIN") == null) {
            Role adminRole = new Role();
            adminRole.setName("ROLE_ADMIN");
            roleRepository.save(adminRole);
        }

        // Create User Role
        if (roleRepository.findByName("ROLE_USER") == null) {
            Role userRole = new Role();
            userRole.setName("ROLE_USER");
            roleRepository.save(userRole);
        }

        // Create Admin User
        if (userRepository.findByUsername("admin").isEmpty()) {
            User admin = new User();
            admin.setUsername("admin");
            admin.setPassword(passwordEncoder.encode("admin123"));
            admin.setEnabled(true);

            Set<Role> roles = new HashSet<>();
            roles.add(roleRepository.findByName("ROLE_ADMIN"));
            roles.add(roleRepository.findByName("ROLE_USER"));
            admin.setRoles(roles);

            userRepository.save(admin);
        }
    }
}
```

- **CommandLineRunner** – Executes code after the Spring Boot application starts.
- **roleRepository.save()** – Saves roles in the database.
- **passwordEncoder.encode()** – Encrypts the admin password.
- **userRepository.save()** – Creates an admin user with:
  - Username: `admin`
  - Password: `admin123`
  - Roles: **ROLE_ADMIN** and **ROLE_USER**

---

### **4.1.4 Testing Role-Based Authorization using Postman**

1. **Start Spring Boot Backend**:
```sh
# Navigate to the Spring Boot project directory
mvn spring-boot:run
```

2. **Access API Endpoints with Postman**:
   - **Login as Admin**:
     - URL: `http://localhost:8080/api/auth/login`
     - Method: `POST`
     - Body (JSON):
       ```json
       {
         "username": "admin",
         "password": "admin123"
       }
       ```
     - Response:
       ```json
       {
         "token": "JWT_TOKEN_HERE"
       }
       ```
     - **Copy the JWT token** from the response.

   - **Access Secured Endpoints**:
     - Add **Authorization Header**:
       ```
       Authorization: Bearer JWT_TOKEN_HERE
       ```
     - Test different endpoints to verify role-based access control:
       - `/api/users` – Accessible only to **ROLE_ADMIN**.
       - `/api/users/create` – Accessible only to **ROLE_ADMIN**.
       - `/api/users/edit/{id}` – Accessible to **ROLE_ADMIN** and **ROLE_USER**.

---

### **4.2 Integrating Role-Based Authorization in Angular**

1. **Start Angular Frontend**:
```sh
# Navigate to the Angular project directory
ng serve
```

2. **Test Angular UI**:
   - **Login as Admin**:
     - Username: `admin`
     - Password: `admin123`
   - **Check Role-Based UI Rendering**:
     - **Admin Menu** should display:
       - **User List**
       - **Add User**
       - **Edit User**
     - **Unauthorized Access**:
       - If accessing a restricted route, the user is redirected to the **Unauthorized** page.

---

## **Next Steps:**
- **5. Enhancing Security with Refresh Tokens**
  - Implementing Refresh Tokens in Spring Boot.
  - Integrating Refresh Token Logic in Angular.
  - Enhancing User Experience with Token Renewal.

---

## **Lesson 74 (Continued): Enhancing Security with Refresh Tokens**

---

## **5. Enhancing Security with Refresh Tokens**

In this section, we will:
- **Implement Refresh Tokens** in Spring Boot to:
  - **Renew expired access tokens** without requiring re-login.
  - **Enhance user experience** by maintaining seamless sessions.
- **Integrate Refresh Token Logic** in Angular to:
  - **Automatically renew tokens** using an HTTP Interceptor.
  - **Handle token expiration and renewal gracefully**.
- **Test the Complete Flow** using:
  - **Postman** for backend API testing.
  - **Angular UI** for end-to-end testing.

---

### **5.1 What are Refresh Tokens?**
- **Access Tokens**:
  - Short-lived tokens used to **access protected resources**.
  - Contain user information and roles.
  - **Expire quickly** to enhance security.
- **Refresh Tokens**:
  - Long-lived tokens used to **renew access tokens**.
  - Stored securely (e.g., **HTTP-only cookies**).
  - **Never sent with each request**, reducing attack surface.

---

### **5.2 Refresh Token Flow Overview**

1. **User Login**:
   - User authenticates with username and password.
   - **Access Token** and **Refresh Token** are issued.

2. **Accessing Protected Resources**:
   - Angular sends **Access Token** in **Authorization header**.
   - Spring Boot validates the **Access Token**.

3. **Token Expiration and Renewal**:
   - If the **Access Token** is expired:
     - Angular sends the **Refresh Token** to Spring Boot.
     - Spring Boot verifies the **Refresh Token**.
     - If valid, a **new Access Token** is issued.
     - Angular retries the original request with the new token.

4. **Logout**:
   - Refresh Token is **invalidated** in the database.
   - Access Token is **deleted** from Local Storage.

---

## **5.3 Implementing Refresh Tokens in Spring Boot**

---

### **5.3.1 Updating JwtTokenProvider for Refresh Tokens**

- We will **extend JwtTokenProvider** to:
  - **Generate Refresh Tokens** with a longer expiration time.
  - **Validate Refresh Tokens** during token renewal.
  - **Issue new Access Tokens** upon successful validation.

---

#### **5.3.1.1 Updating application.properties**

```properties
# JWT Configuration
jwt.secret=YourSecretKeyHere
jwt.access.expiration=900000        # 15 minutes in milliseconds
jwt.refresh.expiration=604800000    # 7 days in milliseconds
```

- **jwt.access.expiration** – Expiration time for **Access Tokens**.
- **jwt.refresh.expiration** – Expiration time for **Refresh Tokens**.

---

#### **5.3.1.2 Updating JwtTokenProvider Class**

**Example: Updated JwtTokenProvider**

```java
package com.example.demo.security;

import io.jsonwebtoken.*;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.security.core.Authentication;
import org.springframework.stereotype.Component;
import java.util.Date;
import java.util.function.Function;

@Component
public class JwtTokenProvider {

    @Value("${jwt.secret}")
    private String jwtSecret;

    @Value("${jwt.access.expiration}")
    private long accessTokenExpiration;

    @Value("${jwt.refresh.expiration}")
    private long refreshTokenExpiration;

    // Generate Access Token
    public String generateAccessToken(Authentication authentication) {
        var userDetails = (org.springframework.security.core.userdetails.User) authentication.getPrincipal();
        return Jwts.builder()
                .setSubject(userDetails.getUsername())
                .setIssuedAt(new Date())
                .setExpiration(new Date(new Date().getTime() + accessTokenExpiration))
                .signWith(SignatureAlgorithm.HS256, jwtSecret)
                .compact();
    }

    // Generate Refresh Token
    public String generateRefreshToken(String username) {
        return Jwts.builder()
                .setSubject(username)
                .setIssuedAt(new Date())
                .setExpiration(new Date(new Date().getTime() + refreshTokenExpiration))
                .signWith(SignatureAlgorithm.HS256, jwtSecret)
                .compact();
    }

    // Validate Token
    public boolean validateToken(String token) {
        try {
            Jwts.parser().setSigningKey(jwtSecret).parseClaimsJws(token);
            return true;
        } catch (Exception ex) {
            return false;
        }
    }

    // Extract Username from Token
    public String getUsernameFromToken(String token) {
        return getClaimFromToken(token, Claims::getSubject);
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

- **generateAccessToken()** – Generates a short-lived **Access Token**.
- **generateRefreshToken()** – Generates a long-lived **Refresh Token**.
- **validateToken()** – Validates the token's signature and expiration.
- **getUsernameFromToken()** – Extracts the username from the token.

---

### **5.3.2 Storing and Managing Refresh Tokens**

- We will **store Refresh Tokens** in the database to:
  - **Validate Refresh Tokens** during renewal.
  - **Invalidate Refresh Tokens** during logout.

---

#### **5.3.2.1 Creating RefreshToken Entity**

**Example: RefreshToken Entity**

```java
package com.example.demo.model;

import jakarta.persistence.*;
import lombok.Data;
import java.time.Instant;

@Entity
@Data
public class RefreshToken {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true)
    private String token;

    @Column(nullable = false)
    private Instant expiryDate;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "user_id", nullable = false)
    private User user;
}
```

- **@Entity** – Marks the class as a JPA entity.
- **@ManyToOne** – Links the Refresh Token to a **User**.
- **expiryDate** – Stores the token expiration date.

---

#### **5.3.2.2 Creating RefreshTokenRepository**

**Example: RefreshTokenRepository**

```java
package com.example.demo.repository;

import com.example.demo.model.RefreshToken;
import com.example.demo.model.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import java.util.Optional;

@Repository
public interface RefreshTokenRepository extends JpaRepository<RefreshToken, Long> {
    Optional<RefreshToken> findByToken(String token);
    void deleteByUser(User user);
}
```

- **JpaRepository<RefreshToken, Long>** – Manages `RefreshToken` entity.
- **findByToken(String token)** – Finds a token by its value.
- **deleteByUser(User user)** – Deletes tokens associated with a user.

---

### **5.3.3 Creating Refresh Token Controller**

- We will **create an endpoint** to:
  - **Validate the Refresh Token**.
  - **Issue a new Access Token** if valid.

**Example: RefreshTokenController**

```java
package com.example.demo.controller;

import com.example.demo.security.JwtTokenProvider;
import com.example.demo.model.RefreshToken;
import com.example.demo.repository.RefreshTokenRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/auth")
public class RefreshTokenController {

    @Autowired
    private JwtTokenProvider tokenProvider;

    @Autowired
    private RefreshTokenRepository refreshTokenRepository;

    @PostMapping("/refresh-token")
    public String refreshToken(@RequestBody String refreshToken) {
        RefreshToken token = refreshTokenRepository.findByToken(refreshToken)
                .orElseThrow(() -> new RuntimeException("Invalid refresh token"));

        if (token.getExpiryDate().isBefore(java.time.Instant.now())) {
            throw new RuntimeException("Refresh token has expired");
        }

        return tokenProvider.generateAccessToken(token.getUser().getUsername());
    }
}
```

- **@PostMapping("/refresh-token")** – Endpoint for **refreshing Access Tokens**.
- **validateToken()** – Validates the **Refresh Token**.
- **generateAccessToken()** – Issues a new **Access Token**.

---

## **Next Steps:**
- **5.4 Integrating Refresh Token Logic in Angular**
  - Automatically Renewing Tokens with HTTP Interceptor.
  - Handling Token Expiration Gracefully.

---

## **Lesson 74 (Continued): Integrating Refresh Token Logic in Angular**

---

## **5.4 Integrating Refresh Token Logic in Angular**

In this section, we will:
- **Integrate Refresh Token Logic** in Angular to:
  - **Automatically renew Access Tokens** using an HTTP Interceptor.
  - **Retry failed requests** after successful token renewal.
  - **Handle token expiration** gracefully, including redirecting to login.
- **Enhance User Experience** by maintaining seamless sessions and reducing unnecessary logouts.

---

### **5.4.1 Overview of Refresh Token Flow in Angular**

1. **Sending Requests with Access Token**:
   - Angular sends **Access Token** in the **Authorization header**.
   - Spring Boot **validates the token**.
   - If valid, the request is processed successfully.
   - If **expired**, a `401 Unauthorized` response is returned.

2. **Handling 401 Unauthorized Response**:
   - **HTTP Interceptor** detects `401 Unauthorized`.
   - **Refresh Token** is sent to Spring Boot to get a new Access Token.
   - If the **Refresh Token is valid**:
     - A new **Access Token** is issued.
     - The **original request** is retried with the new token.
   - If the **Refresh Token is invalid or expired**:
     - The user is **redirected to the login page**.

---

### **5.4.2 Creating Token Refresh Logic in AuthenticationService**

- We will **extend AuthenticationService** to:
  - **Send Refresh Token requests** to Spring Boot.
  - **Store the new Access Token** in Local Storage.
  - **Handle Token Expiration** and redirect to login.

---

#### **5.4.2.1 Updating AuthenticationService**

**Example: Updated AuthenticationService**

```typescript
// src/app/services/authentication.service.ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpHeaders } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { map, catchError } from 'rxjs/operators';
import jwt_decode from 'jwt-decode';

@Injectable({
  providedIn: 'root'
})
export class AuthenticationService {

  private apiUrl = 'http://localhost:8080/api/auth';
  private refreshTokenInProgress: boolean = false;

  constructor(private http: HttpClient) { }

  // User Login
  login(username: string, password: string): Observable<boolean> {
    return this.http.post<any>(`${this.apiUrl}/login`, { username, password }).pipe(
      map(response => {
        localStorage.setItem('authToken', response.token);
        localStorage.setItem('refreshToken', response.refreshToken);
        return true;
      })
    );
  }

  // Get Access Token
  getToken(): string | null {
    return localStorage.getItem('authToken');
  }

  // Get Refresh Token
  getRefreshToken(): string | null {
    return localStorage.getItem('refreshToken');
  }

  // Refresh Access Token
  refreshAccessToken(): Observable<string> {
    if (this.refreshTokenInProgress) {
      return throwError('Refresh Token in progress');
    }
    this.refreshTokenInProgress = true;

    return this.http.post<any>(`${this.apiUrl}/refresh-token`, {
      refreshToken: this.getRefreshToken()
    }).pipe(
      map(response => {
        localStorage.setItem('authToken', response.token);
        this.refreshTokenInProgress = false;
        return response.token;
      }),
      catchError(error => {
        this.logout();
        this.refreshTokenInProgress = false;
        return throwError(error);
      })
    );
  }

  // User Logout
  logout(): void {
    localStorage.removeItem('authToken');
    localStorage.removeItem('refreshToken');
  }

  // Check if User is Logged In
  isLoggedIn(): boolean {
    return this.getToken() !== null;
  }
}
```

- **refreshAccessToken()**:
  - Sends a **POST request** to `http://localhost:8080/api/auth/refresh-token`.
  - If successful:
    - The **new Access Token** is saved in **Local Storage**.
    - The **Refresh Token** remains unchanged.
  - If unsuccessful:
    - The user is **logged out**.
    - The **Refresh Token** is invalidated.

---

### **5.4.3 Automatically Renewing Tokens with HTTP Interceptor**

- We will create a **TokenInterceptor** to:
  - **Attach Access Token** to every request.
  - **Intercept 401 Unauthorized** responses.
  - **Request a new Access Token** using the Refresh Token.
  - **Retry the original request** with the new Access Token.

---

#### **5.4.3.1 Creating TokenInterceptor**

```sh
# Generate a new interceptor for token renewal
ng generate interceptor interceptors/token
```

- **ng generate interceptor** – Creates a new interceptor in `src/app/interceptors/token.interceptor.ts`.

---

#### **5.4.3.2 TokenInterceptor Implementation**

**Example: TokenInterceptor**

```typescript
// src/app/interceptors/token.interceptor.ts
import { Injectable } from '@angular/core';
import { HttpEvent, HttpHandler, HttpInterceptor, HttpRequest, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError, switchMap } from 'rxjs/operators';
import { AuthenticationService } from '../services/authentication.service';

@Injectable()
export class TokenInterceptor implements HttpInterceptor {

  constructor(private authService: AuthenticationService) { }

  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    const token = this.authService.getToken();

    let authReq = req;
    if (token) {
      authReq = req.clone({
        headers: req.headers.set('Authorization', `Bearer ${token}`)
      });
    }

    return next.handle(authReq).pipe(
      catchError((error: HttpErrorResponse) => {
        // If 401 Unauthorized, try to refresh the token
        if (error.status === 401) {
          return this.authService.refreshAccessToken().pipe(
            switchMap(newToken => {
              const newAuthReq = req.clone({
                headers: req.headers.set('Authorization', `Bearer ${newToken}`)
              });
              return next.handle(newAuthReq);
            }),
            catchError(err => {
              this.authService.logout();
              return throwError(err);
            })
          );
        }
        return throwError(error);
      })
    );
  }
}
```

- **HttpInterceptor** – Intercepts all HTTP requests.
- **req.clone()** – Clones the request and attaches the **Authorization header**.
- **catchError()** – Handles `401 Unauthorized` responses.
- **switchMap()**:
  - **Requests a new Access Token** using the Refresh Token.
  - **Retries the original request** with the new Access Token.
- **logout()** – Logs out the user if the Refresh Token is invalid.

---

### **5.4.4 Registering TokenInterceptor**

**Example: app.module.ts**

```typescript
// src/app/app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { HttpClientModule, HTTP_INTERCEPTORS } from '@angular/common/http';
import { AppComponent } from './app.component';
import { TokenInterceptor } from './interceptors/token.interceptor';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, HttpClientModule],
  providers: [
    {
      provide: HTTP_INTERCEPTORS,
      useClass: TokenInterceptor,
      multi: true
    }
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

- **provide: HTTP_INTERCEPTORS** – Registers the interceptor as an Angular provider.
- **useClass: TokenInterceptor** – Specifies the **TokenInterceptor** class.

---

## **5.5 Testing Token Renewal Flow**

1. **Start Spring Boot Backend**:
```sh
mvn spring-boot:run
```

2. **Start Angular Frontend**:
```sh
ng serve
```

3. **Test Token Renewal Flow**:
   - **Login** and **access protected resources**.
   - **Wait for the Access Token to expire**.
   - The request should **retry with the new Access Token**.
   - The **user should remain logged in** without any disruption.

---

## **Next Steps:**
- **Lesson 75: Angular + Spring Boot Integration (with Containerization and Deployment)**
  - **Containerizing Angular and Spring Boot Apps**.
  - **Deploying on Cloud Platforms (e.g., AWS, Azure)**.
