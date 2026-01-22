# Day 4: Custom Queries & Entity Relationships

## Part 1: @Query Annotation - Custom Queries

Sometimes method names aren't enough for complex queries. Use `@Query` to write custom SQL/JPQL!

### JPQL (Java Persistence Query Language)

**JPQL uses Entity names and Field names** (not table/column names)

```java
@Query("SELECT p FROM ProductEntity p WHERE p.price > :minPrice")
List<ProductEntity> findProductsAbovePrice(@Param("minPrice") Double minPrice);
```

**Key Points:**
- Uses class name: `ProductEntity` (not table name `products`)
- Uses field name: `p.price` (not column name `price`)
- Database independent (works with MySQL, PostgreSQL, H2, etc.)
- Named parameters with `@Param`

### Native SQL

**Native SQL uses actual Table names and Column names**

```java
@Query(value = "SELECT * FROM products WHERE price > ?1", nativeQuery = true)
List<ProductEntity> findExpensiveNative(Double minPrice);
```

**Key Points:**
- Uses table name: `products`
- Uses column name: `price`
- Database specific (MySQL syntax ≠ PostgreSQL syntax)
- Positional parameters: `?1`, `?2` or named with `@Param`

### Comparison Table

| Aspect | JPQL | Native SQL |
|--------|------|------------|
| Uses | Entity/Field names | Table/Column names |
| Database | Independent | Specific |
| `nativeQuery` | false (default) | true |
| Example | `SELECT p FROM ProductEntity p` | `SELECT * FROM products` |

### Parameter Binding

**Named Parameters (Recommended):**
```java
@Query("SELECT p FROM ProductEntity p WHERE p.price BETWEEN :min AND :max")
List<ProductEntity> findByRange(@Param("min") Double min, @Param("max") Double max);
```

**Positional Parameters:**
```java
@Query(value = "SELECT * FROM products WHERE price > ?1 AND price < ?2", nativeQuery = true)
List<ProductEntity> findByRange(Double min, Double max);
```

### @Modifying Annotation

**REQUIRED for UPDATE/DELETE queries!**

```java
@Modifying
@Query("UPDATE ProductEntity p SET p.inStock = :status WHERE p.price < :price")
int updateStockStatusByPrice(@Param("status") Boolean status, @Param("price") Double price);

@Modifying
@Query("DELETE FROM ProductEntity p WHERE p.inStock = false")
int deleteOutOfStockProducts();
```

**Important:**
- Must use `@Transactional` in Service layer for `@Modifying` queries
- Returns number of affected rows (int)

### Common @Query Examples

```java
// Simple WHERE
@Query("SELECT p FROM ProductEntity p WHERE p.price > :price")
List<ProductEntity> findExpensive(@Param("price") Double price);

// Multiple conditions
@Query("SELECT p FROM ProductEntity p WHERE p.price BETWEEN :min AND :max AND p.inStock = true")
List<ProductEntity> findAvailableInRange(@Param("min") Double min, @Param("max") Double max);

// LIKE with case-insensitive
@Query("SELECT p FROM ProductEntity p WHERE LOWER(p.name) LIKE LOWER(CONCAT('%', :keyword, '%'))")
List<ProductEntity> searchByKeyword(@Param("keyword") String keyword);

// ORDER BY
@Query("SELECT p FROM ProductEntity p WHERE p.inStock = true ORDER BY p.price ASC")
List<ProductEntity> findAllInStockOrdered();

// COUNT
@Query("SELECT COUNT(p) FROM ProductEntity p WHERE p.price > :price")
Long countExpensive(@Param("price") Double price);
```

---

## Part 2: Entity Relationships

Real-world entities are related! Example: One Category has Many Products.

### @ManyToOne Relationship

**Many Products belong to One Category**

```java
@Entity
public class ProductEntity {
    @Id
    private Long id;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "category_id")  // Foreign key column
    private CategoryEntity category;
}
```

**Key Points:**
- **Owning side** - Has the foreign key column
- `@JoinColumn` creates `category_id` column in products table
- Default fetch: **EAGER** (but use LAZY for performance!)

### @OneToMany Relationship

**One Category has Many Products**

```java
@Entity
public class CategoryEntity {
    @Id
    private Long id;
    
    @OneToMany(mappedBy = "category", cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    @JsonIgnore  // Prevent infinite recursion in JSON
    private List<ProductEntity> products = new ArrayList<>();
}
```

**Key Points:**
- **Non-owning side** - Uses `mappedBy`
- `mappedBy = "category"` points to field in ProductEntity
- Does NOT create foreign key (already exists in ProductEntity)
- Default fetch: **LAZY**

### Relationship Summary

| Annotation | Meaning | Has Foreign Key? | Default Fetch |
|------------|---------|------------------|---------------|
| `@ManyToOne` | Many → One | YES (this side) | EAGER |
| `@OneToMany` | One → Many | NO (mappedBy) | LAZY |
| `@OneToOne` | One → One | Either side | EAGER |
| `@ManyToMany` | Many → Many | Join table | LAZY |

### mappedBy Explained

```java
// CategoryEntity (One side)
@OneToMany(mappedBy = "category")  // Points to "category" field in ProductEntity
private List<ProductEntity> products;

// ProductEntity (Many side)
@ManyToOne
@JoinColumn(name = "category_id")  // This creates the foreign key
private CategoryEntity category;  // ← mappedBy points here!
```

**Rule:** `mappedBy` always points to the field in the **owning side** (the side with `@JoinColumn`).

### Fetch Types: LAZY vs EAGER

**LAZY Loading:**
- Loads related data **only when accessed**
- Better for performance
- Default for `@OneToMany`

```java
@ManyToOne(fetch = FetchType.LAZY)
private CategoryEntity category;

// Category is NOT loaded until you call:
product.getCategory();  // ← Now it loads!
```

**EAGER Loading:**
- Loads related data **immediately**
- Can cause N+1 query problem
- Default for `@ManyToOne` (but use LAZY!)

```java
@ManyToOne(fetch = FetchType.EAGER)
private CategoryEntity category;

// Category is loaded immediately with Product
Product p = repo.findById(1);  // Category already loaded!
```

**Best Practice:** Always use **LAZY** unless you really need EAGER!

### Cascade Types

```java
@OneToMany(cascade = CascadeType.ALL)
private List<ProductEntity> products;
```

| Cascade Type | What it does |
|--------------|--------------|
| `ALL` | All operations cascade (save, update, delete) |
| `PERSIST` | Only save cascades |
| `MERGE` | Only update cascades |
| `REMOVE` | Only delete cascades |
| `REFRESH` | Only refresh cascades |
| `DETACH` | Only detach cascades |

**Example:**
```java
Category category = new Category();
category.addProduct(new Product());  // Add product
categoryRepository.save(category);   // Product is also saved! (cascade)
```

---

## Part 3: Pagination

When you have thousands of records, you need pagination!

### Pageable Interface

```java
Pageable pageable = PageRequest.of(page, size);
// page = page number (0-based)
// size = items per page
```

### Page<T> Interface

```java
Page<ProductEntity> page = productRepository.findAll(pageable);
```

**Page contains:**
- `content` - List of products for this page
- `totalPages` - Total number of pages
- `totalElements` - Total number of products
- `number` - Current page number
- `size` - Page size
- `first` - Is this first page?
- `last` - Is this last page?

### Pagination Examples

**Basic Pagination:**
```java
Pageable pageable = PageRequest.of(0, 10);  // Page 0, 10 items
Page<ProductEntity> page = productRepository.findAll(pageable);
```

**With Sorting:**
```java
Sort sort = Sort.by("price").descending();
Pageable pageable = PageRequest.of(0, 10, sort);
Page<ProductEntity> page = productRepository.findAll(pageable);
```

**Multiple Sort Fields:**
```java
Sort sort = Sort.by("price").descending().and(Sort.by("name").ascending());
Pageable pageable = PageRequest.of(0, 10, sort);
```

**In Repository:**
```java
Page<ProductEntity> findByInStock(Boolean inStock, Pageable pageable);
```

**In Service:**
```java
public Page<ProductEntity> findAllPaged(int page, int size) {
    Pageable pageable = PageRequest.of(page, size);
    return productRepository.findAll(pageable);
}
```

**In Controller:**
```java
@GetMapping("/paged")
public ResponseEntity<Page<ProductEntity>> getPaged(
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "10") int size) {
    return ResponseEntity.ok(productService.findAllPaged(page, size));
}
```

### Pagination Response Example

```json
{
  "content": [
    {"id": 1, "name": "Product 1", "price": 100},
    {"id": 2, "name": "Product 2", "price": 200}
  ],
  "totalPages": 5,
  "totalElements": 50,
  "number": 0,
  "size": 10,
  "first": true,
  "last": false
}
```

---

## Common Exam Traps

### Trap 1: Missing @Modifying for UPDATE
```java
// ❌ WRONG
@Query("UPDATE ProductEntity p SET p.price = 0")
int resetPrices();  // Won't work!

// ✅ CORRECT
@Modifying
@Query("UPDATE ProductEntity p SET p.price = 0")
int resetPrices();
```

### Trap 2: Missing @Transactional for @Modifying
```java
// ❌ WRONG - Service method
public int updatePrices() {
    return productRepository.updatePrices();  // Error!
}

// ✅ CORRECT
@Transactional
public int updatePrices() {
    return productRepository.updatePrices();
}
```

### Trap 3: Wrong side has foreign key
```java
// ❌ WRONG - @OneToMany side can't have @JoinColumn
@OneToMany
@JoinColumn(name = "category_id")  // Error!
private List<Product> products;

// ✅ CORRECT - @ManyToOne side has foreign key
@ManyToOne
@JoinColumn(name = "category_id")
private Category category;
```

### Trap 4: Missing mappedBy
```java
// ❌ WRONG - Both sides try to own relationship
@OneToMany
@JoinColumn(name = "category_id")  // Error!
private List<Product> products;

@ManyToOne
@JoinColumn(name = "category_id")  // Error!
private Category category;

// ✅ CORRECT - Use mappedBy
@OneToMany(mappedBy = "category")
private List<Product> products;

@ManyToOne
@JoinColumn(name = "category_id")
private Category category;
```

### Trap 5: Infinite JSON recursion
```java
// ❌ WRONG - No @JsonIgnore
@OneToMany(mappedBy = "category")
private List<Product> products;  // Category → Products → Category → Products... (infinite!)

// ✅ CORRECT
@OneToMany(mappedBy = "category")
@JsonIgnore
private List<Product> products;
```

---

## Quiz Questions with Answers

### Q1. What's the difference between JPQL and Native SQL?
**Answer:** JPQL uses entity/field names (database independent), Native SQL uses table/column names (database specific).

### Q2. What annotation is required for UPDATE queries?
**Answer:** `@Modifying` (along with `@Query`)

### Q3. What's the default fetch type for @ManyToOne?
**Answer:** EAGER (but best practice is LAZY)

### Q4. What's the default fetch type for @OneToMany?
**Answer:** LAZY

### Q5. Which side has the foreign key in @ManyToOne/@OneToMany?
**Answer:** The @ManyToOne side (the "many" side)

### Q6. What does mappedBy do?
**Answer:** Points to the field in the child entity that owns the relationship

### Q7. What does Pageable contain?
**Answer:** Page number, page size, and sort information

### Q8. What does Page<T> return include?
**Answer:** Content, totalPages, totalElements, and metadata

### Q9. What's wrong with this @Query?
```java
@Query("UPDATE Product p SET p.price = 0")
int resetPrices();
```
**Answer:** Missing `@Modifying` annotation!

### Q10. What's wrong with this relationship?
```java
@ManyToOne
private Category category;
```
**Answer:** Missing `@JoinColumn` to specify foreign key column name!

---

## Key Annotations Summary

| Annotation | Purpose |
|------------|---------|
| `@Query` | Custom SQL/JPQL query |
| `@Param` | Named parameter in query |
| `@Modifying` | Required for UPDATE/DELETE queries |
| `@ManyToOne` | Many entities → One entity |
| `@OneToMany` | One entity → Many entities |
| `@JoinColumn` | Foreign key column name |
| `@JsonIgnore` | Prevent infinite recursion in JSON |
| `FetchType.LAZY` | Load on demand |
| `FetchType.EAGER` | Load immediately |
| `CascadeType.ALL` | Cascade all operations |
| `Pageable` | Pagination info |
| `Page<T>` | Paginated results |

---

## Project Structure After Day 4

```
entity/
├── ProductEntity.java      # @ManyToOne relationship added
└── CategoryEntity.java     # @OneToMany relationship added

repository/
├── ProductRepository.java  # @Query methods added
└── CategoryRepository.java # Basic queries

service/
└── ProductDbService.java   # Pagination methods added

controller/
├── ProductDbController.java # Pagination endpoints
└── CategoryController.java   # Category CRUD
```

---

**Day 4 Complete!**
