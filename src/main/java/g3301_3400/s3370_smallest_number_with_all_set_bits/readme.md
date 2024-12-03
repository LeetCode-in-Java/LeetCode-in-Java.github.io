[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3370\. Smallest Number With All Set Bits

Easy

You are given a _positive_ number `n`.

Return the **smallest** number `x` **greater than** or **equal to** `n`, such that the binary representation of `x` contains only **set** bits.

A **set** bit refers to a bit in the binary representation of a number that has a value of `1`.

**Example 1:**

**Input:** n = 5

**Output:** 7

**Explanation:**

The binary representation of 7 is `"111"`.

**Example 2:**

**Input:** n = 10

**Output:** 15

**Explanation:**

The binary representation of 15 is `"1111"`.

**Example 3:**

**Input:** n = 3

**Output:** 3

**Explanation:**

The binary representation of 3 is `"11"`.

**Constraints:**

*   `1 <= n <= 1000`

## Solution

```java
public class Solution {
    public int smallestNumber(int n) {
        int res = 1;
        while (res < n) {
            res = res * 2 + 1;
        }
        return res;
    }
}
```