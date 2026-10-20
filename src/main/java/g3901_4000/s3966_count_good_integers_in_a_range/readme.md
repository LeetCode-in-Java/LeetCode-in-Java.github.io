[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3966\. Count Good Integers in a Range

Hard

You are given three integers `l`, `r` and `k`.

A number is considered **good** if the **absolute difference** between every pair of **adjacent** digits is **at most** `k`.

Return the number of **good** integers in the range `[l, r]` (inclusive).

The **absolute difference** between values `x` and `y` is defined as `abs(x - y)`.

**Example 1:**

**Input:** l = 10, r = 15, k = 1

**Output:** 3

**Explanation:**

*   The good integers in the range are 10, 11, and 12.
*   For 10, `abs(1 - 0) = 1`.
*   For 11, `abs(1 - 1) = 0`.
*   For 12, `abs(1 - 2) = 1`.
*   All these differences are at most `k = 1`. Thus, the answer is 3.

**Example 2:**

**Input:** l = 201, r = 204, k = 2

**Output:** 2

**Explanation:**

*   The good integers in the range are 201 and 202.
*   For 201, `abs(2 - 0) = 2` and `abs(0 - 1) = 1`.
*   For 202, `abs(2 - 0) = 2` and `abs(0 - 2) = 2`.
*   Thus, the answer is 2.

**Constraints:**

*   <code>10 <= l <= r <= 10<sup>15</sup></code>
*   `0 <= k <= 9`

## Solution

```java
public class Solution {
    private final Long[][][][] dp = new Long[16][2][10][10];
    private final int[] digits = new int[16];

    public long goodIntegers(long l, long r, int k) {
        return calc(r, k) - calc(l - 1, k);
    }

    private long calc(long num, int k) {
        int len = 0;
        while (num > 0) {
            digits[len++] = (int) (num % 10);
            num /= 10;
        }
        return dfs(len - 1, 0, 1, true, k);
    }

    private long dfs(int index, int pre, int isLeadingZero, boolean isMaxDigit, int k) {
        if (index == -1) {
            return 1;
        }
        if (!isMaxDigit && dp[index][isLeadingZero][pre][k] != null) {
            return dp[index][isLeadingZero][pre][k];
        }
        int maxDigit = isMaxDigit ? digits[index] : 9;
        long result = 0;
        for (int d = 0; d <= maxDigit; d++) {
            if (isLeadingZero == 1 || Math.abs(d - pre) <= k) {
                result +=
                        dfs(
                                index - 1,
                                d,
                                isLeadingZero == 1 && d == 0 ? 1 : 0,
                                isMaxDigit && d == maxDigit,
                                k);
            }
        }
        if (!isMaxDigit) {
            dp[index][isLeadingZero][pre][k] = result;
        }
        return result;
    }
}
```