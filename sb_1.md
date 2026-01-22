# Day 1: REST APIs & Controllers

## What You'll Learn Today

| Topic | Why Important for Exam |
|-------|----------------------|
| HTTP Methods | MCQ: "Which annotation for POST?" |
| `@PathVariable` vs `@RequestParam` | Debug: "Why is id null?" |
| `@RequestBody` | Debug: "Why is user object empty?" |
| `ResponseEntity` | MCQ: "How to return 201 status?" |
| Status Codes | MCQ: Direct questions |

---

## Part 1: HTTP Methods

### The Problem with `@RequestMapping`

Using `@RequestMapping("/hello")` matches **ALL HTTP methods** (GET, POST, PUT, DELETE). This is bad practice!

### Better Approach: Specific Annotations

| Annotation | HTTP Method | Use Case |
|------------|-------------|----------|
| `@GetMapping` | GET | Fetch data |
| `@PostMapping` | POST | Create new data |
| `@PutMapping` | PUT | Update existing data |
| `@DeleteMapping` | DELETE | Remove data |
| `@PatchMapping` | PATCH | Partial update |

### Exam Trap Alert!

```java
// BAD - accepts all methods
@RequestMapping("/users")

// GOOD - only accepts GET
@GetMapping("/users")

// Also valid - explicit method
@RequestMapping(value = "/users", method = RequestMethod.GET)
```

---

## Part 2: Getting Data from Requests

There are **3 ways** to get data from HTTP requests:

### 1. `@PathVariable` - From URL Path

```
GET /users/123  →  id = 123
```

```java
@GetMapping("/users/{id}")
public User getUser(@PathVariable Long id) { }
```

### 2. `@RequestParam` - From Query String

```
GET /users?name=Dhruv  →  name = "Dhruv"
```

```java
@GetMapping("/users")
public List<User> search(@RequestParam String name) { }
```

### 3. `@RequestBody` - From JSON Body

```
POST /users
Body: {"name": "Dhruv", "email": "d@test.com"}
```

```java
@PostMapping("/users")
public User create(@RequestBody User user) { }
```

### Exam Trap Alert!

- `@PathVariable` - part of URL (`/users/{id}`)
- `@RequestParam` - after `?` (`/users?id=1`)
- `@RequestBody` - JSON in request body (POST/PUT)

---

## Part 3: ResponseEntity

`ResponseEntity` gives you control over:
- Response body
- HTTP status code
- Response headers

### Without ResponseEntity (always returns 200)

```java
@GetMapping("/users/{id}")
public User getUser(@PathVariable Long id) {
    return userService.findById(id);
}
```

### With ResponseEntity (you control status)

```java
@GetMapping("/users/{id}")
public ResponseEntity<User> getUser(@PathVariable Long id) {
    User user = userService.findById(id);
    if (user == null) {
        return ResponseEntity.notFound().build();  // 404
    }
    return ResponseEntity.ok(user);  // 200
}
```

### Common ResponseEntity Methods

| Method | Status Code | When to Use |
|--------|-------------|-------------|
| `ResponseEntity.ok(body)` | 200 | Successful GET |
| `ResponseEntity.created(uri).body(obj)` | 201 | After POST |
| `ResponseEntity.noContent().build()` | 204 | After DELETE |
| `ResponseEntity.badRequest().build()` | 400 | Invalid input |
| `ResponseEntity.notFound().build()` | 404 | Resource not found |
| `ResponseEntity.status(HttpStatus.CREATED).body(obj)` | 201 | Alternative way |

---

## Part 4: HTTP Status Codes (MEMORIZE THESE!)

| Code | Meaning | When Used |
|------|---------|-----------|
| **200** | OK | Successful GET/PUT |
| **201** | Created | After POST creates resource |
| **204** | No Content | After DELETE |
| **400** | Bad Request | Invalid input/validation failed |
| **401** | Unauthorized | Not logged in |
| **403** | Forbidden | Logged in but no permission |
| **404** | Not Found | Resource doesn't exist |
| **405** | Method Not Allowed | Wrong HTTP method |
| **500** | Internal Server Error | Server crashed |

### Memory Trick:
- **2xx** = Success
- **4xx** = Client error (your fault)
- **5xx** = Server error (server's fault)

---

## Part 5: Complete CRUD Controller Example

```java
@RestController
@RequestMapping("/api/products")  // Base URL for all endpoints
public class ProductController {

    private List<Product> products = new ArrayList<>();
    private Long nextId = 1L;

    // GET all products
    // URL: GET /api/products
    @GetMapping
    public ResponseEntity<List<Product>> getAllProducts() {
        return ResponseEntity.ok(products);  // 200 OK
    }

    // GET single product by ID
    // URL: GET /api/products/{id}
    @GetMapping("/{id}")
    public ResponseEntity<Product> getProductById(@PathVariable Long id) {
        for (Product product : products) {
            if (product.getId().equals(id)) {
                return ResponseEntity.ok(product);  // 200 OK
            }
        }
        return ResponseEntity.notFound().build();  // 404 Not Found
    }

    // CREATE new product
    // URL: POST /api/products
    // Body: {"name": "Laptop", "price": 999.99}
    @PostMapping
    public ResponseEntity<Product> createProduct(@RequestBody Product product) {
        product.setId(nextId++);
        products.add(product);
        return ResponseEntity.status(HttpStatus.CREATED).body(product);  // 201 Created
    }

    // UPDATE existing product
    // URL: PUT /api/products/{id}
    @PutMapping("/{id}")
    public ResponseEntity<Product> updateProduct(@PathVariable Long id, @RequestBody Product updatedProduct) {
        for (int i = 0; i < products.size(); i++) {
            if (products.get(i).getId().equals(id)) {
                updatedProduct.setId(id);
                products.set(i, updatedProduct);
                return ResponseEntity.ok(updatedProduct);  // 200 OK
            }
        }
        return ResponseEntity.notFound().build();  // 404 Not Found
    }

    // DELETE product
    // URL: DELETE /api/products/{id}
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteProduct(@PathVariable Long id) {
        for (int i = 0; i < products.size(); i++) {
            if (products.get(i).getId().equals(id)) {
                products.remove(i);
                return ResponseEntity.noContent().build();  // 204 No Content
            }
        }
        return ResponseEntity.notFound().build();  // 404 Not Found
    }

    // SEARCH products by name (demonstrates @RequestParam)
    // URL: GET /api/products/search?name=laptop
    @GetMapping("/search")
    public ResponseEntity<List<Product>> searchProducts(@RequestParam String name) {
        List<Product> results = new ArrayList<>();
        for (Product product : products) {
            if (product.getName().toLowerCase().contains(name.toLowerCase())) {
                results.add(product);
            }
        }
        return ResponseEntity.ok(results);  // 200 OK
    }
}
```

---

## CRUD Endpoints Summary

| HTTP Method | URL | Purpose | Status Code |
|-------------|-----|---------|-------------|
| GET | `/api/products` | Get all products | 200 |
| GET | `/api/products/{id}` | Get one product | 200 or 404 |
| POST | `/api/products` | Create product | 201 |
| PUT | `/api/products/{id}` | Update product | 200 or 404 |
| DELETE | `/api/products/{id}` | Delete product | 204 or 404 |
| GET | `/api/products/search?name=x` | Search by name | 200 |

---

## Quiz Questions

### Q1. Which annotation is used to create a REST controller?
- a) `@Controller`
- b) `@RestController` ✅
- c) `@WebController`
- d) `@ApiController`

> `@Controller` is for MVC (returns views), `@RestController` = `@Controller` + `@ResponseBody` (returns JSON)

### Q2. To get a value from URL path `/users/123`, which annotation is used?
- a) `@RequestParam`
- b) `@RequestBody`
- c) `@PathVariable` ✅
- d) `@QueryParam`

### Q3. What HTTP status code should be returned after successfully creating a resource?
- a) 200
- b) 201 ✅
- c) 204
- d) 202

### Q4. To parse JSON from request body, which annotation is used?
- a) `@PathVariable`
- b) `@RequestParam`
- c) `@RequestBody` ✅
- d) `@JsonBody`

### Q5. Which status code means "resource not found"?
- a) 400
- b) 401
- c) 403
- d) 404 ✅

### Q6. Debug Question - What's wrong with this code?

```java
@PostMapping("/users")
public User createUser(User user) {  // ❌ Missing @RequestBody
    return userService.save(user);
}
```

**Answer:** Missing `@RequestBody`! Without it, Spring won't parse the JSON body.

**Correct code:**
```java
@PostMapping("/users")
public User createUser(@RequestBody User user) {  // ✅ Fixed
    return userService.save(user);
}
```

---

## Key Points to Remember

| Concept | Key Point |
|---------|-----------|
| `@RestController` | Returns JSON (not views) |
| `@GetMapping` | HTTP GET |
| `@PostMapping` | HTTP POST (create) |
| `@PutMapping` | HTTP PUT (update) |
| `@DeleteMapping` | HTTP DELETE |
| `@PathVariable` | From URL path `/users/{id}` |
| `@RequestParam` | From query `?name=value` |
| `@RequestBody` | From JSON body |
| `ResponseEntity` | Control status codes |
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request |
| 404 | Not Found |

---

## Common Debug Errors

| Error | Cause | Fix |
|-------|-------|-----|
| Request body is null | Missing `@RequestBody` | Add `@RequestBody` annotation |
| Path variable is null | Missing `@PathVariable` | Add `@PathVariable` annotation |
| 405 Method Not Allowed | Wrong HTTP method | Check if you're using GET vs POST |
| 415 Unsupported Media Type | Missing Content-Type header | Add `Content-Type: application/json` |

---

**Day 1 Complete!**
