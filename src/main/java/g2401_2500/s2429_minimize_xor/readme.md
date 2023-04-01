[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2429\. Minimize XOR

Medium

Given two positive integers `num1` and `num2`, find the positive integer `x` such that:

*   `x` has the same number of set bits as `num2`, and
*   The value `x XOR num1` is **minimal**.

Note that `XOR` is the bitwise XOR operation.

Return _the integer_ `x`. The test cases are generated such that `x` is **uniquely determined**.

The number of **set bits** of an integer is the number of `1`'s in its binary representation.

**Example 1:**

**Input:** num1 = 3, num2 = 5

**Output:** 3

**Explanation:** The binary representations of num1 and num2 are 0011 and 0101, respectively. The integer **3** has the same number of set bits as num2, and the value `3 XOR 3 = 0` is minimal.

**Example 2:**

**Input:** num1 = 1, num2 = 12

**Output:** 3

**Explanation:** The binary representations of num1 and num2 are 0001 and 1100, respectively. The integer **3** has the same number of set bits as num2, and the value `3 XOR 1 = 2` is minimal.

**Constraints:**

*   <code>1 <= num1, num2 <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int minimizeXor(int num1, int num2) {
        int bits = Integer.bitCount(num2);
        int result = 0;
        for (int i = 30; i >= 0 && bits > 0; --i) {
            if ((1 << i & num1) != 0) {
                --bits;
                result = result ^ (1 << i);
            }
        }
        for (int i = 0; i <= 30 && bits > 0; ++i) {
            if ((1 << i & num1) == 0) {
                --bits;
                result = result ^ (1 << i);
            }
        }
        return result;
    }
}
```