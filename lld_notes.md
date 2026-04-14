# LLD 101 -- Complete Theory Guide (Exam 1: MCQs)

> **Exam:** 14th April 2026 | **Duration:** 60 minutes | **Format:** Objective MCQs
> **Scope:** OOP & Design, Design Principles (SOLID), Design Patterns

---

## Part 1: The Big Picture -- What is LLD and Why Do We Care?

**Low-Level Design (LLD)** is about writing code that is:
- **Easy to understand** -- another engineer can read it without a walkthrough.
- **Easy to extend** -- new features slot in without rewriting old code.
- **Easy to maintain** -- bug fixes stay local; they don't ripple everywhere.

Three pillars make this possible:
1. **OOP fundamentals** -- classes, interfaces, inheritance, polymorphism, abstraction, encapsulation.
2. **SOLID principles** -- five rules of thumb that keep your class design clean.
3. **Design patterns** -- battle-tested recipes for recurring design problems.

---

## Part 2: SOLID Principles 

SOLID is an acronym for 5 design principles. Think of them as **guardrails** -- if you follow them, your code naturally becomes easier to extend and test.

### S -- Single Responsibility Principle (SRP)

> **"A class should have only one reason to change."**

**The Intuition:** Imagine a chef who also does the billing and cleans the restaurant. If the tax rules change, the chef has to change. If the menu changes, the chef has to change. Too many reasons to change = too many responsibilities.

**In Code:**
- If your class has multiple `if/else` blocks handling unrelated behaviors, it probably violates SRP.
- A class doing serialization, file I/O, *and* business logic = SRP violation.
- **Fix:** Split responsibilities into separate classes. e.g., `Employee` holds data, `EmployeeRepository` handles persistence.

**Red Flags for SRP Violations:**
- Multiple unrelated if/else chains in one method
- "God" Util/Helper classes that do everything
- "Monster methods" -- a single method that generates a payslip, converts to JSON, emails it, *and* returns income

---

### O -- Open/Closed Principle (OCP)

> **"Software entities should be open for extension but closed for modification."**

**The Intuition:** Think of a power strip. You can plug in new devices (extend) without rewiring the strip (modifying). New functionality = new classes, not editing old ones.

**In Code:**
- If adding a new employee type means editing `TaxCalculator` with another `if/else` branch, that's OCP violation.
- **Fix:** Use abstraction. Make `TaxCalculator` abstract with a `calculate()` method. Each employee type gets its own subclass (`FTETaxCalculator`, `InternTaxCalculator`). Adding a new type = adding a new class, zero old code touched.

---

### L -- Liskov Substitution Principle (LSP)

> **"Objects should be replaceable with instances of their subtypes without altering correctness."**

**The Intuition:** If you have a function that works with a `Bird` and you pass in a `Kiwi`, the function should still work correctly. But if `Bird` has an abstract `fly()` method and Kiwi can't fly, you've broken LSP.

**In Code:**
- If a subclass throws `UnsupportedOperationException` for an inherited method, LSP is violated.
- If a subclass changes the *meaning* of a parent method (e.g., `withdraw()` on a `FixedDeposit` account silently does nothing), LSP is violated.
- **Fix:** Redesign the hierarchy. Kiwi shouldn't extend a class that promises flying. Use interfaces like `Flyable` for things that actually fly.

---

### I -- Interface Segregation Principle (ISP)

> **"No client should be forced to depend on methods it does not use."**

**The Intuition:** Imagine a restaurant menu that forces you to order appetizer, main course, and dessert every time. What if you just want coffee? A fat interface forces implementing classes to write methods they don't need.

**In Code:**
- An `Employee` interface with `getEmail()`, `processPayment()`, `getSalary()` is fine *if* all employees support all methods.
- But if `UnpaidIntern` can't process payment, the interface is too fat.
- **Fix:** Split into thin interfaces: `Identifiable` (getEmail), `Payable` (processPayment, getSalary). `UnpaidIntern` implements only `Identifiable`.

---

### D -- Dependency Inversion Principle (DIP)

> **"Depend upon abstractions, not concretions."**

**The Intuition:** Your laptop charger has a standard plug. You don't solder the wire directly into the wall. The plug (abstraction) lets you switch walls (implementations) without rewiring.

**In Code:**
```java
// BAD -- depends on concrete class
class PaymentProcessor {
    void pay(String productId) {
        SqlProductRepo repo = new SqlProductRepo(); // tightly coupled!
        Product p = repo.getProductById(productId);
    }
}

// GOOD -- depends on abstraction, injected via constructor
class PaymentProcessor {
    ProductRepo repo;
    PaymentProcessor(ProductRepo repo) { this.repo = repo; }
    void pay(String productId) {
        Product p = repo.getProductById(productId);
    }
}
```
- Switch from SQL to Mongo? Just pass `new MongoProductRepo()` in the constructor. `PaymentProcessor` never changes.

**Key mechanism:** Constructor injection -- pass dependencies through the constructor, typed as interfaces, not concrete classes.

---

## Part 3: Immutable Classes

### What is Immutability?
An object whose **state cannot change after construction**. Once created, it's frozen forever.

### Why Care?
- **Thread-safe** by default -- no locks needed since nothing changes.
- **Easy to reason about** -- no "who changed this?" debugging.
- **Safe as HashMap keys** -- their hashCode never changes.

### The 5 Rules to Make a Class Immutable

| Rule | Why |
|------|-----|
| 1. Make fields `private final` | Prevents external access and reassignment |
| 2. No setters | No way to change state |
| 3. Set values only via constructor | State is locked at birth |
| 4. Mark class `final` | Prevents subclasses from adding mutable state |
| 5. Defensive copy mutable fields | Prevents callers from mutating your internals |

### The Trap: Mutable Fields Inside an Immutable Class

```java
public final class Order {
    private final List<String> items;
    public Order(List<String> items) {
        this.items = items; // DANGEROUS! Caller can still mutate the list.
    }
    public List<String> getItems() { return items; } // DANGEROUS! Caller gets the real list.
}
```

**Fix -- Defensive Copying:**
```java
public Order(List<String> items) {
    this.items = List.copyOf(items);         // copy on the way IN
}
public List<String> getItems() {
    return Collections.unmodifiableList(items); // safe view on the way OUT
}
```

---

## Part 4: Design Patterns

Design patterns are **proven solutions to recurring design problems**. They fall into three families:

| Family | Purpose | Patterns in Your Syllabus |
|--------|---------|--------------------------|
| **Creational** | How objects are created | Singleton, Factory, Builder, Prototype |
| **Structural** | How objects are composed/connected | Adapter, Decorator, Flyweight, Proxy |
| **Behavioral** | How objects communicate/share work | Strategy |

---

### 4.1 Singleton Pattern (Creational)

> **Intent:** Ensure exactly **one instance** of a class exists and provide a **global access point**.

**Use Cases:** Logger, configuration manager, connection pool, metrics registry.

**Core Mechanism:** Private constructor + static instance + public accessor.

#### Evolution of Singleton (know each step and its problem):

| Approach | How | Problem |
|----------|-----|---------|
| **Eager Init** | `private static final Logger INSTANCE = new Logger();` | Created even if never used (wastes memory) |
| **Lazy Init** | Create inside `getInstance()` if null | **Not thread-safe** -- two threads can create two instances |
| **Synchronized** | `public static synchronized getInstance()` | Correct but **slow** -- every call pays synchronization cost |
| **Double-Checked Locking (no volatile)** | Check null, then synchronize, then check null again | **Broken!** Instruction reordering can expose half-constructed object |
| **DCL + volatile** | Add `volatile` to instance field | Correct since Java 5. Prevents reordering + ensures visibility |
| **Holder Idiom** (static inner class) | `private static class Holder { static final INSTANCE = new Logger(); }` | **Best balance**: lazy, thread-safe, no synchronization, clean |
| **Enum Singleton** | `public enum Logger { INSTANCE; }` | **Safest overall**: immune to reflection and serialization attacks. But no lazy init, can't subclass |

#### Why `volatile` Matters in DCL:
Object creation (`new Logger()`) is three steps internally:
1. Allocate memory
2. Initialize the object
3. Assign reference to variable

Without `volatile`, steps 2 and 3 can be **reordered**. Another thread might see a non-null reference to a **half-constructed** object. `volatile` prevents this reordering and ensures visibility across threads.

---

### 4.2 Factory Pattern (Creational)

Two variants to know:

#### Simple Factory (technique, not a GoF pattern)
- A **single static method** that returns the right concrete object based on a parameter.
- Centralizes `new` calls in one place, hides concrete classes from callers.
- **Con:** The factory method itself grows a big switch/if-else when types increase.

```java
final class StoneFactory {
    static Stone create(StoneType t) {
        return switch (t) {
            case SMALL  -> new SmallStone();
            case MEDIUM -> new MediumStone();
            case LARGE  -> new LargeStone();
        };
    }
}
```

#### Factory Method (GoF pattern)
- A **base class owns the algorithm** and calls an abstract `createX()` method.
- **Subclasses override** `createX()` to return different concrete products.
- The algorithm is fixed; only the creation step varies per subclass.

```java
abstract class StoneSpawner {
    protected abstract Stone createStone();  // factory method
    
    public final List<Stone> generateWave(int count) {  // fixed algorithm
        List<Stone> wave = new ArrayList<>();
        for (int i = 0; i < count; i++) wave.add(createStone());
        return wave;
    }
}

class RandomStoneSpawner extends StoneSpawner {
    protected Stone createStone() { /* random pick */ }
}
```

**When to use which:**
- **Simple Factory:** You just want one clean place to centralize `new` calls.
- **Factory Method:** A superclass algorithm must create objects at one step, and the concrete product varies per subclass.

---

### 4.3 Builder Pattern (Creational)

> **Intent:** Simplify creation of complex objects with many parameters (some optional) by separating construction from representation.

**The Problem -- Telescoping Constructors:**
```java
new Order("C101", 2, true, false, true, null, 5, "express");  // What is what?!
```

**The Fix -- Builder:**
```java
Order order = new Order.Builder()
    .customerId("C101")
    .priority(2)
    .giftWrap(true)
    .build();              // clear, readable, validates before creating
```

**Key Details:**
- Builder is a **static inner class** of the target.
- Target class constructor is **private** -- only Builder can call it.
- Builder methods return `this` for **fluent chaining**.
- **Validation** happens in `build()` -- fail fast if required fields are missing.
- Defaults are set in the Builder fields.
- Works best for **immutable objects** -- Builder + Immutability = a powerful combo.

---

### 4.4 Prototype Pattern (Creational)

> **Intent:** Create new objects by **cloning** a configured exemplar (prototype) instead of constructing from scratch.

**When to Use:**
- Construction is expensive (heavy setup, loading styles, etc.)
- You want to create objects "by example" without knowing concrete class names.
- The set of creatable products changes at runtime (plugins, feature flags).

**Key Components:**
- **Prototype interface** with a `copy()` / `clone()` method.
- **Concrete prototypes** implement deep-enough copying.
- **Registry** (Prototype Manager) -- maps keys to prototypes, returns clones.

```java
interface Shape {
    Shape copy();  // returns a deep-enough clone
}

class ShapeRegistry {
    Map<String, Shape> store = new HashMap<>();
    void register(String key, Shape proto) { store.put(key, proto); }
    Shape create(String key) { return store.get(key).copy(); }  // clone!
}
```

**Shallow vs Deep Copy:** Immutable fields can be shared; mutable fields MUST be copied to avoid shared-state bugs.

---

### 4.5 Adapter Pattern (Structural)

> **Intent:** Make two incompatible interfaces work together by translating one into the other.

**Analogy:** A travel plug adapter -- one side matches the wall socket (legacy API), the other matches your laptop plug (your interface). Neither changes; only the adapter speaks both.

**When to Use:**
- You need to integrate a third-party or legacy service whose API doesn't match yours.
- You **cannot modify** the legacy service (different team, third-party, etc.)

**Structure:**
- **Target interface** -- what your code expects (e.g., `SellerSearch`)
- **Adaptee** -- the legacy service with incompatible API (e.g., `SDSellerSearchService`)
- **Adapter** -- implements target, wraps adaptee, translates calls

```java
class SDSearchAdapter implements SellerSearch {
    private final SDSellerSearchService sd;
    
    SDSearchAdapter(SDSellerSearchService sd) { this.sd = sd; }
    
    public List<Seller> getSellersBySku(String sku) {
        // translate SDVendor -> Seller
        return sd.getSellersBySKU(sku).stream()
            .map(v -> new Seller(v.vendorId, v.shopName, v.listPrice, ...))
            .toList();
    }
}
```

**Java uses Object Adapter (composition)**, not Class Adapter (multiple inheritance -- not available in Java for classes).

**SOLID Alignment:** SRP (adapters adapt, ranking ranks), OCP (new provider = new adapter, no old code changes), DIP (high-level depends on `SellerSearch` abstraction, not concrete services).

---

### 4.6 Decorator Pattern (Structural)

> **Intent:** Dynamically add behaviors to an object **without modifying** it, by wrapping it in layers.

**Analogy:** Wrapping a parcel with optional services -- fragile sticker, insurance, tracking. Each wrapper adds functionality; the core package stays the same.

**The Problem It Solves -- Combinatorial Explosion:**
If you have N optional behaviors, subclassing gives 2^N combinations. Decorator gives N small wrapper classes that you stack at runtime.

**Structure:**
1. **Component interface** -- the base contract (e.g., `HttpClient`, `Notifier`)
2. **Concrete component** -- the core implementation (e.g., `BaseHttpClient`)
3. **Abstract decorator** -- implements component, wraps another component
4. **Concrete decorators** -- add specific behavior (retry, caching, auth, metrics...)

```java
// Base
interface Notifier { void notify(String text); }

// Core
class EmailNotifier implements Notifier { ... }

// Decorator base
abstract class NotifierDecorator implements Notifier {
    protected final Notifier inner;
    NotifierDecorator(Notifier inner) { this.inner = inner; }
}

// Concrete decorator
class WhatsAppDecorator extends NotifierDecorator {
    void notify(String text) {
        inner.notify(text);          // forward to wrapped
        sendWhatsApp(text);          // ADD behavior
    }
}

// Compose at runtime:
Notifier n = new SmsDecorator(new WhatsAppDecorator(new EmailNotifier()));
n.notify("Hello");  // sends Email, then WhatsApp, then SMS
```

**Decorator vs Proxy:** Both wrap objects, but:
- **Decorator** = adds new responsibilities/behavior
- **Proxy** = controls access (lazy loading, security, caching)

**Decorator vs Strategy:** 
- **Decorator** = optional, stackable, combinable layers around the same interface
- **Strategy** = swap one entire algorithm for another

---

### 4.7 Proxy Pattern (Structural)

> **Intent:** Provide a **surrogate** or placeholder for another object to **control access** to it.

**Types of Proxy:**

| Type | Purpose | Example |
|------|---------|---------|
| **Virtual Proxy** | Lazy loading -- defer expensive creation until needed | Image loader, BookParser |
| **Protection Proxy** | Access control -- check permissions before forwarding | Role-based document access |
| **Remote Proxy** | Represent an object in another address space | RPC stubs |
| **Smart Reference** | Housekeeping -- ref counting, locking | |

**Key Example -- Virtual Proxy (Lazy Loading):**
```java
class LazyBookParserProxy implements ITextParser {
    private final String book;
    private BookParser realParser;  // created only on demand
    
    public int getWordCount() {
        if (realParser == null) realParser = new BookParser(book); // LAZY!
        return realParser.getWordCount();
    }
}
```

**Why not just put the null-check in the client?**
- Violates **SRP** -- client now manages parser lifecycle
- Violates **OCP** -- every new method repeats the null-check
- Violates **DIP** -- client is coupled to `BookParser` constructor
- Proxy keeps client clean; it just uses `ITextParser` normally

---

### 4.8 Flyweight Pattern (Structural)

> **Intent:** Share heavy, identical state across many objects to **save memory**.

**The Core Idea -- Split state into two:**
- **Intrinsic state** (shared) -- immutable, identical across instances (textures, base stats, mesh data). Stored in the flyweight object.
- **Extrinsic state** (per-instance) -- unique, changes per instance (position, velocity, HP). Stored outside the flyweight.

**Use When:** Large number of objects from a small catalog of types (e.g., 200k bullets but only 12 bullet *types*).

```java
// Intrinsic (shared flyweight)
public final class BulletType {
    public final TextureId texture;
    public final double baseDamage;
    public final double baseSpeed;
}

// Extrinsic (per-instance)
public final class Bullet {
    public BulletType type;  // reference to shared flyweight
    public float x, y;       // unique per bullet
    public float vx, vy;
}
```

**Flyweight Factory** caches flyweights by key, returns existing one or creates new:
```java
class BulletTypeFactory {
    Map<String, BulletType> cache = new HashMap<>();
    BulletType get(...) { return cache.computeIfAbsent(key, k -> new BulletType(...)); }
}
```

**Flyweight vs Object Pool:** Pool reduces allocation churn; flyweight reduces per-instance memory size. They complement each other.

**Critical Rule:** Flyweights MUST be immutable. If you mutate shared state, ALL instances change.

---

### 4.9 Strategy Pattern (Behavioral)

> **Intent:** Encapsulate a family of algorithms, make them **interchangeable**, and let the host object delegate to one strategy at a time.

**The Problem It Solves:**
- Different clients need different algorithms for the same step (e.g., different sorting strategies).
- Putting all variants in one method via if/else violates SRP and OCP.
- Creating subclasses per combination causes **combinatorial explosion**.

**Structure:**
1. **Strategy interface** -- defines the algorithm contract
2. **Concrete strategies** -- each implements one algorithm
3. **Context** -- holds a reference to the strategy, delegates work to it

```java
interface SortStrategy {
    void sort(List<Integer> data);
}

class CountSort01 implements SortStrategy { /* for binary data */ }
class InsertionSort implements SortStrategy { /* for nearly-sorted data */ }
class QuickSort implements SortStrategy { /* for random data */ }

class MyIntList {
    private SortStrategy sorter;
    
    MyIntList(SortStrategy initial) { this.sorter = initial; }
    void setSorter(SortStrategy s) { this.sorter = s; }  // hot-swap at runtime!
    
    void sort() { sorter.sort(this.data); }
}
```

**Key Benefit:** Strategies can be **swapped at runtime** without changing the context class.

**Strategy vs Decorator:**
- **Strategy** = replace ONE algorithm entirely (sorting, fee calculation, pathfinding)
- **Decorator** = stack MULTIPLE optional behaviors around the same core

---

## Part 5: Pattern Quick-Reference Cheat Sheet

| Pattern | Type | One-Line Summary | Key Mechanism |
|---------|------|-----------------|---------------|
| **Singleton** | Creational | Exactly one instance, global access | Private constructor + static instance |
| **Simple Factory** | Creational | Centralize `new` in one static method | Switch/map returning concrete objects |
| **Factory Method** | Creational | Superclass algorithm, subclass decides which object | Abstract `createX()` overridden by subclasses |
| **Builder** | Creational | Step-by-step construction of complex objects | Fluent inner class with `build()` |
| **Prototype** | Creational | Clone a configured exemplar | `copy()`/`clone()` + Registry |
| **Adapter** | Structural | Make incompatible interfaces work together | Wrapper translates API calls |
| **Decorator** | Structural | Add behaviors dynamically by wrapping | Layers implementing same interface |
| **Proxy** | Structural | Control access to another object | Same interface, intercepts before forwarding |
| **Flyweight** | Structural | Share heavy identical state across many objects | Intrinsic (shared) vs extrinsic (per-instance) |
| **Strategy** | Behavioral | Swap algorithms at runtime | Interface + concrete implementations, injected into context |

---

## Part 6: How to Identify a Pattern from Code (Exam Skill)

This is the **#1 skill** for the MCQ exam. Here's how to spot each pattern:

| You See... | It's Probably... |
|------------|-----------------|
| `private constructor` + `static getInstance()` | **Singleton** |
| Static method with switch/if returning different `new` objects | **Simple Factory** |
| Abstract class with abstract `createX()` + fixed algorithm calling it | **Factory Method** |
| Inner `Builder` class with fluent setters + `build()` | **Builder** |
| `copy()` / `clone()` method + registry | **Prototype** |
| Class implements target interface, wraps a different legacy object, translates | **Adapter** |
| Class wraps another of the **same** interface, adds behavior before/after forwarding | **Decorator** |
| Class wraps another of the **same** interface, controls access / lazy loads | **Proxy** |
| Shared immutable type-object + lightweight per-instance object referencing it | **Flyweight** |
| Interface for algorithm + context holds reference + can swap at runtime | **Strategy** |

---

## Part 7: How to Identify SOLID Violations from Code (Exam Skill)

| You See... | Principle Violated |
|------------|-------------------|
| Class does business logic AND persistence AND formatting | **SRP** |
| Adding new type requires editing existing switch/if | **OCP** |
| Subclass throws `UnsupportedOperationException` | **LSP** |
| Class forced to implement empty/useless methods from interface | **ISP** |
| Class creates `new ConcreteX()` internally instead of accepting interface via constructor | **DIP** |

---

## Part 8: Frequently Confused Pairs

### Decorator vs Proxy
- Both wrap an object behind the same interface.
- **Decorator** adds new behavior (logging, retries, metrics).
- **Proxy** controls access (lazy loading, permission checks, caching).

### Decorator vs Strategy
- **Decorator** = N optional stackable layers (each a small class).
- **Strategy** = pick ONE algorithm from a family, inject it into context.

### Factory Method vs Simple Factory
- **Simple Factory** = one static method, one class, returns product based on param.
- **Factory Method** = abstract method in superclass, subclasses override to return different products. Algorithm is inherited; creation is deferred.

### Adapter vs Decorator
- **Adapter** changes the *interface* (translates API A to API B).
- **Decorator** keeps the *same interface* but adds behavior.

### Prototype vs Factory
- **Prototype** = create by cloning an existing configured object.
- **Factory** = create by calling `new` in a centralized place.
- Use Prototype when construction is expensive or types change at runtime.

---

*Good luck on the exam -- understand the WHY behind each pattern, not just the code.*
