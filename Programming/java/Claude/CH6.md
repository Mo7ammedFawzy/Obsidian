# CH6 — Class Design

## 1) Overview

Class design is about how Java types relate to each other: **inheritance**, **constructor chaining**, **method overriding vs. hiding**, **abstract classes**, **interfaces** (with default and static helpers), and **polymorphism with safe casting**. The exam tests subtle modifier interactions, constructor mechanics when parents lack no-arg constructors, the difference between hiding and overriding, and the diamond-default rule for interfaces.

---

## 2) Key Concepts

### Inheritance

- A class `extends` **at most one** parent.
- All classes implicitly extend `java.lang.Object`.
- A class may `implements` multiple interfaces.
- `final class` cannot be extended; `final method` cannot be overridden.

### Constructors and Inheritance

- A subclass constructor must start with `this(...)` or `super(...)`. If neither is written, an implicit `super()` is inserted.
- Constructor execution chains **from `Object` down to the actual class**.
- Constructors are **not** inherited.

### Overriding vs. Hiding

| | Overriding | Hiding |
| --- | --- | --- |
| Applies to | Instance methods | Static methods, fields |
| Resolution | Runtime type | Reference (compile-time) type |
| Modifiers | Same name + parameters; widened return (covariant); same/wider visibility | Same signature with `static` |

Override rules:
- Same parameter list, same name.
- Return type same or **covariant**.
- Cannot reduce visibility.
- Cannot throw broader **checked** exceptions.
- Cannot be `final`/`private`/`static` in parent (`private`/`static` aren't overridable; `final` forbids it).

### Abstract Classes

- `abstract` keyword on the class.
- May contain abstract and concrete methods, fields, constructors, static members.
- Cannot be instantiated.
- A concrete subclass must implement every inherited abstract method.

### Interfaces

- Members:
  - `public abstract` instance methods (default modifiers).
  - `public static final` constants.
  - `default` methods (instance, with body).
  - `static` methods (called via interface name).
  - `private` and `private static` helpers (Java 9+).
- A class can implement **multiple** interfaces; an interface can `extends` multiple interfaces.
- Resolves "diamond" default conflicts by forcing override.

### Polymorphism and Casting

- A parent reference may hold a child object — instance method calls dispatch dynamically.
- **Upcast**: implicit, always safe (`Animal a = new Dog();`).
- **Downcast**: explicit; may throw `ClassCastException`.
- `instanceof` (with optional pattern variable) avoids the throw.

### `var` & polymorphism

`var d = new Dog();` infers `Dog`; calls dispatch by the actual `Dog` type. Use the parent type explicitly when you want polymorphism.

---

## 3) Important Rules

- A class without a constructor gets an implicit `public Class() { super(); }`.
- If the parent has no no-arg constructor and you don't call `super(args)`, the subclass fails to compile.
- Constructors are not inherited but are *chained*.
- Override visibility may **widen** (`protected` → `public`) but never **narrow**.
- The override may throw the same/narrower checked exceptions, **never** broader ones.
- The override return type must be the same or a **subtype** (covariant). For primitives, it must be exactly the same.
- A `static` method declared in both parent and subclass with the same signature **hides** — it's not virtual dispatch.
- Variables (instance or static) are also resolved by reference type (hiding).
- A `private` method in the parent is invisible in the child — declaring the same signature in the child creates a **new method**, not an override.
- `@Override` is optional but always advisable; it forces compile-time verification.
- An interface field is **always** `public static final`.
- An interface's instance method without body is **always** `public abstract`.
- A class implementing two interfaces with the same default method **must** override it.
- A `default` method may be overridden, including by `abstract` to force subclasses to implement it.
- An `abstract` class may have constructors (called via `super(...)`).
- An abstract method **cannot** be `final`, `private`, or `static`.
- `Object` defines `equals`, `hashCode`, `toString`, `getClass`, `clone`, `finalize`, `wait`/`notify`/`notifyAll`. `clone` is conventionally avoided; `finalize` is deprecated.
- Sealed types (Java 17+) restrict subclasses to a `permits` list (covered in CH7 in detail).

---

## 4) Code Examples

### Inheritance & override

```java
class Animal {
    public String speak() { return "?"; }
}
class Dog extends Animal {
    @Override public String speak() { return "Woof"; }
}

Animal a = new Dog();
System.out.println(a.speak());   // Woof
```

### Covariant return

```java
class Box {
    public Object peek() { return null; }
}
class StringBox extends Box {
    @Override public String peek() { return "x"; }   // covariant
}
```

### Constructor chaining (parent has no no-arg)

```java
class Vehicle {
    Vehicle(String make) { /* ... */ }
}
class Car extends Vehicle {
    Car() {
        super("Toyota");          // required
    }
}
```

### Method hiding (static)

```java
class P { static String hi() { return "P"; } }
class C extends P { static String hi() { return "C"; } }

P p = new C();
System.out.println(p.hi());      // P  (resolved by reference type)
System.out.println(C.hi());      // C
```

### Diamond defaults

```java
interface A { default String id() { return "A"; } }
interface B { default String id() { return "B"; } }
class AB implements A, B {
    @Override public String id() { return A.super.id() + B.super.id(); }
}
```

### Abstract class

```java
abstract class Shape {
    abstract double area();
    public String describe() { return "area=" + area(); }
}
class Circle extends Shape {
    double r;
    Circle(double r) { this.r = r; }
    @Override double area() { return Math.PI * r * r; }
}
```

### Interface with default + private helper

```java
interface Greeter {
    default String greet(String n) { return prefix() + n; }
    private String prefix() { return "Hi, "; }
}
```

### Pattern matching for safe downcast

```java
Animal a = ...;
if (a instanceof Dog d) {
    d.fetch();
}
```

### Tricky: variable hiding

```java
class A { int n = 1; }
class B extends A { int n = 2; }

A a = new B();
System.out.println(a.n);     // 1  (resolved by reference type)
```

### Tricky: `private` "override"

```java
class P { private void m() { System.out.println("P"); } }
class C extends P { void m() { System.out.println("C"); } }

P p = new C();
// p.m();    // ❌ private — not visible from outside
((C)p).m();  // C
```

---

## 5) Common Mistakes

- ❌ Forgetting the implicit `super()` when the parent lacks a no-arg constructor.
- ❌ Believing static methods can be overridden — they're hidden.
- ❌ Accessing fields polymorphically (they aren't — fields hide).
- ❌ Reducing visibility on an override.
- ❌ Throwing a broader checked exception in an override.
- ❌ Marking an abstract method `final`/`static`/`private`.
- ❌ Putting two `default` methods with the same signature into a class without overriding (compile error).
- ❌ Writing both `this(...)` and `super(...)` in the same constructor.
- ❌ Casting unrelated reference types (compile error) vs. related (`ClassCastException` at runtime).
- ❌ Relying on `==` to compare objects of two different classes when polymorphism kicks in.

---

## 6) Mental Model

Think of a class hierarchy as a **chain of contracts**: each subclass narrows or specializes the contract of its parent. The compiler ensures compatibility:
- **Contract widening** is forbidden (you can't surprise callers by demanding more).
- **Implementation widening** is allowed (you can do more, return a subtype, expose more access).

Dispatch comes in two flavors:
- **Dynamic** (instance methods) — driven by the actual object.
- **Static** (everything else: static methods, fields, private members) — driven by the declared reference type.

Constructors aren't inherited but are *required to chain*. The first thing any constructor does is call its parent's constructor — failure to satisfy that is a compile error.

Interfaces give Java a controlled form of multiple inheritance: structural inheritance of abstract methods (always allowed) and **default** methods (with conflict resolution required when collisions occur).

For safe downcasting, prefer pattern matching with `instanceof` — it's both safer and clearer than a manual cast.

---

## 7) Quick Revision

- One parent class; multiple interfaces.
- `final` class can't be extended; `final` method can't be overridden.
- Override: same signature, covariant return, ≤ same exceptions, ≥ same visibility.
- Static methods are **hidden**, not overridden.
- Fields are also **hidden**, not polymorphic.
- Implicit `super()` is inserted; needs a matching parent constructor.
- Constructors are *not* inherited.
- Abstract methods can't be `final`/`private`/`static`.
- Diamond default conflict requires explicit override.
- Use `instanceof` with pattern binding for safe downcasts.
