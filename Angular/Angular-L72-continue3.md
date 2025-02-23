---

## **Lesson 72 (Continued): Integrating Angular with Spring Boot APIs**

---

## **4. Integrating Angular with Spring Boot APIs**

In this section, we will:
- **Integrate Angular Frontend** with **Spring Boot Backend**.
- Use **Angular's HttpClient Module** for making API calls.
- Implement **full CRUD operations** (Create, Read, Update, Delete).
- Display data using **Angular Components** and **Services**.

---

### **4.1 Setting up Angular Project for API Integration**

---

#### **4.1.1 Creating a New Angular Project**

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

#### **4.1.2 Installing Angular HttpClient Module**

- **HttpClient** is used to make HTTP requests (GET, POST, PUT, DELETE).
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

### **4.2 Creating Angular Service for API Calls**

#### **4.2.1 Why Use Angular Services for API Calls?**
- **Separation of Concerns**: Keeps components lean by delegating data access to services.
- **Reusability**: Centralized data access logic.
- **Maintainability**: Easier to manage and update APIs.

---

#### **4.2.2 Generating Angular Service**

```sh
# Generate a new service for User management
ng generate service services/user
```

- **ng generate service** – Creates a new service in `src/app/services/user.service.ts`.

---

#### **4.2.3 UserService for CRUD Operations**

**Example: UserService with CRUD Methods**
```typescript
// src/app/services/user.service.ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpHeaders } from '@angular/common/http';
import { Observable } from 'rxjs';
import { User } from '../models/user.model';

@Injectable({
  providedIn: 'root'
})
export class UserService {

  private apiUrl = 'http://localhost:8080/api/users';

  constructor(private http: HttpClient) { }

  // Get all users
  getAllUsers(): Observable<User[]> {
    return this.http.get<User[]>(this.apiUrl);
  }

  // Get user by ID
  getUserById(id: number): Observable<User> {
    return this.http.get<User>(`${this.apiUrl}/${id}`);
  }

  // Create a new user
  createUser(user: User): Observable<User> {
    const headers = new HttpHeaders({ 'Content-Type': 'application/json' });
    return this.http.post<User>(this.apiUrl, user, { headers });
  }

  // Update an existing user
  updateUser(id: number, user: User): Observable<User> {
    const headers = new HttpHeaders({ 'Content-Type': 'application/json' });
    return this.http.put<User>(`${this.apiUrl}/${id}`, user, { headers });
  }

  // Delete a user
  deleteUser(id: number): Observable<void> {
    return this.http.delete<void>(`${this.apiUrl}/${id}`);
  }
}
```

- **@Injectable({ providedIn: 'root' })** – Registers the service at the root level.
- **HttpClient** – Makes HTTP requests.
- **Observable<User[]>** – Returns an array of `User` objects.

---

### **4.3 Creating Angular Components for CRUD Operations**

---

#### **4.3.1 Generating Angular Components**

```sh
# Generate components for CRUD operations
ng generate component components/user-list
ng generate component components/user-create
ng generate component components/user-edit
```

- **ng generate component** – Creates new Angular components for the user module.

---

#### **4.3.2 Displaying User List**

**Example: UserListComponent**

**Template: user-list.component.html**
```html
<h2>User List</h2>
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
    <tr *ngFor="let user of users">
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

**Component Class: user-list.component.ts**
```typescript
import { Component, OnInit } from '@angular/core';
import { User } from '../../models/user.model';
import { UserService } from '../../services/user.service';

@Component({
  selector: 'app-user-list',
  templateUrl: './user-list.component.html',
  styleUrls: ['./user-list.component.css']
})
export class UserListComponent implements OnInit {

  users: User[] = [];

  constructor(private userService: UserService) { }

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
    // Navigate to edit component
  }

  deleteUser(id: number): void {
    this.userService.deleteUser(id).subscribe(
      () => this.loadUsers(),
      error => console.error('Error deleting user:', error)
    );
  }
}
```

- **ngOnInit()** – Lifecycle hook that loads users when the component is initialized.
- **subscribe()** – Subscribes to the Observable to receive data or errors.

---

### **4.4 Angular Routing for CRUD Navigation**

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
- **5. Hands-on Exercise: Building a Complete CRUD Feature**
  - Full-Stack Integration of Angular and Spring Boot
  - Real-world example with detailed implementation
