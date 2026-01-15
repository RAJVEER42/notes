# 🚀 SPRING BOOT JOB PORTAL - COMPLETE LEARNING DOCUMENTATION & RESUME PROMPT

---

## 📌 PURPOSE OF THIS DOCUMENT

This document serves THREE critical purposes:

1. **STANDALONE LEARNING GUIDE** - A complete reference for everything learned in Day 1 of Spring Boot development
2. **CHAT RESUME PROMPT** - Copy-paste this into a NEW AI chat to continue exactly where you left off
3. **INTERVIEW PREP BIBLE** - Deep explanations with Q&A for technical interviews

**USE CASE:** If you lose your current chat/conversation, paste the "RESUME PROMPT" section below into a new chat with Claude/ChatGPT to continue seamlessly.

---

---

# PART 1: COMPLETE LEARNING REFERENCE (DAY 1)

---

## 🎓 PROJECT OVERVIEW

### What Are We Building?
**AI-Powered Job Portal Backend** - An industry-grade REST API system using Spring Boot

**Tech Stack:**
- **Backend:** Spring Boot 3.2.1 (Java 17+)
- **Database:** PostgreSQL
- **Build Tool:** Maven
- **Security:** BCrypt password hashing (JWT authentication - Day 2)
- **Frontend:** React/Next.js (Day 2-3)
- **DevOps:** Docker, Railway/Render deployment (Day 3-4)

**Timeline:**
- **Day 1 (TODAY):** User Management APIs ✅
- **Day 2:** Job APIs, Application APIs, JWT Authentication
- **Day 3:** File Upload, Email Notifications, Frontend Integration, Docker
- **Day 4:** Cloud Deployment, Documentation, Demo Video

### Why This Project is Resume-Gold?
1. ✅ **Full-Stack:** Backend + Frontend + Database
2. ✅ **Modern Stack:** Spring Boot 3.x (latest), React
3. ✅ **Industry Patterns:** Layered architecture, DTOs, Exception handling
4. ✅ **DevOps:** Docker, CI/CD basics, Cloud deployment
5. ✅ **AI Integration:** (Day 2) Resume parsing, job recommendations
6. ✅ **Real Features:** File upload, email, authentication, pagination
7. ✅ **Interview Magnet:** Complex queries, security, performance optimization

---

## 🏗️ SPRING BOOT FUNDAMENTALS (ELI5 Explanations)

### What is Spring Boot?

**5-Year-Old Explanation:**
Imagine you want to open a lemonade stand (your app):
- **WITHOUT Spring Boot:** You build the stand from wood, paint it, make signs, buy a cash box → Takes 2 weeks!
- **WITH Spring Boot:** You get a pre-made lemonade stand kit with everything included → Ready in 1 hour!

**Technical Definition:**
Spring Boot is an **opinionated framework** built on top of Spring Framework that:
1. **Auto-configures** common settings (web server, database connections)
2. Includes **embedded Tomcat** server (no separate server installation)
3. Provides **starter dependencies** (bundles of related libraries)
4. Follows **"convention over configuration"** (sensible defaults)
5. Adds **production-ready features** (health checks, metrics)

**Key Concepts:**

| Concept | Simple Explanation | Technical Term |
|---------|-------------------|----------------|
| **Tomcat** | Built-in web server that runs your app | Embedded Servlet Container |
| **Auto-configuration** | Spring guesses what you need and sets it up | @EnableAutoConfiguration |
| **Starter Dependencies** | Pre-packaged bundles (e.g., spring-boot-starter-web) | Starter POMs |
| **Opinionated** | Makes decisions for you (can be overridden) | Convention over Configuration |

---

### What is REST API?

**Real-Life Analogy:**

```
You (Customer)  →  Waiter (API)  →  Kitchen (Backend)  →  Warehouse (Database)
     ↓                   ↓                  ↓                    ↓
  "I want             Takes order       Chef cooks           Fetches ingredients
   Paneer Tikka"      to kitchen        (business logic)     from storage
```

**Steps:**
1. **Customer orders** "Paneer Tikka" → **HTTP Request:** `POST /orders { "dish": "Paneer Tikka" }`
2. **Waiter takes order** → **Controller:** Receives request
3. **Chef cooks** → **Service:** Processes business logic
4. **Warehouse provides ingredients** → **Repository:** Fetches from database
5. **Waiter serves food** → **Response:** `{ "status": "success", "orderId": 123 }`

**Technical Definition:**
REST = **RE**presentational **S**tate **T**ransfer
- Uses **HTTP methods:** GET (read), POST (create), PUT (update), DELETE (delete)
- **Stateless:** Each request is independent (no session memory)
- **Resource-based:** URLs represent resources (e.g., `/users/123`)
- Returns **JSON** format (lightweight, JavaScript-friendly)

**HTTP Methods:**

| Method | Purpose | Example | Analogy |
|--------|---------|---------|---------|
| GET | Read/Fetch data | `GET /users/1` | Reading a book |
| POST | Create new resource | `POST /users` | Writing a new page |
| PUT | Update entire resource | `PUT /users/1` | Replacing entire chapter |
| PATCH | Partial update | `PATCH /users/1` | Editing few lines |
| DELETE | Remove resource | `DELETE /users/1` | Tearing out a page |

---

### What is MVC Architecture?

**Restaurant Analogy:**

```
┌──────────────────────────────────────────────┐
│          CUSTOMER (Frontend/Client)          │
└─────────────────┬────────────────────────────┘
                  │ Places order
                  ↓
┌──────────────────────────────────────────────┐
│     WAITER (Controller)                      │
│  - Takes orders                              │
│  - Validates (do we have this dish?)         │
│  - Returns food to customer                  │
└─────────────────┬────────────────────────────┘
                  │ Sends order to kitchen
                  ↓
┌──────────────────────────────────────────────┐
│     CHEF (Service)                           │
│  - Applies recipes (business logic)          │
│  - Decides cooking method                    │
│  - Coordinates multiple ingredients          │
└─────────────────┬────────────────────────────┘
                  │ Requests ingredients
                  ↓
┌──────────────────────────────────────────────┐
│     WAREHOUSE MANAGER (Repository)           │
│  - Fetches ingredients from storage          │
│  - Manages inventory                         │
│  - Direct access to storage                  │
└─────────────────┬────────────────────────────┘
                  │ Gets from storage
                  ↓
┌──────────────────────────────────────────────┐
│     WAREHOUSE (Database)                     │
│  Tables: users, jobs, applications           │
└──────────────────────────────────────────────┘
```

**In Spring Boot:**

| Layer | Annotation | Responsibility | Example |
|-------|-----------|----------------|---------|
| **Controller** | `@RestController` | Handle HTTP requests/responses | `UserController.java` |
| **Service** | `@Service` | Business logic, transactions | `UserServiceImpl.java` |
| **Repository** | `@Repository` | Database operations | `UserRepository.java` |
| **Model/Entity** | `@Entity` | Database tables (structure) | `User.java` |
| **DTO** | Plain class | API request/response format | `RegisterRequest.java` |

---

### What is JPA/Hibernate?

**The Magic Translator Analogy:**

**WITHOUT JPA (Manual SQL):**
```java
// You write SQL manually for EVERY operation:
String sql = "INSERT INTO users (email, password, full_name, role) VALUES (?, ?, ?, ?)";
PreparedStatement stmt = connection.prepareStatement(sql);
stmt.setString(1, "rajveer@example.com");
stmt.setString(2, hashedPassword);
stmt.setString(3, "Rajveer Bishnoi");
stmt.setString(4, "CANDIDATE");
stmt.executeUpdate();
// 10+ lines of boilerplate code!
```

**WITH JPA (Magic!):**
```java
// You just write this:
User user = new User("rajveer@example.com", hashedPassword, "Rajveer Bishnoi", UserRole.CANDIDATE);
userRepository.save(user);  
// JPA generates SQL automatically! 1 line!
```

**What Happens Behind the Scenes:**
```
userRepository.save(user)
         ↓
JPA (Interface/Contract)
         ↓
Hibernate (Implementation - does the actual work)
         ↓
Generates SQL: INSERT INTO users (email, password, full_name, role, created_at) 
               VALUES ('rajveer@example.com', '$2a$10$...', 'Rajveer Bishnoi', 'CANDIDATE', NOW())
         ↓
Executes on PostgreSQL Database
```

**Technical Terms:**

| Term | Definition | Example |
|------|------------|---------|
| **JPA** | Java Persistence API (interface/specification) | Like a contract: "save() method must exist" |
| **Hibernate** | Implementation of JPA (does actual work) | The worker who fulfills the contract |
| **ORM** | Object-Relational Mapping | Java Object ↔ Database Table conversion |
| **Entity** | Java class mapped to database table | `User.java` → `users` table |
| **Persistence** | Saving data permanently (not just in memory) | Data survives app restart |

**Why JPA is Powerful:**

1. **Database Independence:** Switch from PostgreSQL to MySQL without changing code
2. **Less Boilerplate:** No manual SQL for CRUD operations
3. **Type Safety:** Compile-time checks (no typos in column names)
4. **Automatic Relationships:** Handles foreign keys, joins automatically
5. **Caching:** Built-in performance optimization

---

### What is Maven?

**Shopping Assistant Analogy:**

**WITHOUT Maven:**
```
You (Developer): "I need Spring Web library"
   ↓
Google search → Download JAR file from random website
   ↓
Copy to project folder manually
   ↓
Repeat for 50+ dependencies
   ↓
Version conflicts: "spring-web 5.3.1 needs spring-core 5.3.1, but you have 5.2.0!"
   ↓
Dependency hell! 🔥
```

**WITH Maven:**
```
You: Write in pom.xml:
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>

Maven:
   ↓
1. Reads pom.xml
   ↓
2. Downloads spring-boot-starter-web from Maven Central Repository
   ↓
3. Auto-downloads ALL transitive dependencies (spring-core, spring-web, tomcat, etc.)
   ↓
4. Manages versions (ensures compatibility)
   ↓
5. Packages your app into JAR/WAR file
   ↓
Done! ✅
```

**What is pom.xml?**

POM = **P**roject **O**bject **M**odel

Think of it as:
- **Recipe Card** (lists ingredients/dependencies)
- **Instruction Manual** (build steps, plugins)
- **Product Label** (project name, version, description)

**Key Sections in pom.xml:**

```xml
<project>
  <!-- Product Label -->
  <groupId>com.jobportal</groupId>  <!-- Company/Organization -->
  <artifactId>jobportal-backend</artifactId>  <!-- Project Name -->
  <version>1.0.0</version>  <!-- Version -->
  
  <!-- Parent (Spring Boot manages dependency versions) -->
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.2.1</version>
  </parent>
  
  <!-- Ingredients (Libraries) -->
  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
      <!-- No version needed! Parent manages it -->
    </dependency>
  </dependencies>
  
  <!-- Build Instructions (Plugins) -->
  <build>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>
</project>
```

**Maven Lifecycle (Commands):**

| Command | Purpose | When to Use |
|---------|---------|-------------|
| `mvn clean` | Delete `target/` folder (compiled code) | Before fresh build |
| `mvn compile` | Compile Java code to `.class` files | Check for syntax errors |
| `mvn test` | Run unit tests | Verify code works |
| `mvn package` | Create JAR/WAR file | Prepare for deployment |
| `mvn install` | Install to local Maven repo | Share with other projects |
| `mvn spring-boot:run` | Run Spring Boot app | Development |

---

### What is Dependency Injection?

**Restaurant Staff Analogy:**

**WITHOUT Dependency Injection (Manual Creation):**
```java
public class UserController {
    private UserService userService;
    
    public UserController() {
        // Controller creates its own dependencies
        UserRepository userRepository = new UserRepository();
        this.userService = new UserServiceImpl(userRepository);
    }
}
// Problem: Tight coupling! Hard to test, hard to change implementation
```

**WITH Dependency Injection (Spring Magic):**
```java
@RestController
public class UserController {
    private final UserService userService;
    
    // Spring automatically provides UserService
    public UserController(UserService userService) {
        this.userService = userService;
    }
}
// Spring creates UserService, UserRepository automatically and "injects" them!
```

**How Spring DI Works:**

```
Spring Container (IOC Container)
         ↓
1. Scans for @Component, @Service, @Repository, @Controller
         ↓
2. Creates instances (Beans)
         ↓
3. Wires dependencies (Injects)
         ↓
4. Manages lifecycle (creates, destroys)
```

**Types of Dependency Injection:**

| Type | How | Example |
|------|-----|---------|
| **Constructor Injection** | Via constructor (RECOMMENDED) | `public UserController(UserService userService)` |
| **Setter Injection** | Via setter method | `public void setUserService(UserService userService)` |
| **Field Injection** | Via `@Autowired` on field | `@Autowired private UserService userService;` |

**Why Constructor Injection is Best:**
1. ✅ **Immutable:** Dependencies are `final` (can't be changed after creation)
2. ✅ **Testable:** Easy to provide mock dependencies in tests
3. ✅ **Fail-Fast:** App won't start if dependency missing (catches errors early)
4. ✅ **No NullPointerException:** Guaranteed to be initialized

---

---

## 🗄️ DATABASE & JPA DEEP DIVE

### Entity Relationships Explained

**Our Database Schema:**

```
┌─────────────────────┐
│       USER          │
├─────────────────────┤
│ id (PK)             │
│ email (UNIQUE)      │
│ password            │
│ full_name           │
│ role (ENUM)         │
│ phone               │
│ created_at          │
│ updated_at          │
└──────┬──────────────┘
       │ 1 (One user)
       │
       │ has many (posts many jobs if recruiter, applies to many jobs if candidate)
       │
       │ * (Many)
┌──────▼──────────────┐         ┌─────────────────────┐
│    APPLICATION      │   *:1   │        JOB          │
├─────────────────────┤────────►├─────────────────────┤
│ id (PK)             │         │ id (PK)             │
│ user_id (FK)        │         │ recruiter_id (FK)   │───┐
│ job_id (FK)         │         │ title               │   │
│ resume_url          │         │ description         │   │
│ cover_letter        │         │ company             │   │
│ status              │         │ location            │   │ 1 (One recruiter)
│ applied_at          │         │ salary_range        │   │
└─────────────────────┘         │ skills_required     │   │ posted by
                                │ posted_at           │   │
                                │ is_active           │   │ * (Many jobs)
                                └─────────────────────┘   │
                                        │                  │
                                        └──────────────────┘
```

**Relationship Types:**

1. **One-to-Many (User → Jobs)**
   - One recruiter posts many jobs
   - Annotation: `@OneToMany` on User side
   - Example: `List<Job> postedJobs`

2. **Many-to-One (Job → User)**
   - Many jobs belong to one recruiter
   - Annotation: `@ManyToOne` on Job side
   - Example: `User recruiter`

3. **Many-to-Many (Users ↔ Jobs via Applications)**
   - Many candidates apply to many jobs
   - Join table: `applications`
   - Annotations: `@ManyToMany` with `@JoinTable`

---

### JPA Annotations Explained

#### Entity-Level Annotations

```java
@Entity  // Marks class as database table
@Table(name = "users")  // Custom table name (optional, default is class name lowercase)
public class User {
    // ...
}
```

**Why use `@Table(name = "users")`?**
- Default would be `user` (singular)
- Best practice: Use plural for table names
- Avoids SQL reserved keywords (e.g., `order` is a SQL keyword, use `orders`)

---

#### Primary Key Annotations

```java
@Id  // Marks as primary key
@GeneratedValue(strategy = GenerationType.IDENTITY)  // Auto-increment
private Long id;
```

**Generation Strategies:**

| Strategy | How it Works | Best For |
|----------|--------------|----------|
| `IDENTITY` | Database auto-increment | PostgreSQL, MySQL |
| `SEQUENCE` | Database sequence object | Oracle, PostgreSQL |
| `TABLE` | Separate table for ID generation | Portable but slow |
| `AUTO` | JPA chooses strategy | Default (not recommended) |

**Why use `Long` instead of `int` for ID?**
- `int`: Max value = 2.1 billion
- `Long`: Max value = 9.2 quintillion
- Always use `Long` for IDs in production!

---

#### Column Annotations

```java
@Column(nullable = false, unique = true, length = 255, name = "email_address")
private String email;
```

**Attributes:**

| Attribute | Purpose | Example |
|-----------|---------|---------|
| `nullable = false` | NOT NULL constraint | Email required |
| `unique = true` | UNIQUE constraint | No duplicate emails |
| `length = 255` | VARCHAR(255) | String max length |
| `name = "..."` | Custom column name | `email` → `email_address` |
| `updatable = false` | Can't be updated after creation | `createdAt` |
| `insertable = false` | Can't be set manually | Auto-generated fields |

---

#### Enum Handling

```java
@Enumerated(EnumType.STRING)  // Store as text: "CANDIDATE", "RECRUITER"
private UserRole role;

// Alternative:
@Enumerated(EnumType.ORDINAL)  // Store as number: 0, 1, 2 (AVOID THIS!)
```

**Why `EnumType.STRING` over `ORDINAL`?**

| Type | Storage | Problem |
|------|---------|---------|
| `ORDINAL` | 0, 1, 2 | If you add new enum value in middle, all existing data becomes wrong! |
| `STRING` | "CANDIDATE" | Readable, safe to reorder |

**Example of ORDINAL problem:**
```java
// Original enum
enum UserRole {
    CANDIDATE,  // 0
    RECRUITER,  // 1
    ADMIN       // 2
}

// Later, you add new role:
enum UserRole {
    CANDIDATE,  // 0
    GUEST,      // 1  ← NEW! Now RECRUITER becomes 2!
    RECRUITER,  // 2  ← All existing RECRUITERs (stored as 1) become GUESTs!
    ADMIN       // 3
}
// Database disaster! All data corrupted!
```

---

#### Timestamp Annotations

```java
@CreationTimestamp  // Auto-set on INSERT
@Column(updatable = false)  // Prevent changes
private LocalDateTime createdAt;

@UpdateTimestamp  // Auto-update on every UPDATE
private LocalDateTime updatedAt;
```

**Why `LocalDateTime` over `Date`?**
- `Date` is old (Java 1.0), mutable, timezone issues
- `LocalDateTime` is modern (Java 8+), immutable, clear semantics
- `LocalDateTime`: Date + Time without timezone
- `LocalDate`: Date only (e.g., birthdate)
- `LocalTime`: Time only (e.g., 14:30)
- `ZonedDateTime`: With timezone (e.g., "2026-01-15T14:30:00+05:30[Asia/Kolkata]")

---

#### Lombok Annotations

```java
@Data  // Generates: getters, setters, toString, equals, hashCode
@NoArgsConstructor  // Generates: empty constructor
@AllArgsConstructor  // Generates: constructor with all fields
@Builder  // Enables builder pattern: User.builder().email("...").build()
public class User {
    // ...
}
```

**What Lombok Does (Code Generation):**

**Without Lombok:**
```java
public class User {
    private Long id;
    private String email;
    
    // Empty constructor
    public User() {}
    
    // All-args constructor
    public User(Long id, String email) {
        this.id = id;
        this.email = email;
    }
    
    // Getters
    public Long getId() { return id; }
    public String getEmail() { return email; }
    
    // Setters
    public void setId(Long id) { this.id = id; }
    public void setEmail(String email) { this.email = email; }
    
    // toString
    @Override
    public String toString() {
        return "User{id=" + id + ", email='" + email + "'}";
    }
    
    // equals and hashCode
    @Override
    public boolean equals(Object o) { /* 20+ lines */ }
    @Override
    public int hashCode() { /* 5+ lines */ }
}
// Total: ~100 lines of boilerplate!
```

**With Lombok:**
```java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class User {
    private Long id;
    private String email;
}
// Total: 6 lines! Same functionality!
```

---

---

## 🎯 COMPLETE CODE WALKTHROUGH (DAY 1)

### File Structure Overview

```
jobportal-backend/
├── src/main/java/com/jobportal/backend/
│   ├── JobportalBackendApplication.java  ← Main entry point
│   ├── config/                           ← (Empty for now, Day 2: Security config)
│   ├── controller/                       ← API endpoints (HTTP layer)
│   │   └── UserController.java
│   ├── dto/                              ← Request/Response objects
│   │   ├── ApiResponse.java
│   │   ├── ErrorResponse.java
│   │   ├── RegisterRequest.java
│   │   └── UserResponse.java
│   ├── exception/                        ← Custom exceptions & global handler
│   │   ├── DuplicateResourceException.java
│   │   ├── GlobalExceptionHandler.java
│   │   └── ResourceNotFoundException.java
│   ├── model/                            ← Database entities
│   │   ├── User.java
│   │   └── UserRole.java
│   ├── repository/                       ← Database queries
│   │   └── UserRepository.java
│   ├── service/                          ← Business logic
│   │   ├── UserService.java              (Interface)
│   │   └── UserServiceImpl.java          (Implementation)
│   └── util/                             ← Helper classes
│       └── PasswordUtil.java
└── src/main/resources/
    └── application.properties            ← Configuration
```

---

### 1. application.properties (Configuration File)

**Purpose:** Central configuration for database, server, logging

```properties
# ========================================
# DATABASE CONFIGURATION
# ========================================
spring.application.name=jobportal-backend

# PostgreSQL Connection String
# Format: jdbc:postgresql://HOST:PORT/DATABASE_NAME
spring.datasource.url=jdbc:postgresql://localhost:5432/jobportal_db
spring.datasource.username=postgres
spring.datasource.password=admin123

# Show SQL queries in console (ONLY for development!)
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

# Hibernate DDL mode
# - create: Drop and recreate tables on startup (DANGEROUS!)
# - create-drop: Drop tables on shutdown
# - update: Update schema without data loss (RECOMMENDED for dev)
# - validate: Only validate schema, no changes
# - none: Do nothing
spring.jpa.hibernate.ddl-auto=update

# Tell Hibernate to use PostgreSQL dialect (syntax)
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect

# ========================================
# SERVER CONFIGURATION
# ========================================
server.port=8080  # Application runs on http://localhost:8080

# ========================================
# LOGGING
# ========================================
logging.level.org.springframework.web=DEBUG
logging.level.com.jobportal.backend=DEBUG
```

**Key Learnings:**

**Q: What is `spring.jpa.hibernate.ddl-auto=update`?**
A: Auto-updates database schema from Java entities without deleting data.

**Q: Why not use `create` in production?**
A: It DROPS ALL TABLES and recreates them → ALL DATA LOST!

**Q: What if I change `server.port` to 9090?**
A: App runs on http://localhost:9090 instead. Useful if 8080 is already in use.

---

### 2. Model Layer (Database Entities)

#### UserRole.java (Enum)

```java
package com.jobportal.backend.model;

public enum UserRole {
    CANDIDATE,   // Job seekers
    RECRUITER,   // Companies posting jobs
    ADMIN        // Platform administrators
}
```

**Interview Q&A:**

**Q: Why enum over String for roles?**
A: 
1. **Type Safety:** Can't store "RECRUTIER" (typo)
2. **Limited Choices:** Only 3 valid values
3. **Better Performance:** Stored efficiently
4. **IDE Support:** Auto-completion, refactoring

**Q: How to add new role dynamically without redeploying?**
A: Use database table instead of enum. Trade-off: Lose type safety.

---

#### User.java (Entity)

```java
package com.jobportal.backend.model;

import jakarta.persistence.*;
import lombok.*;
import org.hibernate.annotations.CreationTimestamp;
import org.hibernate.annotations.UpdateTimestamp;
import java.time.LocalDateTime;

@Entity  // JPA: This is a database table
@Table(name = "users")  // Table name in PostgreSQL
@Data  // Lombok: Auto-generates getters, setters, toString, equals, hashCode
@NoArgsConstructor  // Lombok: Empty constructor (required by JPA)
@AllArgsConstructor  // Lombok: Constructor with all fields
@Builder  // Lombok: Enables User.builder().email("...").build()
public class User {
    
    @Id  // Primary Key
    @GeneratedValue(strategy = GenerationType.IDENTITY)  // Auto-increment
    private Long id;
    
    @Column(nullable = false, unique = true)  // NOT NULL + UNIQUE constraint
    private String email;
    
    @Column(nullable = false)
    private String password;  // Stored as BCrypt hash
    
    @Column(name = "full_name", nullable = false)
    private String fullName;
    
    @Enumerated(EnumType.STRING)  // Store as "CANDIDATE", not 0
    @Column(nullable = false)
    private UserRole role;
    
    private String phone;  // Optional field (nullable = true by default)
    
    @CreationTimestamp  // Auto-fills when record created
    @Column(name = "created_at", updatable = false)  // Can't be changed later
    private LocalDateTime createdAt;
    
    @UpdateTimestamp  // Auto-updates on every modification
    @Column(name = "updated_at")
    private LocalDateTime updatedAt;
}
```

**Generated SQL (by Hibernate):**

```sql
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    email VARCHAR(255) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    full_name VARCHAR(255) NOT NULL,
    role VARCHAR(50) NOT NULL,
    phone VARCHAR(255),
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);
```

**Interview Q&A:**

**Q: Why `@NoArgsConstructor` if we have `@AllArgsConstructor`?**
A: JPA requires empty constructor for reflection (creating instances). Lombok generates both.

**Q: What happens if I forget `@Entity`?**
A: Hibernate ignores the class. No table created. App throws error: "No entity found for query."

**Q: Can I use `int` instead of `Long` for ID?**
A: Technically yes, but `Long` is better:
- `int`: Max 2.1 billion records
- `Long`: Max 9.2 quintillion records

**Q: Why `updatable = false` on `createdAt`?**
A: Creation timestamp shouldn't change. Prevents accidental updates.

**Q: What if I remove `@Table(name = "users")`?**
A: Default table name becomes `user` (class name lowercase). Works but less readable.

---

### 3. Repository Layer (Database Access)

#### UserRepository.java

```java
package com.jobportal.backend.repository;

import com.jobportal.backend.model.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import java.util.Optional;

@Repository  // Marks as Spring-managed bean (optional with JpaRepository, but good practice)
public interface UserRepository extends JpaRepository<User, Long> {
    //                                                    ↑     ↑
    //                                                Entity  ID type
    
    // Spring auto-generates SQL: SELECT * FROM users WHERE email = ?
    Optional<User> findByEmail(String email);
    
    // Spring auto-generates: SELECT EXISTS(SELECT 1 FROM users WHERE email = ?)
    boolean existsByEmail(String email);
}
```

**Free Methods from JpaRepository:**

```java
// CRUD operations (no code needed, Spring provides these automatically!)
userRepository.save(user);           // INSERT or UPDATE
userRepository.findById(1L);         // SELECT * WHERE id = 1
userRepository.findAll();            // SELECT * FROM users
userRepository.deleteById(1L);       // DELETE WHERE id = 1
userRepository.count();              // SELECT COUNT(*) FROM users
userRepository.existsById(1L);       // SELECT EXISTS(SELECT 1 WHERE id = 1)
```

**Custom Query Method Naming Convention:**

| Method Name | Generated SQL |
|-------------|---------------|
| `findByEmail(String email)` | `SELECT * FROM users WHERE email = ?` |
| `findByEmailAndRole(String email, UserRole role)` | `WHERE email = ? AND role = ?` |
| `findByCreatedAtAfter(LocalDateTime date)` | `WHERE created_at > ?` |
| `countByRole(UserRole role)` | `SELECT COUNT(*) WHERE role = ?` |
| `deleteByEmail(String email)` | `DELETE FROM users WHERE email = ?` |

**Interview Q&A:**

**Q: Why return `Optional<User>` instead of `User`?**
A: User might not exist. `Optional` forces you to handle null case:
```java
// Without Optional (BAD)
User user = userRepository.findByEmail("test@example.com");
user.getName();  // NullPointerException if not found!

// With Optional (GOOD)
Optional<User> userOpt = userRepository.findByEmail("test@example.com");
if (userOpt.isPresent()) {
    User user = userOpt.get();
    // Safe to use
} else {
    throw new ResourceNotFoundException("User not found");
}
```

**Q: Can I write custom SQL queries?**
A: Yes, using `@Query`:
```java
@Query("SELECT u FROM User u WHERE u.email LIKE %:domain%")
List<User> findByEmailDomain(@Param("domain") String domain);
```

**Q: What's the difference between JPQL and native SQL?**
A: 
- **JPQL:** Uses entity class names (`SELECT u FROM User u`)
- **Native SQL:** Uses table names (`SELECT * FROM users`)
- JPQL is database-independent

---

### 4. DTO Layer (Data Transfer Objects)

#### RegisterRequest.java (Input DTO)

```java
package com.jobportal.backend.dto;

import com.jobportal.backend.model.UserRole;
import jakarta.validation.constraints.Email;
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.NotNull;
import jakarta.validation.constraints.Size;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class RegisterRequest {
    
    @NotBlank(message = "Email is required")  // Not null, not empty, not whitespace
    @Email(message = "Invalid email format")  // Must be valid email
    private String email;
    
    @NotBlank(message = "Password is required")
    @Size(min = 6, messag
