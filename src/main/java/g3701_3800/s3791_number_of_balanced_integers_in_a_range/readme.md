[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3791\. Number of Balanced Integers in a Range

Hard

You are given two integers `low` and `high`.

An integer is called **balanced** if it satisfies **both** of the following conditions:

*   It contains **at least** two digits.
*   The **sum of digits at even positions** is equal to the **sum of digits at odd positions** (the leftmost digit has position 1).

Return an integer representing the number of balanced integers in the range `[low, high]` (both inclusive).

**Example 1:**

**Input:** low = 1, high = 100

**Output:** 9

**Explanation:**

The 9 balanced numbers between 1 and 100 are 11, 22, 33, 44, 55, 66, 77, 88, and 99.

**Example 2:**

**Input:** low = 120, high = 129

**Output:** 1

**Explanation:**

Only 121 is balanced because the sum of digits at even and odd positions are both 2.

**Example 3:**

**Input:** low = 1234, high = 1234

**Output:** 0

**Explanation:**

1234 is not balanced because the sum of digits at odd positions `(1 + 3 = 4)` does not equal the sum at even positions `(2 + 4 = 6)`.

**Constraints:**

*   <code>1 <= low <= high <= 10<sup>15</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    private long solve(String number, int idx, int diff, int tight, long[][][] dp) {
        if (idx == number.length()) {
            return diff == 0 ? 1 : 0;
        }
        if (dp[idx][diff + dp[0].length / 2][tight] != -1) {
            return dp[idx][diff + dp[0].length / 2][tight];
        }
        long ans = 0;
        int ub = tight == 1 ? number.charAt(idx) - '0' : 9;
        for (int i = 0; i <= ub; i++) {
            int newTight = (tight == 1 && i == ub) ? 1 : 0;
            if (idx % 2 == 0) {
                ans += solve(number, idx + 1, diff - i, newTight, dp);
            } else {
                ans += solve(number, idx + 1, diff + i, newTight, dp);
            }
        }
        dp[idx][diff + dp[0].length / 2][tight] = ans;
        return dp[idx][diff + dp[0].length / 2][tight];
    }

    private long solve(long num) {
        if (num == 0) {
            return 1;
        }
        String number = Long.toString(num);
        int len = number.length();
        long[][][] dp =
                len % 2 == 1 ? new long[len][(len + 1) * 9 + 1][2] : new long[len][len * 9 + 1][2];
        for (long[][] tbl : dp) {
            for (long[] row : tbl) {
                Arrays.fill(row, -1);
            }
        }
        return solve(number, 0, 0, 1, dp);
    }

    public long countBalanced(long low, long high) {
        return solve(high) - solve(low - 1);
    }
}
```