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

## 12.2 (cont.) — Creating Unmodifiable Lists

### What is an Unmodifiable List?
An **unmodifiable list** is a list that is **locked** — you can read it but you cannot change it.

> Think of it like a printed menu — you can read it, but you can't add or remove items.

### How to Create One — `List.of()`

```java
List<String> names = List.of("Ada", "kayak", "Naan");   // ✅ Unmodifiable list
```

- `List.of()` is overloaded — accepts **0 to 10 elements** (plus a varargs version for more)
- Elements are stored in the **same order** as passed

### Key Rules

| Rule | Detail |
|---|---|
| Can add elements? | ❌ `UnsupportedOperationException` |
| Can remove elements? | ❌ `UnsupportedOperationException` |
| Can replace elements? | ❌ `UnsupportedOperationException` |
| Can sort? | ❌ `UnsupportedOperationException` |
| Allows `null`? | ❌ `NullPointerException` if you try |
| Allows duplicates? | ✅ Yes |
| Order preserved? | ✅ Yes — same order as arguments |

### `List.copyOf()` — Snapshot Copy

```java
List<String> fab4 = new ArrayList<>();
fab4.add("John"); fab4.add("Paul"); fab4.add("George"); fab4.add("Ringo");

List<String> fabAlways = List.copyOf(fab4);  // Unmodifiable snapshot

fab4.remove("John");   // modifies original list
fab4.remove("George");

System.out.println(fab4);       // [Paul, Ringo]        — original changed
System.out.println(fabAlways);  // [John, Paul, George, Ringo] — copy unchanged!
```

> `List.copyOf()` creates a **fixed snapshot** — changes to the original do NOT affect the copy.

### Unmodifiable vs Regular List

| Feature | Regular ArrayList | Unmodifiable List (`List.of`) |
|---|---|---|
| Add elements | ✅ Yes | ❌ No |
| Remove elements | ✅ Yes | ❌ No |
| Read elements | ✅ Yes | ✅ Yes |
| Allows `null` | ✅ Yes | ❌ No |
| Memory efficient | Normal | ✅ More efficient |

---

## 12.3 Modifying an ArrayList

### Adding Elements

#### `add(E element)` — Append to end
```java
list.add("Bob");       // adds "Bob" at the END of the list
```
Returns `true` if the list was changed.

#### `add(int index, E element)` — Insert at position
```java
list.add(0, "Alice");  // inserts "Alice" at index 0, shifts everything right
```
Throws `IndexOutOfBoundsException` if index < 0 or > size().

#### `addAll(Collection c)` — Add all from another collection
```java
list.addAll(otherList);         // appends all elements from otherList at the end
list.addAll(2, otherList);      // inserts all at index 2, shifts rest right
```

### Removing Elements

#### `remove(int index)` — Remove by position
```java
list.remove(0);        // removes element at index 0 — shifts remaining elements left
```

#### `remove(Object o)` — Remove by value
```java
list.remove("John");   // removes the FIRST occurrence of "John"
```

> ⚠️ **Tricky:** For `List<Integer>`, `remove(1)` removes by **index**, not value.
> To remove the value `1`, use: `list.remove(Integer.valueOf(1))`

### Replacing Elements

#### `set(int index, E element)` — Replace at position
```java
list.set(0, "NewValue");  // replaces element at index 0 with "NewValue"
```
Returns the old element that was replaced.

### Quick Method Reference

| Method | What it does |
|---|---|
| `add(E e)` | Append to end |
| `add(int i, E e)` | Insert at index `i` |
| `addAll(Collection c)` | Add all from another collection at end |
| `addAll(int i, Collection c)` | Insert all at index `i` |
| `remove(int i)` | Remove at index `i` |
| `remove(Object o)` | Remove first occurrence of value |
| `set(int i, E e)` | Replace element at index `i` |

---

## 12.4 Querying an ArrayList

### Size and Empty Check

```java
list.size();       // number of elements currently in the list
list.isEmpty();    // true if size == 0
```

- First element is always at index `0`
- Last element is always at index `size() - 1`

### Getting Elements

```java
list.get(0);                  // gets first element
list.get(list.size() - 1);    // gets last element
```
Throws `IndexOutOfBoundsException` if index < 0 or >= size().

### Searching

```java
list.contains("kayak");       // true if "kayak" is in the list
list.indexOf("Bob");          // index of FIRST occurrence (-1 if not found)
list.lastIndexOf("Bob");      // index of LAST occurrence (-1 if not found)
```
`contains()`, `indexOf()`, `lastIndexOf()` use **`.equals()`** to compare — not `==`.

### Comparing Two Lists

```java
List<String> strList2 = new ArrayList<>(strList);
boolean equal = strList.equals(strList2);   // true — same size and same elements in order
```
Two lists are equal if: same size + all corresponding elements are equal (by `.equals()`).

### Getting a Sublist

```java
List<String> sub = strList.subList(1, 4);  // elements from index 1 to 3 (not 4)
```

> ⚠️ `subList()` returns a **view** — not a copy. Changes to the sublist **affect the original list** and vice versa!

```java
// [Naan, kayak, Bob, Rotator, Bob]
List<String> strList3 = strList.subList(1, 4);  // [kayak, Bob, Rotator]
strList3.remove(0);                              // removes "kayak" from sublist
// Now strList = [Naan, Bob, Rotator, Bob]       — original is also changed!
```

### All Query Methods

| Method | Returns | What it does |
|---|---|---|
| `size()` | `int` | Number of elements |
| `isEmpty()` | `boolean` | Is the list empty? |
| `get(int i)` | `E` | Element at index `i` |
| `contains(Object o)` | `boolean` | Is `o` in the list? |
| `indexOf(Object o)` | `int` | Index of first match (-1 if none) |
| `lastIndexOf(Object o)` | `int` | Index of last match (-1 if none) |
| `subList(int from, int to)` | `List<E>` | View from `from` to `to-1` |
| `equals(Object o)` | `boolean` | Same size and elements? |

---

## Chapter 12 — Quick Summary

| Section | Topic | Key Idea |
|---|---|---|
| 12.1 | Lists | Ordered, indexed, allows duplicates; dynamic unlike arrays |
| 12.1 | Collections Framework | `Iterable → Collection → List → ArrayList` hierarchy |
| 12.2 | Declaring ArrayLists | Use `List<E>` type, `new ArrayList<>()`, diamond operator |
| 12.2 | Constructors | Empty (default), empty (sized), or copy from another collection |
| 12.2 | Adding Elements | `add(E)` appends to end; list grows automatically |
| 12.2 | Unmodifiable Lists | `List.of()` — locked list; no add/remove/null; `List.copyOf()` for snapshot |
| 12.3 | Modifying ArrayList | `add`, `remove`, `set`, `addAll` — insert, delete, replace elements |
| 12.4 | Querying ArrayList | `size`, `get`, `contains`, `indexOf`, `subList`, `equals` |

---

*Back to [README](../README.md)*
