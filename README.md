

---

# 📚 Spring Boot REST API — Library Management System

A simple and complete **Library Management System API** built using Spring Boot.  
This project covers essential backend skills such as service layers, validation, exception handling, pagination, security, and API testing with Postman.

---

## 📑 Table of Contents

| No. | Topic | Description |
|:--|:--|:--|
| [1](#1-github-basics) | GitHub Basics | Set up project version control |
| [2](#2-github-ide-integration) | GitHub IDE Integration | Work with Git inside IDE |
| [3](#3-gradle-basics) | Gradle Basics | Manage project dependencies |
| [4](#4-add-validation--exception-handling) | Validation & Exception Handling | Make APIs reliable |
| [5](#5-pagination--sorting) | Pagination & Sorting | Efficiently handle large data |
| [6](#6-service-layer) | Service Layer | Organize business logic |
| [7](#7-secure-api-with-basic-authentication) | API Security (Basic Auth) | Protect APIs |
| [8](#8-postman-testing-tutorial) | Postman Testing | Test APIs easily |
| [9](#9-final-best-practices) | Final Best Practices | Development recommendations |

---

# 🛠 Entities Overview

### BookEntity
- `id`
- `title`
- `author`
- `isbn`
- `availableCopies`

### MemberEntity
- `id`
- `fullName`
- `email`
- `membershipDate`

### BorrowedBookEntity
- `id`
- `memberId`
- `bookId`
- `borrowDate`
- `returnDate`

---

# 🧩 Project Steps

---

## 1. GitHub Basics

Use Git to manage source code versions effectively.

### Core Git Commands:
```bash
git init
git clone <repository-url>
git add .
git commit -m "Meaningful commit message"
git push origin main
git pull origin main
```

**Tips:**
- Always pull before you push to avoid conflicts.
- Write clear and descriptive commit messages (example: `Add validation for member registration`).

---

## 2. GitHub IDE Integration

Integrate GitHub directly into IntelliJ IDEA or VSCode.

- Connect GitHub Account.
- Manage commits, branches, and pull requests directly from IDE.

**Tips:**
- Use **Git tool window** in IntelliJ for easier file tracking.
- Regularly sync (Fetch/Push) to keep code up-to-date.

---

## 3. Gradle Basics

Use Gradle to manage dependencies and automate builds.

### Common Gradle Commands:
```bash
./gradlew build
./gradlew bootRun
```

Example dependency (in `build.gradle`):
```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
}
```

**Tips:**
- Always refresh Gradle after adding new dependencies.
- Use Gradle Wrapper (`./gradlew`) instead of installing Gradle manually.

---

## 4. Add Validation & Exception Handling

Apply validation to incoming data and manage exceptions globally.

### Example Validation:
```java
public class MemberDTO {
    @NotBlank
    private String fullName;

    @Email
    private String email;
}
```

### Global Exception Handling:
```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<String> handleNotFound(ResourceNotFoundException ex) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(ex.getMessage());
    }
}
```

**Tips:**
- Validate all incoming requests using annotations like `@NotBlank`, `@Email`.
- Use `@RestControllerAdvice` to handle errors consistently.

---

## 5. Pagination & Sorting

Implement pagination and sorting for better performance when retrieving large datasets.

### Example Controller:
```java
@GetMapping("/books")
public Page<BookEntity> getAllBooks(Pageable pageable) {
    return bookRepository.findAll(pageable);
}
```

Request Example:
```
GET /books?page=0&size=5&sort=title,asc
```

**Tips:**
- Always provide default pagination if users don’t pass page parameters.
- Avoid returning too many results at once to protect server memory.

---

## 6. Service Layer

Keep controllers lightweight by pushing business logic into service classes.

### Service Example:
```java
@Service
public class BookService {
    public BookEntity findBookById(Long id) {
        return bookRepository.findById(id)
            .orElseThrow(() -> new ResourceNotFoundException("Book not found"));
    }
}
```

**Tips:**
- Make service classes reusable and easy to test.
- Controllers should only call service methods — no business logic inside controllers.

---

## 7. Secure API with Basic Authentication

Use Basic Authentication to protect your endpoints.

### Steps:

Add dependency:
```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-security'
}
```

Security Configuration:
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .anyRequest().authenticated()
            .and()
            .httpBasic();
    }
}
```

**Tips:**
- Use strong usernames and passwords even during development.
- Switch to more secure authentication methods (JWT, OAuth2) for production.

---

## 8. Postman Testing Tutorial

Use Postman to manually test your APIs.

### Basic Steps:
- Install Postman.
- Create a new Collection: "Library Management API".
- Add endpoints for Books, Members, Borrowing.
- Set Authorization → Basic Auth (username/password).

**Tips:**
- Save environment variables (base URLs, credentials) to save time.
- Write test scripts inside Postman to automate response checking.

---

## 9. Final Best Practices

Maintain clean coding practices and scalable architecture.

**Checklist:**
- Structure code into `controller`, `service`, `repository`, `entity`, and `exception` layers.
- Always validate incoming requests.
- Handle exceptions gracefully.
- Secure APIs appropriately.
- Test using Postman and write unit tests gradually.

**Tips:**
- Start small, then refactor your code to improve.
- Always write your code as if someone else will maintain it.

---

# 📦 Project Folder Structure

```
library-management/
├── src/main/java/com/example/library
│   ├── controller/
│   │   ├── BookController.java
│   │   ├── MemberController.java
│   │   └── BorrowController.java
│   ├── service/
│   │   ├── BookService.java
│   │   ├── MemberService.java
│   │   └── BorrowService.java
│   ├── repository/
│   │   ├── BookRepository.java
│   │   ├── MemberRepository.java
│   │   └── BorrowedBookRepository.java
│   ├── entity/
│   │   ├── BookEntity.java
│   │   ├── MemberEntity.java
│   │   └── BorrowedBookEntity.java
│   └── exception/
│       ├── GlobalExceptionHandler.java
│       └── ResourceNotFoundException.java
├── build.gradle
└── README.md
```

---

---

Would you also like me to help generate a **short sample code snippet** for `BorrowController` to complete the tutorial even better? 🚀  
(Just say "**Yes, add sample BorrowController.**")
