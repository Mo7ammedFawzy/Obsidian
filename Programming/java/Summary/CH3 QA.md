## 🔀 Statements & `if` / `else`

### Q1: What is a statement vs. a block in Java?

**A:**

- **Statement:** a complete unit of execution ending in `;`.
- **Block:** zero or more statements wrapped in `{ }` and treated as one statement.

---

### Q2: Does an `if` statement require braces?

**A:** No — for a single statement they're optional, but **always recommended**:

```java
if (x > 0)
    System.out.println("positive");
    count++;   // ALWAYS runs (NOT part of the if!)
```

---

### Q3: Why does `if (x = 5)` not compile?

**A:** The condition of an `if` must be a `boolean`. `x = 5` is an assignment that returns `int`. Java does not treat non-zero as `true` like C/C++.

---

### Q4: How does `else if` chaining work?

**A:** Conditions are evaluated **top-down** and execution stops at the first match.

```java
if (h < 11)        greeting = "Morning";
else if (h < 15)   greeting = "Afternoon";
else               greeting = "Evening";
```

---

## 🧩 Pattern Matching with `instanceof` (Java 16+)

### Q5: What is pattern matching with `instanceof`?

**A:** It combines a type check, a cast, and a binding into one expression:

```java
if (num instanceof Integer data) {
    System.out.println(data.intValue());
}
```

---

### Q6: What is *flow scoping*?

**A:** The pattern variable is in scope **only where the compiler can prove the type** — not strictly tied to braces.

```java
if (num instanceof Integer data) {
    // 'data' usable here
}
// 'data' NOT usable here
```

---

### Q7: Why does `if (i instanceof Integer data)` fail when `i` is already `Integer`?

**A:** The pattern is **redundant** — `i` is already known to be of that type. The compiler rejects redundant patterns.

---

## 🎚️ `switch` Statements

### Q8: Which data types are valid in a `switch`?

**A:** `byte`, `short`, `int`, `char` (and their wrappers), `String`, `enum`, and `var` if it resolves to one of those.

**Not allowed:** `boolean`, `long`, `float`, `double`.

---

### Q9: What are the rules for `case` values?

**A:**

- Must be a **compile-time constant** (literal or `final` variable initialized at declaration).
- Type must be compatible with the switch value.
- Cannot be `null`.
- Cannot duplicate values across cases.

---

### Q10: What is fall-through and how do you prevent it?

**A:** Without `break`, execution continues into subsequent cases. Use `break` (or a `return` / `throw`) to stop:

```java
switch (m) {
    case 1: System.out.println("Jan"); break;
    case 2: System.out.println("Feb"); break;
    default: System.out.println("?");
}
```

---

### Q11: Where can `default` appear in a `switch`?

**A:** Anywhere — beginning, middle, or end. It only runs if no `case` matched (and any preceding case fell through to it).

---

## ➡️ `switch` Expressions (Java 14+)

### Q12: What's the difference between a `switch` *statement* and a `switch` *expression*?

**A:**

|                 | Statement              | Expression                |
| --------------- | ---------------------- | ------------------------- |
| Returns a value | No                     | Yes                       |
| Syntax          | `case X:` + `break;`   | `case X ->`               |
| Fall-through    | Yes (without `break`)  | No                        |
| Exhaustive      | Optional               | **Required**              |
| Multiple labels | One per `case`         | `case 1, 2, 3 ->`         |

---

### Q13: Show a `switch` expression returning a value.

**A:**

```java
String type = switch (day) {
    case 0          -> "Sunday";
    case 1,2,3,4,5  -> "Weekday";
    case 6          -> "Saturday";
    default         -> "Unknown";
};
```

---

### Q14: What is `yield` used for?

**A:** To return a value from a **block-form** arrow case in a `switch` expression.

```java
int days = switch (m) {
    case 2 -> {
        boolean leap = (year % 4 == 0);
        yield leap ? 29 : 28;
    }
    default -> 30;
};
```

---

## 🔁 Loops

### Q15: Difference between `while` and `do-while`?

**A:**

- `while`: condition checked **before** body — may run **0 times**.
- `do-while`: condition checked **after** body — runs **at least once**.

---

### Q16: What are the three parts of a `for` loop?

**A:** `for (initialization; condition; update)`. All three are optional — `for(;;){}` is an infinite loop.

---

### Q17: Can a `for` loop declare two variables of different types?

**A:** No. The init section must declare variables of the **same type**:

```java
for (int i = 0, j = 10; i < j; i++, j--) { }   // OK
for (int i = 0, long j = 10; ...) { }          // DOES NOT COMPILE
```

---

### Q18: What is the enhanced `for` loop?

**A:** A simpler syntax for iterating arrays or `Iterable` collections:

```java
for (int n : nums) {
    System.out.println(n);
}
```

The right side **must** be an array or implement `java.lang.Iterable`.

---

## 🚦 Branching: `break`, `continue`, `return`, Labels

### Q19: What does `break` do?

**A:** Exits the **innermost** enclosing loop or `switch` immediately.

---

### Q20: What does `continue` do?

**A:** Skips the rest of the current iteration and re-tests the loop condition.

---

### Q21: How do labels work?

**A:** A label names a loop so `break` / `continue` can target an outer one:

```java
OUTER:
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (i == 1 && j == 1) break OUTER;
        if (j == 0)           continue OUTER;
    }
}
```

---

### Q22: What does `return` do inside a loop?

**A:** Exits the **entire method**, not just the loop.

---

### Q23: What is unreachable code?

**A:** Code that can never execute. The compiler rejects it:

```java
while (true) { }
System.out.println("done");   // DOES NOT COMPILE
```

---

## 💡 Exam Traps

### Q24: What's the output?

```java
int x = 0;
if (x == 0)
    System.out.println("A");
    System.out.println("B");
else
    System.out.println("C");
```

**A:** **DOES NOT COMPILE.** The unbraced `if` only attaches to the first statement; `else` then has no matching `if`.

---

### Q25: Will this compile?

```java
int hot = 7;
switch (temperature) {
    case hot: ...
}
```

**A:** **No.** `hot` is not `final` / not a compile-time constant. Either make it `final int hot = 7;` or use a literal.

---

### Q26: Can `break` or `continue` appear outside a loop / switch?

**A:** No — that's a compile-time error (except `break` is also valid inside `switch`).

---

### Q27: What's the result?

```java
int sum = 0;
for (int i = 0; i < 5; i++) {
    if (i == 3) continue;
    sum += i;
}
```

**A:** `sum = 0 + 1 + 2 + 4 = 7` (skips `i == 3`).

---

### Q28: Does a `switch` expression always need `default`?

**A:** It must be **exhaustive**. For an `enum` covering every constant, `default` can be omitted; for `int`/`String` you almost always need a `default`.

---

### Q29: How are multiple `case` labels combined?

**A:**

- *Statement form:* stacked `case` labels with no body ⇒ shared body via fall-through.
- *Expression form:* commas inside one `case` ⇒ `case 1, 2, 3 -> ...`.

---

### Q30: Quick comparison of decision constructs.

| Construct          | Tests Before? | Min Iterations | Use When                           |
| ------------------ | ------------- | -------------- | ---------------------------------- |
| `if / else`        | n/a           | n/a            | One-shot decision                  |
| `switch` statement | n/a           | n/a            | Many constant cases (may fall thru)|
| `switch` expr.     | n/a           | n/a            | Many cases returning a value       |
| `while`            | yes           | 0              | Unknown count, may skip            |
| `do-while`         | no            | 1              | At least one execution required    |
| `for`              | yes           | 0              | Counter-driven                     |
| `for-each`         | yes           | 0              | Iterating arrays / collections     |
