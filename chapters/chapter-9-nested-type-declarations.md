# Chapter 9 — Nested Type Declarations

> A **nested type** is a type (class, interface, enum, or record) declared **inside** another type declaration.

---

## 9.1 Overview of Nested Type Declarations

A **type declaration** lets you define a new reference type. There are two kinds:

- **Top-level type declaration** — defined *outside* any other type (normal classes, interfaces, enums, record classes).
- **Nested type declaration** — defined *inside* another type declaration.

### Type Declaration Tree

```
Reference Type Declarations
│
├── Top-level Types
│   ├── Class
│   ├── Interface
│   ├── Enum type
│   └── Record class
│
└── Nested Types
    ├── Static Member Types
    │   ├── Class
    │   ├── Interface
    │   ├── Enum type
    │   └── Record class
    │
    ├── Inner Classes (Non-static)
    │   ├── Non-static member class
    │   ├── Anonymous class
    │   └── Local class
    │
    └── Static Local Types
        ├── Interface
        ├── Enum type
        └── Record class
```

### Three Categories of Nested Types

| Category | Also Known As |
|---|---|
| Static member types | Static nested types |
| Inner classes | Non-static nested classes |
| Static local types | Static local nested types |

---

## 9.2 Static Member Types

### What Are They?

A **static member type** is a type declared inside another type using the `static` keyword. It behaves just like a top-level type — it does **not** need an instance of the enclosing class to exist.

There are **four kinds** of static member types:
- Static member class (a.k.a. *static nested class*)
- Static member interface (a.k.a. *nested interface*)
- Static member enum type
- Static member record class

### Key Rules

| Rule | Detail |
|---|---|
| Keyword | Use `static` when declaring inside a class |
| Enum / Record / Interface inside a class | Implicitly `static` — keyword can be omitted |
| Access levels | `public`, `protected`, package, `private` all allowed |
| Inside an interface | Implicitly `public` and `static` — cannot use `private` |

### Example

```java
// File: ListPool.java
package smc;

public class ListPool {                           // Top-level class

    public static class MyLinkedList {            // Static member class

        private interface ILink { }               // Static member interface (implicitly static)

        public static class BiNode
            implements IBiLink {                  // Static member class
        }
    }
}
```

### Why Use Static Member Types?
- No need for an outer object — use them independently.
- Logically group helper types inside the class they belong to.
- Cleaner code organization compared to separate top-level types.

---

## 9.3 Non-Static Member Classes (Inner Classes)

### What Are They?

A **non-static member class** (also called an **inner class**) is declared inside another type **without** the `static` keyword. It is an *instance member* — every inner class object is **tied to** an instance of the enclosing class.

### Key Rules

| Rule | Detail |
|---|---|
| Keyword | No `static` keyword |
| Needs outer instance? | ✅ Yes — cannot exist without enclosing object |
| Can access outer fields? | ✅ Yes — including `private` ones |
| Declared in an interface? | ❌ Not allowed (interface members are implicitly static) |
| Access levels | `public`, `protected`, package, `private` all allowed |

### Typical Use Case

A classic example is a **linked list** with a `Node` inner class:
- `Node` is `private` → hidden from outside
- `Node` objects can't exist without a list object → makes logical sense
- Inner class can access the list's private fields directly

### Example

```java
class MyLinkedList {
    private String message = "Shine the light";

    public Node makeNode(String info, Node next) {
        return new Node(info, next);
    }

    public class Node {                          // Non-static member class
        static int maxNumOfNodes = 100;          // Static field (allowed in Java 16+)
        // ...
    }
}
```

### Instantiating an Inner Class

```java
MyLinkedList list = new MyLinkedList();
MyLinkedList.Node node = list.new Node();   // Needs an enclosing instance!
```

---

## 9.4 Local Classes

### What Are They?

A **local class** is an inner class defined **inside a block** — visible and usable only within that block.

Blocks where a local class can be declared:
- Method body
- Constructor body
- Initializer block
- Loop body (`for`, `while`)
- `if-else` statement
- `try-catch-finally` block

### Key Rules

| Rule | Detail |
|---|---|
| Access modifier | ❌ Cannot have `public`, `private`, etc. |
| `abstract` / `final` | ✅ Allowed |
| Must be declared before use | ✅ Yes — like a local variable |
| Members & constructors | Can have any access level (but only visible inside the block anyway) |
| `static` keyword on the class | ❌ Not allowed |

### Static vs Non-Static Context

| Declared In | Behavior |
|---|---|
| Non-static method / block | Has a `this` reference → like a non-static member class |
| Static method / static block | Implicitly static → no outer instance needed |

### Lifecycle

> Like a local variable — the local class only lives during the execution of the block. Once the method returns, the object is gone (unless a reference was returned or stored elsewhere).

### Example

```java
class Top {
    static void staticMethod() {           // (1) Static context

        // ❌ Can't use StaticLocal here — not defined yet!

        class StaticLocal {                // (5) Local class
            void greet() {
                System.out.println("Hello from local class!");
            }
        }

        StaticLocal obj = new StaticLocal(); // ✅ Valid — defined before use
        obj.greet();
    }
}
```

---

## 9.5 Static Local Types

### What Are They?

You can declare **local interfaces**, **local enum types**, and **local record classes** inside a block. These are called **static local types**.

> ⚠️ These are **not the same** as a local class declared in a static context. Static local types are *always* implicitly `static`.

### Key Points

- Local **interfaces**, **enums**, and **records** declared inside a block → implicitly `static`
- Local **classes** are never `static` (unless declared inside a static method — then they behave like static, but are not truly static local types)

### What "implicitly static" means for these types:

| Implication | Detail |
|---|---|
| No outer instance needed | Can be instantiated with `new` directly (for record classes) |
| Can only access `static` members of enclosing class | ❌ Cannot access non-static (instance) fields |
| Interfaces and enums | Cannot be instantiated with `new` anyway |

### Example

```java
class Outer {
    static int count = 10;       // static field
    int value = 99;              // non-static field

    void method() {
        record Point(int x, int y) { }  // Static local record class

        Point p = new Point(1, 2);      // ✅ No outer instance needed
        System.out.println(count);      // ✅ Can access static field
        // System.out.println(value);   // ❌ Cannot access non-static field
    }
}
```

---

## 9.6 Anonymous Classes

### What Are They?

An **anonymous class** combines **defining** and **instantiating** a class into a **single step** — with no name!

- Defined right where they are used (inline)
- No name → can only create **one instance** at the point of definition
- Like local classes — can be used in both static and non-static contexts

### Key Rules

| Rule | Detail |
|---|---|
| Has a name? | ❌ No — that's why it's "anonymous" |
| Access modifier | ❌ Not allowed |
| `static` / `final` / `abstract` | ❌ Not allowed |
| Can extend a class? | ✅ Yes |
| Can implement an interface? | ✅ Yes |
| Constructors | ❌ Cannot define (no name!) — use instance initializer instead |

### When to Use Anonymous Classes?

Typically used for **one-off, short-lived** implementations:
- Event listeners in GUI apps
- Simple thread tasks (`Runnable`)
- Comparators for sorting objects

### Syntax — Extending a Class

```java
new SuperclassName(optional_constructor_args) {
    // member declarations — override methods here
};
```

### Syntax — Implementing an Interface

```java
new InterfaceName() {
    // must implement all abstract methods
};
```

### Example

```java
// Instead of creating a whole new class, do it inline:
Runnable r = new Runnable() {
    @Override
    public void run() {
        System.out.println("Running anonymously!");
    }
};
r.run();
```

### Important Notes

- No `extends` keyword — the superclass/interface name is written after `new`
- If extending an **abstract class** → must implement all abstract methods
- The statement ends with a **semicolon (`;`)** after the closing brace `}`
- Members of the anonymous class are only accessible via the object reference, which must be of a supertype

---

## Summary — All Nested Type Categories

| Type | `static` keyword | Needs Outer Instance | Where Defined | Access Modifier |
|---|---|---|---|---|
| Static Member Class | ✅ Yes | ❌ No | Inside a class/interface | Any |
| Non-Static Member Class (Inner) | ❌ No | ✅ Yes | Inside a class | Any |
| Local Class | ❌ Not allowed | Depends on context | Inside a block | ❌ None |
| Static Local Type (interface/enum/record) | Implicitly ✅ | ❌ No | Inside a block | ❌ None |
| Anonymous Class | ❌ Not allowed | Depends on context | Inline expression | ❌ None |

---

## Static vs Non-Static Member Class — Quick Comparison

| Feature | Static Member Class | Non-Static (Inner) Class |
|---|---|---|
| Needs enclosing instance? | ❌ No | ✅ Yes |
| Can access outer instance fields? | ❌ No | ✅ Yes |
| Declared with? | `static` keyword | No keyword |
| Instantiation | `new Outer.Inner()` | `outerObj.new Inner()` |
| Good for | Independent helpers | Data structures, iterators |

---

*Back to [README](../README.md)*
