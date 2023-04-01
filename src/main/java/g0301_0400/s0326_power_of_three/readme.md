[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 326\. Power of Three

Easy

Given an integer `n`, return _`true` if it is a power of three. Otherwise, return `false`_.

An integer `n` is a power of three, if there exists an integer `x` such that <code>n == 3<sup>x</sup></code>.

**Example 1:**

**Input:** n = 27

**Output:** true 

**Example 2:**

**Input:** n = 0

**Output:** false 

**Example 3:**

**Input:** n = 9

**Output:** true 

**Constraints:**

*   <code>-2<sup>31</sup> <= n <= 2<sup>31</sup> - 1</code>

**Follow up:** Could you solve it without loops/recursion?

## Solution

```java
public class Solution {
    // regular method that has a loop
    public boolean isPowerOfThree(int n) {
        if (n < 3 && n != 1) {
            return false;
        }
        while (n != 1) {
            if (n % 3 != 0) {
                return false;
            }
            n /= 3;
        }
        return true;
    }
}
```