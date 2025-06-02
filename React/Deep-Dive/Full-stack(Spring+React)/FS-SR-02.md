## 📘 Phase 1 – Lesson 2: Project Structure & Tools Setup (Spring Boot + React)

---

### 🎯 Goal:

* Set up a clean, modular Spring Boot backend
* Set up a React (Vite) frontend
* Structure both projects for real-world development
* Prepare for secure full-stack integration

---

## 🔧 Step 1: Create the Backend (Spring Boot)

### ✅ Option A: Using [start.spring.io](https://start.spring.io)

#### Select:

* **Project**: Maven
* **Language**: Java
* **Spring Boot**: Latest stable version
* **Group**: `com.tushar`
* **Artifact**: `user-management-api`
* **Dependencies**:

  * Spring Web
  * Spring Data JPA
  * Spring Boot DevTools
  * PostgreSQL Driver
  * Spring Security (we'll use later)
  * Validation

> 📦 Download and unzip the project, then open in **IntelliJ** or **VS Code**.

---

### ✅ Recommended Backend Structure:

```
user-management-api/
├── src/main/java/com/tushar/api/
│   ├── controller/
│   ├── service/
│   ├── repository/
│   ├── dto/
│   ├── entity/
│   ├── config/
│   └── UserManagementApiApplication.java
├── src/main/resources/
│   ├── application.properties
│   └── db/migration/  (Flyway later)
```

---

## 🧱 Step 2: Create the Frontend (React + Vite)

```bash
npm create vite@latest user-management-ui --template react
cd user-management-ui
npm install
```

Then run:

```bash
npm run dev
```

✅ Your React app runs at `http://localhost:5173`

---

### ✅ Recommended Frontend Structure:

```
user-management-ui/
├── src/
│   ├── components/
│   ├── pages/
│   ├── api/
│   ├── features/
│   ├── store/
│   ├── App.jsx
│   └── main.jsx
├── public/
├── .env
```

---

## 🌐 Step 3: Enable CORS in Spring Boot

📄 In a `CorsConfig.java`:

```java
@Configuration
public class CorsConfig {
    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return registry -> registry.addMapping("/**")
                .allowedOrigins("http://localhost:5173")
                .allowedMethods("*");
    }
}
```

✅ This allows your React app to call the backend.

---

## 🔐 Step 4: Prepare for Authentication (Preview)

We'll later:

* Add JWT filters to Spring Security
* Protect routes like `/api/users`
* Store token in Redux/Zustand in React

---

## 🧪 Exercise:

1. ✅ Run both apps:

   * Spring Boot: `./mvnw spring-boot:run`
   * React: `npm run dev`
2. ✅ Confirm base URLs:

   * Backend: `http://localhost:8080`
   * Frontend: `http://localhost:5173`
3. ✅ Confirm folder structure is clean and scalable
4. ✅ Push to GitHub (initial commit)

---

## ✅ Lesson Recap

| What You Did                  | Why It Matters                 |
| ----------------------------- | ------------------------------ |
| Created Spring Boot backend   | REST API provider              |
| Created React frontend        | UI layer                       |
| Structured projects for scale | Easier to grow app             |
| Enabled CORS                  | Allow full-stack communication |

---

## 🎯 What’s Next?

👉 **Phase 1 – Lesson 3: RESTful API Design + Swagger + TestController**

We’ll:

* Design proper REST APIs (naming, HTTP methods)
* Create your first `UserController`
* Add **Swagger/OpenAPI** for live API docs
* Test APIs with Postman + React
