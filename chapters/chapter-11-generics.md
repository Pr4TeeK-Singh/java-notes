# Chapter 11 — Generics

> Generics allow you to write code that works with **any type** while still being **type-safe** — the compiler checks types for you so errors are caught early, not at runtime.

---

## 11.1 Introducing Generics

### What Are Generics?
Generics let you write a **single class, interface, or method** that can work with **different types** — without duplicating code.

Think of it like a **template** — you define the structure once, and plug in the actual type when you use it.

### Real-World Analogy
> A box can hold anything — but a **typed box** (`Box<Books>`) only holds books. You don't need to check what's inside — you already know!

### Why Use Generics?

| Benefit | Explanation |
|---|---|
| **Type-safety** | Compiler catches type errors at compile time, not runtime |
| **No explicit casting** | You don't need to cast objects manually |
| **Less code** | One generic class replaces many type-specific classes |
| **Better readability** | Code is clearer about what types are expected |

### How Generics Work Under the Hood
- Generic type declarations are **compiled once** into a single `.class` file
- The compiler **inserts casts automatically** where needed
- At runtime, the JVM has **no knowledge of generic types** (called *type erasure*)
- No extra class files are created per type usage

### Backward Compatibility
- Generics were designed to be **compatible with old (non-generic) Java code**
- You can still use generic classes without type parameters (called *raw types*), but you lose type-safety

---

## 11.2 Generic Types and Parameterized Types

### Part A — Generic Types

#### What is a Generic Type?
A **generic type** is a class or interface that has **one or more type parameters** (placeholders for actual types).

```java
class Node<E> {       // E is the type parameter (placeholder)
    // ...
}
```

Here, `E` is just a **placeholder** — it doesn't specify a real type yet. You define what `E` is when you actually use the class.

#### Type Parameter Naming Convention

| Letter | Typically Used For |
|---|---|
| `E` | Element (used in collections) |
| `T` | General type |
| `K` | Key (in maps) |
| `V` | Value (in maps) |
| `T1, T2, ...` | Multiple type parameters |

#### Multiple Type Parameters
```java
class Pair<T1, T2> {   // Two type parameters
    T1 first;
    T2 second;
}
```

#### Where Can Type Parameters Be Used?
Inside the class, `E` can be used just like any normal type:

```java
class Node<E> {
    private E data;           // (1) field type
    
    public Node(E data) { }   // constructor parameter
    
    public E getData() {      // (2) return type
        return data;
    }
    
    public void setData(E d) { // (3) method parameter
        this.data = d;
    }
}
```

> ⚠️ The type parameter is **NOT used** after the class name in the constructor declaration — `Node(E data)` not `Node<E>(E data)`

---

### Part B — Parameterized Types

#### What is a Parameterized Type?
A **parameterized type** is when you take a generic type and **supply an actual type** for it.

```java
// Generic type:       Node<E>
// Parameterized type: Node<Integer>
Node<Integer> intNode = new Node<Integer>(2020, null);
```

Think of it like calling a method — you pass in the actual argument (`Integer`) for the parameter (`E`).

#### Key Points

| Feature | Detail |
|---|---|
| Actual type replaces `E` | `Node<Integer>` → every `E` becomes `Integer` |
| Compiler treats it as a new type | `Node<Integer>` is treated like a brand new class |
| Type-checked at compile time | Wrong types cause compile errors, not runtime errors |
| No cast needed | Compiler inserts casts automatically |

#### Example

```java
Node<Integer> intNode = new Node<>(2020, null);  // ✅ diamond operator <> infers type

Integer value = intNode.getData();  // ✅ No cast needed — compiler knows it's Integer

intNode.setData(2020);              // ✅ OK
intNode.setData("TwentyTwenty");    // ❌ Compile-time error! String ≠ Integer
intNode.setNext(new Node<String>("Hi", null)); // ❌ Compile-time error! String ≠ Integer
```

#### Diamond Operator `<>`
Instead of repeating the type, use `<>` and let the compiler infer it:
```java
Node<Integer> n = new Node<>(42, null);   // ✅ Shorter — compiler infers <Integer>
```

---

### Part C — Generic Interfaces

#### What Are They?
Just like generic classes, **interfaces can also be generic** — declared the same way with type parameters.

```java
interface IMonoLink<E> {
    void setData(E data);
    E getData();
    void setNext(IMonoLink<E> next);
    IMonoLink<E> getNext();
}
```

#### Implementing a Generic Interface

**Option 1 — Generic class implements generic interface (keeps `E`)**
```java
class MonoNode<E> implements IMonoLink<E> {
    private E data;
    private IMonoLink<E> next;
    // implement all methods...
}

// Usage:
IMonoLink<String> strNode = new MonoNode<>("Bye", null);
System.out.println(strNode.getData());   // Prints: Bye
```

**Option 2 — Non-generic class implements generic interface (fixes the type)**
```java
class LymphNode implements IMonoLink<Lymph> {  // Lymph is a concrete type
    private Lymph body;
    // implement all methods with Lymph instead of E
}
```

#### Rules for Generic Interfaces

| Rule | Detail |
|---|---|
| Can be parameterized like a class | ✅ `IMonoLink<String>` |
| Cannot be instantiated directly | ❌ `new IMonoLink<>()` → compile error |
| Implementing class can fix type | ✅ `class X implements IMonoLink<Lymph>` |
| Real-world examples | `Comparable<E>`, `Comparator<E>`, `Collection<E>`, `List<E>`, `Map<K,V>` |

---

## Chapter 11 — Quick Summary

| Section | Topic | Key Idea |
|---|---|---|
| 11.1 | Introducing Generics | Write once, use with any type; type-safe + no casting needed |
| 11.2A | Generic Types | Class/interface with type parameter `<E>` as a placeholder |
| 11.2B | Parameterized Types | Supply actual type: `Node<Integer>` — compiler enforces it |
| 11.2C | Generic Interfaces | Interfaces can also have type parameters; implemented generically or concretely |

---

*Back to [README](../README.md)*
