# Statements & Decision Making

- A **statement** is a complete unit of execution ending in `;`.
- A **block** is a group of zero or more statements wrapped in `{ }` and treated as one statement.
- A **control flow statement** breaks the top-down execution order using *decision-making*, *looping*, or *branching*.

## The `if` Statement

- Executes a block **only if** a `boolean` expression evaluates to `true`.
	```java
	if (hourOfDay < 11) {
	    System.out.println("Good Morning");
	}
	```
- Braces `{}` are optional for a single statement, but **always recommended** to avoid bugs.
	```java
	if (hourOfDay < 11)
	    System.out.println("Good Morning");   // legal
	    morningGreetingCount++;               // ALWAYS executes (not part of if!)
	```
- The condition **must** be a `boolean`. Unlike C/C++, Java does **not** treat `0`/non-zero as `false`/`true`.
	```java
	int x = 1;
	if (x)        // DOES NOT COMPILE
	if (x = 5)    // DOES NOT COMPILE (assignment yields int)
	if (x == 5)   // OK
	```

## The `else` Statement

- Provides an alternative branch when the `if` condition is `false`.
	```java
	if (hourOfDay < 11) {
	    System.out.println("Good Morning");
	} else if (hourOfDay < 15) {
	    System.out.println("Good Afternoon");
	} else {
	    System.out.println("Good Evening");
	}
	```
- Java evaluates conditions **top-down** and stops at the first match.

## Pattern Matching with `instanceof` (Java 16+)

- Combines a type check with a cast and variable binding into one expression.
	```java
	Number num = 123;
	if (num instanceof Integer data) {
	    System.out.println(data.intValue());   // 'data' is auto-cast to Integer
	}
	```
- The pattern variable (`data`) is only **in scope** where the compiler can prove the type.
- **Flow scoping**: scope depends on control flow, not block boundaries.
	```java
	if (num instanceof Integer data) {
	    // data accessible
	} 
	// data NOT accessible here
	```
- The compared type must be **reassignable** — a redundant pattern fails to compile:
	```java
	Integer i = 5;
	if (i instanceof Integer data) { }   // DOES NOT COMPILE (redundant)
	```

# `switch` Statements

- A `switch` compares a single value against a list of `case` constants.
	```java
	switch (dayOfWeek) {
	    case 0:
	        System.out.println("Sunday");
	        break;
	    case 1:
	    case 2:
	        System.out.println("Weekday");
	        break;
	    default:
	        System.out.println("Other");
	}
	```

## Supported Data Types

- `byte`, `short`, `int`, `char` and their wrapper classes (`Byte`, `Short`, `Integer`, `Character`)
- `String`
- `enum` values
- `var` (if it resolves to one of the above)
- **NOT supported:** `boolean`, `long`, `float`, `double` (and their wrappers)

## Rules for `case` Values

- Must be a **compile-time constant** of a type compatible with the `switch` value.
	```java
	final int COLD = 5;
	int hot = 7;            // not final
	switch (temperature) {
	    case 1:        break;     // OK — literal
	    case COLD:     break;     // OK — final constant
	    case hot:      break;     // DOES NOT COMPILE — not constant
	}
	```
- Cannot be `null`.
- Cannot duplicate values across cases.

## Fall-Through Behavior

- Without `break`, execution **falls through** into subsequent cases until a `break`/end is reached.
	```java
	switch (month) {
	    case 1:
	    case 2:
	    case 3:
	        System.out.println("Q1");
	        break;     // stops here
	    case 4:
	        System.out.println("Q2 start");
	        // no break -> falls through
	    default:
	        System.out.println("Other");
	}
	```

# `switch` Expressions (Java 14+)

- A `switch` **expression** returns a value and uses the arrow `->` syntax.
	```java
	String result = switch (dayOfWeek) {
	    case 0       -> "Sunday";
	    case 1, 2, 3, 4, 5 -> "Weekday";
	    case 6       -> "Saturday";
	    default      -> "Unknown";
	};
	```
- Key differences from a `switch` statement:
	- **No fall-through** — only the matching branch runs.
	- Multiple labels per case allowed: `case 1, 2, 3 ->`.
	- Each branch is either a single expression, a block, or a `throw`.
	- Must be **exhaustive** (cover every possible value) — usually requires `default` unless all `enum` values are handled.
	- Ends with a semicolon `;` after `}` since the whole thing is an expression.

## Returning a Value from a Block

- Use `yield` to return a value from a block-form arrow case.
	```java
	int days = switch (month) {
	    case 1, 3, 5, 7, 8, 10, 12 -> 31;
	    case 4, 6, 9, 11 -> 30;
	    case 2 -> {
	        boolean leap = (year % 4 == 0);
	        yield leap ? 29 : 28;
	    }
	    default -> throw new IllegalArgumentException();
	};
	```

# `while` Loops

- Repeats a block **as long as** the boolean condition is `true`.
	```java
	int roomTemp = 30;
	while (roomTemp > 22) {
	    roomTemp--;
	}
	```
- Condition tested **before** each iteration — body may run **zero times**.

## `do-while`

- Executes the body **at least once**, then tests the condition.
	```java
	int x;
	do {
	    x = readInput();
	} while (x < 0);
	```

# `for` Loops

## Basic `for`

- `for (initialization; booleanExpression; updateStatement)`
	```java
	for (int i = 0; i < 5; i++) {
	    System.out.println(i);
	}
	```
- Each section is **optional** — `for(;;){}` is an infinite loop.
- Multiple init/update parts are separated by commas (must share the same type in init).
	```java
	for (int i = 0, j = 10; i < j; i++, j--) { ... }
	```
- Variable redeclarations in init are illegal:
	```java
	int i = 0;
	for (int i = 1; i < 5; i++) { }   // DOES NOT COMPILE — duplicate i
	```

## Enhanced `for` (for-each)

- Iterates arrays or `Iterable` collections without an index.
	```java
	int[] nums = {1, 2, 3};
	for (int n : nums) {
	    System.out.println(n);
	}
	```
- Right side must be an array or implement `java.lang.Iterable`.
- The loop variable type must match (or be assignable from) the element type.

# Branching: `break`, `continue`, `return`

- **`break`** — exits the **innermost** loop or `switch` immediately.
- **`continue`** — skips the rest of the current iteration and re-tests the loop condition.
- **`return`** — exits the entire method (not just the loop).

## Optional Labels

- Any statement can be prefixed by an identifier label followed by `:`.
- `break label;` / `continue label;` target an enclosing labeled loop.
	```java
	OUTER:
	for (int i = 0; i < 3; i++) {
	    for (int j = 0; j < 3; j++) {
	        if (i == 1 && j == 1) break OUTER;
	        if (j == 0) continue OUTER;
	    }
	}
	```
- Labels are conventionally **UPPER_CASE**.

## Unreachable Code

- The compiler rejects code that can **never execute**.
	```java
	while (true) {
	    System.out.println("loop");
	}
	System.out.println("done");   // DOES NOT COMPILE — unreachable
	```
- A `return`, `throw`, `break`, or `continue` followed by another statement in the same block triggers the same error.

# Quick Reference Table

| Construct          | Tests Before? | Min Iterations | Use When                                |
| ------------------ | ------------- | -------------- | --------------------------------------- |
| `if / else`        | n/a           | n/a            | One-shot decision                       |
| `switch` statement | n/a           | n/a            | Many constant cases (may fall through)  |
| `switch` expr.     | n/a           | n/a            | Many cases, returning a value           |
| `while`            | yes           | 0              | Unknown count, may skip                 |
| `do-while`         | no            | 1              | Need at least one execution             |
| `for`              | yes           | 0              | Counter-driven loops                    |
| `for-each`         | yes           | 0              | Iterating arrays/collections            |

## Practice / Interview Questions

- **What's the difference between a `switch` statement and a `switch` expression?**
- **Which data types are valid in a `switch`? Which are not?**
- **What is fall-through, and how do you prevent it?**
- **When does a `case` label need to be a compile-time constant? Show an example that fails to compile.**
- **Explain pattern matching with `instanceof`. What is flow scoping?**
- **What is the difference between `while` and `do-while`?**
- **Can a `for` loop omit all three parts? What happens if it does?**
- **Difference between `break`, `continue`, and a labeled `break`?**
- **What is `yield` used for in a `switch` expression?**
- **Why does the compiler reject unreachable code? Give an example.**