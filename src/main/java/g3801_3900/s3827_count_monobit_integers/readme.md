[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3827\. Count Monobit Integers

Easy

You are given an integer `n`.

An integer is called **Monobit** if all bits in its binary representation are the same.

Return the count of **Monobit** integers in the range `[0, n]` (inclusive).

**Example 1:**

**Input:** n = 1

**Output:** 2

**Explanation:**

*   The integers in the range `[0, 1]` have binary representations `"0"` and `"1"`.
*   Each representation consists of identical bits. Thus, the answer is 2.

**Example 2:**

**Input:** n = 4

**Output:** 3

**Explanation:**

*   The integers in the range `[0, 4]` include binaries `"0"`, `"1"`, `"10"`, `"11"`, and `"100"`.
*   Only 0, 1 and 3 satisfy the Monobit condition. Thus, the answer is 3.

**Constraints:**

*   `0 <= n <= 1000`

## Solution

```java
public class Solution {
    public int countMonobit(int n) {
        int count = 1;

        int val = 1;
        while (val <= n) {
            count++;
            val = (val << 1) | 1;
        }

        return count;
    }
}
```