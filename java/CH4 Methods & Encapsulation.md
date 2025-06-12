
## 1. Designing Methods

- **Modifiers:** `public`, `protected`, `private`, `static`, `abstract`, `final`, `synchronized` (OCP).
### Code Example

```java
public static double calculateArea(double radius) {
    return Math.PI * radius * radius;
}
```
### Interview Questions

- Q: What parts make up a method signature in Java?
    
- A: Access modifiers, return type, method name, parameter list (and thrown exceptions for overloading).
    
---
## 2. Varargs

- **Syntax:** `method(Type... name)` allows 0+ arguments as an array.
    
- **Rules:** Must be the last parameter. You cannot have two varargs.

### Code Example

```java
public void logMessages(String prefix, String... messages) {
    for (String msg : messages) {
        System.out.println(prefix + msg);
    }
}
```
### Interview Questions

- Q: How does Java treat varargs under the hood?
- A: As an array of that type (`String...` becomes `String[]`).
    

---

## 3. Access Modifiers

| Modifier    | Same Class | Package | Subclass | World |
| ----------- | ---------- | ------- | -------- | ----- |
| `public`    | ✓          | ✓       | ✓        | ✓     |
| `protected` | ✓          | ✓       | ✓        | ✗     |
| _(default)_ | ✓          | ✓       | ✗        | ✗     |
| `private`   | ✓          | ✗       | ✗        | ✗     |

### Code Example

```java
class Person {
  private String name;
  protected int age;
  public void setName(String n) { name = n; }
  String getDetails() { return name + " " + age; } // default
}
```

### Interview Questions

- Q: When would you choose `protected` over `private`?
    
- A: When you want subclasses in any package (and same-package classes) to access a member.
    

---

## 4. Static Methods & Fields

- Belong to the **class**, not instances.
    
- Invoked as `ClassName.method()` or `method()` within same class.
    
- **Static initializer:** `static { ... }` runs once at class load.

### Code Example

```java
public class MathUtil {
    public static final double PI = 3.14159;
    static {
        // static setup
    }
    public static int add(int a, int b) {
        return a + b;
    }
}
```

### Interview Questions

- Q: Can static methods access instance variables?
    
- A: No. They can only access static members directly.
    

---

## 5. Overloading & Passing Data

- **Overloading:** Same name, different parameter lists.
    
- **Passing primitives vs. objects:** Primitives are passed by value; object references passed by value (the object itself is not copied).
    

### Code Example

```java
public void process(int x) { x++; }
public void process(StringBuilder sb) { sb.append("!"); }

int a = 5; process(a); // a remains 5
StringBuilder s = new StringBuilder("Hi"); process(s); // s becomes "Hi!"
```

### Interview Questions

- Q: Why can you modify an object’s state in a method but not a primitive?
    
- A: Object reference is passed by value; you can call methods on the referenced object, altering its state.
    

---

## 6. Constructors & Initialization

- **Default constructor:** Provided if no constructors are defined.
    
- **Overloading:** Multiple constructors with different parameter lists.
    
- **`this()` call:** Invoke another constructor in the same class; must be first statement.
    
- **`super()` call:** Invoke parent constructor; if omitted, Java inserts no‑arg `super()`.
    
### Code Example

```java
public class Rectangle {
    private int w, h;
    public Rectangle() { this(1,1); }
    public Rectangle(int w, int h) {
        this.w = w; this.h = h;
    }
}
```

### Interview Questions

- Q: What happens if you define a constructor with arguments but no default?
    
- A: No-arg constructor is not provided automatically; you must define it if needed.
    

---

## 7. Encapsulation & Immutable Classes

- **Encapsulation:** Keep fields private; expose via getters/setters.
    
- **Immutable class:** All fields `final`, no setters, defensive copies on mutable fields.
    

### Code Example

```java
public final class Point {
    private final int x, y;
    public Point(int x, int y) { this.x = x; this.y = y; }
    public int getX() { return x; }
    public int getY() { return y; }
}
```

### Interview Questions

- Q: How do you design an immutable class in Java?
    
- A: Mark class `final`, all fields `private final`, no setters, use defensive copies for mutable fields.
    

---

## 8. Lambdas (Java 8)

- **Syntax:** `(parameters) -> expression` or `(params) -> { statements }`
    
- **Functional interface:** Interface with a single abstract method (e.g., `Predicate<T>`, `Function<T,R>`).
    

### Code Example

```java
Predicate<String> isLong = s -> s.length() > 10;
List<String> names = Arrays.asList("Alice","Bob","Christopher");
names.stream().filter(isLong).forEach(System.out::println);
```

### Interview Questions

- Q: What makes an interface a functional interface?
    
- A: Exactly one abstract method; default/static methods are allowed.
    

---

