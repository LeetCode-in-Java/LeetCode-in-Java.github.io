[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3747\. Count Distinct Integers After Removing Zeros

Medium

You are given a **positive** integer `n`.

For every integer `x` from 1 to `n`, we write down the integer obtained by removing all zeros from the decimal representation of `x`.

Return an integer denoting the number of **distinct** integers written down.

**Example 1:**

**Input:** n = 10

**Output:** 9

**Explanation:**

The integers we wrote down are 1, 2, 3, 4, 5, 6, 7, 8, 9, 1. There are 9 distinct integers (1, 2, 3, 4, 5, 6, 7, 8, 9).

**Example 2:**

**Input:** n = 3

**Output:** 3

**Explanation:**

The integers we wrote down are 1, 2, 3. There are 3 distinct integers (1, 2, 3).

**Constraints:**

*   <code>1 <= n <= 10<sup>15</sup></code>

## Solution

```java
public class Solution {
    public long countDistinct(long n) {
        String digits = Long.toString(n);
        int m = digits.length();
        long[] power9 = new long[m + 1];
        power9[0] = 1;
        for (int i = 1; i <= m; i++) {
            power9[i] = power9[i - 1] * 9;
        }
        long total = 0;
        for (int length = 1; length < m; length++) {
            total += power9[length];
        }
        for (int idx = 0; idx < m; idx++) {
            int d = digits.charAt(idx) - '0';
            if (d == 0) {
                return total;
            }
            total += (d - 1) * power9[m - idx - 1];
        }
        return total + 1;
    }
}
```