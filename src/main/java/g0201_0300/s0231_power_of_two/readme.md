[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 231\. Power of Two

Easy

Given an integer `n`, return _`true` if it is a power of two. Otherwise, return `false`_.

An integer `n` is a power of two, if there exists an integer `x` such that <code>n == 2<sup>x</sup></code>.

**Example 1:**

**Input:** n = 1

**Output:** true

**Explanation:** 2<sup>0</sup> = 1 

**Example 2:**

**Input:** n = 16

**Output:** true

**Explanation:** 2<sup>4</sup> = 16 

**Example 3:**

**Input:** n = 3

**Output:** false 

**Example 4:**

**Input:** n = 4

**Output:** true 

**Example 5:**

**Input:** n = 5

**Output:** false 

**Constraints:**

*   <code>-2<sup>31</sup> <= n <= 2<sup>31</sup> - 1</code>

**Follow up:** Could you solve it without loops/recursion?

## Solution

```java
public class Solution {
    public boolean isPowerOfTwo(int n) {
        if (n <= 0) {
            return false;
        }
        while (true) {
            if (n == 1) {
                return true;
            }
            if (n % 2 == 1) {
                return false;
            }
            n /= 2;
        }
    }
}
```