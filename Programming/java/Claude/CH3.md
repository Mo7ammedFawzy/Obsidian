# CH3 — Making Decisions

## 1) Overview

Java's control flow comes in three flavors: **decision statements** (`if`, `switch`), **loop statements** (`while`, `do-while`, `for`, enhanced `for`), and **branching statements** (`break`, `continue`, `return`, with optional labels). Java 14+ added **switch expressions**, and Java 16+ added **pattern matching** with `instanceof`. The exam tests subtleties: case-label rules, fall-through, label scope, exhaustiveness, and unreachable-code errors.

---

## 2) Key Concepts

### `if` / `else`

```java
if (cond1) { ... }
else if (cond2) { ... }
else { ... }
```

- The condition must be a `boolean`.
- Braces are optional for a single statement (always recommended).

### Pattern Matching with `instanceof` (Java 16+)

```java
if (obj instanceof Integer i) {
    System.out.println(i.intValue());
}
```

- The pattern variable is in scope under **flow scoping**: only where the compiler can prove the type.
- `null instanceof T` is always `false`.
- Redundant patterns (target already known to match) fail to compile.

### `switch` Statement

```java
switch (value) {
    case 1: ...; break;
    case 2:
    case 3: ...; break;
    default: ...;
}
```

- Allowed types: `byte short int char` and their wrappers, `String`, `enum`, and `var` if the inferred type is one of these.
- **NOT** allowed: `boolean long float double`.
- Case labels must be **compile-time constants**.

### Fall-through

Without `break`, execution continues into the next case. Useful for shared logic, dangerous if forgotten.

### `switch` Expression (Java 14+)

```java
String type = switch (day) {
    case 1, 2, 3, 4, 5 -> "weekday";
    case 6, 7          -> "weekend";
    default            -> "?";
};
```

- Returns a value; ends with `;` after `}`.
- Arrow syntax: no fall-through.
- Multiple labels per case: `case 1, 2, 3 ->`.
- Must be **exhaustive** (cover every possible value).
- Use `yield` to return from a block branch.

### Loops

```java
while (cond) { ... }
do { ... } while (cond);
for (init; cond; update) { ... }
for (Type x : iterableOrArray) { ... }
```

- `for(;;)` is an infinite loop.
- The init section may declare multiple variables of the **same type**.
- The enhanced `for` requires `Iterable<T>` or an array.

### Branching

- `break;` exits the innermost loop / switch.
- `continue;` skips to the next iteration's condition.
- `return;` exits the method entirely.
- **Labels** target outer loops:

```java
OUTER:
for (...) {
    for (...) {
        if (x) break OUTER;
        if (y) continue OUTER;
    }
}
```

### Unreachable Code

The compiler rejects code that can never execute (after `while(true)` without break, `return`, `throw`, etc.).

---

## 3) Important Rules

- The `if` condition must be `boolean` — `if (x = 5)` and `if (1)` don't compile.
- A pattern variable from `instanceof` is **flow-scoped** — its visibility depends on control flow.
- Case labels in a `switch` must be compile-time constants of the same type as the switch value.
- A `case` label cannot be `null` (in classic switch). Pattern-matching `switch` (Java 21) allows `case null`.
- A `switch` value may be `null` only if the cases include `null` patterns; otherwise `NullPointerException`.
- `boolean`, `long`, `float`, `double` (and their wrappers) are not allowed in classic `switch`.
- `switch` expressions must be **exhaustive**; for `enum` you may omit `default` if all constants are covered.
- `yield` returns a value from a block in a `switch` expression. Inside `case X -> { ... }` blocks, you can't use `return` (that would exit the enclosing method, not the switch).
- A `break label;` must reference a label declared on an enclosing loop or block.
- `do-while` always runs the body at least once.
- `for(;;)` is legal — the missing condition is implicitly `true`.
- The init section of `for` declares only one type: `for (int i=0, j=1; ...)` is fine, but `for (int i=0, long j=0; ...)` is not.
- `continue` in a `for` loop jumps to the **update** step, then re-tests the condition.
- Code after a `while(true) {}` without a reachable break is **unreachable**.
- In an enhanced `for`, the right-hand expression must be an array or implement `Iterable`.

---

## 4) Code Examples

### Pattern matching with flow scoping

```java
Object o = 42;
if (o instanceof Integer i && i > 0) {
    System.out.println("positive int: " + i);
}
// 'i' not in scope here
```

### Classic switch fall-through

```java
int m = 2;
switch (m) {
    case 1:
    case 2:
    case 3:
        System.out.println("Q1");
        break;
    case 4:
        System.out.println("Q2 start");
        // no break: falls through
    default:
        System.out.println("else");
}
// prints: Q1
```

### Switch expression with yield

```java
int days = switch (month) {
    case 1, 3, 5, 7, 8, 10, 12 -> 31;
    case 4, 6, 9, 11 -> 30;
    case 2 -> {
        boolean leap = (year % 4 == 0);
        yield leap ? 29 : 28;
    }
    default -> throw new IllegalArgumentException();
};
```

### Pattern-matching switch (Java 21)

```java
Object o = 5;
String s = switch (o) {
    case null      -> "null";
    case Integer i -> "int " + i;
    case String x  -> "str " + x;
    default        -> "?";
};
```

### Labeled break

```java
OUTER:
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (i == 1 && j == 1) break OUTER;
    }
}
```

### Tricky: dangling-else

```java
if (a)
if (b) System.out.println("ab");
else System.out.println("not b");   // attaches to the inner if
```

### Tricky: case label must be constant

```java
final int FINAL_C = 5;
int hot = 7;
switch (n) {
    case 1: break;
    case FINAL_C: break;     // OK — compile-time constant
    case hot: break;         // ❌ not constant
}
```

### Tricky: unreachable code

```java
while (true) {
    if (done) break;
    // ... loop work
}
System.out.println("after");      // OK because of break

while (true) { }
System.out.println("never");      // ❌ unreachable
```

---

## 5) Common Mistakes

- ❌ Forgetting `break` and getting accidental fall-through.
- ❌ Using a non-final variable as a case label.
- ❌ Switching on a `boolean` (not allowed) or `long` (not allowed).
- ❌ Treating `if (x = 5)` as a comparison (it's an assignment).
- ❌ Confusing `==` with `equals` inside switch — switch compares using `equals` for `String`/`enum`.
- ❌ Putting a `return` inside a `case X -> { ... }` block expecting it to exit the switch (it exits the method).
- ❌ Forgetting to handle `null` in a switch on a wrapper or `String` (NPE).
- ❌ Declaring two types in a `for` init (`int i, long j`) — illegal.
- ❌ Modifying the loop variable inside an enhanced `for` and expecting iteration to change.
- ❌ Putting unreachable code after an infinite loop.

---

## 6) Mental Model

Decisions and loops are the **flow control verbs** of Java. Keep three categories straight:

- **Decisions**: `if/else`, `switch`. They pick a path *once*.
- **Loops**: `while`, `do-while`, `for`. They repeat until a condition fails.
- **Branching**: `break`, `continue`, `return`, with optional labels. They modify the default flow.

For switches, think of two flavors:
- **Classic statement** (with `:`): can fall through, no value, allows side effects.
- **Modern expression** (with `->`): no fall-through, returns a value, must be exhaustive.

Pattern matching with `instanceof` and the `switch` patterns of Java 21 unify type checks, casts, and bindings into one ergonomic expression — fewer redundant casts, fewer `NullPointerException`s.

The compiler enforces **reachability**: every statement must be possibly reached. This is why `while(true)` without a `break` poisons everything that follows it.

---

## 7) Quick Revision

- `if` requires `boolean`; assignments don't count.
- `instanceof T t` binds `t` under flow scoping.
- Switch on: `byte short char int` (+ wrappers), `String`, `enum`, `var`.
- Switch labels must be compile-time constants.
- `switch` expression: arrow form, no fall-through, exhaustive, `yield` from blocks.
- `do-while` runs the body at least once; `for(;;)` is infinite.
- `continue` in a `for` jumps to **update** then **condition**.
- Labels target outer loops/blocks via `break label` / `continue label`.
- Pattern-matching `switch` (Java 21) supports `case null`, type patterns, and `when` guards.
- The compiler rejects unreachable code.
