# Java Interview Questions & Answers

> Quick, easy answers for Java interviews — based on Chapters 1–12.

---

## Chapter 1 — Introduction to Java

**Q1. What is the JVM?**
> The **Java Virtual Machine** — it runs Java bytecode. Java code compiles to bytecode, and the JVM executes it on any machine.

**Q2. What is "Write Once, Run Anywhere"?**
> Java bytecode runs on **any OS** that has a JVM installed — no need to rewrite code for each platform.

**Q3. What is Garbage Collection?**
> Java automatically **frees memory** of objects that are no longer used. You don't need to delete objects manually.

**Q4. What is the difference between a class and an object?**
> A **class** is a blueprint. An **object** is a real instance created from that blueprint using `new`.

**Q5. What is encapsulation?**
> **Hiding internal details** of an object. Only expose what's needed through public methods.

**Q6. What is inheritance?**
> A **subclass** automatically gets all features of its **superclass** using the `extends` keyword.

**Q7. What is aggregation?**
> When one object **contains** another object as a field — also called composition.

---

## Chapter 2 — Basic Language Elements

**Q8. What are the 8 primitive data types in Java?**
> `byte`, `short`, `int`, `long`, `float`, `double`, `char`, `boolean`

**Q9. What is the default type of a whole number like `42`?**
> `int`. To make it `long`, add `L` → `42L`.

**Q10. What is the difference between `==` on primitives vs objects?**
> On **primitives**: compares values. On **objects**: compares **references** (memory addresses), not content. Use `.equals()` for content.

**Q11. What is short-circuit evaluation?**
> With `&&`, if the left side is `false`, the right is **skipped**. With `||`, if left is `true`, right is **skipped**.

**Q12. What is the difference between prefix `++i` and postfix `i++`?**
> **Prefix** (`++i`): increments first, then uses the value. **Postfix** (`i++`): uses the value first, then increments.

**Q13. What does the `>>>` operator do?**
> **Unsigned right shift** — shifts bits right and fills left with `0` (always gives a positive result).

---

## Chapter 3 — Declarations

**Q14. What is method overloading?**
> Multiple methods with the **same name** but **different parameter lists**. The compiler picks the right one based on the arguments.

**Q15. What is the default value of array elements?**
> Numbers → `0`, `boolean` → `false`, object references → `null`.

**Q16. What happens if you access an array with an out-of-bounds index?**
> `ArrayIndexOutOfBoundsException` at **runtime**.

**Q17. What are the required modifiers for `main()`?**
> `public`, `static`, `void` — in any order. Parameter must be `String[]`.

**Q18. Can `main()` be overloaded?**
> ✅ Yes, but the JVM only calls the one with `String[]` parameter.

**Q19. What is the difference between a field variable and a local variable?**
> **Field**: belongs to a class/object, has a default value. **Local**: inside a method, must be initialized before use.

---

## Chapter 4 — Control Flow

**Q20. What is the dangling else problem?**
> When nested `if` statements make it unclear which `if` an `else` belongs to. An `else` always belongs to the **nearest `if`** above it.

**Q21. What is fall-through in a `switch` statement?**
> Without `break`, execution continues into the **next case** automatically.

**Q22. What is the difference between `while` and `do-while`?**
> `while` checks condition **before** running — may run 0 times. `do-while` checks **after** — always runs at least once.

**Q23. What is the difference between `break` and `continue`?**
> `break` **exits** the loop. `continue` **skips** the current iteration and goes to the next.

**Q24. What is a labeled break?**
> `break label;` exits a **specific outer loop** by name — useful in nested loops.

---

## Chapter 5 — OOP

**Q25. What is the difference between overriding and overloading?**
> **Overriding**: subclass redefines a parent method with the same signature — decided at **runtime**. **Overloading**: same method name, different parameters — decided at **compile time**.

**Q26. Can you override a static method?**
> ❌ No. Static methods are **hidden**, not overridden. They are bound at compile time.

**Q27. What is an abstract class?**
> A class declared with `abstract` — **cannot be instantiated**. Can have abstract methods (no body) that subclasses must implement.

**Q28. What is the difference between abstract class and interface?**
> **Abstract class**: can have state (fields), constructors, and concrete methods. **Interface**: defines a contract — no state, no constructors (traditionally).

**Q29. What does `final` mean on a class, method, and variable?**
> **Class**: cannot be extended. **Method**: cannot be overridden. **Variable**: value/reference cannot be reassigned.

**Q30. What is polymorphism?**
> A **supertype reference** can point to any subtype object. The method that runs is determined by the **actual object type** at runtime.

**Q31. What is an enum?**
> A special class with a **fixed set of named constants**, e.g., `enum Day { MON, TUE, WED }`.

**Q32. What is a record class?**
> A shortcut for **data-holding classes**. The compiler auto-generates constructor, getters, `equals()`, `hashCode()`, and `toString()`.

**Q33. What is a sealed class?**
> A class that **restricts which classes can extend it** using `permits`. Gives the author control over the class hierarchy.

---

## Chapter 6 — Access Control

**Q34. What are the four access modifiers in Java?**

| Modifier | Accessible From |
|---|---|
| `public` | Everywhere |
| `protected` | Same package + subclasses |
| *(default)* | Same package only |
| `private` | Same class only |

**Q35. What is a package?**
> A **group of related classes/interfaces** — like a folder. Declared with `package com.example;`.

**Q36. Can a Java file have multiple classes?**
> ✅ Yes, but only **one can be `public`**, and the **file name must match** that public class name.

---

## Chapter 7 — Exception Handling

**Q37. What is the difference between checked and unchecked exceptions?**
> **Checked**: must be handled or declared with `throws` (e.g., `IOException`). **Unchecked**: runtime exceptions — no requirement to handle (e.g., `NullPointerException`).

**Q38. What is the exception hierarchy?**
> `Throwable` → `Exception` (checked) / `Error` (serious JVM issues) → `RuntimeException` (unchecked).

**Q39. What is `finally` used for?**
> Code in `finally` **always runs** — whether or not an exception occurred. Used for cleanup.

**Q40. What is try-with-resources?**
> Automatically **closes** resources (like files) when done — no need for `finally` to close them manually.

---

## Chapter 8 — Selected API Classes

**Q41. What is the `java.lang` package?**
> The **most important package** — automatically imported. Contains `Object`, `String`, `Math`, wrapper classes, etc.

**Q42. What is the `Object` class?**
> The **root of all Java classes**. Every class directly or indirectly extends `Object`. Provides `equals()`, `hashCode()`, `toString()`, etc.

**Q43. What are wrapper classes?**
> Classes that **wrap primitives as objects** — e.g., `int` → `Integer`, `double` → `Double`. Required when objects are needed (like in collections).

**Q44. What is autoboxing and unboxing?**
> **Autoboxing**: Java automatically converts `int` → `Integer`. **Unboxing**: `Integer` → `int`. Done automatically by the compiler.

**Q45. Why is `String` immutable?**
> Once created, a `String`'s content **cannot change**. This makes it thread-safe and allows string pooling (memory optimization).

**Q46. What is the String pool?**
> A **cache of string literals** in memory. If two string literals have the same content, they share the **same object** — saves memory.

**Q47. Why should you use `.equals()` instead of `==` for Strings?**
> `==` compares **references** (memory addresses). `.equals()` compares **content**. Two different `String` objects can have the same content.

---

## Chapter 9 — Nested Type Declarations

**Q48. What are the three categories of nested types?**
> **Static member types**, **inner classes (non-static)**, and **local classes**.

**Q49. What is the difference between a static nested class and an inner class?**
> **Static nested class**: doesn't need an outer instance — accessed like `Outer.Inner`. **Inner class**: tied to an outer object — needs `outerObj.new Inner()`.

**Q50. What is a local class?**
> A class defined **inside a method or block** — only visible within that block.

**Q51. What is an anonymous class?**
> A class **defined and instantiated at the same time** with no name — used for one-off implementations like listeners or comparators.

**Q52. Can an inner class access private members of the outer class?**
> ✅ Yes — inner classes have full access to all members of the enclosing class, including `private` ones.

---

## Chapter 10 — Object Lifetime

**Q53. What is the heap?**
> The area of memory where **objects are stored** when created with `new`.

**Q54. When does an object become eligible for garbage collection?**
> When there are **no more references** pointing to it — it becomes **unreachable**.

**Q55. Can circular references be garbage collected?**
> ✅ Yes — Java's GC can detect and collect circular references as long as they are not reachable from any active thread.

**Q56. What are the three types of initializers?**
> **Field initializer expressions** (inline), **static initializer blocks** (`static { }`), and **instance initializer blocks** (`{ }`).

**Q57. When does a static initializer block run?**
> Once, when the **class is first loaded** by the JVM.

**Q58. When does an instance initializer block run?**
> Every time a **new object** of the class is created — before the constructor body.

---

## Chapter 11 — Generics

**Q59. What are generics?**
> A way to write **type-safe, reusable code**. You write code once and it works with any type — e.g., `List<String>`, `List<Integer>`.

**Q60. What is a type parameter?**
> A **placeholder** like `E` or `T` in a generic class — replaced with an actual type when used. E.g., `class Box<T>`.

**Q61. What is a parameterized type?**
> When you supply an actual type to a generic class — e.g., `Box<Integer>` or `List<String>`.

**Q62. What is the diamond operator `<>`?**
> Lets the compiler **infer** the type parameter. Instead of `new ArrayList<String>()`, write `new ArrayList<>()`.

**Q63. Is `List<Integer>` a subtype of `List<Number>`?**
> ❌ No — parameterized types are **invariant**. Even though `Integer` is a subtype of `Number`, `List<Integer>` is NOT a subtype of `List<Number>`.

**Q64. What is a wildcard `?` in generics?**
> Represents an **unknown type**. `? extends T` means any subtype of T. `? super T` means any supertype of T.

**Q65. What is type erasure?**
> At compile time, the compiler **removes all generic type information**. At runtime, the JVM sees no generics — `List<String>` becomes just `List`.

**Q66. Why can't you create a generic array like `new T[10]`?**
> Because of **type erasure** — at runtime the JVM doesn't know what `T` is, so it can't create the array safely. Use `List<T>` instead.

**Q67. Why can't a generic class extend `Throwable`?**
> Because of type erasure — the JVM can't distinguish `MyException<String>` from `MyException<Integer>` at runtime, making `catch` blocks unreliable.

---

## Chapter 12 — Collections & ArrayLists

**Q68. What is the Java Collections Framework?**
> A set of **ready-made data structures** in `java.util` — includes lists, sets, queues, maps, etc.

**Q69. What is the hierarchy of `ArrayList`?**
> `Iterable<E>` → `Collection<E>` → `List<E>` → `ArrayList<E>`

**Q70. What is the difference between an array and an ArrayList?**
> **Array**: fixed size, fast. **ArrayList**: dynamic size, easy insert/delete, slightly slower.

**Q71. Why should you declare a list as `List<E>` instead of `ArrayList<E>`?**
> **Flexibility** — you can easily swap the implementation (e.g., to `LinkedList`) without changing the rest of your code.

**Q72. What does `add(E)` do in an ArrayList?**
> Appends the element to the **end** of the list. The list grows automatically.

**Q73. What is the default initial capacity of an ArrayList?**
> **10** — but it grows automatically when elements are added beyond capacity.

**Q74. Is `ArrayList` thread-safe?**
> ❌ No. Use `Collections.synchronizedList()` or `CopyOnWriteArrayList` for thread-safe lists.

---

## General / Tricky Questions

**Q75. What is the difference between `String`, `StringBuilder`, and `StringBuffer`?**
> **String**: immutable. **StringBuilder**: mutable, fast, not thread-safe. **StringBuffer**: mutable, thread-safe but slower.

**Q76. What is `null`?**
> A special value meaning a reference **points to nothing**. Accessing a member on a `null` reference causes `NullPointerException`.

**Q77. Can you have a `try` block without a `catch`?**
> ✅ Yes — if it has a `finally` block: `try { } finally { }`.

**Q78. What is the difference between `==` and `.equals()`?**
> `==` compares **references** (memory address). `.equals()` compares **content/value** (can be overridden).

**Q79. Can an interface have a constructor?**
> ❌ No — interfaces cannot be instantiated and have no constructors.

**Q80. What is the `@Override` annotation?**
> Tells the compiler to **verify** that the method actually overrides a supertype method. Gives a compile error if it doesn't — prevents typos.

---

*Based on: A Programmer's Guide to Java — Chapters 1–12*

*Back to [README](../README.md)*
