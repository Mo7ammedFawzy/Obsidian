## Operators

- **Categories**
    
    - Arithmetic: `+`, `-`, `*`, `/`, `%`, `++`, `--`
        
    - Assignment: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, etc.
        
    - Relational: `<`, `<=`, `>`, `>=`
        
    - Equality: `==`, `!=`
        
    - Logical: `&&`, `||`, `!`
        
    - Bitwise: `&`, `|`, `^`, `~`, `<<`, `>>`, `>>>`
        
- **Precedence & Associativity**
    
    - Multiplicative (`* / %`) > Additive (`+ -`) > Relational > Equality > Logical
        
    - Operators of equal precedence are _left-associative_ (except assignment and ternary, which are _right-associative_).
        
    - Use parentheses `()` to override.
        
- **Increment/Decrement**
    
    - Prefix (`++x`): increment before use.
        
    - Postfix (`x++`): use value, then increment.
        
- **Compound Assignment**
    
    - `x += 5;` is shorthand for `x = x + 5;`.
        
    - In expressions like `x = y += 2;`, `y += 2` happens first.
        
- **Ternary Operator**
    
    ```java
    int max = (a > b) ? a : b;
    ```
    
## Conditional Statements

- **`if`**
    
    ```java
    if (condition) {
        // executes if true
    }
    ```
    
- **`if-else` / `if-else-if`**
    
    ```java
    if (score >= 90) {
        grade = 'A';
    } else if (score >= 80) {
        grade = 'B';
    } else {
        grade = 'C';
    }
    ```
    
- **`switch`**
    
    - Works with `byte`, `short`, `char`, `int`, `enum`, `String`.
        
    - Always include `break` to avoid fall-through:
        
        ```java
        switch (month) {
            case 1: name = "Jan"; break;
            case 2: name = "Feb"; break;
            default: name = "Invalid";
        }
        ```
        
    - **Fall-through**: Omitting `break` lets execution continue into the next case.
## Loop Constructs

- **`while`**
    
    ```java
    while (condition) {
        // executes zero or more times
    }
    ```
    
- **`do-while`**
    
    ```java
    do {
        // executes at least once
    } while (condition);
    ```
    
- **`for`**
    
    ```java
    for (int i = 1; i <= 5; i++) {
        System.out.println(i);
    }
    ```

    - Initialization; condition; update
        
    - Can omit any section (`for (;;)` is infinite)
        
- **Enhanced `for` (for-each)**
    
    ```java
    int[] nums = {1,2,3};
    for (int n : nums) {
        System.out.println(n);
    }
    ```
    

## Flow Control: `break` & `continue`

- **`break`**: exits the nearest loop or `switch` immediately.
    
- **`continue`**: skips to the next iteration of the loop.
    
- **Labeled**
    
    ```java
    outer:
    for (...) {
      for (...) {
        if (condition) break outer;   // exits outer loop
        if (other) continue outer;     // jumps to next iteration of outer loop
      }
    }
    ```
    

## Common Interview Questions

1. **Operator Precedence**: Which part of `a + b * c - d / e` runs first?
    
    - Answer: `b * c` and `d / e` (multiplicative before additive).
        
2. **Short-circuit vs. Bitwise**: Difference between `&&` and `&` in boolean expressions?
    
    - Answer: `&&` short-circuits (skips right operand if left is false); `&` always evaluates both sides.
        
3. **`if-else` vs `switch`**: When to use each?
    
    - Answer: `if-else` for ranges or boolean logic; `switch` for one variable against many constants.
        
4. **Loop Differences**: `while` vs `do-while`?
    
    - Answer: `while` may skip body; `do-while` always runs body once.
        
5. **`break` & `continue`**: How do they work in nested loops?
    
    - Answer: Unlabeled affects innermost loop; labeled can target an outer loop.
        
6. **`switch` Fall-through**: What happens if `break` is omitted?
    
    - Answer: Execution falls through to subsequent cases until a `break` or end of block.