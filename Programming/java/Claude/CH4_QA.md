# CH4 — Core APIs: Q & A

---

### Q1. Output?

```java
String s = "hello";
s.toUpperCase();
System.out.println(s);
```

**Options**
- A) `HELLO`
- B) `hello`
- C) Compile error
- D) Throws

**Correct Answer:** **B**

**Explanation**
Strings are immutable; `toUpperCase` returns a new `String` that's discarded. The original `s` is unchanged.

---

### Q2. (True/False) `"abc".substring(2, 2)` returns an empty string.

**Correct Answer:** **True**

**Explanation**
When start equals end, the result is an empty string — not an exception.

---

### Q3. Output?

```java
StringBuilder sb = new StringBuilder("ab");
sb.insert(1, 'X');
sb.append("YZ");
System.out.println(sb);
```

**Options**
- A) `abXYZ`
- B) `aXbYZ`
- C) `aXYbZ`
- D) `XaYbZ`

**Correct Answer:** **B**

**Explanation**
`insert(1, 'X')` puts `X` at index 1: `aXb`. `append("YZ")` makes `aXbYZ`.

---

### Q4. Which is true about `List.of(1,2,3)`?

**Options**
- A) Immutable; null-hostile.
- B) Mutable; allows nulls.
- C) Fixed-size; allows nulls.
- D) Lazy view of an array.

**Correct Answer:** **A**

**Explanation**
`List.of(...)` is deeply immutable; passing `null` throws NPE.

---

### Q5. Output?

```java
int[] a = {1,2,3};
int[] b = {1,2,3};
System.out.println((a == b) + " " + Arrays.equals(a,b));
```

**Options**
- A) `true true`
- B) `false false`
- C) `false true`
- D) `true false`

**Correct Answer:** **C**

**Explanation**
Arrays are objects; `==` compares references (different) — but `Arrays.equals` compares contents.

---

### Q6. (True/False) `Arrays.asList(arr)` allows you to call `set` but not `add`.

**Correct Answer:** **True**

**Explanation**
The list is a fixed-size view backed by the array. You can replace elements but not change the size.

---

### Q7. Output?

```java
LocalDate d = LocalDate.of(2024, 1, 31).plusMonths(1);
System.out.println(d);
```

**Options**
- A) `2024-03-02`
- B) `2024-02-29`
- C) `2024-02-28`
- D) Throws

**Correct Answer:** **B**

**Explanation**
`plusMonths(1)` clamps to the last valid day of the target month — Feb 2024 is a leap year, so day 29.

---

### Q8. Which throws `DateTimeException`?

**Options**
- A) `LocalDate.of(2024, 12, 31)`
- B) `LocalDate.of(2024, 13, 1)`
- C) `LocalDate.of(2024, 2, 29)`
- D) `LocalDate.parse("2024-02-29")`

**Correct Answer:** **B**

**Explanation**
Month must be 1–12. `2024-02-29` is valid in a leap year. The standard ISO format parses fine.

---

### Q9. Output?

```java
String s = "Hello, World";
System.out.println(s.indexOf("o", 5));
```

**Options**
- A) `4`
- B) `7`
- C) `8`
- D) `-1`

**Correct Answer:** **C**

**Explanation**
Searching for `'o'` from index 5: `s.charAt(8) = 'o'` (the second one).

---

### Q10. (True/False) `Period.ofDays(30)` and `Duration.ofDays(30)` produce equivalent shifts when added to a `LocalDate`.

**Correct Answer:** **False**

**Explanation**
`LocalDate` rejects `Duration` (time-based) — throws `UnsupportedTemporalTypeException`. Use `Period`.

---

### Q11. Output?

```java
String a = new String("x");
String b = "x";
System.out.println(a == b);
System.out.println(a.intern() == b);
```

**Options**
- A) `true true`
- B) `false false`
- C) `false true`
- D) `true false`

**Correct Answer:** **C**

**Explanation**
Without `intern`, `a` is a fresh object — `false`. After `intern`, `a` returns the pooled instance equal to `b`.

---

### Q12. Output?

```java
List<Integer> nums = Arrays.asList(1,2,3);
nums.add(4);
```

**Options**
- A) `[1,2,3,4]`
- B) Throws `UnsupportedOperationException`
- C) Compile error
- D) `[1,2,3]`

**Correct Answer:** **B**

**Explanation**
`Arrays.asList` returns a fixed-size list; `add` is unsupported.

---

### Q13. (True/False) `Math.round(2.5)` returns `3`.

**Correct Answer:** **True**

**Explanation**
Half-up rounding for positives. But `Math.round(-2.5)` returns `-2` (toward positive infinity).

---

### Q14. Which is the result?

```java
String s = "  hi  ";
System.out.println(s.strip().length());
```

**Options**
- A) `2`
- B) `4`
- C) `6`
- D) `0`

**Correct Answer:** **A**

**Explanation**
`strip` removes leading/trailing whitespace (Unicode-aware). `"hi"` is length 2.

---

### Q15. Output?

```java
Integer a = 127, b = 127, c = 200, d = 200;
System.out.println((a == b) + " " + (c == d));
```

**Options**
- A) `true true`
- B) `false false`
- C) `false true`
- D) `true false`

**Correct Answer:** **D**

**Explanation**
`Integer` cache covers `[-128, 127]`, so `a == b` is `true`. `200` exceeds the cache; `c == d` is `false`.

---

### Q16. (True/False) `Math.random()` returns a value in `[0.0, 1.0]` (inclusive on both ends).

**Correct Answer:** **False**

**Explanation**
The range is `[0.0, 1.0)` — `1.0` is **never** returned.

---

### Q17. Output?

```java
LocalDate d1 = LocalDate.of(2024, 1, 1);
LocalDate d2 = LocalDate.of(2024, 4, 10);
Period p = Period.between(d1, d2);
System.out.println(p);
```

**Options**
- A) `P3M9D`
- B) `P3M10D`
- C) `P0Y3M9D`
- D) `P100D`

**Correct Answer:** **A**

**Explanation**
ISO 8601 duration: 3 months + 9 days. `Period` rolls into years/months/days; the years component is omitted from `toString` when 0.

---

### Q18. Output?

```java
String[] parts = "a,b,,c".split(",");
System.out.println(parts.length);
```

**Options**
- A) `3`
- B) `4`
- C) `5`
- D) `2`

**Correct Answer:** **B**

**Explanation**
`split(",")` on `"a,b,,c"` yields `["a","b","","c"]` — 4 elements (trailing empties are kept up to but not after final non-empty). With `split(",", -1)` you'd keep all trailing empties.

---

### Q19. (True/False) `StringBuilder` is thread-safe.

**Correct Answer:** **False**

**Explanation**
`StringBuilder` is **not** synchronized. `StringBuffer` is the older, synchronized variant.

---

### Q20. Output?

```java
String text = """
        line1
          line2
        """;
System.out.print(text);
```

**Options**
- A) `line1\n  line2\n`
- B) `        line1\n          line2\n`
- C) `line1\nline2\n`
- D) Compile error

**Correct Answer:** **A**

**Explanation**
Text blocks strip the *common* leading indentation (here: 8 spaces). The relative indent (`line2` 2 spaces deeper) is preserved.
