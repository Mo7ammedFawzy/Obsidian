# CH5 — Methods and Encapsulation

## 1) Overview

This chapter is about how Java declares and calls methods, controls access, and encapsulates state. Topics include modifiers (`public`, `private`, `protected`, package-private, `static`, `final`, `abstract`), parameters (varargs, autoboxing), method overloading rules, return types, encapsulation patterns, and the lambda-friendly emphasis on small, well-encapsulated APIs. Exam questions often hinge on the *exact* overload chosen by the compiler, the role of `static` initialization, and access boundaries between packages.

---

## 2) Key Concepts

### Method Anatomy

```java
[modifiers] returnType name([params]) [throws ...] { body }
```

- **Modifiers**: access (`public`, `protected`, `private`, none) plus optional non-access (`static`, `final`, `abstract`, `synchronized`, `native`, `default`).
- **Return type**: a primitive, reference type, or `void`.
- **Parameters**: zero or more, optionally **varargs** (last only): `String...`.
- **Throws**: declares checked exceptions.
- **Body**: braces (omit only for `abstract` and interface methods without body).

### Access Modifiers

| Modifier | Same class | Same package | Subclass (other pkg) | Other |
| --- | --- | --- | --- | --- |
| `private` | ✅ | ❌ | ❌ | ❌ |
| (default / package) | ✅ | ✅ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| `public` | ✅ | ✅ | ✅ | ✅ |

### Non-Access Modifiers

- `static` — belongs to the class, not instances. Called via `Class.method()` or qualified.
- `final` — for variables: assigned once. For methods: cannot be overridden. For classes: cannot be extended.
- `abstract` — no body; class must be `abstract` if any method is.
- `synchronized` — acquires intrinsic lock.
- `native` / `strictfp` (deprecated) / `default` (interfaces).

### Encapsulation

- Make fields `private`, expose them via getters/setters.
- Records (Java 16+) automate the read-only case.
- Java Beans convention: `getX()` / `setX()` / `isX()` for boolean.

### Overloading

Same method name, different parameter list (different types, count, or order — return type alone is **not enough**).

Resolution preference (best match wins):
1. **Exact** type match.
2. **Widening** primitives (`int → long`).
3. **Autoboxing** / unboxing.
4. **Varargs**.

### Pass-by-Value

- Primitives: copies of the value.
- References: copy of the reference (so the callee can mutate the same object but cannot reassign the caller's variable).

### Varargs

```java
void log(String level, String... messages) { ... }
log("INFO", "a", "b");        // OK
log("INFO");                  // OK — empty array
log("INFO", new String[]{"a"});// OK
```

- `String...` is essentially `String[]` at the call site.
- Varargs must be the **last** parameter.
- Only **one** varargs per method.

### `this` and `super`

- `this` — current instance reference; also used for constructor chaining `this(args)` (must be first statement).
- `super` — parent reference; `super(args)` invokes a parent constructor (must be first statement, mutually exclusive with `this(...)`).

### Static Members

- Static fields/methods belong to the class.
- A static method can only access static state directly. To touch instance state, it must receive an instance.
- Static initializers run once when the class is loaded.

### Effectively Final

A local variable is *effectively final* if it isn't modified after assignment — a requirement for capture in lambdas and anonymous inner classes.

---

## 3) Important Rules

- A method without an access modifier has **package-private** visibility.
- `protected` is broader than package-private — it also exposes to subclasses in **other** packages.
- A static method **cannot** access instance fields/methods directly.
- Overloading is resolved at **compile time** based on declared (static) types.
- Overriding (instance methods) is resolved at **runtime** based on the actual object.
- Return type alone cannot distinguish overloaded methods.
- A method with a varargs parameter can be invoked with zero arguments.
- `this()` and `super()` must be the **first statement** of a constructor; only one of them may appear.
- A constructor without a `this`/`super` call gets an implicit `super();`. If the parent has no no-arg constructor, this fails to compile.
- A `final` local variable can be assigned exactly once.
- A `final` field must be assigned by the end of every constructor (or in a static initializer for `static final` fields).
- Constants are conventionally `public static final` and `UPPER_SNAKE_CASE`.
- Two methods cannot differ only in autoboxing — `void m(int)` and `void m(Integer)` are valid overloads but resolution goes for **exact** match first.
- A method's `throws` clause may be widened by overloads but **narrowed** by overrides only.
- Constructors are **not inherited**.

---

## 4) Code Examples

### Encapsulation pattern

```java
public class Person {
    private String name;
    private int age;

    public String getName() { return name; }
    public void setName(String name) {
        if (name == null) throw new IllegalArgumentException();
        this.name = name;
    }
    public int getAge() { return age; }
    public void setAge(int age) {
        if (age < 0) throw new IllegalArgumentException();
        this.age = age;
    }
}
```

### Constructor chaining

```java
class A {
    A() { this("default"); }
    A(String name) { System.out.println("A(" + name + ")"); }
}

class B extends A {
    B() {
        super("B");                  // calls A(String)
        System.out.println("B()");
    }
}
```

### Overload resolution preference

```java
void m(int x) { System.out.println("int"); }
void m(long x) { System.out.println("long"); }
void m(Integer x) { System.out.println("Integer"); }
void m(int... xs) { System.out.println("varargs"); }

m(5);          // "int"      (exact)
m(5L);         // "long"     (exact)
m((Integer)5); // "Integer"  (exact)
// Without int overload, m(5) prefers "long" (widening) over "Integer" (boxing) over varargs
```

### Varargs caveat

```java
void greet(String... names) {
    for (String n : names) System.out.println("Hi, " + n);
}

greet();                       // OK
greet("a", "b");               // OK
greet((String[])null);         // calling with null array → NPE on iteration
```

### Pass-by-value pitfall

```java
void clear(List<Integer> xs) { xs.clear(); }       // mutates same list
void replace(List<Integer> xs) { xs = List.of(); } // local; caller unaffected
```

### Tricky: `this` to disambiguate shadowing

```java
class X {
    int n;
    void set(int n) { this.n = n; }    // 'this.n' = field; 'n' = parameter
}
```

### Tricky: static method via instance

```java
class C { static int s = 5; }
C c = null;
System.out.println(c.s);    // OK — resolved as C.s, no NPE
```

### Tricky: missing super constructor

```java
class P { P(String x) {} }
class Q extends P { /* compile error: implicit super() not found */ }
```

---

## 5) Common Mistakes

- ❌ Overloading by changing only the return type.
- ❌ Calling an instance method from a static method.
- ❌ Forgetting that `this(...)` and `super(...)` must be first statement (and mutually exclusive).
- ❌ Believing `protected` means "subclass only" — it also includes the package.
- ❌ Mutating method parameters and expecting the caller's variable to change (pass-by-value).
- ❌ Putting varargs anywhere but the last parameter.
- ❌ Two varargs in one method (illegal).
- ❌ Writing `final int x;` as an instance field and never assigning it (compile error).
- ❌ Forgetting that `final` on a method means *can't override*, while `final` on a class means *can't extend*.
- ❌ Treating `static final` constants as immutable when they reference mutable objects.

---

## 6) Mental Model

A method is a **named transformation** with three parts: a contract (signature), a behavior (body), and visibility (modifiers).

Encapsulation is about **drawing a private fence** around state and offering a small, controlled set of methods to interact with it. Records make the trivial case ergonomic; classes give you all the levers when you need invariants or behavior.

Two resolution mechanisms to keep distinct:
- **Compile-time**: overloading, static method calls, field references — based on declared types.
- **Runtime**: instance method dispatch (overriding) — based on actual object.

Pass-by-value is the **single rule** for arguments. Treat the argument variable as a fresh local in the callee — for objects it points at the same heap object, but reassigning it doesn't echo back.

The overload-resolution ladder (exact → widen → autobox → varargs) is the surest way to reason about which `m(...)` the compiler picks.

---

## 7) Quick Revision

- 4 access levels: `private`, package, `protected`, `public`.
- `static` belongs to the class; can't access instance members directly.
- `final`: variable (assign once), method (no override), class (no extend).
- Overloading differs by parameter list; return type alone doesn't count.
- Resolution: exact > widening > autoboxing > varargs.
- `this(...)` / `super(...)` must be first; mutually exclusive.
- Constructors aren't inherited.
- Varargs must be last; only one allowed.
- Pass-by-value: primitives copy values, references copy addresses.
- Effectively final = unmodified after init (required for lambda capture).
