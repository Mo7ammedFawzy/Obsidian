# CH3 — Making Decisions: Q & A

---

### Q1. Which type is **NOT** allowed in a classic `switch`?

**Options**
- A) `int`
- B) `String`
- C) `long`
- D) `enum`

**Correct Answer:** **C**

**Explanation**
`long`, `boolean`, `float`, `double` (and their wrappers) are not allowed. `int`, `String`, `enum`, `byte`, `short`, `char` are.

---

### Q2. Output?

```java
int x = 0;
if (x = 1) System.out.println("yes"); else System.out.println("no");
```

**Options**
- A) `yes`
- B) `no`
- C) Compile error
- D) Runtime error

**Correct Answer:** **C**

**Explanation**
`x = 1` is an assignment producing `int`, but `if` requires a `boolean`. Java does not treat non-zero as `true`.

---

### Q3. Output?

```java
int n = 2;
switch (n) {
    case 1: System.out.println("one");
    case 2: System.out.println("two");
    case 3: System.out.println("three");
    default: System.out.println("?");
}
```

**Options**
- A) `two`
- B) `two three ?`
- C) `two three`
- D) `?`

**Correct Answer:** **B**

**Explanation**
No `break` ⇒ fall-through from case 2 onwards prints `two`, `three`, and `?`.

---

### Q4. (True/False) `null instanceof String` returns `true`.

**Correct Answer:** **False**

**Explanation**
`instanceof` always returns `false` for `null`, regardless of the type.

---

### Q5. Output?

```java
String label = switch (3) {
    case 1, 2, 3 -> "small";
    case 4, 5, 6 -> "medium";
    default -> "?";
};
System.out.println(label);
```

**Options**
- A) `small`
- B) `medium`
- C) `?`
- D) Compile error

**Correct Answer:** **A**

**Explanation**
Switch expression with arrow form and multiple labels per case. `3` matches the first arm.

---

### Q6. Which is invalid?

**Options**
- A) `for (int i=0, j=10; i<j; i++, j--) {}`
- B) `for (;;) { break; }`
- C) `for (int i=0, long j=0; i<10; i++) {}`
- D) `for (int i=0; ; i++) {}`

**Correct Answer:** **C**

**Explanation**
The `for` init clause may declare multiple variables, but they must share the same type. **B** is an infinite loop with a break (legal). **D** has no condition (always true).

---

### Q7. Output?

```java
int sum = 0;
for (int i = 1; i <= 5; i++) {
    if (i == 3) continue;
    sum += i;
}
System.out.println(sum);
```

**Options**
- A) `15`
- B) `12`
- C) `6`
- D) `10`

**Correct Answer:** **B**

**Explanation**
Adds `1+2+4+5 = 12` (skipping 3).

---

### Q8. (True/False) A `do-while` may execute the body zero times.

**Correct Answer:** **False**

**Explanation**
`do-while` always runs the body at least once before evaluating the condition.

---

### Q9. Output?

```java
int n = 10;
final int FINAL_C = 5;
switch (n) {
    case 1: System.out.println("one"); break;
    case FINAL_C: System.out.println("five"); break;
    default: System.out.println("?");
}
```

**Options**
- A) `?`
- B) `five`
- C) `one`
- D) Compile error

**Correct Answer:** **A**

**Explanation**
`FINAL_C` is a compile-time constant (legal label). None match `10`, so `default` runs.

---

### Q10. Which compiles?

**Options**
- A) `if (x > 0) System.out.println("a"); System.out.println("b"); else System.out.println("c");`
- B) `if (x > 0) { System.out.println("a"); } else { System.out.println("c"); }`
- C) `if x > 0 then System.out.println("a");`
- D) `if (x > 0) ; ; else System.out.println("c");`

**Correct Answer:** **B**

**Explanation**
Standard `if/else` with braces. **A** has a stray statement before `else`. **C** uses `then` (not Java). **D** is malformed.

---

### Q11. Output?

```java
Object o = "hi";
if (o instanceof String s) System.out.println(s.length());
```

**Options**
- A) `0`
- B) `2`
- C) Compile error
- D) Runtime error

**Correct Answer:** **B**

**Explanation**
Pattern variable `s` is bound to `"hi"`; `length()` is `2`.

---

### Q12. (True/False) A `switch` expression must always have a `default` branch.

**Correct Answer:** **False**

**Explanation**
For an `enum`/sealed type whose every constant/subtype is covered, `default` may be omitted. For `int`/`String`, you almost always need it.

---

### Q13. Output?

```java
OUTER:
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (i == 1 && j == 1) break OUTER;
        System.out.print(i + "" + j + " ");
    }
}
```

**Options**
- A) `00 01 02 10 `
- B) `00 01 02 10 11 12 `
- C) `00 01 02 `
- D) `00 01 02 10 11 `

**Correct Answer:** **A**

**Explanation**
When `i == 1 && j == 1` we break the outer loop; previously printed: `00 01 02 10`.

---

### Q14. Which is a compile error?

```java
while (true) {
    System.out.println("loop");
}
System.out.println("after");
```

**Options**
- A) The `println("loop")`
- B) The `while`
- C) The `println("after")`
- D) None — this compiles

**Correct Answer:** **C**

**Explanation**
The infinite loop has no `break`, making the line after unreachable.

---

### Q15. Output?

```java
int x = 5;
String r = switch (x) {
    case 5 -> { yield "five"; }
    default -> "?";
};
System.out.println(r);
```

**Options**
- A) `five`
- B) `?`
- C) Compile error
- D) Empty

**Correct Answer:** **A**

**Explanation**
`yield` returns a value from a block-form arrow case in a switch expression.

---

### Q16. (True/False) In a switch statement on a `String`, comparison uses `==`.

**Correct Answer:** **False**

**Explanation**
String/enum cases compare via `equals`. `==` would be unreliable for non-interned strings.

---

### Q17. Output?

```java
int n = 0;
do {
    System.out.print(n + " ");
    n++;
} while (n < 3);
```

**Options**
- A) `0 1 2 `
- B) `1 2 3 `
- C) Empty
- D) Compile error

**Correct Answer:** **A**

**Explanation**
Body runs at least once, then increments. Loop runs for `n = 0, 1, 2`.

---

### Q18. Output?

```java
int sum = 0;
outer:
for (int i = 1; i <= 3; i++) {
    for (int j = 1; j <= 3; j++) {
        if (j == 2) continue outer;
        sum += j;
    }
}
System.out.println(sum);
```

**Options**
- A) `6`
- B) `3`
- C) `9`
- D) `12`

**Correct Answer:** **B**

**Explanation**
Each outer iteration adds `j=1`, then `continue outer` skips the rest. `1+1+1 = 3`.

---

### Q19. Which switch on `null` is correct in Java 21?

**Options**
- A) `case null:`
- B) `case "null":`
- C) `case null ->`
- D) Both **A** and **C** depending on switch form

**Correct Answer:** **D**

**Explanation**
Java 21 pattern-matching switch supports `case null` in both statement (`:`) and expression (`->`) forms; the value `null` matches that case rather than throwing NPE.

---

### Q20. (True/False) `for (int x : new int[0]) {}` runs the body zero times.

**Correct Answer:** **True**

**Explanation**
The enhanced `for` over an empty array doesn't execute the body. Same for an empty `Iterable`.
