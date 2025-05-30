## 1. Strings and `StringBuilder`

### Strings (Immutable)

- Once created, the contents of a `String` cannot change.
    
- Benefits: thread-safety, safe sharing via the String pool, predictable behavior.
    
- Common methods:
    
    - `length()`, `charAt(int)`, `substring(int, int)`
        
    - `equals(Object)`, `compareTo(String)`, `indexOf(String)`
        
    - `toLowerCase()`, `toUpperCase()`, `trim()`
        
- **Comparison:** Always use `equals()` for content equality; `==` checks reference equality.
    
    ```java
    String s1 = "hello";
    String s2 = new String("hello");
    System.out.println(s1 == s2);       // false
    System.out.println(s1.equals(s2));  // true
    ```
    

### StringBuilder (Mutable)

- Designed for efficient, in-place string modifications.
    
- Common methods: `append(...)`, `insert(...)`, `delete(...)`, `reverse()`, `toString()`
    
    ```java
    StringBuilder sb = new StringBuilder("hello");
    sb.append(" ").append("world").append("!");
    System.out.println(sb);  // "hello world!"
    ```
    

---

## 2. Arrays

### Declaration & Initialization

- Fixed-size, zero-based index collections.
    
- Default values: primitives → `0`, boolean → `false`, objects → `null`
    
    ```java
    int[] nums = new int[5];
    String[] days = {"Sun","Mon"};
    int[][] grid = {{1,2},{3,4,5}};
    ```
    

### Operations

- Access: `arr[index]`; Length: `arr.length`
    
- Sorting & Searching:
    
    ```java
    Arrays.sort(arr);
    int idx = Arrays.binarySearch(arr, key); // must sort before search
    ```
    

---

## 3. ArrayList

- A dynamic, resizable array from `java.util`.
    
- Common methods: `add()`, `remove()`, `get()`, `set()`, `size()`
    
    ```java
    ArrayList<Integer> list = new ArrayList<>();
    list.add(10); // autoboxed
    int n = list.get(0); // unboxed
    ```
    
- Capacity expands automatically. For efficiency, predefine size using constructor.
    

### Conversion

- Array → List:
    
    ```java
    String[] arr = {"a","b"};
    List<String> list = Arrays.asList(arr);
    ```
    
- List → Array:
    
    ```java
    String[] newArr = list.toArray(new String[0]);
    ```
    

### Interview Questions

- Q: What’s the difference between `ArrayList` and array?
    
- A: Arrays are fixed-size and can hold primitives; `ArrayList` is resizable and holds objects only.
    
- Q: What happens if you modify a list returned by `Arrays.asList()`?
    
- A: It throws `UnsupportedOperationException` if you try to add/remove elements.
    

---

## 4. Date & Time API (`java.time`)

### Core Classes

- `LocalDate`, `LocalTime`, `LocalDateTime` – immutable and thread-safe.
    
    ```java
    LocalDate date = LocalDate.now();
    LocalTime time = LocalTime.of(14, 30);
    LocalDateTime dt = LocalDateTime.of(date, time);
    ```
    

### Period & Duration

- Use `Period.between(start, end)` for date difference.
    
    ```java
    Period p = Period.between(LocalDate.of(2020,1,1), LocalDate.of(2023,6,15));
    ```
    

### Formatting & Parsing

- With `DateTimeFormatter`:
    
    ```java
    DateTimeFormatter fmt = DateTimeFormatter.ofPattern("MM/dd/yyyy");
    String str = date.format(fmt);
    LocalDate parsed = LocalDate.parse(str, fmt);
    ```
    

### Interview Questions

- Q: What is the difference between `Period` and `Duration`?
    
- A: `Period` is for date differences (years/months/days); `Duration` is for time-based (hours/minutes/seconds).
    
- Q: Are `LocalDate`, `LocalTime` mutable?
    
- A: No, they are immutable and thread-safe.
    

---

_Mastering these APIs helps with writing robust Java code and succeeding in technical interviews._