## 1. If-Else Statements

### Code Example

```java
int score = 76;
char grade;
if (score >= 90) {
    grade = 'A';
} else if (score >= 80) {
    grade = 'B';
} else if (score >= 70) {
    grade = 'C';
} else if (score >= 60) {
    grade = 'D';
} else {
    grade = 'F';
}
System.out.println("Grade = " + grade);  // Output: Grade = C
```

**Interview Tip:** Only **`boolean`** conditions are allowed. Using an int or object in an `if` causes a compile error.

---
## 2. Switch Statements

- Tests a single expression against multiple `case` labels.
    
- Allowed types (Java SE 8): `byte`, `short`, `char`, `int` (and wrappers), `enum`, **`String`**.
    
- **Fall-through**: Without `break`, execution continues into subsequent cases until a `break` appears.
    
- Always use `break` unless intentional fall-through.
    
- `default` runs if no case matches; placement can be anywhere but is usually last.
### Code Example

```java
int month = 8;
String name;
switch (month) {
    case 1:  name = "January";  break;
    case 2:  name = "February"; break;
    case 3:  name = "March";    break;
    // ...
    default: name = "Invalid";  break;
}
```

### Grouped Cases

```java
switch (month) {
    case 1: case 3: case 5: case 7: case 8: case 10: case 12:
        System.out.println("31 days"); break;
    case 4: case 6: case 9: case 11:
        System.out.println("30 days"); break;
    case 2:
        System.out.println("28 or 29 days"); break;
    default:
        System.out.println("Invalid month");
}
```

**Interview Tip:** Forgetting `break` causes fall-through into subsequent cases unintentionally.

---

## 3. Loops

### For Loop

- Syntax: `for(init; condition; update) { body }`
    
- Variables declared in `init` are local to the loop.
    
- Examples: `for(int i=1; i<=10; i++) { ... }`
    
- You can declare multiple variables of the same type: `for(int i=0, j=10; i<j; i++, j--) { ... }`.
    
- `for(;;)` is an infinite loop.
    

### While Loop

- Syntax: `while(condition) { body }`
    
- Tests condition **before** each iteration; may execute zero times if false initially.

### Do-While Loop

- Syntax: `do { body } while(condition);`
    
- Tests condition **after** the loop body; body runs at least once.

### Enhanced For (For-Each)

- Syntax: `for(Type item : arrayOrCollection) { ... }`
    
- Iterates through each element without an index.
    
- Use when you don’t need the index and just want to process every element.

**Interview Tip:** Variables declared in the loop header have scope only inside that loop.

---

### Break and Continue

- `break;` exits the **innermost** loop or `switch`. Labeled `break` (e.g., `break outer;`) exits the named outer loop.
    
- `continue;` skips to the next iteration of the **innermost** loop. Labeled `continue` (`continue outer;`) jumps to the next iteration of the labeled outer loop.
    

**Interview Tip:** An unlabeled `break`/`continue` applies only to the immediate loop.

---

## 4. Exception Handling Basics

- **Exception:** An event at runtime that disrupts normal flow (e.g., `IOException`, `NullPointerException`).
    
- **Checked Exceptions:** Must be caught or declared (`throws`), otherwise compile error.
    
- **Unchecked Exceptions:** Subclasses of `RuntimeException` or `Error`; not required to catch or declare.
    

### Try-Catch-Finally

```java
try {
    BufferedReader br = new BufferedReader(new FileReader("file.txt"));
    String line = br.readLine();
} catch (FileNotFoundException ex) {
    System.err.println("File not found: " + ex.getMessage());
} catch (IOException ex) {
    System.err.println("I/O error: " + ex.getMessage());
} finally {
    System.out.println("This always runs");
}
```

- If an exception is thrown in `try`, execution jumps to the first matching `catch`. If none match, it propagates up the call stack.
    
- If no exception occurs, `catch` blocks are skipped.
    
- `finally` always executes (unless JVM exits or thread is killed) after `try`/`catch`.
    

### Throw vs. Throws

- `throw new Exception();` actually throws an exception object.
    
- `method() throws IOException` declares that the method may throw that checked exception.
    

### Example: Return in Catch with Finally

```java
public static int assignment() {
    int number = 1;
    try {
        number = 3;
        if (true) throw new Exception("Test");
        number = 2;
    } catch (Exception ex) {
        return number;
    } finally {
        number = 4;
    }
    return number;
}
System.out.println(assignment());  // prints 3
```

- The `catch` returns `3`, then `finally` runs (number=4), but the return value remains `3`.
    

**Interview Tip:** Checked exceptions must be caught or declared; unchecked exceptions don’t have this requirement.

---

## Interview Q&A

**Q1:** What types of expressions are allowed in an `if` condition?  
**A:** Only boolean expressions. Using non-boolean (e.g., int) causes a compile-time error.

**Q2:** What data types can you switch on in Java SE 8?  
**A:** `byte`, `short`, `char`, `int` (and their wrappers), `enum`, and `String`. Not `long`, `float`, `double`, or `boolean`.

**Q3:** What happens if you forget a `break` in a `switch` case?  
**A:** Execution falls through into subsequent cases until a `break` or end of switch.

**Q4:** How is a `while` loop different from a `do-while`?  
**A:** A `while` checks the condition before the first iteration (may execute zero times). A `do-while` checks after, so the body always executes at least once.

**Q5:** When is an enhanced for loop useful?  
**A:** When iterating over arrays or `Iterable`s and you don’t need the index (more concise than classic for-loop).

**Q6:** How do `break` and `continue` work in nested loops?  
**A:** Unlabeled `break`/`continue` apply to the innermost loop. Labeled versions (`break label;` or `continue label;`) apply to the named outer loop.

**Q7:** What is the difference between `throw` and `throws`?  
**A:** `throw` actually throws an exception object. `throws` declares that a method may throw certain checked exceptions, forcing callers to handle or declare them.

**Q8:** What’s the difference between checked and unchecked exceptions?  
**A:** Checked exceptions (non-`RuntimeException`) must be caught or declared. Unchecked exceptions (`RuntimeException` or `Error`) do not require handling.

**Q9:** What does this code print?

```java
public static int assignment() {
    int number = 1;
    try {
        number = 3;
        if (true) throw new Exception("Test");
        number = 2;
    } catch (Exception ex) {
        return number;
    } finally {
        number = 4;
    }
    return number;
}
System.out.println(assignment());
```

**A:** It prints `3`. The catch returns `number=3`; `finally` sets `number=4` after the return is determined, but it doesn’t change the returned result.

---

_Master these flow-control rules and pitfalls to excel on the OCA exam and in technical interviews!_