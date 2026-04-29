# CH9 — Collections and Generics

## 1) Overview

The Java Collections Framework (JCF) provides reusable data structures: `List`, `Set`, `Queue`, `Deque`, and `Map`. **Generics** add compile-time type safety to these structures while erasing types at runtime. This chapter covers the contract of each collection, how generics work (including bounded wildcards and type inference), and the algorithms in `Collections` and `Comparator`.

---

## 2) Key Concepts

### Collections Hierarchy

```
            Iterable
                │
            Collection
   ┌──────────┼─────────┐
  List       Set       Queue / Deque
                       (LinkedList, ArrayDeque, PriorityQueue)
```

`Map` is **not** a `Collection` — it's a separate root.

### Common Implementations

| Interface | Implementations            | Notes                                     |
| --------- | -------------------------- | ----------------------------------------- |
| `List`    | `ArrayList`, `LinkedList`  | Ordered, allows duplicates; index access. |
| `Set`     | `HashSet`, `LinkedHashSet`, `TreeSet` | No duplicates; tree variant is sorted. |
| `Queue`/`Deque` | `LinkedList`, `ArrayDeque`, `PriorityQueue` | FIFO/LIFO/heap.       |
| `Map`     | `HashMap`, `LinkedHashMap`, `TreeMap` | Key→value mappings; tree is sorted. |

### Iteration & Removal

- The for-each loop relies on `Iterable`.
- Removing during iteration requires `Iterator.remove()` — modifying the collection directly throws `ConcurrentModificationException` (CME) on the next `next()`.
- `Collection.removeIf(Predicate)` is a safe shortcut.

### Generics

- Declared as `class Box<T>` / `interface Pair<K,V>`.
- Type arguments are erased at runtime: `List<String>` and `List<Integer>` share the same class object.
- A generic method has its type parameter declared **before** the return type:

```java
public static <T> T pickOne(T a, T b) { ... }
```

### Bounded Type Parameters

```java
<T extends Number>            // T is Number or subtype
```

### Wildcards (only in usage, never in declarations)

| Form          | Meaning                      | Use case               |
| ------------- | ---------------------------- | ---------------------- |
| `?`           | Unknown                      | Read-only of any type  |
| `? extends T` | Some subtype of T (read)     | **Producer** (PECS)    |
| `? super T`   | Some supertype of T (write)  | **Consumer** (PECS)    |

**PECS:** *Producer Extends, Consumer Super.*

### Comparable vs. Comparator

- `Comparable<T>` defines **natural ordering** via `compareTo`.
- `Comparator<T>` is an external/alternative ordering, often via lambda or method reference, with composition: `comparing`, `thenComparing`, `reversed`, `naturalOrder`, `reverseOrder`, `nullsFirst`, `nullsLast`.

### `Collections` Utility Class

- `sort`, `binarySearch`, `reverse`, `shuffle`, `min`, `max`, `frequency`, `unmodifiableList`, `synchronizedList`.

### Factory Methods

- `List.of(...)`, `Set.of(...)`, `Map.of(...)`, `Map.ofEntries(...)`: **immutable**, **null-hostile** collections (NPE on `null` argument).
- `List.copyOf(coll)`: defensive immutable copy.

---

## 3) Important Rules

- A `List<Object>` is **not** a supertype of `List<String>` — generics are *invariant*.
- You cannot create generic arrays: `new T[5]` fails to compile. Workaround: `(T[]) new Object[5]`.
- You cannot use primitives as type arguments — `List<int>` is illegal; use `List<Integer>`.
- `?` in a declared type means *unknown*, but you may not call methods that take `?` as an argument (you can read; can't write — except `null`).
- A `TreeSet` / `TreeMap` requires its elements/keys to be `Comparable` or to be supplied a `Comparator`. Adding non-comparable elements throws `ClassCastException` at runtime.
- `HashSet`/`HashMap` rely on consistent `equals`/`hashCode`. Mutating a key after insertion can corrupt the map.
- `Collections.unmodifiableList(list)` wraps the original — modifying the original still affects the wrapped view.
- `List.of(...)` is **deeply** immutable (you cannot `set`, `add`, or `remove`) and rejects `null`.
- `binarySearch` requires the list to be **sorted**; otherwise the result is undefined (a non-negative number with no semantic meaning, or a negative insertion point).
- `Map.replaceAll(BiFunction)` modifies values in place; `keySet()`, `values()`, `entrySet()` return **views** backed by the map.
- `compareTo` returning 0 should be **consistent with `equals`** — otherwise `TreeSet` and `TreeMap` may "lose" elements that would otherwise be considered equal in a `HashSet`.

---

## 4) Code Examples

### Generic class

```java
public class Box<T> {
    private T value;
    public T get() { return value; }
    public void set(T value) { this.value = value; }
}
```

### Generic method with bound

```java
public static <T extends Comparable<T>> T max(List<T> list) {
    T best = list.get(0);
    for (T x : list) if (x.compareTo(best) > 0) best = x;
    return best;
}
```

### PECS

```java
public static double sum(Collection<? extends Number> nums) {     // producer
    double total = 0;
    for (Number n : nums) total += n.doubleValue();
    return total;
}

public static void addInts(Collection<? super Integer> dst) {     // consumer
    for (int i = 0; i < 5; i++) dst.add(i);
}
```

### Comparator composition

```java
record Person(String name, int age) {}

Comparator<Person> byName = Comparator.comparing(Person::name);
Comparator<Person> byAgeDesc = Comparator.comparingInt(Person::age).reversed();

people.sort(byName.thenComparing(byAgeDesc));
```

### Iterator removal vs. CME

```java
List<Integer> xs = new ArrayList<>(List.of(1,2,3,4));
Iterator<Integer> it = xs.iterator();
while (it.hasNext()) {
    if (it.next() % 2 == 0) it.remove();    // safe
}
// xs.removeIf(x -> x % 2 == 0);            // simpler equivalent
```

### TreeMap ordering

```java
NavigableMap<Integer,String> m = new TreeMap<>();
m.put(2,"b"); m.put(1,"a"); m.put(3,"c");
System.out.println(m.firstKey());   // 1
System.out.println(m.floorKey(2));  // 2
```

### Map factory caveat

```java
Map.of("a", 1, "a", 2);   // throws IllegalArgumentException: duplicate key
Map.of("a", null);        // throws NullPointerException
```

---

## 5) Common Mistakes

- ❌ Calling `add` on a `List<? extends T>` — only `null` is accepted.
- ❌ Mutating a key in a `HashSet` or `HashMap` after insertion.
- ❌ Returning the *unmodifiableList* wrapper while still keeping a reference to the underlying list and mutating that.
- ❌ Sorting an `Arrays.asList(...)`-backed list with a fixed-size restriction — `sort` works, but `add`/`remove` won't.
- ❌ Using primitive in generics: `List<int>` (impossible).
- ❌ Mixing `Comparable` and `Comparator` and forgetting that `Comparator.comparing(...)` chain returns a new comparator (does not mutate).
- ❌ Forgetting that `binarySearch`'s negative return is `-(insertionPoint) - 1`.
- ❌ Trying to create `new ArrayList<T>[10]` (generic array creation forbidden).
- ❌ Believing `LinkedList` is a faster `ArrayList` for random access — it isn't (O(n)).
- ❌ Adding `null` to `TreeSet`/`TreeMap` (`NullPointerException` for natural ordering).

---

## 6) Mental Model

Picture three orthogonal axes for choosing a collection:

1. **Order**:
   - Insertion-preserving → `ArrayList`, `LinkedList`, `LinkedHashSet`, `LinkedHashMap`.
   - Sorted → `TreeSet`, `TreeMap`, `PriorityQueue`.
   - Hash (no order) → `HashSet`, `HashMap`.
2. **Duplicates**:
   - Allowed → `List`.
   - Not allowed → `Set`/`Map keys`.
3. **Access pattern**:
   - Index → `List`.
   - FIFO → `Queue`.
   - Stack/double-ended → `Deque`.
   - Key/value → `Map`.

Generics: imagine a **type filter** at the language layer. The JVM doesn't see it (erasure). It exists to keep you honest at compile time.

PECS is about *direction*:
- A producer **gives** values out — you should be able to read more types than you'd write, hence `extends`.
- A consumer **takes** values in — you must accept more types than the input, hence `super`.

---

## 7) Quick Revision

- `Collection` ≠ `Map`. Map sits separately.
- Use `removeIf` or `Iterator.remove()` to avoid CME.
- Generics are erased at runtime; no primitive type args, no generic arrays.
- PECS: Producer Extends, Consumer Super.
- `List.of`, `Set.of`, `Map.of` are immutable and null-hostile.
- `TreeSet`/`TreeMap` need `Comparable` or `Comparator`; `HashSet`/`HashMap` need consistent `equals`/`hashCode`.
- `Comparator.comparing(...).thenComparing(...).reversed()` for fluent ordering.
- `binarySearch` insertion point is `-(idx) - 1`.
- `keySet`, `values`, `entrySet` return **views** over the map.
- Generic method type parameter goes **before** the return type.
