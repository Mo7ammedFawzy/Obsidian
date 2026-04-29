# Class Design (Chapter 5: OCA Java SE 8)

This document provides a detailed explanation of Chapter 5 (Class Design) from the _OCA: Oracle Certified Associate Java SE 8 Programmer I Study Guide_, tailored for an intermediate Java learner. It covers inheritance, constructors, method overriding/hiding, abstract classes, interfaces, and polymorphism. Examples and exam tips are included.

---

## Table of Contents

1. Inheritance and Class Extension
    
2. Constructors and Inheritance
    
3. Method Overriding vs. Hiding
    
4. Abstract Classes
    
5. Interfaces
    
6. Polymorphism and Casting
    
7. Key Takeaways
    

---

## Inheritance and Class Extension

Inheritance allows a **subclass** to derive (`extends`) from a **superclass**, reusing its `public` and `protected` members. Java supports **single** class inheritance (one direct parent) but allows multiple levels.

- `**final**` **classes/methods** cannot be extended/overridden.
    
- All classes implicitly extend `java.lang.Object`.
    

```java
class Animal {
    public void speak() { System.out.println("Animal speaks"); }
}

class Dog extends Animal {
    @Override
    public void speak() { System.out.println("Woof!"); }
}

final class Cat extends Animal {
    @Override
    public void speak() { System.out.println("Meow!"); }
}

Animal a = new Dog();
a.speak(); // Woof!
```

> **Tip:** Use `@Override` to catch signature mismatches at compile time.

---

## Constructors and Inheritance

A subclass constructor must begin with either:

- `this(...)` — call another constructor in the same class
    
- `super(...)` — call a constructor in the direct parent
    

If omitted, `super()` (no-arg) is inserted automatically.

```java
class Parent {
    Parent(String msg) { System.out.println("Parent: " + msg); }
}

class Child extends Parent {
    Child() {
        super("hello");  // required if Parent has no no-arg constructor
        System.out.println("Child constructor");
    }
}

new Child();
// Output:
// Parent: hello
// Child constructor
```

> **Pitfall:** If the parent only has parameterized constructors and you forget `super(args)`, code won’t compile.

---

## Method Overriding vs. Hiding

### Overriding (Instance Methods)

- Must match signature and return type (or covariant).
    
- Visibility cannot be reduced.
    
- Cannot throw broader checked exceptions.
    

```java
class Animal {
    public void move() { System.out.println("Animal moves"); }
}
class Bird extends Animal {
    @Override
    public void move() { System.out.println("Bird flies"); }
}

Animal a = new Bird();
a.move(); // Bird flies
```

### Hiding (Static Methods)

Static methods with the same signature are hidden, not overridden.

```java
class Parent {
    public static void greet() { System.out.println("Hello from Parent"); }
}
class Child extends Parent {
    public static void greet() { System.out.println("Hello from Child"); }
    public static void main(String[] args) {
        Parent.greet(); // Parent
        Child.greet();  // Child
    }
}
```

> **Note:** Avoid hiding static methods; it leads to confusing code.

**Variable hiding:** Declaring a field with the same name in a subclass creates two copies. Use `super.field` to access the parent’s field.

---

## Abstract Classes

An `abstract` class cannot be instantiated and may contain both abstract and concrete methods.

```java
abstract class Shape {
    String color;
    abstract double area();    // no body
    public void setColor(String c) { color = c; }
}

class Circle extends Shape {
    double radius;
    Circle(double r) { radius = r; }

    @Override
    double area() { return Math.PI * radius * radius; }
}
```

- Concrete subclasses **must** implement all inherited abstract methods.
    
- Abstract classes can have constructors, fields, and any access level on members.
    

---

## Interfaces

An `interface` defines a contract: abstract methods, constants, default and static methods.

```java
interface Vehicle {
    int MAX_SPEED = 120;             // public static final
    void startEngine();              // public abstract

    default void honk() {
        System.out.println("Beep beep");
    }
    static void info() {
        System.out.println("Vehicles have engines.");
    }
}

class Car implements Vehicle {
    @Override
    public void startEngine() {
        System.out.println("Car engine started");
    }
}
```

- A class can implement **multiple** interfaces.
    
- Default methods help evolve interfaces without breaking clients.
    
- If two interfaces define the same default method signature, the implementing class **must** override it:
    

```java
interface Flyer { default String id() { return "I fly"; } }
interface Walker { default String id() { return "I walk"; } }

class Pegasus implements Flyer, Walker {
    @Override
    public String id() {
        return Flyer.super.id() + " & " + Walker.super.id();
    }
}
```

> **Exam Tip:** Static interface methods are called on the interface, e.g., `Vehicle.info()`.

---

## Polymorphism and Casting

Polymorphism allows a parent reference to point to a child object. Method calls are resolved at **runtime** (virtual methods).

```java
Animal a = new Dog(); // upcast
a.speak();            // Dog.speak() at runtime
```

- **Upcasting** (subclass → superclass) is implicit.
    
- **Downcasting** (superclass → subclass) requires an explicit cast and may throw `ClassCastException`:
    

```java
if (a instanceof Dog) {
    Dog d = (Dog) a;
    d.fetch();
}
```

Use `instanceof` to guard against invalid casts.

---

## Key Takeaways

- **Single inheritance** for classes; use interfaces for multiple-type inheritance.
    
- Chain constructors with `this()` or `super()`; parent constructors run first.
    
- Override instance methods (`@Override`), hide static methods (avoid it).
    
- Abstract classes combine abstract and concrete methods; concrete subclasses must implement abstract ones.
    
- Interfaces define a contract; Java 8+ adds default and static methods.
    
- Polymorphism and dynamic binding let you write flexible, reusable code.
    

---

## Interview Q&A

**Q1: What is the difference between inheritance and composition?**  
**A1:** Inheritance is an IS-A relationship (subclass extends superclass). Composition is a HAS-A relationship (class holds a reference to another).

**Q2: What happens if a subclass constructor does not explicitly call a superclass constructor?**  
**A2:** Java inserts `super()` by default. If the superclass has no no-arg constructor, this causes a compile error; you must explicitly call a valid `super(...)`.

**Q3: Can you override a static method?**  
**A3:** No. Declaring a static method with the same signature in a subclass hides the superclass’s method. Invocation depends on the reference type.

**Q4: What restrictions apply when overriding a method?**  
**A4:** The overriding method must have the same signature, a compatible (covariant) return type, cannot reduce visibility, and cannot throw broader checked exceptions than the original.

**Q5: How do `super` and `this` differ in constructors?**  
**A5:** `super(...)` calls a superclass constructor; `this(...)` calls another constructor in the same class. Both must be the first statement.

**Q6: Can a private method in the parent be overridden?**  
**A6:** No. Private methods are not visible in subclasses; a method with the same signature in the subclass is a new method, not an override.

---

_Master these class-design concepts with code examples to excel at the OCA exam and technical interviews!_