
## Java Operators

- **Definition:**  _“Operators are used to perform operations on variables and values.”.
    
- **Categories:** Operators fall into categories such as _arithmetic_ (`+ - * / % ++ --`), _assignment_ (`=, +=, -=, *=, /=, %=, etc.`), _relational_ (`<, <=, >, >=`), _equality_ (`==, !=`), _logical_ (`&&, ||, !`), and _bitwise_ (`& | ^ ~ << >> >>>`). For example:
    
    
    

```java
int sum = 5 + 3;  // arithmetic addition 
boolean ok = (x > 0) && (y == 10);  // logical AND, both conditions must be true a &= 0xFF; // bitwise AND assignment
```
- **Operator Precedence:** _“operators on the same line have equal precedence… All binary operators except for assignment are evaluated left-to-right; assignment operators right-to-left.”.  Always use parentheses to clarify complex expressions.
    
- **Increment/Decrement:** The `++` and `--` operators come in prefix and postfix forms. `++x` (_**`prefix`**) increments before using the value, while `x++` (**`postfix`**) uses the value then increments. Example:
    
```java
int x = 5; int a = x++;  // a = 5, then x becomes 6 int b = ++x;  // x becomes 7, then b = 7`
```
- **Assignment Operators:** forms (`+=, -=, *=, /=, %=, &=`, etc.)
- **Ternary Operator:** Java supports the conditional (ternary) operator `?:` as a compact `if-else` expression:
```java
int max = (a > b) ? a : b;
```
    
- **Operator Associativity:**  For example, in `a - b - c`, Java does `(a - b) - c`; in `a = b = 5`, it does `a = (b = 5)`.
## Conditional Statements

## Loop Constructs

- **`while` Loop:** Repeats a block _while_ a condition is true.  Syntax:
    ```java
    while (condition) {     // statements }
```
    
- **`do-while` Loop:** Similar to `while`, but tests _after_ the loop body. Syntax:
    
    `do {     // statements } while (condition);`
    
    The key difference is the body is always executed _at least once_ (because the condition is checked at the end). Example:
    
    `int i = 1; do {     System.out.println(i);     i++; } while (i <= 5);`
    
Even if `i` started at a value that fails the condition, the loop body runs once. The OCA tutorial notes this “bottom-tested” behavior as the defining distinction.
    
- **`for` Loop:** A compact loop form for counting/indexing[docs.oracle.com](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/for.html#:~:text=The%20,can%20be%20expressed%20as%20follows). General syntax:
    
    java
    
    Copy code
    
    `for (initialization; condition; update) {     // statements }`
    
    - _Initialization_ runs once at the start.
        
    - _Condition_ is checked before each iteration; if false, the loop ends.
        
    - _Update_ runs after each iteration.  
        For example, to print 1–5:
        
    
    java
    
    Copy code
    
    `for (int i = 1; i <= 5; i++) {     System.out.println(i); }`
    
    Here `i=1` initializes, `i<=5` is the loop guard, and `i++` increments each time. The tutorial highlights that if the loop variable is only needed inside the loop, it’s best declared in the `for` header so its scope is limited[docs.oracle.com](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/for.html#:~:text=Notice%20how%20the%20code%20declares,life%20span%20and%20reduces%20errors). Also, any of the three expressions can be omitted: `for (;;)` creates an infinite loop[docs.oracle.com](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/for.html#:~:text=The%20three%20expressions%20of%20the,can%20be%20created%20as%20follows).
    
- **Enhanced `for` (for-each) Loop:** Java also provides an _enhanced_ for loop to iterate over arrays or collections more simply[docs.oracle.com](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/for.html#:~:text=The%20,the%20numbers%201%20through%2010). Syntax: `for(type item : array)`. Example:
    
    java
    
    Copy code
    
    `int[] nums = {1,2,3,4,5}; for (int n : nums) {     System.out.println(n); }`
    
    This prints each element of `nums`. The Oracle tutorial recommends using this form for arrays/collections when possible, since it avoids index bugs[docs.oracle.com](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/for.html#:~:text=class%20EnhancedForDemo%20,). (Note: this enhanced loop is covered later in Chapter 3 but is a common loop form in Java.)
    

## Control Flow: `break`, `continue`, Labels, Nested Loops

- **`break` Statement:** `break` immediately exits the nearest enclosing loop (`for`/`while`/`do`) or switch. In a loop, execution jumps to the statement following the loop[docs.oracle.com](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/branch.html#:~:text=The%20,in%20the%20following%20BreakDemo%20program)[geeksforgeeks.org](https://www.geeksforgeeks.org/break-statement-in-java/#:~:text=The%20Break%20Statement%20in%20Java,first%20statement%20after%20the%20loop). For example:
    
    java
    
    Copy code
    
    `for (int i = 0; i < 10; i++) {     if (i == 5) break;     System.out.println(i); } System.out.println("Done");`
    
    This prints 0 through 4, then “Done”. The loop stops entirely when `i==5`. In a `switch`, each `case` usually ends with a `break` to prevent fall-through[docs.oracle.com](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/switch.html#:~:text=Another%20point%20of%20interest%20is,SwitchDemoFallThrough%20shows%20statements%20in%20a); if omitted, execution would continue into the next case’s code. In summary, _break_ is a control-flow exit: **“as soon as the break statement is encountered ... control returns to the first statement after the loop or switch.”*[geeksforgeeks.org](https://www.geeksforgeeks.org/break-statement-in-java/#:~:text=The%20Break%20Statement%20in%20Java,first%20statement%20after%20the%20loop).
    
    - _Labeled break:_ You can also label a loop and use `break label;` to exit an outer loop from within an inner loop. For example:
        
        java
        
        Copy code
        
        `outer: for (int i=0; i<3; i++) {     for (int j=0; j<3; j++) {         if (someCondition) {             break outer;  // exits the outer loop         }     } }`
        
        This immediately breaks out of the loop labeled `outer`. (Labels are optional and rarely needed, but the exam may test your understanding of them.)
        
- **`continue` Statement:** `continue` skips the rest of the current loop iteration and proceeds to the next iteration[docs.oracle.com](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/branch.html#:~:text=The%20,counting%20the). In a `for` loop it jumps to the update expression; in `while`/`do-while` it rechecks the condition. _Example:_
    
    java
    
    Copy code
    
    `for (int i = 0; i < 5; i++) {     if (i % 2 == 0) continue;     System.out.println(i); }`
    
    This prints only odd numbers (1 and 3), because when `i` is even the `continue` causes the loop to immediately go to the next iteration. As GeeksforGeeks notes, _“continue statement in Java is used to skip the current iteration of a loop”_[geeksforgeeks.org](https://www.geeksforgeeks.org/break-and-continue-statement-in-java/#:~:text=The%20continue%20statement%20in%20Java,statement%20after%20the%20continue%20statement).
    
    - _Labeled continue:_ Like `break`, you can label loops and use `continue label;` to skip to the next iteration of the labeled outer loop. For example, skipping the rest of the inner loop and continuing the next pass of the outer loop.
        
- **Nested Loops:** Java allows loops inside loops. Each level has its own loop variable. By default, `break` or `continue` without a label only affects the innermost loop. For example, a `break` inside an inner loop just exits that loop, then the outer loop continues. To exit multiple loops at once, use labeled breaks as above. Nested loops often appear in array/matrix traversal or pattern generation; be careful with their control flow.
    

## Common Interview Questions

- **Q:** _What does operator precedence mean in Java?_  
    **A:** Operator precedence defines which parts of an expression are evaluated first. For example, in `a + b * c`, the multiplication has higher precedence, so `b*c` is done before adding `a`[docs.oracle.com](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/operators.html#:~:text=operators%20in%20the%20following%20table,are%20evaluated%20right%20to%20left)[docs.oracle.com](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/operators.html#:~:text=Operator%20Precedence%20Operators%20Precedence%20postfix,bitwise%20AND). If operators have equal precedence, associativity (usually left-to-right) applies. Always use parentheses to avoid confusion.
    
- **Q:** _How do `&&` and `&` differ in a boolean expression?_  
    **A:** `&&` is the short-circuit logical AND: if the left operand is false, Java skips evaluating the right operand. `&` is the bitwise AND (also usable as a logical AND on booleans) but _does not_ short-circuit – it always evaluates both sides. This means `if(a != 0 && b/a > 1)` will never divide by zero if `a` is 0 (because the right side is skipped), whereas `&` would still evaluate `b/a` and could throw an error. (This relates to operator precedence as well: `&&` has lower precedence than `==`, etc., as shown in the Oracle precedence table[docs.oracle.com](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/operators.html#:~:text=operators%20in%20the%20following%20table,are%20evaluated%20right%20to%20left)[docs.oracle.com](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/operators.html#:~:text=Operator%20Precedence%20Operators%20Precedence%20postfix,bitwise%20AND).)
    
- **Q:** _When would you use `if-else` vs. `switch`?_  
    **A:** Use `if-else` when conditions involve ranges or complex boolean logic. Use `switch` when checking one variable against many constant values (integers, enums, or strings)[geeksforgeeks.org](https://www.geeksforgeeks.org/switch-vs-else/#:~:text=1,integer%2C%20enumerated%20value%2C%20or%20String). For example, `if(x>0)` vs. `switch(dayOfWeek)`. A `switch` often leads to cleaner code when selecting among many discrete values, but it only works for exact matches.
    
- **Q:** _What is the difference between `while` and `do-while` loops?_  
    **A:** A `while` loop checks its condition _before_ each iteration, so the body may not execute at all if the condition is false initially. A `do-while` loop checks _after_ its body, guaranteeing that the loop body executes at least once[docs.oracle.com](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/while.html#:~:text=The%20difference%20between%20%60do,least%20once%2C%20as%20shown%20in). Use `do-while` when you need the code to run once before any check.
    
- **Q:** _How do `break` and `continue` affect loop execution?_  
    **A:** `break` immediately exits the loop (or switch) entirely[geeksforgeeks.org](https://www.geeksforgeeks.org/break-statement-in-java/#:~:text=The%20Break%20Statement%20in%20Java,first%20statement%20after%20the%20loop). `continue` skips the rest of the current iteration and jumps to the next iteration. In nested loops, an unlabeled `break`/`continue` only affects the innermost loop. Labeled `break` or `continue` can target an outer loop. For example, `break` inside a `switch` is used to stop fall-through[docs.oracle.com](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/switch.html#:~:text=Another%20point%20of%20interest%20is,SwitchDemoFallThrough%20shows%20statements%20in%20a).
    
- **Q:** _What happens if you omit `break` in a `switch` case?_  
    **A:** Without `break`, Java falls through to the next case. That means it will execute the code for the matched case and then continue executing code in all following cases until it finds a `break` or reaches the end of the switch[docs.oracle.com](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/switch.html#:~:text=Another%20point%20of%20interest%20is,SwitchDemoFallThrough%20shows%20statements%20in%20a). This is sometimes used intentionally (for example, to let multiple cases share code), but usually you include `break` to avoid unexpected fall-through.
    
- **Q:** _How do you exit from nested loops?_  
    **A:** By default, a `break` exits only the innermost loop. To break out of an outer loop, you can label the loop and use `break label;`. For example:
    
    java
    
    Copy code
    
    `outer: for (...) {     for (...) {         if (condition) break outer;  // breaks out of the outer loop     } }`
    
    Similarly, `continue label;` skips to the next iteration of the labeled outer loop[docs.oracle.com](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/branch.html#:~:text=test%3A%20for%20,).
    

These points and examples cover the essential concepts of operators, statements, and flow control in Chapter 2. Reviewing them – along with writing and running small code snippets yourself – will help solidify understanding for the Oracle Java Programmer I exam.