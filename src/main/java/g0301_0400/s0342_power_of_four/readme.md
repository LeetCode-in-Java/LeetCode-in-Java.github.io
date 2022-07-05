[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 342\. Power of Four

Easy

Given an integer `n`, return _`true` if it is a power of four. Otherwise, return `false`_.

An integer `n` is a power of four, if there exists an integer `x` such that <code>n == 4<sup>x</sup></code>.

**Example 1:**

**Input:** n = 16

**Output:** true 

**Example 2:**

**Input:** n = 5

**Output:** false 

**Example 3:**

**Input:** n = 1

**Output:** true 

**Constraints:**

*   <code>-2<sup>31</sup> <= n <= 2<sup>31</sup> - 1</code>

**Follow up:** Could you solve it without loops/recursion?

## Solution

```java
public class Solution {
    public boolean isPowerOfFour(int n) {
        while (n >= 4) {
            if (n % 4 != 0) {
                return false;
            }
            n = n / 4;
        }

        return n == 1;
    }
}
```