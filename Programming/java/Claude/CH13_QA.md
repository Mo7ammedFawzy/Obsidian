# CH13 — Concurrency: Q & A

---

### Q1. Which call actually starts a new thread?

**Options**
- A) `t.run();`
- B) `t.start();`
- C) `t.join();`
- D) `t.execute();`

**Correct Answer:** **B**

**Explanation**
`start()` schedules the thread and eventually invokes `run()` on the new thread. Calling `run()` directly executes synchronously on the current thread.

---

### Q2. Output?

```java
volatile int x = 0;
ExecutorService es = Executors.newFixedThreadPool(10);
for (int i = 0; i < 10_000; i++) es.submit(() -> x++);
es.shutdown();
es.awaitTermination(1, TimeUnit.MINUTES);
System.out.println(x);
```

**Options**
- A) Always `10000`.
- B) Always `0`.
- C) Frequently less than `10000` due to lost updates.
- D) Always throws.

**Correct Answer:** **C**

**Explanation**
`volatile` provides visibility, not atomicity. `x++` is read-modify-write — concurrent invocations lose updates. Use `AtomicInteger` or `synchronized`.

---

### Q3. (True/False) `wait()`, `notify()`, and `notifyAll()` may be called outside a `synchronized` block.

**Correct Answer:** **False**

**Explanation**
The current thread must own the monitor — call them inside a `synchronized` block on the same object. Otherwise: `IllegalMonitorStateException`.

---

### Q4. Which executor is best for **millions of blocking I/O tasks** in Java 21?

**Options**
- A) `Executors.newFixedThreadPool(1000)`
- B) `Executors.newCachedThreadPool()`
- C) `Executors.newVirtualThreadPerTaskExecutor()`
- D) `Executors.newSingleThreadExecutor()`

**Correct Answer:** **C**

**Explanation**
Virtual threads are extremely cheap; one per task scales to millions. Platform-thread pools cap at the OS thread count.

---

### Q5. What does `Future.get(timeout, unit)` throw if the time elapses?

**Options**
- A) `InterruptedException`
- B) `ExecutionException`
- C) `TimeoutException`
- D) `CancellationException`

**Correct Answer:** **C**

**Explanation**
A timeout throws `TimeoutException` (the future is *not* cancelled automatically). The other three are thrown in different scenarios.

---

### Q6. Which collection allows safe concurrent updates without throwing CME?

**Options**
- A) `ArrayList`
- B) `HashMap`
- C) `ConcurrentHashMap`
- D) `LinkedList`

**Correct Answer:** **C**

**Explanation**
`ConcurrentHashMap` is designed for concurrent access. Only it provides the right guarantees here.

---

### Q7. (True/False) Calling `start()` twice on the same `Thread` instance is allowed.

**Correct Answer:** **False**

**Explanation**
A thread can only be started once. Subsequent calls throw `IllegalThreadStateException`.

---

### Q8. Which type's `call()` is allowed to throw checked exceptions?

**Options**
- A) `Runnable`
- B) `Callable<T>`
- C) `Thread`
- D) `Supplier<T>`

**Correct Answer:** **B**

**Explanation**
`Callable.call()` declares `throws Exception`. `Runnable.run()` cannot throw checked exceptions.

---

### Q9. Output?

```java
ExecutorService es = Executors.newFixedThreadPool(2);
es.submit(() -> { while (true) {} });
System.out.println("submitted");
```

**Options**
- A) Prints "submitted", JVM exits cleanly.
- B) Prints "submitted", JVM hangs.
- C) Compile error.
- D) Throws immediately.

**Correct Answer:** **B**

**Explanation**
The pool creates **non-daemon** threads by default. A running task plus the lack of `shutdown()` keeps the JVM alive indefinitely.

---

### Q10. Which is a one-shot synchronizer that cannot be reused?

**Options**
- A) `CountDownLatch`
- B) `CyclicBarrier`
- C) `Semaphore`
- D) `ReentrantLock`

**Correct Answer:** **A**

**Explanation**
`CountDownLatch` reaches zero and stays there. `CyclicBarrier` resets after every trip.

---

### Q11. (True/False) `parallelStream()` always preserves encounter order.

**Correct Answer:** **False**

**Explanation**
Order depends on the source and terminal op. `forEach` may not be ordered; use `forEachOrdered` to enforce order. Many ops keep order on `LinkedHashSet`/`List` sources.

---

### Q12. Output?

```java
AtomicInteger n = new AtomicInteger();
n.set(5);
System.out.println(n.compareAndSet(4, 7) + " " + n.get());
```

**Options**
- A) `true 7`
- B) `false 5`
- C) `false 7`
- D) `true 5`

**Correct Answer:** **B**

**Explanation**
The expected value `4` doesn't match the current `5`, so CAS fails and the value remains `5`.

---

### Q13. What does `synchronized` provide?

**Options**
- A) Mutual exclusion only.
- B) Visibility only.
- C) Mutual exclusion **and** memory visibility.
- D) Atomicity for compound operations only.

**Correct Answer:** **C**

**Explanation**
Synchronized blocks form a happens-before edge between unlock and lock — providing both mutex and visibility.

---

### Q14. Which scenario is a deadlock?

**Options**
- A) Two threads each acquire one lock, then attempt to acquire the other.
- B) A thread continues yielding to another that yields back.
- C) A thread never gets CPU because higher-priority threads dominate.
- D) Two threads execute on the same CPU sequentially.

**Correct Answer:** **A**

**Explanation**
Classic AB/BA deadlock. **B** is livelock; **C** is starvation; **D** is just sequential execution.

---

### Q15. (True/False) `ReentrantLock.lock()` throws `InterruptedException`.

**Correct Answer:** **False**

**Explanation**
`lock()` does not — it ignores interruption. `lockInterruptibly()` is the variant that throws.

---

### Q16. Which is a correct way to safely release a `ReentrantLock`?

**Options**
- A) Call `unlock()` only on success path.
- B) Use `try/finally` and call `unlock()` in `finally`.
- C) Rely on garbage collection.
- D) Call `Thread.currentThread().interrupt()`.

**Correct Answer:** **B**

**Explanation**
Always release in `finally` to ensure unlock runs even when the try-block throws.

---

### Q17. What does `BlockingQueue.put` do when the queue is full?

**Options**
- A) Returns false.
- B) Throws.
- C) Blocks until space is available.
- D) Discards the head.

**Correct Answer:** **C**

**Explanation**
`put` blocks until space is available. `offer` returns false (or false after timeout). `add` throws.

---

### Q18. Output?

```java
CompletableFuture<Integer> cf = CompletableFuture
    .supplyAsync(() -> 5)
    .thenApply(x -> x * 2)
    .thenApply(x -> x + 1);
System.out.println(cf.join());
```

**Options**
- A) `10`
- B) `11`
- C) `15`
- D) Throws

**Correct Answer:** **B**

**Explanation**
Pipeline: `5 → *2 = 10 → +1 = 11`.

---

### Q19. (True/False) Virtual threads are scheduled by the OS like platform threads.

**Correct Answer:** **False**

**Explanation**
Virtual threads are scheduled **by the JVM** on a small pool of carrier (platform) threads. The OS sees only the carriers.

---

### Q20. Which method shuts down the executor *immediately*, interrupting running tasks?

**Options**
- A) `shutdown()`
- B) `shutdownNow()`
- C) `awaitTermination()`
- D) `close()`

**Correct Answer:** **B**

**Explanation**
`shutdownNow()` attempts to stop running tasks (via interruption) and returns the queued tasks. `shutdown()` lets queued tasks finish.
