[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3704\. Count No-Zero Pairs That Sum to N

Hard

A **no-zero** integer is a **positive** integer that **does not contain the digit** 0 in its decimal representation.

Given an integer `n`, count the number of pairs `(a, b)` where:

*   `a` and `b` are **no-zero** integers.
*   `a + b = n`

Return an integer denoting the number of such pairs.

**Example 1:**

**Input:** n = 2

**Output:** 1

**Explanation:**

The only pair is `(1, 1)`.

**Example 2:**

**Input:** n = 3

**Output:** 2

**Explanation:**

The pairs are `(1, 2)` and `(2, 1)`.

**Example 3:**

**Input:** n = 11

**Output:** 8

**Explanation:**

The pairs are `(2, 9)`, `(3, 8)`, `(4, 7)`, `(5, 6)`, `(6, 5)`, `(7, 4)`, `(8, 3)`, and `(9, 2)`. Note that `(1, 10)` and `(10, 1)` do not satisfy the conditions because 10 contains 0 in its decimal representation.

**Constraints:**

*   <code>2 <= n <= 10<sup>15</sup></code>

## Solution

```java
public class Solution {
    public long countNoZeroPairs(long n) {
        int m = 0;
        long base = 1;
        while (base <= n) {
            m++;
            base = base * 10;
        }
        int[] digits = new int[m];
        long c = n;
        for (int i = 0; i < m; i++) {
            digits[i] = (int) (c % 10);
            c = c / 10;
        }
        long total = 0;
        long[] extra = {1, 0};
        base = 1;
        for (int p = 0; p < m; p++) {
            long[] nextExtra = {0, 0};
            for (int e = 0; e <= 1; e++) {
                for (int i = 1; i <= 9; i++) {
                    for (int j = 1; j <= 9; j++) {
                        if ((i + j + e) % 10 == digits[p]) {
                            nextExtra[(i + j + e) / 10] += extra[e];
                        }
                    }
                }
            }
            extra = nextExtra;
            base = base * 10;
            for (int e = 0; e <= 1; e++) {
                long left = n / base - e;
                if (left < 0) {
                    continue;
                }
                if (left == 0) {
                    total += extra[e];
                } else if (isGood(left)) {
                    total += 2 * extra[e];
                }
            }
        }

        return total;
    }

    private boolean isGood(long num) {
        while (num > 0) {
            if (num % 10 == 0) {
                return false;
            }
            num = num / 10;
        }
        return true;
    }
}
```