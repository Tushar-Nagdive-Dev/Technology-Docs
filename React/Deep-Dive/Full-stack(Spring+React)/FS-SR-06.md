## 📘 Phase 2 – Lesson 2: React Login with JWT + Token Handling + Protected Routes

---

### 🎯 Goal:

* Build a login form using **Formik + Yup**
* Authenticate with Spring Boot API
* Store token in **Redux Toolkit** or **Zustand**
* Send token in Axios requests
* Protect frontend routes based on login status

---

## 🛠 Prerequisites

✅ Backend running at: `http://localhost:8080`
✅ Endpoint available: `POST /api/auth/login`

---

## 🧩 Step 1: Set Up Axios Client with Auth Token

📄 `src/api/axios.js`

```jsx
import axios from 'axios';

const api = axios.create({
  baseURL: 'http://localhost:8080/api',
});

api.interceptors.request.use((config) => {
  const token = localStorage.getItem('token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

export default api;
```

---

## 🧩 Step 2: Create Redux Slice for Auth (or Zustand if preferred)

📄 `features/auth/authSlice.js`

```jsx
import { createSlice } from '@reduxjs/toolkit';

const initialState = {
  token: localStorage.getItem('token'),
  user: null,
  isAuthenticated: !!localStorage.getItem('token')
};

const authSlice = createSlice({
  name: 'auth',
  initialState,
  reducers: {
    loginSuccess(state, action) {
      state.token = action.payload.token;
      state.user = { role: action.payload.role };
      state.isAuthenticated = true;
      localStorage.setItem('token', action.payload.token);
    },
    logout(state) {
      state.token = null;
      state.user = null;
      state.isAuthenticated = false;
      localStorage.removeItem('token');
    }
  }
});

export const { loginSuccess, logout } = authSlice.actions;
export default authSlice.reducer;
```

📄 `store.js`

```jsx
import { configureStore } from '@reduxjs/toolkit';
import authReducer from './features/auth/authSlice';

export const store = configureStore({
  reducer: {
    auth: authReducer
  }
});
```

Wrap your app in `Provider` (if not already):

📄 `main.jsx`

```jsx
<Provider store={store}>
  <App />
</Provider>
```

---

## 🧩 Step 3: Create Login Form with Formik + Yup

📄 `pages/Login.jsx`

```jsx
import { Formik, Form, Field, ErrorMessage } from 'formik';
import * as Yup from 'yup';
import api from '../api/axios';
import { useDispatch } from 'react-redux';
import { loginSuccess } from '../features/auth/authSlice';

export default function Login() {
  const dispatch = useDispatch();

  return (
    <div>
      <h2>Login</h2>
      <Formik
        initialValues={{ email: '', password: '' }}
        validationSchema={Yup.object({
          email: Yup.string().email('Invalid email').required('Required'),
          password: Yup.string().min(6).required('Required')
        })}
        onSubmit={async (values, { setSubmitting, setErrors }) => {
          try {
            const res = await api.post('/auth/login', values);
            dispatch(loginSuccess(res.data));
          } catch (err) {
            setErrors({ email: 'Invalid credentials' });
          } finally {
            setSubmitting(false);
          }
        }}
      >
        <Form>
          <div>
            <Field name="email" placeholder="Email" />
            <ErrorMessage name="email" component="div" />
          </div>
          <div>
            <Field name="password" type="password" placeholder="Password" />
            <ErrorMessage name="password" component="div" />
          </div>
          <button type="submit">Login</button>
        </Form>
      </Formik>
    </div>
  );
}
```

---

## 🧩 Step 4: Protect Frontend Routes

📄 `components/ProtectedRoute.jsx`

```jsx
import { Navigate } from 'react-router-dom';
import { useSelector } from 'react-redux';

export default function ProtectedRoute({ children }) {
  const isAuth = useSelector((state) => state.auth.isAuthenticated);
  return isAuth ? children : <Navigate to="/login" />;
}
```

---

📄 `App.jsx`

```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import Dashboard from './pages/Dashboard';
import Login from './pages/Login';
import ProtectedRoute from './components/ProtectedRoute';

<BrowserRouter>
  <Routes>
    <Route path="/login" element={<Login />} />
    <Route
      path="/dashboard"
      element={
        <ProtectedRoute>
          <Dashboard />
        </ProtectedRoute>
      }
    />
  </Routes>
</BrowserRouter>
```

---

## 🧪 Test the Full Flow

1. ✅ Try accessing `/dashboard` — should redirect to `/login`
2. ✅ Login with valid credentials (from Spring Boot DB)
3. ✅ You should see the dashboard, token saved in localStorage
4. ✅ Open DevTools → check `Authorization` header in Axios

---

## ✅ Summary

| Feature          | Result                               |
| ---------------- | ------------------------------------ |
| Login form       | Validated & connected to backend     |
| JWT token        | Stored securely and sent on requests |
| Auth state       | Managed with Redux                   |
| Protected routes | Access controlled based on login     |

---

## 🎯 What’s Next?

👉 **Phase 2 – Lesson 3: Role-Based Authorization (Frontend + Backend)**

You’ll:

* Show/hide UI based on roles (Admin/User)
* Protect backend endpoints with role checks
* Build role-based dashboard sections
