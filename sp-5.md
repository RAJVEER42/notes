# Day 5: Spring Security Basics

## Part 1: Authentication vs Authorization

### Authentication = "Who are you?"

**Definition:** Verifies the user's identity (username/password)

**What happens:**
1. User provides credentials (username + password)
2. Spring Security verifies credentials
3. If valid, creates `SecurityContext` with user info
4. User is now "authenticated"

**Example:** Login process

### Authorization = "What can you do?"

**Definition:** Checks if authenticated user has permission to access a resource

**What happens:**
1. User is already authenticated
2. Spring Security checks user's roles/permissions
3. If user has required role, allow access
4. If not, return 403 Forbidden

**Example:** Can user access `/admin` endpoint?

### Flow Diagram

```
Request → Authentication → Authorization → Controller
           (Who are you?)  (Can you do this?)
```

### Key Difference

| Aspect | Authentication | Authorization |
|--------|----------------|--------------|
| Question | "Who are you?" | "What can you do?" |
| When | First (login) | After authentication |
| Checks | Username/password | Roles/permissions |
| Failure | 401 Unauthorized | 403 Forbidden |

---

## Part 2: Security Filter Chain

### What is a Filter Chain?

**Filters run BEFORE controllers** - they intercept every request!

```
HTTP Request
    ↓
Security Filter Chain  ← Intercepts here!
    ↓
Controller
```

### SecurityFilterChain Configuration

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .authorizeHttpRequests(auth -> auth
            .requestMatchers("/api/public/**").permitAll()      // No auth needed
            .requestMatchers("/api/admin/**").hasRole("ADMIN")  // Admin only
            .requestMatchers("/api/products/**").hasAnyRole("USER", "ADMIN")
            .anyRequest().authenticated()                        // All others need auth
        )
        .httpBasic();  // Use HTTP Basic authentication
    
    return http.build();
}
```

### Authorization Rules

| Method | Meaning |
|--------|---------|
| `permitAll()` | No authentication required |
| `authenticated()` | Any authenticated user |
| `hasRole("ADMIN")` | Must have ADMIN role |
| `hasAnyRole("USER", "ADMIN")` | Must have USER or ADMIN role |
| `hasAuthority("READ")` | Must have specific authority |

### Request Flow

1. Request arrives at `/api/admin/dashboard`
2. Security Filter checks: Is user authenticated? → Yes
3. Security Filter checks: Does user have ADMIN role? → No
4. Return **403 Forbidden**
5. Request never reaches controller!

---

## Part 3: Password Encoding (BCrypt)

### Why Encode Passwords?

**NEVER store plain text passwords!**

If database is hacked, attacker can see all passwords!

### BCryptPasswordEncoder

**One-way hashing algorithm:**
- Can't decrypt (one-way)
- Adds salt automatically
- Same password = different hash each time!

### Example

```java
PasswordEncoder encoder = new BCryptPasswordEncoder();

String password = "admin123";
String hash1 = encoder.encode(password);  // $2a$10$abc123...
String hash2 = encoder.encode(password);  // $2a$10$xyz789... (different!)

// Verification
boolean matches = encoder.matches("admin123", hash1);  // true
```

### Key Points

- **One-way:** Can't get password back from hash
- **Salt:** Each hash is unique (even for same password)
- **Secure:** Industry standard for password storage

---

## Part 4: UserDetailsService

### What is UserDetailsService?

Provides user information for authentication.

### In-Memory Example

```java
@Bean
public UserDetailsService userDetailsService(PasswordEncoder encoder) {
    UserDetails admin = User.builder()
            .username("admin")
            .password(encoder.encode("admin123"))
            .roles("ADMIN")
            .build();
    
    UserDetails user = User.builder()
            .username("user")
            .password(encoder.encode("user123"))
            .roles("USER")
            .build();
    
    return new InMemoryUserDetailsManager(admin, user);
}
```

### Real-World

In production, you'd load users from database:

```java
@Service
public class CustomUserDetailsService implements UserDetailsService {
    
    @Autowired
    private UserRepository userRepository;
    
    @Override
    public UserDetails loadUserByUsername(String username) {
        UserEntity user = userRepository.findByUsername(username);
        // Convert to Spring Security UserDetails
        return User.builder()
                .username(user.getUsername())
                .password(user.getPassword())
                .roles(user.getRole())
                .build();
    }
}
```

---

## Part 5: SecurityContext

### What is SecurityContext?

Holds current user's authentication information.

### Accessing Current User

```java
Authentication auth = SecurityContextHolder.getContext().getAuthentication();
String username = auth.getName();
Collection<GrantedAuthority> roles = auth.getAuthorities();
```

### Key Points

- **Thread-local:** Each request has its own SecurityContext
- **Available everywhere:** Can access in any controller/service
- **Contains:** Username, roles, authorities

### Example

```java
@GetMapping("/me")
public ResponseEntity<String> getCurrentUser() {
    Authentication auth = SecurityContextHolder.getContext().getAuthentication();
    String username = auth.getName();
    return ResponseEntity.ok("Current user: " + username);
}
```

---

## Part 6: Method-Level Security

### @PreAuthorize

**Checks authorization BEFORE method executes**

```java
@PreAuthorize("hasRole('ADMIN')")
@GetMapping("/admin/users")
public List<User> getAllUsers() {
    // Only executes if user has ADMIN role
}
```

### @Secured

**Simpler, role-based only**

```java
@Secured("ROLE_ADMIN")
@GetMapping("/admin/users")
public List<User> getAllUsers() {
    // Only executes if user has ADMIN role
}
```

### Enable Method Security

```java
@Configuration
@EnableWebSecurity
@EnableMethodSecurity  // Required for @PreAuthorize/@Secured
public class SecurityConfig {
    // ...
}
```

### Common Expressions

| Expression | Meaning |
|-----------|---------|
| `hasRole('ADMIN')` | Must have ADMIN role |
| `hasAnyRole('USER', 'ADMIN')` | Must have USER or ADMIN |
| `hasAuthority('READ')` | Must have READ authority |
| `isAuthenticated()` | Must be authenticated |
| `permitAll()` | Anyone can access |

---

## Part 7: HTTP Status Codes

### Security-Related Status Codes

| Code | Meaning | When |
|------|---------|------|
| **401 Unauthorized** | Not authenticated | User not logged in |
| **403 Forbidden** | No permission | Logged in but wrong role |
| **200 OK** | Success | Authenticated and authorized |

### Example Scenarios

**Scenario 1:** Access `/api/admin/dashboard` without login
- Result: **401 Unauthorized** (not authenticated)

**Scenario 2:** Access `/api/admin/dashboard` as regular user
- Result: **403 Forbidden** (authenticated but no permission)

**Scenario 3:** Access `/api/admin/dashboard` as admin
- Result: **200 OK** (authenticated and authorized)

---

## Common Exam Traps

### Trap 1: 401 vs 403 Confusion

```java
// ❌ WRONG understanding
// 401 = Wrong password
// 403 = Right password but wrong role

// ✅ CORRECT
// 401 = Not logged in (no credentials provided)
// 403 = Logged in but no permission
```

### Trap 2: Missing @EnableMethodSecurity

```java
// ❌ WRONG - @PreAuthorize won't work
@Configuration
@EnableWebSecurity
public class SecurityConfig { }

// ✅ CORRECT
@Configuration
@EnableWebSecurity
@EnableMethodSecurity  // Required!
public class SecurityConfig { }
```

### Trap 3: Plain Text Passwords

```java
// ❌ WRONG - Never store plain text!
UserDetails user = User.builder()
        .password("admin123")  // Plain text!
        .build();

// ✅ CORRECT - Always encode!
UserDetails user = User.builder()
        .password(passwordEncoder.encode("admin123"))
        .build();
```

### Trap 4: Role Prefix

```java
// When checking roles in code:
hasRole("ADMIN")  // Spring adds "ROLE_" prefix automatically

// But in database/UserDetails:
.roles("ADMIN")  // Stored as "ROLE_ADMIN" internally
```

---

## Quiz Questions with Answers

### Q1. What's the difference between Authentication and Authorization?
**Answer:** Authentication = verify identity (who are you?), Authorization = check permissions (what can you do?)

### Q2. What does @EnableWebSecurity do?
**Answer:** Enables Spring Security and auto-configuration

### Q3. What is SecurityFilterChain?
**Answer:** Bean that configures security rules (which URLs need auth, roles, etc.)

### Q4. What password encoder should you use?
**Answer:** BCryptPasswordEncoder (one-way hashing, adds salt)

### Q5. What's the difference between 401 and 403?
**Answer:** 401 = Not authenticated (not logged in), 403 = Authenticated but no permission

### Q6. Where is current user info stored?
**Answer:** SecurityContextHolder.getContext().getAuthentication()

### Q7. What annotation enables @PreAuthorize?
**Answer:** @EnableMethodSecurity

### Q8. What does permitAll() do?
**Answer:** Allows access without authentication

### Q9. What's the default behavior if no security config?
**Answer:** All endpoints require authentication (Spring Security default)

### Q10. What does hasRole("ADMIN") check?
**Answer:** Checks if user has ROLE_ADMIN (Spring adds "ROLE_" prefix)

---

## Key Annotations Summary

| Annotation | Purpose |
|------------|---------|
| `@EnableWebSecurity` | Enables Spring Security |
| `@EnableMethodSecurity` | Enables @PreAuthorize/@Secured |
| `@PreAuthorize` | Check authorization before method |
| `@Secured` | Simpler method-level security |
| `@Bean PasswordEncoder` | BCrypt password encoder |
| `@Bean UserDetailsService` | Provides user info for auth |
| `@Bean SecurityFilterChain` | Configures security rules |

---

## Configuration Summary

### Required Dependencies

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

### Required Annotations

```java
@Configuration
@EnableWebSecurity
@EnableMethodSecurity
public class SecurityConfig {
    // ...
}
```

### Required Beans

1. `PasswordEncoder` - BCryptPasswordEncoder
2. `UserDetailsService` - Provides users
3. `SecurityFilterChain` - Configures security rules

---

## Project Structure After Day 5

```
config/
└── SecurityConfig.java          # Security configuration

controller/
├── PublicController.java        # No auth required
├── SecuredController.java       # Requires authentication
└── AdminController.java         # Requires ADMIN role

examples/
└── SecurityConceptsDemo.java    # Concepts explanation
```

---

**Day 5 Complete!**
