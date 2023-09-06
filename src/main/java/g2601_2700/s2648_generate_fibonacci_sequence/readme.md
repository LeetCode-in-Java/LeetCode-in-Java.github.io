[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2648\. Generate Fibonacci Sequence

Easy

Write a generator function that returns a generator object which yields the **fibonacci sequence**.

The **fibonacci sequence** is defined by the relation <code>X<sub>n</sub> = X<sub>n-1</sub> + X<sub>n-2</sub></code>.

The first few numbers of the series are `0, 1, 1, 2, 3, 5, 8, 13`.

**Example 1:**

**Input:** callCount = 5

**Output:** [0,1,1,2,3]

**Explanation:** 

    const gen = fibGenerator(); 
    gen.next().value; // 0 
    gen.next().value; // 1 
    gen.next().value; // 1 
    gen.next().value; // 2 
    gen.next().value; // 3

**Example 2:**

**Input:** callCount = 0

**Output:** []

**Explanation:** gen.next() is never called so nothing is outputted

**Constraints:**

*   `0 <= callCount <= 50`

## Solution

```typescript
public class Solution {
    public String decode(String value) {
        return value;
    }
}
```