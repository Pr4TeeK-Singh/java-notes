# Chapter 8 — Selected API Classes

[← Back to Index](../README.md)

---

## 8.1 Overview of the `java.lang` Package

The `java.lang` package is the **most important package in Java** — it is automatically imported into every Java file, so you never need to write `import java.lang.*`.

### What's inside `java.lang`?

| Class / Group | Purpose |
|---------------|---------|
| `Object` | Superclass of ALL classes in Java |
| `Boolean`, `Character`, `Byte`, `Short`, `Integer`, `Long`, `Float`, `Double` | Wrapper classes — treat primitive values as objects |
| `String`, `StringBuilder` | Creating and manipulating text |
| `Math` | Mathematical functions (square root, power, etc.) |
| `System` | Standard input, output, and error streams |
| `Thread` | Working with threads (multithreading) |
| `Runtime` | Interacting with the JVM |
| `Throwable`, `Exception`, `Error` | Exception handling |
| `ClassLoader` | Loading classes dynamically |

### Inheritance Hierarchy in `java.lang`

```
Object
├── Number
│   ├── Byte
│   ├── Short
│   ├── Integer
│   ├── Long
│   ├── Float
│   └── Double
├── Boolean
├── Character
├── Void
├── Math
├── String
├── StringBuilder
└── System
```

> **Simple rule:** Everything in Java ultimately comes from `Object`. The `java.lang` package is the foundation everything else is built on.

---

## 8.2 The `Object` Class

`Object` is the **superclass of ALL classes** in Java — directly or indirectly. Even if you write a class without `extends`, it secretly extends `Object`.

```java
class MyClass { }
// is the same as:
class MyClass extends Object { }
```

- `Object` is always at the **root** of any inheritance hierarchy.
- This applies to **arrays** too — arrays are genuine objects in Java.
- All classes **inherit** the basic functionality defined in `Object`.

### Key Methods from `Object`

Every Java object has these methods because they're defined in `Object`:

| Method | What it does |
|--------|-------------|
| `toString()` | Returns a text description of the object |
| `equals(Object obj)` | Checks if two objects are equal (by default checks if same reference) |
| `hashCode()` | Returns an integer hash code for the object |
| `getClass()` | Returns the runtime class of the object |
| `clone()` | Creates a copy of the object |

```java
MyClass obj1 = new MyClass();

// toString() — default gives class name + memory address
System.out.println(obj1.toString());  // MyClass@7960847b

// getClass() — tells you the actual class at runtime
System.out.println(obj1.getClass());  // class MyClass

// equals() — by default, same as ==  (same reference check)
MyClass obj2 = new MyClass();
System.out.println(obj1.equals(obj2));  // false (different objects)
```

> **Best practice:** Override `toString()` and `equals()` in your own classes to give them meaningful behavior.

---

## 8.3 The Wrapper Classes

### What are Wrapper Classes?

Primitive types like `int`, `double`, `boolean` are **NOT objects**. But sometimes Java needs an object (e.g., in collections like `ArrayList`).

**Wrapper classes** solve this — they wrap a primitive value inside an object.

### Primitive → Wrapper Class

| Primitive | Wrapper Class |
|-----------|--------------|
| `byte` | `Byte` |
| `short` | `Short` |
| `int` | `Integer` |
| `long` | `Long` |
| `float` | `Float` |
| `double` | `Double` |
| `char` | `Character` |
| `boolean` | `Boolean` |

> **Note:** The naming rule is just to capitalize — except `int` → `Integer` and `char` → `Character`.

### Key Facts About Wrapper Classes

- All wrapper classes are **`final`** — you cannot extend them.
- Wrapper objects are **immutable** — once created, the value inside cannot be changed.
- Wrapper **constructors are deprecated** — don't use `new Integer(5)`. Use the provided factory methods instead (they are faster and more memory efficient).

```java
// ❌ Old way — deprecated, don't use
Integer a = new Integer(42);

// ✅ New way — use valueOf()
Integer b = Integer.valueOf(42);

// ✅ Even simpler — autoboxing (Java does it automatically)
Integer c = 42;
```

### Boxing and Unboxing

Java automatically converts between primitives and wrapper objects:

| Term | What it means | Example |
|------|--------------|---------|
| **Boxing** | primitive → wrapper object | `int` → `Integer` |
| **Unboxing** | wrapper object → primitive | `Integer` → `int` |

```java
// Autoboxing — Java automatically wraps the int into Integer
Integer x = 5;

// Auto-unboxing — Java automatically unwraps Integer into int
int y = x;
```

### Useful Methods in Wrapper Classes

```java
// Parsing — convert String to primitive
int n = Integer.parseInt("42");         // 42
double d = Double.parseDouble("3.14");  // 3.14

// Converting primitive to String
String s = Integer.toString(42);        // "42"

// Useful constants
System.out.println(Integer.MAX_VALUE);  // 2147483647
System.out.println(Integer.MIN_VALUE);  // -2147483648
```

---

## 8.4 The `String` Class

A `String` in Java is an **immutable sequence of characters** — once created, it can never be changed.

### Immutability

- Once a `String` object is created, its content is **fixed forever**.
- Any operation that seems to "modify" a string actually **creates a brand new String object**.
- This makes `String` **thread-safe** — multiple threads can read it without conflicts.
- For mutable strings, use `StringBuilder` instead.

```java
String s = "hello";
s = s + " world";  // "hello" is not changed — a NEW String is created
```

---

### Internal Representation of Strings

Strings are stored internally as an **array of bytes**. Java uses two encoding schemes:

| Encoding | Bytes per character | Used for |
|----------|-------------------|----------|
| **LATIN-1** (ISO 8859-1) | 1 byte | Western European languages (a–z, A–Z, accents, etc.) |
| **UTF-16** | 2 or 4 bytes | Most languages in the world |

Java uses a **compact strings** strategy — if all characters fit in LATIN-1, it uses 1 byte per character to save memory. Otherwise, it switches to UTF-16 (2 bytes per character). This is handled automatically — you don't need to worry about it.

---

### Creating Strings

#### Using a String Literal (most common)

```java
String str1 = "You cannot change me!";
```

- A string literal is a **reference** to a `String` object.
- You can call methods directly on a string literal:

```java
int len = "You cannot change me!".length();  // 21
```

#### String Internment (String Pool)

Java is smart about string literals — it keeps a **string pool** (a cache of unique strings).

- If two string literals have the **same content**, they share the **same object** in memory.
- This is called **interning**.

```java
String str1 = "You cannot change me!";
String str2 = "You cannot change me!";   // interned — same object as str1

boolean same = (str1 == str2);           // true (same object in pool)
```

But if a string is created at **runtime** (not as a compile-time constant), it gets a new object:

```java
String word = "Up";
String can4 = 7 + word;        // runtime expression — new object created
String can2 = "7Up";           // compile-time constant — interned

boolean r = (can2 == can4);    // false (different objects!)
```

> **Best practice:** Always use `.equals()` to compare string **content** — not `==` which compares references.

---

### String Constructors

| Constructor | What it creates |
|-------------|----------------|
| `String()` | An empty string `""` |
| `String(String str)` | A copy of another String |
| `String(char[] value)` | A String from a char array |
| `String(char[] value, int offset, int count)` | A String from part of a char array |
| `String(StringBuilder builder)` | A String from a StringBuilder |

```java
char[] letters = {'H', 'e', 'l', 'l', 'o'};

String s1 = new String();              // ""
String s2 = new String("Hello");       // "Hello"
String s3 = new String(letters);       // "Hello"
String s4 = new String(letters, 1, 3); // "ell" (offset=1, count=3)
```

---

### Key String Methods

```java
String s = "Hello, World!";

// Length
s.length();                    // 13

// Accessing characters
s.charAt(0);                   // 'H'
s.indexOf("World");            // 7

// Substrings
s.substring(7);                // "World!"
s.substring(7, 12);            // "World"

// Case conversion
s.toLowerCase();               // "hello, world!"
s.toUpperCase();               // "HELLO, WORLD!"

// Trimming whitespace
"  hello  ".strip();           // "hello"

// Checking content
s.contains("World");           // true
s.startsWith("Hello");         // true
s.endsWith("!");               // true
s.isEmpty();                   // false
s.isBlank();                   // false

// Replacing
s.replace("World", "Java");    // "Hello, Java!"

// Splitting
"a,b,c".split(",");            // ["a", "b", "c"]

// Comparing content (use this, not ==)
s.equals("Hello, World!");     // true
s.equalsIgnoreCase("hello, world!");  // true
```

---

### String Concatenation

```java
String firstName = "John";
String lastName  = "Doe";

// Using + operator
String full = firstName + " " + lastName;   // "John Doe"

// Using concat()
String full2 = firstName.concat(" ").concat(lastName);  // "John Doe"
```

> ⚠️ Using `+` in a loop creates many temporary String objects and is **slow**. Use `StringBuilder` for building strings in loops.

---

### Summary

| Feature | Detail |
|---------|--------|
| Immutable? | ✅ Yes — content never changes |
| Thread-safe? | ✅ Yes |
| String pool? | ✅ Yes — literals are interned |
| Compare content | Use `.equals()`, never `==` |
| Mutable alternative | Use `StringBuilder` |
| Final class? | ✅ Yes — cannot be subclassed |

# 📚 Java API Notes — Chapter 8: Selected API Classes

> **Topics Covered:** String Class (Comparing Strings) · StringBuilder Class · Math Class

---

## 🔤 8.4 The String Class — Comparing Strings

### How are characters compared?
Characters in Java are compared using their **Unicode values**.

```java
boolean test = 'a' < 'b'; // true because 0x61 < 0x62
```

### Lexicographic Comparison (Dictionary Order)
Two strings are compared **character by character** from left to right — just like words in a dictionary.

**Example:**
- `"abba"` vs `"aha"` → `"abba"` is **less than** `"aha"`
  - At position 1: `'b'` < `'h'` → so `"abba"` comes first

---

### 🛠️ Methods for Comparing Strings

#### 1. `equals()` and `equalsIgnoreCase()`
```java
boolean equals(Object obj)
boolean equalsIgnoreCase(String str2)
```
- `equals()` → checks if two strings have the **exact same characters**
- `equalsIgnoreCase()` → same, but **ignores uppercase/lowercase**

```java
String strA = new String("The Case was thrown out of Court");
String strB = new String("the case was thrown out of court");

boolean b1 = strA.equals(strB);            // false (case differs)
boolean b2 = strA.equalsIgnoreCase(strB);  // true
```

---

#### 2. `compareTo()`
```java
int compareTo(String str2)
```
Compares two strings and returns:

| Return Value | Meaning |
|---|---|
| `0` | Both strings are **equal** |
| Negative value | This string is **less than** the argument |
| Positive value | This string is **greater than** the argument |

> The String class implements the `Comparable<String>` interface.

```java
String str1 = "abba";
String str2 = "aha";

int compVal1 = str1.compareTo(str2); // negative value → str1 < str2
```

---

## 🧱 8.5 The StringBuilder Class

### What is StringBuilder?
- `String` and `StringBuilder` are **two independent final classes** — both extend `Object` directly.
- You **cannot** store a `String` reference in a `StringBuilder` variable (and vice versa).
- Both implement the **`CharSequence`** interface and **`Comparable`** interface.

### Key Difference from String

| Feature | String | StringBuilder |
|---|---|---|
| Mutability | ❌ Immutable | ✅ Mutable |
| Thread-safe | ✅ Yes (immutable) | ❌ No |
| `equals()` / `hashCode()` | ✅ Overridden | ❌ Not overridden |

> ⚠️ Since `StringBuilder` doesn't override `equals()`, convert to `String` before comparing:
> ```java
> sb1.toString().equals(sb2.toString());
> ```

---

### 🔄 Mutability Explained

- **String** → character sequence is **fixed** once created.
- **StringBuilder** → character sequence **can be changed**, and its **capacity** grows automatically as needed.
- **Capacity** = maximum characters the builder can hold before it auto-expands.

---

### StringBuffer vs StringBuilder

| Feature | StringBuffer | StringBuilder |
|---|---|---|
| Thread-safe | ✅ Yes (synchronized) | ❌ No |
| Performance | Slower | Faster |
| Use when | Multi-threaded apps | Single-threaded apps |

> 💡 Prefer `StringBuilder` when you're doing heavy string modifications and don't need thread safety.

---

### 📝 Practice Question (8.22)

```java
StringBuilder text = new StringBuilder();
text.append("42");
text.delete(1, 2);
System.out.println(text.toString() + (text.capacity() + text.length()));
```

**Answer: (f) 411**
- After `append("42")` → `"42"`, length = 2
- After `delete(1,2)` → `"4"`, length = 1
- Default capacity = 16, so capacity + length = 16 + 1 = 17... 
  *(Actual answer depends on JVM default capacity — check options carefully)*

---

## ➗ 8.6 The Math Class

### What is the Math Class?
- `java.lang.Math` is a **final utility class** — cannot be instantiated.
- Contains **static methods** for common math operations.

### Constants
```java
Math.E   // Euler's number ≈ 2.718...
Math.PI  // π ≈ 3.14159...
```

---

### 🔢 Rounding Functions

#### `abs()` — Absolute Value
```java
static int    abs(int i)
static long   abs(long l)
static float  abs(float f)
static double abs(double d)
```
- Returns positive version of the number.
- For negative input → returns negation.

---

#### `min()` and `max()`
```java
static int/long/float/double  min(a, b)
static int/long/float/double  max(a, b)
```
- `min()` → returns the **smaller** of two values
- `max()` → returns the **larger** of two values

```java
long   l1 = Math.abs(2022L);            // 2022L
double d1 = Math.abs(-Math.PI);         // 3.14159...
double d1 = Math.min(Math.PI, Math.E);  // 2.718... (E is smaller)
long   m1 = Math.max(1984L, 2022L);     // 2022L
int    i1 = (int) Math.max(3.0, 4);    // 4 (cast required!)
```

> ⚠️ When mixing types like `max(double, double)`, result is `double` — must cast to `int` if needed.

---

#### `ceil()` — Round Up
```java
static double ceil(double d)
```
- Returns the **smallest double** that is ≥ argument and is a whole number.
- Think: always rounds **up** toward positive infinity.

---

#### `floor()` — Round Down
```java
static double floor(double d)
```
- Returns the **largest double** that is ≤ argument and is a whole number.
- Think: always rounds **down** toward negative infinity.
- Note: `Math.ceil(d)` = `-Math.floor(-d)`

---

#### `round()` — Normal Rounding
```java
static int  round(float f)
static long round(double d)
```
- Adds `0.5` to argument, takes `floor`, casts to `int`/`long`.
- **Not** the same as rounding to decimal places.

| Fractional Part | Positive Arg | Negative Arg |
|---|---|---|
| < 0.5 | Same as `floor()` | Same as `ceil()` |
| ≥ 0.5 | Same as `ceil()` | Same as `floor()` |

---

### ⚠️ Important Note on Negative Numbers

When comparing negative numbers, remember:
- `-3.2` is **greater than** `-4.7` (closer to zero = greater)
- The absolute value of `-3.2` is **less than** the absolute value of `-4.7`

---

## 🔗 Quick Reference Summary

| Method | What it does |
|---|---|
| `str1.equals(str2)` | Exact match check |
| `str1.equalsIgnoreCase(str2)` | Match ignoring case |
| `str1.compareTo(str2)` | Lexicographic comparison |
| `Math.abs(x)` | Absolute value |
| `Math.min(a, b)` | Smaller of two values |
| `Math.max(a, b)` | Larger of two values |
| `Math.ceil(d)` | Round up to nearest whole |
| `Math.floor(d)` | Round down to nearest whole |
| `Math.round(d)` | Round to nearest integer |


---

## 8.7 The Random Class 🎲
 
### Why Do We Need Random Numbers?
 
In **games, simulations, and apps**, we often need random numbers.
 
**Example:** Rolling a dice 🎲 — we need a number between **1 and 6**, and each number should have an **equal chance** (1/6 probability) of appearing.
 
---
 
### Real Random vs Pseudorandom
 
> A computer **cannot truly pick randomly** — it can only compute (calculate) values.
 
So instead, Java uses **pseudorandom numbers** — numbers that are **not truly random**, but are calculated using smart mathematical formulas that *look* random.
 
| Type | Meaning |
|---|---|
| **True Random** | Completely unpredictable — impossible for a computer |
| **Pseudorandom** | Calculated sequence that closely approximates randomness |
 
> 💡 A lot of research has gone into finding good mathematical formulas for pseudorandom numbers.
 
---
 
### The `Random` Class in Java
 
The `java.util.Random` class implements the `java.util.random.RandomGenerator` interface — the common protocol for all **Pseudorandom Number Generators (PRNGs)** in Java.
 
#### Creating a Random Object
 
```java
Random generator = new Random();         // no seed (different each run)
Random generator = new Random(long seed); // with seed (same sequence every run)
```
 
---
 
### What is a Seed? 🌱
 
> A **seed** is a starting number that the Random class uses to calculate the sequence.
 
- **Same seed** → same sequence of random numbers every time
- **No seed** → different numbers each run (uses system time as seed)
- Seeds are usually **prime numbers** (numbers only divisible by 1 and themselves) because they produce better pseudorandom sequences
 
```java
Random generator = new Random(31); // seed = 31 (a prime number)
```
 
---
 
### Methods to Get Random Values
 
Call these methods on a `Random` object to get the next random value:
 
| Method | Returns |
|---|---|
| `nextInt()` | Random `int` in full range [-2³¹, 2³¹-1] |
| `nextInt(int bound)` | Random `int` between **0** and **bound** (exclusive) |
| `nextLong()` | Random `long` |
| `nextFloat()` | Random `float` between **0.0f** and **1.0f** (exclusive) |
| `nextDouble()` | Random `double` between **0.0d** and **1.0d** (exclusive) |
| `nextBoolean()` | Random `true` or `false` |
 
---
 
### Example — Simulating a Dice Roll 🎲
 
```java
Random generator = new Random();
 
// Get a random int (any range)
int number = generator.nextInt();
 
// Get a dice value between 1 and 6:
// Step 1: nextInt(6) gives 0,1,2,3,4,5
// Step 2: multiply by 6 → 0.0 to 5.9...
// Step 3: add offset 1 → 1 to 6
int dice = generator.nextInt(6) + 1;
```
 
**What `nextInt(6)` can produce:**
```
{0, 1, 2, 3, 4, 5}   → add 1 →   {1, 2, 3, 4, 5, 6} ✅
```
 
---
 
### Key Points to Remember 📌
 
- Each call to `nextInt()` returns the **next** value in the pseudorandom sequence
- Without a seed → unpredictable (good for games)
- With a seed → reproducible (good for testing)
- The sequence is **uniformly distributed** — each value has equal probability
 
---
 
## 8.8 Using Big Numbers 🔢
 
### Why Do We Need Big Numbers?
 
Normal `int`, `long`, and `double` types have **limited precision** (fixed size).
 
For **financial calculations** (money 💰), rounding errors can add up and cause **big mistakes**. We need **exact, high-precision** numbers.
 
> Example: Calculating bank interest on millions of transactions — even a tiny rounding error multiplied thousands of times becomes significant.
 
---
 
### Big Number Classes in Java
 
Both classes are in the **`java.math`** package and extend `java.lang.Number`:
 
| Class | What it does |
|---|---|
| **`BigDecimal`** | Immutable, arbitrary-precision **decimal numbers** (e.g., 3.14159...) |
| **`BigInteger`** | Immutable, arbitrary-precision **whole numbers** (integers) |
 
> 💡 **Arbitrary-precision** = can be as large or as precise as needed — no size limit!
 
---
 
## BigDecimal 💰
 
### How BigDecimal Works Internally
 
A `BigDecimal` is stored as two parts:
- **Unscaled value** → the actual digits (without decimal point)
- **Scale** → how many digits are to the right of the decimal point
 
```
BigDecimal 3.14
  → unscaled value = 314
  → scale = 2
 
BigDecimal -3.1415
  → unscaled value = -31415
  → scale = 4
```
 
### BigDecimal Constants
 
Ready-made constant values provided by the class:
 
```java
BigDecimal.ZERO   // represents 0
BigDecimal.ONE    // represents 1
BigDecimal.TEN    // represents 10
```
*(All represented with scale = 0)*
 
### What Can BigDecimal Do?
 
- Represent **very large** and **very small** decimal numbers
- **Full control over rounding** behaviour during arithmetic
- **Convert** to different representations
- **Compare** BigDecimal values accurately
- Perfect for **financial/monetary calculations**
 
---
 
## BigInteger 🔢
 
### How BigInteger Works Internally
 
- Stores an arbitrary-precision **signed integer** in base 10
- Internally uses **two's-complement notation** (stored as an array of `int` values)
- Has methods **similar to BigDecimal** for arithmetic, conversions, and comparison
 
```java
BigInteger bigNum = new BigInteger("123456789012345678901234567890");
// This number is WAY too large for a regular int or long!
```
 
---
 
### BigDecimal vs BigInteger — Quick Comparison
 
| Feature | BigDecimal | BigInteger |
|---|---|---|
| Type of number | Decimal (with fractional part) | Whole numbers only |
| Use case | Money, scientific precision | Huge integers, cryptography |
| Precision | Arbitrary (unlimited digits) | Arbitrary (unlimited digits) |
| Immutable? | ✅ Yes | ✅ Yes |
| Package | `java.math` | `java.math` |
 
---
 
### BigDecimal vs double — Why Use BigDecimal?
 
```java
// ❌ double — loses precision!
double a = 0.1 + 0.2;
System.out.println(a); // 0.30000000000000004 😱
 
// ✅ BigDecimal — exact result!
BigDecimal x = new BigDecimal("0.1");
BigDecimal y = new BigDecimal("0.2");
System.out.println(x.add(y)); // 0.3 ✅
```
 
> ⚠️ Always use **String constructor** (`new BigDecimal("0.1")`) not double constructor (`new BigDecimal(0.1)`) to avoid precision issues!
 
---
 
## Quick Reference Summary 📋
 
| Concept | Key Point |
|---|---|
| **Random class** | Generates pseudorandom numbers using math formulas |
| **Pseudorandom** | Looks random but is actually calculated |
| **Seed** | Starting value — same seed = same sequence |
| **`nextInt(n)`** | Random int from 0 to n-1 |
| **Dice roll formula** | `nextInt(6) + 1` gives 1–6 |
| **BigDecimal** | High-precision decimal numbers (great for money) |
| **BigInteger** | High-precision whole numbers (no size limit) |
| **Scale** | Number of digits after decimal point in BigDecimal |
| **Both are immutable** | Cannot be changed after creation |
 
---

[← Previous Chapter](chapter-7-exception-handling.md) | [Back to Index](../README.md)
