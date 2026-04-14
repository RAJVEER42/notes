# LLD 101 -- MCQ Practice Questions (Exam 1 Prep)

> Questions are modeled after the **2027 batch PYQ style**: code-based pattern identification, SOLID principle identification, and conceptual scenario questions. Each question includes a detailed explanation.

---

## Section A: Identify the Design Pattern from Code

---

### Q1.

```java
interface Coffee {
    double getCost();
    String getIngredients();
}

class SimpleCoffee implements Coffee {
    public double getCost() { return 1; }
    public String getIngredients() { return "Coffee"; }
}

class WithMilk implements Coffee {
    private Coffee coffee;
    public WithMilk(Coffee c) { this.coffee = c; }
    public double getCost() { return coffee.getCost() + 0.5; }
    public String getIngredients() { return coffee.getIngredients() + ", Milk"; }
}

class WithSugar implements Coffee {
    private Coffee coffee;
    public WithSugar(Coffee c) { this.coffee = c; }
    public double getCost() { return coffee.getCost() + 0.3; }
    public String getIngredients() { return coffee.getIngredients() + ", Sugar"; }
}
```

**What is a key feature of the design pattern illustrated above?**

- A) It primarily focuses on reducing the number of objects needed for application functionality.
- B) It allows for the dynamic addition of attributes to objects without modifying their internal structure.
- C) It is mainly used to create a single interface for multiple subclasses.
- D) It ensures that classes are closed for modification but open for extension.

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

This is the **Decorator Pattern**. The tell-tale signs:
- `WithMilk` and `WithSugar` **implement the same interface** (`Coffee`) as the object they wrap.
- They hold a reference to another `Coffee` and **add behavior** (extra cost, extra ingredient) before/after delegating.
- You can stack them: `new WithSugar(new WithMilk(new SimpleCoffee()))` -- dynamic composition at runtime.

Option D describes OCP (a principle, not a pattern feature). The *key feature* of Decorator is dynamically adding attributes/behaviors without modifying the original class.

> **PYQ Match:** This is directly from the 2027 PYQ (Q3, S1).
</details>

---

### Q2.

```java
interface Message {
    void send(String content);
}

class TextMessage implements Message {
    public void send(String content) {
        System.out.println("Sending Text Message: " + content);
    }
}

class ImageMessage implements Message {
    public void send(String content) {
        System.out.println("Sending Image Message: " + content);
    }
}

class MessageHelper {
    public static Message getMessage(String type) {
        if ("text".equalsIgnoreCase(type)) {
            return new TextMessage();
        } else if ("image".equalsIgnoreCase(type)) {
            return new ImageMessage();
        }
        return null;
    }
}
```

**Which design pattern does this code demonstrate?**

- A) Factory Method Pattern
- B) Adapter Pattern
- C) Builder Pattern
- D) Prototype Pattern

<details>
<summary>Answer & Explanation</summary>

**Answer: A**

This is a **Simple Factory** (often referred to as Factory Method Pattern in exams). The key sign:
- `MessageHelper.getMessage()` is a **static method** that takes a parameter and uses if/else to decide **which concrete class to instantiate**.
- Client code calls `MessageHelper.getMessage("text")` and gets back a `Message` without knowing the concrete class.

Why not the others?
- **Adapter:** Would wrap one interface to make it look like another. No translation here.
- **Builder:** Would have fluent setter methods and a `build()` call.
- **Prototype:** Would clone an existing object rather than calling `new`.

> **PYQ Match:** This is directly from the 2027 PYQ (Q4, S1).
</details>

---

### Q3.

In a cab booking application, there's a need to **dynamically add features to the ride without modifying the existing codebase**. For instance, adding options like a baby seat, additional luggage, or a premium ride experience. Which design pattern is most suitable?

- A) Strategy Pattern
- B) Decorator Pattern
- C) Flyweight Pattern
- D) Adapter Pattern

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

**Decorator Pattern** is the right choice because:
- The features are **optional and combinable** -- a ride could have baby seat + luggage, or just premium, or all three.
- We want to **add features dynamically** without modifying existing ride classes.
- Each feature wraps the base ride and adds its own behavior/cost.

Why not the others?
- **Strategy:** Would replace the *entire* ride algorithm, not add optional features on top.
- **Flyweight:** Is for sharing heavy state across many similar objects, not adding features.
- **Adapter:** Is for making incompatible interfaces work together.

> **PYQ Match:** This is directly from the 2027 PYQ (Q5, S1).
</details>

---

### Q4.

**Which of the following code snippets best demonstrates the Dependency Inversion Principle?**

**Option 1:**
```java
class LightBulb {
    void turnOn() { /*...*/ }
    void turnOff() { /*...*/ }
}
class LightSwitch {
    private LightBulb lightBulb = new LightBulb();
    void operate() { /* ... */ }
}
```

**Option 2:**
```java
interface Switchable {
    void turnOn();
    void turnOff();
}
class LightBulb implements Switchable {
    public void turnOn() { /*...*/ }
    public void turnOff() { /*...*/ }
}
class LightSwitch {
    private Switchable device;
    public LightSwitch(Switchable device) {
        this.device = device;
    }
    void operate() { /* ... */ }
}
```

**Option 3:**
```java
class LightBulb {
    void turnOn() { /*...*/ }
    void turnOff() { /*...*/ }
}
class Fan {
    void start() { /*...*/ }
    void stop() { /*...*/ }
}
class LightSwitch {
    private LightBulb lightBulb = new LightBulb();
    private Fan fan = new Fan();
    void operateLight() { /* ... */ }
    void operateFan() { /* ... */ }
}
```

**Option 4:**
```java
class LightBulb {
    void turnOn() { /*...*/ }
    void turnOff() { /*...*/ }
}
class LightSwitch {
    void operate(LightBulb lightBulb) { /* ... */ }
}
```

Select one: **1, 2, 3, or 4?**

<details>
<summary>Answer & Explanation</summary>

**Answer: 2**

Option 2 follows DIP because:
1. **Depends on abstraction:** `LightSwitch` depends on the `Switchable` **interface**, not on the concrete `LightBulb` class.
2. **Constructor injection:** The dependency is injected via the constructor, not created internally with `new`.
3. **Easily extensible:** You can pass a `Fan implements Switchable` or any new device without changing `LightSwitch`.

Why others fail:
- **Option 1:** `LightSwitch` creates `new LightBulb()` internally -- tightly coupled to a concrete class. Classic DIP violation.
- **Option 3:** Even worse -- hardcodes TWO concrete classes. Maximum coupling.
- **Option 4:** Accepts `LightBulb` as a parameter, which is better than creating it internally, but still depends on the **concrete class**, not an abstraction.

> **PYQ Match:** This is directly from the 2027 PYQ (Q6, S1).
</details>

---

### Q5.

```java
public class DatabaseConnection {
    private static volatile DatabaseConnection instance;
    private DatabaseConnection() { }
    
    public static DatabaseConnection getInstance() {
        if (instance == null) {
            synchronized (DatabaseConnection.class) {
                if (instance == null) {
                    instance = new DatabaseConnection();
                }
            }
        }
        return instance;
    }
}
```

**Which design pattern does this code implement?**

- A) Factory Method
- B) Prototype
- C) Singleton
- D) Builder

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

This is the **Singleton Pattern** using **Double-Checked Locking (DCL) with volatile**. The signs:
- **Private constructor** -- prevents external instantiation.
- **Static instance** field -- the single instance.
- **`getInstance()`** -- global access point.
- **Double null-check** with **synchronized block** -- thread-safe lazy initialization.
- **`volatile`** keyword -- prevents instruction reordering, ensuring other threads never see a half-constructed object.

</details>

---

### Q6.

```java
interface PaymentGateway {
    void processPayment(double amount);
}

class StripePayment {
    void makePayment(double amt, String currency) {
        System.out.println("Stripe: " + amt + " " + currency);
    }
}

class StripeAdapter implements PaymentGateway {
    private StripePayment stripe;
    StripeAdapter(StripePayment stripe) { this.stripe = stripe; }
    
    public void processPayment(double amount) {
        stripe.makePayment(amount, "USD");
    }
}
```

**Which design pattern does this code demonstrate?**

- A) Strategy Pattern
- B) Decorator Pattern
- C) Adapter Pattern
- D) Proxy Pattern

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

This is the **Adapter Pattern**. The signs:
- `StripePayment` has a **different interface** (`makePayment(amt, currency)`) than what the client expects (`processPayment(amount)`).
- `StripeAdapter` **implements the target interface** (`PaymentGateway`) and **wraps the adaptee** (`StripePayment`).
- It **translates** the call -- converting `processPayment(double)` into `makePayment(double, String)`.

**Key Distinction from Decorator/Proxy:** The adapter changes the *interface shape* (different method signatures). Decorator and Proxy keep the *same interface*.

</details>

---

### Q7.

```java
interface Shape {
    Shape clone();
    void draw();
}

class Circle implements Shape {
    private int radius;
    Circle(int r) { this.radius = r; }
    public Shape clone() { return new Circle(this.radius); }
    public void draw() { System.out.println("Circle r=" + radius); }
}

class ShapeCache {
    private static Map<String, Shape> cache = new HashMap<>();
    
    static void loadCache() {
        cache.put("circle", new Circle(10));
    }
    static Shape getShape(String key) {
        return cache.get(key).clone();
    }
}
```

**Which design pattern does this code demonstrate?**

- A) Singleton
- B) Prototype
- C) Factory Method
- D) Flyweight

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

This is the **Prototype Pattern with a Registry**. The signs:
- Each shape has a `clone()` method that creates a **copy of itself**.
- `ShapeCache` acts as a **registry** -- stores pre-configured prototypes.
- `getShape()` returns a **clone** of the stored prototype, not the original.
- New objects are created by copying, not by calling `new` with complex parameters.

Why not Flyweight? Flyweight *shares* objects (many instances point to the same type). Prototype *clones* objects (each call returns a fresh, independent copy).

</details>

---

### Q8.

```java
interface Image {
    void display();
}

class RealImage implements Image {
    private String filename;
    RealImage(String filename) {
        this.filename = filename;
        System.out.println("Loading " + filename + " from disk...");
    }
    public void display() { System.out.println("Displaying " + filename); }
}

class ProxyImage implements Image {
    private String filename;
    private RealImage realImage;
    
    ProxyImage(String filename) { this.filename = filename; }
    
    public void display() {
        if (realImage == null) {
            realImage = new RealImage(filename);
        }
        realImage.display();
    }
}
```

**What is the PRIMARY purpose of the `ProxyImage` class?**

- A) To add extra visual effects when displaying images
- B) To defer the expensive image loading until the image is actually displayed
- C) To adapt the `RealImage` to a different interface
- D) To share one image object across multiple displays

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

This is a **Virtual Proxy** for **lazy loading**. The key behavior:
- `ProxyImage` does NOT load the image in its constructor. The `RealImage` (which does the heavy disk loading) is created **only when `display()` is called for the first time**.
- Subsequent calls reuse the already-loaded `realImage`.

Why not the others?
- **A (Decorator):** A decorator would add new behavior. The proxy doesn't add display effects -- it just controls *when* the real object is created.
- **C (Adapter):** Both `ProxyImage` and `RealImage` implement the **same** `Image` interface. No interface translation.
- **D (Flyweight):** Flyweight shares a single object across many. Here, each proxy owns its own `RealImage`.

</details>

---

### Q9.

```java
class BulletType {
    final String texture;
    final int damage;
    final double speed;
    BulletType(String texture, int damage, double speed) {
        this.texture = texture; this.damage = damage; this.speed = speed;
    }
}

class BulletTypeFactory {
    private Map<String, BulletType> cache = new HashMap<>();
    BulletType get(String texture, int damage, double speed) {
        String key = texture + "|" + damage + "|" + speed;
        return cache.computeIfAbsent(key, k -> new BulletType(texture, damage, speed));
    }
}

class Bullet {
    BulletType type;  // shared
    float x, y;       // unique per bullet
    float vx, vy;
}
```

**Which design pattern is being used here?**

- A) Prototype
- B) Singleton
- C) Flyweight
- D) Strategy

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

This is the **Flyweight Pattern**. The signs:
- **Intrinsic state** (shared, immutable): `BulletType` holds texture, damage, speed -- same across thousands of bullets of the same type.
- **Extrinsic state** (per-instance): `Bullet` holds position and velocity -- unique per bullet.
- **Factory with cache:** `BulletTypeFactory` ensures that the same `BulletType` is **shared** (not duplicated) across all bullets of the same type.
- Memory savings: 200,000 bullets share maybe 12 `BulletType` objects instead of duplicating the data 200,000 times.

</details>

---

### Q10.

```java
interface SortStrategy {
    void sort(List<Integer> data);
}

class BubbleSort implements SortStrategy {
    public void sort(List<Integer> data) { /* bubble sort */ }
}

class MergeSort implements SortStrategy {
    public void sort(List<Integer> data) { /* merge sort */ }
}

class DataProcessor {
    private SortStrategy strategy;
    DataProcessor(SortStrategy strategy) { this.strategy = strategy; }
    
    void setStrategy(SortStrategy strategy) { this.strategy = strategy; }
    
    void process(List<Integer> data) {
        strategy.sort(data);
        // ... rest of processing
    }
}
```

**Which design pattern is demonstrated?**

- A) Decorator
- B) Adapter
- C) Strategy
- D) Factory Method

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

This is the **Strategy Pattern**. The signs:
- A **strategy interface** (`SortStrategy`) defines the algorithm contract.
- Multiple **concrete strategies** (`BubbleSort`, `MergeSort`) each implement a different algorithm.
- A **context class** (`DataProcessor`) holds a reference to the strategy and **delegates** the sorting work.
- The strategy can be **swapped at runtime** via `setStrategy()`.

Why not Decorator? Decorator would wrap another `SortStrategy` and add behavior around it. Here, each strategy *replaces* the previous one entirely.

</details>

---

## Section B: SOLID Principles

---

### Q11.

```java
class Employee {
    String name;
    double salary;
    
    void calculateTax() { /* tax logic */ }
    void saveToDatabase() { /* persistence logic */ }
    void generatePayslip() { /* formatting + PDF generation */ }
    void sendPayslipEmail() { /* email sending logic */ }
}
```

**Which SOLID principle does this class violate?**

- A) Open/Closed Principle
- B) Liskov Substitution Principle
- C) Single Responsibility Principle
- D) Interface Segregation Principle

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

This class has **four unrelated responsibilities**: tax calculation, database persistence, PDF generation, and email sending. That's four different reasons to change. SRP says a class should have **only one reason to change**.

Fix: Split into `TaxCalculator`, `EmployeeRepository`, `PayslipGenerator`, `EmailService`.

</details>

---

### Q12.

```java
class NotificationService {
    void send(String message, String type) {
        if (type.equals("email")) {
            // send email
        } else if (type.equals("sms")) {
            // send sms
        } else if (type.equals("push")) {
            // send push notification
        }
    }
}
```

**Adding support for WhatsApp notifications requires modifying the `send()` method. Which principle does this violate?**

- A) Single Responsibility Principle
- B) Open/Closed Principle
- C) Liskov Substitution Principle
- D) Dependency Inversion Principle

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

The **Open/Closed Principle** says software should be **open for extension, closed for modification**. Adding a new notification type here requires **editing existing code** (adding another `else if`), which risks breaking the existing channels.

Fix: Define a `NotificationSender` interface. Create `EmailSender`, `SmsSender`, `PushSender`, `WhatsAppSender` classes. Adding WhatsApp = adding a new class, zero old code modified.

</details>

---

### Q13.

```java
interface Bird {
    void fly();
    void eat();
}

class Eagle implements Bird {
    public void fly() { System.out.println("Soaring high!"); }
    public void eat() { System.out.println("Eating fish"); }
}

class Penguin implements Bird {
    public void fly() {
        throw new UnsupportedOperationException("Penguins can't fly!");
    }
    public void eat() { System.out.println("Eating krill"); }
}
```

**Which SOLID principle(s) does this design violate?**

- A) Only SRP
- B) LSP and ISP
- C) Only OCP
- D) Only DIP

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Two principles are violated:

1. **LSP (Liskov Substitution):** Any code expecting a `Bird` and calling `fly()` will crash if passed a `Penguin`. The subtype cannot be substituted without altering correctness.

2. **ISP (Interface Segregation):** The `Bird` interface forces `Penguin` to implement `fly()` which it doesn't support. The interface is too fat.

Fix: Split into `Flyable` and `Eatable` interfaces. `Eagle` implements both; `Penguin` implements only `Eatable`.

</details>

---

### Q14.

```java
class OrderService {
    private MySQLDatabase db = new MySQLDatabase();
    
    void placeOrder(Order order) {
        db.save(order);
    }
}
```

**Which principle is violated, and what is the correct fix?**

- A) SRP -- split OrderService into smaller classes
- B) OCP -- make OrderService abstract
- C) DIP -- depend on a Database interface and inject it via constructor
- D) ISP -- split the Database interface into smaller ones

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

The **Dependency Inversion Principle** is violated because:
- `OrderService` (high-level module) directly depends on `MySQLDatabase` (low-level concrete class).
- It creates the dependency internally with `new` -- maximum coupling.

Fix:
```java
class OrderService {
    private Database db;
    OrderService(Database db) { this.db = db; }  // inject abstraction
    void placeOrder(Order order) { db.save(order); }
}
```
Now switching to PostgreSQL or MongoDB = passing a different implementation, zero changes to `OrderService`.

</details>

---

## Section C: Conceptual & Scenario Questions

---

### Q15.

**Which Singleton implementation is immune to BOTH reflection attacks AND serialization attacks?**

- A) Double-Checked Locking with volatile
- B) Static Inner Class (Holder Idiom)
- C) Eager Initialization
- D) Enum Singleton

<details>
<summary>Answer & Explanation</summary>

**Answer: D**

The **Enum Singleton** is the only approach immune to both:
- **Reflection:** Java's reflection API explicitly prevents `newInstance()` on enum constructors.
- **Serialization:** The JVM guarantees that enum deserialization returns the existing constant, not a new object.

All other approaches (DCL, Holder, Eager) can be broken by reflection (accessing the private constructor) and serialization (deserializing creates a new object unless `readResolve()` is implemented).

</details>

---

### Q16.

**In a lazy-initialized Singleton using Double-Checked Locking, what happens if the `volatile` keyword is omitted from the instance field?**

- A) The Singleton will throw a NullPointerException
- B) Another thread might see a reference to a half-constructed object
- C) The synchronized block will have no effect
- D) The Singleton will always create two instances

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Without `volatile`, the JVM is free to **reorder** the instructions of `instance = new Logger()`:
1. Allocate memory
2. Assign reference to `instance` (instance is now non-null!)
3. Initialize the object (constructor body runs)

If step 2 happens before step 3, another thread doing the first null-check sees a non-null `instance` and returns it -- but the object isn't fully constructed yet. This leads to accessing a **half-initialized object**, not necessarily a NullPointerException.

`volatile` creates a **happens-before** relationship that prevents this reordering.

</details>

---

### Q17.

**What is the key difference between the Decorator pattern and the Proxy pattern?**

- A) Decorator uses interfaces; Proxy uses abstract classes
- B) Decorator adds new responsibilities; Proxy controls access to the real object
- C) Decorator is a creational pattern; Proxy is a structural pattern
- D) Decorator creates new objects; Proxy reuses existing objects

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Both Decorator and Proxy:
- Are structural patterns
- Wrap an object behind the same interface
- Forward calls to the wrapped object

The **intent** is different:
- **Decorator:** Adds or enhances behavior (retries, logging, compression, caching layers)
- **Proxy:** Controls access to the real object (lazy loading, permission checks, remote access)

</details>

---

### Q18.

**An immutable class contains a `List<String>` field. Which of the following is REQUIRED to preserve immutability?**

- A) Declare the field as `public final`
- B) Only provide a getter, no setter
- C) Make a defensive copy in the constructor AND return an unmodifiable view in the getter
- D) Use `volatile` on the field

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

`final` only prevents reassignment of the *reference*, not mutation of the *contents*. A caller could still do:
```java
List<String> myList = new ArrayList<>();
Order order = new Order(myList);
myList.add("hacked!");  // mutates the "immutable" object!
```

The fix requires **two things:**
1. **Defensive copy in constructor:** `this.items = List.copyOf(items);` -- breaks the link to the caller's list.
2. **Unmodifiable view in getter:** `return Collections.unmodifiableList(items);` -- prevents mutation through the getter.

Option B (no setter) is necessary but NOT sufficient -- the list can still be mutated via the getter if you return the original reference.

</details>

---

### Q19.

**You're building a document editor where users can select Rectangle, Circle, or Arrow tools from a palette and place shapes on a canvas. Constructing each shape involves loading styles, constraints, and handles. Users place hundreds of the same shape. Which pattern should the palette use to produce new shapes?**

- A) Singleton
- B) Factory Method
- C) Prototype
- D) Flyweight

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

**Prototype Pattern** is ideal because:
- Construction is expensive (loading styles, constraints, handles).
- Users place many instances of the same *kind* of shape.
- The palette keeps one configured exemplar per shape type and **clones** it on each click.
- New shape types (plugins) can be registered at runtime.

Why not the others?
- **Singleton:** We need *many* instances, not one.
- **Factory Method:** Works when creation policy varies per subclass. Here, we're cloning pre-configured objects.
- **Flyweight:** Would share the style data across shapes, but doesn't address the expensive construction problem. Could be used *alongside* Prototype for optimization.

</details>

---

### Q20.

**A video streaming platform needs to dynamically adjust quality settings (Low, Medium, High) based on network conditions. The streaming manager should be able to switch the quality adjustment algorithm at runtime. Which pattern is most appropriate?**

- A) Decorator
- B) Adapter
- C) Strategy
- D) Proxy

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

**Strategy Pattern** is perfect because:
- There's a family of algorithms (Low/Medium/High quality adjustment).
- The algorithms are **interchangeable** -- the system picks one based on network conditions.
- The algorithm must be **swappable at runtime** (network changes dynamically).
- The `VideoStreamingManager` (context) delegates quality adjustment to the current strategy.

This matches the 2027 PYQ coding question (Q2, S2) which required implementing exactly this.

</details>

---

### Q21.

**Which of the following is NOT a valid use case for the Builder pattern?**

- A) Creating an immutable `GameConfig` object with 10 optional parameters
- B) Ensuring only one instance of a `Logger` exists in the application
- C) Constructing an `Order` object where `customerId` is required but `giftWrap` and `priority` are optional
- D) Building a complex `HttpRequest` object step by step with fluent method calls

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Ensuring only one instance exists is the **Singleton** pattern, not Builder.

Builder is used when:
- Objects have **many parameters** (some optional, some required)
- Construction has **multiple steps**
- You want **readable, fluent** object creation
- You want to **validate** before creating the object

Options A, C, and D are all classic Builder use cases.

</details>

---

### Q22.

```java
interface Sorter {
    void sort(int[] arr);
}

class QuickSorter implements Sorter {
    public void sort(int[] arr) { /* quicksort */ }
}

class LoggingSorter implements Sorter {
    private Sorter inner;
    LoggingSorter(Sorter inner) { this.inner = inner; }
    public void sort(int[] arr) {
        System.out.println("Sorting started, size=" + arr.length);
        inner.sort(arr);
        System.out.println("Sorting finished");
    }
}

class TimingSorter implements Sorter {
    private Sorter inner;
    TimingSorter(Sorter inner) { this.inner = inner; }
    public void sort(int[] arr) {
        long t0 = System.nanoTime();
        inner.sort(arr);
        System.out.println("Took " + (System.nanoTime() - t0) + " ns");
    }
}

// Usage:
Sorter s = new TimingSorter(new LoggingSorter(new QuickSorter()));
s.sort(data);
```

**Which pattern is used here?**

- A) Strategy
- B) Chain of Responsibility
- C) Decorator
- D) Proxy

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

This is the **Decorator Pattern**. The signs:
- `LoggingSorter` and `TimingSorter` **implement the same interface** as the object they wrap (`Sorter`).
- They wrap another `Sorter` and **add behavior before/after** the delegated call.
- They are **stacked**: `TimingSorter` wraps `LoggingSorter` wraps `QuickSorter`.
- Each layer adds one concern (logging, timing) without modifying the core sorting algorithm.

Why not Strategy? Strategy would *replace* the sorting algorithm (QuickSort vs MergeSort). Here, logging and timing are **additional layers around** the algorithm, not replacement algorithms.

</details>

---

### Q23.

**Consider a map application that renders 100,000 markers on screen. All "restaurant" markers share the same icon image and color scheme, differing only in latitude/longitude. Which pattern should be used to avoid storing 100,000 copies of the same icon?**

- A) Prototype
- B) Flyweight
- C) Singleton
- D) Builder

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

**Flyweight Pattern** because:
- **Intrinsic state** (shared): icon image, color scheme -- same for all restaurant markers.
- **Extrinsic state** (per-instance): latitude, longitude -- unique per marker.
- A `MarkerStyleFactory` caches one `MarkerStyle` per type (restaurant, hospital, etc.) and returns the shared instance.
- 100,000 markers share maybe 10 `MarkerStyle` objects instead of duplicating icon data 100,000 times.

This directly matches your **flyweight-markers** assignment in the course!

</details>

---

### Q24.

**Which of the following correctly describes the relationship between the Builder and Immutable Classes concepts?**

- A) Builder creates mutable objects; immutability is achieved separately
- B) Builder validates and constructs immutable objects step by step, solving the telescoping constructor problem
- C) Builder and immutability are unrelated patterns that should not be used together
- D) Builder replaces the need for immutability by providing controlled mutation

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Builder and Immutable Classes are a **powerful combination**:
- Immutable classes need ALL fields set at construction time (no setters).
- With many fields, the constructor becomes unreadable (telescoping constructors).
- Builder lets you set fields **step by step** with fluent methods, then **`build()` validates and creates** the immutable object.
- The target class constructor is `private` -- only Builder can call it.

This is exactly the pattern used in the `IncidentTicket` assignment with the `IncidentTicket.Builder`.

</details>

---

### Q25.

**A music streaming app supports multiple audio output formats: Bluetooth, Wired, AirPlay. Each requires a different audio encoding algorithm. The user can switch output mid-playback. The playback engine should not know the details of each format. Which pattern fits?**

- A) Adapter
- B) Decorator
- C) Proxy
- D) Strategy

<details>
<summary>Answer & Explanation</summary>

**Answer: D**

**Strategy Pattern** because:
- There's a family of interchangeable algorithms (Bluetooth encoding, Wired encoding, AirPlay encoding).
- The playback engine (context) delegates to one strategy at a time.
- The strategy can be **swapped at runtime** (user switches from Bluetooth to AirPlay mid-song).
- The playback engine depends on the `AudioOutputStrategy` interface, not on any concrete format.

Why not Adapter? Adapter is for making an *existing incompatible interface* work with your code. Here, you're designing from scratch with interchangeable algorithms.

</details>

---

## Section D: Tricky Conceptual Questions

---

### Q26.

**Which of the following statements is TRUE about the Static Inner Class (Holder Idiom) approach to Singleton?**

- A) It requires the `volatile` keyword for thread safety
- B) It uses the JVM's classloading guarantees for lazy, thread-safe initialization
- C) It is eagerly initialized when the outer class is loaded
- D) It is not thread-safe without explicit synchronization

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

The Holder Idiom works because:
- The inner class `Holder` is **not loaded** until `getInstance()` is called.
- The JVM guarantees that class initialization is **thread-safe** (done exactly once by a single thread).
- Therefore: lazy (created only when first accessed) + thread-safe (JVM handles it) + no synchronization overhead.
- No `volatile` needed. No `synchronized` needed. Clean and efficient.

</details>

---

### Q27.

**Which of the following is a sign that the Flyweight pattern is NOT appropriate?**

- A) You have millions of objects from 5 types
- B) The shared state is large and immutable
- C) Each object has unique, heavy data that cannot be shared
- D) Per-instance state is small (just position and velocity)

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Flyweight's benefit comes from sharing **heavy, identical intrinsic state**. If every object has **unique heavy data** (e.g., a unique mesh per character), there's nothing to share -- no benefit from Flyweight.

Options A, B, and D describe the **ideal scenario** for Flyweight: large N, small set of types, heavy immutable shared state, tiny per-instance state.

</details>

---

### Q28.

**What is the fundamental difference between Simple Factory and Factory Method?**

- A) Simple Factory uses interfaces; Factory Method uses abstract classes
- B) Simple Factory is a single static method centralizing `new`; Factory Method is an overridable method in a superclass whose subclasses decide the concrete product
- C) Simple Factory is thread-safe; Factory Method is not
- D) Simple Factory creates multiple products; Factory Method creates one

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

- **Simple Factory:** One class, one static method, a switch/if-else returning the right `new` object. Centralizes creation. Not a GoF pattern -- it's a technique.
- **Factory Method:** An abstract class has a fixed algorithm that at one step calls an abstract `createX()` method. **Subclasses override** this method to return different concrete products. The algorithm is inherited; only the creation step varies.

Key insight: Factory Method leverages **inheritance and polymorphism** for creation, while Simple Factory is just a centralized static utility.

</details>

---

### Q29.

**In the Adapter pattern, what type of adapter does Java typically use, and why?**

- A) Class adapter via multiple inheritance, because Java supports it
- B) Object adapter via composition, because Java does not support multiple class inheritance
- C) Object adapter via inheritance, because composition is slower
- D) Class adapter via interfaces, because interfaces allow multiple inheritance

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Java uses **Object Adapter** (composition):
- The adapter **implements** the target interface and **wraps** (has-a) the adaptee.
- Java does not support multiple class inheritance, so **Class Adapter** (which requires extending both target and adaptee classes) is not possible.
- Composition also provides better flexibility -- the adapter can work with any subclass of the adaptee.

</details>

---

### Q30.

**Which statement about the `final` keyword and immutability is correct?**

- A) Making a field `final` makes the object it references immutable
- B) A `final` field reference cannot be reassigned, but the object it points to can still be mutated
- C) A `final` class cannot have any methods
- D) `final` on a method means it returns an immutable value

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

`final` controls the **reference**, not the **object**:
```java
final List<String> list = new ArrayList<>();
list = new ArrayList<>();  // COMPILE ERROR -- can't reassign reference
list.add("hello");         // WORKS FINE -- can mutate the object!
```

This is exactly why immutable classes need **defensive copies** of mutable fields. `final` alone is NOT enough.

`final` on a class means it can't be subclassed (important for immutability -- prevents subclasses from adding mutable state). `final` on a method means it can't be overridden.

</details>

---

*Total: 30 Questions | Covers: All 5 SOLID principles, All 9 design patterns, Immutable Classes, PYQ-style code identification*
