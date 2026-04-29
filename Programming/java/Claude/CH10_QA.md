# CH10 — Functional Programming (Streams): Q & A

---

### Q1. What's the output?

```java
Stream<Integer> s = Stream.of(1,2,3);
s.forEach(System.out::print);
s.forEach(System.out::print);
```

**Options**
- A) `123123`
- B) `123` then `IllegalStateException`
- C) Compile error
- D) `123` then nothing

**Correct Answer:** **B**

**Explanation**
A stream is **single-use**. The first `forEach` consumes/closes it; the second throws `IllegalStateException: stream has already been operated upon or closed`.

---

### Q2. Which operation is **terminal**?

**Options**
- A) `filter`
- B) `map`
- C) `peek`
- D) `count`

**Correct Answer:** **D**

**Explanation**
`count` produces a `long` — it's terminal. `filter`, `map`, and `peek` are intermediate (lazy).

---

### Q3. (True/False) `peek` should be relied upon for production side effects.

**Correct Answer:** **False**

**Explanation**
`peek` is documented as a debugging aid. The JDK may skip elements that aren't required by the terminal op (e.g., `count`), so it's unreliable for side effects.

---

### Q4. Output?

```java
Optional<String> o = Optional.ofNullable(null);
System.out.println(o.orElse("X"));
```

**Options**
- A) `null`
- B) `X`
- C) Throws NPE
- D) `Optional.empty`

**Correct Answer:** **B**

**Explanation**
`ofNullable(null)` produces `Optional.empty()`. `orElse("X")` returns the fallback `"X"`.

---

### Q5. What does this print?

```java
int sum = IntStream.rangeClosed(1, 5).sum();
System.out.println(sum);
```

**Options**
- A) `10`
- B) `15`
- C) `14`
- D) `20`

**Correct Answer:** **B**

**Explanation**
`rangeClosed(1,5)` includes both ends — `1+2+3+4+5 = 15`. `range(1,5)` would have been `1+2+3+4 = 10`.

---

### Q6. Output?

```java
List<List<Integer>> nested = List.of(List.of(1,2), List.of(3,4));
long total = nested.stream().flatMap(List::stream).count();
System.out.println(total);
```

**Options**
- A) `2`
- B) `4`
- C) `0`
- D) Compile error

**Correct Answer:** **B**

**Explanation**
`flatMap` flattens to `Stream<Integer>` of size 4.

---

### Q7. (True/False) `Stream.iterate(0, i -> i + 1).count()` returns `Long.MAX_VALUE`.

**Correct Answer:** **False**

**Explanation**
The stream is infinite, so `count` **never terminates**. You must use `limit` (or the Java 9+ form with a `hasNext` predicate).

---

### Q8. What's the result?

```java
Stream<String> s = Stream.of("a","bb","ccc");
String joined = s.collect(Collectors.joining(",","[","]"));
System.out.println(joined);
```

**Options**
- A) `[a,bb,ccc]`
- B) `a,bb,ccc`
- C) `[a, bb, ccc]`
- D) Compile error

**Correct Answer:** **A**

**Explanation**
`joining(delim, prefix, suffix)` wraps and separates exactly as given.

---

### Q9. Which line throws at runtime?

```java
Optional<Integer> a = Optional.of(5);                    // 1
Optional<Integer> b = Optional.ofNullable(null);         // 2
int v = b.get();                                         // 3
Optional<Integer> c = Optional.of(null);                 // 4
```

**Options**
- A) Line 1
- B) Line 2
- C) Line 3
- D) Line 4

**Correct Answer:** **C** *(Line 4 also throws — but the question asks which throws first; line 3 throws `NoSuchElementException` while line 4 throws NPE if reached)*

**Explanation**
Most exam keys treat **line 3** as the answer (`get` on empty Optional throws `NoSuchElementException`). Line 4 also throws (`Optional.of(null)` → NPE), but execution doesn't reach it because line 3 fails first.

---

### Q10. What's printed?

```java
Map<Integer, Long> byLen = Stream.of("a","bb","cc")
    .collect(Collectors.groupingBy(String::length, Collectors.counting()));
System.out.println(byLen);
```

**Options**
- A) `{1=1, 2=2}`
- B) `{1=a, 2=bb,cc}`
- C) `{a=1, bb=1, cc=1}`
- D) Compile error

**Correct Answer:** **A**

**Explanation**
Grouping by length with `counting` downstream gives counts: 1 string of length 1, 2 strings of length 2.

---

### Q11. Which **must** you provide to avoid `IllegalStateException` from `Collectors.toMap` on duplicate keys?

**Options**
- A) A `Comparator`
- B) A merge function `(BinaryOperator)`
- C) A custom `Map` factory
- D) `distinct()` before collecting

**Correct Answer:** **B**

**Explanation**
The 3-arg form `toMap(keyFn, valFn, merger)` provides a merge strategy. Without one, duplicate keys throw `IllegalStateException`.

---

### Q12. Output?

```java
List<Integer> xs = List.of(3, 1, 4, 1, 5, 9, 2, 6);
int v = xs.stream().sorted().distinct().skip(2).findFirst().get();
System.out.println(v);
```

**Options**
- A) `3`
- B) `4`
- C) `2`
- D) `1`

**Correct Answer:** **A**

**Explanation**
After `sorted().distinct()` the elements are `[1, 2, 3, 4, 5, 6, 9]`. `skip(2)` drops `1` and `2`; the next `findFirst()` returns `3`.

---

### Q13. (True/False) `findFirst()` on a parallel stream of `[1,2,3]` always returns `Optional[1]`.

**Correct Answer:** **True**

**Explanation**
`findFirst` is order-preserving regardless of parallelism. `findAny` is the one that may return any element in parallel.

---

### Q14. What does this print?

```java
double avg = IntStream.of().average().orElse(-1);
System.out.println(avg);
```

**Options**
- A) `0.0`
- B) `-1.0`
- C) Throws NoSuchElementException
- D) `NaN`

**Correct Answer:** **B**

**Explanation**
`IntStream.of()` is empty, so `average` returns `OptionalDouble.empty()`; `orElse(-1)` ⇒ `-1.0`.

---

### Q15. Which is the correct way to convert `Stream<Integer>` to `int[]`?

**Options**
- A) `s.toArray()` then cast
- B) `s.mapToInt(Integer::intValue).toArray()`
- C) `s.toIntArray()`
- D) `s.collect(Collectors.toList()).toArray(int[]::new)`

**Correct Answer:** **B**

**Explanation**
`mapToInt` produces an `IntStream` whose `toArray` returns `int[]`. The other options either don't exist or won't compile.

---

### Q16. What happens here?

```java
Stream<Integer> s = Stream.iterate(1, i -> i + 1);
List<Integer> first5 = s.limit(5).collect(Collectors.toList());
System.out.println(first5);
```

**Options**
- A) `[1,2,3,4,5]`
- B) Hangs forever
- C) Throws OOM
- D) `[]`

**Correct Answer:** **A**

**Explanation**
`limit(5)` is a short-circuiting intermediate op that bounds the infinite stream. `collect` realizes the first five elements.

---

### Q17. Output?

```java
List<Integer> xs = List.of(1,2,3,4);
int sum = xs.stream().reduce(0, Integer::sum);
System.out.println(sum);
```

**Options**
- A) `10`
- B) `Optional[10]`
- C) `0`
- D) Compile error

**Correct Answer:** **A**

**Explanation**
The 2-arg `reduce` returns `T` directly with the supplied identity; here `int 10`.

---

### Q18. (True/False) `Optional` is intended to be used as a field type for entities.

**Correct Answer:** **False**

**Explanation**
The Java team intends `Optional` for **return types**, not as a field type or parameter (it's not `Serializable` and adds little vs. nullable fields). The exam may not test this idiomatic guidance directly, but it's a frequent best-practice question.

---

### Q19. What does this print?

```java
String result = Stream.of("a","b","c")
    .collect(Collectors.collectingAndThen(
        Collectors.toList(),
        list -> String.join("-", list).toUpperCase()));
System.out.println(result);
```

**Options**
- A) `A-B-C`
- B) `[a, b, c]`
- C) `abc`
- D) Compile error

**Correct Answer:** **A**

**Explanation**
`collectingAndThen` finishes with a transformation: collect to a `List`, then join and uppercase.

---

### Q20. Which sequence is purely **intermediate** (no terminal)?

**Options**
- A) `filter, map, sorted`
- B) `filter, map, count`
- C) `filter, forEach`
- D) `peek, toList`

**Correct Answer:** **A**

**Explanation**
`count`, `forEach`, and `toList` are terminal. Only **A** is fully intermediate.
