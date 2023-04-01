[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2457\. Minimum Addition to Make Integer Beautiful

Medium

You are given two positive integers `n` and `target`.

An integer is considered **beautiful** if the sum of its digits is less than or equal to `target`.

Return the _minimum **non-negative** integer_ `x` _such that_ `n + x` _is beautiful_. The input will be generated such that it is always possible to make `n` beautiful.

**Example 1:**

**Input:** n = 16, target = 6

**Output:** 4

**Explanation:** Initially n is 16 and its digit sum is 1 + 6 = 7. After adding 4, n becomes 20 and digit sum becomes 2 + 0 = 2. It can be shown that we can not make n beautiful with adding non-negative integer less than 4.

**Example 2:**

**Input:** n = 467, target = 6

**Output:** 33

**Explanation:** Initially n is 467 and its digit sum is 4 + 6 + 7 = 17. After adding 33, n becomes 500 and digit sum becomes 5 + 0 + 0 = 5. It can be shown that we can not make n beautiful with adding non-negative integer less than 33.

**Example 3:**

**Input:** n = 1, target = 1

**Output:** 0

**Explanation:** Initially n is 1 and its digit sum is 1, which is already smaller than or equal to target.

**Constraints:**

*   <code>1 <= n <= 10<sup>12</sup></code>
*   `1 <= target <= 150`
*   The input will be generated such that it is always possible to make `n` beautiful.

## Solution

```java
public class Solution {
    public long makeIntegerBeautiful(long n, int target) {
        if (sumOfDigits(n) <= target) {
            return 0;
        }
        long old = n;
        long newNumber = 1;
        while (sumOfDigits(n) > target) {
            newNumber = newNumber * 10;
            n = n / 10 + 1;
        }
        newNumber = (n) * newNumber;
        return newNumber - old;
    }

    public long sumOfDigits(long n) {
        long sum = 0;
        while (n > 0) {
            sum = sum + n % 10;
            n = n / 10;
        }
        return sum;
    }
}
```