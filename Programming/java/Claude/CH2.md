# CH2 — Operators

## 1) Overview

Java has a rich operator system with strict precedence and subtle behaviors around evaluation order, type promotion, and short-circuiting. The OCP exam routinely tests precedence, the difference between bitwise and logical operators, casting and overflow, the ternary operator, increment/decrement positioning, and how `==` differs across primitives, references, and wrappers.

---

## 2) Key Concepts

### Operator Categories

| Category | Operators |
| --- | --- |
| Unary | `+ - ! ~ ++ -- (cast)` |
| Multiplicative | `* / %` |
| Additive | `+ -` |
| Shift | `<< >> >>>` |
| Relational | `< <= > >= instanceof` |
| Equality | `== !=` |
| Bitwise | `& ^ \|` |
| Logical (short-circuit) | `&& \|\|` |
| Ternary | `?:` |
| Assignment | `= += -= *= /= %= &= ^= \|= <<= >>= >>>=` |

### Precedence (high → low)

1. `()` `.` `[]` postfix `++ --`
2. Unary `++ -- + - ! ~ (cast)`
3. `* / %`
4. `+ -`
5. `<< >> >>>`
6. `< <= > >= instanceof`
7. `== !=`
8. `&`
9. `^`
10. `|`
11. `&&`
12. `||`
13. `?:`
14. Assignment

Most operators are **left-to-right associative**; assignment and ternary are **right-to-left**.

### Numeric Promotion

When two operands meet:
- If either is `double`, the other is promoted to `double`.
- Else if either is `float`, the other is promoted to `float`.
- Else if either is `long`, the other is promoted to `long`.
- Else both are promoted to `int` (so `byte + byte` is `int`).

### Increment / Decrement

- **Prefix** `++x`: increment then use.
- **Postfix** `x++`: use then increment.

```java
int a = 5;
int b = ++a;     // a=6, b=6
int c = a++;     // a=7, c=6
```

### Bitwise vs. Logical

- `& | ^` on `boolean` always evaluates **both** sides.
- `&&` `||` short-circuit: skip right side if result is determined.

```java
if (s != null && s.length() > 0)   // safe: short-circuits when null
```

### Compound Assignment

`x += y;` is equivalent to `x = (typeof x)(x + y);` — it does an **implicit cast**:

```java
byte b = 10;
b += 5;       // OK: implicit cast back to byte
b = b + 5;    // ❌ compile error: int + int = int
```

### Ternary `?:`

```java
int max = (a > b) ? a : b;
```

- Right-associative: `a ? b : c ? d : e` is `a ? b : (c ? d : e)`.
- Both branches must produce a compatible type; the result type is the wider one.

### Equality

- Primitives: `==` compares values (with promotion).
- References: `==` compares addresses; `equals` compares content.
- Wrappers: `==` may return `true` for cached small values (e.g., `Integer` from -128 to 127), but **don't rely on it**.

### Casting

- **Implicit** widening: `byte → short → int → long → float → double`.
- **Explicit** narrowing requires `(type)`. May overflow silently:

```java
int big = 200;
byte b = (byte) big;     // -56 due to overflow
```

- Casting between unrelated reference types is a compile error; between related types may throw `ClassCastException` at runtime.

### Bitwise & Shift

- `<<` left shift (multiplies by 2).
- `>>` arithmetic right shift (sign-preserving).
- `>>>` logical right shift (zero-fill — only for `int`/`long`).
- For `int`, the shift distance is taken `mod 32`; for `long`, `mod 64`.

```java
-1 >> 1   == -1                  // sign extends
-1 >>> 1  == 2147483647          // zero-fills
1 << 33   == 1 << 1   == 2       // 33 mod 32 = 1
```

### `instanceof`

Tests if a reference is an instance of a type (and not `null`). Pattern matching (Java 16+) introduces a binding:

```java
if (o instanceof String s) {
    // s is a String here
}
```

---

## 3) Important Rules

- All arithmetic on integral types narrower than `int` is **promoted to `int`**.
- Compound assignment performs an **implicit cast back** to the LHS type.
- Postfix `++` / `--` evaluate the **original value**, then update.
- Division by zero:
  - Integers: throws `ArithmeticException`.
  - Floating point: yields `Infinity`, `-Infinity`, or `NaN`.
- `NaN != NaN` is **always** `true`; `NaN == NaN` is `false`. Use `Double.isNaN(x)`.
- `&&`/`||` are **short-circuiting**; `&`/`|` aren't.
- The two arms of `?:` must have compatible types; mixed `int`/`Integer` causes auto-(un)boxing.
- `==` between two `String` literals is `true` due to the string pool, but creating with `new String(...)` returns a fresh object.
- Wrapper caching: `Integer.valueOf(n)` returns the same instance for `n` in `[-128, 127]` — `==` may surprise you.
- `(byte)256 == 0`, `(byte)-129 == 127` (overflow wraps).
- A cast can be applied even to expressions: `(int)(d + 0.5)`.
- Operator precedence is fixed; you can't change it — only parentheses do.
- `instanceof null` is **always false**.
- `boolean` cannot be cast to/from any other type.

---

## 4) Code Examples

### Precedence trap

```java
int x = 1 + 2 * 3 - 4 / 2;        // 1 + 6 - 2 = 5
int y = (1 + 2) * (3 - 4) / 2;    // 3 * -1 / 2 = -1
```

### Promotion / overflow

```java
byte a = 10, b = 20;
// byte c = a + b;                // ❌ result is int
byte c = (byte)(a + b);
int big = 130;
byte ov = (byte) big;              // ov = -126 (overflow)
```

### Short-circuit safety

```java
String s = null;
if (s != null && s.startsWith("x")) { /* safe */ }
if (s != null & s.startsWith("x"))  { /* NPE */ }
```

### Compound vs explicit

```java
short s = 10;
s += 1;        // OK
// s = s + 1;  // ❌ int can't be assigned to short
```

### Ternary types

```java
int n = 5;
Number num = (n > 0) ? n : 0.0;    // result widens to Number (Integer + Double → Number)
```

### Bitwise

```java
int flags = 0b0101;
flags |= 0b0010;            // 0b0111 = 7
flags &= ~0b0100;           // 0b0011 = 3
flags ^= 0b0001;            // toggles last bit -> 0b0010 = 2
```

### Tricky: postfix vs prefix

```java
int a = 1;
int b = a++ + ++a;
// a++ uses 1, then a=2; ++a sets a=3 and uses 3; b = 1 + 3 = 4; final a=3
System.out.println(a + " " + b);   // 3 4
```

### Tricky: NaN

```java
double d = 0.0 / 0.0;
System.out.println(d == d);              // false
System.out.println(Double.isNaN(d));     // true
```

### Tricky: integer cache

```java
Integer x = 127, y = 127;
Integer p = 128, q = 128;
System.out.println(x == y);   // true  (cached)
System.out.println(p == q);   // false (separate objects)
System.out.println(p.equals(q)); // true
```

### `instanceof` and pattern variable

```java
Object o = "hello";
if (o instanceof String s && !s.isEmpty()) {
    System.out.println(s.charAt(0));
}
```

---

## 5) Common Mistakes

- ❌ Forgetting that `byte + byte` is `int`.
- ❌ Using `&`/`|` where short-circuit `&&`/`||` is needed (causes NPE).
- ❌ Comparing `String`/`Integer` with `==` instead of `equals`.
- ❌ Believing postfix `++` returns the new value.
- ❌ Forgetting that integer division truncates: `5/2 == 2`.
- ❌ Mixing `?:` arms whose types force unwanted boxing/unboxing (NPE if a wrapper is `null`).
- ❌ Using `>>>` on `byte`/`short` (it promotes them to `int` first — sign-extension can surprise you).
- ❌ Assuming `Integer.valueOf(200) == Integer.valueOf(200)` is `true` (it isn't).
- ❌ Casting unrelated reference types (compile error) vs. related types (runtime CCE).
- ❌ Misreading precedence: `a == b & c` means `a == b` *and-bitwise* `c` is parsed weirdly — always parenthesize.

---

## 6) Mental Model

Think of an expression as a **tree** built using precedence and associativity. Java evaluates left-to-right within each level, applying operator semantics. Understanding **two key transformations** helps:

1. **Numeric promotion** — narrow types become `int` or wider before arithmetic.
2. **Cast / boxing / unboxing** — these happen automatically and silently; bugs hide here (NaN propagation, integer overflow, NPE on unboxing `null`).

Boolean logic comes in two flavors: *eager* (`&`/`|`/`^`) and *lazy* (`&&`/`||`). Use lazy ones for safety (null checks) and eager when you need both sides evaluated.

For pattern matching with `instanceof`, treat the bound variable as an extra "lift" the compiler grants you in the branch where the type is proven.

---

## 7) Quick Revision

- Operator precedence (top): postfix → unary → `* / %` → `+ -` → shifts → relational → equality → bitwise → logical → ternary → assignment.
- Integer math on `byte`/`short` always returns `int`.
- Compound assignment includes an implicit cast.
- `&&` / `||` short-circuit; `&` / `|` don't.
- Ternary is right-associative; arms must have compatible types.
- `==` on references is identity, not equality.
- Wrapper cache: `[-128, 127]` shares instances; outside it doesn't.
- `>>>` is logical right shift (zero-fill); only meaningful on `int`/`long`.
- Integer divide-by-zero throws; floating-point yields Infinity/NaN.
- `NaN != NaN` is `true`; use `Double.isNaN`.
