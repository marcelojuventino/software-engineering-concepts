# Software Engineering Concepts

- [Big O Notation](#big-o-notation)

## Big O Notation

Big O notation is a way to describe the efficiency of algorithms, specifically in terms of time and space as the input size grows.

### Common Big O Notations

**O(1) \- Constant Time**: The runtime doesn't change with the input size.

Example: Accessing an element in an array.

```java
public static int getElement(int[] array, int index) {
    return array[index]; // O(1)
}
```
---
**O(n) - Linear Time:** The runtime increases linearly with the input size.

Example: Linear search in an array.

```java
public static int linearSearch(int[] array, int target) {
    for (int i = 0; i < array.length; i++) {
        if (array[i] == target) {
            return i; // O(1)
        }
    }
    return -1; // O(1)
}
```
---
**O(n^2) - Quadratic Time:** The runtime increases quadratically with the input size.

Example: Bubble sort algorithm.

```java
public static int linearSearch(int[] array, int target) {
    for (int i = 0; i < array.length; i++) {
        if (array[i] == target) {
            return i; // O(1)
        }
    }
    return -1; // O(1)
}
```


### Visualization of Big O

Think of Big O notation as a scale of efficiency:

1. O(1) is super fast.
2. O(n) is moderate.
3. O(n^2) is slower as the input size grows.

### Summary

Big O notation provides a high-level understanding of algorithm performance by focusing on the most significant factor that impacts runtime or space usage.
