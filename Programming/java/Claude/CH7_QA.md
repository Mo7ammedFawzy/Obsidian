# CH7 — Beyond Classes: Q & A

---

### Q1. Which of the following is *implicitly applied* to every interface member field?

**Options**
- A) `public final`
- B) `protected static`
- C) `public static final`
- D) `private static`

**Correct Answer:** **C**

**Explanation**
Interface fields are constants — the compiler implicitly adds `public static final`. Anything contradicting (`private`, `protected`, non-`final`) won't compile. **A** is missing `static`. **B** and **D** are illegal access modifiers on an interface field.

---

### Q2. What does this code print?

```java
interface A { default String id() { return "A"; } }
interface B { default String id() { return "B"; } }

class C implements A, B {
    @Override public String id() { return A.super.id() + B.super.id(); }
}

System.out.println(new C().id());
```

**Options**
- A) `A`
- B) `B`
- C) `AB`
- D) Compile error

**Correct Answer:** **C**

**Explanation**
The class **must** override the conflicting default. `A.super.id()` and `B.super.id()` explicitly invoke each parent default. Without the override, you'd get **D**.

---

### Q3. Which is a valid record declaration?

**Options**
- A) `record Point(int x, int y) { int z; }`
- B) `record Point(int x, int y) { static int Z = 1; }`
- C) `record Point(int x) extends Number { }`
- D) `public record Point(int x) { public int getX() { return x; } }` (and accessors auto-generated as `getX()`)

**Correct Answer:** **B**

**Explanation**
Records may have **static** members but not instance fields outside the header (**A** wrong). Records implicitly extend `java.lang.Record` and cannot extend other classes (**C** wrong). Auto-generated accessors are named after the components (`x()`, not `getX()`), so **D**'s premise is wrong.

---

### Q4. (True/False) A `sealed` parent class can have a subtype declared `protected`.

**Correct Answer:** **False**

**Explanation**
Direct subtypes of a sealed type must themselves be `final`, `sealed`, or `non-sealed`. `protected` is an access modifier, not one of the three required completeness modifiers — it's irrelevant here, and a subtype lacking one of the three modifiers will fail to compile.

---

### Q5. What's the output?

```java
enum Op {
    PLUS  { int apply(int a, int b) { return a + b; } },
    MINUS { int apply(int a, int b) { return a - b; } };
    abstract int apply(int a, int b);
}

System.out.println(Op.MINUS.apply(5, 2));
```

**Options**
- A) `7`
- B) `3`
- C) Compile error
- D) Runtime error

**Correct Answer:** **B**

**Explanation**
Each enum constant supplies its own body for the abstract method. `MINUS.apply(5,2)` runs `5 - 2 = 3`. Constant-specific bodies are a legitimate (and exam-favourite) feature of enums.

---

### Q6. Which statement about enum constructors is correct?

**Options**
- A) Enum constructors may be `public`.
- B) Enum constructors may be `protected`.
- C) Enum constructors are implicitly `private`.
- D) Enums cannot have constructors.

**Correct Answer:** **C**

**Explanation**
Enum constructors are implicitly `private` (or package-private if you write nothing). You can declare them `private` explicitly, but `public`/`protected` are illegal.

---

### Q7. Will this compile?

```java
public sealed interface Animal permits Dog {}
public class Dog implements Animal {}
```

**Options**
- A) Yes
- B) No, `Dog` must declare `final`, `sealed`, or `non-sealed`
- C) No, `permits` is not allowed on interfaces
- D) No, sealed interfaces must permit at least two subtypes

**Correct Answer:** **B**

**Explanation**
Direct subtypes of a sealed type must specify completeness via one of `final`, `sealed`, or `non-sealed`. Bare `class Dog` is invalid here. Sealed interfaces are valid (eliminates **C**), and any number of permitted subtypes is allowed (eliminates **D**).

---

### Q8. Which can a `static` nested class access from its enclosing class?

**Options**
- A) Only `static` members
- B) Both static and instance members
- C) Neither
- D) Only `private` members

**Correct Answer:** **A**

**Explanation**
A `static` nested class has no enclosing instance, so it cannot reference instance members directly. It can access static members (including `private static` ones — same enclosing class).

---

### Q9. What does this print?

```java
record Range(int lo, int hi) {
    public Range {
        if (lo > hi) { int t = lo; lo = hi; hi = t; }   // normalize
    }
}
System.out.println(new Range(5, 1));
```

**Options**
- A) `Range[lo=5, hi=1]`
- B) `Range[lo=1, hi=5]`
- C) Compile error — cannot reassign components in compact constructor
- D) Runtime error

**Correct Answer:** **B**

**Explanation**
A compact constructor may **reassign the parameter variables** (`lo`, `hi`) — those are then used to set the fields. What you cannot do is `this.lo = ...` inside a compact constructor. The toString from the synthesized record method shows the normalized values.

---

### Q10. Which is true about `instanceof` pattern matching?

**Options**
- A) The pattern variable is in scope of the entire enclosing method.
- B) The pattern variable's scope follows control flow — only where the type is proven.
- C) Pattern matching can be used with primitive types like `int`.
- D) `instanceof` always returns `true` for `null`.

**Correct Answer:** **B**

**Explanation**
Flow scoping limits the pattern variable to branches where the compiler can prove the type. **C** is false — pattern matching is for reference types. **D** is false — `null instanceof X` is always `false`. **A** overstates scope.

---

### Q11. (True/False) An interface can declare `private static` methods that other interface methods can call.

**Correct Answer:** **True**

**Explanation**
Since Java 9, `private` and `private static` methods are allowed in interfaces as helpers shared by `default`/`static` methods inside that interface. They cannot be called from outside the interface.

---

### Q12. What's the result?

```java
class A { static String hi() { return "A"; } }
class B extends A { static String hi() { return "B"; } }

A a = new B();
System.out.println(a.hi());
```

**Options**
- A) `A`
- B) `B`
- C) Compile error
- D) Runtime error

**Correct Answer:** **A**

**Explanation**
`hi()` is `static`, so it's resolved by the **reference type** (`A`), not by the runtime object — this is **method hiding**, not overriding. If `hi()` were an instance method, the answer would be `B`.

---

### Q13. Which line fails to compile?

```java
final class Animal {}                         // 1
class Dog extends Animal {}                   // 2
sealed class Cat permits Persian {}           // 3
final class Persian extends Cat {}            // 4
```

**Options**
- A) Line 1
- B) Line 2
- C) Line 3
- D) Line 4

**Correct Answer:** **B**

**Explanation**
`Animal` is `final`, so `Dog` cannot extend it. The sealed hierarchy on lines 3–4 is valid: `Cat` is sealed and `Persian` is final — both fulfil the completeness rule.

---

### Q14. (True/False) An anonymous class can refer to a non-final local variable from its enclosing method.

**Correct Answer:** **False**

**Explanation**
The local must be **effectively final** — its value cannot change after the anonymous class is created. The same rule applies to lambdas.

---

### Q15. Which is *not* synthesized for every record?

**Options**
- A) `equals(Object)`
- B) `hashCode()`
- C) `clone()`
- D) `toString()`

**Correct Answer:** **C**

**Explanation**
Records auto-generate `equals`, `hashCode`, `toString`, accessors, and the canonical constructor. They do **not** override `clone()` — `Record` doesn't implement `Cloneable`.

---

### Q16. What's printed?

```java
interface Greeter {
    default String greet() { return "Hi"; }
}
class Polite implements Greeter {
    public String greet() { return Greeter.super.greet() + "!"; }
}
System.out.println(new Polite().greet());
```

**Options**
- A) `Hi`
- B) `Hi!`
- C) Compile error
- D) `null`

**Correct Answer:** **B**

**Explanation**
`Greeter.super.greet()` invokes the interface's default explicitly. Concatenated with `"!"`, the result is `"Hi!"`.

---

### Q17. (Multiple choice) Which is true about an enum?

**Options**
- A) An enum may extend another class.
- B) An enum may implement multiple interfaces.
- C) `values()` is inherited from `Enum`.
- D) Enum constants may be reassigned at runtime.

**Correct Answer:** **B**

**Explanation**
**A** is false — enums implicitly extend `java.lang.Enum`. **C** is false — `values()` is generated by the compiler per enum, not inherited from `Enum`. **D** is false — enum constants are effectively final singletons. Only **B** is correct.

---

### Q18. What is the output?

```java
public sealed interface Shape permits Circle, Square {}
record Circle(double r) implements Shape {}
record Square(double s) implements Shape {}

static double area(Shape sh) {
    return switch (sh) {
        case Circle c -> Math.PI * c.r() * c.r();
        case Square sq -> sq.s() * sq.s();
    };
}
System.out.println(area(new Square(3)));
```

**Options**
- A) `9.0`
- B) Compile error: switch must have default
- C) `0.0`
- D) Runtime error

**Correct Answer:** **A**

**Explanation**
The switch is **exhaustive** because `Shape` is sealed and every permitted subtype has a case. No `default` is needed. `Square(3).s() * Square(3).s() = 9.0`.

---

### Q19. (True/False) A non-static inner class can declare a `static int` field.

**Correct Answer:** **False** *(unless it's a `static final` compile-time constant)*

**Explanation**
Inner (non-static) classes cannot declare static members **except** `static final` compile-time constants. A plain `static int` field is rejected by the compiler.

---

### Q20. Which statement about a record's canonical constructor is correct?

**Options**
- A) It can be more restrictive in visibility than the record class.
- B) It can be omitted only if a compact constructor is provided.
- C) Its parameter names must match the record's component names.
- D) It is implicitly `private`.

**Correct Answer:** **C**

**Explanation**
Canonical constructor parameter names must match the component names exactly. **A** is illegal — visibility cannot be reduced. **B** is wrong — the canonical constructor is auto-generated whenever neither a canonical nor compact constructor is declared. **D** is wrong — its accessibility matches the record itself.
