## 🧬 Inheritance

### Q1: What is inheritance and which keyword expresses it?

**A:** Inheritance is an **IS-A** relationship that lets a subclass reuse the `public` and `protected` members of a superclass. Use `extends`:

```java
class Dog extends Animal { ... }
```

---

### Q2: Does Java support multiple class inheritance?

**A:** No — a class can `extends` **only one** direct parent. Multiple **interfaces** can be implemented to achieve multi-type inheritance.

---

### Q3: What does it mean that all classes extend `Object`?

**A:** Every class implicitly inherits from `java.lang.Object`, gaining methods such as `toString()`, `equals()`, `hashCode()`, and `getClass()`.

---

### Q4: What does the `final` keyword mean on a class or method?

**A:**

- `final class` → cannot be extended.
- `final method` → cannot be overridden in a subclass.

---

## 🏗️ Constructors and Inheritance

### Q5: What's the first statement of every constructor?

**A:** Either `this(...)` (calls another constructor in the same class) or `super(...)` (calls a parent constructor). If neither is written, Java inserts an implicit `super();`.

---

### Q6: What happens if the parent has no no-arg constructor and the child omits `super(...)`?

**A:** Compile error. The child **must** explicitly call a valid `super(args)` because the implicit `super()` won't exist.

---

### Q7: In what order do constructors execute?

**A:** **Parent first, then child.** Constructors chain up to `Object` before bodies execute downward.

---

### Q8: What's the difference between `this()` and `super()` in a constructor?

**A:**

- `this(...)` → calls another constructor of the same class.
- `super(...)` → calls the parent's constructor.
- Both must be the **first statement** — they cannot be combined.

---

## 🔄 Overriding vs. Hiding

### Q9: What are the rules for overriding an instance method?

**A:**

- Same **name and parameter list**.
- Return type must be the same or **covariant** (a subtype).
- Visibility **cannot be reduced** (`public` ↛ `protected`).
- Cannot throw **broader** checked exceptions.
- Cannot be `static`, `final`, or `private` in the parent.

---

### Q10: Can you override a `static` method?

**A:** No — declaring a `static` method with the same signature in a subclass **hides** the parent's method. Resolution depends on the **reference type**, not the runtime object.

```java
Parent.greet();   // Parent's
Child.greet();    // Child's
```

---

### Q11: Can a `private` method be overridden?

**A:** No. `private` methods aren't visible in subclasses. A method with the same signature in the child is a **brand-new** method, not an override.

---

### Q12: What is variable hiding?

**A:** Declaring a field in a subclass with the same name as one in the parent creates **two separate fields**. Use `super.field` to reach the parent's field. Field references are resolved by **reference type**, not runtime type.

---

### Q13: What does `@Override` do?

**A:** Marks a method as intended to override a parent method. The compiler checks the signature and reports an error on a mismatch — preventing silent typos.

---

## 🧪 Abstract Classes

### Q14: Can an abstract class be instantiated?

**A:** No. You can only instantiate a **concrete subclass** that implements all inherited abstract methods.

---

### Q15: Can an abstract class have constructors and concrete methods?

**A:** Yes — abstract classes may have constructors (called via `super(...)` from subclasses), fields, concrete methods, and abstract methods.

---

### Q16: What happens if a concrete subclass forgets to implement an abstract method?

**A:** Compile error — the subclass itself must also be marked `abstract`.

---

### Q17: Can a method be both `abstract` and `final` / `private` / `static`?

**A:** **No** to all three:

- `abstract final` → contradicts (must be overridable).
- `abstract private` → can't be inherited/overridden.
- `abstract static` → static methods aren't overridden.

---

## 🔌 Interfaces

### Q18: What members can an interface contain?

**A:**

- `public abstract` methods (default modifiers).
- `public static final` constants (default modifiers).
- `default` methods (with body, since Java 8).
- `static` methods (with body, since Java 8).
- `private` methods (since Java 9, helpers shared by other methods in the interface).

---

### Q19: How is a `static` interface method called?

**A:** Through the **interface name**, not via an instance:

```java
Vehicle.info();
```

---

### Q20: What is a `default` method, and why was it added?

**A:** A method with a body in an interface. It allows interfaces to **evolve without breaking** existing implementations.

---

### Q21: A class implements two interfaces with the same default method. What happens?

**A:** Compile error — the class **must override** the method to resolve the conflict, optionally calling either via `Iface.super.method()`:

```java
class Pegasus implements Flyer, Walker {
    @Override
    public String id() { return Flyer.super.id() + " & " + Walker.super.id(); }
}
```

---

### Q22: Can a class implement multiple interfaces?

**A:** Yes — Java supports multiple **interface** inheritance even though only single class inheritance is allowed.

---

## 🌀 Polymorphism & Casting

### Q23: What is polymorphism?

**A:** A parent reference can point to a child object, and instance method calls are resolved at **runtime** (dynamic dispatch):

```java
Animal a = new Dog();
a.speak();   // Dog's speak()
```

---

### Q24: Difference between upcasting and downcasting?

**A:**

- **Upcasting** (subclass → superclass): implicit, always safe.
- **Downcasting** (superclass → subclass): requires an explicit cast and may throw `ClassCastException` at runtime.

---

### Q25: How do you safely downcast?

**A:** Guard the cast with `instanceof` (or use Java 16+ pattern matching):

```java
if (a instanceof Dog d) {
    d.fetch();
}
```

---

### Q26: Are method calls and field references resolved the same way?

**A:** **No.**

- **Instance methods:** runtime type (overridden).
- **Static methods, fields, and `private` methods:** reference type (no dynamic dispatch).

---

## 🧠 Conceptual / Trap Questions

### Q27: Inheritance vs. composition?

**A:**

- **Inheritance:** IS-A relationship via `extends`. Tightly coupled.
- **Composition:** HAS-A relationship — class holds a reference. More flexible; favored for reuse.

---

### Q28: Can a subclass widen the visibility of an overridden method?

**A:** Yes (e.g., `protected` → `public`), but **never narrow** it.

---

### Q29: Can a subclass throw a new checked exception not declared by the parent?

**A:** No. The override may throw the **same** or **narrower** checked exceptions, or none at all — but not **broader** ones.

---

### Q30: What's the output?

```java
class A { static String hi() { return "A"; } }
class B extends A { static String hi() { return "B"; } }

A a = new B();
System.out.println(a.hi());
```

**A:** `A` — `static` methods are resolved by **reference type**, not runtime type.

---

### Q31: Key takeaways

- Single class inheritance, multiple interface inheritance.
- Constructor chains run **parent → child**.
- **Override** instance methods; **hide** static methods/fields (and avoid hiding).
- Abstract classes mix abstract + concrete; interfaces are pure contracts (with default/static helpers).
- Polymorphism resolves instance methods at runtime — guard downcasts with `instanceof`.
