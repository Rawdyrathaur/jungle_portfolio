# ☕ The Complete Junior Java Developer Guide — 2026 Edition

> **For developers with 0–2 years of experience** aiming to pass technical interviews, meet employer expectations, and build a world-class Java career.
>
> *Deep research compiled from InterviewBit, DataCamp, Glassdoor, HR Dive, BSWEN, and 200+ job postings — March 2026*
>
> 
## 📑 Table of Contents

1. [Core Java Fundamentals](#1-core-java-fundamentals)
2. [Spring Boot Essentials](#2-spring-boot-essentials)
3. [Data Structures & Algorithms](#3-data-structures--algorithms)
4. [Database Skills: SQL & Hibernate/JPA](#4-database-skills-sql--hibernatejpa)
5. [Real-World Skills Employers Expect](#5-real-world-skills-employers-expect)
6. [Tricky Interview Questions & Model Answers](#6-tricky-interview-questions--model-answers)
7. [Soft Skills — What Interviewers Really Look For](#7-soft-skills--what-interviewers-really-look-for)
8. [Trending Topics for 2026](#8-trending-topics-for-2026)
9. [Your 90-Day Roadmap to Interview Readiness](#9-your-90-day-roadmap-to-interview-readiness)

---

## 1. Core Java Fundamentals

> These topics appear in **virtually every Java interview**. Master them cold — they are non-negotiable.

---

### 1.1 Object-Oriented Programming (OOP) — The 4 Pillars

OOP is the **single most tested topic** at the junior level. You must be able to define each pillar AND give a real-world example.

| Pillar | One-Line Definition | Classic Interview Example |
|---|---|---|
| **Encapsulation** | Hide internal state; expose only via methods | Private fields + public getters/setters |
| **Abstraction** | Hide complexity, show only what's needed | Abstract class / Interface hiding implementation |
| **Inheritance** | Child class acquires parent class properties | `Dog extends Animal`; method reuse |
| **Polymorphism** | Same method name, different behavior | Overriding `toString()` in each subclass |

#### Encapsulation in Code

```java
public class BankAccount {
    private double balance;  // hidden internal state

    public double getBalance() {         // controlled access
        return balance;
    }

    public void deposit(double amount) { // controlled mutation
        if (amount > 0) balance += amount;
    }
}
```

#### Polymorphism in Code

```java
// Compile-time (Method Overloading)
public int add(int a, int b) { return a + b; }
public double add(double a, double b) { return a + b; }

// Runtime (Method Overriding)
class Animal { void sound() { System.out.println("..."); } }
class Dog extends Animal { @Override void sound() { System.out.println("Woof"); } }
```

---

### 1.2 JVM, JDK, JRE — Most Asked Starter Question

> 💡 **This is asked in nearly 100% of Java interviews. Memorize the hierarchy: JDK ⊃ JRE ⊃ JVM**

```
┌─────────────────────────────────────┐
│  JDK (Java Development Kit)         │
│  ┌───────────────────────────────┐  │
│  │  JRE (Java Runtime Env)       │  │
│  │  ┌─────────────────────────┐  │  │
│  │  │  JVM (Java Virtual Mach)│  │  │
│  │  └─────────────────────────┘  │  │
│  │  + Core Libraries             │  │
│  └───────────────────────────────┘  │
│  + Compiler (javac) + Debugger      │
└─────────────────────────────────────┘
```

- **JVM** — executes bytecode; platform-specific but bytecode is universal ("write once, run anywhere")
- **JRE** — JVM + core libraries; needed to *run* Java apps
- **JDK** — JRE + development tools (compiler, debugger); needed to *develop* Java apps
- **JIT Compiler** — converts bytecode to native machine code at runtime for performance

---

### 1.3 Collections Framework — Critical for Every Interview

> ⚠️ **ArrayList vs LinkedList, HashMap internals, and fail-fast iterators are asked constantly. Know the Big-O complexities.**

| Collection | Best For | Time Complexity (get/add/remove) |
|---|---|---|
| `ArrayList` | Fast random access | O(1) / O(1) amortized / O(n) middle |
| `LinkedList` | Fast insert/delete at ends | O(n) / O(1) ends / O(1) ends |
| `HashMap` | Key-value storage | O(1) avg / O(1) avg / O(1) avg |
| `LinkedHashMap` | Ordered key-value storage | O(1) / O(1) / O(1) |
| `TreeMap` | Sorted keys | O(log n) all operations |
| `HashSet` | Unique elements | O(1) contains |
| `ArrayDeque` | Stack / Queue | O(1) both ends |
| `ConcurrentHashMap` | Thread-safe HashMap | O(1) avg, thread-safe |
| `PriorityQueue` | Min/max heap | O(log n) add/remove, O(1) peek |

#### When to Use What

```java
// Need fast index access? → ArrayList
List<String> names = new ArrayList<>();

// Need fast add/remove at both ends (queue/deque)? → ArrayDeque
Deque<String> queue = new ArrayDeque<>();

// Need key-value pairs? → HashMap
Map<String, Integer> scores = new HashMap<>();

// Need sorted keys? → TreeMap
Map<String, Integer> sorted = new TreeMap<>();

// Need unique values only? → HashSet
Set<String> unique = new HashSet<>();
```

#### Fail-Fast vs Fail-Safe Iterators

| Type | Collections | Behavior |
|---|---|---|
| **Fail-Fast** | `ArrayList`, `HashMap` | Throws `ConcurrentModificationException` if modified during iteration |
| **Fail-Safe** | `CopyOnWriteArrayList`, `ConcurrentHashMap` | Iterates over a snapshot; no exception |

```java
// Fail-Fast — DO NOT modify during iteration
List<String> list = new ArrayList<>(Arrays.asList("a", "b", "c"));
for (String s : list) {
    list.remove(s); // ❌ throws ConcurrentModificationException
}

// Safe removal during iteration
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    it.next();
    it.remove(); // ✅ safe
}
```

---

### 1.4 Exception Handling

```
Throwable
├── Error          (JVM errors, don't catch: OutOfMemoryError, StackOverflowError)
└── Exception
    ├── Checked    (must handle: IOException, SQLException, ClassNotFoundException)
    └── RuntimeException (unchecked: NullPointerException, ArrayIndexOutOfBoundsException)
```

```java
// Basic try-catch-finally
try {
    FileReader fr = new FileReader("file.txt");
} catch (FileNotFoundException e) {
    System.out.println("File not found: " + e.getMessage());
} finally {
    System.out.println("Always runs — cleanup here");
}

// try-with-resources (auto-closes — PREFERRED pattern)
try (BufferedReader br = new BufferedReader(new FileReader("file.txt"))) {
    String line = br.readLine();
} catch (IOException e) {
    e.printStackTrace();
}

// Custom exception
public class InsufficientFundsException extends RuntimeException {
    public InsufficientFundsException(String message) {
        super(message);
    }
}
```

> **Q: What is the difference between checked and unchecked exceptions?**
>
> Checked exceptions are verified at **compile time** and must be handled (declared with `throws` or caught). Unchecked exceptions (subclasses of `RuntimeException`) occur at **runtime** and don't require explicit handling. For example, `IOException` is checked; `NullPointerException` is unchecked.

---

### 1.5 Memory Management & Garbage Collection

```
JVM Memory Layout
┌─────────────────────────────────────────────────┐
│  Heap (all objects live here)                   │
│  ┌───────────────────┐  ┌────────────────────┐  │
│  │  Young Generation  │  │  Old Generation    │  │
│  │  (new objects)     │  │  (survived GC)     │  │
│  └───────────────────┘  └────────────────────┘  │
├─────────────────────────────────────────────────┤
│  Stack (per thread — method calls, primitives)  │
├─────────────────────────────────────────────────┤
│  Metaspace (class metadata, since Java 8)       │
└─────────────────────────────────────────────────┘
```

- **Heap** — where objects are stored; managed by the Garbage Collector (GC)
- **Stack** — stores method call frames and local variables; each thread has its own stack
- **GC automatically reclaims** unreachable objects — no manual `free()` like in C/C++
- **Common GC algorithms**: G1 (default, balanced), ZGC (low latency, large heaps), Shenandoah

#### Common Memory Leak Causes

```java
// ❌ Memory leak — static collection grows forever
public class Cache {
    private static List<Object> cache = new ArrayList<>(); // objects never removed!
}

// ❌ Unclosed resources
Connection conn = getConnection();
// If an exception is thrown, conn is never closed → leak

// ✅ Fixed with try-with-resources
try (Connection conn = getConnection()) {
    // use connection
}
```

---

### 1.6 String Immutability & String Pool

```java
// String literal — uses String pool
String s1 = "hello";
String s2 = "hello";
System.out.println(s1 == s2);       // true (same pool reference)
System.out.println(s1.equals(s2));  // true

// new String — bypasses pool
String s3 = new String("hello");
System.out.println(s1 == s3);       // false (different object)
System.out.println(s1.equals(s3));  // true ← ALWAYS use .equals()

// StringBuilder — mutable, use in loops
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    sb.append("x"); // efficient — only one object mutated
}
String result = sb.toString();
```

| Class | Mutable? | Thread-Safe? | Use Case |
|---|---|---|---|
| `String` | No | Yes (immutable) | Default for all text |
| `StringBuilder` | Yes | No | String building in single thread |
| `StringBuffer` | Yes | Yes | String building in multi-thread |

---

### 1.7 Access Modifiers

| Modifier | Same Class | Same Package | Subclass | Everywhere |
|---|---|---|---|---|
| `private` | ✅ | ❌ | ❌ | ❌ |
| `default` (no keyword) | ✅ | ✅ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| `public` | ✅ | ✅ | ✅ | ✅ |

---

### 1.8 Key Keywords to Know

```java
// final — constant variable, non-overridable method, non-extendable class
final int MAX = 100;
final class ImmutableClass {}
public final void cannotOverride() {}

// static — belongs to class, not object
public static int count = 0;
public static void utilityMethod() {}

// this — reference to current object
public Person(String name) {
    this.name = name; // distinguishes field from parameter
}

// super — reference to parent class
public Dog() {
    super(); // calls parent constructor
}
```

---

## 2. Spring Boot Essentials

> Spring Boot powers **82.7% of Java enterprise applications**. It is the single most important framework to know.

---

### 2.1 What Is Spring Boot & Why It Matters

```
Spring Framework (Core DI + AOP + Web + Data)
         +
Auto-Configuration (reads classpath, auto-sets up beans)
         +
Embedded Server (Tomcat/Jetty/Undertow built-in)
         +
Production-Ready Features (Actuator, metrics, health)
         =
Spring Boot 🚀
```

> **KEY DISTINCTION**: Spring is the core framework. Spring Boot adds rapid-startup defaults on top. When people say "Spring" in job postings, they almost always mean Spring Boot.

---

### 2.2 Must-Know Annotations — Complete Reference

#### Application Layer Annotations

```java
@SpringBootApplication  // = @Configuration + @EnableAutoConfiguration + @ComponentScan
public class MyApp {
    public static void main(String[] args) {
        SpringApplication.run(MyApp.class, args);
    }
}
```

#### Stereotype Annotations (Bean Declaration)

```java
@Component      // generic Spring-managed bean
@Service        // business logic layer (semantically meaningful)
@Repository     // data access layer (+ exception translation)
@Controller     // web layer — returns view names
@RestController // web layer — returns JSON (@Controller + @ResponseBody)
```

#### Web/REST Annotations

```java
@RequestMapping("/api/users")   // base path for class
@GetMapping("/{id}")            // HTTP GET
@PostMapping                    // HTTP POST
@PutMapping("/{id}")            // HTTP PUT (full update)
@PatchMapping("/{id}")          // HTTP PATCH (partial update)
@DeleteMapping("/{id}")         // HTTP DELETE

@PathVariable Long id           // extract from URL path: /users/42
@RequestParam String name       // extract from query: ?name=John
@RequestBody UserDTO dto        // deserialize JSON body
@ResponseBody                   // serialize return value to JSON
@ResponseStatus(HttpStatus.CREATED) // set HTTP status code
```

#### Data/JPA Annotations

```java
@Entity           // maps class to DB table
@Table(name="users") // customize table name
@Id               // primary key
@GeneratedValue(strategy = GenerationType.IDENTITY) // auto-increment
@Column(name="first_name", nullable=false, length=50)
@OneToMany(mappedBy="user", cascade=CascadeType.ALL, fetch=FetchType.LAZY)
@ManyToOne
@JoinColumn(name="user_id")
@Transactional    // wraps method in DB transaction
```

#### Configuration Annotations

```java
@Configuration     // marks class as configuration source
@Bean              // declares a Spring bean via method
@Value("${app.name}") // injects value from application.properties
@ConfigurationProperties(prefix="app") // binds properties to POJO
@Profile("dev")    // activates bean only for 'dev' profile
@Conditional       // conditional bean creation
```

---

### 2.3 Dependency Injection (DI) — The Three Ways

```java
// ✅ RECOMMENDED: Constructor Injection
// — testable, immutable, explicit dependencies
@Service
public class OrderService {
    private final PaymentService paymentService;

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}

// ⚠️ OK: Setter Injection — for optional dependencies
@Service
public class ReportService {
    private EmailService emailService;

    @Autowired
    public void setEmailService(EmailService emailService) {
        this.emailService = emailService;
    }
}

// ❌ AVOID in production: Field Injection — hard to test
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository; // can't test without Spring context
}
```

---

### 2.4 Building a REST API — Full Example

```java
// Entity
@Entity
@Table(name = "products")
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;
    private double price;

    // constructors, getters, setters
}

// Repository
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {
    List<Product> findByNameContaining(String keyword); // Spring generates the query!
}

// Service
@Service
@Transactional
public class ProductService {
    private final ProductRepository repo;

    public ProductService(ProductRepository repo) {
        this.repo = repo;
    }

    public List<Product> getAllProducts() { return repo.findAll(); }
    public Product getById(Long id) {
        return repo.findById(id)
            .orElseThrow(() -> new RuntimeException("Product not found: " + id));
    }
    public Product save(Product p) { return repo.save(p); }
    public void delete(Long id) { repo.deleteById(id); }
}

// Controller
@RestController
@RequestMapping("/api/products")
public class ProductController {
    private final ProductService service;

    public ProductController(ProductService service) {
        this.service = service;
    }

    @GetMapping
    public ResponseEntity<List<Product>> getAll() {
        return ResponseEntity.ok(service.getAllProducts());
    }

    @GetMapping("/{id}")
    public ResponseEntity<Product> getById(@PathVariable Long id) {
        return ResponseEntity.ok(service.getById(id));
    }

    @PostMapping
    public ResponseEntity<Product> create(@Valid @RequestBody Product product) {
        return ResponseEntity.status(HttpStatus.CREATED).body(service.save(product));
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> delete(@PathVariable Long id) {
        service.delete(id);
        return ResponseEntity.noContent().build();
    }
}
```

---

### 2.5 application.properties / application.yml

```properties
# application.properties — common settings

# Server
server.port=8080

# Database
spring.datasource.url=jdbc:postgresql://localhost:5432/mydb
spring.datasource.username=admin
spring.datasource.password=secret
spring.datasource.driver-class-name=org.postgresql.Driver

# JPA / Hibernate
spring.jpa.hibernate.ddl-auto=update     # create / create-drop / update / validate / none
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

# Active profile
spring.profiles.active=dev

# Actuator
management.endpoints.web.exposure.include=health,info,metrics
```

```yaml
# application.yml — same settings, YAML format (cleaner for complex config)
server:
  port: 8080

spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/mydb
    username: admin
    password: secret
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
  profiles:
    active: dev
```

---

### 2.6 Bean Lifecycle

```
Container Starts
      ↓
Instantiation (constructor called)
      ↓
Dependency Injection (@Autowired fields/setters)
      ↓
@PostConstruct method (setup/init logic)
      ↓
Bean READY for use
      ↓
Application running...
      ↓
@PreDestroy method (cleanup logic)
      ↓
Bean destroyed → Container shuts down
```

```java
@Service
public class CacheService {

    @PostConstruct
    public void init() {
        System.out.println("Bean ready — loading cache...");
    }

    @PreDestroy
    public void cleanup() {
        System.out.println("Bean destroying — flushing cache...");
    }
}
```

---

## 3. Data Structures & Algorithms

> Junior developers are **NOT** expected to ace Hard LeetCode problems. Easy/Medium fundamentals are required.

---

### 3.1 Topics by Frequency & Difficulty

| Topic | Difficulty | Common Interview Problems |
|---|---|---|
| Arrays & Strings | 🟢 Easy | Reverse array, find duplicates, palindrome, anagram |
| HashMap / HashSet | 🟢 Easy | Two Sum, frequency count, first non-repeating char |
| Linked Lists | 🟡 Easy-Medium | Reverse list, detect cycle, merge sorted lists |
| Stacks & Queues | 🟡 Easy-Medium | Balanced parentheses, implement queue with stacks |
| Binary Search | 🟡 Medium | Search in sorted array, find peak element |
| Sorting Algorithms | 🟢 Easy (conceptual) | Know complexity of Bubble/Merge/Quick sort |
| Recursion | 🟡 Medium | Fibonacci, factorial, tree traversals |
| Trees (Binary) | 🟡 Medium | Inorder/Preorder/Postorder, height of tree |
| Graphs (BFS/DFS) | 🔴 Medium-Hard | Shortest path, connected components (rarely for juniors) |

---

### 3.2 Big-O Complexity Cheat Sheet

| Operation | Complexity | Notes |
|---|---|---|
| Array access by index | O(1) | |
| HashMap get/put (average) | O(1) | O(n) worst case (all collisions) |
| Binary Search | O(log n) | Array must be sorted |
| Linear Search | O(n) | |
| ArrayList add at end | O(1) amortized | |
| ArrayList insert in middle | O(n) | Shift elements |
| MergeSort | O(n log n) | Stable sort |
| QuickSort (average) | O(n log n) | O(n²) worst case |
| BubbleSort / SelectionSort | O(n²) | Never use in production |
| Tree traversal (all nodes) | O(n) | |

---

### 3.3 Most Common Coding Patterns with Examples

#### Two Pointers — O(n)

```java
// Check if string is palindrome
public boolean isPalindrome(String s) {
    int left = 0, right = s.length() - 1;
    while (left < right) {
        if (s.charAt(left) != s.charAt(right)) return false;
        left++;
        right--;
    }
    return true;
}
```

#### HashMap Frequency — O(n)

```java
// Two Sum — return indices of two numbers that add up to target
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[]{map.get(complement), i};
        }
        map.put(nums[i], i);
    }
    return new int[]{};
}
```

#### First Non-Repeating Character — O(n)

```java
// Most common junior interview question per Glassdoor
public char firstNonRepeating(String s) {
    Map<Character, Integer> freq = new LinkedHashMap<>();
    for (char c : s.toCharArray()) {
        freq.put(c, freq.getOrDefault(c, 0) + 1);
    }
    for (Map.Entry<Character, Integer> entry : freq.entrySet()) {
        if (entry.getValue() == 1) return entry.getKey();
    }
    return '\0';
}
```

#### Stack — Balanced Parentheses — O(n)

```java
public boolean isValid(String s) {
    Deque<Character> stack = new ArrayDeque<>();
    for (char c : s.toCharArray()) {
        if (c == '(' || c == '[' || c == '{') {
            stack.push(c);
        } else {
            if (stack.isEmpty()) return false;
            char top = stack.pop();
            if (c == ')' && top != '(') return false;
            if (c == ']' && top != '[') return false;
            if (c == '}' && top != '{') return false;
        }
    }
    return stack.isEmpty();
}
```

#### Binary Search — O(log n)

```java
public int binarySearch(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2; // avoids integer overflow
        if (arr[mid] == target) return mid;
        if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
```

---

### 3.4 Sorting Algorithm Comparison

| Algorithm | Best | Average | Worst | Space | Stable? | When to Use |
|---|---|---|---|---|---|---|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | Yes | Never (educational only) |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | No | Never |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | Yes | Small or nearly-sorted arrays |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes | When stability needed |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | No | General purpose (Java's default) |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) | No | When memory matters |

> `Arrays.sort()` in Java uses **Dual-Pivot QuickSort** for primitives and **TimSort** (Merge + Insertion) for objects.

---

## 4. Database Skills: SQL & Hibernate/JPA

---

### 4.1 Core SQL Skills

```sql
-- SELECT with filtering
SELECT first_name, last_name, salary
FROM employees
WHERE department = 'Engineering' AND salary > 70000
ORDER BY salary DESC
LIMIT 10;

-- Aggregate functions
SELECT department,
       COUNT(*) AS headcount,
       AVG(salary) AS avg_salary,
       MAX(salary) AS top_salary
FROM employees
GROUP BY department
HAVING COUNT(*) > 5  -- filter on groups, not rows
ORDER BY avg_salary DESC;
```

#### JOIN Types Explained

```sql
-- INNER JOIN: only matching rows from both tables
SELECT o.order_id, c.name
FROM orders o
INNER JOIN customers c ON o.customer_id = c.id;

-- LEFT JOIN: all rows from left, matching from right (NULL if no match)
SELECT c.name, o.order_id
FROM customers c
LEFT JOIN orders o ON c.id = o.customer_id;
-- Customers with no orders show NULL for order_id

-- RIGHT JOIN: all rows from right table
-- FULL OUTER JOIN: all rows from both tables
```

> **Q: What is the difference between WHERE and HAVING?**
>
> `WHERE` filters rows **before** grouping. `HAVING` filters groups **after** `GROUP BY`. You cannot use aggregate functions in `WHERE`, but you can in `HAVING`.

```sql
-- Wrong: can't use COUNT in WHERE
SELECT department FROM employees WHERE COUNT(*) > 5 GROUP BY department; -- ❌

-- Correct: use HAVING for aggregate filters
SELECT department FROM employees GROUP BY department HAVING COUNT(*) > 5; -- ✅
```

#### Indexing

```sql
-- Create index to speed up queries on a frequently searched column
CREATE INDEX idx_employees_email ON employees(email);

-- Composite index for queries filtering on multiple columns
CREATE INDEX idx_orders_customer_date ON orders(customer_id, created_at);
```

| Aspect | With Index | Without Index |
|---|---|---|
| SELECT speed | Fast O(log n) | Slow O(n) full scan |
| INSERT/UPDATE/DELETE speed | Slower (index maintained) | Faster |
| Storage | More space used | Less space |

---

### 4.2 ACID Properties (Transaction Guarantees)

| Property | Meaning | Example |
|---|---|---|
| **Atomicity** | All or nothing — partial success impossible | Bank transfer: debit + credit both succeed or both fail |
| **Consistency** | DB moves from one valid state to another | Foreign key constraints always maintained |
| **Isolation** | Concurrent transactions don't interfere | Two users reading same account simultaneously get correct data |
| **Durability** | Committed data survives crashes | Written to disk, not just memory |

---

### 4.3 Hibernate / JPA — Complete Setup

```java
// 1. Entity Mapping
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "email", unique = true, nullable = false, length = 100)
    private String email;

    @Column(name = "created_at")
    private LocalDateTime createdAt;

    // One user has many orders
    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    private List<Order> orders = new ArrayList<>();
}

@Entity
@Table(name = "orders")
public class Order {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    // Many orders belong to one user
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "user_id")
    private User user;
}

// 2. Repository (Spring Data JPA — ZERO SQL needed for common queries)
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    // Spring generates: SELECT * FROM users WHERE email = ?
    Optional<User> findByEmail(String email);

    // Spring generates: SELECT * FROM users WHERE email LIKE %?%
    List<User> findByEmailContaining(String keyword);

    // Custom JPQL query
    @Query("SELECT u FROM User u WHERE u.createdAt > :date")
    List<User> findUsersCreatedAfter(@Param("date") LocalDateTime date);

    // Native SQL query
    @Query(value = "SELECT * FROM users WHERE email LIKE %:domain", nativeQuery = true)
    List<User> findByEmailDomain(@Param("domain") String domain);
}
```

---

### 4.4 The N+1 Problem — Most Important ORM Interview Topic

> ⚠️ **The N+1 problem is a notorious interview question. Know what it is AND how to fix it.**

```java
// ❌ N+1 PROBLEM: 1 query for users + N queries for each user's orders
List<User> users = userRepository.findAll(); // Query 1: SELECT * FROM users
for (User user : users) {
    user.getOrders().size(); // Query 2-N: SELECT * FROM orders WHERE user_id = ?
}
// If 100 users → 101 queries! VERY slow.

// ✅ FIX 1: JOIN FETCH in JPQL — single query with JOIN
@Query("SELECT u FROM User u JOIN FETCH u.orders")
List<User> findAllWithOrders();

// ✅ FIX 2: @EntityGraph
@EntityGraph(attributePaths = {"orders"})
List<User> findAll();
```

---

### 4.5 Lazy vs Eager Loading

```java
// LAZY (recommended default) — orders loaded only when accessed
@OneToMany(fetch = FetchType.LAZY)   // don't load until needed
private List<Order> orders;

// EAGER — orders loaded with every User query (can be expensive)
@OneToMany(fetch = FetchType.EAGER)  // always load immediately
private List<Order> orders;
```

**Rule of thumb**: Default to `LAZY`. Use `EAGER` or `JOIN FETCH` only when you know you always need the related data.

---

## 5. Real-World Skills Employers Expect

> Based on analysis of 200+ Java job postings in 2026, these practical skills are **deal-breakers if missing**.

---

### 5.1 Git — Version Control (Non-Negotiable)

> 🚨 **Every single job posting lists Git as required.** If you can't resolve a merge conflict or explain branching strategy, you will fail the practical round.

```bash
# Daily workflow
git clone https://github.com/org/repo.git
git checkout -b feature/user-authentication  # create feature branch
git add .
git commit -m "feat: add JWT authentication for user login"
git push origin feature/user-authentication
# Open Pull Request → Code Review → Merge

# Keeping branch up to date
git checkout main
git pull origin main
git checkout feature/user-authentication
git rebase main  # or: git merge main

# Resolving a merge conflict
git status           # see conflicting files
# Edit conflicting files manually — remove <<<< ==== >>>> markers
git add resolved-file.java
git rebase --continue  # or: git commit

# Useful inspection commands
git log --oneline --graph    # visualize branch history
git diff main feature/branch  # compare branches
git stash                     # temporarily save uncommitted changes
git stash pop                 # restore stashed changes
```

#### Git Branching Strategy (GitFlow)

```
main (production)
  └── develop (integration)
        ├── feature/user-login
        ├── feature/payment-api
        └── hotfix/critical-bug → main (emergency fix)
```

---

### 5.2 Maven / Gradle — Build Tools

```xml
<!-- pom.xml — Maven dependency example -->
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

```bash
# Maven commands
mvn clean install     # clean build directory, compile, test, package
mvn spring-boot:run   # run Spring Boot app
mvn test              # run tests only
mvn package           # create .jar file
```

---

### 5.3 Microservices Architecture — Conceptual Knowledge

Even junior developers need to understand microservices — it's the dominant architecture in enterprise Java in 2026.

```
Client → API Gateway (routing, auth, rate limiting)
            ├── User Service    (port 8081, own DB)
            ├── Product Service (port 8082, own DB)
            ├── Order Service   (port 8083, own DB)
            └── Notification Service (async via Kafka)

Service Registry (Eureka):
  → Services register themselves on startup
  → Other services discover them by name, not hardcoded URL
```

#### Key Microservices Concepts

| Concept | Tool/Pattern | What to Know |
|---|---|---|
| API Gateway | Spring Cloud Gateway, Kong | Single entry point; routing, auth, rate limiting |
| Service Discovery | Netflix Eureka | Services find each other dynamically |
| Circuit Breaker | Resilience4j | Stops calls to failing service; prevents cascades |
| Message Queue | Apache Kafka, RabbitMQ | Async communication between services |
| Config Server | Spring Cloud Config | Centralized configuration management |
| Distributed Tracing | Zipkin, Sleuth | Track requests across services |

> **Monolith vs Microservices trade-offs:**
> Microservices enable independent scaling and deployment but add operational complexity (service discovery, distributed tracing, network latency). A monolith is often the right starting point for small teams.

---

### 5.4 Testing — JUnit 5 & Mockito

```java
// Unit Test — testing a single class in isolation
@ExtendWith(MockitoExtension.class)
class ProductServiceTest {

    @Mock
    private ProductRepository productRepository; // mocked — no real DB

    @InjectMocks
    private ProductService productService;       // real class being tested

    @Test
    void getById_whenProductExists_returnsProduct() {
        // Arrange
        Product expected = new Product(1L, "Laptop", 999.99);
        when(productRepository.findById(1L)).thenReturn(Optional.of(expected));

        // Act
        Product actual = productService.getById(1L);

        // Assert
        assertEquals(expected.getName(), actual.getName());
        assertEquals(expected.getPrice(), actual.getPrice());
        verify(productRepository, times(1)).findById(1L);
    }

    @Test
    void getById_whenProductNotFound_throwsException() {
        when(productRepository.findById(99L)).thenReturn(Optional.empty());

        assertThrows(RuntimeException.class, () -> productService.getById(99L));
    }
}

// Integration Test — loads Spring context, real database
@SpringBootTest
@AutoConfigureMockMvc
class ProductControllerIntegrationTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    void getAllProducts_returns200() throws Exception {
        mockMvc.perform(get("/api/products")
                .contentType(MediaType.APPLICATION_JSON))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$").isArray());
    }

    @Test
    void createProduct_withValidBody_returns201() throws Exception {
        String json = """
            {"name": "Keyboard", "price": 79.99}
            """;

        mockMvc.perform(post("/api/products")
                .contentType(MediaType.APPLICATION_JSON)
                .content(json))
            .andExpect(status().isCreated())
            .andExpect(jsonPath("$.name").value("Keyboard"));
    }
}
```

#### Testing Annotations Quick Reference

| Annotation | Purpose |
|---|---|
| `@Test` | Marks a test method |
| `@ExtendWith(MockitoExtension.class)` | Enables Mockito in unit tests |
| `@Mock` | Creates a mock object |
| `@InjectMocks` | Creates instance with mocks injected |
| `@SpringBootTest` | Loads full Spring context |
| `@WebMvcTest` | Loads only web layer (faster) |
| `@DataJpaTest` | Loads only JPA layer |
| `@BeforeEach` | Runs before each test |
| `@AfterEach` | Runs after each test |
| `@ParameterizedTest` | Runs test with multiple inputs |

---

### 5.5 Docker — Containerization Basics

```dockerfile
# Dockerfile for Spring Boot app
FROM eclipse-temurin:21-jre-alpine        # base image with Java 21 JRE (slim)
WORKDIR /app
COPY target/myapp-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

```yaml
# docker-compose.yml — run app + database together
version: '3.8'
services:
  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      - SPRING_DATASOURCE_URL=jdbc:postgresql://db:5432/mydb
      - SPRING_DATASOURCE_USERNAME=admin
      - SPRING_DATASOURCE_PASSWORD=secret
    depends_on:
      - db

  db:
    image: postgres:15-alpine
    environment:
      - POSTGRES_DB=mydb
      - POSTGRES_USER=admin
      - POSTGRES_PASSWORD=secret
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

```bash
# Docker commands to know
docker build -t myapp:latest .     # build image
docker run -p 8080:8080 myapp      # run container
docker ps                           # list running containers
docker logs <container-id>          # view logs
docker-compose up --build          # start all services
docker-compose down                 # stop all services
```

---

### 5.6 AWS Basics — What Junior Developers Need to Know

| Service | What It Is | Junior Level Expectation |
|---|---|---|
| **EC2** | Virtual servers in the cloud | Know what it is; deploy one app there as a portfolio project |
| **S3** | Object/file storage | Know it exists; uploading/downloading files from code |
| **RDS** | Managed relational databases | Spin up PostgreSQL instance instead of self-managing |
| **IAM** | Identity & access management | Understand roles, users, least-privilege principle |
| **Lambda** | Serverless functions | Know the concept; event-driven, no server management |
| **CloudWatch** | Monitoring & logs | Basic log viewing for deployed apps |

---

## 6. Tricky Interview Questions & Model Answers

> These questions are designed to reveal **depth of understanding**, not just memorization.

---

### 6.1 Core Java

**Q: Why is String immutable in Java?**

> Three reasons: **Security** (String used for class loading, DB connections, network — mutable strings would be a security hole), **Thread safety** (immutable objects are inherently safe without synchronization), and **Performance** (String pool caching only works because the value never changes). Changing a String always creates a new object.

---

**Q: What is the contract between `hashCode()` and `equals()`?**

> If `a.equals(b)` is `true`, then `a.hashCode()` MUST equal `b.hashCode()`. However, equal hashCodes don't guarantee equal objects (hash collision). If you break this contract — for example, override `equals()` without overriding `hashCode()` — your object will be stored in a HashMap but may never be found.

```java
// ALWAYS override both together
@Override
public boolean equals(Object o) {
    if (this == o) return true;
    if (!(o instanceof User)) return false;
    User u = (User) o;
    return Objects.equals(id, u.id);
}

@Override
public int hashCode() {
    return Objects.hash(id); // must use same field(s) as equals
}
```

---

**Q: Can you override a static method in Java?**

> **No.** Static methods belong to the class, not the instance. You can declare a static method with the same signature in a subclass — this is **method hiding**, not overriding. The method called depends on the **reference type** at compile time, not the actual object type at runtime. This is the opposite of runtime polymorphism.

```java
class Parent { static void greet() { System.out.println("Parent"); } }
class Child extends Parent { static void greet() { System.out.println("Child"); } }

Parent obj = new Child();
obj.greet(); // prints "Parent" — reference type decides for static methods
```

---

**Q: What is the difference between `final`, `finally`, and `finalize()`?**

| Term | Type | Purpose |
|---|---|---|
| `final` | Keyword | Variable = constant; method = non-overridable; class = non-inheritable |
| `finally` | Block | Always executes after try-catch; used for cleanup (closing resources) |
| `finalize()` | Method | Called by GC before reclaiming object — **deprecated in Java 9, avoid** |

---

**Q: What is autoboxing? When can it cause issues?**

> Autoboxing is the automatic conversion between primitive types and their wrapper classes (`int` ↔ `Integer`). Issues can arise with `==` comparison (compares references, not values for Integer outside -128 to 127 cache range) and `NullPointerException` when unboxing a null wrapper.

```java
Integer a = 127;
Integer b = 127;
System.out.println(a == b);   // true (cached range -128 to 127)

Integer c = 128;
Integer d = 128;
System.out.println(c == d);   // false (outside cache — different objects!)
System.out.println(c.equals(d)); // true (always use .equals() for wrappers)
```

---

### 6.2 OOP Tricky Questions

**Q: What is the difference between an abstract class and an interface?**

| Feature | Abstract Class | Interface |
|---|---|---|
| Instantiation | Cannot instantiate | Cannot instantiate |
| Concrete methods | Yes | Yes (default/static since Java 8) |
| State (instance vars) | Yes | No (only constants) |
| Constructors | Yes | No |
| Multiple inheritance | Only one parent | Can implement multiple |
| Use case | "is-a" with shared implementation | "can-do" capability contract |

```java
// Abstract class: Animal IS-A shared concept with default behavior
abstract class Animal {
    String name;
    abstract void makeSound();        // must override
    void breathe() { System.out.println("breathing"); }  // shared
}

// Interface: Flyable IS A CAPABILITY (not an "is-a" relationship)
interface Flyable { void fly(); }
interface Swimmable { void swim(); }

// A class can implement multiple interfaces but extend ONE abstract class
class Duck extends Animal implements Flyable, Swimmable {
    @Override public void makeSound() { System.out.println("Quack"); }
    @Override public void fly() { System.out.println("Flying"); }
    @Override public void swim() { System.out.println("Swimming"); }
}
```

---

### 6.3 Collections Tricky Questions

**Q: How does HashMap work internally?**

> HashMap uses an **array of buckets** (default size 16). When you `put(key, value)`:
> 1. Compute `key.hashCode()`
> 2. Apply hash function to get bucket index
> 3. If bucket is empty — store entry
> 4. If collision — Java 8+ uses linked list (≤8 entries) or red-black tree (>8 entries)
>
> On retrieval, same process: compute hash → find bucket → use `equals()` to find exact entry. Load factor (default 0.75) triggers resize to double capacity when 75% full.

---

**Q: What happens if you use a mutable object as a HashMap key?**

> If the object's state changes after insertion, its `hashCode()` changes too. The HashMap will look in the wrong bucket and never find the entry — effectively losing data. **Always use immutable objects (String, Integer, enums) or final field-based keys for HashMap.**

---

### 6.4 Spring Boot Tricky Questions

**Q: What is the difference between `@Controller` and `@RestController`?**

> `@Controller` is for Spring MVC — it returns view names that get resolved to HTML templates (e.g., Thymeleaf). `@RestController` = `@Controller` + `@ResponseBody` — it automatically serializes the return value to JSON/XML and writes directly to the HTTP response. Always use `@RestController` for REST API endpoints.

---

**Q: What happens if two beans of the same type exist and you `@Autowire` that type?**

> Spring throws `NoUniqueBeanDefinitionException`. Solutions:
> 1. `@Qualifier("beanName")` — specify which bean to inject
> 2. `@Primary` on the preferred bean — makes it the default choice
> 3. `@ConditionalOnProperty` — activate bean based on config value

```java
@Service @Primary
public class EmailNotificationService implements NotificationService {}

@Service
public class SmsNotificationService implements NotificationService {}

// With @Primary, this injects EmailNotificationService by default
@Autowired
private NotificationService notificationService;

// Or be explicit with @Qualifier
@Autowired
@Qualifier("smsNotificationService")
private NotificationService smsService;
```

---

**Q: What is `@Transactional` and when would you use it?**

> `@Transactional` wraps a method (or class) in a database transaction. If the method completes successfully, changes are committed. If a `RuntimeException` is thrown, all changes are rolled back automatically. Use it on service layer methods that perform multiple DB operations that must succeed or fail together.

```java
@Service
public class TransferService {

    @Transactional  // if any step fails, ALL steps are rolled back
    public void transferFunds(Long fromId, Long toId, double amount) {
        accountRepo.debit(fromId, amount);   // step 1
        auditRepo.log("debit", fromId);       // step 2
        accountRepo.credit(toId, amount);    // step 3 — if this fails, steps 1&2 roll back
    }
}
```

---

## 7. Soft Skills — What Interviewers Really Look For

> A 2026 survey of 1,005 US hiring managers found that **62% say hard and soft skills are equally important** — and 24% say soft skills matter MORE.

---

### 7.1 Top Soft Skills in Tech Interviews (2026 Survey)

| Rank | Skill | What It Means in an Interview |
|---|---|---|
| #1 | **Communication** | Explain code logic clearly; articulate trade-offs; ask good clarifying questions |
| #2 | **Professionalism** | Punctuality, preparation, respectful disagreement, follow-through |
| #3 | **Time Management** | Completing tasks, managing priorities, realistic estimates |
| #4 | **Accountability** | Owning mistakes; not blaming tools or teammates |
| #5 | **Resilience** | Staying calm when stuck; pivoting strategy instead of freezing |
| #6 | **Problem Solving** | Breaking complex problems into smaller steps; thinking out loud |
| #7 | **Critical Thinking** | Evaluating options; recognizing trade-offs |
| #8 | **Collaboration** | Mentioning how you worked with teammates; asking for help appropriately |

---

### 7.2 What Interviewers Actually Evaluate Beyond Technical Knowledge

- **Can they think aloud?** — Talk through your approach, not just code silently
- **Do they ask clarifying questions?** — Great engineers clarify requirements before building
- **How do they handle being stuck?** — Try something, explain your thinking, then ask for a hint gracefully
- **Do they know what they don't know?** — Saying *"I'm not sure, but here's how I'd find out"* beats guessing
- **Are they curious and learning-oriented?** — Mention side projects, recent things you've learned
- **Would I want to work with this person every day?** — Culture fit and collaborative attitude

---

### 7.3 STAR Method for Behavioral Questions

When asked "Tell me about a time when..." — structure your answer as:

```
S — Situation: Set the context briefly (2-3 sentences)
T — Task:      What was YOUR specific responsibility?
A — Action:    What did YOU do? (use "I", not "we")
R — Result:    Measurable outcome — what improved/was fixed/was learned?
```

**Example: "Tell me about a time you solved a difficult bug"**

> **S**: We had a production issue where our REST API was returning wrong data for about 5% of requests intermittently.
> **T**: I was assigned to investigate and fix it within 24 hours.
> **A**: I added detailed logging, reproduced the issue locally, and found a race condition in our cache invalidation logic. Two threads were updating the same cache key simultaneously without synchronization.
> **R**: I fixed it using `ConcurrentHashMap` and `computeIfAbsent()`. The issue was resolved in 6 hours. I also wrote a unit test to prevent regression.

---

### 7.4 Common Behavioral Questions to Prepare

1. Tell me about a time you had a disagreement with a teammate. How did you handle it?
2. Describe a situation where you had to learn something quickly under pressure.
3. Tell me about a project you're most proud of. What was your contribution?
4. Describe a time you made a mistake. What did you do?
5. Tell me about a time you had to deal with an unclear or changing requirement.
6. How do you approach a problem you've never seen before?
7. Tell me about a time you received critical feedback. How did you respond?

---

## 8. Trending Topics for 2026

> Knowing these topics separates candidates who memorized basics from those **actively keeping up with the ecosystem**.

---

### 8.1 Virtual Threads — Java 21 (Project Loom) 🔥 HOT TOPIC

> Virtual threads are **production-ready since Java 21** and are interview-worthy at forward-looking companies in 2026.

```
Traditional (Platform) Threads:
┌────────────┐    ┌────────────┐    ┌────────────┐
│ OS Thread  │    │ OS Thread  │    │ OS Thread  │
│  (~1MB)    │    │  (~1MB)    │    │  (~1MB)    │
└────────────┘    └────────────┘    └────────────┘
Max ~thousands per JVM (OS limit)

Virtual Threads (Java 21+):
┌──────────────────────────────────────────────┐
│              JVM Scheduler                   │
│  [vThread1][vThread2][vThread3]...[vThreadN] │
│  (millions, each ~few KB)                    │
│              ↓          ↓                   │
│        OS Thread 1  OS Thread 2              │
└──────────────────────────────────────────────┘
```

```java
// Creating virtual threads (Java 21)
// Method 1: Thread.ofVirtual()
Thread vThread = Thread.ofVirtual().start(() -> {
    System.out.println("Running in virtual thread");
});

// Method 2: ExecutorService with virtual thread factory
try (ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor()) {
    for (int i = 0; i < 1_000_000; i++) {  // 1 MILLION tasks easily!
        executor.submit(() -> {
            Thread.sleep(Duration.ofSeconds(1));  // blocks virtual thread, not OS thread
            return "done";
        });
    }
}

// Spring Boot 3.2+ — enable virtual threads in application.properties
// spring.threads.virtual.enabled=true
// That's it! All request handling uses virtual threads automatically.
```

**Why it matters for interviews**: Virtual threads eliminate the need for reactive/async programming (WebFlux, CompletableFuture chains) for most I/O-bound services while matching the performance. Write simple blocking code with the scalability of async.

---

### 8.2 Modern Java Features (Java 17–21)

#### Records — Java 16+ (Data Classes)

```java
// BEFORE (verbose POJO)
public class Point {
    private final int x;
    private final int y;
    public Point(int x, int y) { this.x = x; this.y = y; }
    public int x() { return x; }
    public int y() { return y; }
    @Override public boolean equals(Object o) { /* boilerplate */ }
    @Override public int hashCode() { /* boilerplate */ }
    @Override public String toString() { return "Point[x=" + x + ", y=" + y + "]"; }
}

// AFTER (Java record — generates all of the above automatically!)
public record Point(int x, int y) {}

// Usage
Point p = new Point(3, 4);
System.out.println(p.x());        // 3
System.out.println(p);             // Point[x=3, y=4]
System.out.println(p.equals(new Point(3, 4))); // true

// Use records as DTOs in Spring Boot
public record UserDTO(Long id, String email, String name) {}
```

#### Pattern Matching — Java 16+

```java
// BEFORE
if (obj instanceof String) {
    String s = (String) obj;  // redundant cast
    System.out.println(s.length());
}

// AFTER (Java 16+ pattern matching for instanceof)
if (obj instanceof String s) {  // cast + binding in one line
    System.out.println(s.length());
}

// Pattern matching in switch (Java 21)
String result = switch (obj) {
    case Integer i -> "Integer: " + i;
    case String s  -> "String: " + s.toUpperCase();
    case null      -> "null value";
    default        -> "Unknown: " + obj.getClass().getSimpleName();
};
```

#### Text Blocks — Java 15+

```java
// BEFORE — JSON/HTML in strings was a nightmare
String json = "{\n  \"name\": \"John\",\n  \"age\": 30\n}";

// AFTER — text blocks (clean multi-line strings)
String json = """
    {
      "name": "John",
      "age": 30
    }
    """;

// Super useful in tests
mockMvc.perform(post("/api/users")
    .contentType(MediaType.APPLICATION_JSON)
    .content("""
        {
          "email": "john@example.com",
          "password": "secret123"
        }
        """));
```

#### Sealed Classes — Java 17+

```java
// Sealed classes restrict which classes can extend them
public sealed interface Shape permits Circle, Rectangle, Triangle {}

public record Circle(double radius) implements Shape {}
public record Rectangle(double width, double height) implements Shape {}
public record Triangle(double base, double height) implements Shape {}

// Now switch is exhaustive — no default needed
double area = switch (shape) {
    case Circle c    -> Math.PI * c.radius() * c.radius();
    case Rectangle r -> r.width() * r.height();
    case Triangle t  -> 0.5 * t.base() * t.height();
};
```

#### Switch Expressions — Java 14+

```java
// Old switch statement (verbose, fall-through bugs)
String label;
switch (day) {
    case MONDAY: case TUESDAY: case WEDNESDAY:
        label = "Weekday"; break;
    case SATURDAY: case SUNDAY:
        label = "Weekend"; break;
    default: label = "Unknown";
}

// New switch expression (concise, exhaustive, no fall-through)
String label = switch (day) {
    case MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY -> "Weekday";
    case SATURDAY, SUNDAY -> "Weekend";
};
```

---

### 8.3 Java Version Timeline — LTS Versions to Know

| Version | LTS? | Key Features | Status |
|---|---|---|---|
| Java 8 | LTS | Lambdas, Streams, Optional, Method References | Many legacy systems |
| Java 11 | LTS | `var` in lambdas, HTTP Client, String methods | Widely used |
| Java 17 | LTS ✅ | Sealed classes, Pattern matching, Records (final) | Current enterprise standard |
| Java 21 | LTS ✅ | Virtual threads, Record patterns, Sequenced collections | **Target for 2026** |
| Java 25 | LTS (upcoming) | Project Valhalla (value types preview) | Watch this space |

> **For interviews**: Know Java 8 features (lambdas, streams) AND Java 17+ features. Mention "I'm learning Java 21's virtual threads" — it signals you're forward-looking.

---

### 8.4 GraalVM Native Image

```
Traditional Java App:
Source Code → Bytecode → JVM (runtime) → Machine Code
Startup: slow (~2–5 seconds), warm-up needed

GraalVM Native Image:
Source Code → [Ahead-of-Time compilation] → Native executable
Startup: fast (~50ms), no JVM required, lower memory
```

- Spring Boot 3 natively supports GraalVM compilation (`mvn -Pnative native:compile`)
- Ideal for serverless/Lambda functions where cold-start time matters
- Trade-off: longer build time; some reflection-heavy code needs configuration hints

---

### 8.5 AI Tools in Development — 2026 Reality

> **Employers now expect junior developers to use AI coding tools (GitHub Copilot, Claude) productively. The key: you must be able to explain and debug every line of AI-generated code.**

**What interviewers look for:**
- Can you use AI to accelerate work without losing understanding?
- Do you validate AI output or blindly copy-paste?
- Can you explain why the code works, even if AI suggested it?

**Red flag**: Submitting AI-generated code you can't explain. This is immediately caught in follow-up questions.

---

## 9. Your 90-Day Roadmap to Interview Readiness

---

### Month 1 — Solidify the Foundation

- [ ] Master all 4 OOP pillars with code examples YOU wrote (not copied)
- [ ] Implement ArrayList and HashMap from scratch once — to truly understand internals
- [ ] Solve 2 Easy LeetCode problems daily: Arrays, Strings, HashMaps
- [ ] Build a Spring Boot REST API: CRUD for one entity, connected to PostgreSQL
- [ ] Learn Git branching: create feature branches, open PRs on your own GitHub

**Goal**: Confidently answer every question in Sections 1 and 6 of this guide.

---

### Month 2 — Expand and Build

- [ ] Add JUnit 5 + Mockito tests to your Spring Boot project (aim for 70%+ coverage)
- [ ] Learn SQL: practice JOINs, GROUP BY, subqueries on a real dataset (use SQLZoo or LeetCode SQL)
- [ ] Dockerize your Spring Boot app — write a Dockerfile and docker-compose.yml
- [ ] Study Spring Boot annotations deeply: know WHY each one exists, not just what it does
- [ ] Solve Medium LeetCode: Binary Search, Two Pointers, Sliding Window patterns
- [ ] Read and diagram a simple microservices architecture from a blog post or YouTube

**Goal**: Deploy your Spring Boot app + database using docker-compose locally.

---

### Month 3 — Interview Prep & Portfolio

- [ ] Build 1 portfolio project (see checklist below)
- [ ] Practice 10 behavioral questions using STAR method (record yourself — watch it back)
- [ ] Study Java 21 features: Records, Pattern Matching, Virtual Threads at conceptual level
- [ ] Do 5 mock interviews with a timer — treat them as real
- [ ] Answer every Q&A in Section 6 of this guide out loud, without reading
- [ ] Apply to 3–5 targeted roles per week

**Goal**: One deployed portfolio project + 10 STAR stories ready.

---

### Portfolio Project — Gold Standard Checklist

Employers want to see **real-world patterns** over tutorial clones. A simple but complete project beats an impressive-sounding half-finished one.

**Minimum Viable Portfolio Project:**

- [ ] REST API with 4+ endpoints (GET, POST, PUT, DELETE)
- [ ] Spring Security with JWT authentication
- [ ] PostgreSQL or MySQL database with JPA/Hibernate relationships (at least @OneToMany)
- [ ] Unit tests with JUnit 5 + Mockito (at least 10 tests)
- [ ] Integration test for at least one controller endpoint
- [ ] Dockerized with `docker-compose.yml` for easy local setup
- [ ] `README.md` with architecture diagram, setup instructions, and API documentation
- [ ] Deployed somewhere: Railway, Render, AWS EC2, or Heroku
- [ ] GitHub repo with clean commit history (conventional commits are a bonus)
- [ ] No hardcoded secrets — use environment variables or `.env` files

**Bonus points:**
- Swagger/OpenAPI documentation (`springdoc-openapi-starter-webmvc-ui`)
- GitHub Actions CI/CD pipeline that runs tests on every push
- Pagination on list endpoints (`Page<T>` with Spring Data)
- Custom exception handling with `@RestControllerAdvice`

---

### Resources — Curated Learning Path

| Resource | What to Use It For |
|---|---|
| **LeetCode** | Coding practice — filter Easy/Medium, Java, Arrays/HashMap/LinkedList |
| **Baeldung.com** | Best Java/Spring Boot tutorials on the web — bookmark it |
| **Spring.io/guides** | Official Spring Boot getting-started guides |
| **SQLZoo.net** | Interactive SQL practice |
| **interviewing.io** | Mock technical interviews (free practice rooms available) |
| **GitHub** | Study open-source Spring Boot projects — read real code |
| **Java Documentation (oracle.com/java)** | Reference for any API questions |
| **YouTube: Amigoscode** | Clear Spring Boot / Java tutorials |

---

## Quick Reference — Interview Day Cheat Sheet

```
Before the interview:
✅ Review OOP pillars + hashCode/equals contract
✅ Review HashMap internals
✅ Prepare 5 STAR stories
✅ Know your portfolio project end-to-end

During the coding round:
✅ Ask clarifying questions FIRST (input types? edge cases? constraints?)
✅ Think out loud — narrate your approach
✅ Start with brute force, then optimize
✅ Test your solution with an example

Common mistakes to avoid:
❌ Using == for String comparison
❌ Ignoring null checks
❌ Using raw types (List instead of List<String>)
❌ Forgetting to close resources (use try-with-resources)
❌ Modifying a collection while iterating

Most-asked one-liners:
• String is immutable → security, thread safety, String pool caching
• == vs .equals() → reference vs value comparison
• abstract class vs interface → "is-a" with state vs "can-do" capability
• checked vs unchecked → compile-time enforced vs runtime exceptions
• ArrayList vs LinkedList → random access vs frequent end insertions
• @Transactional → auto rollback on RuntimeException
• N+1 problem → use JOIN FETCH or @EntityGraph
• virtual threads → JVM-managed lightweight threads, Java 21
```

---

*You have everything you need. Now go build, practice, and iterate.* ☕

*Deep research compiled March 2026 from InterviewBit, DataCamp, Glassdoor, HR Dive, BSWEN, Spring.io, Oracle Java Docs, and 200+ job postings.*
