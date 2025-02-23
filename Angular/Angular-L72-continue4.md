### **Lesson 72 (Continued): Integrating Angular with Spring Boot APIs** (Deep Dive Edition)

---

## **4. Integrating Angular with Spring Boot APIs**

In this extended section, we will:
- **Deep dive into Angular's HttpClient Module** for API integration.
- Implement **full CRUD operations** (Create, Read, Update, Delete) in Angular.
- **Handle HTTP errors** gracefully in Angular.
- Display data with **Angular Components**, **Services**, and **Pipes**.
- **Optimize performance** with **trackBy** and **async Pipe**.

---

## **4.5 Deep Dive: Angular HttpClient Module**

---

### **4.5.1 What is HttpClient Module?**
- **HttpClient** is an **Angular service** for making HTTP requests.
- It is part of **HttpClientModule** from `@angular/common/http`.
- Uses **Observables** from **RxJS** for asynchronous programming.

---

### **4.5.2 Why Use HttpClient Module?**
- **Asynchronous Operations**: Uses **Observables** for async data flow.
- **Type Safety**: Ensures type safety for request and response payloads.
- **Interceptors**: Intercepts requests/responses for logging, authentication, etc.
- **Error Handling**: Handles HTTP errors with **HttpErrorResponse**.
- **JSON by Default**: Automatically parses JSON responses.

---

### **4.5.3 Setting up HttpClientModule**

```typescript
// src/app/app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { HttpClientModule } from '@angular/common/http';
import { AppComponent } from './app.component';
import { UserListComponent } from './components/user-list/user-list.component';

@NgModule({
  declarations: [
    AppComponent,
    UserListComponent
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
- Must be imported in the **AppModule** or a **Core Module**.

---

### **4.5.4 Making HTTP Requests with HttpClient**
- **GET** – Fetch data from the server.
- **POST** – Create new data on the server.
- **PUT** – Update existing data on the server.
- **DELETE** – Remove data from the server.

---

## **4.6 CRUD Operations with Angular HttpClient**

We will now implement:
- **GET** – Retrieve a list of users and a single user by ID.
- **POST** – Create a new user.
- **PUT** – Update an existing user.
- **DELETE** – Delete a user by ID.

---

### **4.6.1 Service Layer: UserService**

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
      // Client-side or network error
      console.error('An error occurred:', error.error.message);
    } else {
      // Backend returned an unsuccessful response code
      console.error(`Backend returned code ${error.status}, body was: ${error.error}`);
    }
    return throwError('Something went wrong; please try again later.');
  }
}
```

- **getAllUsers()** – Fetches all users.
- **getUserById()** – Fetches a user by ID.
- **createUser()** – Creates a new user.
- **updateUser()** – Updates an existing user.
- **deleteUser()** – Deletes a user by ID.
- **catchError()** – Handles HTTP errors using **RxJS**.

---

## **4.7 Displaying Data in Angular Components**

### **4.7.1 User List Component**

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

  trackByUserId(index: number, user: User): number {
    return user.id;
  }
}
```

- **ngOnInit()** – Loads users when the component is initialized.
- **trackByUserId()** – Improves performance by tracking items by unique ID.
- **deleteUser()** – Deletes a user and reloads the list.

---

### **4.7.2 Performance Optimization with trackBy and async Pipe**
- **trackBy** – Improves performance in `ngFor` loops by tracking items by unique ID.
- **async Pipe** – Subscribes to an Observable and automatically unsubscribes when the component is destroyed.

---

## **Next Steps:**
- **5. Hands-on Exercise: Building a Complete CRUD Feature**
  - Full-Stack Integration of Angular and Spring Boot
  - Real-world example with detailed implementation
