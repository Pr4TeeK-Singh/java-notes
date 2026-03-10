# Chapter 6 — Access Control

[← Back to Index](../README.md)

---

## 6.1 Design Principle: Encapsulation

**Encapsulation** means hiding the internal details of an object and only showing what is necessary to the outside world.

> **Example:** Think about a TV remote. You press buttons to control the TV, but you don't need to know how the internal electronics work.

### Key Ideas

Every object has two parts:

| Part | Description |
|------|-------------|
| **Contract** | What the object *can do* (its public interface) |
| **Implementation** | *How* the object does it internally |

- The internal implementation can **change** without affecting the users of that object.
- Encapsulation **reduces complexity** because users only see what they need to see.

### How Encapsulation is Achieved in Java

#### 1. Method / Block Level
Variables declared inside a method or block can **only be used inside that block**.  
These are called **local variables**.

#### 2. Reference Type Level (Class / Interface)
Access modifiers like `public` and `private` control who can access class members.

```java
class BankAccount {
    private double balance;         // hidden from outside

    public double getBalance() {    // controlled access
        return balance;
    }
}
```

> **Common Practice:** Make fields `private` and provide `public` methods to access them. This protects data from direct modification.

#### 3. Package Level
Classes that are related can be grouped into **packages** using the `package` keyword.

```java
package com.company.project;
```

- Use `import` to use classes from another package.
- Access can be controlled using `public` or **default** (package-private) access.

#### 4. Module Level
Multiple packages can be grouped into a **module**.  
Modules control which packages are visible to other modules.

---

## 6.2 Java Source File Structure

A Java source file follows this order:

```java
// 1. Package declaration (optional, only ONE allowed)
package com.company.project;

// 2. Import statements (zero or more)
import java.io.*;
import java.util.*;

// 3. Type declarations (classes, interfaces, enums, records)
public class NewApp { }

class A { }

interface IX { }

record B() { }

enum C { }
```

### Important Rules

| Rule | Detail |
|------|--------|
| Multiple types | A source file **can** contain multiple type declarations |
| Public class | Only **one** class can be `public` in a file |
| File name | If a class is `public`, the **file name must match** the class name |
| Package membership | All types in a file belong to the **same package** |
| Scope | Everything except `package` and `import` must be inside a class/interface/enum/record |

> **Example:** If class name is `NewApp`, the file **must** be named `NewApp.java`.

---

## 6.3 Packages

A **package** is a group of related classes, interfaces, enums, and records.  
It is similar to **folders** in a computer where related files are stored together.

### Package Naming

Packages use **dot notation**.

```
wizard.pandorasbox.LovePotion
```

**Common practice:** Use **reverse domain names** to make package names unique worldwide.

```
// If company owns: sorcerersltd.com
// Package name:
com.sorcerersltd.wizard
```

### Rules for Package Names

| Rule | Detail |
|------|--------|
| Valid identifiers | Each part of the name must be a valid Java identifier |
| Hyphens | **Not allowed** |
| Underscores | **Allowed** |

```java
// Invalid ❌
org.covid-19.2022.vaccine       // hyphen not allowed

// Valid ✅
org.covid_19_2022.vaccine       // underscore is fine
```

### Defining a Package

```java
package fully_qualified_package_name;
```

- Only **one** `package` statement is allowed in a file.
- If no package is declared → the class goes into the **default unnamed package**.

### Subpackages

- Subpackages are simply **subfolders** used for organization.

```
pandorasbox.artifacts           // subpackage of pandorasbox
```

- Subpackages do **not** have a special relationship with their parent package — they are mainly used to **organize code**.

---

## Access Modifiers — Quick Reference

| Modifier | Class | Package | Subclass | World |
|----------|-------|---------|----------|-------|
| `public` | ✅ | ✅ | ✅ | ✅ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| *(default)* | ✅ | ✅ | ❌ | ❌ |
| `private` | ✅ | ❌ | ❌ | ❌ |

---

## 6.5 Access Modifiers

Access modifiers (also called **visibility modifiers**) control who can see and use a class or its members.

### Access Modifiers for Top-Level Types

A **top-level type** is a class, interface, enum, or record that is NOT declared inside another type.

| Modifier | Who can access it? |
|----------|--------------------|
| `public` | Everyone — from any package |
| *(no modifier)* | Only classes in the **same package** (package-private / default access) |

```java
// File: Clown.java
package wizard.pandorasbox;

import wizard.pandorasbox.artifacts.Ailment;  // importing from another package

public class Clown implements Magic {
    LovePotion tlc;      // same package — simple name works
    Ailment problem;     // imported — simple name works

    public static void main(String[] args) {
        Clown joker = new Clown();
        joker.levitate();
        joker.mixPotion();
        joker.healAilment();
    }
}
```

> **Tip:** To use a class from another package, either use its **fully qualified name** (`wizard.pandorasbox.Clown`) or **import** it so you can use the simple name.

---

### Access Modifiers for Class Members

By putting access modifiers on fields and methods, a class controls exactly which services it offers to clients (other classes).

The 4 access levels for members are: `public`, `protected`, package (no modifier), `private`.

---

### `public` Members

- **Most open** — accessible from **anywhere**.
- Any class in any package can use a `public` member.
- Subclasses can access their inherited `public` members by simple name.

```java
// pubInt is public — all 4 clients (same/different package, sub/non-sub) can access it
out.println(obj1.pubInt);   // OK from anywhere
```

---

### `protected` Members

- Accessible to all classes in the **same package** AND to **subclasses** in any package.
- More restrictive than `public` but more open than package access.

> ⚠️ **Tricky rule:** A subclass in a **different package** can only access a `protected` member through a reference of its **own type** — not through a superclass reference.

```java
// Client 3 (subclass in pkg2):
Superclass1 obj1 = new Superclass1();
out.println(obj1.proStr);   // ❌ Compile-time error! (superclass reference)

Subclass2 obj2 = new Subclass2();
out.println(obj2.proStr);   // ✅ OK! (own type reference — inherited member)
```

---

### Members with Package Access (no modifier)

- Accessible **only within the same package**.
- Even if the class is visible in another package, its package-access members are **not**.
- More restrictive than `protected`.

```java
// pkgBool has package access — only Client 1 and Client 2 (same package) can use it
// Client 3 and Client 4 (different package) CANNOT access pkgBool
```

---

### `private` Members

- **Most restrictive** — accessible **only inside the defining class** itself.
- Not accessible by subclasses, not inherited, not visible anywhere else.
- Standard practice: make all fields `private`, provide `public` getter methods.

```java
class BankAccount {
    private long privLong;       // only accessible inside this class

    public long getPrivLong() {  // controlled access via public method
        return privLong;
    }
}
```

> **Design tip:** Auxiliary helper methods are often declared `private` — they are internal tools, not part of the public contract.

---

### Summary Table

| Member | Same Class | Same Package (non-sub) | Same Package (subclass) | Other Package (subclass) | Other Package (non-sub) |
|--------|-----------|----------------------|------------------------|--------------------------|------------------------|
| `public` | ✅ | ✅ | ✅ | ✅ | ✅ |
| `protected` | ✅ | ✅ | ✅ | ✅ (own type only) | ❌ |
| *(default)* | ✅ | ✅ | ✅ | ❌ | ❌ |
| `private` | ✅ | ❌ | ❌ | ❌ | ❌ |

---

## 6.6 Scope Rules

**Scope** = the part of the program where a name (variable/method) can be used.

Java has two main scope types:

| Scope | What it controls |
|-------|-----------------|
| **Class scope** | How members (fields, methods) are accessed within a class |
| **Block scope** | How local variables are accessed within a block `{ }` |

### Class Scope for Members

Class scope means: code **inside** a class can access **all members** of that class (and inherited ones) directly by name — no object reference needed.

```java
class SuperClass {
    int instanceVarInSuper;
    static int staticVarInSuper;
}
```

- **Static code** (static methods, static blocks) can access: static members of the class and its superclasses.
- **Non-static code** (instance methods, constructors) can access: ALL members — both static and instance.

> **Key point:** Inside a class, you can use member names directly. Outside the class, you need a reference (`object.field`) or class name (`ClassName.staticField`).

---

## 6.7 Implementing Immutability

### What is an Immutable Object?

An **immutable object** is one whose **state cannot be changed** after it is created.

- Since no one can change it, there are no conflicts between threads — **thread safety comes for free**.
- The state can only be read, never modified → always consistent.

> **Examples of immutable classes in Java:**

| Class | Description |
|-------|-------------|
| `java.lang.String` | Strings are always immutable |
| `Boolean`, `Integer`, `Double`, etc. | Wrapper classes — wrap a primitive value |
| `LocalDate`, `LocalTime`, `LocalDateTime` | Date/time values |
| `java.time.format.DateTimeFormatter` | Date formatting |
| `java.util.Locale` | Geographical/cultural region |
| `java.nio.file.Path` | File path |

---

### Guidelines for Making an Immutable Class

Follow these rules when writing your own immutable class:

#### 1. Make the class `final` — so no one can extend it

```java
public final class WeeklyStats { ... }
```

Or alternatively, use a **private constructor** + a **static factory method** to prevent subclassing without making the class `final`.

#### 2. Make all fields `private` and `final`

```java
private final String description;   // immutable string value
private final int weekNumber;       // immutable primitive
private final int[] stats;          // reference is fixed (but array contents need care!)
```

#### 3. No setter methods

- Don't provide any methods that change field values.
- Only provide **getter** (accessor) methods.

#### 4. Defensive copying for mutable fields

If a field is a reference to a **mutable object** (like an array or list), make a private copy — don't store the original reference passed in.

```java
// Constructor — make a copy of the array, don't store the original
public WeeklyStats(String desc, int weekNum, int[] stats) {
    this.description = desc;
    this.weekNumber = weekNum;
    this.stats = Arrays.copyOf(stats, stats.length);  // defensive copy
}

// Getter — return a copy, not the original
public int[] getWeeklyStats() {
    return Arrays.copyOf(stats, stats.length);  // defensive copy
}
```

> This technique is called **defensive copying** — it stops clients from secretly modifying your object's internal data.

#### 5. Check consistency in the constructor

- Validate all inputs when the object is created.
- Throw an exception if the inputs would create an invalid state.

```java
if (weekNumber <= 0 || weekNumber > 52)
    throw new IllegalArgumentException("Invalid week number: " + weekNumber);
if (stats.length != 7)
    throw new IllegalArgumentException("Stats not for whole week: " + Arrays.toString(stats));
```

---

### Immutability Summary

| Rule | Why it matters |
|------|---------------|
| `final` class | Prevents subclasses from breaking immutability |
| `private final` fields | Values can't be changed from inside or outside |
| No setters | No way to modify state after creation |
| Defensive copy in constructor | Client can't modify your data through the array they passed |
| Defensive copy in getter | Client can't modify your data through the array they received |
| Validate in constructor | Object is always in a valid, consistent state |

---

[← Previous Chapter](chapter-5-oops.md) | [Back to Index](../README.md)
