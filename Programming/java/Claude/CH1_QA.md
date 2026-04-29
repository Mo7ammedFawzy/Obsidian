# CH1 — Building Blocks: Q & A

---

### Q1. Which is a valid `main` method signature?

**Options**
- A) `public void main(String[] args)`
- B) `public static int main(String[] args)`
- C) `public static void main(String... args)`
- D) `static void main(String[] args)`

**Correct Answer:** **C**

**Explanation**
Must be `public static void` taking a single `String[]` (or varargs `String...`). **A** lacks `static`, **B** has wrong return type, **D** is missing `public`.

---

### Q2. (True/False) A `.java` file may declare multiple `public` top-level classes.

**Correct Answer:** **False**

**Explanation**
At most **one** public top-level type per source file, and its name must match the filename.

---

### Q3. Which numeric literal **does NOT** compile?

**Options**
- A) `int a = 0b1010;`
- B) `long b = 9_000_000_000L;`
- C) `int c = 1_000_000_;`
- D) `int d = 0xFF;`

**Correct Answer:** **C**

**Explanation**
A trailing underscore is illegal. Underscores cannot start, end, or be adjacent to a `.` or sign. **B** correctly uses `L` for the large value.

---

### Q4. Which is the default value of an instance field of type `boolean`?

**Options**
- A) `true`
- B) `false`
- C) `null`
- D) Compile error if not assigned

**Correct Answer:** **B**

**Explanation**
Instance fields are initialized to "zero" of their type — `false` for `boolean`. Locals have no default; using them uninitialized is a compile error.

---

### Q5. Output?

```java
class A {
    int x;
    A(int x) { this.x = x; }
    public static void main(String[] args) {
        A a = new A(5);
        System.out.println(a.x);
    }
}
```

**Options**
- A) `0`
- B) `5`
- C) Compile error
- D) Runtime error

**Correct Answer:** **B**

**Explanation**
The instance field is set in the constructor. There is no default constructor inserted because an explicit one was declared.

---

### Q6. Which `var` declaration **fails** to compile?

**Options**
- A) `var x = 1;`
- B) `var name = "Mo";`
- C) `var n = null;`
- D) `var list = new ArrayList<String>();`

**Correct Answer:** **C**

**Explanation**
The compiler can't infer the type from `null` alone. **A**, **B**, **D** have a concrete initializer.

---

### Q7. Which import compiles successfully?

**Options**
- A) `import java.util.Date; import java.sql.Date;`
- B) `import java.util.*; import java.util.List.*;`
- C) `import static java.lang.Math.PI;`
- D) `import .*;`

**Correct Answer:** **C**

**Explanation**
Static import is valid. **A** has a name collision; **B** treats `List` as a package; **D** is invalid syntax.

---

### Q8. (True/False) An object becomes eligible for garbage collection the moment its last reference goes out of scope.

**Correct Answer:** **True**

**Explanation**
Eligibility is reachability-based. The collection itself happens later, at the JVM's discretion.

---

### Q9. What does this print?

```java
class T {
    static int s = init("S");
    int i = init("I");
    T()           { System.out.println("ctor"); }
    static int init(String x) { System.out.println(x); return 0; }
    public static void main(String[] a) { new T(); }
}
```

**Options**
- A) `S I ctor`
- B) `I S ctor`
- C) `ctor S I`
- D) `S ctor I`

**Correct Answer:** **A**

**Explanation**
Static fields run on class load (`S`), then instance fields when `new T()` is called (`I`), then constructor body (`ctor`).

---

### Q10. Which is **not** a primitive type?

**Options**
- A) `byte`
- B) `boolean`
- C) `String`
- D) `char`

**Correct Answer:** **C**

**Explanation**
`String` is a class. The eight primitives: `boolean byte short int long float double char`.

---

### Q11. Output?

```java
String a = "hi";
String b = new String("hi");
System.out.println(a == b);
System.out.println(a.equals(b));
```

**Options**
- A) `true true`
- B) `false true`
- C) `true false`
- D) `false false`

**Correct Answer:** **B**

**Explanation**
`new String(...)` always allocates a new object — `==` compares references and is `false`. `equals` compares content and is `true`.

---

### Q12. (True/False) `import java.util.*;` also imports `java.util.concurrent.*;`.

**Correct Answer:** **False**

**Explanation**
Wildcard imports do **not** recurse into subpackages.

---

### Q13. Which compiles?

**Options**
- A) `int x = 0X1A_;`
- B) `int x = _0X1A;`
- C) `int x = 0X1_A;`
- D) `int x = 0X.1A;`

**Correct Answer:** **C**

**Explanation**
Underscores cannot be at the beginning or end, but may sit between digits. **D** is malformed (decimal in hex literal).

---

### Q14. Output?

```java
public class M {
    public static void main(String[] args) {
        System.out.println(args.length);
    }
}
```

Run as: `java M one two`

**Options**
- A) `0`
- B) `1`
- C) `2`
- D) `3`

**Correct Answer:** **C**

**Explanation**
The arguments after the class name are passed as an array; `args.length == 2`.

---

### Q15. (True/False) Java is pass-by-reference for objects.

**Correct Answer:** **False**

**Explanation**
Java is always pass-by-value. For object types, the *value* passed is the value of the reference (the address).

---

### Q16. Which file declaration is invalid?

**Options**
- A) `package com.x;` then `import com.y.Z;` then `public class A { }`
- B) `import com.y.Z;` then `package com.x;` then `public class A { }`
- C) `package com.x;` then `class A { }`
- D) `class A { } class B { }`

**Correct Answer:** **B**

**Explanation**
`package` (if present) must precede `import`. **D** compiles — both classes are package-private.

---

### Q17. Which identifier is illegal?

**Options**
- A) `$value`
- B) `_total`
- C) `2cool`
- D) `name1`

**Correct Answer:** **C**

**Explanation**
Identifiers cannot start with a digit. `_` alone (single underscore) would also be reserved (Java 9+).

---

### Q18. Output?

```java
double d = 3_14e_2;
```

**Options**
- A) `31400.0`
- B) `0.0314`
- C) Compile error
- D) `314.0`

**Correct Answer:** **C**

**Explanation**
Underscores can't be adjacent to `e` (the exponent indicator). The `_2` after `e` is invalid.

---

### Q19. Which statement compiles?

**Options**
- A) `int i = 100; long l = i;`
- B) `long l = 100L; int i = l;`
- C) `float f = 3.14;`
- D) `var v;`

**Correct Answer:** **A**

**Explanation**
Implicit widening from `int` to `long` is allowed. **B** is narrowing (needs cast). **C** assigns a `double` literal to a `float` (needs `f` suffix). **D** has no initializer.

---

### Q20. Which class is **not** auto-imported?

**Options**
- A) `String`
- B) `Math`
- C) `List`
- D) `Object`

**Correct Answer:** **C**

**Explanation**
`String`, `Math`, `Object` are in `java.lang`, which is auto-imported. `List` is in `java.util` — needs explicit import.
