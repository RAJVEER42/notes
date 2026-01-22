# Day 2: Configuration & Dependency Injection

## What is IoC & Dependency Injection?

### Traditional Way (Without Spring)
```java
public class Dev {
    private Laptop laptop;
    
    public Dev() {
        this.laptop = new Laptop();  // Dev creates its own dependency - TIGHT COUPLING
    }
}
```

### Spring Way (IoC)
```java
@Component
public class Dev {
    @Autowired
    private Laptop laptop;  // Spring creates and injects - LOOSE COUPLING
}
```

**IoC = Inversion of Control** → Spring controls object creation
**DI = Dependency Injection** → Spring injects dependencies

---

## Stereotype Annotations

| Annotation | Layer | Purpose |
|------------|-------|---------|
| `@Component` | Generic | Any Spring-managed bean |
| `@Service` | Business | Business logic layer |
| `@Repository` | Data | Database access (+ exception translation) |
| `@Controller` | Web | MVC controller (returns views) |
| `@RestController` | Web | REST API (returns JSON) |

### Exam Note
All are functionally same as `@Component`, but indicate layer purpose!

---

## Three Types of Dependency Injection

### 1. Field Injection (Not Recommended)
```java
@Component
public class MyService {
    @Autowired
    private UserRepository userRepo;  // Spring injects after construction
}
```
- Pros: Simple, less code
- Cons: Cannot use `final`, hard to test

### 2. Constructor Injection (RECOMMENDED ✅)
```java
@Component
public class MyService {
    private final UserRepository userRepo;  // Can use final!
    
    // @Autowired is OPTIONAL with single constructor!
    public MyService(UserRepository userRepo) {
        this.userRepo = userRepo;
    }
}
```
- Pros: Can use `final`, easy to test, explicit dependencies
- Cons: More code

### 3. Setter Injection (Rarely Used)
```java
@Component
public class MyService {
    private UserRepository userRepo;
    
    @Autowired
    public void setUserRepo(UserRepository userRepo) {
        this.userRepo = userRepo;
    }
}
```
- Pros: Optional dependencies possible
- Cons: Dependency can be changed after construction

### Summary Table

| Type        | @Autowired Required? | Can use final? | Recommended? |
|-------------|---------------------|----------------|--------------|
| Field       | YES                 | NO             | NO           |
| Constructor | NO (if single)      | YES            | YES ✅        |
| Setter      | YES                 | NO             | Rarely       |

---

## Configuration Properties

### application.properties
```properties
server.port=8080
app.name=My Application
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
```

### application.yml (same thing, different syntax)
```yaml
server:
  port: 8080
app:
  name: My Application
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mydb
```

### Key Differences
- Properties: uses `=` and dots
- YAML: uses `:` and indentation
- YAML more readable for nested config

---

## @Value Annotation

### Basic Injection
```java
@Value("${app.name}")
private String appName;
```

### With Default Value
```java
@Value("${app.author:Unknown}")  // Uses "Unknown" if not found
private String author;
```

### SpEL Expression
```java
@Value("#{2 + 2}")  // Evaluates to 4
private int result;
```

### Syntax Summary

| Syntax | Meaning |
|--------|---------|
| `@Value("${prop}")` | Inject property value |
| `@Value("${prop:default}")` | Inject with default |
| `@Value("#{expression}")` | SpEL expression |
| `@Value("literal")` | Literal string |

### Exam Trap
If property not found and no default → Application fails with `IllegalArgumentException`!

---

## Bean Scopes

| Scope | Instances Created | When to Use |
|-------|-------------------|-------------|
| singleton | ONE for whole app | Stateless services (DEFAULT) |
| prototype | NEW each time | Stateful beans |
| request | ONE per HTTP request | Request-specific data |
| session | ONE per user session | User session data |

### How to Set Scope
```java
@Component
@Scope("prototype")
public class MyPrototypeBean { }
```

### Exam Questions
- Default scope = **singleton**
- Singleton = ONE instance per container
- Prototype = NEW instance each time requested

---

## @Primary vs @Qualifier

### Problem: Multiple beans of same type
```java
@Component
public class MacBook implements Computer { }

@Component
public class Dell implements Computer { }

@Component
public class Developer {
    @Autowired
    Computer computer;  // ❌ NoUniqueBeanDefinitionException!
}
```

### Solution 1: @Primary (default choice)
```java
@Component
@Primary  // This becomes default
public class MacBook implements Computer { }
```

### Solution 2: @Qualifier (specific choice)
```java
@Component
public class Developer {
    @Autowired
    @Qualifier("dell")  // Bean name = class name in camelCase
    Computer computer;
}
```

### Summary

| Situation | Solution |
|-----------|----------|
| Want default when multiple beans | `@Primary` on bean |
| Need specific bean | `@Qualifier("name")` at injection |
| Both present | `@Qualifier` wins! |

### Default Bean Names
- `MacBook` class → bean name `macBook`
- `UserService` class → bean name `userService`

---

## Quiz Questions with Answers

### Q1. Default scope of Spring bean?
**Answer: singleton**

### Q2. Recommended injection type?
**Answer: Constructor injection**

### Q3. When is @Autowired optional?
**Answer: When class has only one constructor**

### Q4. @Primary vs @Qualifier - which wins?
**Answer: @Qualifier always wins**

### Q5. Syntax for default value in @Value?
**Answer: @Value("${prop:default}")**

### Q6. Error when multiple beans, no @Primary/@Qualifier?
**Answer: NoUniqueBeanDefinitionException**

### Q7. Can you use @Autowired on final field?
**Answer: NO! Use constructor injection instead**

---

## Common Debug Errors

| Error | Cause | Fix |
|-------|-------|-----|
| `NoUniqueBeanDefinitionException` | Multiple beans, no @Primary/@Qualifier | Add @Primary or @Qualifier |
| `NoSuchBeanDefinitionException` | Bean not found | Add @Component/@Service annotation |
| `BeanCurrentlyInCreationException` | Circular dependency | Use setter injection or @Lazy |
| Field is null | Missing @Autowired (field injection) | Add @Autowired or use constructor |
| Cannot use final with @Autowired field | Field injection limitation | Use constructor injection |

---

## Project Structure After Day 2

```
src/main/java/com/Dhruv/demoApp/
├── config/
│   └── AppConfig.java          # @Value examples
├── controller/
│   └── ProductControllerV2.java # Constructor injection
├── service/
│   └── ProductService.java      # @Service layer
├── examples/
│   ├── Computer.java            # Interface
│   ├── MacBook.java             # @Primary example
│   ├── Dell.java                # Second implementation
│   ├── Developer.java           # @Qualifier example
│   ├── InjectionTypesDemo.java  # DI types reference
│   └── BeanScopesDemo.java      # Scopes reference
├── Dev.java                     # Original @Autowired
├── Laptop.java                  # Original @Component
├── Product.java
├── ProductController.java
└── DemoAppApplication.java

src/main/resources/
└── application.properties       # Configuration
```

---

## Key Annotations Summary

| Annotation | Purpose |
|------------|---------|
| `@Component` | Generic Spring bean |
| `@Service` | Business logic layer |
| `@Repository` | Data access layer |
| `@Controller` | MVC controller |
| `@RestController` | REST API controller |
| `@Autowired` | Inject dependency |
| `@Value` | Inject property value |
| `@Primary` | Default bean choice |
| `@Qualifier` | Specific bean choice |
| `@Scope` | Bean scope (singleton/prototype) |
| `@PostConstruct` | Run after bean created |

---

**Day 2 Complete!**
