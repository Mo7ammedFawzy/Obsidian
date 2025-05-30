## Strings and StringBuilder

**Q1:** What is the difference between `String` and `StringBuilder`?

**A1:**

- `String` is **immutable**: each modification creates a new object.
    
- `StringBuilder` is **mutable**: methods like `append()` modify the same instance, making it more efficient for repeated concatenations.
    

---

**Q2:** Why is `String` immutable in Java?

**A2:**

- **Security:** prevents unauthorized changes when strings are used for file paths, network connections, etc.
    
- **Caching/String Pooling:** immutable strings can be shared safely.
    
- **Thread-safety:** no need for external synchronization when multiple threads access a `String`.
    
- **HashCode caching:** the hash code can be cached at creation, speeding up hash-based collections.
    

---

## Arrays

**Q3:** Can an array hold different types?

**A3:** No. Arrays are **homogeneous**: all elements must be of the declared type (or its subclasses for reference types).

---

**Q4:** What happens if you access an array index out of bounds?

**A4:** Java throws a `java.lang.ArrayIndexOutOfBoundsException` at runtime.

---
## `ArrayList``<E>`

**Q5**:  What’s the difference between an `ArrayList` and an array?

**A5:**
- **Size:** Arrays have fixed length; `ArrayList` is dynamically resizable.
    
- **Type:** Arrays can hold primitives directly; `ArrayList` holds only objects (use wrapper classes for primitives).
    

---

**Q6:** What happens if you modify the list returned by `Arrays.asList()`?

**A6:** The returned list is a **fixed-size** view of the array. Attempts to `add()` or `remove()` elements throw `UnsupportedOperationException`.

---

## Date & Time API (`java.time`)

**Q7:** What is the difference between `Period` and `Duration`?

**A7:**

- `Period`: measures date-based amount (years, months, days).
    
- `Duration`: measures time-based amount (hours, minutes, seconds).
    

---

**Q8:** Are `LocalDate` and `LocalTime` mutable?

**A8:** No. All classes in `java.time` (e.g., `LocalDate`, `LocalTime`, `LocalDateTime`) are **immutable** and thread-safe. Methods like `plusDays()` return new instances.

---
