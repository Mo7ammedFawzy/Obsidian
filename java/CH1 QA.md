# Java Basics Q&A Flashcards

### Q1: What is the correct signature of the `main()` method in Java?
**A:**
```java
public static void main(String[] args)
```

### Q2: How do you compile and run a Java program from the command line?
**A:**
```bash
javac MyClass.java   # compiles to MyClass.class
java MyClass         # runs the bytecode
```

### Q3: What is the difference between the JDK and JRE?
**A:**
- **JDK**: includes compiler (`javac`), debugger, and developer tools.
- **JRE**: contains JVM and core libraries; for running bytecode only.

### Q4: Why must the Java source filename match the public class name?
**A:**
The compiler enforces that a public class name and its `.java` filename match so the **`JVM`** can `locate` and `load` the correct class at runtime.

### Q5: Name the eight primitive data types in Java and give an example for each.
**A:**
- `byte`: `byte b = 127;`
- `short`: `short s = 32768;`
- `int`: `int i = 30;`
- `long`: `long L = 7800000000L;`
- `float`: `float f = 3.14f;`
- `double`: `double d = 3.14159;`
- `char`: `char c = 'A';`
- `boolean`: `boolean flag = true;`

### Q6: What do the keywords `public`, `static`, and `void` mean in the context of `main()`?
**A:**
- `public`: method visible to the JVM.
- `static`: belongs to the class, so the JVM can call it without instantiating an object.
- `void`: the method returns no value.

### Q7: How do you represent integer literals in different bases in Java?
**A:**
- Decimal: `123`
- Hex: `0x7B` or `0X7B`
- Binary: `0b1111011` or `0B1111011`
- Octal: `0173`

### Q8: How do you declare a float or long literal in Java?
**A:**
- **Float**: suffix with `f` or `F`, e.g., `3.14f`
- **Long**: suffix with `l` or `L`, e.g., `12345678901L`

### Q9: What are the three types of comments in Java, and how are they written?
**A:**
- **Single-line:** `// comment`
- **Multi-line:** `/* comment */`
- **Javadoc:** `/** documentation */`

### Q10: Give examples of valid and invalid Java identifiers.
**A:**
- **Valid:** `_value`, `$temp`, `myVariable`, `CamelCase`
- **Invalid:** `1stPlace` (starts with a digit), `class` (reserved word), `my-variable` (hyphen not allowed)
