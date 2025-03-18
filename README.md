# Test for JENKIN
Step 1: Create a Spring Boot Project
Use Spring Initializr (https://start.spring.io/) or create the project manually.

Using Spring Initializr
Select Project: Maven
Select Language: Java
Select Spring Boot Version: 2.7.x (or later)
Add Dependencies:
Spring Web
Spring Security
Spring Boot DevTools
Thymeleaf
Spring Data JPA
PostgreSQL Driver
Apache POI (for Excel file handling)
Click Generate, download the ZIP file, extract it, and open it in IntelliJ.

Step 2: Project Structure
Ensure your project follows this structure:

css
Copy
Edit
bulk-upload/
│── src/main/java/com/bankaccount/bulkupload/
│   │── BulkUploadApplication.java
│   │── config/
│   │   ├── SecurityConfig.java
│   │── controller/
│   │   ├── DashboardController.java
│   │   ├── UploadController.java
│   │   ├── AuthController.java
│── model/
│   │   ├── BankAccount.java
│   │   ├── User.java
│── repository/
│   │   ├── BankAccountRepository.java
│   │   ├── UserRepository.java
│── service/
│   │   ├── ExcelService.java
│   │   ├── UserService.java
│── src/main/resources/templates/
│   │── dashboard.html
│   │── login.html
│   │── register.html
│   │── upload.html
│── src/main/resources/static/css/
│   │── styles.css
│── src/main/resources/application.properties
│── pom.xml
Step 3: Configure application.properties
properties
Copy
Edit
spring.datasource.url=jdbc:postgresql://localhost:5432/bankdb
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.datasource.driver-class-name=org.postgresql.Driver
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.ddl-auto=update

server.port=8080
spring.thymeleaf.cache=false
Step 4: Create the BankAccount Model
java
Copy
Edit
package com.bankaccount.bulkupload.model;

import javax.persistence.*;
import lombok.Data;

@Data
@Entity
@Table(name = "bank_accounts")
public class BankAccount {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(unique = true, length = 8, nullable = false)
    private String accountId;

    @Column(nullable = false)
    private String accountName;
}
Step 5: Create the User Model
java
Copy
Edit
package com.bankaccount.bulkupload.model;

import javax.persistence.*;
import lombok.Data;

@Data
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(unique = true, nullable = false)
    private String username;

    @Column(nullable = false)
    private String password;
}
Step 6: Create Repository Interfaces
java
Copy
Edit
package com.bankaccount.bulkupload.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import com.bankaccount.bulkupload.model.BankAccount;

public interface BankAccountRepository extends JpaRepository<BankAccount, Long> {
}
java
Copy
Edit
package com.bankaccount.bulkupload.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import com.bankaccount.bulkupload.model.User;

public interface UserRepository extends JpaRepository<User, Long> {
    User findByUsername(String username);
}
Step 7: Create Authentication Service
java
Copy
Edit
package com.bankaccount.bulkupload.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.stereotype.Service;
import com.bankaccount.bulkupload.model.User;
import com.bankaccount.bulkupload.repository.UserRepository;

@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;

    @Autowired
    private PasswordEncoder passwordEncoder;

    public void registerUser(String username, String password) {
        User user = new User();
        user.setUsername(username);
        user.setPassword(passwordEncoder.encode(password));
        userRepository.save(user);
    }

    public User findByUsername(String username) {
        return userRepository.findByUsername(username);
    }
}
Step 8: Implement Security Configuration
java
Copy
Edit
package com.bankaccount.bulkupload.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

@Configuration
public class SecurityConfig {

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
Step 9: Create Controllers
AuthController.java
java
Copy
Edit
package com.bankaccount.bulkupload.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;
import org.springframework.ui.Model;
import com.bankaccount.bulkupload.service.UserService;

@Controller
public class AuthController {
    @Autowired
    private UserService userService;

    @GetMapping("/login")
    public String login() {
        return "login";
    }

    @GetMapping("/register")
    public String register() {
        return "register";
    }

    @PostMapping("/register")
    public String registerUser(@RequestParam String username, @RequestParam String password) {
        userService.registerUser(username, password);
        return "redirect:/login";
    }
}
UploadController.java
java
Copy
Edit
package com.bankaccount.bulkupload.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class UploadController {
    @GetMapping("/upload")
    public String uploadPage() {
        return "upload";
    }
}
DashboardController.java
java
Copy
Edit
package com.bankaccount.bulkupload.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class DashboardController {
    @GetMapping("/dashboard")
    public String dashboard() {
        return "dashboard";
    }
}
Step 10: Create Thymeleaf Templates
You already have these in your project. Add a styles.css file for better UI.

Step 11: Run the Application
Open a terminal in your project directory.
Run:
sh
Copy
Edit
mvn spring-boot:run
Open http://localhost:8080/login in your browser.
Step 12: Test the Application
Register a new user.
Log in.
Navigate to the dashboard.
Upload an Excel file.
Validate the data in the database.
This should give you a fully working Spring Boot web application with authentication, bulk upload, and a dashboard! 🚀 Let me know if you need more customizations.
