# 1-  Differences between `var`, `let`, and `const`

| Feature       | `var`                                  | `let`                                               | `const`                                                     |
| ------------- | -------------------------------------- | --------------------------------------------------- | ----------------------------------------------------------- |
| **Scope**     | Function‑scoped (or global)            | Block‑scoped                                        | Block‑scoped                                                |
| **Hoisting**  | Hoisted and initialized to `undefined` | Hoisted but in Temporal Dead Zone until declaration | Hoisted but in Temporal Dead Zone until declaration         |
| **Redeclare** | Can be redeclared in same scope        | Cannot be redeclared in same scope                  | Cannot be redeclared in same scope                          |
| **Reassign**  | Can be reassigned                      | Can be reassigned                                   | Cannot be reassigned (but object/array contents can mutate) |

## Scope Differences Summary

# Function Scope vs. Block Scope

| Aspect         | Function Scope                                 | Block Scope                                            |
| -------------- | ----------------------------------------------- | ------------------------------------------------------ |
| **Definition** | Variables visible throughout the containing function. | Variables visible only within the nearest `{ … }` block. |
| **Declarations**| `var`                                         | `let`, `const`                                         |

 **Example**    

```js
function fn() {
  if (true) {
    var x = 1;
  }
  console.log(x); // 1 (accessible anywhere in fn)
}
```  ```js
function fn() {
  if (true) {
    let y = 2;
    const z = 3;
  }
  console.log(y); // ReferenceError
  console.log(z); // ReferenceError
}
```

**Key Takeaways:**
- Use **function scope** (`var`) for legacy support, but be cautious of unintended sharing across blocks.
- Prefer **block scope** (`let`/`const`) to limit variable visibility and avoid bugs.
- `const` ensures bindings cannot be reassigned, enhancing code safety.