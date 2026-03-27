# Chapter 12 — Collections, Part I: ArrayLists

> Java provides a powerful set of ready-made data structures called the **Java Collections Framework**. This chapter focuses on **Lists**, especially `ArrayList`.

---

## 12.1 Lists

### What is a Collection?
A **collection** is a group of objects stored together so you can work with them as a unit. Java uses the term **container** for this. Examples: lists, sets, queues, stacks.

### What is a List?
A **list** is a collection that:
- Stores elements **in order** (called *positional order*)
- Allows **duplicate** elements
- Lets you **access any element by its index** (like an array)

### List vs Array

| Feature | Array | List (e.g. ArrayList) |
|---|---|---|
| Size fixed? | ✅ Yes — fixed at creation | ❌ No — grows/shrinks dynamically |
| Index-based access? | ✅ Yes | ✅ Yes |
| Duplicates allowed? | ✅ Yes | ✅ Yes |
| Can insert/delete? | ❌ Hard | ✅ Easy |

### Sorting
A list can be **sorted** — elements ordered by some criteria (e.g. alphabetically). But sorting is **not mandatory** — a list is just ordered by insertion by default.

---

### Overview of the Java Collections Framework

The `Collection<E>` interface (in `java.util`) is the **root** of the collections hierarchy. It defines the general operations all collections support.

#### Inheritance Hierarchy (simplified)

```
java.lang.Iterable<E>
    └── java.util.Collection<E>
            └── java.util.List<E>
                        └── ArrayList<E>   ← most commonly used
```

Key points:
- `Collection<E>` defines general operations (add, remove, size, contains, etc.)
- `List<E>` adds **position-based** operations (get by index, set, subList, etc.)
- `ArrayList<E>` is the **best general-purpose** implementation of `List<E>`

### Why ArrayList?
- Backed by a **dynamic array** — resizes automatically
- **Fast random access** — get any element in constant time O(1)
- Not thread-safe (don't use in multithreaded code without synchronization)
- Best choice for most everyday use cases

---

## 12.2 Declaring References and Constructing ArrayLists

### Creating an ArrayList

```java
// Basic empty ArrayList of Strings
ArrayList<String> palindromes = new ArrayList<>();   // ✅ using diamond operator <>
```

- `String` is the **element type** — only Strings can be stored
- `<>` — diamond operator lets the compiler infer the type
- Initial **capacity** = 10 (default) — how many elements it can hold before resizing

### Three Ways to Construct

#### 1. Empty ArrayList (default capacity 20)
```java
ArrayList<String> palindromes = new ArrayList<>(20);  // initial capacity 20
```

#### 2. Empty ArrayList (default capacity)
```java
ArrayList<String> palindromes = new ArrayList<>();    // capacity starts at 10
```

#### 3. ArrayList from another collection
```java
List<String> wordList = new ArrayList<>(palindromes); // copies all elements in
```

---

### Best Practice — Use the Interface Type

```java
// ✅ BEST — declare as List<E> interface type
List<String> palindromes = new ArrayList<>();

// This means you can easily swap ArrayList for LinkedList later:
List<String> palindromes = new LinkedList<>();   // just change the right side!
```

> 💡 Always declare the variable as `List<E>` (the interface), not `ArrayList<E>` (the class) — this keeps your code flexible.

---

### Using the Wrong Type

```java
ArrayList<String> palindromes = new ArrayList<>();

// ❌ Using raw type (no type parameter) — unchecked warning
ArrayList palindromes = new ArrayList();

// ❌ Wrong element type — compile error
ArrayList<Integer> intList = new ArrayList<>();
intList.add(100);
intList.add(1000);
// intList.get(0) returns Integer, not String
```

---

### Adding Elements with `add(E)`

```java
List<String> palindromes = new ArrayList<>();
palindromes.add("Ada");       // added at end
palindromes.add("kayak");     // added at end
palindromes.add("Naan");      // added at end

System.out.println(palindromes);
// Output: [Ada, kayak, Naan]
```

- `add(E element)` appends to the **end** of the list
- The list **grows by 1** each time

---

### Printing an ArrayList

```java
// Method 1 — toString() directly
System.out.println(palindromes);         // [Ada, kayak, Naan]

// Method 2 — using for-each loop
for (String word : palindromes) {
    System.out.println(word);            // one element per line
}
```

---

### Key ArrayList Facts

| Feature | Detail |
|---|---|
| Package | `java.util` |
| Implements | `List<E>`, `Collection<E>`, `Iterable<E>` |
| Backed by | Dynamic array |
| Default capacity | 10 |
| Thread-safe? | ❌ No |
| Allows duplicates? | ✅ Yes |
| Allows `null`? | ✅ Yes |
| Best for | General-purpose list, fast random access |

---

## Chapter 12 — Quick Summary

| Section | Topic | Key Idea |
|---|---|---|
| 12.1 | Lists | Ordered, indexed, allows duplicates; dynamic unlike arrays |
| 12.1 | Collections Framework | `Iterable → Collection → List → ArrayList` hierarchy |
| 12.2 | Declaring ArrayLists | Use `List<E>` type, `new ArrayList<>()`, diamond operator |
| 12.2 | Constructors | Empty (default), empty (sized), or copy from another collection |
| 12.2 | Adding Elements | `add(E)` appends to end; list grows automatically |

---

*Back to [README](../README.md)*
