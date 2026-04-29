# CH5 — Methods and Encapsulation: Q & A

---

### Q1. Which is **NOT** a valid way to overload `void m(int x)`?

**Options**
- A) `void m(long x)`
- B) `void m(int x, int y)`
- C) `int m(int x)` (different return type only)
- D) `void m(Integer x)`

**Correct Answer:** **C**

**Explanation**
Return type alone does not distinguish overloads. Different parameter list (type, count, order) is required.

---

### Q2. Output?

```java
void m(int x)     { System.out.println("int"); }
void m(Integer x){ System.out.println("Integer"); }
m(5);
```

**Options**
- A) `int`
- B) `Integer`
- C) Compile error — ambiguous
- D) Runtime error

**Correct Answer:** **A**

**Explanation**
Exact match (primitive `int`) wins over autoboxing.

---

### Q3. (True/False) `protected` access allows usage from any subclass, even in a different package.

**Correct Answer:** **True**

**Explanation**
`protected` exposes a member to all subclasses (regardless of package) plus same-package classes.

---

### Q4. Output?

```java
class A { A(String x){} }
class B extends A {}
```

**Options**
- A) Both compile.
- B) Compile error in `B` — no implicit super call.
- C) Compile error in `A`.
- D) Runtime error when constructing `B`.

**Correct Answer:** **B**

**Explanation**
`B` has an implicit `super()` call, but `A` only declares `A(String)`. Either provide a no-arg constructor in `A` or have `B`'s constructor call `super("...")` explicitly.

---

### Q5. Output?

```java
void f(int x, int... ys) { System.out.println(ys.length); }
f(1);
```

**Options**
- A) `0`
- B) `1`
- C) Compile error — varargs requires at least one element
- D) Throws

**Correct Answer:** **A**

**Explanation**
Varargs accept zero or more arguments; here `ys` is an empty array.

---

### Q6. Which throws on parameter validation?

```java
public void setAge(int age) {
    if (age < 0) throw new IllegalArgumentException();
    this.age = age;
}
```

**Options**
- A) `setAge(0)`
- B) `setAge(10)`
- C) `setAge(-1)`
- D) None

**Correct Answer:** **C**

**Explanation**
Negative ages trigger the validation. `0` is non-negative.

---

### Q7. (True/False) `static` methods can be overridden by subclasses.

**Correct Answer:** **False**

**Explanation**
Static methods are **hidden**, not overridden. Resolution is by reference type.

---

### Q8. Output?

```java
class C { static void hi() { System.out.println("C"); } }
class D extends C { static void hi() { System.out.println("D"); } }
C c = new D();
c.hi();
```

**Options**
- A) `C`
- B) `D`
- C) Compile error
- D) Runtime error

**Correct Answer:** **A**

**Explanation**
Static methods resolve by reference type (`C`).

---

### Q9. Which is illegal?

**Options**
- A) `void f(int... a, int... b)`
- B) `void f(int a, int... b)`
- C) `void f(int... a)`
- D) `void f(int... a) throws IOException`

**Correct Answer:** **A**

**Explanation**
A method may have **at most one** varargs parameter, and it must be **last**.

---

### Q10. Output?

```java
class P {
    P() { System.out.println("P"); }
    P(int x) { System.out.println("P(int)"); }
}
class Q extends P {
    Q() { this(5); System.out.println("Q"); }
    Q(int x) { super(x); System.out.println("Q(int)"); }
}
new Q();
```

**Options**
- A) `P Q(int) Q`
- B) `P(int) Q(int) Q`
- C) `P P(int) Q(int) Q`
- D) `P Q`

**Correct Answer:** **B**

**Explanation**
`new Q()` chains via `this(5)` to `Q(int)`, which calls `super(5)` (`P(int)`), prints `P(int)`, then `Q(int)`, then control returns and prints `Q`.

---

### Q11. (True/False) Mutating a parameter inside a method changes the caller's variable.

**Correct Answer:** **False**

**Explanation**
Java is pass-by-value. The callee's parameter is a separate variable; reassigning it has no effect on the caller. (Mutating the *object* the reference points to does affect the caller's view of that object.)

---

### Q12. Output?

```java
class K { static int n = 1; }
K k = null;
System.out.println(k.n);
```

**Options**
- A) `1`
- B) NPE
- C) Compile error
- D) `0`

**Correct Answer:** **A**

**Explanation**
`k.n` is resolved at compile time to `K.n` — the actual receiver `k` isn't dereferenced.

---

### Q13. Which compiles?

**Options**
- A) `private final int x; { x = 5; }`
- B) `private final int x;` (with no other initialization)
- C) `private static final int x;` (with no other initialization)
- D) Both **A** and **C**

**Correct Answer:** **A**

**Explanation**
**A** assigns in an instance initializer (legal). **B** never assigns the final field. **C** must be assigned in a `static {}` block. So only **A** compiles cleanly.

---

### Q14. (True/False) A constructor with `this(...)` may also call `super(...)`.

**Correct Answer:** **False**

**Explanation**
`this(...)` and `super(...)` are mutually exclusive in the same constructor. Either one must be the first statement, not both.

---

### Q15. Which is the correct overload preference order?

**Options**
- A) Boxing → widening → exact → varargs
- B) Exact → widening → boxing → varargs
- C) Exact → varargs → widening → boxing
- D) Widening → exact → varargs → boxing

**Correct Answer:** **B**

**Explanation**
Compiler tries the cheapest conversion: exact, then widening, then boxing/unboxing, then varargs.

---

### Q16. Output?

```java
class T {
    private int n = 0;
    public T() { System.out.println("T:" + n); }
    public T(int n) { this(); this.n = n; System.out.println("T(int):" + this.n); }
}
new T(5);
```

**Options**
- A) `T(int):5`
- B) `T:0`
- C) `T:0 T(int):5`
- D) `T:5 T(int):5`

**Correct Answer:** **C**

**Explanation**
`T(int)` chains to `T()` first → prints `T:0`, then assigns and prints `T(int):5`.

---

### Q17. (True/False) Two methods that differ only in `throws` clause can coexist as overloads.

**Correct Answer:** **False**

**Explanation**
The `throws` clause is not part of the signature for overload resolution. The methods would be duplicates.

---

### Q18. Which signature is illegal?

**Options**
- A) `public final void f()`
- B) `public abstract void f()` in an abstract class
- C) `public abstract final void f()`
- D) `public static void f()`

**Correct Answer:** **C**

**Explanation**
`abstract` and `final` are contradictory: an abstract method must be overridable. The compiler rejects this combination.

---

### Q19. Output?

```java
void mod(int[] arr) { arr[0] = 99; arr = new int[]{0,0}; }
int[] a = {1,2,3};
mod(a);
System.out.println(a[0] + " " + a.length);
```

**Options**
- A) `99 3`
- B) `0 2`
- C) `1 3`
- D) `99 2`

**Correct Answer:** **A**

**Explanation**
Mutation of the same array is visible (`a[0] = 99`). Reassigning `arr` to a new array only changes the local variable.

---

### Q20. (True/False) A `static` method may declare a `throws` clause.

**Correct Answer:** **True**

**Explanation**
Static methods may declare `throws` for checked exceptions just like instance methods.
