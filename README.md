# ☕ Java Programming — Complete Study Notes

> A structured, beginner-friendly guide to Java covering core concepts from fundamentals to OOP and beyond.

---

## 📖 Table of Contents

| # | Chapter | Topics Covered |
|---|---------|----------------|
| 1 | [Introduction to Java](chapters/chapter-1-introduction-to-java.md) | Features, JVM, Bytecode, Classes, Objects, Inheritance, Compilation |
| 2 | [Basic Language Elements](chapters/chapter-2-basic-language-elements.md) | Tokens, Data Types, Operators, Literals, Precedence, Shifts |
| 3 | [Declarations](chapters/chapter-3-declarations.md) | Classes, Methods, Variables, Arrays, Overloading, main() |
| 4 | [Control Flow](chapters/chapter-4-control-flow.md) | if/else, switch, Loops, break, continue, return |
| 5 | [Object-Oriented Programming](chapters/chapter-5-oops.md) | Inheritance, Polymorphism, Interfaces, Abstract Classes, Enums, Records, Sealed Classes |
| 6 | [Access Control](chapters/chapter-6-access-control.md) | Encapsulation, Packages, Source File Structure, Access Modifiers |
| 7 | [Exception Handling](chapters/chapter-7-exception-handling.md) | Stack Execution, Exception Types, Checked vs Unchecked, Error Class |
| 8 | [Selected API Classes](chapters/chapter-8-selected-api-classes.md) | java.lang Package, Object Class, Wrapper Classes, String Class |
| 9 | [Nested Type Declarations](chapters/chapter-9-nested-type-declarations.md) | Static Member Types, Inner Classes, Local Classes, Anonymous Classes |

---

## 📚 Chapter Summaries

### [Chapter 1 — Introduction to Java](chapters/chapter-1-introduction-to-java.md)
Covers what makes Java special: **Write Once, Run Anywhere**, the **JVM and bytecode**, garbage collection, and security. Introduces the building blocks — **classes**, **objects**, **instance members**, **inheritance**, and **aggregation** — and explains how to compile and run Java programs.

### [Chapter 2 — Basic Language Elements](chapters/chapter-2-basic-language-elements.md)
Dives into the **syntax layer** of Java: identifiers, keywords, literals, and separators. Explains all **8 primitive data types**, **operator precedence and associativity**, arithmetic, relational, equality, conditional (`&&`, `||`), increment/decrement, and **bit-shift operators**.

### [Chapter 3 — Declarations](chapters/chapter-3-declarations.md)
Explains how to formally declare **classes**, **methods**, and **variables** in Java. Covers **method overloading**, **arrays** (declaration, creation, initialization, access), and the special rules around the **`main()` method** as the program entry point.

### [Chapter 4 — Control Flow](chapters/chapter-4-control-flow.md)
Covers how Java controls the order of execution. Includes **selection** (`if`, `if-else`, `switch`), **iteration** (`while`, `do-while`, `for`, enhanced `for`), and **transfer** statements (`break`, `continue`, `return`). Also explains labeled breaks and the dangling-else problem.

### [Chapter 5 — Object-Oriented Programming](chapters/chapter-5-oops.md)
The heart of Java: **inheritance**, **overriding vs overloading**, **abstract classes**, **interfaces**, **polymorphism**, **final** keyword, **enums**, **record classes**, and **sealed classes**. Covers the `super` keyword and field/static method hiding.

### [Chapter 6 — Access Control](chapters/chapter-6-access-control.md)
Explains **encapsulation** as a design principle and how Java enforces it through **access modifiers**, **packages**, and **modules**. Covers Java source file structure, package naming conventions, and how to import and organize classes.

### [Chapter 7 — Exception Handling](chapters/chapter-7-exception-handling.md)
Covers how Java handles unexpected events using the **throw-and-catch** model. Explains the **JVM stack** and how exceptions propagate, the full **exception hierarchy** (`Throwable`, `Exception`, `Error`), common runtime and checked exceptions with real examples, and the difference between **checked vs unchecked exceptions**.

### [Chapter 8 — Selected API Classes](chapters/chapter-8-selected-api-classes.md)
Introduces the **`java.lang` package** and its key classes. Covers the **`Object` class** (root of all Java classes) and its core methods, **wrapper classes** (boxing/unboxing, `Integer`, `Double`, etc.), and the **`String` class** in depth — immutability, string pool/interning, constructors, and the most useful string methods.

### [Chapter 9 — Nested Type Declarations](chapters/chapter-9-nested-type-declarations.md)
Covers types declared **inside** other types. Explains the three categories of nested types: **static member types** (behave like top-level types, no outer instance needed), **non-static member classes / inner classes** (tied to an enclosing object, great for data structures), and **local classes** (defined inside a method or block, scoped like a local variable). Includes comparison tables and code examples throughout.

---

## 🗂️ How to Use These Notes

- Each chapter file is self-contained and can be read independently.
- Code examples are included throughout for every key concept.
- Notes are written in plain English — ideal for beginners and revision.

---

## 🛠️ Prerequisites

- Basic understanding of programming logic is helpful but not required.
- Java JDK 11+ recommended for running examples.
- Any IDE (IntelliJ IDEA, Eclipse, VS Code) or a simple text editor works.

---

## 📌 Quick Reference

| Concept | Chapter |
|---------|---------|
| How JVM works | [Ch. 1](chapters/chapter-1-introduction-to-java.md#12-classes) |
| Primitive data types | [Ch. 2](chapters/chapter-2-basic-language-elements.md#22-primitive-data-types) |
| Arrays | [Ch. 3](chapters/chapter-3-declarations.md#36-arrays) |
| Loops | [Ch. 4](chapters/chapter-4-control-flow.md#43-iteration-statements-loops) |
| Interfaces & Polymorphism | [Ch. 5](chapters/chapter-5-oops.md#8-polymorphism) |
| Packages & Encapsulation | [Ch. 6](chapters/chapter-6-access-control.md) |
| Exception Types & Hierarchy | [Ch. 7](chapters/chapter-7-exception-handling.md) |
| Checked vs Unchecked | [Ch. 7](chapters/chapter-7-exception-handling.md#checked-vs-unchecked-exceptions) |
| Wrapper Classes & Boxing | [Ch. 8](chapters/chapter-8-selected-api-classes.md#83-the-wrapper-classes) |
| String Methods | [Ch. 8](chapters/chapter-8-selected-api-classes.md#84-the-string-class) |
| Static Member Types | [Ch. 9](chapters/chapter-9-nested-type-declarations.md#92-static-member-types) |
| Inner Classes | [Ch. 9](chapters/chapter-9-nested-type-declarations.md#93-non-static-member-classes-inner-classes) |
| Local Classes | [Ch. 9](chapters/chapter-9-nested-type-declarations.md#94-local-classes) |

---

*Notes compiled for learning purposes. Java is a trademark of Oracle Corporation.*
