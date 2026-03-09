# Chapter 3 — Declarations

[← Back to Index](../README.md)

---

## 3.1 Class Declarations

- A class declaration creates a new **reference type** — a blueprint for objects.

```java
class ClassName extends ParentClass implements Interface {
    // fields, methods, constructors, member types
}
```

- The **only mandatory parts** are: keyword `class`, class name, and curly brackets `{ }`.
- **Class modifiers:** `public`, `protected`, `private`, `abstract`, `final`, `static`, `sealed`, `non-sealed`.

### Static Context vs Non-Static Context

| Context | Belongs To | Can Access |
|---------|-----------|------------|
| Static | The **class** (static methods, fields, blocks) | Only static members |
| Non-static | **Objects** (instance methods, fields, constructors) | Everything |

---

## 3.2 Method Declarations

```java
modifiers returnType methodName(parameters) throws Exception {
    // method body
}
```

- **Return type:** what the method gives back. Use `void` if it gives back nothing.
- A **non-void** method **MUST** have a `return` statement.
- **Method signature** = method name + parameter types (NOT the return type).

```java
// Example
double cubeVolume(double length, double width, double height) { }
// Signature: cubeVolume(double, double, double)
```

### Access Modifiers
`public`, `protected`, `private`

### Non-Access Modifiers
`static`, `abstract`, `final`, `synchronized`

> **Note:** Cannot use `var` as a parameter type.

---

## 3.3 Statements

- A **statement** is one complete instruction in Java, usually ending with `;`.

| Type | Description |
|------|-------------|
| Declaration statement | Declares a variable |
| Control flow statement | `if`, `for`, `while`, etc. |
| Expression statement | An expression followed by `;` |

- A **lone semicolon** `;` is an empty statement — does nothing.
- A **block** `{ }` groups multiple statements and acts as a single compound statement. Blocks can be nested.

---

## 3.4 Variable Declarations

- A variable stores a value of a specific type. It has a **name**, a **type**, and a **value**.
- Two kinds: **field variables** (in classes/interfaces/enums) and **local variables** (in methods/constructors/blocks).

### Declaring and Initializing Variables

```java
char a, b, c;           // declare three char variables
double area;            // one double variable
boolean flag;           // one boolean variable

int i = 10, j = 0b101; // declare and initialize (i=10, j=5)
long big = 2147483648L;
```

### Reference Variables

```java
Pizza myPizza;          // declares a reference — does NOT create an object
```

- A reference variable stores the **address** of an object — not the object itself.
- A field reference is automatically set to `null` if not initialized.
- **Local** reference variables must be **explicitly initialized** before use.

---

## 3.5 Method Overloading

- **Overloading** = multiple methods with the **same name** but **different parameter lists**.
- Java picks the right version to call based on the arguments you pass.
- The **return type alone** cannot differentiate overloaded methods.

```java
// All named min(), different parameter types
public static double min(double a, double b)
public static float  min(float a,  float b)
public static int    min(int a,    int b)
public static long   min(long a,   long b)
```

> ⚠️ Two methods with the same name AND same parameter types = **compile-time error** (not valid overloading).

---

## 3.6 Arrays

- An **array** is a container that holds a **fixed number** of values all of the **same type**.
- Each value is accessed by its **index** (starts at **0**).
- Array size is **fixed** — you cannot change it after creation.
- Arrays are **objects** in Java and have a `length` field.

### Declaring Array Variables

```java
int[] anIntArray;                       // preferred style
int anIntArray[];                       // also valid

Pizza[] mediumPizzas, largePizzas;      // both are Pizza arrays
```

> Declaration alone does **NOT** create the array — just creates a reference.

### Constructing (Creating) an Array

```java
anIntArray   = new int[10];   // array of 10 integers — default value: 0
mediumPizzas = new Pizza[5];  // array of 5 Pizza refs — default value: null
```

| Type | Default Value |
|------|---------------|
| Numbers (`int`, `double`, etc.) | `0` |
| `boolean` | `false` |
| Object references | `null` |

- Minimum size is 0 (zero-length array is valid).
- Negative size throws `NegativeArraySizeException`.
- Size is stored in the `length` field: `mediumPizzas.length` → `5`

### Initializing an Array (Shortcut)

```java
int[] anIntArray = {13, 49, 267, 15, 215};              // size = 5 automatically
Pizza[] pizzaOrder = { new Pizza(), new Pizza(), null };
```

### Using Array Elements

```java
anIntArray[0]   // first element
anIntArray[4]   // fifth element (last in a size-5 array)
```

> ⚠️ If the index is out of bounds → `ArrayIndexOutOfBoundsException` at runtime.

---

## 3.7 The `main()` Method

- The `main()` method is the **entry point** of a Java program — where execution begins.
- The JVM looks for `main()` in the class specified on the command line.

```java
public static void main(String[] args)    // valid
public static void main(String args[])   // also valid
public static void main(String... args)  // also valid (varargs)
```

### Required Characteristics

| Modifier | Reason |
|----------|--------|
| `public` | JVM must be able to call it |
| `static` | No object needed to call it |
| `void` | Returns nothing |

- The three modifiers (`public`, `static`, `void`) can appear in **any order**.
- `main()` can be **overloaded**, but JVM only calls the one with `String[]` parameter.

### Program Arguments

```bash
java Colors red yellow green "blue velvet"
```

| Index | Value |
|-------|-------|
| `args[0]` | `"red"` |
| `args[1]` | `"yellow"` |
| `args[2]` | `"green"` |
| `args[3]` | `"blue velvet"` |

- Arguments are separated by **spaces**.
- To include spaces in one argument, wrap it in **double quotes**.
- If no arguments are given, `args` is a `String` array of length 0 — it is **NEVER null**.

---

[← Previous Chapter](chapter-2-basic-language-elements.md) | [Back to Index](../README.md) | [Next Chapter →](chapter-4-control-flow.md)
