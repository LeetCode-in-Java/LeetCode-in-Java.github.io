[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2376\. Count Special Integers

Hard

We call a positive integer **special** if all of its digits are **distinct**.

Given a **positive** integer `n`, return _the number of special integers that belong to the interval_ `[1, n]`.

**Example 1:**

**Input:** n = 20

**Output:** 19

**Explanation:** All the integers from 1 to 20, except 11, are special. Thus, there are 19 special integers. 

**Example 2:**

**Input:** n = 5

**Output:** 5

**Explanation:** All the integers from 1 to 5 are special. 

**Example 3:**

**Input:** n = 135

**Output:** 110

**Explanation:** There are 110 integers from 1 to 135 that are special.

Some of the integers that are not special are: 22, 114, and 131.

**Constraints:**

*   <code>1 <= n <= 2 * 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    private int[] cntMap;
    // number n as an array, splitted by each digit
    private int[] digits;

    public int countSpecialNumbers(int n) {
        if (n < 10) {
            return n;
        }
        int len = (int) Math.log10(n) + 1;
        cntMap = new int[len - 1];
        int res = countUnbounded(len);
        digits = new int[len];
        for (int i = len - 1; i >= 0; i--, n /= 10) {
            digits[i] = n % 10;
        }
        return res + dfs(0, 0);
    }

    private int dfs(int i, int mask) {
        if (i == digits.length) {
            return 1;
        }
        int res = 0;
        for (int j = i == 0 ? 1 : 0; j < digits[i]; j++) {
            if ((mask & (1 << j)) == 0) {
                // unbounded lens left
                int unbounded = digits.length - 2 - i;
                res += unbounded >= 0 ? count(unbounded, 9 - i) : 1;
            }
        }
        if ((mask & (1 << digits[i])) == 0) {
            res += dfs(i + 1, mask | (1 << digits[i]));
        }
        return res;
    }

    private int count(int i, int max) {
        if (i == 0) {
            return max;
        }
        return (max - i) * count(i - 1, max);
    }

    private int countUnbounded(int len) {
        int res = 9;
        cntMap[0] = 9;
        for (int i = 0; i < len - 2; i++) {
            res += cntMap[i + 1] = cntMap[i] * (9 - i);
        }
        return res;
    }
}
```