# CH8 — Lambdas and Functional Interfaces: Q & A

---

### Q1. Which lambda is invalid?

**Options**
- A) `() -> 42`
- B) `x -> x + 1`
- C) `(int x, y) -> x + y`
- D) `(x, y) -> x + y`

**Correct Answer:** **C**

**Explanation**
You may either type **all** parameters or **none**. Mixing typed and untyped (`int x, y`) is illegal. **A**, **B**, and **D** follow the rules.

---

### Q2. What's the output?

```java
int n = 5;
Runnable r = () -> System.out.println(n);
n = 10;
r.run();
```

**Options**
- A) `5`
- B) `10`
- C) Compile error
- D) Runtime error

**Correct Answer:** **C**

**Explanation**
`n` is captured by the lambda but reassigned afterward — it isn't *effectively final*. The compiler rejects the lambda. If `n = 10` were removed, the program prints `5`.

---

### Q3. Which interface in `java.util.function` represents a function `T → boolean`?

**Options**
- A) `Function<T, Boolean>`
- B) `Predicate<T>`
- C) `Supplier<Boolean>`
- D) `BiFunction<T, T, Boolean>`

**Correct Answer:** **B**

**Explanation**
`Predicate<T>` returns the primitive `boolean` and is the canonical choice. **A** technically works but boxes; the OCP exam expects `Predicate`.

---

### Q4. (True/False) `@FunctionalInterface` is required for a lambda to target an interface.

**Correct Answer:** **False**

**Explanation**
The annotation only **enforces** the SAM rule at compile time. Any interface with exactly one abstract method is implicitly functional and is a valid lambda target.

---

### Q5. Which method reference is correct for `String::length` to be assigned to?

**Options**
- A) `Supplier<Integer>`
- B) `Function<String, Integer>`
- C) `Consumer<String>`
- D) `BiFunction<String, String, Integer>`

**Correct Answer:** **B**

**Explanation**
`String::length` is an **unbound** reference: it takes a `String` and returns an `int` — i.e. `Function<String, Integer>`. **A** would need a bound reference like `s::length`.

---

### Q6. What does this print?

```java
Function<Integer, Integer> f = x -> x + 1;
Function<Integer, Integer> g = x -> x * 2;
System.out.println(f.andThen(g).apply(3));
```

**Options**
- A) `7`
- B) `8`
- C) `9`
- D) Compile error

**Correct Answer:** **B**

**Explanation**
`andThen` runs `f` first, then `g`: `(3+1)*2 = 8`. With `compose` it would be `g(f(...))` reversed → `3*2+1 = 7`.

---

### Q7. Which is **NOT** a built-in `java.util.function` interface?

**Options**
- A) `BiSupplier<T,U>`
- B) `BiConsumer<T,U>`
- C) `BiFunction<T,U,R>`
- D) `BinaryOperator<T>`

**Correct Answer:** **A**

**Explanation**
There's no `BiSupplier` — a Supplier takes no arguments by definition. The other three exist.

---

### Q8. What's the result?

```java
Predicate<String> notEmpty = s -> !s.isEmpty();
Predicate<String> short3   = s -> s.length() < 3;
boolean ok = notEmpty.and(short3).test("");
System.out.println(ok);
```

**Options**
- A) `true`
- B) `false`
- C) Compile error
- D) Throws exception

**Correct Answer:** **B**

**Explanation**
`""` is empty, so `notEmpty.test("")` is `false`. `and` short-circuits, so the combined predicate is `false`.

---

### Q9. What does `this` refer to inside a lambda?

**Options**
- A) The lambda itself
- B) The functional interface's anonymous instance
- C) The enclosing class instance
- D) `null` if not assigned

**Correct Answer:** **C**

**Explanation**
Unlike anonymous inner classes (where `this` is the anonymous instance), a lambda's `this` is the enclosing class's `this`. This is why lambdas cannot recursively reference themselves through `this`.

---

### Q10. Which lambda compiles?

**Options**
- A) `Comparator<Integer> c = (Integer a, b) -> a - b;`
- B) `Comparator<Integer> c = (a, b) -> a - b;`
- C) `Comparator<Integer> c = (Integer a, Integer b) -> { a - b; };`
- D) `Comparator<Integer> c = a, b -> a - b;`

**Correct Answer:** **B**

**Explanation**
**A** mixes typed and untyped parameters. **C**'s block lacks `return`. **D** is missing the parentheses required for two parameters.

---

### Q11. What is printed?

```java
Function<Integer,Integer> sq = n -> n * n;
List.of(1,2,3).forEach(n -> System.out.print(sq.apply(n) + " "));
```

**Options**
- A) `1 2 3 `
- B) `1 4 9 `
- C) `2 4 6 `
- D) Compile error

**Correct Answer:** **B**

**Explanation**
The lambda squares each element via `sq`. `1²=1, 2²=4, 3²=9`.

---

### Q12. Which is a **bound** method reference?

**Options**
- A) `Integer::parseInt`
- B) `String::length`
- C) `myList::add`
- D) `ArrayList::new`

**Correct Answer:** **C**

**Explanation**
`myList::add` binds the receiver to the specific `myList` instance. **A** is static, **B** is unbound, **D** is constructor.

---

### Q13. (True/False) A lambda's parameter may have the same name as a local variable in the enclosing scope.

**Correct Answer:** **False**

**Explanation**
Unlike inner classes, lambdas **share the local scope** of the enclosing method, so re-declaring a name causes a compile error.

---

### Q14. What's the output?

```java
List<Integer> ints = new ArrayList<>(List.of(3,1,2));
ints.sort((a,b) -> a - b);
System.out.println(ints);
```

**Options**
- A) `[1, 2, 3]`
- B) `[3, 2, 1]`
- C) `[3, 1, 2]`
- D) Compile error

**Correct Answer:** **A**

**Explanation**
`a - b` produces a natural-order comparator (ascending). `List.of(...)` is unmodifiable but is wrapped in a new mutable `ArrayList`, so `sort` works.

---

### Q15. Which built-in interface fits a function that **takes nothing and returns `T`**?

**Options**
- A) `Consumer<T>`
- B) `Supplier<T>`
- C) `UnaryOperator<T>`
- D) `Predicate<T>`

**Correct Answer:** **B**

**Explanation**
`Supplier<T>` has `T get()` — zero args, returns `T`. **A** is `T → void`, **C** is `T → T`, **D** is `T → boolean`.

---

### Q16. What's the result?

```java
Function<String, Integer> parse = Integer::parseInt;
System.out.println(parse.apply("42") + 1);
```

**Options**
- A) `421`
- B) `43`
- C) Compile error
- D) Runtime error

**Correct Answer:** **B**

**Explanation**
`parse.apply("42")` returns `Integer 42`, auto-unboxed for arithmetic; `42 + 1 = 43`. **A** would result from string concatenation, but `+` on two ints is numeric.

---

### Q17. (Multiple choice) Which statement about variable capture is true?

**Options**
- A) Lambdas can mutate captured local variables.
- B) Lambdas cannot reference instance fields.
- C) Captured locals must be effectively final.
- D) Lambdas cannot use static fields.

**Correct Answer:** **C**

**Explanation**
Locals must be effectively final (**C** correct). Lambdas can read **and write** instance/static fields freely (eliminates **A**, **B**, **D**).

---

### Q18. What's printed?

```java
BiFunction<Integer,Integer,Integer> op = Integer::sum;
System.out.println(op.apply(3, 4));
```

**Options**
- A) `7`
- B) `34`
- C) Compile error
- D) Runtime error

**Correct Answer:** **A**

**Explanation**
`Integer::sum` is a static method reference matching `(int a, int b) -> a + b`. Returns `7`.

---

### Q19. (True/False) `Consumer<T>` has both `andThen` and `compose` default methods.

**Correct Answer:** **False**

**Explanation**
`Consumer` has only `andThen`. `Function` has both `andThen` and `compose`. Composing in reverse for a consumer doesn't make sense (no return value to feed forward).

---

### Q20. Which lambda is ambiguous and must be cast?

```java
void run(Runnable r) { ... }
void run(Callable<?> c) { ... }
run(() -> doWork());
```

**Options**
- A) The compiler chooses `Runnable` automatically.
- B) The call is ambiguous; cast is required.
- C) The call always picks `Callable`.
- D) Compile error because `() -> doWork()` is invalid syntax.

**Correct Answer:** **B**

**Explanation**
With overloaded methods accepting different functional interfaces, the compiler can't pick a target type. You must cast: `run((Runnable)() -> doWork());`.
