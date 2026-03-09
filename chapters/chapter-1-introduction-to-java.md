# Chapter 1 — Introduction to Java

[← Back to Index](../README.md)

---

## 1.1 Key Features of Java

### Multi-Paradigm Programming

- Java supports multiple styles of writing code — **object-oriented**, **procedural**, and **functional**.
- In object-oriented style, data and actions are kept together inside **objects**.
- **Encapsulation** = hiding internal details. You only see what the object lets you see (its public interface).
- **Lambda expressions** allow writing code in a functional style (like math functions).
- You can reuse existing code and break big projects into smaller, manageable pieces.

### Bytecode and JVM

- Java code is first converted into **bytecode** (a middle format), which the **JVM** then runs.
- **JIT (Just-In-Time) compiler** speeds up execution by converting frequently used bytecode to machine code on the fly.
- Other languages like Scala and Groovy also run on the JVM.

### Write Once, Run Anywhere

- The same bytecode runs on **any machine** that has a JVM installed — no need to rewrite for each OS.
- This is Java's biggest advantage for **cross-platform development**.

### Simplicity

- Java removed confusing C++ features like **pointers**, **multiple inheritance**, and **manual memory management**.
- Java handles memory cleanup automatically using **Garbage Collection** — no need to free memory manually.

### Dynamic and Distributed

- JVM can load extra libraries while the program is already running.
- Java supports **network programming** — apps can talk to each other across machines (e.g., using RMI or sockets).

### Robust and Secure

- Java is **strictly typed** — the compiler catches most errors before the program runs.
- Array bounds are checked at runtime to prevent crashes. No raw pointer access.
- **Exception handling** lets you deal with errors gracefully without crashing the program.
- JVM runs untrusted code in a **sandbox** — limits what it can do to protect the system.

### High Performance and Multithreading

- JIT finds the slow parts (hotspots) and converts them to faster machine code automatically.
- Java supports **multithreading** — run multiple tasks at the same time in one application.
- Stream API and lambda expressions help use all CPU cores efficiently for **parallel processing**.

---

## 1.2 Classes

- A **class** is like a blueprint or template. It describes what an object will look like and what it can do.
- **Fields** = data/properties. **Methods** = actions/behaviors.
- **Example:** A `Car` class has fields like `color`, `speed` and methods like `accelerate()`, `brake()`.

---

## 1.3 Objects

### Creating and Using Objects

- An **object** is a real instance created from a class using the `new` keyword.
- A **reference variable** holds the address (location in memory) of the object — not the object itself.
- A reference can be `null` — meaning it points to nothing.
- You cannot access memory directly — you always go through the reference.
- When you assign one reference to another (`a = b`), both now point to the **same object**.
- Each object has its own **separate copy** of its fields (its own data/state).

---

## 1.4 Instance Members

- **Instance variables:** each object gets its own copy — like each person having their own name.
- **Instance methods:** all objects share the same method code, but act on their own data.
- **Static members:** belong to the class, not any specific object — like a shared notice board.

### Calling Methods

```java
objectName.methodName(arguments);   // instance method call
ClassName.methodName();             // static method call (best practice)
```

---

## 1.5 Inheritance

- **Inheritance** = one class gets all features of another class automatically.
- The parent class is called **superclass**. The child class is called **subclass**.
- The child class can add new features or change existing ones.
- Use the `extends` keyword. A class can extend **only one parent** (no multiple inheritance).

```java
class Animal { }
class Dog extends Animal { }  // Dog gets all Animal features plus its own
```

- **Superclass** = general. **Subclass** = more specific.

---

## 1.6 Aggregation

- **Aggregation** = one object contains (or uses) other objects as its parts.
- Also called **composition**. Java does this using reference variables (fields that hold other objects).
- The main object passes work to its inner objects — called **delegation**.

---

## 1.7 Compiling and Running Java Programs

### Compiling

```bash
javac MyClass.java
```

- Use `javac` to compile `.java` files into `.class` files (bytecode).
- Each class gets its own `.class` file.
- A source file can have only **one public class**, and the **file name must match** that class name.

### Running

```bash
java MyClass
```

- Use `java` to run the compiled `.class` file.
- Java launches a JVM to execute the bytecode.
- Specify the class that has the `main()` method — that is where execution starts.

### Single-File Shortcut (Java 11+)

```bash
java Demo-App.java
```

- If all your code is in one `.java` file, you can skip compiling and run it directly.
- The **first class** in the file must have the `main()` method.
- No `.class` files from that file should exist when you do this.
- Code is compiled **in memory** — no files are created on disk.

---

[← Back to Index](../README.md) | [Next Chapter →](chapter-2-basic-language-elements.md)
