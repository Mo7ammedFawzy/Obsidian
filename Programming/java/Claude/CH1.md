# CH1 — Building Blocks

## 1) Overview

This chapter sets the foundation: what makes a Java program valid, how it is compiled and executed, the eight primitive types, identifiers, literals, packages and imports, variables and their scope, and the lifecycle of objects. The OCP exam loves trick questions about file structure, identifier rules, literal formatting, default values, and order of initialization.

---

## 2) Key Concepts

### Source File Structure

A `.java` file may contain at most:
- **One package declaration** (must be the first non-comment line, optional).
- Zero or more **import declarations**.
- Any number of **type declarations** (classes, interfaces, enums, records).
- **At most one `public` top-level type**, and its name must match the file name.

```java
package com.example;          // 1. package
import java.util.List;        // 2. imports
public class Zoo {            // 3. public type matches file name
    static class Inner {}     //    other types may be non-public
}
```

### `main()` Method (JVM Entry Point)

A valid main signature for the JVM:

```java
public static void main(String[] args)
public static void main(String args[])
public static void main(String... args)
```

- `public` — JVM must reach it from outside.
- `static` — JVM calls it without instantiating the class.
- `void` — no return value.
- One parameter: an array (or varargs) of `String`.

### Compilation & Execution

```bash
javac Zoo.java        # Zoo.java -> Zoo.class (bytecode)
java Zoo              # JVM loads Zoo.class and calls main
java Zoo.java         # Java 11+ single-file source-code launcher
```

### JDK / JRE / JVM

- **JVM** runs bytecode — platform-specific implementation that makes Java portable.
- **JRE** = JVM + core libraries (no compiler).
- **JDK** = JRE + compiler (`javac`) + tools (`jar`, `jdeps`, etc.).

### Primitive Types

| Type | Bits | Range / Notes |
| --- | --- | --- |
| `boolean` | 1 | `true` / `false` |
| `byte` | 8 | -128…127 |
| `short` | 16 | -32 768…32 767 |
| `int` | 32 | ±2.1B |
| `long` | 64 | ±9.2 × 10¹⁸ — literal suffix `L` |
| `float` | 32 | IEEE-754 — literal suffix `f`/`F` |
| `double` | 64 | IEEE-754 (default for decimals) |
| `char` | 16 | Unsigned Unicode 0…65 535 |

### Literals

- Integer default type is `int`. Use `L` for `long`.
- Decimal literal default is `double`. Use `f` for `float`.
- Bases: decimal (`42`), hex (`0x2A`), octal (`052`), binary (`0b101010`).
- Underscores `_` may separate digits (not at start, end, or adjacent to `.` / sign): `1_000_000`.
- Char literal: single quotes `'A'`. Numeric escape: `'A'`.

### Identifiers

- Start with letter, `$`, or `_`. Cannot start with digit.
- Cannot be a reserved keyword (`class`, `int`, `static`, etc.).
- Cannot be `_` alone (Java 9+) — it's reserved.
- Are case-sensitive.

### Variables

- **Local** variables — declared inside methods/blocks; **must be initialized** before use; no default value.
- **Instance** variables — declared in a class; default to `0`/`false`/`null`.
- **Class (static)** variables — same defaults as instance, but per-class.

### Reference Types

Anything that's not primitive: classes, interfaces, arrays, enums. References hold an address (not the object). They may be `null`.

### `var` (Local Variable Type Inference, Java 10+)

```java
var x = 42;             // int
var list = new ArrayList<String>();
```

- Only for **local variables** with an initializer.
- Cannot be used for fields, method parameters, return types, or `null`-only initialization.

### Packages, Imports

- `import java.util.List;` — single type.
- `import java.util.*;` — every type (not subpackages).
- `import static java.lang.Math.PI;` — static member.
- Classes in the same package don't need imports. `java.lang` is auto-imported.
- A `class A` in package `p` may import `import p.A;` redundantly but legally.

### Garbage Collection

- An object is eligible when no live reference can reach it.
- `System.gc()` is a hint; the JVM may ignore it.
- `finalize()` is **deprecated**. Java 21 ships with no usable replacement other than `Cleaner` and `try-with-resources`.

### Initialization Order

1. **Static** initializers and static fields run when the class is loaded — top-down within the file.
2. **Instance** initializers and instance fields run before the constructor body — top-down.
3. Then the matching **constructor body**.

When a subclass is created: its `super(...)` runs first, recursively up to `Object`, then instance init blocks/fields, then the subclass constructor body.

---

## 3) Important Rules

- A file can have **only one** `public` top-level type, and its name must match the filename.
- Local variables have **no default values** — using them uninitialized is a compile error.
- Instance/class variables default to `0`/`false`/`null`.
- `int x = 0xFF_FF_FF_FF_FF;` won't compile — too large for `int`.
- Underscores in numeric literals can't sit next to `.`, sign, or be at the very start/end.
- `var` requires a non-null initializer in the same statement; `var x;` is illegal.
- The `String[] args` parameter of `main` must be the only parameter; the JVM ignores any other valid `main` overloads for entry.
- `package` (if present) must precede every `import`, which must precede every type declaration.
- `import a.b.*;` does **not** import `a.b.c.*;` — wildcards aren't recursive.
- Java is **pass-by-value** for everything; the value of a reference is the address.
- A class without an explicit constructor receives a **default no-arg public constructor**.
- `final` local variables must be assigned exactly once before use; final fields must be assigned by the end of the constructor (or in a static initializer for static finals).
- Wrapper types (`Integer`, `Double`, …) can be `null`; primitives can't.
- `String` literals are interned; `==` between two literals returns `true`, but between `new String("x")` and `"x"` returns `false`.

---

## 4) Code Examples

### Hello World with explicit modifiers

```java
package com.namasoft.intro;

public class Hello {
    public static void main(String... args) {
        System.out.println("Hello, world!");
    }
}
```

### Defaults vs. local-variable rule

```java
class Defaults {
    int instance;            // defaults to 0
    static String shared;    // defaults to null

    void demo() {
        int local;
        // System.out.println(local); // compile error: not initialized
        local = 5;
        System.out.println(local);
    }
}
```

### Initialization order

```java
class Init {
    static { System.out.println("static block"); }
    static int s = init("static field");

    { System.out.println("instance block"); }
    int i = init("instance field");

    Init() { System.out.println("constructor"); }
    static int init(String s) { System.out.println(s); return 0; }

    public static void main(String[] a) {
        new Init();
        new Init();
    }
}
/* Output:
static block
static field
instance block
instance field
constructor
instance block
instance field
constructor
*/
```

### `var` rules

```java
var n = 10;                       // int
var name = "Mo";                  // String
// var x;                         // ❌ no initializer
// var y = null;                  // ❌ cannot infer
// var z = { 1, 2, 3 };           // ❌ array initializer not allowed
```

### Tricky: numeric literals

```java
long big = 10_000_000_000L;       // OK
int hex = 0x2A;                   // 42
int bin = 0b1010;                 // 10
// int bad = 1_000_;              // ❌ trailing underscore
// int bad2 = _1000;              // ❌ leading underscore
double d = 3.14_15;               // OK
// double e = 3._14;              // ❌ underscore adjacent to '.'
```

### Tricky: import collisions

```java
import java.util.Date;
import java.sql.Date;             // ❌ same simple name twice
```

### Garbage-collection eligibility

```java
String s = new String("hello");
s = null;     // original String is now eligible for GC
```

---

## 5) Common Mistakes

- ❌ Using a local variable before initializing it.
- ❌ Naming the file differently from the public class.
- ❌ Putting `package` after an `import`.
- ❌ Wildcard imports for unrelated types — fine to compile, but doesn't recurse into subpackages.
- ❌ Treating `==` as value equality on `String` / wrapper types.
- ❌ Forgetting that `var` cannot infer a type from `null` alone.
- ❌ Adding `L` to the wrong literal: `0L` is fine, but `0X1L` (hex with int suffix omitted) might overflow if you forget `L` for huge numbers.
- ❌ Putting underscores in literal positions that aren't allowed.
- ❌ Believing `System.gc()` guarantees a collection.
- ❌ Defining two `public` classes in one file.

---

## 6) Mental Model

A Java program is a tree of **types** (classes, interfaces, enums, records) organized into **packages**. Each `.java` file is a *compilation unit* — it produces one or more `.class` files. The JVM then loads classes lazily and runs `main`.

Memory has two regions you must keep in mind:
- **Stack**: per-thread frames holding local variables and primitives.
- **Heap**: shared store for objects; references on the stack point here.

Initialization happens in a strict order: static stuff first (once per class), then instance stuff (once per object), then the constructor body. Any subclass chains its parent's initialization first.

Primitives carry **values**; references carry **addresses**. Java is *always* pass-by-value, so when you "pass an object" you pass the value of the reference — both caller and callee can mutate the same object, but reassignment is local.

---

## 7) Quick Revision

- One public top-level type per file; file name = class name.
- `main(String[] args)` — `public static void`, exactly one array/varargs parameter.
- 8 primitives: `boolean byte short int long float double char`.
- Default values: `0` / `false` / `null`. Locals have **no** default.
- Numeric literal suffixes: `L`/`l` for `long`, `F`/`f` for `float`, `D`/`d` for `double`.
- Bases: `0x` hex, `0b` binary, leading `0` octal; `_` separators allowed.
- `var` is **inference for locals only**; needs a non-null initializer.
- Init order: static (top-down) → instance (top-down) → constructor.
- `import a.*` doesn't cover subpackages; `java.lang` is auto-imported.
- Java is pass-by-value — including references.
