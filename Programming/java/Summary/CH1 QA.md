# 🧩 Java Basics — Study Q&A Summary  
  
## 📘 1. Java Class & Structure  
  
**Q: What is a class in Java?**  A class is a **template or blueprint** for creating objects. It defines their **state (fields)** and **behavior (methods)**.    
```java  
public class Animal {
    String name;     // field    void move() {}   // method}  
```  
  
**Q: What is the `main()` method in Java?**  It’s the **entry point** where program execution begins.    
```java  
public static void main(String[] args) {
    // program starts here}  
```  
  
**Q: Explain each part of `public static void main(String[] args)`.**  
- `public` → accessible everywhere    
- `static` → runs without creating an object    
- `void` → no return value    
- `String[] args` → holds command-line arguments    
  
---  
  
## ⚙️ 2. Compilation & Execution  
  
**Q: How do you compile and run a Java program from terminal?**  
```bash  
javac Zoo.java   # compile source -> creates Zoo.class (bytecode)java Zoo         # run program -> JVM executes main()
```  
  
**Q: Why must the file name match the public class name?**  Because the compiler enforces it to locate the main class correctly.    
Example → `Zoo.java` must contain `public class Zoo`.  
  
---  
  
## ☕ 3. JDK, JRE, JVM Explained  
  
| Component | Purpose | Contains |  
|------------|----------|-----------|  
| **JDK** | For **development** (write + compile) | JRE + compiler + tools |  
| **JRE** | For **running** Java programs | JVM + core libraries |  
| **JVM** | Executes **bytecode** | — |  
  
**Q: What’s the relationship?**  `JDK` → contains `JRE`, and `JRE` → contains `JVM`.    
🧠 **JDK(JRE(JVM))**  
  
**Q: Key difference between JDK and JRE?**  - **JDK:** Used to develop Java programs.    
- **JRE:** Used only to run them.  
  
---  
  
## 🔢 4. Primitive Data Types  
  
Java has **8 primitive types**:  
  
| Type    | Bits | Example              | Description                |     |
| ------- | ---- | -------------------- | -------------------------- | --- |
| byte    | 8    | `byte b = 10;`       | small integer              |     |
| short   | 16   | `short s = 1000;`    | medium integer             |     |
| int     | 32   | `int x = 50000;`     | default integer type       |     |
| long    | 64   | `long n = 123L;`     | large integer (suffix `L`) |     |
| float   | 32   | `float f = 3.14f;`   | decimal (suffix `f`)       |     |
| double  | 64   | `double d = 3.14;`   | precise decimal            |     |
| char    | 16   | `char c = 'A';`      | single Unicode character   |     |
| boolean | 1    | `boolean ok = true;` | true or false              |     |
  
**Q: What’s the default numeric type?**  `int` for integers, `double` for decimals.  
  
**Q: Can primitives be null?**  No — only reference types can be `null`.  
  
---  
  
## 🧮 5. Literals & Number Bases  
  
**Q: How to represent numbers in different bases?**  
  
| Base | Prefix | Example |  
|------|---------|----------|  
| Decimal | none | `42` |  
| Binary | `0b` | `0b1010` |  
| Octal | `0` | `052` |  
| Hexadecimal | `0x` | `0x2A` |  
  
**Q: How to declare float and long literals?**  
```java  
float price = 9.99f;  // suffix f  
long distance = 5000L; // suffix L  
```  
  
---  
  
## 🧱 6. Reference Types vs Primitives  
  
**Q: What’s the difference?**  - **Primitive:** holds actual value (stack).    
- **Reference:** holds a reference (address) to an object in heap.    
```java  
int x = 5;          // primitive value  
String s = "Hello"; // reference to object  
```  
  
---  
  
## 🧾 7. Syntax & Identifiers  
  
**Q: What are valid identifier rules?**  - Start with letter, `_`, or `$`  - Cannot start with a digit    
- Case-sensitive    
✅ `age`, `_name`, `$salary`  ❌ `2value`, `class`, `my-variable`  
  
---  
  
## 📦 8. Packages & Imports  
  
**Q: What are packages in Java?**  They group related classes and interfaces.  
  
```java  
package myapp;
import java.util.List;  
```  
  
**Q: What does `import java.util.*;` do?**  It imports all classes from `java.util` package.  
  
---  
  
## 🧠 10. Quick Recap Summary  
  
- Java = **platform-independent** because of JVM.    
- **File name = public class name.**  - **JDK** for coding, **JRE** for running, **JVM** for executing bytecode.    
- **8 primitive types** only — everything else is a **reference type**.    
- **main()** → entry point with exact signature.    
- **Suffix:** `L` for long, `f` for float.    
- **Comments:** //, /* */, /** */    
  
---  
  
✅ **Use this file as a revision sheet before interviews or tests!**