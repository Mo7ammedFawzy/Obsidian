# `Java Class` Structure

- **Classes and Members:** Java programs are built fro[[README]]m _classes_
- A **`Java class`** is a **template** for creating objects, defining their state (fields) and behavior (methods, functions).
	```java
	public class Animal {
	    String name;    // a field (variable) holding state
	    void move(){};  // a method (function) holding behavior
	}
	```
- The `main()` Method (Entry Point)
	- **Signature:** Execution begins with the `main`
		``` java 
		 public class Zoo {
				public static void main(String[] args) {
					// program statements
				}
			}
		 ```
	- `public`: (`accessible everywhere`) access modifier allowing the JVM to call this method from outside the class 
	- `static`: (`belongs to the class, not an object`) means the JVM can invoke `main()` without creating an object instance. The JVM effectively does `Zoo.main()` behind the scenes.
	- `void`: indicates `main()` returns no value
	- `String[] args` array for **command-line arguments**
- **Compilation:** Java source files (`.java`) are compiled into bytecode (`.class`) using the Java compiler (`javac`) for example `$ javac Zoo.java` => `Zoo.class`(`bytecode`)
- **`JVM`** (`Java Virtual Machine`) looks for the `main` method as the entry point
- **`JDK`**(`Java Development Kit`) vs. **`JRE`**(`Java Runtime Environment`):
	- **`JDK`** includes compiler (`javac`) and tools
	- **`JRE`** alone can only run existing `bytecode` 
- **`Execution Details`**:
	- compiler enforces that the source filename and public class name **`match`** 
	- once `compiled` , **`JVM`** loads the `bytecode`  and starts execution at `main()`

# **`JDK(JRE(JVM))`** 

- **`JDK:`**
	- contains **`JRE`**
	- provides development tools(`compiler,debugger`)
	- `shortly`: **`JDK`** for development `writing,compilecode` ,**`JRE`** for just running java programs
- **`JRE:`** ^add6de
	- contains **`JVM`**
	- and contains also `core libraries` needed to run java bytecode
- **`JVM:`**
	- `platform:` part that executes `java bytecode`
	- java is **`platform-independent`**
	- **`JVM`** included in both **`JDK,JRE`** , without it java programs cannot run.
	
**`Key Point:`** “**`JDK`** is the **development** platform, while **`JRE`** is for **execution**”
**`Q:`** where to find the compiler?
	- **`JDK`** **=>**  **`java`** command in (**`JRE/JDK`**) loads the **`JVM`** to run your program

# Basic Data Types

- **Primitive Types:** Java has **eight primitive types:
    
    - `byte` (8-bit signed integer, –128 to 127)
        
    - `short` (16-bit signed integer)
        
    - `int` (32-bit signed integer)
        
    - `long` (64-bit signed integer) – suffix with `L` if literal is outside `int` range
        
    - `float` (32-bit floating-point) – suffix literal with `f` (e.g. `3.14f`)
        
    - `double` (64-bit floating-point) – default for decimal literals (e.g. `3.14` is a double)
        
    - `char` (16-bit Unicode character) – literals in single quotes, e.g. `'A'`
        
    - `boolean` (true/false) – holds one of the two values `true` or `false`.
        
- **Examples:**
	```java
	int age = 30;            // 32-bit integer
	long population = 7_800_000_000L;  // 64-bit long (suffix L)
	float f = 3.14f;         // 32-bit float (suffix f)
	double d = 3.14159;      // 64-bit double (no suffix needed)
	char letter = 'A';       // Unicode character
	boolean valid = true;    // boolean value`
	```
`TODO`    
Numeric literals are `int` by default; use suffix `L` for longs (to avoid overflow) and `F` for floats. Java also supports other bases: prefix `0` for octal, `0x` or `0X` for hexadecimal, and `0b` or `0B` for binary literals.

**Reference Types vs. Primitives:** All non-primitive types (like classes and arrays) are reference types. A reference variable (e.g. `String s = "Hello";`) **`points to an object in memory(heap)`**, whereas a **`primitive variable holds its actual value(stack)`**. For example, `int x = 5;` stores the value `5`, but `String s = "hi";` stores a reference to a `String` object. Reference types can be **`null`** (no object), while primitives cannot.
# Basic Syntax and Comments

- **Statements and Blocks:** Most Java statements end with a semicolon `;`. Curly braces `{}` define the body of classes and methods, and group multiple statements into a block. Java is _case-sensitive_: `MyClass` and `myclass` are different identifiers. Reserved words (like `class`, `public`, `int`, etc.) cannot be used as variable or class names.
    
- **Identifiers:** Variable and class names (identifiers) must start with a letter, `$`, or `_` (by convention letters), and cannot start with a digit. By convention, class names use CamelCase (e.g. `MyClass`), and variable/method names start lowercase (e.g. `myVariable`).
    
- **Comments:** Java has three comment styles:
    
    - **Single-line:** `// comment...` (ignores text until end-of-line)
        
    - **Multi-line:** `/* ... */` (can span lines)
        
    - **Javadoc:** `/** ... */` (like multi-line, but for documentation tools)

- **Packages and Imports:** (Covered in Chapter 1) Java classes can be organized into packages. A source file may begin with a `package` declaration, and `import` statements bring in other classes. For example:

    ```java
    package myapp; 
    import java.util.List;
	```
    
    Wildcard imports (`import java.util.*;`) bring in all classes in a package. 
## Practice / Interview Questions

- **What is the correct signature of the `main()` method in Java?**
    
- **How do you compile and run a Java program from the command line?** (Include example commands.)
    
- **What is the difference between the JDK and JRE?**
    
- **Why must the Java source filename match the public class name?**
    
- **Name the eight primitive data types in Java and give a use or example for each.**
    
- **What do the keywords `public`, `static`, and `void` mean in the context of `main()`?**
    
- **How do you represent integer literals in different bases (decimal, hex, binary, octal) in Java?**
    
- **How do you declare a float or long literal in Java?** (Hint: suffixes.)
    
- **What are the three types of comments in Java, and how are they written?**
    
- **Give examples of valid and invalid Java identifiers (names).**
