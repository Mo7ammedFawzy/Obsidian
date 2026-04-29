# CH6 — Class Design: Q & A

---

### Q1. (True/False) A class can extend multiple classes in Java.

**Correct Answer:** **False**

**Explanation**
Java supports single class inheritance. Multiple inheritance is allowed only via interfaces.

---

### Q2. Output?

```java
class A { String hi() { return "A"; } }
class B extends A { String hi() { return "B"; } }
A a = new B();
System.out.println(a.hi());
```

**Options**
- A) `A`
- B) `B`
- C) Compile error
- D) Runtime error

**Correct Answer:** **B**

**Explanation**
Instance method dispatch is dynamic — the actual object is `B`.

---

### Q3. Output?

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
Static methods are **hidden** — resolved by reference type (`A`), not the runtime object.

---

### Q4. Which is **NOT** a valid override?

```java
class P { protected Number m() throws IOException { ... } }
```

**Options**
- A) `class C extends P { public Integer m() throws IOException { ... } }`
- B) `class C extends P { public Number m() { ... } }`
- C) `class C extends P { protected Object m() throws IOException { ... } }`
- D) `class C extends P { public Integer m() { ... } }`

**Correct Answer:** **C**

**Explanation**
The return type `Object` is **not covariant** with `Number` — it's a supertype. Visibility may widen (`protected` → `public`) but return must match or be a subtype.

---

### Q5. (True/False) The override may throw a broader checked exception than the parent declared.

**Correct Answer:** **False**

**Explanation**
The override may throw the same or narrower checked exceptions, never broader.

---

### Q6. Output?

```java
class A {
    A() { System.out.println("A"); }
    A(String s) { System.out.println("A " + s); }
}
class B extends A {
    B() { System.out.println("B"); }
}
new B();
```

**Options**
- A) `B`
- B) `A B`
- C) `A`
- D) Compile error

**Correct Answer:** **B**

**Explanation**
Implicit `super()` runs first (`A`), then `B`'s body (`B`).

---

### Q7. Which fails to compile?

**Options**
- A) `final class A { }` and `class B extends A {}`
- B) `class A { final void m() {} }` and `class B extends A { void m() {} }`
- C) `abstract class A { abstract void m(); }`
- D) Both **A** and **B**

**Correct Answer:** **D**

**Explanation**
`final` classes can't be extended; `final` methods can't be overridden. Both **A** and **B** violate these rules. **C** is fine.

---

### Q8. (True/False) An interface may declare `private static` helper methods.

**Correct Answer:** **True**

**Explanation**
Since Java 9, private and private static methods are allowed inside interfaces as helpers used by other methods within that interface.

---

### Q9. Output?

```java
interface X { default String id() { return "X"; } }
interface Y { default String id() { return "Y"; } }
class Z implements X, Y { }
```

**Options**
- A) Compiles; calls return `"X"` first.
- B) Compile error — `Z` must override `id()`.
- C) Calls return `"Y"`.
- D) Runtime error.

**Correct Answer:** **B**

**Explanation**
A diamond default-method conflict forces the implementing class to override and resolve.

---

### Q10. Which is true about an abstract class?

**Options**
- A) Cannot have constructors.
- B) Cannot have fields.
- C) Cannot be instantiated directly.
- D) Cannot have concrete methods.

**Correct Answer:** **C**

**Explanation**
Abstract classes can have constructors, fields, and concrete methods. They simply cannot be instantiated.

---

### Q11. Output?

```java
class A { int n = 1; }
class B extends A { int n = 2; }
A a = new B();
System.out.println(a.n);
```

**Options**
- A) `1`
- B) `2`
- C) Compile error
- D) Runtime error

**Correct Answer:** **A**

**Explanation**
Fields are resolved by reference type (hiding), not by runtime object.

---

### Q12. (True/False) Constructors are inherited from parent to child.

**Correct Answer:** **False**

**Explanation**
Constructors are not inherited. Subclass constructors must chain via `super(...)` (or `this(...)` then super eventually).

---

### Q13. Which combination is illegal?

**Options**
- A) `abstract` and `static`
- B) `abstract` and `final`
- C) `abstract` and `private`
- D) All of the above

**Correct Answer:** **D**

**Explanation**
Abstract methods cannot be `static` (must be overridable in instances), `final` (can't be overridden), or `private` (can't be inherited).

---

### Q14. Output?

```java
class P { void m() { System.out.println("P"); } }
class C extends P { @Override public void m() { System.out.println("C"); } }
((P) new C()).m();
```

**Options**
- A) `P`
- B) `C`
- C) Compile error
- D) Runtime error

**Correct Answer:** **B**

**Explanation**
Casting only affects the reference's apparent type. Instance method dispatch still picks the runtime type's method.

---

### Q15. (True/False) `interface A {}` and `interface B {}` can both be implemented by `class C implements A, B {}`.

**Correct Answer:** **True**

**Explanation**
A class can implement multiple interfaces. There are no conflicts because both are empty.

---

### Q16. Output?

```java
class A { A() { print(); } void print() { System.out.println("A"); } }
class B extends A { String s = "S"; void print() { System.out.println(s); } }
new B();
```

**Options**
- A) `A`
- B) `S`
- C) `null`
- D) Compile error

**Correct Answer:** **C**

**Explanation**
The parent constructor calls `print()`, which dispatches to `B.print()`. But `B`'s field `s` hasn't been initialized yet when `super()` runs, so it's still `null`.

---

### Q17. Which is the correct way to safely downcast?

**Options**
- A) `(Sub) ref;`
- B) `if (ref instanceof Sub s) { /* use s */ }`
- C) `ref.getClass() == Sub.class`
- D) Both **B** and **C**

**Correct Answer:** **B**

**Explanation**
Pattern matching with `instanceof` performs the test, the cast, and the binding. **A** alone risks `ClassCastException`. **C** doesn't account for subclasses of `Sub`.

---

### Q18. Output?

```java
abstract class S {
    int x = 5;
    abstract int compute();
    int total() { return x + compute(); }
}
class T extends S { int compute() { return 10; } }
System.out.println(new T().total());
```

**Options**
- A) `5`
- B) `10`
- C) `15`
- D) Compile error

**Correct Answer:** **C**

**Explanation**
`T` provides `compute() = 10`. `total() = 5 + 10 = 15`. Abstract classes can have fields and concrete methods.

---

### Q19. (True/False) An interface field is implicitly `private static final`.

**Correct Answer:** **False**

**Explanation**
Interface fields are implicitly `public static final` — not `private`.

---

### Q20. Which is true about `@Override`?

**Options**
- A) Required for any override.
- B) Optional, but causes a compile error if it's not actually overriding anything.
- C) Required only for static methods.
- D) Only valid on `default` interface methods.

**Correct Answer:** **B**

**Explanation**
`@Override` is optional but recommended; the compiler verifies that the method really overrides one — useful for catching typos.
