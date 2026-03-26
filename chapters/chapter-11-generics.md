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

### Generic Types

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

### Parameterized Types

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

### Generic Interfaces

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

## 11.3 Collections and Generics

### The Problem — Before Generics
Before Java 1.5, collections like `ArrayList` could hold **any object** — no type-checking at all. This caused problems:

```java
// OLD WAY — no generics
List wordList = new ArrayList();        // Can add ANYTHING
wordList.add("two zero two zero");      // ✅ String added
wordList.add(2020);                     // ✅ Integer added — no error!

Object element = wordList.get(0);      // Returns just "Object" — you don't know the type
if (element instanceof String str) {   // Had to do runtime type check to avoid crash
    // use str
}
```

Problems:
- ❌ No compile-time type checking
- ❌ Required manual casting and `instanceof` checks
- ❌ Easy to accidentally put the wrong type in

### The Fix — With Generics

```java
// NEW WAY — with generics
List<String> wordList = new ArrayList<>();  // Only Strings allowed!
wordList.add("two zero two zero");          // ✅ OK
wordList.add(2020);                         // ❌ Compile-time error — caught early!

String element = wordList.get(0);          // Always returns a String — no cast needed!
```

### Before vs After Comparison

| Feature | Without Generics | With Generics |
|---|---|---|
| Type checking | ❌ Runtime only | ✅ Compile time |
| Casting needed? | ✅ Yes — manual | ❌ No — compiler handles it |
| Wrong type caught? | ❌ Only at runtime (crash!) | ✅ At compile time |
| Code clarity | ❌ Harder to read | ✅ Clear what type is expected |

> 💡 **Key Idea:** With generics, `ArrayList<String>` is still a generic implementation under the hood, but its *usage* is specific — it can only hold Strings.

---

## 11.4 Wildcards

### The Subtype Problem with Parameterized Types

Here's a surprising thing about generics — **`Node<Integer>` is NOT a subtype of `Node<Number>`**, even though `Integer` IS a subtype of `Number`.

```java
Node<Integer> intNode    = new Node<>(2020, null);   // (1)
Node<Double>  doubleNode = new Node<>(3.14, null);   // (2)
Node<Number>  numNode    = new Node<>(2021, null);   // (3)

// These WORK — Integer and Double are subtypes of Number:
numNode.setData(10.5);   // ✅ (4) Double is a Number
numNode.setData(2022);   // ✅ (5) Integer is a Number

// These FAIL — parameterized types are NOT covariant:
numNode.setNext(intNode);                        // ❌ (6) Compile error!
numNode = new Node<Number>(2030, doubleNode);    // ❌ (7) Compile error!
numNode = intNode;                               // ❌ (8) Compile error!
numNode = doubleNode;                            // ❌ (9) Compile error!
```

### Why Does This Happen?

> `Node<Integer>` and `Node<Double>` are **NOT subtypes** of `Node<Number>` — they are completely separate types.

This is intentional! If `Node<Integer>` were a subtype of `Node<Number>`, you could accidentally store a `Double` into a `Node<Integer>` through a `Node<Number>` reference — breaking type safety.

### Arrays vs Generics — The Difference

| Feature | Arrays | Parameterized Types |
|---|---|---|
| `Integer[]` subtype of `Number[]`? | ✅ Yes (covariant) | ❌ No (invariant) |
| Type-safe? | ❌ Not always (runtime error possible) | ✅ Always (compile-time safe) |

### The Solution — Wildcards `?`

To handle this flexibility issue, Java provides **wildcards** using `?`:

| Wildcard | Meaning | Use When |
|---|---|---|
| `?` | Any type (unknown) | Read-only — you don't know/care what type |
| `? extends T` | Any subtype of T | Reading — upper bounded wildcard |
| `? super T` | Any supertype of T | Writing — lower bounded wildcard |

```java
// Without wildcard — too restrictive:
void printNode(Node<Number> n) { }   // Only accepts Node<Number>, NOT Node<Integer>!

// With wildcard — flexible:
void printNode(Node<? extends Number> n) { }  // ✅ Accepts Node<Integer>, Node<Double>, etc.
```

---

## 11.5 Implementing a Simplified Generic Stack

### What This Demonstrates
A **generic stack** (`MyStack<E>`) is a great real-world example of generics in action. It works with **any type** — `MyStack<Integer>`, `MyStack<String>`, etc.

### Generic Stack Interface

```java
public interface IStack<E> extends Iterable<E> {
    void push(E element);         // Add element to top of stack
    E pop();                      // Remove and return top element
    E peek();                     // Look at top element without removing
    int size();                   // Number of elements
    boolean isEmpty();            // Is the stack empty?
    boolean isMember(E element);  // Is element in the stack?
    E[] toArray(E[] toArray);     // Copy stack to array
    String toString();            // Text representation: (e1, e2, ..., en)
}
```

### Key Points
- `MyStack<E>` implements `IStack<E>` and uses `Node<E>` internally
- `NodeIterator<E>` provides iteration so you can use the **enhanced for loop** `for(:)`
- The stack is `Iterable<E>` → works with `for(E item : stack)`

### Usage Example

```java
MyStack<Integer> stack = new MyStack<>();
stack.push(1);
stack.push(2);
stack.push(3);

for (int num : stack) {            // Works because MyStack<E> is Iterable<E>
    System.out.println(num);
}
// Output: 3, 2, 1
```

---

## 11.6 Type Erasure

### What Is It?

**Type erasure** is what happens when your Java generic code is compiled — the compiler **removes all type parameter information** and produces regular bytecode with no generics.

> The JVM at runtime has **no idea** that generics were used. It just sees regular classes and objects.

### Simple Analogy
> Generics are like **labels on boxes** — they help you at packing time (compile time). But once the box is shipped (runtime), the label is gone. The JVM just sees a plain box.

### How Type Erasure Works — The Rules

The compiler replaces type parameters using these 3 rules:

| Rule | What Happens | Example |
|---|---|---|
| **Rule 1** | Drop all `<...>` from parameterized types | `Node<Integer>` → `Node` |
| **Rule 2a** | Replace type param with its **bound** (if it has one) | `<E extends Comparable>` → `Comparable` |
| **Rule 2b** | Replace type param with **`Object`** (if no bound) | `<E>` → `Object` |
| **Rule 2c** | Replace with **first bound** (if multiple bounds) | `<E extends A & B>` → `A` |

### Before and After Erasure

```java
// BEFORE erasure (your source code):
class Node<E> {
    private E data;
    public E getData() { return data; }
    public void setData(E d) { this.data = d; }
}

// AFTER erasure (what the compiler produces):
class Node {
    private Object data;            // E → Object (no bound)
    public Object getData() { return data; }
    public void setData(Object d) { this.data = d; }
}
```

### Why Does This Matter?

- You **cannot** use a type parameter at runtime (e.g., `new E()` or `instanceof E` → compile error)
- Generic types are checked at **compile time only**
- **Bridge methods** are sometimes inserted by the compiler to maintain backward compatibility with old (pre-generics) code

---

## 11.7 Implications for Overloading and Overriding

### Method Signatures — Quick Recap
A **method signature** = method name + formal parameter list (types only, not names).

Two methods are **override-equivalent** if:
- Their signatures are the same, OR
- One signature is a **subsignature** of the other (after type erasure they become the same)

---

### Implications for Overloading

#### The Problem
After **type erasure**, different generic method signatures can become identical — making it impossible to overload them.

```java
// These three look different before erasure:
static <T> void merge(MyStack<T> s1, MyStack<T> s2)          { }
static <T> void merge(MyStack<? extends T> s1, MyStack<T> s2) { }
static <T> void merge(MyStack<? super T> s1, MyStack<T> s2)   { }

// After erasure, ALL THREE become:
//  merge(MyStack, MyStack)   ← same signature!
// → Compile-time ERROR: cannot overload these methods
```

#### Key Rule
> If two methods have the **same erased signature**, they **cannot be overloaded** in the same class — the compiler will report an error.

---

### Implications for Overriding

#### Override Criteria for Generic Methods
For a subtype method to correctly **override** a supertype method, these conditions must be satisfied:

| Condition | Detail |
|---|---|
| **Signature** | The subtype method's signature must be a subsignature of the supertype method |
| **Return type** | Must be **compatible** (same type or a subtype — covariant return) |
| **`throws` clauses** | Must be **compatible** with the supertype's throws clause |

#### Example — Wrong Override

```java
class CmpNode<E extends Comparable<E>> extends Node<E>
    implements Comparable<CmpNode<E>> {

    // ❌ WRONG — parameter type is wrong for overriding equals() from Object
    // @Override
    // boolean equals(CmpNode<E> other) { ... }   // Compile error!

    // ✅ CORRECT — matches Object.equals(Object o)
    @Override
    public boolean equals(Object other) { ... }

    // ✅ CORRECT — matches Comparable<CmpNode<E>>.compareTo()
    @Override
    public int compareTo(CmpNode<E> other) { ... }
}
```

---

### The `@Override` Annotation

`@Override` is a **safety annotation** — if you write it above a method, the compiler checks that you are actually overriding something. If not, it gives a **compile error**.

```java
@Override
public boolean equals(Object obj) { ... }   // ✅ Compiler confirms this overrides Object.equals

@Override
public boolean equals(String s) { ... }     // ❌ Compile error! — String ≠ Object, not a real override
```

> 💡 **Best practice:** Always use `@Override` when you intend to override a method — it catches typos and wrong parameter types immediately.

---

### Implications for Arrays

#### The Core Issue
Arrays support **subtype covariance** — `String[]` is a subtype of `Object[]`. But this can cause runtime problems with generics.

```java
String[] strArray = new String[] {"Hi", "Hello", "Howdy"};
Object[] objArray = strArray;        // ✅ Compiles — covariant arrays
objArray[0] = 2020.5;               // ❌ ArrayStoreException at RUNTIME!
// (You stored a Double into a String array — caught only at runtime)
```

#### You Cannot Create Generic Arrays

```java
// T is a type parameter:
T t = new T();        // ❌ Compile error — can't instantiate type parameter
T[] anArray = new T[10];  // ❌ Compile error — can't create generic array

// Also NOT allowed:
List<String>[] list1 = new List<String>[3];       // ❌ Compile error
List<String>[] list2 = new List[3];               // ⚠️ Unchecked warning
```

#### Why?
At runtime, due to **type erasure**, the JVM can't verify the type of a generic array — this could silently allow wrong types to be stored, breaking type safety.

#### Workaround
Use a typed `List` instead of an array when working with generics:
```java
List<List<String>> listOfLists = new ArrayList<>();  // ✅ Preferred
```

---

### Implications for Exception Handling

When using generics with exceptions, these strict rules apply:

| Rule | Detail | Example |
|---|---|---|
| Generic class cannot extend `Throwable` | ❌ Not allowed | `class MyException<T> extends Exception` → compile error |
| Parameterized type in `catch` block | ❌ Not allowed | `catch (MyException<String> e)` → compile error |
| Type in `catch` must be reifiable | ✅ Must be a concrete type | Only subtype of `Throwable` allowed |
| Type in `throws` clause | ✅ Allowed if it's a subtype of `Throwable` | — |

```java
// ❌ Not allowed:
class MyGenericException<T> extends Exception { }   // Compile error!

// ❌ Not allowed in catch:
try { ... }
catch (SomeException<String> e) { ... }             // Compile error!

// ✅ Allowed — type parameter in throws clause:
<T extends Exception> void doSomething() throws T { ... }
```

#### Simple Reason
Exceptions need to be checked at **runtime**, but generic types are erased at compile time — so the JVM can't distinguish between `MyException<String>` and `MyException<Integer>` at runtime, making them unusable in catch blocks.

---

## Chapter 11 — Quick Summary

| Section | Topic | Key Idea |
|---|---|---|
| 11.1 | Introducing Generics | Write once, use with any type; type-safe + no casting needed |
| 11.2 | Generic Types | Class/interface with type parameter `<E>` as a placeholder |
| 11.2 | Parameterized Types | Supply actual type: `Node<Integer>` — compiler enforces it |
| 11.2 | Generic Interfaces | Interfaces can also have type parameters; implemented generically or concretely |
| 11.3 | Collections and Generics | Before generics: unsafe, needed casts; after: type-safe, clean |
| 11.4 | Wildcards | `Node<Integer>` ≠ subtype of `Node<Number>`; use `?` wildcards for flexibility |

| 11.5 | Generic Stack Example | Real-world generic class using `IStack<E>` and `Node<E>` |
| 11.6 | Type Erasure | Compiler removes all type info at compile time; JVM sees no generics |

| 11.7 | Overloading Implications | Erased signatures can clash → compile error |
| 11.7 | Overriding Implications | Signature, return type, `throws` must all be compatible |
| 11.7 | `@Override` Annotation | Compiler validates you're actually overriding — always use it! |
| 11.7 | Arrays & Generics | Can't create generic arrays; use `List` instead |
| 11.7 | Exceptions & Generics | Generic classes can't extend `Throwable`; no parameterized `catch` |

---

*Back to [README](../README.md)*
