## Overview
This chapter focuses on Java's exception handling mechanism: how exceptions work, how to handle them using try-catch-finally, the use of try-with-resources, and recognizing common Java exception types.

---

## Exception Types

### 1. Checked Exceptions
- Must be caught or declared.
- Example: `IOException`, `FileNotFoundException`.

### 2. Unchecked Exceptions (Runtime Exceptions)
- Do **not** need to be declared or caught.
- Example: `NullPointerException`, `ArithmeticException`, `ArrayIndexOutOfBoundsException`.

### 3. Errors
- System-level problems, not meant to be caught.
- Example: `OutOfMemoryError`, `StackOverflowError`.

---

## Using try-catch

```java
try {
    // Risky code
} catch (SpecificException e) {
    // Handle specific
} catch (AnotherException e) {
    // Handle another
}
```

### Multi-catch (Java 7+)

```java
try {
    // Risky code
} catch (IOException | SQLException e) {
    System.out.println(e.getMessage());
}
```

### Catch Order
- More specific exceptions **must** come before more general exceptions.

---

## finally Block

- Runs whether or not an exception occurs.

```java
try {
    // Risky code
} catch (Exception e) {
    // Handle
} finally {
    // Always runs
}
```

---

## Try-with-Resources (Java 7+)

Automatically closes resources that implement `AutoCloseable`:

```java
try (BufferedReader br = new BufferedReader(new FileReader("file.txt"))) {
    System.out.println(br.readLine());
} catch (IOException e) {
    e.printStackTrace();
}
```

---

## Throwing Exceptions

```java
public void process(int age) {
    if (age < 0) {
        throw new IllegalArgumentException("Negative age not allowed");
    }
}
```

### Declaring Exceptions

```java
public void riskyMethod() throws IOException, SQLException {
    // risky code
}
```

---

## Common Runtime Exceptions

| Exception                      | Cause                             |
| ------------------------------ | --------------------------------- |
| NullPointerException           | Accessing methods on null objects |
| ArrayIndexOutOfBoundsException | Accessing invalid array index     |
| ClassCastException             | Invalid object casting            |
| NumberFormatException          | Invalid number parsing            |
| ArithmeticException            | Division by zero                  |
|                                |                                   |

---

## Best Practices

- **Catch specific exceptions first**
- **Use try-with-resources**
- **Avoid empty catch blocks**
- **Do not catch Errors unless absolutely necessary**
- **Avoid flow control via exceptions**

---

## Sample Full Example:

```java
public static void readFile(String fileName) {
    try (BufferedReader br = new BufferedReader(new FileReader(fileName))) {
        String line = br.readLine();
        int num = Integer.parseInt(line);
        System.out.println(num);
    } catch (FileNotFoundException e) {
        System.out.println("File not found!");
    } catch (IOException e) {
        System.out.println("IO Error!");
    } catch (NumberFormatException e) {
        System.out.println("Invalid number format!");
    }
}
```

---

> **Source:** OCA Java SE 8 Programmer I Study Guide by Jeanne Boyarsky and Scott Selikoff.