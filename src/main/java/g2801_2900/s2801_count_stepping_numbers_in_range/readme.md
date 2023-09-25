[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2801\. Count Stepping Numbers in Range

Hard

Given two positive integers `low` and `high` represented as strings, find the count of **stepping numbers** in the inclusive range `[low, high]`.

A **stepping number** is an integer such that all of its adjacent digits have an absolute difference of **exactly** `1`.

Return _an integer denoting the count of stepping numbers in the inclusive range_ `[low, high]`_._

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Note:** A stepping number should not have a leading zero.

**Example 1:**

**Input:** low = "1", high = "11"

**Output:** 10

**Explanation:** The stepping numbers in the range [1,11] are 1, 2, 3, 4, 5, 6, 7, 8, 9 and 10. There are a total of 10 stepping numbers in the range. Hence, the output is 10.

**Example 2:**

**Input:** low = "90", high = "101"

**Output:** 2

**Explanation:** The stepping numbers in the range [90,101] are 98 and 101. There are a total of 2 stepping numbers in the range. Hence, the output is 2.

**Constraints:**

*   <code>1 <= int(low) <= int(high) < 10<sup>100</sup></code>
*   `1 <= low.length, high.length <= 100`
*   `low` and `high` consist of only digits.
*   `low` and `high` don't have any leading zeros.

## Solution

```java
public class Solution {
    private static final int MOD = (int) (1e9 + 7);
    private Integer[][][][] dp;

    public int countSteppingNumbers(String low, String high) {
        dp = new Integer[low.length() + 1][10][2][2];
        int count1 = solve(low, 0, 0, 1, 1);
        dp = new Integer[high.length() + 1][10][2][2];
        int count2 = solve(high, 0, 0, 1, 1);
        return (count2 - count1 + isStep(low) + MOD) % MOD;
    }

    private int solve(String s, int i, int prevDigit, int hasBound, int curIsZero) {
        if (i >= s.length()) {
            if (curIsZero == 1) {
                return 0;
            }
            return 1;
        }
        if (dp[i][prevDigit][hasBound][curIsZero] != null) {
            return dp[i][prevDigit][hasBound][curIsZero];
        }
        int count = 0;
        int limit = 9;
        if (hasBound == 1) {
            limit = s.charAt(i) - '0';
        }
        for (int digit = 0; digit <= limit; digit++) {
            int nextIsZero = (curIsZero == 1 && digit == 0) ? 1 : 0;
            int nextHasBound = (hasBound == 1 && digit == limit) ? 1 : 0;
            if (curIsZero == 1 || Math.abs(digit - prevDigit) == 1) {
                count = (count + solve(s, i + 1, digit, nextHasBound, nextIsZero)) % MOD;
            }
        }

        dp[i][prevDigit][hasBound][curIsZero] = count;
        return dp[i][prevDigit][hasBound][curIsZero];
    }

    private int isStep(String s) {
        boolean isValid = true;
        for (int i = 0; i < s.length() - 1; i++) {
            if (Math.abs(s.charAt(i + 1) - s.charAt(i)) != 1) {
                isValid = false;
                break;
            }
        }
        if (isValid) {
            return 1;
        }
        return 0;
    }
}
```