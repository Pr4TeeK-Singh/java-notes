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

[← Previous Chapter](chapter-5-oops.md) | [Back to Index](../README.md)
