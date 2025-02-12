# 🚀 **Phase 3 - Lesson 22: Advanced Spring Boot Security (2FA, CSRF, Encryption, Keycloak)**  

## **📌 Lesson Objective**  
By the end of this lesson, you will:  
✅ Implement **Two-Factor Authentication (2FA) with Google Authenticator**  
✅ Secure APIs against **Cross-Site Request Forgery (CSRF) attacks**  
✅ Encrypt **sensitive data using Spring Security Crypto Module**  
✅ Use **Keycloak for Single Sign-On (SSO) & OAuth2 Authorization**  
✅ Apply **advanced security best practices**  

---

# 🔑 **Part 1: Implementing Two-Factor Authentication (2FA) in Spring Boot**  

## **1️⃣ What is 2FA & Why Use It?**  
📌 **Two-Factor Authentication (2FA)** requires users to provide **two different factors** to authenticate, making it much more secure than passwords alone.  

### **🔥 2FA Methods**
✔ **SMS OTP (One-Time Password)**  
✔ **Google Authenticator / Authy (TOTP - Time-based OTPs)**  
✔ **Email OTP**  
✔ **Hardware Security Keys (YubiKey, FIDO2, etc.)**  

✅ **Use 2FA when:**  
- You need **stronger security for login & sensitive actions**.  
- You want **to prevent unauthorized access due to password leaks**.  

---

## **2️⃣ Adding 2FA Using Google Authenticator**  
📌 **Step 1: Add Google Authenticator & Spring Security Dependencies**  
```xml
<dependency>
    <groupId>com.warrenstrange</groupId>
    <artifactId>googleauth</artifactId>
    <version>1.4.0</version>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

📌 **Step 2: Generate 2FA QR Code for Users**  
```java
package com.example.demo.service;

import com.warrenstrange.googleauth.GoogleAuthenticator;
import com.warrenstrange.googleauth.GoogleAuthenticatorKey;
import org.springframework.stereotype.Service;

@Service
public class TwoFactorAuthService {
    
    private final GoogleAuthenticator gAuth = new GoogleAuthenticator();

    public String generateSecretKey() {
        GoogleAuthenticatorKey key = gAuth.createCredentials();
        return key.getKey();
    }

    public boolean validateOTP(String secretKey, int otp) {
        return gAuth.authorize(secretKey, otp);
    }
}
```

📌 **Step 3: Expose API for Enabling 2FA (`AuthController.java`)**  
```java
package com.example.demo.controller;

import com.example.demo.service.TwoFactorAuthService;
import org.springframework.web.bind.annotation.*;

import java.util.Map;

@RestController
@RequestMapping("/api/auth")
public class AuthController {

    private final TwoFactorAuthService twoFactorAuthService;

    public AuthController(TwoFactorAuthService twoFactorAuthService) {
        this.twoFactorAuthService = twoFactorAuthService;
    }

    @PostMapping("/enable-2fa")
    public Map<String, String> enable2FA() {
        String secretKey = twoFactorAuthService.generateSecretKey();
        return Map.of("secretKey", secretKey);
    }

    @PostMapping("/validate-2fa")
    public boolean validate2FA(@RequestParam String secretKey, @RequestParam int otp) {
        return twoFactorAuthService.validateOTP(secretKey, otp);
    }
}
```

📌 **Step 4: Test 2FA Integration**
1️⃣ **Get Secret Key:**  
```bash
curl -X POST "http://localhost:8080/api/auth/enable-2fa"
```
✔ **Response:**  
```json
{"secretKey": "JBSWY3DPEHPK3PXP"}
```

2️⃣ **Scan the Secret Key in Google Authenticator App**  
3️⃣ **Get a 6-digit OTP from the app.**  

4️⃣ **Validate OTP:**  
```bash
curl -X POST "http://localhost:8080/api/auth/validate-2fa?secretKey=JBSWY3DPEHPK3PXP&otp=123456"
```
✔ **Response (if valid):** `true`  
✔ **Response (if invalid):** `false`  

🎉 **2FA is now enabled for your Spring Boot app!** 🔐  

---

# 🛡 **Part 2: Preventing Cross-Site Request Forgery (CSRF) Attacks**  

## **3️⃣ What is CSRF & Why is It Dangerous?**  
📌 **Cross-Site Request Forgery (CSRF)** occurs when attackers trick users into submitting **unauthorized requests** to a web application.  

✅ **Enable CSRF Protection when:**  
- Your app has **stateful sessions (JSESSIONID cookies)**.  
- You use **traditional form-based authentication**.  

📌 **Step 1: Enable CSRF Protection in `SecurityConfig.java`**  
```java
package com.example.demo.security;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.web.csrf.CookieCsrfTokenRepository;

@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http.csrf(csrf -> csrf.csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse()))
            .authorizeHttpRequests(auth -> auth.anyRequest().authenticated());

        return http.build();
    }
}
```

📌 **Step 2: Send CSRF Token in API Calls**  
```html
<form action="/submit-form" method="POST">
    <input type="hidden" name="_csrf" value="${_csrf.token}"/>
    <button type="submit">Submit</button>
</form>
```

🎉 **Now your app is protected from CSRF attacks!** 🛡  

---

# 🔑 **Part 3: Encrypting Sensitive Data in Spring Boot**  

## **4️⃣ Encrypting User Data Using Spring Security Crypto Module**  
📌 **Step 1: Add Encryption Library (`pom.xml`)**  
```xml
<dependency>
    <groupId>org.jasypt</groupId>
    <artifactId>jasypt-spring-boot-starter</artifactId>
    <version>3.0.5</version>
</dependency>
```

📌 **Step 2: Encrypt Passwords Securely (`EncryptionService.java`)**  
```java
package com.example.demo.security;

import org.jasypt.util.text.BasicTextEncryptor;
import org.springframework.stereotype.Service;

@Service
public class EncryptionService {
    private final BasicTextEncryptor encryptor = new BasicTextEncryptor();

    public EncryptionService() {
        encryptor.setPassword("super-secret-key");
    }

    public String encrypt(String data) {
        return encryptor.encrypt(data);
    }

    public String decrypt(String encryptedData) {
        return encryptor.decrypt(encryptedData);
    }
}
```

📌 **Step 3: Encrypt & Decrypt Sensitive Data**  
```bash
curl -X POST "http://localhost:8080/api/encrypt?data=mySensitiveData"
```
✔ **Response:** `"ENC(gsd93jdkslkfjs90skdf)"`  

```bash
curl -X POST "http://localhost:8080/api/decrypt?data=ENC(gsd93jdkslkfjs90skdf)"
```
✔ **Response:** `"mySensitiveData"`  

🎉 **Your app now securely encrypts sensitive data!** 🔐  

---

# 🔐 **Part 4: Securing Spring Boot with Keycloak (Single Sign-On - SSO)**  

## **5️⃣ Setting Up Keycloak for OAuth2 Authentication**  
📌 **Step 1: Install & Start Keycloak in Docker**  
```bash
docker run -d --name keycloak -p 8081:8080 \
    -e KEYCLOAK_ADMIN=admin \
    -e KEYCLOAK_ADMIN_PASSWORD=admin \
    quay.io/keycloak/keycloak:latest start-dev
```

📌 **Step 2: Configure Spring Boot to Use Keycloak**  
```yaml
spring:
  security:
    oauth2:
      client:
        registration:
          keycloak:
            client-id: spring-app
            client-secret: your-client-secret
            scope: openid, profile, email
        provider:
          keycloak:
            issuer-uri: http://localhost:8081/realms/master
```

📌 **Step 3: Secure Spring Boot Endpoints with Keycloak**  
```java
http.oauth2Login().and().authorizeHttpRequests(auth -> auth.anyRequest().authenticated());
```

🎉 **Spring Boot now supports Keycloak-based OAuth2 authentication!** 🚀  

---

## 🎯 **Lesson 22 - Summary**  
✅ Implemented **2FA (Google Authenticator) in Spring Boot**  
✅ Secured APIs against **CSRF attacks**  
✅ Encrypted **sensitive user data**  
✅ Used **Keycloak for Single Sign-On (SSO)**  

---
