# CH2 — Operators: Q & A

---

### Q1. Output?

```java
int a = 1;
int b = a++ + ++a;
System.out.println(a + " " + b);
```

**Options**
- A) `2 2`
- B) `3 3`
- C) `3 4`
- D) `2 3`

**Correct Answer:** **C**

**Explanation**
`a++` uses 1 then sets `a=2`. `++a` sets `a=3` and uses 3. `b = 1 + 3 = 4`. Final `a = 3`.

---

### Q2. (True/False) `byte b = 10 + 20;` compiles successfully.

**Correct Answer:** **True**

**Explanation**
Both operands are compile-time constants; the compiler evaluates them and verifies the result fits in `byte`.

---

### Q3. Output?

```java
byte a = 10;
byte b = 20;
byte c = a + b;
```

**Options**
- A) `30`
- B) `0`
- C) Compile error
- D) Throws

**Correct Answer:** **C**

**Explanation**
`a + b` promotes to `int`. Assigning `int` to `byte` requires an explicit cast.

---

### Q4. Which evaluates `true`?

**Options**
- A) `null instanceof Object`
- B) `"x" instanceof String`
- C) `1 == 1.0`  *(applies)*
- D) Both **B** and **C**

**Correct Answer:** **D**

**Explanation**
`null instanceof T` is always `false`. `"x" instanceof String` is `true`. `1 == 1.0` promotes `int` to `double` → `true`.

---

### Q5. Output?

```java
System.out.println(0.0 / 0.0);
```

**Options**
- A) `0.0`
- B) `Infinity`
- C) `NaN`
- D) Throws

**Correct Answer:** **C**

**Explanation**
Floating-point divide of 0/0 yields `NaN`. Integer 0/0 would throw `ArithmeticException`.

---

### Q6. (True/False) `Integer.valueOf(1000) == Integer.valueOf(1000)` returns `true`.

**Correct Answer:** **False**

**Explanation**
Only values in `[-128, 127]` are cached; `1000` produces two separate objects. Use `.equals()`.

---

### Q7. Which evaluates `false`?

**Options**
- A) `5 / 2 == 2`
- B) `5.0 / 2 == 2.5`
- C) `5 % 2 == 1`
- D) `(byte) 200 == 200`

**Correct Answer:** **D**

**Explanation**
`(byte)200` overflows to `-56`. `-56 == 200` is `false`. The integer is widened back to `int` before comparison, so it's literally `-56 == 200`.

---

### Q8. Output?

```java
String s = null;
boolean ok = (s != null && s.length() > 0);
System.out.println(ok);
```

**Options**
- A) `true`
- B) `false`
- C) NPE
- D) Compile error

**Correct Answer:** **B**

**Explanation**
`&&` short-circuits when the left side is false, so `s.length()` is never evaluated. With `&` instead, you'd get an NPE.

---

### Q9. Which is the correct precedence order (high → low)?

**Options**
- A) `+ - > * / % > == != > && > ||`
- B) `* / % > + - > == != > && > ||`
- C) `+ - > == != > * / % > || > &&`
- D) `+ - > == != > && > || > * / %`

**Correct Answer:** **B**

**Explanation**
Multiplicative beats additive, beats relational/equality, beats logical short-circuit, with `&&` higher than `||`.

---

### Q10. Output?

```java
int x = -1;
System.out.println(x >>> 28);
```

**Options**
- A) `-1`
- B) `15`
- C) `0`
- D) `1`

**Correct Answer:** **B**

**Explanation**
`-1` is `0xFFFFFFFF`. `>>>` is logical (zero-fill). Shifting 28 bits leaves `0x0000000F = 15`.

---

### Q11. (True/False) `boolean` can be cast to `int` in Java.

**Correct Answer:** **False**

**Explanation**
`boolean` is not a numeric type; no cast to/from numeric types is allowed.

---

### Q12. What's `result`?

```java
int n = 0;
int result = (n != 0) ? 100 / n : -1;
```

**Options**
- A) Throws ArithmeticException
- B) `-1`
- C) `0`
- D) Compile error

**Correct Answer:** **B**

**Explanation**
`n != 0` is `false` so the division is never evaluated. The ternary returns `-1`.

---

### Q13. Output?

```java
short s = 100;
s += 50;
System.out.println(s);
```

**Options**
- A) `150`
- B) Compile error
- C) `100`
- D) Runtime error

**Correct Answer:** **A**

**Explanation**
Compound assignment includes an implicit cast back to `short`. `s = s + 50` would have failed because `int` can't be assigned to `short` without a cast.

---

### Q14. Which is **NOT** a short-circuit operator?

**Options**
- A) `&&`
- B) `||`
- C) `&`
- D) None of the above are short-circuit

**Correct Answer:** **C**

**Explanation**
`&` evaluates both sides regardless of the left value.

---

### Q15. Output?

```java
double d = Double.NaN;
System.out.println(d == d);
System.out.println(Double.isNaN(d));
```

**Options**
- A) `true true`
- B) `false true`
- C) `true false`
- D) `false false`

**Correct Answer:** **B**

**Explanation**
`NaN` compares unequal to itself by IEEE-754. Always use `Double.isNaN(...)`.

---

### Q16. (True/False) `if (true | flag())` always calls `flag()`.

**Correct Answer:** **True**

**Explanation**
`|` is bitwise/eager OR — it evaluates both operands. `||` would short-circuit and skip `flag()`.

---

### Q17. Output?

```java
String a = "hi";
String b = "hi";
String c = new String("hi");
System.out.println((a == b) + " " + (a == c) + " " + a.equals(c));
```

**Options**
- A) `true true true`
- B) `true false true`
- C) `false false true`
- D) `false true false`

**Correct Answer:** **B**

**Explanation**
String literals share the pool (`a == b` is true). `new String(...)` allocates a new object (`a == c` is false). `equals` is content-based (true).

---

### Q18. Which line fails to compile?

```java
int a = 5;
long b = a;            // 1
short c = a;           // 2
double d = a;          // 3
byte e = (byte) a;     // 4
```

**Options**
- A) Line 1
- B) Line 2
- C) Line 3
- D) Line 4

**Correct Answer:** **B**

**Explanation**
Implicit narrowing from `int` to `short` is **not** allowed (even when the value fits). The other lines are widening or have an explicit cast.

---

### Q19. Output?

```java
System.out.println(10 % 4);
System.out.println(-10 % 4);
```

**Options**
- A) `2 2`
- B) `2 -2`
- C) `-2 -2`
- D) `2 -3`

**Correct Answer:** **B**

**Explanation**
The `%` result has the **sign of the dividend** in Java. `-10 % 4 = -2` (since `-10 = (-3)*4 + 2`? No, `(-2)*4 + -2 = -10`).

---

### Q20. (True/False) `(int) 3.99` evaluates to `4`.

**Correct Answer:** **False**

**Explanation**
Casting `double` → `int` **truncates** toward zero, so `(int) 3.99` is `3`, not `4`. Use `Math.round` for rounding.
