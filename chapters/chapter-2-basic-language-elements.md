# Chapter 2 — Basic Language Elements

[← Back to Index](../README.md)

---

## 2.1 Basic Language Elements

### Lexical Tokens

- The smallest pieces of a Java program are called **tokens** — like letters of the alphabet for code.
- Tokens include: identifiers (names), numbers, operators, and special characters.
- These are combined to build bigger things like expressions, methods, and classes.

### Identifiers (Names in Java)

- An **identifier** is a name given to classes, methods, variables, or labels.
- **Rules:** must start with a letter (not a digit). Can contain letters, digits, underscore `_`, or currency symbols (`$`, `£`, etc.).
- Java is **case-sensitive**: `price` and `Price` are two different names.
- The underscore alone (`_`) is **NOT** a valid identifier name.

| Legal ✅ | Illegal ❌ |
|----------|-----------|
| `number`, `Number`, `sum$` | `48chevy` (starts with digit) |
| `bingo`, `$$100`, `_007` | `all@hands` (`@` not allowed) |
| `mai`, `grüß` | `grand-sum` (`-` is an operator) |
| | `_` (underscore alone is illegal) |

### Keywords

- **Keywords** are special reserved words built into Java — you cannot use them as names.
- All Java keywords are **lowercase** (e.g., `class`, `int`, `for`, `while`, `if`, `else`, `return`).
- Using a keyword as an identifier causes a **compile-time error**.
- `strictfp` is obsolete from Java SE 17 — avoid in new code.
- **Contextual keywords** (like `var`, `sealed`, `record`) are only reserved in specific places.
- `null`, `true`, `false` are reserved literals — cannot be used as names.

### Separators (Punctuation Marks)

| Separator | Purpose |
|-----------|---------|
| `;` | Ends a statement |
| `{ }` | Groups statements into a block |
| `( )` | Method calls, conditions |
| `[ ]` | Array access/declaration |
| `,` | Separates items |
| `.` | Member access |
| `...` | Varargs |
| `@` | Annotations |
| `::` | Method reference |

### Literals (Fixed Values)

- A **literal** is a value written directly in code that never changes.

| Type | Examples |
|------|---------|
| Integer | `2000`, `-7` |
| Floating-point | `3.14`, `-3.14` |
| Character | `'a'`, `'0'` |
| Boolean | `true`, `false` |
| String | `"hello"` |
| Null | `null` |

### Integer Literals

- Default type of any whole number is `int`. To make it `long`, add `L` at the end: `100L`
- Use **uppercase L** (not lowercase `l`) — lowercase looks like digit `1`.

```java
// Four number systems:
27          // Decimal  (base 10)
0b1011      // Binary   (base 2)  — prefix 0b
033         // Octal    (base 8)  — prefix 0
0x1b        // Hex      (base 16) — prefix 0x
```

---

## 2.2 Primitive Data Types

Java has **8 built-in (primitive)** data types:

### Integral Types

| Type | Size | Range |
|------|------|-------|
| `byte` | 8 bits | -128 to 127 |
| `short` | 16 bits | -32,768 to 32,767 |
| `int` | 32 bits | ~-2.1 billion to ~2.1 billion *(default)* |
| `long` | 64 bits | Very large numbers |
| `char` | 16 bits | Unicode characters |

### Floating-Point Types

| Type | Size | Notes |
|------|------|-------|
| `float` | 32 bits | Less precise |
| `double` | 64 bits | More precise *(default)* |

### Boolean Type

- `boolean` — only two values: `true` or `false`.

> **Note:** Primitive values are NOT objects, but each has a wrapper class (e.g., `int` → `Integer`).

---

## 2.3 Precedence and Associativity Rules

### Operator Precedence — Which runs first?

- Higher precedence = runs first (like BODMAS in math).
- Use **parentheses** `()` to override the default order — they always win.

```java
2 + 3 * 4 = 2 + 12 = 14   // * has higher precedence than +
```

### Operator Associativity — Same precedence, then what?

| Associativity | Direction | Examples |
|---------------|-----------|---------|
| Left | Left → Right | `7 - 4 + 2 = (7 - 4) + 2 = 5` |
| Right | Right → Left | Assignment `=`, unary operators |

### Types of Operators by Operand Count

| Type | Operands | Examples |
|------|----------|---------|
| **Unary** | 1 | `++`, `--`, `+`, `-`, `~`, `!` |
| **Binary** | 2 | `+`, `-`, `*`, `/`, `%`, `==` |
| **Ternary** | 3 | `? :` |

### Key Rules Summary

- Postfix `++` and `--` have the **HIGHEST** precedence.
- Most binary operators are **left associative** (left to right).
- Assignment `=` and unary operators are **right associative** (right to left).
- Relational operators (`<`, `>`, `<=`, `>=`) are **non-associative** — cannot chain them like `a < b < c`.

---

## 2.7 The Simple Assignment Operator `=`

```java
variable = expression;
```

- Read as: *"variable gets the value of expression."* The old value is overwritten.
- Assignment has the **LOWEST** precedence — right side is fully calculated first.

```java
int j, k;
j = 0b10;        // j = 2
j = 5;           // j = 5  (old value gone)
k = j;           // k = 5

int i = 5;
i = i + 1;       // i = 6
i = 20 - i * 2;  // i = 8  (20 - (6*2) = 8)
```

---

## 2.8 Arithmetic Operators: `*`, `/`, `%`, `+`, `-`

| Operator | Meaning |
|----------|---------|
| `*` | Multiply |
| `/` | Divide |
| `%` | Remainder (modulo) |
| `+` | Add |
| `-` | Subtract |

- Operands must be **numeric** (includes `char` type).
- Floating-point math follows **IEEE-754** standard.

**Precedence order (highest → lowest):**
1. Unary `-x`
2. `*`, `/`, `%`
3. `+`, `-`

---

## 2.9 Increment (`++`) and Decrement (`--`) Operators

- `++` adds 1. `--` subtracts 1.
- Cannot be used on a `final` variable.

### Prefix vs Postfix

```java
int i = 10;
int result = ++i;  // PREFIX:  i becomes 11 first, result = 11

int j = 10;
long n = j++;      // POSTFIX: n gets old value 10, then j becomes 11
```

> ⚠️ **Key Rule:** Avoid writing expressions where the same variable is changed multiple times — results can be unpredictable.

---

## 2.10 Boolean Expressions

- A **boolean expression** gives either `true` or `false`.
- Used in `if` statements, loops, and other control structures.
- Built using: relational, equality, logical, conditional, and `instanceof` operators.

---

## 2.11 Relational Operators: `<`, `<=`, `>`, `>=`

| Operator | Meaning |
|----------|---------|
| `a < b` | Is a less than b? |
| `a <= b` | Is a less than or equal to b? |
| `a > b` | Is a greater than b? |
| `a >= b` | Is a greater than or equal to b? |

- Result is always `true` or `false`.
- **NON-associative** — `a <= b <= c` is illegal. Use `a <= b && b <= c`.

```java
double hours = 45.5;
boolean overtime = hours >= 35;   // true

char letterA = 'A';
boolean order = letterA < 'a';    // true (compares char values)
```

---

## 2.12 Equality Operators: `==`, `!=`

### For Primitive Values

```java
int year = 2002;
boolean isEven = year % 2 == 0;   // true

boolean compare = '1' == 1;       // false (binary numeric promotion applied)
```

### For Object References

- `==` checks if both variables point to the **SAME object in memory** (not same content).

```java
Pizza pizzaA = new Pizza("Sweet&Sour");
Pizza pizzaB = new Pizza("Sweet&Sour");

boolean test1 = pizzaA == pizzaB;  // false (different objects, same content)

pizzaA = pizzaB;
boolean test2 = pizzaA == pizzaB;  // true (now both point to same object)
```

> ⚠️ `==` on objects does **NOT** compare content. Use `.equals()` for content comparison.

---

## 2.13 Conditional Operators: `&&`, `||`

| Operator | Meaning | Result |
|----------|---------|--------|
| `&&` | AND | `true` only if **both** sides are true |
| `\|\|` | OR | `true` if **either** side is true |

### Truth Table

| A | B | A && B | A \|\| B |
|---|---|--------|--------|
| true | true | true | true |
| true | false | false | true |
| false | true | false | true |
| false | false | false | false |

### Short-Circuit Evaluation

- **`&&`:** If LEFT side is `false` → right side is **skipped** (already false).
- **`||`:** If LEFT side is `true` → right side is **skipped** (already true).
- This saves time and prevents errors (e.g., avoids `NullPointerException`).

---

## 2.14 Shift Operators: `<<`, `>>`, `>>>`

| Operator | Name | Effect |
|----------|------|--------|
| `v << n` | Left shift | Move bits left; fill right with 0s. Equivalent to `v × 2ⁿ` |
| `v >> n` | Signed right shift | Move bits right; copy sign bit from left |
| `v >>> n` | Unsigned right shift | Move bits right; fill left with 0s (always positive result) |

```java
int i = 12;
int result = i << 4;  // 12 × 16 = 192
```

> **Note:** Result is always `int` or `long`. Assigning to a narrower type requires a cast.

---

[← Previous Chapter](chapter-1-introduction-to-java.md) | [Back to Index](../README.md) | [Next Chapter →](chapter-3-declarations.md)
