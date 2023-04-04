[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 693\. Binary Number with Alternating Bits

Easy

Given a positive integer, check whether it has alternating bits: namely, if two adjacent bits will always have different values.

**Example 1:**

**Input:** n = 5

**Output:** true

**Explanation:** The binary representation of 5 is: 101 

**Example 2:**

**Input:** n = 7

**Output:** false

**Explanation:** The binary representation of 7 is: 111.

**Example 3:**

**Input:** n = 11

**Output:** false

**Explanation:** The binary representation of 11 is: 1011.

**Constraints:**

*   <code>1 <= n <= 2<sup>31</sup> - 1</code>

## Solution

```java
public class Solution {
    public boolean hasAlternatingBits(int n) {
        int prev = -1;
        while (n != 0) {
            int v = n & 1;
            n = n >> 1;
            if (prev == v) {
                return false;
            }
            prev = v;
        }
        return true;
    }
}
```