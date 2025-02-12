# 🚀 **Phase 1 - Lesson 9: Sending Emails with Spring Boot (SMTP, Gmail, HTML Emails)** 🚀  

## **📌 Lesson Objective**
By the end of this lesson, you will:  
✅ Understand how **Spring Boot sends emails** using SMTP  
✅ Set up **Gmail SMTP for sending emails**  
✅ Send **plain text and HTML emails**  
✅ Use **attachments in emails**  
✅ Schedule **automatic email notifications**  

---

## **1️⃣ Why Send Emails in a Spring Boot Application?**
📌 **Common Use Cases:**  
✔ **User Registration Confirmation** 📧  
✔ **Password Reset Emails** 🔑  
✔ **Order Confirmations & Notifications** 🛒  
✔ **Scheduled Reports & Alerts** 📊  

Spring Boot makes email sending **easy** using **JavaMailSender**.

---

## **2️⃣ Setting Up Email Sending in Spring Boot**
📌 **Step 1: Add Mail Dependency in `pom.xml`**
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

📌 **Step 2: Configure SMTP Settings in `application.properties`**  
For **Gmail SMTP**, add:
```properties
# SMTP Configuration
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=your-email@gmail.com
spring.mail.password=your-app-password
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
```

📌 **Important:**  
👉 **Enable "Less Secure Apps" or Use App Password** in Gmail:  
1️⃣ **Go to [Google Account Security](https://myaccount.google.com/security)**  
2️⃣ **Turn ON "Less Secure Apps"** OR **Generate an App Password**  
3️⃣ **Use the App Password instead of your Gmail password**  

---

## **3️⃣ Creating an Email Service in Spring Boot**
📌 **Step 3: Create `EmailService` to Send Emails**
Create a new package **`com.example.demo.service`** and add `EmailService.java`:

```java
package com.example.demo.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.mail.SimpleMailMessage;
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.stereotype.Service;

@Service
public class EmailService {

    private final JavaMailSender mailSender;

    @Autowired
    public EmailService(JavaMailSender mailSender) {
        this.mailSender = mailSender;
    }

    public void sendEmail(String to, String subject, String body) {
        SimpleMailMessage message = new SimpleMailMessage();
        message.setTo(to);
        message.setSubject(subject);
        message.setText(body);
        message.setFrom("your-email@gmail.com"); // Sender Email

        mailSender.send(message);
        System.out.println("Email sent successfully to " + to);
    }
}
```

✅ **How It Works?**
- `JavaMailSender` → Sends emails using SMTP.
- `sendEmail()` → Sends a **simple text email**.

---

## **4️⃣ Sending Emails from a REST API**
📌 **Step 4: Create `EmailController`**
Create a new package **`com.example.demo.controller`** and add `EmailController.java`:

```java
package com.example.demo.controller;

import com.example.demo.service.EmailService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/email")
public class EmailController {

    private final EmailService emailService;

    @Autowired
    public EmailController(EmailService emailService) {
        this.emailService = emailService;
    }

    @PostMapping("/send")
    public String sendEmail(
            @RequestParam String to,
            @RequestParam String subject,
            @RequestParam String body) {
        
        emailService.sendEmail(to, subject, body);
        return "Email sent successfully!";
    }
}
```

✅ **How It Works?**
- **API Endpoint:** `POST http://localhost:8080/api/email/send`
- **Request Parameters:**  
  - `to` → Receiver's email  
  - `subject` → Email subject  
  - `body` → Email content  

---

## **5️⃣ Testing Email Sending API**
1️⃣ **Start Spring Boot App**
```bash
mvn spring-boot:run
```
2️⃣ **Send an email using Postman or cURL:**
```bash
curl -X POST "http://localhost:8080/api/email/send?to=recipient@example.com&subject=Hello&body=Welcome!"
```
🎉 **Check your inbox!** The email should arrive.

---

## **6️⃣ Sending HTML Emails**
📌 **Modify `EmailService` to Send HTML Emails**
```java
import org.springframework.mail.javamail.MimeMessageHelper;
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.stereotype.Service;

import jakarta.mail.MessagingException;
import jakarta.mail.internet.MimeMessage;

@Service
public class EmailService {

    private final JavaMailSender mailSender;

    public EmailService(JavaMailSender mailSender) {
        this.mailSender = mailSender;
    }

    public void sendHtmlEmail(String to, String subject, String htmlBody) throws MessagingException {
        MimeMessage message = mailSender.createMimeMessage();
        MimeMessageHelper helper = new MimeMessageHelper(message, true);
        helper.setTo(to);
        helper.setSubject(subject);
        helper.setText(htmlBody, true); // Enable HTML
        helper.setFrom("your-email@gmail.com");

        mailSender.send(message);
        System.out.println("HTML Email sent successfully to " + to);
    }
}
```

📌 **Modify `EmailController` to Send HTML Emails**
```java
@PostMapping("/send-html")
public String sendHtmlEmail(
        @RequestParam String to,
        @RequestParam String subject,
        @RequestParam String htmlBody) throws MessagingException {
    
    emailService.sendHtmlEmail(to, subject, htmlBody);
    return "HTML Email sent successfully!";
}
```

✅ **Test in Postman**
- **Endpoint:** `POST http://localhost:8080/api/email/send-html`
- **Body:**  
```html
<html>
<body>
    <h1>Welcome to Spring Boot Email Service</h1>
    <p>This is an <b>HTML email</b>.</p>
</body>
</html>
```
🎉 **Now, you can send beautiful HTML emails!** 🎨  

---

## **7️⃣ Sending Emails with Attachments**
📌 **Modify `EmailService` to Add Attachments**
```java
import org.springframework.core.io.FileSystemResource;
import org.springframework.mail.javamail.MimeMessageHelper;
import org.springframework.stereotype.Service;

import java.io.File;

public void sendEmailWithAttachment(String to, String subject, String body, String filePath) throws MessagingException {
    MimeMessage message = mailSender.createMimeMessage();
    MimeMessageHelper helper = new MimeMessageHelper(message, true);
    helper.setTo(to);
    helper.setSubject(subject);
    helper.setText(body, true);
    helper.setFrom("your-email@gmail.com");

    // Attach file
    FileSystemResource file = new FileSystemResource(new File(filePath));
    helper.addAttachment(file.getFilename(), file);

    mailSender.send(message);
    System.out.println("Email with attachment sent to " + to);
}
```

✅ **Now, you can send invoices, PDFs, and images as attachments!** 📎  

---

## **8️⃣ Scheduling Email Sending Using Cron Jobs**
📌 **Modify `ScheduledTasks.java` to Send Emails at 9 AM Every Day**
```java
package com.example.demo.scheduler;

import com.example.demo.service.EmailService;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

@Component
public class EmailScheduler {

    private final EmailService emailService;

    public EmailScheduler(EmailService emailService) {
        this.emailService = emailService;
    }

    @Scheduled(cron = "0 0 9 * * ?") // Every day at 9 AM
    public void sendDailyEmail() {
        emailService.sendEmail("recipient@example.com", "Daily Report", "Here is your daily report.");
        System.out.println("Daily Email Sent at 9 AM!");
    }
}
```
🎉 **Now, users get daily reports automatically!** 📅  

---

## 🎯 **Lesson 9 - Summary**
✅ Set up **Spring Boot Email Service using SMTP**  
✅ Sent **Plain Text, HTML, and Attachment Emails**  
✅ Implemented **Scheduled Email Notifications**  
✅ Used **Postman & cURL for testing**  

---
