# Day 3: JPA Entities & Repositories

## What is JPA?

**JPA (Java Persistence API)** = Standard for mapping Java objects to database tables.

**Spring Data JPA** = Makes JPA even easier - you just define interfaces, Spring implements them!

### Before JPA (JDBC - Painful!):
```java
String sql = "SELECT * FROM users WHERE id = ?";
Connection conn = DriverManager.getConnection(url, user, pass);
PreparedStatement stmt = conn.prepareStatement(sql);
stmt.setLong(1, userId);
ResultSet rs = stmt.executeQuery();
// ... manually map each column to object fields
// Don't forget to close everything!
```

### With JPA (Clean!):
```java
User user = userRepository.findById(userId).orElse(null);
// That's it! One line!
```

---

## The Architecture: How Everything Connects

```
HTTP Request (from browser/Postman)
        ↓
   CONTROLLER  (@RestController)
   - Handles HTTP (GET, POST, etc.)
   - Doesn't know about database
        ↓ calls
    SERVICE  (@Service)
    - Business logic
    - Validation, calculations
    - Calls repository
        ↓ calls
   REPOSITORY  (extends JpaRepository)
   - Database operations
   - Spring auto-implements!
        ↓ talks to
    ENTITY  (@Entity)
    - Java class = Database table
        ↓ maps to
    DATABASE  (H2/MySQL/PostgreSQL)
    - Actual tables & data
```

### Restaurant Analogy:
- **Customer** = Browser/Postman (makes request)
- **Waiter** = Controller (takes order, gives response)
- **Chef** = Service (business logic, decides how to cook)
- **Storage Room** = Repository (gets/stores ingredients)
- **Fridge** = Database (where data is actually kept)

---

## Part 1: Entity Annotations

An **Entity** is a Java class that represents a database table. Each instance = one row.

### Required Annotations:

#### `@Entity` (REQUIRED)
```java
@Entity
public class ProductEntity {
```
- Tells JPA "this class maps to a database table"
- Without this, JPA ignores the class completely
- **Exam trap:** Missing `@Entity` = class won't be persisted!

#### `@Id` (REQUIRED)
```java
@Id
private Long id;
```
- Marks the primary key field
- Every entity MUST have exactly one `@Id`
- **Exam trap:** Missing `@Id` = Error! JPA won't work.

### Optional Annotations:

#### `@Table(name = "products")`
```java
@Table(name = "products")
public class ProductEntity {
```
- Specifies the actual table name in database
- If omitted, table name = class name (lowercase)

#### `@GeneratedValue`
```java
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```
- Auto-generates ID values
- **Strategies:**
  - `IDENTITY` - Database auto-increment (MySQL, PostgreSQL, H2)
  - `SEQUENCE` - Database sequence (Oracle)
  - `AUTO` - Let JPA decide (default)
  - `TABLE` - Separate table for IDs (rarely used)

#### `@Column`
```java
@Column(nullable = false, length = 100, unique = true)
private String name;
```
- Customizes column properties
- **Options:**
  - `nullable = false` → NOT NULL constraint
  - `unique = true` → UNIQUE constraint
  - `length = 100` → VARCHAR(100)
  - `name = "product_name"` → Custom column name

#### `@Transient`
```java
@Transient
private Double discountedPrice;
```
- Field is NOT saved to database
- Used for calculated/temporary values

### Complete Entity Example:
```java
@Entity
@Table(name = "products")
public class ProductEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false, length = 100)
    private String name;
    
    @Column(nullable = false)
    private Double price;
    
    @Column(length = 500)
    private String description;
    
    @Column(unique = true)
    private String sku;
    
    @Column(name = "in_stock")
    private Boolean inStock = true;
    
    @Transient  // NOT saved to database
    private Double discountedPrice;

    // Default constructor (REQUIRED by JPA!)
    public ProductEntity() {
    }

    // Getters and Setters (Required by JPA!)
    // ...
}
```

---

## Part 2: Repository Interface

A **Repository** is an interface that provides database operations.

**The Magic:** You don't write the implementation - Spring does it automatically!

### Basic Setup:
```java
@Repository  // Optional for JpaRepository
public interface ProductRepository extends JpaRepository<ProductEntity, Long> {
    // Spring auto-implements everything!
}
```

- First generic = Entity type (`ProductEntity`)
- Second generic = ID type (`Long`)

### Built-in Methods (FREE!):

| Method | What it does | Returns |
|--------|--------------|---------|
| `save(entity)` | INSERT or UPDATE | Entity |
| `findById(id)` | Find by primary key | `Optional<Entity>` |
| `findAll()` | Get all records | `List<Entity>` |
| `deleteById(id)` | Delete by ID | void |
| `count()` | Count total records | long |
| `existsById(id)` | Check if exists | boolean |

### IMPORTANT: findById() returns Optional!
```java
// WRONG - Compile error!
Product p = productRepository.findById(1);

// CORRECT - Handle Optional
Optional<Product> p = productRepository.findById(1L);
if (p.isPresent()) {
    Product product = p.get();
}

// Or use orElse
Product product = productRepository.findById(1L).orElse(null);
```

### Custom Query Methods:

Just define a method name, Spring generates the SQL!

```java
public interface ProductRepository extends JpaRepository<ProductEntity, Long> {
    
    // Find by exact name
    // SQL: SELECT * FROM products WHERE name = ?
    List<ProductEntity> findByName(String name);
    
    // Find by name containing (LIKE)
    // SQL: SELECT * FROM products WHERE name LIKE '%keyword%'
    List<ProductEntity> findByNameContaining(String keyword);
    
    // Find by price less than
    // SQL: SELECT * FROM products WHERE price < ?
    List<ProductEntity> findByPriceLessThan(Double price);
    
    // Find by price between
    // SQL: SELECT * FROM products WHERE price BETWEEN ? AND ?
    List<ProductEntity> findByPriceBetween(Double min, Double max);
    
    // Find by boolean field
    List<ProductEntity> findByInStock(Boolean inStock);
    
    // Combined conditions
    List<ProductEntity> findByNameAndPriceLessThan(String name, Double price);
    
    // Order by
    List<ProductEntity> findByInStockTrueOrderByPriceAsc();
}
```

### Query Method Keywords:

| Keyword | SQL Equivalent | Example |
|---------|----------------|---------|
| `findBy` | WHERE | `findByName(name)` |
| `Containing` | LIKE %...% | `findByNameContaining(x)` |
| `StartingWith` | LIKE x% | `findByNameStartingWith(x)` |
| `EndingWith` | LIKE %x | `findByNameEndingWith(x)` |
| `LessThan` | < | `findByPriceLessThan(x)` |
| `GreaterThan` | > | `findByPriceGreaterThan(x)` |
| `Between` | BETWEEN | `findByPriceBetween(a, b)` |
| `And` | AND | `findByNameAndPrice(...)` |
| `Or` | OR | `findByNameOrPrice(...)` |
| `OrderBy` | ORDER BY | `findByXOrderByYAsc()` |
| `IsNull` | IS NULL | `findByDescriptionIsNull()` |
| `True/False` | = true/false | `findByInStockTrue()` |

---

## Part 3: Database Configuration

### application.properties:
```properties
# H2 In-Memory Database
spring.datasource.url=jdbc:h2:mem:demoDb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

# JPA / Hibernate
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=true

# H2 Console (web interface)
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console
```

### ddl-auto Options (IMPORTANT!):

| Option | What it does | When to use |
|--------|--------------|-------------|
| `none` | Do nothing | Production |
| `validate` | Check schema matches entities | Production |
| `update` | Add new columns (never deletes) | Development |
| `create` | Drop and create tables | Testing |
| `create-drop` | Create on start, drop on shutdown | Development |

**Exam trap:** Never use `create` or `update` in production!

---

## Part 4: Why Do We Need Service Layer?

### The Question:
"If Service just calls Repository, why do we need it?"

```java
// This seems useless...
public List<ProductEntity> findAll() {
    return productRepository.findAll();  // Just forwarding!
}
```

### The Answer: Real-world scenarios need Service!

#### Reason 1: Business Logic
```java
@Service
public class ProductService {
    
    public ProductEntity create(ProductEntity product) {
        // Business rules - Service ka kaam!
        if (product.getPrice() < 0) {
            throw new InvalidPriceException("Price cannot be negative!");
        }
        if (product.getName().length() < 3) {
            throw new InvalidNameException("Name must be at least 3 chars!");
        }
        return productRepository.save(product);
    }
}
```

#### Reason 2: Multiple Repositories
```java
@Service
public class ProductService {
    
    private final ProductRepository productRepo;
    private final CategoryRepository categoryRepo;
    
    public ProductWithCategory getProductWithCategory(Long id) {
        ProductEntity product = productRepo.findById(id).get();
        CategoryEntity category = categoryRepo.findById(product.getCategoryId()).get();
        
        // Combine both - Service coordinates!
        return new ProductWithCategory(product, category);
    }
}
```

#### Reason 3: Multiple Operations
```java
@Service
public class OrderService {
    
    public Order placeOrder(Order order) {
        // Step 1: Save order
        Order saved = orderRepo.save(order);
        
        // Step 2: Update inventory
        inventoryRepo.reduceStock(order.getProductId(), order.getQuantity());
        
        // Step 3: Send email
        emailService.sendOrderConfirmation(order);
        
        return saved;
    }
}
```

### Summary - Why Service Layer?

| Without Service | With Service |
|-----------------|--------------|
| Business logic in Controller | Business logic in Service |
| Controller becomes fat | Controller stays clean |
| Hard to test | Easy to test |
| Code repetition | Reusable methods |
| Hard to manage multiple repos | Service coordinates repos |

### Golden Rule:
- **Controller** = Only HTTP handling (request/response)
- **Service** = Business logic (rules, validation, coordination)
- **Repository** = Database operations

---

## Part 5: How save() Works

```java
productRepository.save(product);
```

- **If product.id is NULL:** → Does INSERT (creates new row)
- **If product.id has value:** → Does UPDATE (updates existing row)

This is why we use `save()` for both create and update!

---

## Part 6: Repository Hierarchy

```
Repository (marker interface)
    ↓
CrudRepository (basic CRUD)
    ↓
PagingAndSortingRepository (+ pagination, sorting)
    ↓
JpaRepository (+ JPA specific methods)
```

**Always use `JpaRepository`** - it has the most features!

---

## Common Exam Traps

### Trap 1: Missing @Entity
```java
// ❌ WRONG - missing @Entity
public class Product {
    @Id
    private Long id;
}
// This class won't be persisted!
```

### Trap 2: Missing @Id
```java
// ❌ WRONG - no @Id
@Entity
public class Product {
    private Long id;
    private String name;
}
// Error: No identifier specified for entity
```

### Trap 3: findById returns Optional
```java
// ❌ WRONG
Product p = repo.findById(1);  // Compile error!

// ✅ CORRECT
Optional<Product> p = repo.findById(1L);
```

### Trap 4: No default constructor
```java
@Entity
public class Product {
    @Id
    private Long id;
    
    // ❌ Only parameterized constructor
    public Product(String name) { }
}
// JPA requires a no-arg constructor!
```

### Trap 5: Primitive types for nullable fields
```java
@Entity
public class Product {
    // ❌ Can't be null (primitive)
    private int quantity;
    
    // ✅ Can be null (wrapper)
    private Integer quantity;
}
```

---

## Quiz Questions with Answers

### Q1. Which annotation marks a class as a JPA entity?
**Answer: `@Entity`**

### Q2. What happens if `@Id` is missing?
**Answer: Error - every entity must have @Id**

### Q3. What is the default GenerationType?
**Answer: AUTO**

### Q4. What interface should a repository extend?
**Answer: JpaRepository (has most features)**

### Q5. What does `findByNameContaining(x)` generate?
**Answer: `WHERE name LIKE '%x%'`**

### Q6. What's the return type of `findById()`?
**Answer: `Optional<Entity>`**

### Q7. Which ddl-auto creates tables and drops on shutdown?
**Answer: `create-drop`**

### Q8. What's wrong with this entity?
```java
@Entity
public class User {
    private Long id;
    private String name;
}
```
**Answer: Missing `@Id` annotation on id field!**

### Q9. Why do we need Service layer if it just calls Repository?
**Answer:**
- Business logic belongs in Service
- Service coordinates multiple repositories
- Service handles validation and rules
- Keeps Controller clean (only HTTP)
- Makes code testable and reusable

---

## Project Structure After Day 3

```
src/main/java/com/Dhruv/demoApp/
├── entity/
│   ├── ProductEntity.java      # @Entity - maps to products table
│   └── CategoryEntity.java     # @Entity - maps to categories table
├── repository/
│   ├── ProductRepository.java  # extends JpaRepository
│   └── CategoryRepository.java # extends JpaRepository
├── service/
│   ├── ProductService.java     # In-memory (Day 2)
│   └── ProductDbService.java   # Uses Repository (Day 3)
├── controller/
│   ├── ProductController.java  # In-memory (Day 1)
│   ├── ProductControllerV2.java # With Service (Day 2)
│   └── ProductDbController.java # With Database (Day 3)
└── ...

src/main/resources/
└── application.properties      # Database config
```

---

## Key Annotations Summary

| Annotation | Purpose |
|------------|---------|
| `@Entity` | Mark class as JPA entity (REQUIRED) |
| `@Table` | Custom table name |
| `@Id` | Primary key field (REQUIRED) |
| `@GeneratedValue` | Auto-generate ID |
| `@Column` | Customize column properties |
| `@Transient` | Don't persist to database |
| `@Repository` | Mark as repository (optional for JpaRepository) |

---

## One-liner Summary

> **Entity** defines the table structure → **Repository** provides database methods → **Service** adds business logic → **Controller** handles HTTP requests.

---

**Day 3 Complete!**
