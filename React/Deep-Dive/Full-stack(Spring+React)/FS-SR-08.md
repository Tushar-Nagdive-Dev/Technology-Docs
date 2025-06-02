## 📘 Phase 2 – Lesson 4: JWT Filter + Complete Spring Security Configuration

---

### 🎯 Goal:

* Create a JWT authentication filter
* Integrate it with Spring Security filter chain
* Automatically extract user from token
* Enable secure, stateless requests

---

## 🧩 Step 1: Create Custom JWT Filter

📄 `JwtAuthenticationFilter.java`

```java
@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {

  @Autowired private JwtUtil jwtUtil;
  @Autowired private UserRepository userRepo;

  @Override
  protected void doFilterInternal(HttpServletRequest request,
                                  HttpServletResponse response,
                                  FilterChain filterChain) throws ServletException, IOException {
    final String authHeader = request.getHeader("Authorization");

    if (authHeader == null || !authHeader.startsWith("Bearer ")) {
      filterChain.doFilter(request, response);
      return;
    }

    final String token = authHeader.substring(7);
    final String email = jwtUtil.extractEmail(token);

    if (email != null && SecurityContextHolder.getContext().getAuthentication() == null) {
      User user = userRepo.findByEmail(email).orElse(null);
      if (user != null) {
        UsernamePasswordAuthenticationToken auth = new UsernamePasswordAuthenticationToken(
            user, null, List.of(new SimpleGrantedAuthority("ROLE_" + user.getRole().name()))
        );
        SecurityContextHolder.getContext().setAuthentication(auth);
      }
    }

    filterChain.doFilter(request, response);
  }
}
```

---

## 🧩 Step 2: Update Spring Security Config

📄 `SecurityConfig.java`

```java
@Configuration
@EnableMethodSecurity
public class SecurityConfig {

  @Autowired private JwtAuthenticationFilter jwtFilter;

  @Bean
  public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    http.csrf().disable()
        .authorizeHttpRequests()
        .requestMatchers("/api/auth/**", "/swagger-ui/**", "/v3/api-docs/**").permitAll()
        .anyRequest().authenticated()
        .and()
        .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
        .and()
        .addFilterBefore(jwtFilter, UsernamePasswordAuthenticationFilter.class);

    return http.build();
  }

  @Bean
  public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
  }
}
```

---

## 🧩 Step 3: Verify Security Context Works

Anywhere in your controllers:

```java
@GetMapping("/me")
public ResponseEntity<?> getCurrentUser(Authentication authentication) {
    User user = (User) authentication.getPrincipal(); // if using custom User
    return ResponseEntity.ok(user.getEmail());
}
```

---

## 📄 Optional: Custom `UserDetailsService` (Advanced)

If needed, implement `UserDetailsService` and wrap your `User` entity — but in our current example, we manually set the `Authentication` in the filter, so it works fine for now.

---

## ✅ Full Security Flow:

1. Frontend sends token in `Authorization` header
2. Filter extracts and validates JWT
3. Loads user and sets it into Spring’s security context
4. Controller methods can access user and role

---

## 🧪 Test Cases

✅ Call protected endpoint with valid token → ✅ success
✅ Call without token → ❌ 403 Forbidden
✅ Try `@PreAuthorize("hasRole('ADMIN')")` → ⛔ if role mismatches
✅ Try accessing `Authentication` in controller → ✅ user extracted

---

## 🧠 Summary

| Feature               | Implemented                    |
| --------------------- | ------------------------------ |
| JWT filter            | Custom Spring filter           |
| Stateless auth        | ✅                              |
| Role injection        | ✅ via `SimpleGrantedAuthority` |
| Token validation      | On every request               |
| SecurityContext usage | Available in endpoints         |

---

## 🎯 What’s Next?

👉 **Phase 3 – Lesson 1: Connecting Frontend to Protected APIs + Logout Flow**

We’ll:

* Call secured backend APIs with token
* Display user info in header or dashboard
* Add logout flow to clear auth state
