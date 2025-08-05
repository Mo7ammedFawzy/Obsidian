
### Q1: What is a Java class and how do you define one?

**A:**  
A Java class is a **`template`** for creating objects, defining their **`state`** (fields) and **`behavior`** (methods). For example:

```java
public class Animal {
    String name;    // a field (state)
    void move() {}  // a method (behavior)
}
```

---

### Q2: What is the correct signature of the `main()` method in Java?

**A:**

```java
public static void main(String[] args)
```

- `public`: method visible to the JVM.
    
- `static`: belongs to the class, so the JVM can call it without instantiating an object.
    
- `void`: the method returns no value.
    
- `String[] args`: array for command-line arguments.
    

---

### Q3: How do you compile and run a Java program from the command line?

**A:**

```bash
javac MyClass.java   # compiles to MyClass.class
java MyClass         # runs the bytecode
```

---

### Q4: What is the difference between the JDK and JRE?

**A:**

- **JDK**: includes compiler (`javac`), debugger, and developer tools.
    
- **JRE**: contains JVM and core libraries; for running bytecode only.
    

---

### Q5: Why must the Java source filename match the public class name?

**A:**  
The compiler enforces that a public class name and its `.java` filename match so the **`JVM`** can `locate` and `load` the correct class at runtime.

---

### Q6: Name the eight primitive data types in Java and give an example for each.

**A:**

|Type|Size|Example|
|---|---|---|
|`byte`|8-bit|`byte b = 127;`|
|`short`|16-bit|`short s = 32768;`|
|`int`|32-bit|`int i = 30;`|
|`long`|64-bit|`long L = 7800000000L;`|
|`float`|32-bit|`float f = 3.14f;`|
|`double`|64-bit|`double d = 3.14159;`|
|`char`|16-bit|`char c = 'A';`|
|`boolean`|1-bit|`boolean flag = true;`|

---

### Q7: What is the difference between primitive types and reference types?

**A:**

- **Primitive types** store actual values in the stack.
    
- **Reference types** store references to objects in the heap and can be `null`.
    

---

### Q8: How do you represent integer literals in different bases in Java?

**A:**

- Decimal: `123`
    
- Hex: `0x7B` or `0X7B`
    
- Binary: `0b1111011` or `0B1111011`
    
- Octal: `0173`
    

---

### Q9: How do you declare a float or long literal in Java?

**A:**

- **Float**: suffix with `f` or `F`, e.g., `3.14f`
    
- **Long**: suffix with `l` or `L`, e.g., `12345678901L`
    

---

### Q10: What are the three types of comments in Java, and how are they written?

**A:**

- **Single-line:** `// comment`
    
- **Multi-line:** `/* comment */`
    
- **Javadoc:** `/** documentation */`
    

---

### Q11: Give examples of valid and invalid Java identifiers.

**A:**

- **Valid:** `_value`, `$temp`, `myVariable`, `CamelCase`
    
- **Invalid:** `1stPlace` (starts with a digit), `class` (reserved word), `my-variable` (hyphen not allowed)
    

---

### Q12: What are basic syntax rules in Java?

**A:**

- Statements end with `;`
    
- Blocks use `{}`
    
- Java is case-sensitive.
    
- Reserved words like `class`, `public`, etc., cannot be used as identifiers.
    

---

### Q13: What are packages and imports in Java?

**A:**  
Java classes can be grouped using `package` and accessed with `import` statements.

```java
package myapp;
import java.util.List;
```

You can also use wildcard imports:

```java
import java.util.*;
```

---

**Key Point:** "JDK is the development platform, while JRE is for execution."

**Q:** Where do you find the Java compiler?  
**A:** Inside the JDK, which provides the `javac` command for compiling Java source code.