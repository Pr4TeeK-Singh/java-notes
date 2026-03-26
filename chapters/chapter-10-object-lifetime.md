# Chapter 10 — Object Lifetime

> This chapter covers how Java manages memory automatically, when objects are created and destroyed, and how to initialize object state properly.

---

## 10.1 Garbage Collection

### What is it?
When you create objects in Java using `new`, they are stored in a part of memory called the **heap**. Over time, objects that are no longer needed take up space. **Garbage Collection (GC)** is Java's way of automatically **freeing that memory**.

### Key Points

- Java handles memory management **automatically** — you don't need to delete objects manually.
- The **garbage collector** runs in the background and reclaims memory from objects no longer in use.
- This prevents **dangling references** (a reference pointing to an object that no longer exists).
- ⚠️ GC runs as a background task — **you can't control exactly when it runs**.
- Time-critical apps should be aware that GC may slightly impact performance.

### Simple Analogy
> Think of GC like a cleaner who comes in and throws away trash (unused objects) when needed — you don't have to do it yourself.

---

## 10.2 Reachable Objects

### What is a Reachable Object?
An object is **reachable** if there is **at least one reference** pointing to it from an active part of the program (like a local variable in a running method on the JVM stack).

### Reachability Chain

```
JVM Stack (active threads)
    └── local reference → Object A (reachable ✅)
                              └── field reference → Object B (also reachable ✅)

No reference → Object C (unreachable ❌ → eligible for GC)
```

### Key Terms

| Term | Meaning |
|---|---|
| **Reachable** | Object has at least one active reference pointing to it |
| **Reachable reference** | A reference that makes an object reachable |
| **Unreachable** | No active reference → object is "lost" |
| **Alive** | Reachable and accessible by a live thread |
| **Eligible for GC** | Unreachable — memory can be reclaimed |

### Important Rules

- An object can be referenced by **more than one** reference (aliases).
- If a **compound object** (an object containing other objects) becomes unreachable → all its inner objects become unreachable too.
- A **circular reference** (A → B → A) does NOT keep objects alive if there's no outside reference to them — they are still eligible for GC.

### Example

```java
// Object is reachable
String s = new String("hello");   // 's' points to the object ✅

// Object becomes unreachable
s = null;                         // no reference anymore ❌ → eligible for GC
```

---

## 10.3 Facilitating Garbage Collection

### The Big Idea
Java's GC does the heavy lifting, but **you can help** by writing code that releases objects as soon as they're no longer needed.

### Good Practices

#### 1. Null out references when done
```java
long[] bigArray = new long[20000];
// ... use bigArray ...
bigArray = null;   // ✅ Now eligible for GC — don't hold on unnecessarily
```

#### 2. Close resources properly
Objects like **files**, **database connections**, and **network connections** should be closed when done. Use **try-with-resources** — it always closes the resource automatically:

```java
try (FileReader reader = new FileReader("file.txt")) {
    // use reader
}   // reader is automatically closed here ✅
```

#### 3. Don't hold references longer than needed
- Local variables in a method are automatically cleaned up when the method finishes.
- If you return, pass, or store a reference somewhere, the object stays alive longer.

### Key Points

| Tip | Why |
|---|---|
| Set references to `null` when done | Helps GC reclaim memory sooner |
| Use try-with-resources for files/connections | Ensures proper cleanup |
| Don't store unnecessary references | Reduces memory usage |
| Don't rely on GC for cleanup timing | GC runs on its own schedule |

---


## 10.4 Initializers

### What Are Initializers?
**Initializers** are used to set **initial values** for fields when an object or class is first created.

There are **3 kinds** of initializers in Java:

| Type | Used For |
|---|---|
| **Field initializer expressions** | Set a field's value directly in its declaration |
| **Static initializer blocks** | Initialize `static` fields — runs once when class is loaded |
| **Instance initializer blocks** | Initialize instance fields — runs every time an object is created |

### Quick Preview

```java
class Example {
    int x = 10;                    // Field initializer expression

    static int count;
    static {                       // Static initializer block
        count = 0;
    }

    String name;
    {                              // Instance initializer block
        name = "default";
    }
}
```

> Details on each type are covered in sections 10.6 onwards.

---

## 10.5 Field Initializer Expressions

### What Are They?
A **field initializer expression** lets you give a field its starting value **right where it is declared**.

```java
class Car {
    int speed = 0;           // ✅ Field initializer
    String brand = "Toyota"; // ✅ Field initializer
    double price = 9.99 * 2; // ✅ Can use expressions too
}
```

### Rules

| Rule | Detail |
|---|---|
| Where? | Right next to the field declaration |
| When does it run? | When the object is created (for instance fields) / when class loads (for static fields) |
| Can use expressions? | ✅ Yes — any valid expression |
| Can call methods? | ✅ Yes |
| Can refer to other fields? | ✅ Yes, but only fields declared **before** it |

### Static vs Instance Field Initializers

```java
class Counter {
    static int total = 0;       // Static field initializer — runs once
    int id = ++total;           // Instance field initializer — runs per object
}

// Counter c1 → id = 1, total = 1
// Counter c2 → id = 2, total = 2
```

---

## 10.6.1 — Exception Handling and Initializer Expressions

### The Rule — Simple Version
> Field initializer expressions **must not throw a checked exception** that goes uncaught.

If a method called inside a field initializer can throw a **checked exception**, that exception must be **caught and handled inside the method itself** — you cannot let it bubble up from the initializer.

### Why?
The initializer expression has **no way to declare a `throws` clause** — so the compiler will complain if a checked exception could escape.

### Checked vs Unchecked in Initializers

| Exception Type | In Field Initializer | In Instance Initializer (anonymous class) |
|---|---|---|
| Checked exception | ❌ Must be caught inside | ✅ Allowed (more freedom) |
| Unchecked exception | ✅ Can propagate | ✅ Can propagate |

### Example

```java
// ✅ CORRECT — checked exception caught inside the method
static HotelPool pool = createHotelPool();  // field initializer calls method

static HotelPool createHotelPool() {
    try {
        // ... code that throws TooManyHotelsException (checked)
    } catch (TooManyHotelsException e) {
        // handled here ✅
    }
    return pool;
}

// ❌ WRONG — if createHotelPool() used 'throws' instead of try-catch,
// the field initializer would need to handle it — but it can't!
```

---

> 📌 *Section 10.7 (Static Initializer Blocks) — notes coming soon.*

---

## 10.8 Instance Initializer Blocks

### What Are They?
An **instance initializer block** is a block of code `{ }` written directly inside a class (not inside any method or constructor). It runs **every time a new object is created**.

Think of it as extra setup code that runs automatically before or alongside a constructor.

### Syntax

```java
class InstanceInitializers {
    long[] squares = new long[10];    // (1) Field initializer

    // (2) Instance Initializer Block
    {
        for (int i = 0; i < squares.length; i++)
            squares[i] = i * i;       // fills array with 0,1,4,9,16...
    }
}
```

### Key Rules

| Rule | Detail |
|---|---|
| When does it run? | Every time an object of the class is created |
| Where is it? | Directly in the class body — NOT inside a method |
| Can a class have multiple? | ✅ Yes — executed top to bottom in order |
| Order of execution | Field initializers + instance blocks run in the order declared |
| Purpose | Complex initialization that can't fit in a single expression |

### When to Use?

- When you need **loops or complex logic** to set up a field (can't do this with a simple `=` expression)
- For **anonymous classes** (which can't have constructors) — instance initializer blocks are the only way to do constructor-like setup

---

## 10.8.1 — Exception Handling in Instance Initializer Blocks

### How It Differs from Field Initializer Expressions

| Feature | Field Initializer Expression | Instance Initializer Block |
|---|---|---|
| Checked exceptions allowed? | ❌ Must be caught inside | ✅ Can propagate — but every constructor must declare it in `throws` |
| Unchecked exceptions | ✅ Can propagate | ✅ Can propagate |
| In anonymous classes | — | ✅ Even more freedom — no constructors involved |

### The Rule in Simple Words
- In an **instance initializer block**, a checked exception CAN be thrown — but then **every constructor** in the class must also declare that exception in its `throws` clause.
- In a **static initializer block**, an uncaught checked exception will cause a compile error.
- In **anonymous classes**, instance initializer blocks have the most freedom since there are no constructors.

### Example

```java
class MyClass {
    {
        // Instance initializer block that throws checked exception
        if (someCondition)
            throw new SomeCheckedException();  // ✅ allowed IF...
    }

    MyClass() throws SomeCheckedException {    // ...every constructor declares it
    }
}
```

---

## Chapter 10 — Quick Summary

| Section | Topic | Key Idea |
|---|---|---|
| 10.1 | Garbage Collection | Java auto-manages memory; GC reclaims unused objects |
| 10.2 | Reachable Objects | Object is alive if reachable; unreachable = eligible for GC |
| 10.3 | Facilitating GC | Null references, use try-with-resources, release early |
| 10.4 | Initializers | 3 types: field expression, static block, instance block |
| 10.5 | Field Initializer Expressions | Set field values inline at declaration |
| 10.6.1 | Exception Handling in Field Initializers | Checked exceptions must be caught inside — can't escape initializer |
| 10.7 | Static Initializer Blocks | *(notes coming soon)* |
| 10.8 | Instance Initializer Blocks | Runs on every object creation; useful for complex/loop-based setup |
| 10.8.1 | Exception Handling in Instance Blocks | Checked exceptions allowed if every constructor declares `throws` |

---

[← Previous Chapter](chapter-9-nested-type-declarations.md) | [Back to Index](../README.md)
