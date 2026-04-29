## ⚡ Exception Hierarchy

### Q1: What is the top of Java's exception hierarchy?

**A:** `java.lang.Throwable`. Its two main subclasses are `Error` and `Exception`. `RuntimeException` is a subclass of `Exception`.

```
Throwable
 ├── Error          (system-level, do not catch)
 └── Exception      (checked)
      └── RuntimeException  (unchecked)
```

---

### Q2: What's the difference between checked and unchecked exceptions?

**A:**

- **Checked** (subclass of `Exception` but **not** `RuntimeException`) — must be **caught** or **declared** with `throws`. E.g. `IOException`, `SQLException`.
- **Unchecked** (subclass of `RuntimeException`) — neither catching nor declaring is required. E.g. `NullPointerException`, `ArithmeticException`.

---

### Q3: What is an `Error`, and should you catch one?

**A:** `Error` represents system-level failures the JVM raises (e.g., `OutOfMemoryError`, `StackOverflowError`). They generally **should not be caught** — the application typically can't recover.

---

## 🧯 try / catch / finally

### Q4: What's the basic try-catch syntax?

**A:**

```java
try {
    // risky code
} catch (SpecificException e) {
    // handle
} catch (AnotherException e) {
    // handle
}
```

---

### Q5: What is the rule for catch-block ordering?

**A:** **More specific exceptions must be caught before more general ones.** Otherwise the later block is unreachable and won't compile:

```java
try { ... }
catch (Exception e) { }      // general
catch (IOException e) { }    // DOES NOT COMPILE — already handled above
```

---

### Q6: When does the `finally` block run?

**A:** **Always** — whether the `try` completed normally, threw an exception, or returned. Used for cleanup (closing files, releasing locks).

---

### Q7: Can a `finally` block prevent an exception from propagating?

**A:** Yes — if `finally` itself `return`s or throws, it overrides whatever the `try`/`catch` was doing. This is generally an **anti-pattern**.

---

### Q8: Can `try` be used without `catch`?

**A:** Yes, as long as it has a `finally` (or it's a try-with-resources). A bare `try { }` is a compile error.

---

## 🔀 Multi-catch (Java 7+)

### Q9: What is multi-catch?

**A:** Catching multiple unrelated exceptions in one `catch` block:

```java
try { ... }
catch (IOException | SQLException e) {
    System.out.println(e.getMessage());
}
```

---

### Q10: What are the rules for multi-catch?

**A:**

- The exceptions cannot be in a **parent-child relationship** (e.g., `IOException | Exception` won't compile — redundant).
- The variable `e` is **implicitly final** — you cannot reassign it.

---

## 🔒 Try-with-Resources (Java 7+)

### Q11: What is try-with-resources?

**A:** A `try` that declares one or more resources implementing `AutoCloseable`; the JVM **automatically closes** them in reverse declaration order:

```java
try (BufferedReader br = new BufferedReader(new FileReader("f.txt"))) {
    System.out.println(br.readLine());
}
```

---

### Q12: What interface must a resource implement?

**A:** `java.lang.AutoCloseable` (or its subinterface `java.io.Closeable`).

---

### Q13: In what order are resources closed?

**A:** **Reverse** of declaration order — last opened, first closed.

---

### Q14: What is a *suppressed* exception?

**A:** If both the `try` body and `close()` throw, the close exception is attached as **suppressed** to the primary one. Retrieve it via `Throwable.getSuppressed()`.

---

## 🚀 throw and throws

### Q15: What's the difference between `throw` and `throws`?

**A:**

- `throw` — *statement* that actually raises an exception: `throw new IllegalArgumentException();`
- `throws` — *method clause* declaring which checked exceptions a method can propagate: `void m() throws IOException { ... }`

---

### Q16: When must a method declare `throws`?

**A:** When it can propagate a **checked** exception that it does not catch. Unchecked exceptions don't require declaration.

---

### Q17: What rules apply when overriding a method that declares `throws`?

**A:** The override:

- May throw the **same** checked exceptions, **narrower** ones, or **none**.
- **Cannot** add new or broader checked exceptions.
- May freely throw any unchecked exceptions.

---

## 🐛 Common Runtime Exceptions

### Q18: List common runtime exceptions and their causes.

**A:**

| Exception                        | Cause                                    |
| -------------------------------- | ---------------------------------------- |
| `NullPointerException`           | Dereferencing `null`                     |
| `ArrayIndexOutOfBoundsException` | Invalid array index                      |
| `ClassCastException`             | Invalid object cast                      |
| `NumberFormatException`          | Bad numeric parsing (`Integer.parseInt`) |
| `ArithmeticException`            | Integer divide-by-zero                   |
| `IllegalArgumentException`       | Method argument out of valid range       |
| `IllegalStateException`          | Object in wrong state for the operation  |

---

### Q19: Does `1.0 / 0` throw `ArithmeticException`?

**A:** **No.** Floating-point division by zero produces `Infinity`, `-Infinity`, or `NaN`. Only **integer** division by zero throws.

---

## 💡 Conceptual & Exam Traps

### Q20: What's the output if `try` and `finally` both `return`?

```java
int m() {
    try { return 1; }
    finally { return 2; }
}
```

**A:** Returns **`2`** — `finally`'s `return` overrides the `try`'s. (Bad practice but legal.)

---

### Q21: What is *flow control via exceptions*, and why is it bad?

**A:** Using exceptions to drive normal program logic (e.g., catching `NumberFormatException` to test if a string is numeric). It's slow, hard to read, and hides intent. Use proper checks instead.

---

### Q22: Can you catch multiple exceptions and rethrow them?

**A:** Yes — typically wrapping in a higher-level exception preserves the cause:

```java
try { ... }
catch (IOException e) {
    throw new MyAppException("read failed", e);   // 'e' as cause
}
```

---

### Q23: Difference between an empty `catch` and rethrowing?

**A:** An empty `catch` **swallows** the exception silently (anti-pattern). Rethrowing keeps the failure visible up the call stack — almost always preferable.

---

### Q24: Best-practice checklist.

**A:**

- ✅ Catch the **most specific** exception you can handle.
- ✅ Use **try-with-resources** for `AutoCloseable` resources.
- ❌ Avoid empty `catch` blocks.
- ❌ Don't catch `Error` unless you really know why.
- ❌ Don't use exceptions for normal flow control.
- ✅ Always preserve the original cause when wrapping.

---

### Q25: Will this compile?

```java
try { throw new IOException(); }
catch (Exception e) { }
catch (IOException e) { }
```

**A:** **No.** `IOException` is a subclass of `Exception`; the second `catch` is unreachable. Reorder so specific exceptions come first.

---

### Q26: Sample full example.

```java
public static void readFile(String fileName) {
    try (BufferedReader br = new BufferedReader(new FileReader(fileName))) {
        String line = br.readLine();
        int num = Integer.parseInt(line);
        System.out.println(num);
    } catch (FileNotFoundException e) {
        System.out.println("File not found!");
    } catch (IOException e) {
        System.out.println("IO error!");
    } catch (NumberFormatException e) {
        System.out.println("Invalid number format!");
    }
}
```

Notes:

- `FileNotFoundException` (subclass of `IOException`) is caught **before** the more general `IOException`.
- `NumberFormatException` is unchecked but can still be caught for cleaner messaging.
- The `BufferedReader` is closed automatically by try-with-resources.
