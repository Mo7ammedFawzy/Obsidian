
Q1: Why does the following java code not compile?
```java
public void zooAnimalCheckup(boolean isWeekend) {
    final int rest;
    if (isWeekend) rest = 5;
    System.out.print(rest); // DOES NOT COMPILE
}
```
A1: The variable rest a local final variable, and java requires the local variable must 
be assigned to a value before they are used.

Q2: What is the difference between **final**,**volatile**,**transient**

A2:
- final: instance variable must be initialized exactly once
- volatile: tells the **JVM** that the value inside this variable can be modified by other threads
- transient: instance variable should not be serialized with the class
