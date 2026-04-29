# CH9 — Collections and Generics: Q & A

---

### Q1. Which interface does `Map` extend?

**Options**
- A) `Collection`
- B) `Iterable`
- C) `Map` does not extend `Collection` or `Iterable`
- D) `List`

**Correct Answer:** **C**

**Explanation**
`Map` is a **separate root** in the JCF — it does not extend `Collection`. You can iterate its `entrySet()`, `keySet()`, or `values()`, but the `Map` itself is not `Iterable`.

---

### Q2. What's the output?

```java
List<Integer> xs = new ArrayList<>(List.of(1,2,3));
for (Integer x : xs) {
    if (x == 2) xs.remove(x);
}
```

**Options**
- A) `[1, 3]`
- B) `[1, 2, 3]`
- C) Throws `ConcurrentModificationException`
- D) Throws `IndexOutOfBoundsException`

**Correct Answer:** **C**

**Explanation**
Direct mutation during for-each iteration triggers `ConcurrentModificationException` from the iterator's fail-fast check. Use `Iterator.remove()` or `removeIf` instead.

---

### Q3. Which line fails to compile?

```java
List<? extends Number> nums = List.of(1, 2.0, 3L);   // 1
Number n = nums.get(0);                              // 2
nums.add(5);                                         // 3
nums.add(null);                                      // 4
```

**Options**
- A) Line 1
- B) Line 2
- C) Line 3
- D) Line 4

**Correct Answer:** **C**

**Explanation**
A `? extends Number` list is a *producer* — you can read elements as `Number` (line 2), but cannot add anything except `null` (line 4 OK, line 3 illegal).

---

### Q4. (True/False) `List<String>` is a subtype of `List<Object>`.

**Correct Answer:** **False**

**Explanation**
Generics are **invariant**. `List<String>` and `List<Object>` are unrelated types. To accept a list of any subtype, use `List<? extends Object>`.

---

### Q5. What does this print?

```java
List<Integer> xs = new ArrayList<>(List.of(3,1,4,1,5));
Collections.sort(xs);
int i = Collections.binarySearch(xs, 2);
System.out.println(i);
```

**Options**
- A) `2`
- B) `-2`
- C) `-3`
- D) Random / undefined

**Correct Answer:** **C**

**Explanation**
After sorting: `[1,1,3,4,5]`. `2` would be inserted at index 2, so `binarySearch` returns `-(2) - 1 = -3`.

---

### Q6. Which is the correct **PECS** usage to *write* into a collection?

**Options**
- A) `Collection<? extends T>`
- B) `Collection<? super T>`
- C) `Collection<?>`
- D) `Collection<T>`

**Correct Answer:** **B**

**Explanation**
**Consumer Super**: to write `T` instances *into* a collection, the collection must be `Collection<? super T>` so it can hold `T` and its supertypes.

---

### Q7. Output?

```java
TreeSet<String> s = new TreeSet<>();
s.add("banana"); s.add("apple"); s.add("cherry");
System.out.println(s.first());
```

**Options**
- A) `banana`
- B) `apple`
- C) `cherry`
- D) Compile error

**Correct Answer:** **B**

**Explanation**
`TreeSet` orders by natural ordering (alphabetical for strings); `first()` returns the smallest, `apple`.

---

### Q8. Which is invalid?

**Options**
- A) `List<Integer> l = new ArrayList<>();`
- B) `List<int> l = new ArrayList<>();`
- C) `Map<String, List<Integer>> m = new HashMap<>();`
- D) `<T> T f(T x) { return x; }`

**Correct Answer:** **B**

**Explanation**
Generics cannot use primitive types. Use the wrapper `Integer`. Others are valid.

---

### Q9. Output?

```java
List<Integer> xs = List.of(1,2,3);
xs.add(4);
```

**Options**
- A) `[1,2,3,4]`
- B) `[1,2,3]`
- C) Throws `UnsupportedOperationException`
- D) Compile error

**Correct Answer:** **C**

**Explanation**
`List.of(...)` returns an immutable list. Any structural modification throws `UnsupportedOperationException` at runtime (the call **does** compile).

---

### Q10. (True/False) `HashMap` allows one `null` key and any number of `null` values.

**Correct Answer:** **True**

**Explanation**
`HashMap` permits a single `null` key and multiple `null` values. `Hashtable` and `ConcurrentHashMap` do not allow `null` keys/values.

---

### Q11. What's the output?

```java
record Person(String name, int age) {}
List<Person> ps = new ArrayList<>(List.of(
    new Person("A", 30), new Person("B", 25), new Person("A", 20)));

ps.sort(Comparator.comparing(Person::name)
                  .thenComparingInt(Person::age));
System.out.println(ps);
```

**Options**
- A) `[A 30, A 20, B 25]`
- B) `[A 20, A 30, B 25]`
- C) `[B 25, A 20, A 30]`
- D) Compile error

**Correct Answer:** **B**

**Explanation**
Sort by name (ascending), then by age (ascending) for ties. The two `A`s tie on name, so 20 precedes 30.

---

### Q12. Which collection guarantees insertion order **and** O(1) lookups?

**Options**
- A) `HashSet`
- B) `TreeSet`
- C) `LinkedHashSet`
- D) `ArrayList`

**Correct Answer:** **C**

**Explanation**
`LinkedHashSet` keeps a doubly-linked list of insertion order plus the hash table for O(1) `contains`. `HashSet` has no order; `TreeSet` is sorted; `ArrayList` allows duplicates.

---

### Q13. Output?

```java
Map<String, Integer> m = new HashMap<>();
m.put("x", 1);
m.merge("x", 10, Integer::sum);
m.merge("y", 5, Integer::sum);
System.out.println(m);
```

**Options**
- A) `{x=10, y=5}`
- B) `{x=11, y=5}`
- C) `{x=1, y=5}`
- D) Throws

**Correct Answer:** **B**

**Explanation**
`merge` combines existing value (`1`) with new value (`10`) via `Integer::sum` → `11`. For absent key `y`, it just inserts `5`.

---

### Q14. (True/False) You can create `new ArrayList<String>[10]`.

**Correct Answer:** **False**

**Explanation**
Generic array creation is forbidden because of erasure — type-safety can't be guaranteed at runtime. You can `(ArrayList<String>[]) new ArrayList[10]` with an unchecked warning.

---

### Q15. Output?

```java
List<Integer> xs = new ArrayList<>(List.of(1,2,3,4,5));
xs.removeIf(n -> n % 2 == 0);
System.out.println(xs);
```

**Options**
- A) `[1,3,5]`
- B) `[2,4]`
- C) `[1,2,3,4,5]`
- D) Throws CME

**Correct Answer:** **A**

**Explanation**
`removeIf` removes evens safely without iterator pitfalls.

---

### Q16. Which `Comparator` factory creates a comparator that puts `null` values **before** non-null?

**Options**
- A) `Comparator.naturalOrder()`
- B) `Comparator.reverseOrder()`
- C) `Comparator.nullsFirst(Comparator.naturalOrder())`
- D) `Comparator.nullsLast(Comparator.naturalOrder())`

**Correct Answer:** **C**

**Explanation**
`nullsFirst` wraps another comparator and treats `null` as smallest. `nullsLast` puts them after.

---

### Q17. (Multiple choice) Which is true about `TreeMap`?

**Options**
- A) Allows `null` keys.
- B) Iteration order is insertion order.
- C) Lookups are O(log n).
- D) Values must be `Comparable`.

**Correct Answer:** **C**

**Explanation**
`TreeMap` is a red-black tree, hence O(log n). It rejects `null` keys for natural-ordering, iterates in **sorted** key order, and doesn't constrain *values* — only **keys** must be comparable (or have a `Comparator`).

---

### Q18. Output?

```java
List<String> a = List.of("a","b");
List<? extends CharSequence> b = a;          // 1
CharSequence first = b.get(0);               // 2
b.add("c");                                  // 3
```

**Options**
- A) Compiles successfully
- B) Compile error on line 1
- C) Compile error on line 3
- D) Runtime exception

**Correct Answer:** **C**

**Explanation**
Lines 1 and 2 are fine. Line 3 violates the `? extends` constraint — you can't add to a producer.

---

### Q19. (True/False) `Iterator.remove()` is the only safe way to remove during a for-each loop.

**Correct Answer:** **False**

**Explanation**
Inside an explicit `Iterator` loop, `Iterator.remove()` is safe. Inside a for-each (which uses an iterator implicitly), you don't have direct access to the iterator. `Collection.removeIf(...)` is also safe and often clearer.

---

### Q20. What's the result?

```java
Map<String,Integer> m = Map.of("a",1, "b",2);
m.put("c", 3);
```

**Options**
- A) `{a=1, b=2, c=3}`
- B) Throws `UnsupportedOperationException`
- C) Throws `NullPointerException`
- D) Compile error

**Correct Answer:** **B**

**Explanation**
`Map.of(...)` is **immutable**. Any structural change throws `UnsupportedOperationException`. The call compiles, so it's not **D**.
