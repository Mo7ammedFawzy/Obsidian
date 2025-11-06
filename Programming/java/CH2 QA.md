## ⚙️ Operators

### Q1: What are the main categories of operators in Java?

**A:**

- Arithmetic: `+ - * / % ++ --`
    
- Assignment: `= += -= *= /= %=`
    
- Relational: `< <= > >=`
    
- Equality: `== !=`
    
- Logical: `&& || !`
    
- Bitwise: `& | ^ ~ << >> >>>`
    
- Ternary: `?:`
    

---

### Q2: What is operator precedence?

**A:** It determines the order in which operators are evaluated.  
**Example:** `a + b * c` → `b * c` is evaluated before `+`.

---

### Q3: What is operator associativity?

**A:**  
It defines how operators of the same precedence are grouped:

- Most operators are **left-associative** (evaluated left → right).
    
- **Assignment (`=`)** and **ternary (`?:`)** are **right-associative**.
    

---

### Q4: How can you override precedence?

**A:** Use parentheses `()`. Example: `(a + b) * c`.

---

### Q5: What’s the difference between prefix and postfix increment?

**A:**

- `++x`: increments before use.
    
- `x++`: uses the value first, then increments.
    

---

### Q6: What does a compound assignment do?

**A:**  
Combines an operation and assignment:  
`x += 5` → `x = x + 5`.  
It also performs an implicit cast if needed (e.g., `byte += int` is allowed).

---

### Q7: What is the ternary operator syntax and use?

**A:**  
`condition ? valueIfTrue : valueIfFalse`  
Example:

```java
int max = (a > b) ? a : b;
```

---

### Q8: Difference between `&&` and `&` in conditionals?

**A:**

- `&&`: short-circuits (right side not evaluated if left is false).
    
- `&`: always evaluates both sides.
    

---

### Q9: Difference between `||` and `|`?

**A:**  
Same as above but for logical OR:

- `||`: short-circuits (skips right if left is true).
    
- `|`: always evaluates both.
    

---

### Q10: What is the use of bitwise operators?

**A:**  
They operate on bits (integers). Example:  
`5 & 3 → 1`, `5 | 3 → 7`, `5 ^ 3 → 6`, `~5 → -6`.

---

### Q11: What are the shift operators?

**A:**

- `<<` → left shift
    
- `>>` → signed right shift
    
- `>>>` → unsigned right shift (fills with 0s)
    

---

## 🔄 Conditional Statements

### Q12: How does an `if` statement work?

**A:**  
Executes a block if its condition is true.

```java
if (score > 90) grade = 'A';
```

---

### Q13: How does an `if-else` chain work?

**A:**  
Checks sequentially; only the first true condition runs.

---

### Q14: When should you use `switch` instead of `if-else`?

**A:**  
Use `switch` for comparing one variable to fixed constants.  
`if-else` is better for ranges or complex conditions.

---

### Q15: What data types are valid in a `switch` expression?

**A:**  
`byte`, `short`, `char`, `int`, `enum`, and `String`.

---

### Q16: What happens if you omit `break` in a `switch` case?

**A:**  
Execution **falls through** to the next case until a `break` or the end.

---

### Q17: What is the purpose of `default` in a `switch`?

**A:**  
Runs if no `case` matches. It can appear anywhere in the block.

---

## 🔁 Loop Constructs

### Q18: What’s the difference between `while` and `do-while` loops?

**A:**

- `while`: checks condition before running → may run **0 times**.
    
- `do-while`: checks after running → runs **at least once**.
    

---

### Q19: What are the three parts of a `for` loop?

**A:**

1. Initialization
    
2. Condition
    
3. Update  
    All are optional: `for(;;)` is an infinite loop.
    

---

### Q20: What is the enhanced `for` loop used for?

**A:**  
Iterates over arrays or collections.

```java
for (int n : nums) System.out.println(n);
```

---

## 🔚 Flow Control: `break` & `continue`

### Q21: What does `break` do?

**A:**  
Immediately exits the nearest loop or switch.

---

### Q22: What does `continue` do?

**A:**  
Skips the rest of the current loop iteration and jumps to the next one.

---

### Q23: How do labeled loops work?

**A:**  
You can label a loop and refer to it with `break` or `continue`.

```java
outer:
for (...) {
  for (...) {
    if (a) break outer;     // exits outer loop
    if (b) continue outer;  // next outer iteration
  }
}
```

---

## 💡 Common Interview & Exam Questions

### Q24: Which runs first in `a + b * c - d / e`?

**A:**  
`b * c` and `d / e` (multiplicative operators first).

---

### Q25: Can `switch` work with `long`, `float`, or `double`?

**A:**  
No. Only `byte`, `short`, `char`, `int`, `enum`, and `String` are allowed.

---

### Q26: What is short-circuit evaluation?

**A:**  
When `&&` or `||` skips the right-hand expression if the result is already known.

---

### Q27: What happens if you use `continue` in a `for` loop?

**A:**  
It jumps to the **update** expression, then checks the condition again.

---

### Q28: What’s the output of:

```java
int x = 1;
int y = x++ + ++x;
```

**A:**  
`y = 1 + 3 = 4`, `x = 3`.

---

### Q29: Why is `x += 5` not the same as `x = x + 5` in type promotion?

**A:**  
Compound assignment (`+=`) **casts implicitly** to the variable’s type;  
normal addition may cause a compile error with smaller types (e.g., `byte`).

---

### Q30: Can `break` and `continue` be used outside loops?

**A:**  
No. They’re valid only inside loops or `switch` blocks.

---