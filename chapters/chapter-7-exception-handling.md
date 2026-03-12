# Chapter 7 — Exception Handling

[← Back to Index](../README.md)

---

## What is an Exception?

An **exception** is an unexpected event that **disrupts the normal flow** of a program.

Common causes:
- File not found
- Array index out of bounds
- Network connection failed
- Division by zero

Instead of writing `if` checks everywhere (which makes code messy), Java has a built-in **exception handling mechanism** to deal with these events cleanly.

---

## 7.1 Stack-Based Execution and Exception Propagation

### The Throw-and-Catch Idea

Java uses the **throw-and-catch** model:

| Term | Meaning |
|------|---------|
| **Throw** | Signal that something went wrong |
| **Catch** | Handle the problem gracefully |
| **Exception handler** | The code that catches and deals with the exception |

> The exception does NOT have to be caught in the same method where it was thrown — it can travel up the call chain.

---

### How the JVM Stack Works

Think of the JVM stack like a **stack of plates**:

- Every time a method is called → a new **stack frame** (activation frame) is **pushed** on top.
- The stack frame holds all local variables and info about that method call.
- The method on **top** is the one currently running.
- When a method finishes → its frame is **popped off** → execution returns to the method below it.
- All the active methods on the stack at any moment = the **stack trace**.

```
main() calls printAverage()
printAverage() calls computeAverage()

Stack (top to bottom):
┌─────────────────────┐
│  computeAverage()   │  ← currently running
├─────────────────────┤
│  printAverage()     │  ← waiting
├─────────────────────┤
│  main()             │  ← waiting
└─────────────────────┘
```

When `computeAverage()` finishes → its frame is removed → `printAverage()` resumes.

### Exception Propagation

If an exception is thrown and **not caught** in the current method:
- The current method's frame is **popped off** the stack.
- The exception **travels up** to the method that called it.
- This continues until the exception is caught, or the program crashes with a stack trace printed.

---

## 7.2 Exception Types

All exceptions in Java are **objects**. Every exception class ultimately inherits from `java.lang.Throwable`.

### The Hierarchy

```
java.lang.Throwable
├── Exception                    ← things a program should handle
│   ├── RuntimeException         ← programming bugs (unchecked)
│   │   ├── ArithmeticException
│   │   ├── ArrayIndexOutOfBoundsException
│   │   ├── NullPointerException
│   │   ├── ClassCastException
│   │   ├── IllegalArgumentException
│   │   │   ├── IllegalThreadStateException
│   │   │   └── NumberFormatException
│   │   ├── IndexOutOfBoundsException
│   │   │   └── ArrayIndexOutOfBoundsException
│   │   └── UnsupportedOperationException
│   ├── ClassNotFoundException   ← checked
│   └── IOException              ← checked
│       ├── EOFException
│       ├── FileNotFoundException
│       └── NotSerializableException
└── Error                        ← serious JVM problems (don't catch these)
    └── AssertionError
```

> **Shaded classes** (RuntimeException, Error and their subclasses) = **Unchecked exceptions**

---

### Common Methods on All Throwables

Every exception object has these useful methods:

| Method | What it does |
|--------|-------------|
| `getMessage()` | Returns the detail message (what went wrong) |
| `printStackTrace()` | Prints the full stack trace to the error stream |
| `toString()` | Returns class name + detail message |

```java
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println(e.getMessage());      // "/ by zero"
    e.printStackTrace();                      // prints full trace
}
```

---

## The `java.lang.Exception` Class

`Exception` is the parent of all exceptions a program would normally want to **catch and handle**.

It has two main branches:

| Branch | Type | Description |
|--------|------|-------------|
| `RuntimeException` | Unchecked | Programming bugs — usually avoidable |
| Other subclasses | Checked | External problems — must be handled |

---

## The `java.lang.RuntimeException` Class

**Runtime exceptions** are caused by **programming mistakes** — bugs in the code. They are usually avoidable if the code is written correctly.

> These are **unchecked** — the compiler does NOT force you to handle them.

### Common Runtime Exceptions

#### `ArithmeticException`
Thrown when an **illegal arithmetic operation** happens — most commonly **dividing by zero**.
```java
int x = 10 / 0;   // throws ArithmeticException: / by zero
```

#### `ArrayIndexOutOfBoundsException`
Thrown when you try to access an array with an **invalid index** (less than 0 or ≥ array length).
```java
int[] arr = new int[5];
arr[10] = 1;   // throws ArrayIndexOutOfBoundsException
```

#### `ArrayStoreException`
Thrown when you try to store an **object of the wrong type** into an array.
```java
Object[] arr = new String[3];
arr[0] = 42;   // throws ArrayStoreException (42 is not a String)
```

#### `ClassCastException`
Thrown when you try to **cast** an object to a type it doesn't belong to.
```java
Object obj = "hello";
Integer i = (Integer) obj;   // throws ClassCastException
```

#### `IllegalArgumentException`
Thrown when a method receives an **illegal or inappropriate argument**.
```java
// e.g., passing an invalid date format pattern to DateTimeFormatter
```

#### `IllegalThreadStateException`
A subclass of `IllegalArgumentException`. Thrown when a **thread operation** is attempted at the wrong time — for example, calling `start()` on a thread that has already started.

#### `NumberFormatException`
A subclass of `IllegalArgumentException`. Thrown when trying to **convert a string to a number** but the string is not a valid number.
```java
int n = Integer.parseInt("abc");   // throws NumberFormatException
```

#### `NullPointerException`
Thrown when the **JVM tries to use a `null` reference** — calling a method on null, or accessing a field through null.
```java
String s = null;
s.toLowerCase();   // throws NullPointerException
```
> One of the **most common exceptions** in Java. The error message tells you exactly which reference was null and where.

#### `UnsupportedOperationException`
Thrown when a method is **not supported** by the implementing class. Common in the Collections Framework when an optional operation isn't implemented.

---

## Checked Exceptions

**Checked exceptions** = subclasses of `Exception` that are **NOT** `RuntimeException` or `Error`.

- They represent **external problems** that the program cannot always avoid (e.g., file not found).
- The **compiler forces** you to either `catch` them or declare them with `throws`.

### Common Checked Exceptions

#### `java.lang.ClassNotFoundException`
Thrown when the **JVM cannot find a class** by its name — often a misspelled class name in the `java` command.

#### `java.io.IOException`
Parent of all **I/O (input/output) related** exceptions. Thrown when reading/writing files or streams fails.

#### `java.io.EOFException`
Subclass of `IOException`. Thrown when the **end of a file or stream is reached unexpectedly** — meaning more input was expected but there's nothing left to read.

#### `java.io.FileNotFoundException`
Subclass of `IOException`. Thrown when trying to **open a file that doesn't exist**, or when the program doesn't have permission to access it.

#### `java.io.NotSerializableException`
Subclass of `IOException`. Thrown when trying to **serialize an object** (convert to bytes) but the object's class doesn't implement the `Serializable` interface.

#### `java.sql.SQLException`
Thrown when a **database-related error** occurs. Carries information about what went wrong with the database operation.

#### `java.text.ParseException`
Thrown when **parsing a string** (converting text to a number, date, etc.) fails due to unexpected format.

#### `java.time.DateTimeException`
Thrown when **creating, querying, or manipulating date-time objects** fails.

#### `java.time.format.DateTimeParseException`
Subclass of `DateTimeException`. Thrown when parsing a **date or time string** fails.

#### `java.util.MissingResourceException`
Thrown when a **resource bundle** (like a language file) cannot be found for a given key or base name.

---

## The `java.lang.Error` Class

**Errors** represent **serious JVM-level problems** that are almost always **unrecoverable**.

- You should **NOT try to catch** these — they indicate the JVM itself is broken.
- They are signaled by the JVM, not by your application code.

| Subclass | What it means |
|----------|--------------|
| `VirtualMachineError` | JVM is broken |
| `StackOverflowError` | Stack ran out of space (infinite recursion) |
| `OutOfMemoryError` | JVM ran out of memory for objects |
| `LinkageError` | Class definition issues |
| `NoClassDefFoundError` | Class was present at compile time but missing at runtime |
| `AssertionError` | Java assertion (`assert`) failed |

---

## Checked vs Unchecked Exceptions

| Type | Classes | Compiler Forces You? | Cause |
|------|---------|---------------------|-------|
| **Checked** | `Exception` subclasses (except `RuntimeException`) | ✅ Yes — must catch or declare | External problems (file missing, network, DB) |
| **Unchecked** | `RuntimeException` + `Error` subclasses | ❌ No | Programming bugs / JVM errors |

### Simple Rule

- **Checked** = things that can go wrong even if your code is perfect (file missing, server down) → compiler makes you handle them.
- **Unchecked (Runtime)** = bugs in your code (null pointer, bad index, wrong cast) → fix the code instead of catching them.
- **Error** = JVM is broken → don't catch, just let it crash.

---

[← Previous Chapter](chapter-6-access-control.md) | [Back to Index](../README.md)
