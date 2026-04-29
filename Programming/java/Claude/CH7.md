# CH7 — Beyond Classes

## 1) Overview

Chapter 7 expands the type system beyond ordinary classes: **interfaces**, **enums**, **sealed classes**, **records**, **nested types**, and **polymorphism**. These are heavy on the OCP exam — many "trick" questions hinge on the subtle modifier rules and access semantics described here.

---

## 2) Key Concepts

### Interfaces

- An interface defines an **abstract type**: a contract a class agrees to implement.
- Members may be:
  - `public abstract` instance methods (implicit modifiers).
  - `public static final` constants (implicit modifiers).
  - `default` methods (with body, instance scope).
  - `static` methods (with body, called on the interface).
  - `private` methods / `private static` methods — helpers usable only inside the interface (Java 9+).
- A class can `implements` **multiple** interfaces; an interface can `extends` multiple interfaces.

### Enums

- An `enum` is a fixed, type-safe set of constants. Implicitly `final` and extends `java.lang.Enum`.
- May have **constructors** (implicitly `private`), **fields**, **methods**, and **constant-specific bodies**.
- `values()`, `valueOf(String)`, `name()`, `ordinal()` are auto-generated/inherited.
- An `enum` may implement interfaces but cannot `extends` another class.

### Sealed Classes (Java 17+)

- A `sealed` class/interface restricts which other types may extend/implement it.
- Direct subtypes must be one of: `final`, `sealed`, or `non-sealed`.
- Subtypes must be in the **same module** (or same package if unnamed module).
- The `permits` clause lists allowed subtypes (may be omitted if all subtypes are in the same compilation unit).

### Records (Java 16+)

- A **record** is an immutable, transparent data carrier.
- The compiler synthesizes:
  - `private final` field per component.
  - **Canonical constructor** matching the header.
  - Accessor methods named **exactly like the components** (no `get` prefix).
  - `equals()`, `hashCode()`, `toString()`.
- Records are implicitly `final` and extend `java.lang.Record`. They cannot extend other classes; they may implement interfaces.

### Nested Types

| Type                | Static? | Can access enclosing instance? | Use case |
| ------------------- | ------- | ----------------------------- | -------- |
| **Inner class**     | No      | Yes                           | Tight coupling to outer instance |
| **Static nested**   | Yes     | No (only static members)      | Helper grouped under a class |
| **Local class**     | No      | Yes (effectively-final locals)| Inside a method body |
| **Anonymous class** | No      | Yes                           | One-shot subclass/impl |

### Polymorphism

- A reference of a parent type can hold a subtype object — **virtual method dispatch** runs the subtype method.
- Resolution rules:
  - **Instance methods** → runtime type.
  - **Static methods, fields, `private`** → reference (compile-time) type.
- `instanceof` (with optional pattern variable, Java 16+) lets you safely downcast.

---

## 3) Important Rules

- An interface field is **always** `public static final`.
- An interface method (without `default`/`static`/`private`) is **always** `public abstract`.
- A `default` method may be overridden by an implementing class; if two interfaces provide the same default, the class **must** override and may call either via `Iface.super.method()`.
- A `private` interface method **cannot** be `abstract`. A `private static` interface method may be called from any other method in that interface.
- An enum **cannot** be `extended` (it is implicitly `final`); switch on an enum uses **unqualified** constant names in the case label.
- A constant-specific body inside an enum may override methods declared in the enum body — this lets each constant supply its own behavior.
- A `sealed` parent's permitted subtype must be one of: `final`, `sealed`, `non-sealed`. Anything else fails to compile.
- Records cannot declare instance fields outside the header; they may declare `static` fields and methods.
- A record's canonical constructor cannot be **less accessible** than the record itself.
- A **compact constructor** of a record has no parameter list and assigns from the components automatically — you can validate/normalize but cannot reassign `this.x`.
- Inner (non-static) classes cannot declare `static` members except `static final` compile-time constants.
- Anonymous classes may capture only **effectively final** local variables.

---

## 4) Code Examples

### Interfaces with default + private helpers

```java
interface Greeter {
    default String greet(String name) {
        return prefix() + name;
    }
    private String prefix() { return "Hello, "; }
}
```

### Diamond default conflict

```java
interface A { default String id() { return "A"; } }
interface B { default String id() { return "B"; } }

class C implements A, B {
    @Override
    public String id() { return A.super.id() + B.super.id(); }
}
```

### Enum with constant-specific behavior

```java
enum Op {
    PLUS  { @Override int apply(int a, int b) { return a + b; } },
    MINUS { @Override int apply(int a, int b) { return a - b; } };

    abstract int apply(int a, int b);
}
```

### Sealed hierarchy

```java
public sealed interface Shape permits Circle, Square, Triangle {}

public final class Circle implements Shape { }
public sealed class Square implements Shape permits ColoredSquare { }
public non-sealed class Triangle implements Shape { }   // allows further extension
```

### Records: canonical, compact, and custom constructors

```java
public record Range(int lo, int hi) {
    // Compact constructor: validates components
    public Range {
        if (lo > hi) throw new IllegalArgumentException();
    }
    // Custom static factory
    public static Range of(int x) { return new Range(x, x); }
}
```

### Tricky: pattern matching with `instanceof`

```java
Object o = "hello";
if (o instanceof String s && s.length() > 0) {
    System.out.println(s.toUpperCase());
}
```

`s` is in scope because flow proves it's non-null and a `String`.

### Tricky: inner class capturing `this`

```java
class Outer {
    private int x = 10;
    class Inner { int get() { return x; } }   // legal — sees Outer.this.x
}
```

A static nested class **cannot** access `x` directly because it has no enclosing instance.

---

## 5) Common Mistakes

- ❌ Marking an interface field `private` or `protected` — only `public` is allowed (implicit).
- ❌ Forgetting that records' accessors are `name()`, **not** `getName()`.
- ❌ Trying to `extends` more than one class — Java doesn't allow it; use interfaces.
- ❌ Using `==` to compare enum values is fine, but using `equals()` returns the same result — both work.
- ❌ Assuming a sealed parent forces all subtypes into the same file. **Same module**, not file. They must be in the same package only in the **unnamed module** scenario.
- ❌ Adding instance fields to a record body — illegal. All instance state lives in the header.
- ❌ Trying to call a non-static outer member from a `static` nested class.
- ❌ Forgetting that anonymous classes can only capture **effectively final** locals.
- ❌ Believing static methods are overridden — they are **hidden**, resolved by reference type.

---

## 6) Mental Model

Think of the type system as concentric rings:

1. **Class** — the default type with full freedom.
2. **Abstract class** — partially defined; expects extension.
3. **Interface** — pure contract with optional default helpers; no instance state.
4. **Sealed type** — a closed enum-like hierarchy with named subtypes.
5. **Enum** — a finite list of named singletons of the same type.
6. **Record** — immutable, exhaustive snapshot of named components.

Inheritance rules then narrow choices:
- Single inheritance for **classes**.
- Multiple inheritance for **interfaces**.
- Polymorphism uses runtime dispatch only for **instance methods**.

Records are essentially "POJOs as a one-liner with `equals/hashCode/toString` thrown in." Sealed types let you write **exhaustive** `switch` over a closed hierarchy.

---

## 7) Quick Revision

- Interface members are implicitly `public abstract` (methods) or `public static final` (fields).
- A class implementing two interfaces with the same default must override it.
- Enums: implicitly `final`, constructor implicitly `private`, methods overridable per constant.
- Sealed: subtypes are `final`, `sealed`, or `non-sealed`, in the same module.
- Records: header components → fields, accessors, canonical constructor, `equals`, `hashCode`, `toString`. Implicitly `final`.
- Compact constructor validates but cannot reassign `this.x`.
- Inner class accesses outer instance; static nested doesn't.
- Anonymous classes capture **effectively final** locals.
- `instanceof` pattern binds variable in scope where flow proves the type.
- Static methods are **hidden**, not overridden.
