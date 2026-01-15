# 🚀 AI-POWERED JOB PORTAL - COMPLETE REVISION GUIDE

**Project:** Spring Boot Backend for Job Portal with AI Integration  
**Student:** RAJVEER42  
**Timeline:** 4 Days (Jan 15-18, 2026)  
**Tech Stack:** Spring Boot 3.2.1, PostgreSQL, Docker, React/Next.js

---

## 📚 TABLE OF CONTENTS
1. [Core Concepts](#core-concepts)
2. [Architecture Overview](#architecture)
3. [Database Schema](#database)
4. [Code Walkthrough](#code)
5. [Interview Questions](#interview)
6. [Commands Cheatsheet](#commands)

---

## 🎓 CORE CONCEPTS

### What is Spring Boot?
**Simple Answer:** Pre-configured Java framework for building web applications quickly.

**5-Year-Old Analogy:** 
- **Without Spring Boot:** Building a restaurant from scratch (walls, kitchen, menu) → 6 months
- **With Spring Boot:** Getting a food truck kit (everything pre-assembled) → 1 day

**Technical Definition:** 
Spring Boot is an opinionated framework built on top of Spring Framework that:
- Auto-configures common settings (web server, database connections)
- Includes embedded Tomcat server (no separate server installation needed)
- Provides starter dependencies (bundles of related libraries)
- Follows "convention over configuration" (sensible defaults)

**Interview Counter-Question:** *"Why not just use Spring Framework?"*
- **Answer:** Spring Boot eliminates XML configuration, provides auto-configuration, and includes production-ready features (health checks, metrics) out-of-the-box.

---

### What is REST API?
**Simple Answer:** Way for applications to talk to each other over the internet.

**Real-Life Analogy:** 
You (Frontend) → Waiter (API) → Kitchen (Backend) → Database

**Steps:**
1. You order "Paneer Tikka" (HTTP Request: `POST /orders`)
2. Waiter takes order to kitchen (Controller receives request)
3. Chef cooks (Service layer processes business logic)
4. Waiter serves food (Response: `{ "status": "success", "orderId": 123 }`)

**Technical Definition:**
REST = Representational State Transfer
- Uses HTTP methods: GET (read), POST (create), PUT (update), DELETE (delete)
- Stateless: Each request independent (no memory of previous requests)
- Returns JSON format (lightweight data format)

**Interview Counter-Question:** *"Why JSON over XML?"*
- **Answer:** JSON is lighter (less bandwidth), easier to parse in JavaScript, and more human-readable.

---

### What is MVC Architecture?
**Restaurant Analogy:**
- **Model** (Kitchen/Menu): Data + Database
- **View** (Presentation/Plating): What customer sees (we'll use React for this)
- **Controller** (Waiter): Takes orders, coordinates between customer and kitchen

**In Spring Boot:**
```
Client → Controller → Service → Repository → Database
         ↓                                        ↑
    (Waiter)                              (Warehouse)
         ↓                                        ↑
       DTO ←——— Business Logic ←——— Entity (Model)
```

**Interview Counter-Question:** *"Why separate Controller and Service layers?"*
- **Answer:** 
  - **Separation of Concerns:** Controller handles HTTP, Service handles business logic
  - **Reusability:** Multiple controllers can use same service
  - **Testing:** Easy to unit test service without HTTP overhead

---

### What is JPA/Hibernate?
**Simple Answer:** Tool that converts Java objects to database tables (and vice versa).

**Magic Analogy:**
Without JPA:
```sql
-- You write this manually:
INSERT INTO users (name, email) VALUES ('Rajveer', 'raj@example.com');
```

With JPA:
```java
// You just write this:
User user = new User("Rajveer", "raj@example.com");
userRepository.save(user);  // JPA generates SQL automatically!
```

**Technical Definition:**
- **JPA (Java Persistence API):** Interface/specification (like a contract)
- **Hibernate:** Implementation of JPA (does the actual work)
- **ORM (Object-Relational Mapping):** Bridges gap between Java objects and SQL tables

**Interview Counter-Question:** *"What's the N+1 problem in JPA?"*
- **Answer:** Fetching 1 parent + N children requires N+1 queries. Fix: Use `@EntityGraph` or `JOIN FETCH`.

---

### What is Maven?
**Shopping Assistant Analogy:**
You write a recipe (`pom.xml`):
```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

Maven:
1. Reads recipe
2. Downloads library from Maven Central Repository
3. Manages versions (no conflicts!)
4. Packages your app into JAR/WAR file

**Interview Counter-Question:** *"Maven vs Gradle?"*
- **Answer:** Maven uses XML (verbose but clear), Gradle uses Groovy/Kotlin (concise but steeper learning curve). Maven is industry standard for Spring Boot.

---

## 🏛️ ARCHITECTURE OVERVIEW

### Layered Architecture (Our Restaurant)

```
┌─────────────────────────────────────┐
│         FRONTEND (React)            │  ← Customer
│  (Day 2-3: We'll build this)        │
└──────────────┬──────────────────────┘
               │ HTTP Requests (JSON)
               ↓
┌─────────────────────────────────────┐
│      CONTROLLER LAYER               │  ← Waiter (Takes orders)
│  @RestController, @RequestMapping   │
│  - Receives HTTP requests           │
│  - Validates input (basic)          │
│  - Returns HTTP responses           │
└──────────────┬──────────────────────┘
               │ Calls methods
               ↓
┌─────────────────────────────────────┐
│       SERVICE LAYER                 │  ← Chef (Business logic)
│  @Service                           │
│  - Business rules                   │
│  - Data transformation              │
│  - Calls multiple repositories      │
└──────────────┬──────────────────────┘
               │ CRUD operations
               ↓
┌─────────────────────────────────────┐
│     REPOSITORY LAYER                │  ← Warehouse Manager
│  @Repository (extends JpaRepository)│
│  - Database queries                 │
│  - Auto-generated methods           │
└──────────────┬──────────────────────┘
               │ SQL queries
               ↓
┌─────────────────────────────────────┐
│      DATABASE (PostgreSQL)          │  ← Storage
│  Tables: users, jobs, applications  │
└─────────────────────────────────────┘
```

---

## 🗄️ DATABASE SCHEMA

### Entity-Relationship Diagram (ERD)

```
┌─────────────────────┐
│       USER          │
├─────────────────────┤
│ id (PK)             │
│ email (UNIQUE)      │
│ password            │
│ full_name           │
│ role (ENUM)         │  ← CANDIDATE, RECRUITER, ADMIN
│ phone               │
│ created_at          │
│ updated_at          │
└──────┬──────────────┘
       │ 1
       │ has many
       │ *
┌──────▼──────────────┐         ┌─────────────────────┐
│    APPLICATION      │   *:1   │        JOB          │
├─────────────────────┤────────►├─────────────────────┤
│ id (PK)             │         │ id (PK)             │
│ user_id (FK)        │         │ recruiter_id (FK)   │───┐
│ job_id (FK)         │         │ title               │   │
│ resume_url          │         │ description         │   │
│ cover_letter        │         │ company             │   │
│ status (ENUM)       │         │ location            │   │
│ applied_at          │         │ salary_range        │   │
└─────────────────────┘         │ experience_required │   │
                                │ skills_required     │   │ 1
                                │ posted_at           │   │
                                │ deadline            │   │ posted by
                                │ is_active           │   │
                                └─────────────────────┘   │
                                        │                  │
                                        └──────────────────┘
                                               *
                                        (Many jobs per recruiter)
```

### Table Relationships Explained

**1. USER → APPLICATION (One-to-Many)**
- One candidate can apply to many jobs
- One application belongs to one user

**2. JOB → APPLICATION (One-to-Many)**
- One job can have many applications
- One application is for one job

**3. USER (Recruiter) → JOB (One-to-Many)**
- One recruiter can post many jobs
- One job is posted by one recruiter

---

## 📝 CODE WALKTHROUGH

### Step 1: User Entity (Model)

**File:** `model/User.java`

```java
package com.jobportal.backend.model;

import jakarta.persistence.*;
import lombok.*;
import org.hibernate.annotations.CreationTimestamp;
import org.hibernate.annotations.UpdateTimestamp;
import java.time.LocalDateTime;

@Entity  // Tells JPA: This is a database table
@Table(name = "users")  // Table name in PostgreSQL
@Data  // Lombok: Auto-generates getters, setters, toString
@NoArgsConstructor  // Lombok: Creates empty constructor
@AllArgsConstructor  // Lombok: Creates constructor with all fields
@Builder  // Lombok: Enables User.builder().email("...").build()
public class User {
    
    @Id  // Primary Key
    @GeneratedValue(strategy = GenerationType.IDENTITY)  // Auto-increment
    private Long id;
    
    @Column(nullable = false, unique = true)  // NOT NULL + UNIQUE constraint
    private String email;
    
    @Column(nullable = false)
    private String password;  // Will be hashed (BCrypt)
    
    @Column(name = "full_name", nullable = false)
    private String fullName;
    
    @Enumerated(EnumType.STRING)  // Store as text, not number
    @Column(nullable = false)
    private UserRole role;  // CANDIDATE, RECRUITER, ADMIN
    
    private String phone;
    
    @CreationTimestamp  // Auto-fills when record created
    @Column(name = "created_at", updatable = false)
    private LocalDateTime createdAt;
    
    @UpdateTimestamp  // Auto-updates on every change
    @Column(name = "updated_at")
    private LocalDateTime updatedAt;
}
```

**Annotation Breakdown:**

| Annotation | Purpose | Example |
|------------|---------|---------|
| `@Entity` | Marks class as database table | JPA scans this |
| `@Table(name="users")` | Custom table name | Default would be "user" |
| `@Id` | Primary key | Like Aadhar card (unique ID) |
| `@GeneratedValue` | Auto-increment ID | 1, 2, 3, 4... |
| `@Column` | Customize column | Set constraints, rename |
| `@Enumerated` | Store enum as string/int | "CANDIDATE" vs 0 |
| `@CreationTimestamp` | Auto-set creation time | Like "Date of Birth" |
| `@UpdateTimestamp` | Auto-update modified time | Like "Last Modified" |

**Interview Counter-Questions:**

**Q1:** *"Why use `@Data` from Lombok instead of writing getters/setters manually?"*
- **Answer:** Reduces boilerplate code (100 lines → 1 annotation). Keeps code clean. Auto-updates if fields change.
- **Gotcha:** Lombok requires IDE plugin + Maven dependency.

**Q2:** *"What if I use `@GeneratedValue(strategy = GenerationType.AUTO)` instead of `IDENTITY`?"*
- **Answer:** 
  - `AUTO`: JPA chooses strategy (might use sequence/table)
  - `IDENTITY`: Uses database auto-increment (best for PostgreSQL/MySQL)
  - `SEQUENCE`: Uses database sequence (Oracle/PostgreSQL)
  - `TABLE`: Uses separate table (slower, avoid)

**Q3:** *"Why hash passwords? Can't we just store plain text?"*
- **Answer:** If database is hacked, attackers see passwords. Hashing (BCrypt) converts "password123" → "$2a$10$randomhash". One-way (can't decrypt). We'll use `BCryptPasswordEncoder` in Spring Security.

**Q4:** *"What's the difference between `@Column(nullable = false)` and `@NotNull` validation?"*
- **Answer:**
  - `@Column(nullable = false)`: Database constraint (DB rejects null)
  - `@NotNull`: Application-level validation (Spring validates before sending to DB)
  - **Best Practice:** Use both for defense-in-depth!

---

### Step 2: UserRole Enum

**File:** `model/UserRole.java`

```java
package com.jobportal.backend.model;

public enum UserRole {
    CANDIDATE,   // Job seekers
    RECRUITER,   // Companies posting jobs
    ADMIN        // Platform administrators
}
```

**Interview Counter-Question:** *"Why enum over storing role as String?"*
- **Answer:** 
  - Type safety (can't store "RECRUTIER" typo)
  - Limited choices (only 3 valid roles)
  - Better performance (stored efficiently)
  - IDE auto-completion

---

### Step 3: User Repository

**File:** `repository/UserRepository.java`

```java
package com.jobportal.backend.repository;

import com.jobportal.backend.model.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import java.util.Optional;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    
    // Spring auto-generates SQL: SELECT * FROM users WHERE email = ?
    Optional<User> findByEmail(String email);
    
    // Spring auto-generates: SELECT EXISTS(SELECT 1 FROM users WHERE email = ?)
    boolean existsByEmail(String email);
}
```

**Magic Explained:**

You write: `findByEmail`  
Spring generates:
```sql
SELECT * FROM users WHERE email = :email
```

**Naming Convention Rules:**
- `findBy[FieldName]` → WHERE clause
- `existsBy[FieldName]` → Returns true/false
- `countBy[FieldName]` → COUNT(*)
- `deleteBy[FieldName]` → DELETE WHERE

**Interview Counter-Questions:**

**Q1:** *"Why return `Optional<User>` instead of `User`?"*
- **Answer:** User might not exist. `Optional` forces you to handle null case:
```java
Optional<User> userOpt = userRepository.findByEmail("test@example.com");
if (userOpt.isPresent()) {
    User user = userOpt.get();
} else {
    // Handle not found
}
```

**Q2:** *"What methods does `JpaRepository` provide for free?"*
- **Answer:** `save()`, `findById()`, `findAll()`, `deleteById()`, `count()`, `existsById()` - we don't write these!

**Q3:** *"How to write complex queries like 'Find users created in last 7 days'?"*
- **Answer:** Three options:
  1. **Query Methods:** `findByCreatedAtAfter(LocalDateTime date)`
  2. **@Query:** Custom JPQL/SQL
  3. **Specifications:** For dynamic queries

---

## 🎤 INTERVIEW QUESTIONS BY MODULE

### Module: Spring Boot Basics

**Q1:** What is Spring Boot and why use it over plain Spring?
- **Answer:** Spring Boot = Spring Framework + auto-configuration + embedded server + production-ready features. Reduces boilerplate XML config, faster development.

**Q2:** Explain the flow of a REST API request in Spring Boot.
- **Answer:** 
```
Client → DispatcherServlet → Controller → Service → Repository → Database
                                 ↓                                    ↑
                              Response ←────────────────────────────┘
```

**Q3:** What is the difference between `@Component`, `@Service`, `@Repository`, `@Controller`?
- **Answer:** All create Spring beans, but:
  - `@Component`: Generic
  - `@Service`: Business logic layer (semantic)
  - `@Repository`: Data access layer + exception translation
  - `@Controller`/`@RestController`: Web layer

**Q4:** What is dependency injection?
- **Answer:** Spring creates objects and "injects" them where needed. Example:
```java
@Service
public class UserService {
    private final UserRepository userRepository;  // Spring injects this
    
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

**Q5:** What is the difference between `@RestController` and `@Controller`?
- **Answer:** 
  - `@Controller`: Returns view names (HTML pages)
  - `@RestController`: Returns JSON/XML (`@Controller` + `@ResponseBody`)

---

### Module: JPA/Hibernate

**Q1:** What is the N+1 query problem?
- **Answer:** Fetching 1 parent + N children fires N+1 queries:
```java
List<User> users = userRepository.findAll();  // 1 query
for (User user : users) {
    user.getApplications().size();  // N queries (one per user!)
}
```
- **Fix:** Use `@EntityGraph` or `JOIN FETCH`

**Q2:** Difference between `@OneToMany` and `@ManyToOne`?
- **Answer:** Opposite directions of same relationship:
  - `User` has `@OneToMany` applications (owns relationship)
  - `Application` has `@ManyToOne` user (foreign key side)

**Q3:** What is lazy vs eager loading?
- **Answer:**
  - **Lazy (default):** Loads related data only when accessed
  - **Eager:** Loads everything immediately (can cause performance issues)

**Q4:** How to prevent SQL injection in JPA?
- **Answer:** JPA uses parameterized queries:
```java
// SAFE - uses parameters
@Query("SELECT u FROM User u WHERE u.email = :email")
User findByEmail(@Param("email") String email);

// UNSAFE - string concatenation
@Query("SELECT u FROM User u WHERE u.email = '" + email + "'")  // DON'T DO THIS!
```

**Q5:** What is `@Transactional` and when to use it?
- **Answer:** Wraps method in database transaction. If exception occurs, all changes rollback.
```java
@Transactional
public void transferMoney(Long fromId, Long toId, BigDecimal amount) {
    // If any step fails, both revert
    accountRepository.debit(fromId, amount);
    accountRepository.credit(toId, amount);
}
```

---

## 🛠️ COMMANDS CHEATSHEET

### Maven Commands
```bash
# Clean build artifacts
mvn clean

# Compile code
mvn compile

# Run tests
mvn test

# Package as JAR
mvn package

# Run Spring Boot app
mvn spring-boot:run

# Install to local Maven repo
mvn install

# Skip tests during build
mvn package -DskipTests
```

### PostgreSQL Commands
```sql
-- Connect to database
psql -U postgres -d jobportal_db

-- List all tables
\dt

-- Describe table structure
\d users

-- View all users
SELECT * FROM users;

-- Count records
SELECT COUNT(*) FROM users;

-- Exit
\q
```

### Git Commands (Day 4)
```bash
# Initialize repository
git init

# Add all files
git add .

# Commit
git commit -m "Initial commit: User API"

# Link to GitHub
git remote add origin <your-repo-url>

# Push to GitHub
git push -u origin main
```

---

## 📊 PROGRESS TRACKER

### Day 1: Backend Foundation
- [x] Environment setup
- [x] Project structure
- [x] Database schema design
- [ ] User entity created
- [ ] User repository created
- [ ] User registration API
- [ ] User login API (JWT)

### Day 2: Advanced Features
- [ ] Job entity + APIs
- [ ] Application entity + APIs
- [ ] File upload (resume)
- [ ] Email notifications
- [ ] Frontend setup

### Day 3: Polish & Testing
- [ ] Frontend-backend integration
- [ ] Docker setup
- [ ] Unit tests
- [ ] Integration tests

### Day 4: Deployment
- [ ] Cloud deployment
- [ ] README + documentation
- [ ] Demo video
- [ ] Interview prep

---

## 🚨 COMMON PITFALLS & FIXES

### Pitfall 1: Port Already in Use
**Error:** `Port 8080 is already in use`

**Fix:**
```bash
# Find process using port 8080
lsof -i :8080

# Kill process
kill -9 <PID>

# OR change port in application.properties
server.port=8081
```

### Pitfall 2: Database Connection Failed
**Error:** `Connection refused: localhost:5432`

**Checks:**
1. Is PostgreSQL running? `pg_ctl status`
2. Correct password in `application.properties`?
3. Database exists? `psql -U postgres -l`

### Pitfall 3: Lombok Not Working
**Error:** Cannot resolve method `getId()`

**Fix:**
1. Install Lombok plugin in VS Code
2. Verify in `pom.xml`:
```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
```
3. Enable annotation processing (VS Code: already enabled)

---

---

## 🔥 EXCEPTION HANDLING MODULE

### Custom Exceptions Created

**1. ResourceNotFoundException**
- **Purpose:** Thrown when resource (user, job) doesn't exist
- **HTTP Status:** 404 NOT FOUND
- **Example:** User with ID 999 not found

**2. DuplicateResourceException**
- **Purpose:** Thrown when trying to create duplicate resource
- **HTTP Status:** 409 CONFLICT
- **Example:** Email already registered

### Global Exception Handler

**File:** `exception/GlobalExceptionHandler.java`

**Key Annotations:**
- `@RestControllerAdvice`: Global exception handler for all controllers
- `@ExceptionHandler`: Catches specific exception type

**Exception Flow:**
```
Controller throws Exception
       ↓
GlobalExceptionHandler catches it
       ↓
Converts to ErrorResponse
       ↓
Returns appropriate HTTP status
```

### Error Response Structure

**Success Response:**
```json
{
  "success": true,
  "message": "Operation successful",
  "data": { /* actual data */ },
  "timestamp": "2026-01-15T15:00:00"
}
```

**Error Response:**
```json
{
  "success": false,
  "message": "Error description",
  "timestamp": "2026-01-15T15:00:00",
  "errors": ["validation error 1", "validation error 2"]
}
```

### HTTP Status Codes Used

| Code | Name | When Used |
|------|------|-----------|
| 200 | OK | Successful GET request |
| 201 | CREATED | Successful POST (resource created) |
| 400 | BAD REQUEST | Validation failed |
| 404 | NOT FOUND | Resource doesn't exist |
| 409 | CONFLICT | Duplicate resource |
| 500 | INTERNAL SERVER ERROR | Unexpected error |

### Interview Questions: Exception Handling

**Q1:** Why use global exception handler instead of try-catch in every controller?
- **Answer:** DRY principle (Don't Repeat Yourself). Centralized error handling. Consistent error response format across all APIs.

**Q2:** What's the difference between `@ControllerAdvice` and `@RestControllerAdvice`?
- **Answer:** `@RestControllerAdvice` = `@ControllerAdvice` + `@ResponseBody`. Returns JSON automatically.

**Q3:** How does Spring find the right exception handler?
- **Answer:** Matches exception type. Most specific match wins. If no match, falls back to generic `Exception.class` handler.

**Q4:** What is `MethodArgumentNotValidException`?
- **Answer:** Thrown when `@Valid` validation fails. Contains all field errors.

**Q5:** Should we expose stack traces to clients?
- **Answer:** NO! Security risk. Log internally, return generic message to client.

---

## 📊 DAY 1 COMPLETION STATUS

### ✅ Completed Tasks

- [x] Environment setup (Java, Maven, PostgreSQL, VS Code)
- [x] Project structure created
- [x] Database configuration
- [x] User entity with annotations
- [x] UserRole enum
- [x] UserRepository with custom queries
- [x] DTOs (RegisterRequest, UserResponse, ErrorResponse, ApiResponse)
- [x] UserService interface + implementation
- [x] UserController with REST endpoints
- [x] Custom exceptions (ResourceNotFoundException, DuplicateResourceException)
- [x] Global exception handler
- [x] API testing with Thunder Client

### 📝 Files Created (Day 1)

**Model Layer (2 files):**
- `model/User.java`
- `model/UserRole.java`

**Repository Layer (1 file):**
- `repository/UserRepository.java`

**Service Layer (2 files):**
- `service/UserService.java`
- `service/UserServiceImpl.java`

**Controller Layer (1 file):**
- `controller/UserController.java`

**DTO Layer (4 files):**
- `dto/RegisterRequest.java`
- `dto/UserResponse.java`
- `dto/ErrorResponse.java`
- `dto/ApiResponse.java`

**Exception Layer (3 files):**
- `exception/ResourceNotFoundException.java`
- `exception/DuplicateResourceException.java`
- `exception/GlobalExceptionHandler.java`

**Total Files:** 13 files + `application.properties` = 14 files

### 🎯 API Endpoints Built

| Method | Endpoint | Purpose | Status Code |
|--------|----------|---------|-------------|
| POST | `/api/users/register` | Register new user | 201 |
| GET | `/api/users/{id}` | Get user by ID | 200 |
| GET | `/api/users/email/{email}` | Get user by email | 200 |

---

**Last Updated:** Day 1 - Step 8 (Exception Handling Complete)  
**Next:** Password Hashing with BCrypt → JWT Authentication → Job Entity
