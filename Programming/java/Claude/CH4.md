# CH4 — Core APIs

## 1) Overview

This chapter covers the core, everyday APIs that show up in nearly every Java program: `String`, `StringBuilder`, arrays, `ArrayList`, the `java.time` date/time API, and `Math`. The exam tests immutability, method behavior at edge cases (negative indices, out-of-bounds), pool semantics for strings, and the difference between `Period`, `Duration`, and `Instant`.

---

## 2) Key Concepts

### `String`

- **Immutable** — every "modification" returns a new `String`.
- Backed by the **string pool** for literals; `new String("x")` creates a fresh heap object.
- Compared with `equals` (content) — `==` checks reference identity.

Common methods (all 0-based):

| Method | Behavior |
| --- | --- |
| `length()` | Number of chars |
| `charAt(i)` | Char at index `i`, throws if invalid |
| `indexOf(s)` / `indexOf(ch)` | First index, `-1` if absent |
| `substring(a, b)` | `[a, b)`, throws on invalid range |
| `concat(s)` | Same as `+` for strings |
| `replace(a, b)` | All occurrences |
| `trim()` / `strip()` | Remove whitespace; `strip` is Unicode-aware (Java 11+) |
| `toLowerCase()` / `toUpperCase()` | Case conversion |
| `equals` / `equalsIgnoreCase` | Content equality |
| `compareTo` | Lexicographic ordering |
| `isEmpty()` / `isBlank()` | Length zero / only whitespace |
| `startsWith` / `endsWith` / `contains` | Prefix/suffix/substring tests |
| `split(regex)` | Split into array |
| `format(fmt, args...)` | Like `printf`-style |

Java 15+: **text blocks** `"""..."""` for multi-line literals with intelligent indentation handling.

### `StringBuilder`

- **Mutable** sibling of `String`. Use for repeated concatenations.
- `append`, `insert`, `delete`, `reverse`, `replace(i, j, str)`, `setCharAt`, `charAt`, `length`, `toString`.
- `StringBuffer` is the older, **synchronized** version — rarely needed today.

### Arrays

- Fixed length at creation.
- Indexed from 0 to `length - 1`.
- `int[] a = new int[3];` zero-initialized.
- `int[] a = {1,2,3};` shorthand.
- `Arrays.toString(a)`, `Arrays.sort(a)`, `Arrays.binarySearch(a, key)`, `Arrays.equals(a,b)`, `Arrays.fill(a,v)`, `Arrays.copyOf(a,len)`.
- Multi-dim: `int[][] grid = new int[3][3];`. Inner arrays may differ in length (jagged).

### `ArrayList<E>`

- Dynamic resizable array, in `java.util`.
- `add`, `add(i, e)`, `remove(o)`, `remove(int)`, `set`, `get`, `size`, `clear`, `contains`, `indexOf`.
- `Arrays.asList(...)` returns a **fixed-size** view of the array (cannot `add`/`remove` but can `set`).
- `List.of(...)` is **immutable** (Java 9+) and rejects `null`.

### Wrapper Classes & Autoboxing

- `Integer`, `Double`, `Boolean`, etc. — immutable boxes around primitives.
- Autoboxing/unboxing converts automatically. Unboxing `null` throws NPE.
- `Integer.parseInt(str)` / `Integer.valueOf(str)` parse a string.
- `Integer.toBinaryString`, `toHexString`, `toOctalString`.

### `java.time`

| Type | Represents |
| --- | --- |
| `LocalDate` | Date only (year-month-day) |
| `LocalTime` | Time only (hour:min:sec.nano) |
| `LocalDateTime` | Date + time, no zone |
| `ZonedDateTime` | Date + time + zone |
| `Instant` | Machine timestamp (UTC) |
| `Period` | Date-based (years/months/days) |
| `Duration` | Time-based (hours/minutes/seconds/nanos) |
| `DateTimeFormatter` | Parsing/formatting |

All immutable. Mutation methods (`plusDays`, `withYear`, etc.) return new instances.

### `Math`

- `abs`, `min`, `max`, `pow`, `sqrt`, `random`, `floor`, `ceil`, `round`, `signum`, `floorDiv`, `floorMod`.
- `Math.random()` returns a `double` in `[0.0, 1.0)`.

---

## 3) Important Rules

- `String` is immutable: `"a".concat("b")` returns a new string; the original is untouched.
- `s == "a"` works for the **same literal** (pool); use `equals` everywhere else.
- `substring(a, b)` is exclusive of `b`; if `a == b` returns empty string.
- `s.indexOf(ch, fromIndex)` searches starting from `fromIndex` (clamped to 0 if negative).
- `StringBuilder.toString()` produces a `String`; the builder remains mutable.
- `Arrays.asList(arr)` reflects changes in the underlying array — these two share storage.
- `List.of(...)`, `Set.of(...)`, `Map.of(...)` are **deeply** immutable and **null-hostile**.
- An array's `length` is a final field, not a method; a `String`'s `length()` is a method.
- Two arrays with the same contents: `arr1 == arr2` is `false`; `Arrays.equals(arr1, arr2)` is `true`.
- `Integer.parseInt("12L")` throws `NumberFormatException`.
- Wrapper cache (`Integer.valueOf` for `[-128, 127]`) makes `==` deceptively true for small ints.
- `LocalDate.of(2024, 13, 1)` throws `DateTimeException` — months are 1–12.
- `LocalDate.parse("2024/01/01")` throws — default format is ISO `yyyy-MM-dd`.
- `Period.between(d1, d2)` is **end-exclusive**: from 2024-01-01 to 2024-02-01 = 1 month, 0 days.
- `Period.ofMonths(2).plusDays(3)` chains, returning a new `Period`.
- `Duration` cannot mix with date-based units; `LocalDate.plus(Duration)` throws.
- `Math.round(2.5)` returns `3` (rounds half-up). `Math.round(-2.5)` returns `-2` (toward positive infinity).
- `Math.random()` is non-deterministic; for reproducibility use `java.util.Random`/`SplittableRandom`.

---

## 4) Code Examples

### String pool vs new

```java
String a = "hello";
String b = "hello";
String c = new String("hello");
System.out.println(a == b);     // true
System.out.println(a == c);     // false
System.out.println(a.equals(c));// true
```

### Building strings efficiently

```java
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) sb.append(i).append(',');
String result = sb.toString();
```

### Text blocks (Java 15+)

```java
String json = """
    {
      "name": "Mo",
      "age": 30
    }
    """;
```

### Arrays utility

```java
int[] a = {3, 1, 4, 1, 5};
Arrays.sort(a);                         // {1,1,3,4,5}
int idx = Arrays.binarySearch(a, 3);    // 2
int[] b = Arrays.copyOf(a, 7);          // [1,1,3,4,5,0,0]
int[] c = Arrays.copyOfRange(a, 1, 4);  // [1,3,4]
```

### List vs unmodifiable list vs immutable

```java
List<Integer> mutable = new ArrayList<>(List.of(1,2,3));
mutable.add(4);                 // OK

List<Integer> view = Arrays.asList(1,2,3);
view.set(0, 9);                 // OK (set allowed)
// view.add(4);                 // ❌ UnsupportedOperationException

List<Integer> imm = List.of(1,2,3);
// imm.add(4);                  // ❌ Unsupported
// imm.set(0, 9);               // ❌ Unsupported
```

### Date/time

```java
LocalDate today = LocalDate.now();
LocalDate next = today.plusDays(7).withMonth(12);
Period age = Period.between(LocalDate.of(1995, 5, 1), today);

LocalTime t = LocalTime.of(14, 30);
Duration meeting = Duration.ofHours(1).plusMinutes(30);

ZonedDateTime kahirah = ZonedDateTime.now(ZoneId.of("Africa/Cairo"));
Instant epoch = Instant.now();
```

### Tricky: Period vs Duration

```java
LocalDate d = LocalDate.of(2024, 1, 1);
LocalDate e = d.plus(Period.ofDays(30));   // OK
// LocalDate f = d.plus(Duration.ofDays(30));   // ❌ Unsupported (date can't take time-based)
```

### Tricky: integer cache

```java
Integer x = 100, y = 100;
Integer p = 200, q = 200;
System.out.println(x == y);    // true  (cached)
System.out.println(p == q);    // false (new objects)
```

### Tricky: substring out of range

```java
"abc".substring(0, 4);          // ❌ StringIndexOutOfBoundsException
"abc".substring(3);             // "" (empty, ok)
"abc".substring(2, 2);          // "" (empty, ok)
```

---

## 5) Common Mistakes

- ❌ Concatenating thousands of strings with `+` instead of `StringBuilder`.
- ❌ Comparing `String`/`Integer` with `==`.
- ❌ Trusting the wrapper cache for arbitrary values.
- ❌ Calling `Arrays.asList(...)` then `add`/`remove` (fixed size).
- ❌ Modifying a `List.of(...)` (immutable).
- ❌ Using `List.of(null)` (NPE).
- ❌ Forgetting that `LocalDate.of(2024, 2, 30)` throws.
- ❌ Confusing `Period` and `Duration` units.
- ❌ Using `parseInt` on a non-numeric string without try/catch.
- ❌ Believing `String` mutates — `s.toUpperCase()` returns a new instance.

---

## 6) Mental Model

`String` is the **safe immutable text type**; `StringBuilder` is its **mutable sibling**. Use `String` until performance demands the builder.

Arrays are the **fixed-size, primitive-friendly** collection — fast but rigid. `ArrayList` is the **dynamic-array** alternative that holds objects (with autoboxing for primitives) and offers a richer API.

The wrapper classes turn primitives into objects but introduce subtle pitfalls: caching, NPE on unboxing, identity vs. equality.

`java.time` is **immutable, fluent, type-safe**. Each type represents exactly one concept (date, time, instant, etc.). To compute durations and shifts, pair the right amount type:
- date math → `Period`,
- clock math → `Duration`,
- machine moments → `Instant`,
- zoned events → `ZonedDateTime`.

`Math` is a static-only utility — straightforward, but mind half-up rounding and `floorDiv` vs `/` for negative values.

---

## 7) Quick Revision

- Strings are immutable; literals share the pool.
- `equals`/`hashCode` for content; `==` for identity.
- `StringBuilder` is mutable; use it for heavy concatenation.
- Arrays: `length` is a field; `Arrays` utility class for ops.
- `Arrays.asList` is fixed-size view; `List.of` is fully immutable & null-hostile.
- Wrapper cache: `[-128,127]`. Use `equals`.
- `LocalDate`, `LocalTime`, `LocalDateTime`, `ZonedDateTime`, `Instant` — all immutable.
- `Period` for date math; `Duration` for time math; never mix.
- `Math.round(2.5) = 3`, `Math.round(-2.5) = -2`.
- `Math.random()` returns `[0.0, 1.0)`.
