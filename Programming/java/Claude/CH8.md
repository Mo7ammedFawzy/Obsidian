# CH8 — Lambdas and Functional Interfaces

## 1) Overview

Java's lambda expressions let you treat behavior as data — passing code into methods just like values. They rest on **functional interfaces**: any interface with exactly one abstract method (SAM). The chapter also covers **method references**, **variable capture** rules, and the built-in functional interfaces in `java.util.function`.

---

## 2) Key Concepts

### Lambda Expression Syntax

```java
(parameters) -> expression
(parameters) -> { statements; }
```

- Parentheses are optional with **exactly one** untyped parameter: `x -> x*2`.
- Parameter types are optional but, if specified, must be supplied for **every** parameter.
- A single-expression body has an implicit `return`; a block body needs explicit `return`.

### Functional Interfaces

- An interface with exactly **one abstract method** (the SAM).
- May have any number of `default`, `static`, `private` methods.
- `@FunctionalInterface` is optional but enforces the contract at compile time.
- Methods inherited from `Object` (`equals`, `hashCode`, `toString`) **do not count** as abstract.

### Built-in Functional Interfaces (`java.util.function`)

| Interface         | Method        | Signature           |
| ----------------- | ------------- | ------------------- |
| `Supplier<T>`     | `get()`       | `() -> T`           |
| `Consumer<T>`     | `accept(T)`   | `T -> void`         |
| `BiConsumer<T,U>` | `accept(T,U)` | `(T,U) -> void`     |
| `Predicate<T>`    | `test(T)`     | `T -> boolean`      |
| `BiPredicate<T,U>`| `test(T,U)`   | `(T,U) -> boolean`  |
| `Function<T,R>`   | `apply(T)`    | `T -> R`            |
| `BiFunction<T,U,R>` | `apply(T,U)`| `(T,U) -> R`        |
| `UnaryOperator<T>`| `apply(T)`    | `T -> T`            |
| `BinaryOperator<T>` | `apply(T,T)`| `(T,T) -> T`        |

There are also primitive specializations: `IntPredicate`, `ToLongFunction<T>`, `IntFunction<R>`, etc., that avoid boxing.

### Method References (`::`)

Four flavors:

| Form                      | Example                  | Equivalent lambda             |
| ------------------------- | ------------------------ | ----------------------------- |
| Static                    | `Integer::parseInt`      | `s -> Integer.parseInt(s)`    |
| Bound (specific instance) | `s::length` where s is String | `() -> s.length()`        |
| Unbound (any instance)    | `String::length`         | `s -> s.length()`             |
| Constructor               | `ArrayList::new`         | `() -> new ArrayList<>()`     |

### Variable Capture

- Lambdas can read **local variables**, **method parameters**, **fields**, and **static fields**.
- Captured **locals/parameters** must be **effectively final** (assigned exactly once).
- **Fields** can be reassigned freely — they are accessed through the implicit `this`.

### Composition

- `Predicate`: `and`, `or`, `negate`, plus static `isEqual`, `not`.
- `Function`: `andThen`, `compose`.
- `Consumer`: `andThen`.

---

## 3) Important Rules

- Exactly **one parameter without parentheses** is allowed: `x -> ...`. Zero, two, or any explicitly typed parameter must use parentheses.
- You cannot mix typed and untyped parameters: `(int x, y) -> ...` is illegal.
- A lambda cannot redeclare a name that already exists in its enclosing scope (no shadowing of locals).
- A lambda's body returning `void` cannot end with a value-producing expression unless the expression itself is a `void` call.
- Method references demand a **compatible target** functional interface; the resolved overload is the one whose parameter list matches the SAM.
- Inside a lambda, `this` refers to the **enclosing class** (not the lambda itself), unlike anonymous classes where `this` is the anonymous instance.
- A `Predicate<T>` can be chained: `p1.and(p2).or(p3.negate())`.
- For overloaded SAM targets the compiler may need an **explicit cast** to disambiguate: `Object o = (Runnable)() -> ...;`
- The **target type** drives lambda inference. Without one (e.g. assigning to `Object`), the lambda doesn't compile.
- Arrays in lambdas: you can mutate array elements (`arr[0]++`), because the array reference itself is effectively final even if its contents change.

---

## 4) Code Examples

### Filtering with a Predicate

```java
List<String> names = List.of("Mo", "Ali", "Yara");
names.stream()
     .filter(n -> n.length() > 2)
     .forEach(System.out::println);
```

### Custom functional interface

```java
@FunctionalInterface
interface Calc { int apply(int a, int b); }

Calc add = (a, b) -> a + b;
Calc mul = (a, b) -> { return a * b; };
System.out.println(add.apply(3, 4));   // 7
```

### All four method-reference flavors

```java
Function<String, Integer> f1 = Integer::parseInt;     // static
String s = "hello";
Supplier<Integer> f2 = s::length;                     // bound
Function<String, Integer> f3 = String::length;        // unbound
Supplier<List<String>> f4 = ArrayList::new;           // constructor
```

### Effectively-final trap

```java
int n = 0;
Runnable r = () -> System.out.println(n);   // OK
n++;                                        // ❌ now lambda doesn't compile
```

### Composition

```java
Predicate<Integer> positive = x -> x > 0;
Predicate<Integer> even     = x -> x % 2 == 0;
Predicate<Integer> positiveEven = positive.and(even);

Function<Integer,Integer> times2 = x -> x * 2;
Function<Integer,Integer> plus3  = x -> x + 3;
times2.andThen(plus3).apply(5);   // (5*2)+3 = 13
times2.compose(plus3).apply(5);   // (5+3)*2 = 16
```

### `this` semantics

```java
class Counter {
    int n = 0;
    Runnable inc = () -> this.n++;     // 'this' = enclosing Counter
}
```

---

## 5) Common Mistakes

- ❌ Mutating a captured local variable (it must be effectively final).
- ❌ Forgetting parentheses for zero or multi-arg lambdas.
- ❌ Using `return` in a single-expression lambda — should be either `x -> x+1` or `x -> { return x+1; }`, not `x -> return x+1;`.
- ❌ Treating `@FunctionalInterface` as required — it isn't, it's a check.
- ❌ Confusing bound vs unbound method references; if the SAM takes the receiver as first arg, use **unbound**.
- ❌ Assuming `this` inside a lambda points at the lambda — it points at the enclosing instance.
- ❌ Trying to compose `Consumer.compose` (doesn't exist; only `andThen`).
- ❌ Boxing performance: using `Function<Integer,Integer>` instead of `IntUnaryOperator` for primitive math.
- ❌ Overloaded targets: an unqualified lambda can be ambiguous — the compiler will refuse to choose.

---

## 6) Mental Model

A lambda is **syntactic sugar** for an anonymous class implementing a functional interface, with two crucial differences:
- It has **no own `this`** — `this` refers to the enclosing class.
- Variable capture follows the **effectively final** rule, so the JVM can implement lambdas via `invokedynamic` without copying mutable state.

Think of it as: *"the compiler synthesizes a method that matches the SAM and packages it as an instance of the target interface."*

The built-in functional interfaces are organized along three dimensions:
1. **Arity**: `0 → Supplier`, `1 → Consumer/Function/Predicate/UnaryOperator`, `2 → BiConsumer/BiFunction/BiPredicate/BinaryOperator`.
2. **Returns boolean?** → `Predicate`/`BiPredicate`.
3. **Returns void?** → `Consumer`/`BiConsumer`.

When a function takes the same type and returns it, prefer the *Operator* alias (`UnaryOperator<T>`, `BinaryOperator<T>`).

---

## 7) Quick Revision

- A functional interface has exactly one abstract method.
- `@FunctionalInterface` is optional; it enforces SAM at compile time.
- Lambda body: expression has implicit `return`; block body needs explicit `return`.
- Captured locals must be **effectively final**; fields don't need to be.
- `this` in a lambda = enclosing instance.
- Method references: **static**, **bound**, **unbound**, **constructor**.
- `Predicate` composes with `and`, `or`, `negate`.
- `Function` composes with `andThen` (forward) and `compose` (reverse).
- Use primitive specializations (`IntFunction`, `ToIntFunction`) to avoid boxing.
- Without a target type, a lambda doesn't compile.
