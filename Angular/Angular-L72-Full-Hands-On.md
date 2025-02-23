---

## **Lesson 72 (Continued): Hands-on Exercise - Building a Complete CRUD Feature**

---

## **5. Hands-on Exercise: Building a Complete CRUD Feature**

In this section, we will:
- Build a **full-stack CRUD application** using **Angular** and **Spring Boot**.
- Implement:
  - **Create**, **Read**, **Update**, and **Delete** operations.
  - **Form validation** and **error handling**.
  - **Two-way data binding** in Angular forms.
  - **Routing and navigation** for seamless user experience.
- Deploy the application locally with **Spring Boot** as the backend and **Angular** as the frontend.

---

## **5.1 Application Overview**

### **5.1.1 Project Description**
We will create a **User Management System** that allows users to:
- **List** all users.
- **Add** a new user.
- **Edit** an existing user.
- **Delete** a user.

---

### **5.1.2 Tech Stack and Tools**
- **Frontend**: Angular 15+ with Angular Material for UI
- **Backend**: Spring Boot 3.x with Spring Data JPA
- **Database**: MySQL or PostgreSQL (Your choice)
- **Tools**:
  - **Node.js and Angular CLI** for Angular development.
  - **Maven** or **Gradle** for Spring Boot.
  - **Postman** for API testing.
  - **Visual Studio Code** and **IntelliJ IDEA** as IDEs.

---

### **5.1.3 Application Architecture**
- **Frontend (Angular)**:
  - **User Service** for API communication.
  - **Components** for User List, Create, Edit.
  - **Reactive Forms** for form validation.
  - **Routing** for navigation between pages.
- **Backend (Spring Boot)**:
  - **Controller Layer** – Manages HTTP requests and responses.
  - **Service Layer** – Business logic.
  - **Repository Layer** – Data persistence using Spring Data JPA.
  - **CORS Configuration** – Allows communication with Angular frontend.

---

## **5.2 Backend: Spring Boot CRUD API**

---

### **5.2.1 Project Setup and Dependencies**

#### **Create a Spring Boot Project**

- Go to **[Spring Initializr](https://start.spring.io)** and configure:
  - **Project Type**: Maven
  - **Language**: Java
  - **Spring Boot Version**: 3.x.x (Latest)
  - **Dependencies**:
    - **Spring Web** – For RESTful endpoints.
    - **Spring Data JPA** – For database access.
    - **MySQL Driver** or **PostgreSQL Driver** – For database connection.
    - **Lombok** – For boilerplate code reduction.

- **Download** and **Import** the project into your IDE (IntelliJ or Spring Tool Suite).

---

#### **Add Dependencies in pom.xml (Maven)**

```xml
<dependencies>
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

- **spring-boot-starter-web** – For building RESTful APIs.
- **spring-boot-starter-data-jpa** – For database operations with JPA.
- **mysql-connector-java** – JDBC driver for MySQL.
- **Lombok** – Reduces boilerplate code for models and services.

---

### **5.2.2 Configuration: application.properties**

**MySQL Configuration:**
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/user_management
spring.datasource.username=root
spring.datasource.password=yourpassword
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
```

**PostgreSQL Configuration (Alternative):**
```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/user_management
spring.datasource.username=postgres
spring.datasource.password=yourpassword
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
```

- **spring.jpa.hibernate.ddl-auto=update** – Automatically creates/updates tables.
- **spring.jpa.properties.hibernate.dialect** – Configures the database dialect.

---

### **5.2.3 Model Layer: User Entity**

**Example: User Entity Class**
```java
package com.example.demo.model;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.Table;
import lombok.Data;

@Entity
@Table(name = "users")
@Data
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String email;
    private String password;
}
```

- **@Entity** – Marks the class as a JPA entity.
- **@Table(name = "users")** – Maps to the `users` table.
- **@Id** – Primary key.
- **@GeneratedValue** – Auto-generates primary key values.
- **@Data (Lombok)** – Generates getters, setters, `equals()`, `hashCode()`, and `toString()`.

---

### **5.2.4 Repository Layer: UserRepository**

**Example: UserRepository Interface**
```java
package com.example.demo.repository;

import com.example.demo.model.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
}
```

- **JpaRepository<User, Long>** – Manages `User` entity with `Long` as the primary key.
- **Spring Data JPA** automatically implements the CRUD operations.

---

### **5.2.5 Service Layer: UserService**

**Example: UserService Class**
```java
package com.example.demo.service;

import com.example.demo.model.User;
import com.example.demo.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Optional;

@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    public List<User> getAllUsers() {
        return userRepository.findAll();
    }

    public Optional<User> getUserById(Long id) {
        return userRepository.findById(id);
    }

    public User createUser(User user) {
        return userRepository.save(user);
    }

    public User updateUser(Long id, User userDetails) {
        User user = userRepository.findById(id)
            .orElseThrow(() -> new RuntimeException("User not found with id: " + id));
        user.setName(userDetails.getName());
        user.setEmail(userDetails.getEmail());
        user.setPassword(userDetails.getPassword());
        return userRepository.save(user);
    }

    public void deleteUser(Long id) {
        userRepository.deleteById(id);
    }
}
```

- **@Service** – Registers the class as a Spring service.
- **@Autowired** – Injects `UserRepository`.
- **Optional<User>** – Handles potential null values.
- **RuntimeException** – Custom exception for not found scenarios.

---

## **Next Steps:**
- **5.2.6 Controller Layer: UserController**
  - Expose REST endpoints for CRUD operations.
  - Integrate with Angular using **HttpClientModule**.
- **5.3 Frontend: Angular CRUD Integration**
  - Creating Angular components for User List, Create, Edit.
  - Routing and Navigation.
  - Form validation and error handling.

---

## **Lesson 72 (Continued): Building a Complete CRUD Feature - Controller Layer and Frontend Integration**

---

## **5.2.6 Controller Layer: UserController**

In this section, we will:
- **Create a REST Controller** in Spring Boot to expose CRUD endpoints.
- **Integrate Spring Boot API** with Angular using **HttpClientModule**.
- **Handle CORS** to allow communication between Angular and Spring Boot.

---

### **5.2.6.1 Creating UserController**

- **UserController** is the entry point for all HTTP requests related to the `User` entity.
- It exposes the following endpoints:
  - `GET /api/users` – Retrieve all users.
  - `GET /api/users/{id}` – Retrieve a user by ID.
  - `POST /api/users` – Create a new user.
  - `PUT /api/users/{id}` – Update an existing user.
  - `DELETE /api/users/{id}` – Delete a user by ID.

---

**Example: UserController Class**
```java
package com.example.demo.controller;

import com.example.demo.model.User;
import com.example.demo.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/users")
public class UserController {

    @Autowired
    private UserService userService;

    // Get all users
    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }

    // Get user by ID
    @GetMapping("/{id}")
    public ResponseEntity<User> getUserById(@PathVariable Long id) {
        return userService.getUserById(id)
            .map(ResponseEntity::ok)
            .orElse(ResponseEntity.notFound().build());
    }

    // Create a new user
    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User createdUser = userService.createUser(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(createdUser);
    }

    // Update an existing user
    @PutMapping("/{id}")
    public ResponseEntity<User> updateUser(@PathVariable Long id, @RequestBody User userDetails) {
        User updatedUser = userService.updateUser(id, userDetails);
        return ResponseEntity.ok(updatedUser);
    }

    // Delete a user
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        userService.deleteUser(id);
        return ResponseEntity.noContent().build();
    }
}
```

---

### **Explanation:**
- **@RestController** – Defines this class as a REST controller.
- **@RequestMapping("/api/users")** – Base URI for all user-related endpoints.
- **@GetMapping, @PostMapping, @PutMapping, @DeleteMapping** – Map HTTP methods to controller methods.
- **@PathVariable** – Extracts dynamic URI values.
- **@RequestBody** – Maps the request body to the User object.
- **ResponseEntity** – Configures the HTTP response status and payload.

---

### **5.2.6.2 CORS Configuration**

- Since Angular and Spring Boot run on different ports, we need to configure **CORS** to allow cross-origin requests.
- We will allow Angular (`http://localhost:4200`) to communicate with Spring Boot (`http://localhost:8080`).

**Example: Global CORS Configuration**
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig {

    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/**").allowedOrigins("http://localhost:4200");
            }
        };
    }
}
```

- **@Configuration** – Registers this as a configuration class.
- **addMapping("/**")** – Applies CORS to all endpoints.
- **allowedOrigins("http://localhost:4200")** – Allows requests from Angular's development server.

---

## **5.3 Frontend: Angular CRUD Integration**

---

### **5.3.1 Setting up Angular Project for CRUD Integration**

---

#### **5.3.1.1 Create a New Angular Project**

```sh
# Create a new Angular project
ng new angular-springboot-frontend

# Navigate to the project folder
cd angular-springboot-frontend

# Serve the Angular app
ng serve
```

- **ng new** – Creates a new Angular project.
- **ng serve** – Runs the application on `http://localhost:4200`.

---

#### **5.3.1.2 Installing Angular HttpClient Module**

- Import `HttpClientModule` in `app.module.ts`.

```typescript
// src/app/app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { HttpClientModule } from '@angular/common/http';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    HttpClientModule // <-- Import HttpClientModule here
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

- **HttpClientModule** – Enables Angular to communicate with REST APIs.

---

### **5.3.2 Angular Service: UserService**

- Centralized service for communicating with Spring Boot backend.
- Handles all HTTP requests (`GET`, `POST`, `PUT`, `DELETE`).

---

**Example: UserService with Full CRUD Operations**
```typescript
// src/app/services/user.service.ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpHeaders, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';
import { User } from '../models/user.model';

@Injectable({
  providedIn: 'root'
})
export class UserService {

  private apiUrl = 'http://localhost:8080/api/users';
  private httpOptions = {
    headers: new HttpHeaders({ 'Content-Type': 'application/json' })
  };

  constructor(private http: HttpClient) { }

  // Get all users
  getAllUsers(): Observable<User[]> {
    return this.http.get<User[]>(this.apiUrl).pipe(
      catchError(this.handleError)
    );
  }

  // Get user by ID
  getUserById(id: number): Observable<User> {
    return this.http.get<User>(`${this.apiUrl}/${id}`).pipe(
      catchError(this.handleError)
    );
  }

  // Create a new user
  createUser(user: User): Observable<User> {
    return this.http.post<User>(this.apiUrl, user, this.httpOptions).pipe(
      catchError(this.handleError)
    );
  }

  // Update an existing user
  updateUser(id: number, user: User): Observable<User> {
    return this.http.put<User>(`${this.apiUrl}/${id}`, user, this.httpOptions).pipe(
      catchError(this.handleError)
    );
  }

  // Delete a user
  deleteUser(id: number): Observable<void> {
    return this.http.delete<void>(`${this.apiUrl}/${id}`).pipe(
      catchError(this.handleError)
    );
  }

  // Error handling
  private handleError(error: HttpErrorResponse) {
    if (error.error instanceof ErrorEvent) {
      console.error('An error occurred:', error.error.message);
    } else {
      console.error(`Backend returned code ${error.status}, body was: ${error.error}`);
    }
    return throwError('Something went wrong; please try again later.');
  }
}
```

- **Observable<User[]>** – Returns an array of `User` objects.
- **catchError()** – Handles HTTP errors using **RxJS**.

---

## **Next Steps:**
- **5.3.3 Angular Components for CRUD Operations**
  - User List, User Create, and User Edit Components.
  - Form validation and error handling.
  - Routing and Navigation for CRUD operations.

---

## **Lesson 72 (Continued): Angular Components for CRUD Operations**

---

## **5.3.3 Angular Components for CRUD Operations**

In this section, we will:
- Create **Angular Components** for User Management:
  - **User List Component** – Display all users.
  - **User Create Component** – Add a new user.
  - **User Edit Component** – Edit an existing user.
- Implement **Reactive Forms** for:
  - **Two-way data binding** with `FormGroup` and `FormControl`.
  - **Form validation** and **error handling**.
- Manage **Routing and Navigation** between the components.
- Display dynamic data using **Angular Directives (`ngFor`, `ngIf`)**.

---

## **5.3.3.1 Generating Angular Components**

```sh
# Generate components for CRUD operations
ng generate component components/user-list
ng generate component components/user-create
ng generate component components/user-edit
```

- **ng generate component** – Creates new Angular components in `src/app/components/`.

---

## **5.3.3.2 User List Component**

- **User List Component** is responsible for:
  - Displaying all users in a table.
  - Providing **Edit** and **Delete** options for each user.
  - Navigating to **Create** and **Edit** pages.

---

### **Template: user-list.component.html**
```html
<h2>User List</h2>
<button routerLink="/users/create">Add New User</button>

<table>
  <thead>
    <tr>
      <th>ID</th>
      <th>Name</th>
      <th>Email</th>
      <th>Actions</th>
    </tr>
  </thead>
  <tbody>
    <tr *ngFor="let user of users; trackBy: trackByUserId">
      <td>{{ user.id }}</td>
      <td>{{ user.name }}</td>
      <td>{{ user.email }}</td>
      <td>
        <button (click)="editUser(user.id)">Edit</button>
        <button (click)="deleteUser(user.id)">Delete</button>
      </td>
    </tr>
  </tbody>
</table>
```

- **ngFor** – Loops through `users` array to display each user.
- **trackByUserId** – Optimizes performance by tracking users by ID.
- **routerLink** – Navigates to the **Create** component.

---

### **Component Class: user-list.component.ts**
```typescript
import { Component, OnInit } from '@angular/core';
import { User } from '../../models/user.model';
import { UserService } from '../../services/user.service';
import { Router } from '@angular/router';

@Component({
  selector: 'app-user-list',
  templateUrl: './user-list.component.html',
  styleUrls: ['./user-list.component.css']
})
export class UserListComponent implements OnInit {

  users: User[] = [];

  constructor(private userService: UserService, private router: Router) { }

  ngOnInit(): void {
    this.loadUsers();
  }

  loadUsers(): void {
    this.userService.getAllUsers().subscribe(
      data => this.users = data,
      error => console.error('Error loading users:', error)
    );
  }

  editUser(id: number): void {
    this.router.navigate(['/users/edit', id]);
  }

  deleteUser(id: number): void {
    this.userService.deleteUser(id).subscribe(
      () => this.loadUsers(),
      error => console.error('Error deleting user:', error)
    );
  }

  trackByUserId(index: number, user: User): number {
    return user.id;
  }
}
```

- **ngOnInit()** – Loads users when the component is initialized.
- **editUser()** – Navigates to the **Edit** component.
- **deleteUser()** – Deletes a user and reloads the list.
- **trackByUserId()** – Improves performance by tracking users by unique ID.

---

## **5.3.3.3 User Create Component**

- **User Create Component** is responsible for:
  - Displaying a form to create a new user.
  - **Two-way data binding** with `Reactive Forms`.
  - **Form validation** and error handling.

---

### **Template: user-create.component.html**
```html
<h2>Add New User</h2>
<form [formGroup]="userForm" (ngSubmit)="onSubmit()">
  <label for="name">Name:</label>
  <input id="name" formControlName="name">
  <div *ngIf="userForm.get('name')?.invalid && userForm.get('name')?.touched">
    Name is required.
  </div>

  <label for="email">Email:</label>
  <input id="email" formControlName="email">
  <div *ngIf="userForm.get('email')?.invalid && userForm.get('email')?.touched">
    Valid email is required.
  </div>

  <label for="password">Password:</label>
  <input type="password" id="password" formControlName="password">
  <div *ngIf="userForm.get('password')?.invalid && userForm.get('password')?.touched">
    Password is required.
  </div>

  <button type="submit" [disabled]="userForm.invalid">Save</button>
</form>
```

- **Reactive Forms**:
  - `[formGroup]="userForm"` – Binds the form to `userForm` object.
  - `formControlName` – Binds input fields to `FormControl` properties.
- **Form Validation**:
  - Displays error messages if fields are **touched** and **invalid**.

---

### **Component Class: user-create.component.ts**
```typescript
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { Router } from '@angular/router';
import { UserService } from '../../services/user.service';

@Component({
  selector: 'app-user-create',
  templateUrl: './user-create.component.html',
  styleUrls: ['./user-create.component.css']
})
export class UserCreateComponent implements OnInit {

  userForm!: FormGroup;

  constructor(private fb: FormBuilder, private userService: UserService, private router: Router) { }

  ngOnInit(): void {
    this.userForm = this.fb.group({
      name: ['', Validators.required],
      email: ['', [Validators.required, Validators.email]],
      password: ['', Validators.required]
    });
  }

  onSubmit(): void {
    if (this.userForm.valid) {
      this.userService.createUser(this.userForm.value).subscribe(
        () => this.router.navigate(['/users']),
        error => console.error('Error creating user:', error)
      );
    }
  }
}
```

- **Reactive Forms**:
  - **FormGroup** – Group of form controls.
  - **FormBuilder** – Simplifies form creation.
  - **Validators** – Built-in validation rules:
    - `Validators.required`
    - `Validators.email`
- **onSubmit()** – Handles form submission and navigates to User List.

---

## **5.3.3.4 Angular Routing for CRUD Navigation**

**Example: app-routing.module.ts**
```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { UserListComponent } from './components/user-list/user-list.component';
import { UserCreateComponent } from './components/user-create/user-create.component';
import { UserEditComponent } from './components/user-edit/user-edit.component';

const routes: Routes = [
  { path: '', redirectTo: '/users', pathMatch: 'full' },
  { path: 'users', component: UserListComponent },
  { path: 'users/create', component: UserCreateComponent },
  { path: 'users/edit/:id', component: UserEditComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

- **path: 'users'** – Navigates to the User List component.
- **path: 'users/create'** – Navigates to the User Create component.
- **path: 'users/edit/:id'** – Navigates to the User Edit component with a dynamic ID.

---

## **Next Steps:**
- **5.3.4 User Edit Component**
- **5.4 Full-Stack Integration and Deployment**

---

## **Lesson 72 (Continued): User Edit Component and Full-Stack Integration**

---

## **5.3.4 User Edit Component**

In this section, we will:
- Implement **User Edit Component** to:
  - **Fetch existing user data** by ID.
  - **Populate the form** with existing data for editing.
  - **Update the user data** using Angular's HttpClient.
  - **Handle form validation** and **error messages**.
- Integrate with **UserService** for **API communication**.
- Utilize **Reactive Forms** for two-way data binding and validation.

---

### **5.3.4.1 User Edit Component Overview**

- **User Edit Component** is responsible for:
  - Fetching the existing user data by ID.
  - Displaying the data in a pre-filled form.
  - Submitting the updated data to the Spring Boot backend.
  - Navigating back to the **User List** after successful update.

---

### **5.3.4.2 Template: user-edit.component.html**

```html
<h2>Edit User</h2>
<form [formGroup]="userForm" (ngSubmit)="onSubmit()">
  <label for="name">Name:</label>
  <input id="name" formControlName="name">
  <div *ngIf="userForm.get('name')?.invalid && userForm.get('name')?.touched">
    Name is required.
  </div>

  <label for="email">Email:</label>
  <input id="email" formControlName="email">
  <div *ngIf="userForm.get('email')?.invalid && userForm.get('email')?.touched">
    Valid email is required.
  </div>

  <label for="password">Password:</label>
  <input type="password" id="password" formControlName="password">
  <div *ngIf="userForm.get('password')?.invalid && userForm.get('password')?.touched">
    Password is required.
  </div>

  <button type="submit" [disabled]="userForm.invalid">Update</button>
  <button type="button" (click)="cancel()">Cancel</button>
</form>
```

- **Reactive Forms**:
  - `[formGroup]="userForm"` – Binds the form to `userForm` object.
  - `formControlName` – Binds input fields to `FormControl` properties.
- **Form Validation**:
  - Displays error messages if fields are **touched** and **invalid**.
- **Cancel Button**:
  - Navigates back to the User List without saving changes.

---

### **5.3.4.3 Component Class: user-edit.component.ts**

```typescript
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { ActivatedRoute, Router } from '@angular/router';
import { UserService } from '../../services/user.service';
import { User } from '../../models/user.model';

@Component({
  selector: 'app-user-edit',
  templateUrl: './user-edit.component.html',
  styleUrls: ['./user-edit.component.css']
})
export class UserEditComponent implements OnInit {

  userForm!: FormGroup;
  userId!: number;

  constructor(
    private fb: FormBuilder,
    private userService: UserService,
    private route: ActivatedRoute,
    private router: Router
  ) { }

  ngOnInit(): void {
    this.userId = Number(this.route.snapshot.paramMap.get('id'));
    this.userForm = this.fb.group({
      name: ['', Validators.required],
      email: ['', [Validators.required, Validators.email]],
      password: ['', Validators.required]
    });
    this.loadUser();
  }

  loadUser(): void {
    this.userService.getUserById(this.userId).subscribe(
      (data: User) => this.userForm.patchValue(data),
      error => console.error('Error loading user:', error)
    );
  }

  onSubmit(): void {
    if (this.userForm.valid) {
      this.userService.updateUser(this.userId, this.userForm.value).subscribe(
        () => this.router.navigate(['/users']),
        error => console.error('Error updating user:', error)
      );
    }
  }

  cancel(): void {
    this.router.navigate(['/users']);
  }
}
```

- **ActivatedRoute** – Accesses route parameters (e.g., user ID).
- **FormGroup** – Manages form controls.
- **FormBuilder** – Simplifies form creation.
- **patchValue()** – Populates form fields with existing user data.
- **onSubmit()** – Handles form submission and updates the user.
- **cancel()** – Navigates back to the User List without saving.

---

### **5.3.4.4 Explanation:**
- **ngOnInit()**:
  - Extracts the `id` from the route parameters.
  - Initializes the form with validation rules.
  - Calls `loadUser()` to fetch existing user data.
- **loadUser()**:
  - Uses `getUserById()` from `UserService` to fetch user data.
  - Populates the form using `patchValue()`.
- **onSubmit()**:
  - Checks if the form is valid.
  - Uses `updateUser()` from `UserService` to send the updated data to the backend.
  - Navigates back to the User List upon success.

---

## **5.4 Full-Stack Integration and Deployment**

---

### **5.4.1 Integrating Angular and Spring Boot**

- **Frontend (Angular)**:
  - **User Service** for API communication.
  - **Components** for User List, Create, Edit.
  - **Reactive Forms** for form validation.
  - **Routing** for navigation between pages.

- **Backend (Spring Boot)**:
  - **Controller Layer** – Manages HTTP requests and responses.
  - **Service Layer** – Business logic.
  - **Repository Layer** – Data persistence using Spring Data JPA.
  - **CORS Configuration** – Allows communication with Angular frontend.

---

### **5.4.2 Running the Application**

1. **Start the Spring Boot Backend**:
```sh
# Navigate to the Spring Boot project directory
cd springboot-backend

# Build and run the Spring Boot application
mvn spring-boot:run
```

- The backend will be running on `http://localhost:8080`.

2. **Start the Angular Frontend**:
```sh
# Navigate to the Angular project directory
cd angular-springboot-frontend

# Serve the Angular app
ng serve
```

- The frontend will be running on `http://localhost:4200`.

---

### **5.4.3 Testing the Full-Stack Application**

- Open a browser and navigate to `http://localhost:4200`.
- Test the following functionalities:
  - **List Users** – Displays all users.
  - **Create User** – Adds a new user and navigates back to the list.
  - **Edit User** – Updates an existing user's information.
  - **Delete User** – Deletes a user from the list.

---

### **5.4.4 Deployment Options**

- **Local Deployment**:
  - Use **Spring Boot's embedded Tomcat server**.
  - Serve Angular's static files using **ng build --prod**.
- **Cloud Deployment**:
  - Deploy using **Docker** containers.
  - Host on **AWS EC2**, **Google Cloud App Engine**, or **Azure App Services**.

---

## **Next Steps:**
- **Lesson 73: JWT Authentication and Authorization**
  - Implementing JWT Security with Spring Boot.
  - Integrating JWT Authentication with Angular.
  - Role-Based Authorization and Secure API Calls.
