[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3770\. Largest Prime from Consecutive Prime Sum

Medium

You are given an integer `n`.

Return the **largest prime number** less than or equal to `n` that can be expressed as the **sum** of one or more **consecutive prime numbers** starting from 2. If no such number exists, return 0.

**Example 1:**

**Input:** n = 20

**Output:** 17

**Explanation:**

The prime numbers less than or equal to `n = 20` which are consecutive prime sums are:

*   `2 = 2`

*   `5 = 2 + 3`

*   `17 = 2 + 3 + 5 + 7`


The largest is 17, so it is the answer.

**Example 2:**

**Input:** n = 2

**Output:** 2

**Explanation:**

The only consecutive prime sum less than or equal to 2 is 2 itself.

**Constraints:**

*   <code>1 <= n <= 5 * 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    static int[] ppr = {
        2, 5, 17, 41, 197, 281, 7699, 8893, 22039, 24133, 25237, 28697, 32353, 37561, 38921, 43201,
        44683, 55837, 61027, 66463, 70241, 86453, 102001, 109147, 116533, 119069, 121631, 129419,
        132059, 263171, 287137, 325019, 329401, 333821, 338279, 342761, 360979, 379667, 393961,
        398771
    };

    public int largestPrime(int n) {
        int i = 0;
        int j = ppr.length - 1;
        int r = 0;
        while (i <= j) {
            int m = (i + j) >> 1;
            if (ppr[m] <= n) {
                r = ppr[m];
                i = m + 1;
            } else {
                j = m - 1;
            }
        }
        return r;
    }
}
```