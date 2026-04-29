# CH12 — Modules: Q & A

---

### Q1. Which directive in `module-info.java` allows another module to see your **public** types?

**Options**
- A) `requires`
- B) `exports`
- C) `opens`
- D) `provides`

**Correct Answer:** **B**

**Explanation**
`exports pkg;` makes the package's public types visible to all other modules at compile and runtime. `requires` is for the reverse direction (this module reads another). `opens` only enables reflection.

---

### Q2. (True/False) `requires transitive java.base` must be declared explicitly to allow consumers to read `java.base`.

**Correct Answer:** **False**

**Explanation**
Every module implicitly requires `java.base`. There's no need to declare it; doing so isn't even allowed for `requires transitive java.base`.

---

### Q3. Which best describes an *automatic module*?

**Options**
- A) A jar with a `module-info.class` placed on the module path.
- B) A jar without a descriptor, placed on the module path; name derived from filename or `Automatic-Module-Name`.
- C) Any jar on the classpath.
- D) A JDK module like `java.sql`.

**Correct Answer:** **B**

**Explanation**
An automatic module is a "regular" jar without `module-info.class` placed on the module path. **A** describes a **named** module. **C** describes the **unnamed** module. **D** is a named module shipped with the JDK.

---

### Q4. What does `opens` do?

**Options**
- A) Exports the package and disables encapsulation entirely.
- B) Allows compile-time access from other modules.
- C) Permits deep reflective access at runtime.
- D) Adds the package to the public API.

**Correct Answer:** **C**

**Explanation**
`opens` is for **reflection** only. It does **not** grant compile-time access (you'd also need `exports`).

---

### Q5. (True/False) Two modules can both contain the same package.

**Correct Answer:** **False**

**Explanation**
Split packages are illegal in JPMS. A package must reside in **one** module only.

---

### Q6. Which command launches a module's main class?

**Options**
- A) `java mymod`
- B) `java --module-path mods --module mymod/com.foo.Main`
- C) `java --classpath mods.jar com.foo.Main`
- D) `java --jar mymod.jar`

**Correct Answer:** **B**

**Explanation**
The `--module-path` (or `-p`) and `--module` (`-m`) flags tell `java` where to find modules and which one to run. `--classpath` uses the unnamed module instead.

---

### Q7. Output of `jdeps --jdk-internals myapp.jar`?

**Options**
- A) Lists modules required by the jar.
- B) Reports usage of unsupported JDK internal APIs (e.g., `sun.misc.Unsafe`).
- C) Compiles the jar to a module.
- D) Splits the jar into modules.

**Correct Answer:** **B**

**Explanation**
This `jdeps` flag is specifically designed to flag uses of internal/unsupported JDK APIs that may break in newer Java versions.

---

### Q8. Which directive provides *implied readability* to consumers?

**Options**
- A) `requires`
- B) `requires transitive`
- C) `requires static`
- D) `exports to`

**Correct Answer:** **B**

**Explanation**
`requires transitive X` re-exports the readability edge — consumers of your module also read `X` without declaring it themselves.

---

### Q9. (True/False) A class in the unnamed module can read all named modules.

**Correct Answer:** **True**

**Explanation**
The unnamed module reads **every** other module for backward compatibility. The reverse is not true — a named module cannot `requires` the unnamed module.

---

### Q10. What does `requires static X` mean?

**Options**
- A) `X` is required at compile time only; absent at runtime is OK.
- B) `X` must always be present.
- C) `X` is loaded as a static dependency for performance.
- D) `X` is forbidden at runtime.

**Correct Answer:** **A**

**Explanation**
`requires static` makes the dependency optional at runtime: useful for compile-time-only annotations or optional integrations.

---

### Q11. Which name does the jar `commons-lang3-3.12.0.jar` get as an automatic module?

**Options**
- A) `commons-lang3-3.12.0`
- B) `commons.lang3`
- C) `org.apache.commons.lang3`
- D) `commons-lang3`

**Correct Answer:** **B**

**Explanation**
The version is stripped, dashes become dots: `commons.lang3`. (If the MANIFEST declares `Automatic-Module-Name`, that wins instead — typically `org.apache.commons.lang3`.)

---

### Q12. What's the result of compiling this descriptor?

```java
module a {
    requires b;
}
module b {
    requires a;
}
```

**Options**
- A) Compiles successfully — modules can mutually depend.
- B) Compile error — circular dependency.
- C) Warning only.
- D) Runtime error on launch.

**Correct Answer:** **B**

**Explanation**
Cyclic `requires` between named modules is forbidden by the compiler.

---

### Q13. Which directive is correct for service registration?

**Options**
- A) `provides com.x.Service to com.y;`
- B) `provides com.x.Service with com.x.impl.ServiceImpl;`
- C) `service com.x.Service implements com.x.impl.ServiceImpl;`
- D) `exports com.x.Service;`

**Correct Answer:** **B**

**Explanation**
The syntax is `provides ServiceType with Implementation`. The consumer side uses `uses ServiceType`.

---

### Q14. (True/False) `exports pkg to A;` allows package `pkg` to be consumed by *any* module.

**Correct Answer:** **False**

**Explanation**
A *qualified* export is restricted to the listed modules — only `A` (and any other named modules in the list) sees the package.

---

### Q15. Which is a runtime command to discover modules?

**Options**
- A) `java --list-modules`
- B) `java --print-modules`
- C) `javac --modules`
- D) `jdeps -m`

**Correct Answer:** **A**

**Explanation**
`java --list-modules` prints all observable modules (JDK + module path). The other options aren't valid flags.

---

### Q16. What does `jlink` do?

**Options**
- A) Compiles modules.
- B) Installs modules into Maven.
- C) Builds a custom runtime image with only the modules you need.
- D) Lists module dependencies.

**Correct Answer:** **C**

**Explanation**
`jlink` produces a trimmed JRE containing only the specified modules — useful for embedded or container deployments.

---

### Q17. Which is true about JDK module `java.base`?

**Options**
- A) Must be required explicitly.
- B) Implicitly required by every module.
- C) Cannot be referenced; it's hidden.
- D) Is the same as `java.se`.

**Correct Answer:** **B**

**Explanation**
`java.base` is the implicit dependency for every module — all `java.lang`, `java.util`, `java.io` types come from it. `java.se` is an aggregator module that transitively requires the SE modules.

---

### Q18. (True/False) An open module (declared `open module x { ... }`) opens **every** package for reflection.

**Correct Answer:** **True**

**Explanation**
`open module` is shorthand for marking *all* packages with `opens`. You can still selectively `exports`.

---

### Q19. Which command compiles a multi-module project?

**Options**
- A) `javac --module-source-path src -d out $(find src -name "*.java")`
- B) `javac -classpath src -d out $(find src -name "*.java")`
- C) `javac --modules src -d out`
- D) `javac --module src/* -d out`

**Correct Answer:** **A**

**Explanation**
`--module-source-path` tells `javac` to treat top-level directories under `src` as separate modules. The other options are invalid.

---

### Q20. What happens if a service has `uses X;` but no provider is found at runtime?

**Options**
- A) `ServiceLoader.load(X.class)` throws.
- B) `ServiceLoader.load(X.class)` returns an empty iterator/stream.
- C) The JVM refuses to start.
- D) `uses` is only a hint.

**Correct Answer:** **B**

**Explanation**
`ServiceLoader` returns an empty iterable; you must handle absence yourself (e.g., `findFirst()` returns `Optional.empty()`).
