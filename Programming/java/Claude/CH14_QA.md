# CH14 — I/O: Q & A

---

### Q1. Which class is for character output to a file?

**Options**
- A) `FileInputStream`
- B) `FileWriter`
- C) `FileOutputStream`
- D) `DataOutputStream`

**Correct Answer:** **B**

**Explanation**
`FileWriter` is a `Writer` (chars). `FileOutputStream` and `DataOutputStream` write bytes; `FileInputStream` is for reading.

---

### Q2. Output?

```java
Path a = Path.of("/home/user");
Path b = a.resolve("/etc/passwd");
System.out.println(b);
```

**Options**
- A) `/home/user/etc/passwd`
- B) `/etc/passwd`
- C) Throws
- D) Empty path

**Correct Answer:** **B**

**Explanation**
`Path.resolve(absolute)` returns the absolute argument unchanged — it does **not** join.

---

### Q3. (True/False) `Files.lines(Path)` returns an eager `List<String>`.

**Correct Answer:** **False**

**Explanation**
It returns a `Stream<String>` that reads lazily. You **must** close it (try-with-resources) or you'll leak the file handle.

---

### Q4. Which serializes correctly?

```java
class A {}
class B extends A implements Serializable {
    int x;
}
```

**Options**
- A) `B` cannot be serialized — parent isn't `Serializable`.
- B) `B` can be serialized only if `A` has a no-arg constructor.
- C) `B` can never be deserialized.
- D) `A` will also be serialized.

**Correct Answer:** **B**

**Explanation**
On deserialization, `A`'s no-arg constructor runs to reconstruct parent state. If `A` lacks one, `InvalidClassException` is thrown.

---

### Q5. Which I/O class **buffers** chars?

**Options**
- A) `BufferedInputStream`
- B) `BufferedReader`
- C) `BufferedOutputStream`
- D) `DataInputStream`

**Correct Answer:** **B**

**Explanation**
`BufferedReader` operates on chars. `BufferedInputStream`/`BufferedOutputStream` buffer bytes. `DataInputStream` doesn't add buffering.

---

### Q6. (True/False) A `transient` field of primitive type `int` becomes `null` on deserialization.

**Correct Answer:** **False**

**Explanation**
Primitives default to `0`, not `null`. Reference fields default to `null`. The point of `transient` is the field isn't part of the byte stream and is left at its default.

---

### Q7. What does `Files.copy(src, dst)` do if `dst` already exists?

**Options**
- A) Overwrites silently.
- B) Skips the copy.
- C) Throws `FileAlreadyExistsException`.
- D) Renames `dst`.

**Correct Answer:** **C**

**Explanation**
You must pass `StandardCopyOption.REPLACE_EXISTING` to allow overwrite.

---

### Q8. Which method reads the whole file as a single string?

**Options**
- A) `Files.readAllBytes`
- B) `Files.readAllLines`
- C) `Files.readString`
- D) `Files.lines`

**Correct Answer:** **C**

**Explanation**
`readString(Path)` returns the file contents as `String`. `readAllBytes` is `byte[]`, `readAllLines` is `List<String>`, `lines` is a `Stream<String>`.

---

### Q9. Output?

```java
Path a = Path.of("a/b/c.txt");
System.out.println(a.getFileName());
```

**Options**
- A) `c.txt`
- B) `a/b`
- C) `a/b/c.txt`
- D) Throws

**Correct Answer:** **A**

**Explanation**
`getFileName()` returns the last name in the path — `c.txt`. `getParent()` would return `a/b`.

---

### Q10. Which directive is used to ensure a stream from `Files.walk` is closed?

**Options**
- A) `flush()`
- B) try-with-resources
- C) `Stream.close()` from inside a `forEach`
- D) None — it auto-closes

**Correct Answer:** **B**

**Explanation**
The returned `Stream<Path>` implements `AutoCloseable`. Wrap with try-with-resources to release file handles.

---

### Q11. (True/False) `static` fields are persisted by default during serialization.

**Correct Answer:** **False**

**Explanation**
Static fields belong to the class, not the instance, so they aren't part of the serialized form.

---

### Q12. Output?

```java
Path p = Path.of("a/b").relativize(Path.of("a/b/c/d"));
System.out.println(p);
```

**Options**
- A) `a/b/c/d`
- B) `c/d`
- C) `..`
- D) `b/c/d`

**Correct Answer:** **B**

**Explanation**
`relativize` returns the relative path that, when resolved against the original, yields the argument. From `a/b` to `a/b/c/d` is `c/d`.

---

### Q13. Which constructor specifies UTF-8 explicitly?

**Options**
- A) `new FileWriter("a.txt")`
- B) `new FileWriter("a.txt", StandardCharsets.UTF_8)`
- C) `new FileOutputStream("a.txt")`
- D) `new BufferedWriter("a.txt")`

**Correct Answer:** **B**

**Explanation**
The 2-arg `FileWriter` accepts a `Charset` since Java 11. The default no-arg form uses the platform charset.

---

### Q14. Which class lets you read a password without echo?

**Options**
- A) `Scanner`
- B) `BufferedReader`
- C) `Console`
- D) `InputStreamReader`

**Correct Answer:** **C**

**Explanation**
`System.console()` returns a `Console` whose `readPassword()` doesn't echo. May be `null` in non-interactive contexts (e.g., IDE).

---

### Q15. (True/False) `Files.exists(path)` returns `true` for both files and directories.

**Correct Answer:** **True**

**Explanation**
Yes — to distinguish them use `isRegularFile` or `isDirectory`.

---

### Q16. Output?

```java
Path p = Path.of("a", "b", "c");
System.out.println(p.getNameCount());
```

**Options**
- A) `0`
- B) `1`
- C) `3`
- D) Compile error

**Correct Answer:** **C**

**Explanation**
There are three name elements: `a`, `b`, `c`.

---

### Q17. Which is **NOT** required for a class to serialize correctly without warnings?

**Options**
- A) `implements Serializable`
- B) Declared `serialVersionUID`
- C) Public no-arg constructor on the class itself
- D) All non-transient fields must be serializable

**Correct Answer:** **C**

**Explanation**
The class itself doesn't need a no-arg constructor; the **non-serializable parent** does. The class only needs `Serializable`, fields that themselves serialize, and a (recommended) `serialVersionUID`.

---

### Q18. Output?

```java
String s = Files.readString(Path.of("missing.txt"));
```

**Options**
- A) Empty string.
- B) `null`
- C) Throws `NoSuchFileException`.
- D) Throws `FileNotFoundException`.

**Correct Answer:** **C**

**Explanation**
NIO.2 throws `NoSuchFileException` (a subclass of `FileSystemException`). The legacy `java.io` would throw `FileNotFoundException`.

---

### Q19. Which is the lazy directory traversal API?

**Options**
- A) `File.listFiles()`
- B) `Files.list(Path)`
- C) `Files.walk(Path)`
- D) Both **B** and **C**

**Correct Answer:** **D**

**Explanation**
Both return lazy streams; `list` is one level deep, `walk` is recursive. `File.listFiles()` is eager and returns an array.

---

### Q20. (True/False) `Files.newBufferedReader(path)` defaults to UTF-8 in modern Java.

**Correct Answer:** **True**

**Explanation**
Since Java 17, NIO.2 defaults to UTF-8. The legacy `FileReader` constructor without a `Charset` still uses the platform default.
