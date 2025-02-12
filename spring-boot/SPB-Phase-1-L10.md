# 🚀 **Phase 1 - Lesson 10: Sending SMS & OTP Verification in Spring Boot** 🚀  

## **📌 Lesson Objective**
By the end of this lesson, you will:  
✅ Send **SMS messages using Twilio API**  
✅ Generate and send **One-Time Passwords (OTP)**  
✅ Implement **OTP verification** in Spring Boot  
✅ Store OTPs temporarily for validation  

---

## **1️⃣ Why Use SMS & OTP in Applications?**
📌 **Common Use Cases:**  
✔ **User Authentication (2FA - Two-Factor Authentication)** 🔑  
✔ **Password Reset via SMS** 🔄  
✔ **Account Verification on Signup** ✅  
✔ **Transaction Confirmation** 💰  

Spring Boot allows SMS integration using **Twilio**, a cloud-based messaging API.

---

## **2️⃣ Setting Up Twilio for Sending SMS**
### **Step 1: Sign Up for a Twilio Account**
1️⃣ **Go to [Twilio Signup](https://www.twilio.com/try-twilio)**  
2️⃣ **Create an account** and verify your phone number  
3️⃣ **Get Twilio Credentials**:
   - **Account SID**
   - **Auth Token**
   - **Twilio Phone Number**  

---

## **3️⃣ Configuring Twilio in Spring Boot**
📌 **Step 2: Add Twilio Dependency in `pom.xml`**
```xml
<dependency>
    <groupId>com.twilio.sdk</groupId>
    <artifactId>twilio</artifactId>
    <version>9.4.1</version>
</dependency>
```

📌 **Step 3: Configure Twilio Credentials in `application.properties`**
```properties
twilio.account_sid=your_twilio_account_sid
twilio.auth_token=your_twilio_auth_token
twilio.phone_number=your_twilio_phone_number
```

📌 **Step 4: Create Twilio Configuration Class**
Create a new package **`com.example.demo.config`** and add `TwilioConfig.java`:

```java
package com.example.demo.config;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Configuration;

@Configuration
public class TwilioConfig {

    @Value("${twilio.account_sid}")
    private String accountSid;

    @Value("${twilio.auth_token}")
    private String authToken;

    @Value("${twilio.phone_number}")
    private String phoneNumber;

    public String getAccountSid() { return accountSid; }
    public String getAuthToken() { return authToken; }
    public String getPhoneNumber() { return phoneNumber; }
}
```

✅ **Now Twilio credentials are loaded from `application.properties`.**  

---

## **4️⃣ Implementing SMS Sending Service**
📌 **Step 5: Create `SmsService.java`**
Create a new package **`com.example.demo.service`** and add:

```java
package com.example.demo.service;

import com.example.demo.config.TwilioConfig;
import com.twilio.Twilio;
import com.twilio.rest.api.v2010.account.Message;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class SmsService {

    private final TwilioConfig twilioConfig;

    @Autowired
    public SmsService(TwilioConfig twilioConfig) {
        this.twilioConfig = twilioConfig;
        Twilio.init(twilioConfig.getAccountSid(), twilioConfig.getAuthToken());
    }

    public String sendSms(String to, String messageBody) {
        Message message = Message.creator(
                new com.twilio.type.PhoneNumber(to), // Recipient's phone number
                new com.twilio.type.PhoneNumber(twilioConfig.getPhoneNumber()), // Twilio phone number
                messageBody
        ).create();

        return "SMS sent successfully with SID: " + message.getSid();
    }
}
```

✅ **How It Works?**
- **Twilio.init()** → Initializes Twilio API using credentials.
- **Message.creator()** → Sends an SMS to the recipient.
- **Returns an SMS SID (message ID) for tracking.**

---

## **5️⃣ Exposing an API for Sending SMS**
📌 **Step 6: Create `SmsController.java`**
Create a new package **`com.example.demo.controller`** and add:

```java
package com.example.demo.controller;

import com.example.demo.service.SmsService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/sms")
public class SmsController {

    private final SmsService smsService;

    @Autowired
    public SmsController(SmsService smsService) {
        this.smsService = smsService;
    }

    @PostMapping("/send")
    public String sendSms(@RequestParam String phoneNumber, @RequestParam String message) {
        return smsService.sendSms(phoneNumber, message);
    }
}
```

✅ **How It Works?**
- **API Endpoint:** `POST http://localhost:8080/api/sms/send`
- **Request Parameters:**  
  - `phoneNumber` → Recipient’s mobile number  
  - `message` → Text message to send  

---

## **6️⃣ Testing SMS Sending API**
1️⃣ **Start Spring Boot App**
```bash
mvn spring-boot:run
```
2️⃣ **Send SMS using Postman or cURL:**
```bash
curl -X POST "http://localhost:8080/api/sms/send?phoneNumber=+1234567890&message=Hello from Spring Boot!"
```
🎉 **Check your mobile inbox! SMS should be received!** 📩  

---

## **7️⃣ Generating & Sending OTP via SMS**
📌 **Step 7: Modify `SmsService` to Generate OTP**
```java
import java.util.Random;
import java.util.concurrent.ConcurrentHashMap;

@Service
public class SmsService {

    private final TwilioConfig twilioConfig;
    private final ConcurrentHashMap<String, String> otpStorage = new ConcurrentHashMap<>();

    @Autowired
    public SmsService(TwilioConfig twilioConfig) {
        this.twilioConfig = twilioConfig;
        Twilio.init(twilioConfig.getAccountSid(), twilioConfig.getAuthToken());
    }

    public String generateOtp() {
        Random random = new Random();
        int otp = 100000 + random.nextInt(900000); // Generates 6-digit OTP
        return String.valueOf(otp);
    }

    public String sendOtp(String phoneNumber) {
        String otp = generateOtp();
        otpStorage.put(phoneNumber, otp);

        Message message = Message.creator(
                new com.twilio.type.PhoneNumber(phoneNumber),
                new com.twilio.type.PhoneNumber(twilioConfig.getPhoneNumber()),
                "Your OTP is: " + otp
        ).create();

        return "OTP sent successfully to " + phoneNumber;
    }

    public boolean verifyOtp(String phoneNumber, String otp) {
        return otpStorage.containsKey(phoneNumber) && otpStorage.get(phoneNumber).equals(otp);
    }
}
```

✅ **How It Works?**
- `generateOtp()` → Generates a random **6-digit OTP**.
- `sendOtp()` → Sends OTP to the user's phone and stores it in memory.
- `verifyOtp()` → Checks if the entered OTP matches the stored OTP.

---

## **8️⃣ Creating an API for OTP Verification**
📌 **Step 8: Modify `SmsController` for OTP Handling**
```java
@RestController
@RequestMapping("/api/otp")
public class OtpController {

    private final SmsService smsService;

    @Autowired
    public OtpController(SmsService smsService) {
        this.smsService = smsService;
    }

    @PostMapping("/send")
    public String sendOtp(@RequestParam String phoneNumber) {
        return smsService.sendOtp(phoneNumber);
    }

    @PostMapping("/verify")
    public String verifyOtp(@RequestParam String phoneNumber, @RequestParam String otp) {
        boolean isValid = smsService.verifyOtp(phoneNumber, otp);
        return isValid ? "OTP Verified Successfully" : "Invalid OTP";
    }
}
```

✅ **How It Works?**
| API | Endpoint | Description |
|------|---------|-------------|
| `POST /api/otp/send` | Sends OTP via SMS |
| `POST /api/otp/verify` | Verifies user-entered OTP |

---

## **9️⃣ Testing OTP API**
### **Step 1: Send OTP**
```bash
curl -X POST "http://localhost:8080/api/otp/send?phoneNumber=+1234567890"
```
✔ **Check your mobile for OTP message.**  

### **Step 2: Verify OTP**
```bash
curl -X POST "http://localhost:8080/api/otp/verify?phoneNumber=+1234567890&otp=123456"
```
✔ **If correct, response:**
```json
"OTP Verified Successfully"
```
❌ **If incorrect, response:**
```json
"Invalid OTP"
```

🎉 **OTP-based authentication is working!** 🔑  

---

## 🎯 **Lesson 10 - Summary**
✅ Integrated **Twilio SMS API in Spring Boot**  
✅ Sent **custom SMS messages**  
✅ Implemented **OTP generation & verification**  
✅ Created **REST API for SMS & OTP handling**  

---
