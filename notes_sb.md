# Spring Boot - Multiple Correct Answer MCQs

## Exam Guidelines
- **Total Questions:** 50
- **Type:** Multiple Correct Answers (Select ALL that apply)
- **Difficulty:** Medium to Hard
- **Topics Focus:** Controller, Service Layer, Repository, DTO, Mapping, Caching & Core Concepts

---

## Section 1: Controllers & REST API Development

### Q1. Which annotations can be used to map HTTP requests to handler methods in a Spring Boot controller?
- [ ] A) `@RequestMapping`
- [ ] B) `@GetMapping`
- [ ] C) `@Controller`
- [ ] D) `@PostMapping`
- [ ] E) `@RestController`

<details>
<summary>Answer</summary>

**Correct: A, B, D**

`@RequestMapping`, `@GetMapping`, and `@PostMapping` are HTTP mapping annotations. `@Controller` and `@RestController` are class-level annotations that declare a class as a controller, not method-level request mappings.
</details>

---

### Q2. What is true about `@RestController` annotation in Spring Boot?
- [ ] A) It combines `@Controller` and `@ResponseBody`
- [ ] B) It automatically serializes return objects to JSON/XML
- [ ] C) It requires `@ResponseBody` on each method for JSON response
- [ ] D) It can only be used with Spring WebFlux
- [ ] E) All handler methods return responses written directly to the response body

<details>
<summary>Answer</summary>

**Correct: A, B, E**

`@RestController` is a convenience annotation that combines `@Controller` and `@ResponseBody`. It eliminates the need for `@ResponseBody` on each method. It works with both Spring MVC and WebFlux.
</details>

---

### Q3. Which statements are correct about `@RequestBody` and `@ResponseBody`?
- [ ] A) `@RequestBody` binds the HTTP request body to a method parameter
- [ ] B) `@ResponseBody` is required in `@RestController` annotated classes
- [ ] C) `@RequestBody` uses `HttpMessageConverter` to deserialize the request
- [ ] D) `@ResponseBody` writes the return value directly to the HTTP response body
- [ ] E) `@RequestBody` can only accept JSON content type

<details>
<summary>Answer</summary>

**Correct: A, C, D**

`@RequestBody` binds request body to parameter using `HttpMessageConverter`. `@ResponseBody` is NOT required in `@RestController` (it's implicit). `@RequestBody` can handle any content type supported by configured message converters (JSON, XML, etc.).
</details>

---

### Q4. Which are valid ways to extract data from an incoming HTTP request in a controller method?
- [ ] A) `@PathVariable` for URI template variables
- [ ] B) `@RequestParam` for query parameters
- [ ] C) `@RequestHeader` for HTTP headers
- [ ] D) `@CookieValue` for cookie values
- [ ] E) `@RequestAttribute` for query parameters

<details>
<summary>Answer</summary>

**Correct: A, B, C, D**

All except E are correct. `@RequestAttribute` is used to access pre-existing request attributes (set by filters/interceptors), not query parameters.
</details>

---

### Q5. What happens when a controller method returns `ResponseEntity<T>`?
- [ ] A) You can set custom HTTP status codes
- [ ] B) You can add custom response headers
- [ ] C) The response body is automatically wrapped in an additional JSON object
- [ ] D) You have full control over the HTTP response
- [ ] E) It bypasses content negotiation

<details>
<summary>Answer</summary>

**Correct: A, B, D**

`ResponseEntity` provides full control over HTTP response including status, headers, and body. It does NOT wrap the body in additional JSON and does NOT bypass content negotiation.
</details>

---

### Q6. Which statements are true about `@RequestMapping` attributes?
- [ ] A) `method` attribute specifies allowed HTTP methods
- [ ] B) `produces` attribute specifies the response content type
- [ ] C) `consumes` attribute restricts the request content type
- [ ] D) `params` attribute can filter requests based on query parameters
- [ ] E) `value` and `path` attributes serve different purposes

<details>
<summary>Answer</summary>

**Correct: A, B, C, D**

All attributes work as described. `value` and `path` are aliases for each other and serve the same purpose (specifying the URL pattern).
</details>

---

### Q7. In a REST controller, which approaches correctly return a 404 status when a resource is not found?
- [ ] A) Return `ResponseEntity.notFound().build()`
- [ ] B) Throw `ResponseStatusException(HttpStatus.NOT_FOUND)`
- [ ] C) Return `null` from the handler method
- [ ] D) Annotate the exception class with `@ResponseStatus(HttpStatus.NOT_FOUND)`
- [ ] E) Return `Optional.empty()`

<details>
<summary>Answer</summary>

**Correct: A, B, D**

Returning `null` typically results in 200 with empty body. Returning `Optional.empty()` doesn't automatically translate to 404. Custom exceptions with `@ResponseStatus` and explicit ResponseEntity/ResponseStatusException are correct approaches.
</details>

---

## Section 2: Service Layer & Dependency Injection

### Q8. Which statements are true about the `@Service` annotation?
- [ ] A) It is a specialization of `@Component`
- [ ] B) It enables transaction management automatically
- [ ] C) It indicates that the class holds business logic
- [ ] D) Spring creates a singleton bean by default
- [ ] E) It's required for a class to be injected as a dependency

<details>
<summary>Answer</summary>

**Correct: A, C, D**

`@Service` is a `@Component` specialization indicating business logic. Beans are singleton by default. It does NOT enable transactions automatically (`@Transactional` is needed). Any `@Component` stereotype can be injected, not just `@Service`.
</details>

---

### Q9. Which are valid ways to inject dependencies in Spring Boot?
- [ ] A) Constructor injection
- [ ] B) Setter injection with `@Autowired`
- [ ] C) Field injection with `@Autowired`
- [ ] D) Method injection with `@Autowired`
- [ ] E) Using `@Inject` annotation from Jakarta

<details>
<summary>Answer</summary>

**Correct: A, B, C, D, E**

All are valid injection methods. Constructor injection is recommended. `@Inject` (Jakarta/JSR-330) is supported as an alternative to `@Autowired`.
</details>

---

### Q10. Why is constructor injection preferred over field injection?
- [ ] A) Dependencies are explicitly declared and visible
- [ ] B) Objects can be instantiated in an invalid state with field injection
- [ ] C) Constructor injection allows for immutable dependencies (final fields)
- [ ] D) Field injection makes unit testing without Spring context easier
- [ ] E) Constructor injection makes circular dependencies fail fast

<details>
<summary>Answer</summary>

**Correct: A, B, C, E**

Constructor injection makes dependencies explicit, ensures valid state, allows final fields, and fails fast on circular dependencies. Field injection (not constructor) makes testing without Spring harder as you need reflection to set private fields.
</details>

---

### Q11. What is true about Spring bean scopes?
- [ ] A) `singleton` creates one instance per Spring IoC container
- [ ] B) `prototype` creates a new instance each time the bean is requested
- [ ] C) `request` scope is available only in web-aware ApplicationContext
- [ ] D) `session` scoped beans are shared across all HTTP sessions
- [ ] E) `prototype` beans are not fully managed by Spring (no destruction callbacks)

<details>
<summary>Answer</summary>

**Correct: A, B, C, E**

`session` scoped beans are per HTTP session, NOT shared across sessions. Prototype beans don't receive destruction callbacks from Spring since the container doesn't track them after creation.
</details>

---

### Q12. Which scenarios correctly demonstrate the service layer's responsibilities?
- [ ] A) Orchestrating multiple repository calls in a single business operation
- [ ] B) Directly handling HTTP request/response objects
- [ ] C) Implementing transaction boundaries using `@Transactional`
- [ ] D) Performing business validation and applying business rules
- [ ] E) Converting entities to DTOs before returning to controller

<details>
<summary>Answer</summary>

**Correct: A, C, D, E**

Service layer handles business logic, transactions, validation, and can handle DTO conversion. HTTP request/response handling belongs to the controller layer, not service layer.
</details>

---

### Q13. What happens when you have multiple beans of the same type and use `@Autowired`?
- [ ] A) Spring throws `NoUniqueBeanDefinitionException` at startup
- [ ] B) `@Primary` can designate a default bean
- [ ] C) `@Qualifier` can specify which bean to inject
- [ ] D) Spring automatically chooses the first bean found
- [ ] E) Field name matching can be used as a fallback resolution strategy

<details>
<summary>Answer</summary>

**Correct: A, B, C, E**

Without disambiguation, Spring throws an exception. `@Primary`, `@Qualifier`, and field name matching are valid resolution strategies. Spring does NOT automatically choose the first bean.
</details>

---

### Q14. Which statements about `@Transactional` are correct?
- [ ] A) By default, it rolls back only on unchecked exceptions
- [ ] B) It can be applied at class level to affect all public methods
- [ ] C) Self-invocation within the same class triggers transaction proxy
- [ ] D) `propagation = REQUIRES_NEW` always creates a new transaction
- [ ] E) `readOnly = true` can enable database optimizations

<details>
<summary>Answer</summary>

**Correct: A, B, D, E**

Self-invocation does NOT trigger the transaction proxy (it bypasses AOP). The method must be called from outside the class for `@Transactional` to work. Default rollback is for unchecked exceptions (RuntimeException and Error).
</details>

---

## Section 3: Repository Layer & Spring Data JPA

### Q15. Which methods are provided by `JpaRepository` interface?
- [ ] A) `save()` and `saveAll()`
- [ ] B) `findById()` and `findAll()`
- [ ] C) `deleteById()` and `delete()`
- [ ] D) `executeNativeQuery()`
- [ ] E) `flush()` and `saveAndFlush()`

<details>
<summary>Answer</summary>

**Correct: A, B, C, E**

`JpaRepository` provides CRUD methods, batch operations, and flush operations. `executeNativeQuery()` is not a method of `JpaRepository`; native queries use `@Query` annotation.
</details>

---

### Q16. Which are valid ways to define custom queries in Spring Data JPA?
- [ ] A) Query methods derived from method names (e.g., `findByEmailAndStatus`)
- [ ] B) `@Query` annotation with JPQL
- [ ] C) `@Query` annotation with native SQL using `nativeQuery = true`
- [ ] D) `@NamedQuery` defined on entity classes
- [ ] E) Implementing `JpaRepository` methods manually

<details>
<summary>Answer</summary>

**Correct: A, B, C, D**

All are valid query definition methods. You don't implement `JpaRepository` methods manually; Spring Data provides the implementation automatically. You extend the interface, not implement it.
</details>

---

### Q17. What is true about derived query methods in Spring Data JPA?
- [ ] A) `findByFirstNameAndLastName` creates an AND condition
- [ ] B) `findByAgeGreaterThan` creates a > comparison
- [ ] C) `findByStatusOrderByCreatedAtDesc` includes sorting
- [ ] D) `findFirst5ByStatus` limits results to 5
- [ ] E) `findByNameContaining` performs exact match

<details>
<summary>Answer</summary>

**Correct: A, B, C, D**

`Containing` creates a LIKE query with wildcards (e.g., `%value%`), not an exact match. All other statements correctly describe derived query method keywords.
</details>

---

### Q18. Which statements about JPA entity mapping are correct?
- [ ] A) `@Entity` annotation is mandatory for JPA entities
- [ ] B) `@Id` annotation marks the primary key field
- [ ] C) `@GeneratedValue(strategy = GenerationType.IDENTITY)` uses database auto-increment
- [ ] D) `@Column` annotation is required for all entity fields
- [ ] E) `@Table` annotation is required to specify the table name

<details>
<summary>Answer</summary>

**Correct: A, B, C**

`@Column` is optional; JPA uses the field name by default. `@Table` is optional; JPA uses the class name by default. Only `@Entity` and `@Id` are mandatory.
</details>

---

### Q19. What is true about JPA relationship mappings?
- [ ] A) `@OneToMany` default fetch type is LAZY
- [ ] B) `@ManyToOne` default fetch type is EAGER
- [ ] C) `mappedBy` attribute indicates the owning side of the relationship
- [ ] D) `@JoinColumn` specifies the foreign key column
- [ ] E) Bidirectional relationships require mapping on both sides

<details>
<summary>Answer</summary>

**Correct: A, B, D, E**

`mappedBy` indicates the INVERSE (non-owning) side, not the owning side. The owning side is where `@JoinColumn` is defined.
</details>

---

### Q20. Which statements about pagination in Spring Data are correct?
- [ ] A) `Pageable` parameter enables pagination in repository methods
- [ ] B) `Page<T>` return type includes total element count
- [ ] C) `Slice<T>` return type includes total element count
- [ ] D) `PageRequest.of(0, 10)` creates a request for the first 10 elements
- [ ] E) `Sort.by("name").descending()` can be combined with pagination

<details>
<summary>Answer</summary>

**Correct: A, B, D, E**

`Slice<T>` does NOT include total count (only knows if there's a next slice). `Page<T>` extends `Slice<T>` and adds total count. `Slice` is more efficient when total count isn't needed.
</details>

---

### Q21. What are the implications of the N+1 problem in JPA?
- [ ] A) It occurs when lazy loading triggers individual queries for each association
- [ ] B) `@EntityGraph` can help solve this problem
- [ ] C) JOIN FETCH in JPQL can solve this problem
- [ ] D) It always occurs with EAGER fetch type
- [ ] E) Batch fetching (`@BatchSize`) can mitigate the problem

<details>
<summary>Answer</summary>

**Correct: A, B, C, E**

N+1 occurs with LAZY fetching when associations are accessed. EAGER doesn't cause N+1 as it loads everything upfront (though it may cause other issues). `@EntityGraph`, JOIN FETCH, and `@BatchSize` are valid solutions.
</details>

---

## Section 4: DTO Layer & Object Mapping

### Q22. What are the main purposes of using DTOs (Data Transfer Objects)?
- [ ] A) Decouple internal entity structure from API contract
- [ ] B) Prevent exposing sensitive entity fields in API responses
- [ ] C) Reduce data transferred by selecting only needed fields
- [ ] D) Replace entities for database operations
- [ ] E) Shape data according to specific client requirements

<details>
<summary>Answer</summary>

**Correct: A, B, C, E**

DTOs are for data transfer and API contracts, NOT for database operations. Entities are used for database operations. DTOs help with decoupling, security, and customizing data shape.
</details>

---

### Q23. Which are best practices for DTO design?
- [ ] A) DTOs should be immutable when possible
- [ ] B) Use separate DTOs for request and response
- [ ] C) DTOs should extend entity classes for code reuse
- [ ] D) Validation annotations should be placed on request DTOs
- [ ] E) DTOs should not contain business logic

<details>
<summary>Answer</summary>

**Correct: A, B, D, E**

DTOs should NOT extend entities; they should be completely separate classes. This violates the decoupling principle that DTOs provide. Immutability, separation of request/response DTOs, and validation on request DTOs are best practices.
</details>

---

### Q24. Which statements about MapStruct are correct?
- [ ] A) It generates mapping code at compile time
- [ ] B) It uses reflection for mapping at runtime
- [ ] C) `@Mapper` annotation defines a mapper interface
- [ ] D) `@Mapping` annotation handles field name mismatches
- [ ] E) It can be integrated with Spring using `componentModel = "spring"`

<details>
<summary>Answer</summary>

**Correct: A, C, D, E**

MapStruct generates code at compile time, NOT runtime reflection. This makes it faster than reflection-based mappers like ModelMapper. All other statements are correct.
</details>

---

### Q25. When using ModelMapper for DTO conversion, which statements are true?
- [ ] A) It uses reflection for object mapping
- [ ] B) It automatically maps fields with matching names
- [ ] C) Complex mappings require custom `PropertyMap` configuration
- [ ] D) It generates mapping code at compile time
- [ ] E) It can be configured with different matching strategies

<details>
<summary>Answer</summary>

**Correct: A, B, C, E**

ModelMapper uses reflection at runtime, NOT compile-time code generation. It supports automatic mapping, custom configurations, and different matching strategies (strict, standard, loose).
</details>

---

### Q26. Which patterns are valid for implementing entity-to-DTO conversion in Spring Boot?
- [ ] A) Static mapper methods in the DTO class
- [ ] B) Dedicated mapper/converter Spring beans
- [ ] C) MapStruct generated mappers
- [ ] D) Constructor-based mapping in the DTO
- [ ] E) Direct type casting from entity to DTO

<details>
<summary>Answer</summary>

**Correct: A, B, C, D**

Entity and DTO are different classes; direct casting is not possible and will throw `ClassCastException`. All other approaches are valid patterns for entity-DTO conversion.
</details>

---

### Q27. What should be considered when mapping nested objects (e.g., entity relationships to nested DTOs)?
- [ ] A) Lazy loading may trigger additional database queries
- [ ] B) Circular references need special handling to avoid infinite loops
- [ ] C) MapStruct requires explicit mapping for nested objects
- [ ] D) All nested associations should always be fully loaded
- [ ] E) Different DTO depths may be needed for different use cases

<details>
<summary>Answer</summary>

**Correct: A, B, E**

MapStruct handles nested objects automatically if types match; explicit mapping is needed only for custom conversion. Loading all associations always is bad practice (over-fetching). LazyInitializationException and circular references are real concerns.
</details>

---

## Section 5: Caching

### Q28. Which statements about Spring Cache abstraction are correct?
- [ ] A) `@EnableCaching` activates the caching infrastructure
- [ ] B) `@Cacheable` stores method return values in cache
- [ ] C) `@CacheEvict` removes entries from the cache
- [ ] D) `@CachePut` always executes the method and updates the cache
- [ ] E) Spring Cache requires Redis as the backing store

<details>
<summary>Answer</summary>

**Correct: A, B, C, D**

Spring Cache is an abstraction that works with multiple cache providers (ConcurrentHashMap, EhCache, Caffeine, Redis, etc.). Redis is NOT required; you can use simple in-memory caches.
</details>

---

### Q29. What is true about the `@Cacheable` annotation?
- [ ] A) The method is executed only if the result is not in cache
- [ ] B) `key` attribute defines the cache key using SpEL
- [ ] C) `condition` attribute can prevent caching based on a condition
- [ ] D) It works on private methods without any additional configuration
- [ ] E) `unless` attribute can prevent caching based on the result

<details>
<summary>Answer</summary>

**Correct: A, B, C, E**

`@Cacheable` doesn't work on private methods because Spring uses AOP proxies. The method must be public and called from outside the class. `condition` evaluates before execution; `unless` evaluates after.
</details>

---

### Q30. Which caching scenarios are implemented correctly?

```java
// Scenario 1
@Cacheable(value = "users", key = "#id")
public User getUserById(Long id)

// Scenario 2
@CacheEvict(value = "users", allEntries = true)
public void deleteUser(Long id)

// Scenario 3
@Cacheable(value = "users")
public User getUserById(Long id, boolean forceRefresh)

// Scenario 4
@CachePut(value = "users", key = "#user.id")
public User updateUser(User user)
```

- [ ] A) Scenario 1 correctly caches by user ID
- [ ] B) Scenario 2 correctly evicts all user cache entries
- [ ] C) Scenario 3 will cache ignoring the `forceRefresh` parameter
- [ ] D) Scenario 4 always updates the cache after method execution
- [ ] E) Scenario 3 will create different cache entries for different `forceRefresh` values

<details>
<summary>Answer</summary>

**Correct: A, B, D, E**

Scenario 3 without explicit key uses all parameters (including `forceRefresh`) as the cache key, so different `forceRefresh` values create different cache entries. This is usually unintended behavior.
</details>

---

### Q31. Which are valid strategies for cache invalidation?
- [ ] A) Time-based expiration (TTL)
- [ ] B) Manual eviction using `@CacheEvict`
- [ ] C) Event-driven invalidation on data updates
- [ ] D) Automatic invalidation when entity is updated via JPA
- [ ] E) Using `@CacheEvict(beforeInvocation = true)` for guaranteed eviction

<details>
<summary>Answer</summary>

**Correct: A, B, C, E**

JPA does NOT automatically invalidate Spring caches when entities are updated. This is a common misconception. Cache invalidation must be explicitly handled. `beforeInvocation = true` ensures eviction even if method throws exception.
</details>

---

### Q32. What are potential issues when using caching incorrectly?
- [ ] A) Stale data when cache is not properly invalidated
- [ ] B) Memory issues with unbounded caches
- [ ] C) Cache stampede when many requests hit missing cache simultaneously
- [ ] D) Automatic performance improvement for all methods
- [ ] E) Serialization issues with complex objects in distributed caches

<details>
<summary>Answer</summary>

**Correct: A, B, C, E**

Caching does NOT automatically improve performance for all methods. Methods that rarely repeat, have side effects, or return frequently changing data may not benefit from caching. All listed issues are real caching concerns.
</details>

---

## Section 6: Spring Security & JWT

### Q33. What is the difference between authentication and authorization?
- [ ] A) Authentication verifies WHO the user is
- [ ] B) Authorization determines WHAT the user can access
- [ ] C) Authentication always happens before authorization
- [ ] D) Authorization can work without authentication
- [ ] E) Both are handled by the same security filter

<details>
<summary>Answer</summary>

**Correct: A, B, C**

Authorization typically requires knowing who the user is first (authentication). They are handled by different filters in the security filter chain (`UsernamePasswordAuthenticationFilter` vs authorization filters).
</details>

---

### Q34. Which statements about JWT (JSON Web Token) are correct?
- [ ] A) JWT consists of three parts: Header, Payload, and Signature
- [ ] B) The payload is encrypted and cannot be read without the secret key
- [ ] C) The signature verifies the token hasn't been tampered with
- [ ] D) JWTs are stateless and don't require server-side session storage
- [ ] E) The secret key used for signing should be stored securely

<details>
<summary>Answer</summary>

**Correct: A, C, D, E**

JWT payload is Base64 encoded, NOT encrypted. Anyone can decode and read the payload. Only the signature provides integrity verification. Sensitive data should not be stored in JWT payload without additional encryption.
</details>

---

### Q35. In a typical JWT authentication flow, which steps are correct?
- [ ] A) User sends credentials, server validates and returns JWT
- [ ] B) Client stores JWT and sends it in Authorization header for subsequent requests
- [ ] C) Server validates JWT signature on each request
- [ ] D) JWT payload should contain sensitive data like passwords
- [ ] E) Token expiration should be checked during validation

<details>
<summary>Answer</summary>

**Correct: A, B, C, E**

NEVER store passwords or sensitive data in JWT payload since it's not encrypted. JWT should contain claims like user ID, roles, expiration time, etc.
</details>

---

### Q36. Which are correct ways to secure REST endpoints in Spring Security?
- [ ] A) Using `@PreAuthorize("hasRole('ADMIN')")` on methods
- [ ] B) Configuring URL patterns in `SecurityFilterChain`
- [ ] C) Using `@Secured("ROLE_ADMIN")` annotation
- [ ] D) Using `@RolesAllowed("ADMIN")` annotation
- [ ] E) Making all methods private

<details>
<summary>Answer</summary>

**Correct: A, B, C, D**

Making methods private doesn't secure endpoints; it makes them inaccessible. Spring Security provides multiple ways to secure endpoints: URL-based configuration and method-level annotations (`@PreAuthorize`, `@Secured`, `@RolesAllowed`).
</details>

---

### Q37. What is true about Spring Security filter chain?
- [ ] A) Filters are executed in a specific order
- [ ] B) `OncePerRequestFilter` ensures the filter runs once per request
- [ ] C) Custom JWT filters should typically be added before `UsernamePasswordAuthenticationFilter`
- [ ] D) Filter chain processing stops after authentication succeeds
- [ ] E) `SecurityContext` holds the authenticated user's details

<details>
<summary>Answer</summary>

**Correct: A, B, C, E**

Filter chain processing continues after authentication; authorization filters and other filters still run. The chain continues to the servlet/controller after all filters pass.
</details>

---

## Section 7: Validation & Error Handling

### Q38. Which validation annotations are provided by Jakarta Bean Validation?
- [ ] A) `@NotNull` - field must not be null
- [ ] B) `@NotBlank` - string must not be null and must contain non-whitespace
- [ ] C) `@Email` - must be valid email format
- [ ] D) `@Size` - collection or string size constraints
- [ ] E) `@Unique` - field must be unique in database

<details>
<summary>Answer</summary>

**Correct: A, B, C, D**

`@Unique` is not a standard Bean Validation annotation. Database uniqueness is typically enforced at the database level with unique constraints, not through Bean Validation.
</details>

---

### Q39. How do you properly enable validation on request DTOs in Spring Boot?
- [ ] A) Add `@Valid` annotation on the `@RequestBody` parameter
- [ ] B) Add `@Validated` annotation on the controller class
- [ ] C) Validation is automatic for all `@RequestBody` parameters
- [ ] D) Add validation annotations directly on entity fields only
- [ ] E) Use `@Valid` for nested object validation

<details>
<summary>Answer</summary>

**Correct: A, B, E**

Validation is NOT automatic; you must use `@Valid` or `@Validated`. Validation annotations should be on DTOs, not only entities. `@Valid` triggers nested object validation recursively.
</details>

---

### Q40. Which approaches correctly implement global exception handling in Spring Boot?
- [ ] A) `@ControllerAdvice` class with `@ExceptionHandler` methods
- [ ] B) `@RestControllerAdvice` for REST APIs
- [ ] C) Implementing `ErrorController` interface
- [ ] D) Adding try-catch in every controller method
- [ ] E) Using `ResponseEntityExceptionHandler` as a base class

<details>
<summary>Answer</summary>

**Correct: A, B, C, E**

Adding try-catch in every controller method works but is NOT the correct approach - it's repetitive and violates DRY principle. Global exception handlers centralize error handling logic.
</details>

---

### Q41. What happens when validation fails for a `@RequestBody` parameter annotated with `@Valid`?
- [ ] A) `MethodArgumentNotValidException` is thrown
- [ ] B) Returns 400 Bad Request by default
- [ ] C) The method still executes with invalid data
- [ ] D) `BindingResult` can capture validation errors if added as parameter
- [ ] E) All validation errors are collected before throwing exception

<details>
<summary>Answer</summary>

**Correct: A, B, D, E**

The method does NOT execute with invalid data when validation fails. If `BindingResult` is added immediately after the validated parameter, you can handle errors manually instead of automatic exception.
</details>

---

## Section 8: Configuration & Properties

### Q42. Which statements about Spring Boot configuration are correct?
- [ ] A) `application.properties` and `application.yml` serve the same purpose
- [ ] B) Properties can be injected using `@Value` annotation
- [ ] C) `@ConfigurationProperties` provides type-safe configuration binding
- [ ] D) Environment variables override application.properties values
- [ ] E) `application-{profile}.properties` files are for environment-specific config

<details>
<summary>Answer</summary>

**Correct: A, B, C, D, E**

All statements are correct. Spring Boot has a property source hierarchy where environment variables have higher precedence than application.properties.
</details>

---

### Q43. What is true about Spring Profiles?
- [ ] A) Active profile can be set using `spring.profiles.active`
- [ ] B) `@Profile` annotation conditionally enables beans
- [ ] C) Multiple profiles can be active simultaneously
- [ ] D) Default profile is used when no profile is specified
- [ ] E) Profile-specific properties always override common properties

<details>
<summary>Answer</summary>

**Correct: A, B, C, D, E**

All statements are correct. Profiles provide powerful configuration management for different environments. Profile-specific files (application-dev.properties) override common properties.
</details>

---

### Q44. Which are valid ways to externalize configuration in Spring Boot?
- [ ] A) Command line arguments (`--server.port=8081`)
- [ ] B) Environment variables (`SERVER_PORT=8081`)
- [ ] C) Properties files in `config/` subdirectory
- [ ] D) JNDI attributes
- [ ] E) Hardcoding values in Java classes

<details>
<summary>Answer</summary>

**Correct: A, B, C, D**

Hardcoding values is NOT externalization - it's the opposite. Spring Boot supports many external configuration sources with a defined precedence order.
</details>

---

## Section 9: Advanced Topics

### Q45. Which statements about `@Async` in Spring Boot are correct?
- [ ] A) `@EnableAsync` must be added to enable async processing
- [ ] B) Async methods should return `void`, `Future`, or `CompletableFuture`
- [ ] C) Self-invocation of `@Async` methods works correctly
- [ ] D) A custom `Executor` can be configured for async tasks
- [ ] E) Exceptions in void async methods are not propagated to the caller

<details>
<summary>Answer</summary>

**Correct: A, B, D, E**

Self-invocation does NOT work because it bypasses the proxy (same as `@Transactional`). Async methods must be called from outside the class. Exceptions in void async methods are only logged, not propagated.
</details>

---

### Q46. What is true about file upload handling in Spring Boot?
- [ ] A) `MultipartFile` interface represents uploaded files
- [ ] B) `spring.servlet.multipart.max-file-size` limits individual file size
- [ ] C) `@RequestParam` can receive multipart files
- [ ] D) `MultipartFile.transferTo()` saves file to filesystem
- [ ] E) Files are automatically validated for security threats

<details>
<summary>Answer</summary>

**Correct: A, B, C, D**

Files are NOT automatically validated for security threats. You must implement your own validation (file type checking, virus scanning, size limits, etc.).
</details>

---

### Q47. Which statements about Spring Data MongoDB are correct?
- [ ] A) `@Document` annotation maps a class to a MongoDB collection
- [ ] B) `MongoRepository` provides CRUD operations similar to `JpaRepository`
- [ ] C) `@Id` annotation marks the document identifier field
- [ ] D) MongoDB requires foreign key constraints like SQL databases
- [ ] E) Query methods can be derived from method names like in JPA

<details>
<summary>Answer</summary>

**Correct: A, B, C, E**

MongoDB is a NoSQL database and does NOT enforce foreign key constraints. Relationships are typically embedded or referenced manually. Spring Data MongoDB supports derived queries similar to JPA.
</details>

---

### Q48. Which are correct statements about API versioning strategies?
- [ ] A) URL path versioning: `/api/v1/users`
- [ ] B) Request header versioning: `Accept: application/vnd.api.v1+json`
- [ ] C) Query parameter versioning: `/api/users?version=1`
- [ ] D) All versioning strategies require code duplication
- [ ] E) Request parameter versioning can be handled with `@RequestMapping(params)`

<details>
<summary>Answer</summary>

**Correct: A, B, C, E**

Versioning does NOT require code duplication. You can often share code between versions using inheritance, composition, or conditional logic. All listed versioning strategies are valid approaches.
</details>

---

### Q49. What is true about Swagger/OpenAPI documentation in Spring Boot?
- [ ] A) `springdoc-openapi` library auto-generates API documentation
- [ ] B) `@Operation` annotation customizes endpoint documentation
- [ ] C) `@Schema` annotation customizes model documentation
- [ ] D) Swagger UI allows testing endpoints directly from the browser
- [ ] E) Documentation is generated at compile time only

<details>
<summary>Answer</summary>

**Correct: A, B, C, D**

Swagger documentation is generated at runtime, not compile time. The application must be running to access Swagger UI and see the generated documentation.
</details>

---

### Q50. Which statements about microservices communication are correct?
- [ ] A) REST is a synchronous communication pattern
- [ ] B) Message queues (Kafka, RabbitMQ) enable asynchronous communication
- [ ] C) API Gateway acts as a single entry point for multiple services
- [ ] D) Circuit breaker pattern prevents cascade failures
- [ ] E) Microservices should share a common database for data consistency

<details>
<summary>Answer</summary>

**Correct: A, B, C, D**

Microservices should NOT share databases - each service should own its data (Database per Service pattern). Shared databases create tight coupling and defeat the purpose of microservices architecture.
</details>

---

## Answer Key Summary

| Q# | Correct Answers | Q# | Correct Answers |
|----|-----------------|----|-----------------|
| 1  | A, B, D         | 26 | A, B, C, D      |
| 2  | A, B, E         | 27 | A, B, E         |
| 3  | A, C, D         | 28 | A, B, C, D      |
| 4  | A, B, C, D      | 29 | A, B, C, E      |
| 5  | A, B, D         | 30 | A, B, D, E      |
| 6  | A, B, C, D      | 31 | A, B, C, E      |
| 7  | A, B, D         | 32 | A, B, C, E      |
| 8  | A, C, D         | 33 | A, B, C         |
| 9  | A, B, C, D, E   | 34 | A, C, D, E      |
| 10 | A, B, C, E      | 35 | A, B, C, E      |
| 11 | A, B, C, E      | 36 | A, B, C, D      |
| 12 | A, C, D, E      | 37 | A, B, C, E      |
| 13 | A, B, C, E      | 38 | A, B, C, D      |
| 14 | A, B, D, E      | 39 | A, B, E         |
| 15 | A, B, C, E      | 40 | A, B, C, E      |
| 16 | A, B, C, D      | 41 | A, B, D, E      |
| 17 | A, B, C, D      | 42 | A, B, C, D, E   |
| 18 | A, B, C         | 43 | A, B, C, D, E   |
| 19 | A, B, D, E      | 44 | A, B, C, D      |
| 20 | A, B, D, E      | 45 | A, B, D, E      |
| 21 | A, B, C, E      | 46 | A, B, C, D      |
| 22 | A, B, C, E      | 47 | A, B, C, E      |
| 23 | A, B, D, E      | 48 | A, B, C, E      |
| 24 | A, C, D, E      | 49 | A, B, C, D      |
| 25 | A, B, C, E      | 50 | A, B, C, D      |

---

## Topic Distribution

| Topic | Question Numbers | Count |
|-------|-----------------|-------|
| Controllers & REST API | 1-7 | 7 |
| Service Layer & DI | 8-14 | 7 |
| Repository & JPA | 15-21 | 7 |
| DTO & Mapping | 22-27 | 6 |
| Caching | 28-32 | 5 |
| Security & JWT | 33-37 | 5 |
| Validation & Error Handling | 38-41 | 4 |
| Configuration | 42-44 | 3 |
| Advanced Topics | 45-50 | 6 |

---

*Document prepared for Spring Boot Introduction Course*
*Focus Areas: Controller, Service Layer, Repository, DTO, Mapping, Caching*
