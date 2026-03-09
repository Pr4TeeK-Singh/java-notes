# Chapter 5 — Object-Oriented Programming (OOPs)

[← Back to Index](../README.md)

---

## 1. Inheritance

A way to create a new class **from an existing one**.

- The new class is called a **subclass** (child).
- The existing class is called the **superclass** (parent).
- The subclass **inherits** all accessible members (fields, methods) from the parent.

```java
class TubeLight extends Light { }
```

- Java allows only **single inheritance** (one parent class per child).
- All classes ultimately inherit from **`java.lang.Object`**.

---

## 2. Overriding vs Overloading

| Feature | Overriding | Overloading |
|---------|-----------|-------------|
| Definition | Subclass redefines a parent method with the **same name and parameters** | Same method name but **different parameters** |
| Decided at | **Runtime** | **Compile time** |
| Applies to | Instance methods | Any method |
| Requirement | Same signature | Different signature |

- Only **non-final instance methods** can be overridden.

---

## 3. Hiding Fields & Static Methods

- A subclass **cannot override** a field — but it can **hide** it by declaring a field with the same name.
- Which hidden field is used depends on the **declared type of the reference** (not the actual object).
- **Static methods** can also be **hidden** (not overridden).
- Static method calls are bound at **compile time**.

---

## 4. The `super` Keyword

- `super` is used to access **parent class members** (fields and methods) from a subclass.
- Useful when the subclass has overridden or hidden a member and you still want the **parent's version**.

```java
class Animal {
    String sound = "...";
    void speak() { System.out.println("Some sound"); }
}

class Dog extends Animal {
    String sound = "Woof";

    void speak() {
        super.speak();              // calls Animal's speak()
        System.out.println(super.sound);  // accesses Animal's sound field
    }
}
```

> ⚠️ Unlike `this`, `super` **cannot** be used as a standalone reference or assigned to a variable.

---

## 5. Abstract Classes & Methods

- An **abstract class** is declared with the `abstract` keyword and **cannot be instantiated**.
- It can have **abstract methods** — methods with no body, meant to be implemented by subclasses.
- If a class has even one abstract method, the **class must also be declared abstract**.

```java
public abstract class Vehicle {
    abstract void move();       // no body — subclasses must implement this
    void fuel() { ... }         // concrete method — optional to override
}

class Car extends Vehicle {
    void move() { System.out.println("Car drives"); }  // must implement
}
```

- **Subclasses must** provide implementation for all abstract methods.

---

## 6. Final Declarations

The `final` keyword means **"cannot be changed"**.

### Final Classes — Cannot be Extended

```java
// java.lang.String is a final class
final class MyClass { }   // nobody can extend MyClass
```

### Final Methods — Cannot be Overridden

```java
class Parent {
    final void display() { }   // no subclass can override this
}
```

- Calls to `final` methods are decided at **compile time** (faster — no dynamic lookup).

### Final Variables — Cannot be Reassigned

```java
final int MAX = 100;        // primitive: value is fixed
final List<String> list = new ArrayList<>();  // reference is fixed, but contents can change
```

| Final On | Effect |
|----------|--------|
| **Class** | Cannot be extended (no subclasses) |
| **Method** | Cannot be overridden |
| **Primitive variable** | Value is fixed |
| **Reference variable** | Reference is fixed (object contents can still change) |

---

## 7. Interfaces

An **interface** is like a **contract** — it defines what a class must do, not how.

```java
public interface Flyable {
    void fly();         // abstract by default
    void land();
}

class Bird implements Flyable {
    public void fly()  { System.out.println("Bird flies"); }
    public void land() { System.out.println("Bird lands"); }
}
```

### Why Interfaces?

- Java allows **only one superclass** (single inheritance).
- But a class can **implement multiple interfaces** → this allows multiple inheritance of **type**.

### Key Rules

| Rule | Detail |
|------|--------|
| Keyword | `interface` |
| Access modifier | `public` or package (no modifier) |
| A class uses | `implements` |
| An interface extends another | `extends` |

---

## 8. Polymorphism

> *Poly = many, morphism = forms → **one thing, many behaviors***

- A **supertype reference** can point to objects of its **subtypes**.

```java
Shape s;
s = new Circle();       // Shape reference points to Circle
s = new Rectangle();    // Now points to Rectangle
s = new Square();       // Now points to Square
```

- The actual method that runs depends on the **real type** of the object at runtime (dynamic dispatch).

---

## 9. Enum Types

An **enum** is a special class that defines a **fixed set of named values**.

```java
enum Season { SPRING, SUMMER, AUTUMN, WINTER }

Season current = Season.SUMMER;
```

### Key Points

- Enum constants are like `public static final` objects of the enum type.
- Enums are also called **enum classes**.
- Enums can have **fields**, **constructors**, and **methods** too.

```java
enum Planet {
    MERCURY(3.303e+23, 2.4397e6),
    VENUS  (4.869e+24, 6.0518e6);

    private final double mass;
    private final double radius;

    Planet(double mass, double radius) {
        this.mass = mass;
        this.radius = radius;
    }
}
```

---

## 10. Record Classes

### The Problem

Sometimes you just need a class to hold data. Writing a full class requires a lot of repetitive code:
- Private fields
- Constructor
- Getter methods
- `equals()`, `hashCode()`, `toString()` methods

### The Solution — Records

A **record** is a shortcut for creating simple data-holding classes. The compiler **automatically generates** all the boilerplate code.

```java
record CD(String artist, String title, int noOfTracks, Year year, Genre genre) { }
```

The compiler generates: constructor, getters (`artist()`, `title()`, etc.), `equals()`, `hashCode()`, and `toString()` automatically.

### Key Features

| Feature | Detail |
|---------|--------|
| Fields | **Immutable** — cannot be changed after creation |
| Use case | Data transfer objects (POJOs) |
| Benefit | Less code, less bugs, efficient memory usage |

---

## 11. Sealed Classes and Interfaces

### The Problem

Normally, **anyone** can extend your class and create unexpected subclasses, which can break your carefully designed model.

### The Solution — Sealed Classes

A **sealed class** gives you control over **which classes can extend it**. Use the `permits` keyword.

```java
public abstract sealed class Book permits PrintedBook, Ebook, Audiobook { }
```

- Now **only** `PrintedBook`, `Ebook`, and `Audiobook` can extend `Book`.
- Any other class trying to extend `Book` gets a **compile-time error**.

### Sealed Interfaces

Same concept, but for interfaces. A sealed interface can have both:
- **Subclasses** that implement it
- **Subinterfaces** that extend it

```java
public sealed interface Subscribable permits Ebook, Audiobook, VIPSubscribable { }
```

### Summary

| Feature | Keyword | Purpose |
|---------|---------|---------|
| Sealed class | `sealed ... permits` | Restrict which classes can extend |
| Sealed interface | `sealed ... permits` | Restrict which types can implement/extend |
| Allowed subclass | Must be `final`, `sealed`, or `non-sealed` | Complete the hierarchy |

---

[← Previous Chapter](chapter-4-control-flow.md) | [Back to Index](../README.md) | [Next Chapter →](chapter-6-access-control.md)
