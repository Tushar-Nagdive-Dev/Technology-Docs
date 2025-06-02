## 📘 Phase 2 – Lesson 3: Role-Based Access Control (RBAC) – Frontend + Backend

---

### 🎯 Goal:

* Restrict access to certain features based on roles (`ADMIN`, `USER`)
* Protect backend routes via `@PreAuthorize`
* Show/hide frontend UI components by role
* Ensure secure role validation on both ends

---

## 🧩 Step 1: Secure Backend Endpoints with Roles

📄 `UserController.java`

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

  @PreAuthorize("hasRole('ADMIN')")
  @GetMapping
  public List<User> getAllUsers() {
    return userRepo.findAll();
  }

  @PreAuthorize("hasAnyRole('ADMIN','USER')")
  @GetMapping("/{id}")
  public ResponseEntity<User> getUser(@PathVariable Long id) {
    return userRepo.findById(id)
      .map(ResponseEntity::ok)
      .orElse(ResponseEntity.notFound().build());
  }
}
```

✅ This ensures:

* Only `ADMIN` can list all users
* Both `ADMIN` and `USER` can access their own data (we'll refine that soon)

---

## 🧩 Step 2: Add Role to JWT & Extract Role from Token

📄 `JwtUtil.java`

Add role claim:

```java
public String generateToken(String email, String role) {
  return Jwts.builder()
    .setSubject(email)
    .claim("role", role)
    .setIssuedAt(new Date())
    .setExpiration(new Date(System.currentTimeMillis() + 86400000))
    .signWith(Keys.hmacShaKeyFor(secret.getBytes()), SignatureAlgorithm.HS256)
    .compact();
}
```

Extract role:

```java
public String extractRole(String token) {
  return Jwts.parserBuilder()
    .setSigningKey(secret.getBytes())
    .build()
    .parseClaimsJws(token)
    .getBody()
    .get("role", String.class);
}
```

📄 `AuthController.java` → when login:

```java
String token = jwtUtil.generateToken(user.getEmail(), user.getRole().name());
```

✅ This embeds the user's role in the token.

---

## 🧩 Step 3: Extract and Validate Role in Spring Security

📄 Add a JWT filter (`JwtAuthenticationFilter.java`)
📄 Register it in `SecurityConfig.java`
💡 (We’ll cover full JWT filter config in next lesson if needed — for now, focus on claim availability and frontend enforcement.)

---

## 🧩 Step 4: Use Role in React (Frontend UI Control)

📄 `authSlice.js` already stores `user.role`

📄 Inside a component:

```jsx
import { useSelector } from 'react-redux';

export default function AdminPanel() {
  const role = useSelector((state) => state.auth.user?.role);

  if (role !== 'ADMIN') {
    return <p>Access Denied</p>;
  }

  return (
    <div>
      <h2>👑 Admin Only Panel</h2>
      {/* Admin-specific UI */}
    </div>
  );
}
```

---

📄 In Navbar (show/hide menu):

```jsx
{role === 'ADMIN' && <Link to="/admin">Admin Dashboard</Link>}
```

---

## 🧠 Summary

| Layer                     | Role Enforcement                                     |
| ------------------------- | ---------------------------------------------------- |
| **Backend (Spring Boot)** | `@PreAuthorize("hasRole('ADMIN')")`                  |
| **JWT**                   | Embed and extract `role`                             |
| **Frontend (React)**      | Use `auth.user.role` to toggle UI & route visibility |

---

### 🧪 Test Cases:

✅ Create one `ADMIN` and one `USER`
✅ Try accessing protected APIs with both
✅ On frontend:

* Login as admin → see admin dashboard
* Login as user → blocked from admin-only pages

---

## 🧪 Bonus Exercise:

Build a `RoleProtectedRoute.jsx`:

```jsx
export default function RoleProtectedRoute({ children, role }) {
  const userRole = useSelector((state) => state.auth.user?.role);
  return userRole === role ? children : <Navigate to="/" />;
}
```

Use like:

```jsx
<Route path="/admin" element={<RoleProtectedRoute role="ADMIN"><AdminDashboard /></RoleProtectedRoute>} />
```

---

## 🎯 What’s Next?

👉 **Phase 2 – Lesson 4: Finalize JWT Filter + Spring Security Config for Token-Based Auth**

We’ll:

* Add `JwtAuthenticationFilter` to validate tokens on every request
* Configure Spring Security properly
* Enable logged-in user extraction for all endpoints
