

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


# 📚 Spring Boot REST API — Library Management System

*(...From previous sections, continues smoothly after Postman Testing and Final Best Practices...)*

---

# 🧩 Expanded Features

## 10. Full CRUD for BorrowedBookEntity + Pagination

Manage borrowing operations between **members** and **books** with **full CRUD** and **pagination**.

### BorrowedBookEntity

```java
@Entity
@Table(name = "borrowed_books")
public class BorrowedBookEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private Long memberId;
    private Long bookId;
    
    private LocalDate borrowDate;
    private LocalDate returnDate;
    
    // Getters and Setters
}
```

---

### BorrowedBookRepository

```java
@Repository
public interface BorrowedBookRepository extends JpaRepository<BorrowedBookEntity, Long> {
    Page<BorrowedBookEntity> findAll(Pageable pageable);
}
```

---

### BorrowController

```java
@RestController
@RequestMapping("/api/borrowed-books")
public class BorrowController {

    @Autowired
    private BorrowedBookRepository borrowedBookRepository;

    @PostMapping
    public ResponseEntity<BorrowedBookEntity> createBorrowRecord(@RequestBody BorrowedBookEntity borrowedBook) {
        BorrowedBookEntity saved = borrowedBookRepository.save(borrowedBook);
        return ResponseEntity.status(HttpStatus.CREATED).body(saved);
    }

    @GetMapping
    public ResponseEntity<Page<BorrowedBookEntity>> listBorrowedBooks(
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "5") int size,
            @RequestParam(defaultValue = "borrowDate") String sortBy) {
        
        Pageable pageable = PageRequest.of(page, size, Sort.by(sortBy));
        Page<BorrowedBookEntity> result = borrowedBookRepository.findAll(pageable);
        return ResponseEntity.ok(result);
    }

    @GetMapping("/{id}")
    public ResponseEntity<BorrowedBookEntity> getBorrowedBookById(@PathVariable Long id) {
        BorrowedBookEntity borrowedBook = borrowedBookRepository.findById(id)
            .orElseThrow(() -> new ResponseStatusException(HttpStatus.NOT_FOUND, "Borrowed Book not found"));
        return ResponseEntity.ok(borrowedBook);
    }

    @PutMapping("/{id}")
    public ResponseEntity<BorrowedBookEntity> updateBorrowedBook(
            @PathVariable Long id, @RequestBody BorrowedBookEntity updatedBorrowedBook) {
        
        BorrowedBookEntity borrowedBook = borrowedBookRepository.findById(id)
            .orElseThrow(() -> new ResponseStatusException(HttpStatus.NOT_FOUND, "Borrowed Book not found"));
        
        borrowedBook.setBookId(updatedBorrowedBook.getBookId());
        borrowedBook.setMemberId(updatedBorrowedBook.getMemberId());
        borrowedBook.setBorrowDate(updatedBorrowedBook.getBorrowDate());
        borrowedBook.setReturnDate(updatedBorrowedBook.getReturnDate());
        
        borrowedBookRepository.save(borrowedBook);
        return ResponseEntity.ok(borrowedBook);
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteBorrowedBook(@PathVariable Long id) {
        borrowedBookRepository.deleteById(id);
        return ResponseEntity.noContent().build();
    }
}
```

---

**Tips:**
- Always validate if a book is available before creating a borrowing record (future enhancement).
- Return proper HTTP status codes (`201 Created`, `200 OK`, `404 Not Found`, `204 No Content`).

---

## 11. Full Basic Authentication (Database Based)

Secure the Library API using **Spring Security** where **user credentials are stored in database**.

---

### UserEntity

```java
@Entity
@Table(name = "users")
public class UserEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(unique = true, nullable = false)
    private String username;
    
    @Column(nullable = false)
    private String password;
    
    private String role;
    
    // Getters and Setters
}
```

---

### UserRepository

```java
@Repository
public interface UserRepository extends JpaRepository<UserEntity, Long> {
    Optional<UserEntity> findByUsername(String username);
}
```

---

### CustomUserDetailsService

```java
@Service
public class CustomUserDetailsService implements UserDetailsService {

    @Autowired
    private UserRepository userRepository;
    
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        UserEntity user = userRepository.findByUsername(username)
            .orElseThrow(() -> new UsernameNotFoundException("User not found"));
        
        return org.springframework.security.core.userdetails.User
            .withUsername(user.getUsername())
            .password(user.getPassword())
            .roles(user.getRole())
            .build();
    }
}
```

---

### SecurityConfig

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Autowired
    private CustomUserDetailsService customUserDetailsService;

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.csrf().disable()
            .authorizeHttpRequests()
                .anyRequest()
                .authenticated()
            .and()
            .httpBasic();
        
        return http.build();
    }

    @Bean
    public AuthenticationManager authenticationManager(HttpSecurity http, PasswordEncoder passwordEncoder) 
            throws Exception {
        return http.getSharedObject(AuthenticationManagerBuilder.class)
                .userDetailsService(customUserDetailsService)
                .passwordEncoder(passwordEncoder)
                .and()
                .build();
    }
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

---

**Tips:**
- Always hash passwords before saving into the database using `BCryptPasswordEncoder`.
- For production, move to **JWT** or **OAuth2** instead of Basic Authentication.
- Create admin users manually through database queries initially.

Example Password Encode:
```java
new BCryptPasswordEncoder().encode("yourpassword")
```

---

# 📦 Final Folder Structure Overview (Extended)

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
│   │   ├── BorrowService.java
│   │   └── CustomUserDetailsService.java
│   ├── repository/
│   │   ├── BookRepository.java
│   │   ├── MemberRepository.java
│   │   ├── BorrowedBookRepository.java
│   │   └── UserRepository.java
│   ├── entity/
│   │   ├── BookEntity.java
│   │   ├── MemberEntity.java
│   │   ├── BorrowedBookEntity.java
│   │   └── UserEntity.java
│   ├── config/
│   │   └── SecurityConfig.java
│   └── exception/
│       ├── GlobalExceptionHandler.java
│       └── ResourceNotFoundException.java
├── build.gradle
└── README.md
```

---

> 🧠 **Learning is building. Start small, improve your structure, refactor often.**

---
Great! Here's a **sample SQL seed file** to insert an **admin user** with a **BCrypt password** into your database:

---

### `data.sql` (Database Seed for Users)

```sql
-- Create an Admin User
INSERT INTO users (username, password, role)
VALUES
  ('admin', '$2a$10$7bGQY7Fq9od6uR8S4FZfyeA9GZjjx3b6sl26yFdi8sJ0xz7g3M8r6', 'ADMIN'); -- password is 'adminpassword'
  
-- Add more users if needed, just ensure the passwords are hashed using BCrypt
INSERT INTO users (username, password, role)
VALUES
  ('user1', '$2a$10$7bGQY7Fq9od6uR8S4FZfyeA9GZjjx3b6sl26yFdi8sJ0xz7g3M8r6', 'USER'), -- password is 'user1password'
  ('user2', '$2a$10$7bGQY7Fq9od6uR8S4FZfyeA9GZjjx3b6sl26yFdi8sJ0xz7g3M8r6', 'USER'); -- password is 'user2password'
```

### Explanation:
- The passwords are **BCrypt hashed**.
- To **generate the BCrypt hash** for a password, you can use `BCryptPasswordEncoder` in your Spring Boot application:
  
  ```java
  String encodedPassword = new BCryptPasswordEncoder().encode("yourpassword");
  System.out.println(encodedPassword); // Run this in a main method or debugger
  ```

---

### Tips:
- **Admin Role**: This seed file inserts an **admin user** (`admin`) with a **`BCrypt` hashed password** (`adminpassword`), which can be used for logging in with Basic Authentication.
- **Security**: Always **hash** the passwords before saving them. The passwords in this seed file are hashed using BCrypt.
- **Extensibility**: You can add more users by following the same format — just ensure to hash the passwords with **BCrypt**.

You can place this file in the `src/main/resources` folder, and it will run automatically when your Spring Boot application starts, **loading these users** into your database.

---

