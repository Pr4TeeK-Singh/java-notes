# Chapter 4 — Control Flow

[← Back to Index](../README.md)

---

## Overview

Control flow statements decide the **order** in which code runs.

| Category | Statements | Purpose |
|----------|-----------|---------|
| **Selection** | `if`, `if-else`, `switch` | Choose between options |
| **Iteration (Loops)** | `while`, `do-while`, `for`, enhanced `for` | Repeat code |
| **Transfer** | `break`, `continue`, `return`, `yield` | Jump to another part of code |

---

## 4.1 Selection Statements

### Simple `if` Statement

Run some code **only if** a condition is true.

```java
if (condition)
    statement;

// Examples
if (emergency)
    operate();

if (temperature > critical)
    soundAlarm();
```

- If condition is `true` → run the statement/block.
- If condition is `false` → skip it.
- The condition **must** give a `boolean` result.

### The `if-else` Statement

Do **one thing** if true, a **different thing** if false.

```java
if (condition)
    statement1;
else
    statement2;

// Example
if (emergency)
    operate();
else
    joinQueue();
```

### Nested `if` and the Dangling Else Problem

- `if` statements can be placed inside other `if` statements — called **nesting**.
- **Golden Rule:** An `else` always belongs to the **nearest `if`** above it that doesn't already have an `else`.
- Always use curly braces `{ }` to make the code clear and avoid bugs.

### Cascading `if-else` (`if-else if-else`)

Check **multiple conditions** one after another.

```java
if (temperature >= upperLimit)
    soundAlarm();
else if (temperature < lowerLimit)
    soundAlarm();
else if (temperature == (upperLimit - lowerLimit) / 2)
    doingFine();
else
    noCauseToWorry();
```

- Java checks each condition **in order**.
- The **first** one that is `true` gets executed; the rest are **skipped**.
- If **none** match and there is a final `else`, that block runs.

---

## 4.2 The `switch` Statement

Used when you have one variable and want to check it against **many possible values**.

```java
switch (value) {
    case 1:
        System.out.println(1);
        break;
    case 2:
        System.out.println(2);
        break;
    default:
        System.out.println("other");
}
```

- `break` at the end of each case stops **fall-through** to the next case.
- `default` runs if no case matches — like the final `else`.
- Without `break`, execution **falls through** to the next case automatically.

---

## 4.3 Iteration Statements (Loops)

Loops let you run the same code **multiple times** without rewriting it.

| Loop | Condition Checked | Minimum Runs |
|------|------------------|--------------|
| `while` | Before each run | 0 |
| `do-while` | After each run | 1 (always) |
| `for (;;)` | Before each run | 0 |
| `for (:)` | Enhanced for-each | Varies |

---

## 4.4 The `while` Loop

Checks condition **first**. Repeats until condition becomes `false`.

```java
while (loop_condition)
    loop_body;

// Example
while (noSignOfLife())
    keepLooking();
```

- If condition is `false` from the start → loop body **never runs** (zero iterations possible).
- Best used when you **do NOT know** in advance how many times to loop.

---

## 4.5 The `do-while` Loop

Runs the loop body **first**, then checks the condition. Always runs **at least once**.

```java
do {
    loop_body;
} while (loop_condition);   // Note: semicolon required here!
```

| Loop | Guarantee |
|------|-----------|
| `while` | May run 0 times |
| `do-while` | Always runs **at least once** |

---

## 4.6 The `break` Statement

`break` **immediately exits** the current loop or `switch` block.

```java
break;        // unlabeled — exits current loop/switch
break label;  // labeled — exits the loop named 'label'
```

### Labeled `break` — Exiting Outer Loops

```java
outer: for (int i = 0; i < 5; i++) {       // 'outer' is the label
    for (int j = 0; j < 5; j++) {
        if (j == 2) break outer;            // exits the outer loop entirely
    }
}
```

- **Unlabeled break** — exits the **innermost** loop or switch only.
- **Labeled break** — exits a **specific outer loop** by name.
- Useful when nested loops need to exit an outer loop from inside an inner loop.
- A statement can have **multiple labels**. A declaration statement cannot have a label.

---

## 4.7 The `continue` Statement

`continue` **skips the rest** of the current iteration and jumps to the **next iteration**.

```java
continue;        // unlabeled
continue label;  // labeled — skips to next iteration of 'label' loop
```

| Loop Type | Where `continue` Jumps |
|-----------|----------------------|
| `while` / `do-while` | To the **condition check** |
| `for (;;)` | To the **update expression** (e.g., `i++`), then condition |

### `break` vs `continue`

| Statement | Effect |
|-----------|--------|
| `break` | **EXITS** the loop completely |
| `continue` | **SKIPS** this turn → goes to the **next turn** |

---

## 4.8 The `return` Statement

`return` **stops a method** and gives control back to the caller.

```java
return;            // stops the method, returns nothing (void methods)
return expression; // stops the method AND sends back a value
```

| Form | In void / constructor | In non-void method |
|------|-----------------------|-------------------|
| `return;` | Optional | Not allowed |
| `return expression;` | Not allowed | Mandatory (unless exception thrown) |

- The value returned **must match** the method's declared return type.
- A `void` method automatically returns at the last line — `return;` can be used to exit **early**.
- Best practice: document the return value in Javadoc using the `@return` tag.

```java
/**
 * Calculates the area of a circle.
 * @param radius the radius of the circle
 * @return the area as a double
 */
public double circleArea(double radius) {
    return Math.PI * radius * radius;
}
```

---

[← Previous Chapter](chapter-3-declarations.md) | [Back to Index](../README.md) | [Next Chapter →](chapter-5-oops.md)
