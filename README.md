# 📚 Library Management API with Spring Boot

A beginner-friendly REST API project built using Spring Boot for managing books, members, and borrowing records.

---

## 🯡 Tech Stack

- Java 17+
- Spring Boot 3+
- Spring Web
- Spring Data JPA
- Spring Security (Basic Authentication)
- H2 Database (dev/testing)
- Lombok (optional)
- Gradle

---

## 💑 Table of Contents

1. [GitHub Basics](#-version-control-github-basics)
2. [GitHub IDE Setup](#-github-ide-setup)
3. [Gradle Basics](#-gradle-basics)
4. [Add Validation & Exception Handling](#-1-add-validation--exception-handling)
5. [Pagination & Sorting](#-2-pagination--sorting)
6. [Service Layer](#-7-add-service-layer)
7. [Secure API with Basic Authentication](#-3-secure-api-with-basic-authentication)
8. [Postman Testing Tutorial](#-4-postman-testing-tutorial)

---

## 🔌 Project Structure

```text
src/
 └── main/
     ├── java/com/example/library
     │   ├── controller
     │   ├── model
     │   ├── repository
     │   ├── service
     │   └── LibraryManagementApplication.java
     └── resources
         ├── application.properties
         └── data.sql
```

---

## 🔌 H2 Database Config

```
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.h2.console.enabled=true
```

---

## 🚀 How to Run (Gradle)

```bash
./gradlew bootRun
```

Access at: `http://localhost:8080`

---

## 📬 REST API Endpoints

### 🔹 Book Endpoints
- `POST    /books`
- `GET     /books`
- `GET     /books/{id}`
- `PUT     /books/{id}`
- `DELETE  /books/{id}`

### 🔹 Member Endpoints
- `POST    /members`
- `GET     /members`
- `GET     /members/{id}`
- `PUT     /members/{id}`
- `DELETE  /members/{id}`

### 🔹 Borrowing
- `POST    /borrow?memberId=1&bookId=1`
- `PUT     /return/{id}`

---

## 🧪 Postman Walkthrough

1. Open Postman
2. Create a new collection called `Library API`
3. Add requests using the above endpoint URLs
4. Set headers to `Content-Type: application/json`
5. Sample `POST /books` Body:
```json
{
  "title": "Clean Code",
  "author": "Robert C. Martin",
  "isbn": "9780132350884"
}
```
6. Use Basic Auth tab: Username = `admin`, Password = `admin123`

---

## 🔐 Authentication (Basic)

You can enable basic HTTP authentication in `application.properties` using Spring Security:
```properties
spring.security.user.name=admin
spring.security.user.password=admin123
```

Then use Postman to send requests with basic auth headers.

---

## 💾 Version Control (GitHub Basics)

1. Initialize Git
```bash
git init
git add .
git commit -m "Initial commit"
```
2. Connect to GitHub
```bash
git remote add origin https://github.com/your-username/library-api.git
git push -u origin master
```

💡 **Tip Advisor:** Commit early and often to track changes better and easily debug!

---

## 💻 GitHub IDE Setup

- Install Git plugins/extensions in your IDE (IntelliJ IDEA, VS Code)
- Clone repositories directly into IDE
- Push and Pull code easily

💡 **Tip Advisor:** Save time by using built-in Git features in your IDE for faster workflow.

---

## 📦 Gradle Basics

Modify `build.gradle`:
```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-security'
    runtimeOnly 'com.h2database:h2'
}
```
Use commands:
```bash
./gradlew bootRun
./gradlew build
./gradlew clean
```

💡 **Tip Advisor:** Gradle auto-downloads dependencies so you don't manually fetch libraries.

---

## 📌 Next Steps

### ✅ 1. Add Validation & Exception Handling
- Use `@Valid` for request validation
- Add `@ControllerAdvice` to handle exceptions globally
- Return custom error responses (e.g., 400, 404)

💡 **Tip Advisor:** Always validate input! Good error handling makes your app user-friendly.

### ✅ 2. Pagination & Sorting
- Use `Pageable` in `findAll()` methods
- Allow sorting by `title`, `author`, etc.
- Example: `GET /books?page=0&size=5&sort=title`

💡 **Tip Advisor:** Pagination helps your app handle large datasets smoothly.

### ✅ 3. Secure API with Basic Authentication
- Use Spring Security starter
- Configure in `application.properties`
- Protect all endpoints with HTTP Basic Auth
- Test with Postman Basic Auth tab

💡 **Tip Advisor:** Even simple apps should protect sensitive endpoints.

### ✅ 4. Postman Testing Tutorial
- Include requests for all `GET`, `POST`, `PUT`, and `DELETE` operations
- Export Postman Collection (`.json`) and share with users

💡 **Tip Advisor:** Collections in Postman let you organize and share API tests easily.

### ✅ 5. GitHub Basics
- Add a `.gitignore` file (e.g., Java/IntelliJ)
- Create `README.md` for project documentation
- Use meaningful commit messages:
  ```bash
  git add .
  git commit -m "Added Book and Member endpoints"
  git push
  ```

### ✅ 6. Gradle Basics
- Learn to modify `build.gradle`
- Understand dependency management

💡 **Tip Advisor:** Gradle scripts are simple and powerful once you understand the structure.

### ✅ 7. Add Service Layer
- Introduce service classes (e.g., `BookService`, `MemberService`)
- Controllers delegate business logic to services
- Improves testability and separation of concerns

💡 **Tip Advisor:** Service layer keeps your controller clean and your code organized.

---

Happy Coding 🎉

