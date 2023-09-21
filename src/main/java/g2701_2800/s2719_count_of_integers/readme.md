[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2719\. Count of Integers

Hard

You are given two numeric strings `num1` and `num2` and two integers `max_sum` and `min_sum`. We denote an integer `x` to be _good_ if:

*   `num1 <= x <= num2`
*   `min_sum <= digit_sum(x) <= max_sum`.

Return _the number of good integers_. Since the answer may be large, return it modulo <code>10<sup>9</sup> + 7</code>.

Note that `digit_sum(x)` denotes the sum of the digits of `x`.

**Example 1:**

**Input:** num1 = "1", num2 = "12", `min_sum` = 1, max\_sum = 8

**Output:** 11

**Explanation:** There are 11 integers whose sum of digits lies between 1 and 8 are 1,2,3,4,5,6,7,8,10,11, and 12. Thus, we return 11.

**Example 2:**

**Input:** num1 = "1", num2 = "5", `min_sum` = 1, max\_sum = 5

**Output:** 5

**Explanation:** The 5 integers whose sum of digits lies between 1 and 5 are 1,2,3,4, and 5. Thus, we return 5.

**Constraints:**

*   <code>1 <= num1 <= num2 <= 10<sup>22</sup></code>
*   `1 <= min_sum <= max_sum <= 400`

## Solution

```java
import java.util.Arrays;

public class Solution {
    private int[][][][] dp;
    private static final int MOD = (int) (1e9 + 7);

    private int countStrings(
            int i, boolean tight1, boolean tight2, int sum, String num1, String num2) {
        if (sum < 0) {
            return 0;
        }
        if (i == num2.length()) {
            return 1;
        }
        if (dp[i][tight1 ? 1 : 0][tight2 ? 1 : 0][sum] != -1) {
            return dp[i][tight1 ? 1 : 0][tight2 ? 1 : 0][sum];
        }
        int lo = tight1 ? (num1.charAt(i) - '0') : 0;
        int hi = tight2 ? (num2.charAt(i) - '0') : 9;
        int count = 0;
        for (int idx = lo; idx <= hi; idx++) {
            count =
                    (count % MOD
                                    + countStrings(
                                                    i + 1,
                                                    tight1 && (idx == lo),
                                                    tight2 && (idx == hi),
                                                    sum - idx,
                                                    num1,
                                                    num2)
                                            % MOD)
                            % MOD;
        }

        dp[i][tight1 ? 1 : 0][tight2 ? 1 : 0][sum] = count;
        return count;
    }

    public int count(String num1, String num2, int minSum, int maxSum) {
        int maxLength = num2.length();
        int minLength = num1.length();
        int leadingZeroes = maxLength - minLength;
        String num1extended = "0".repeat(leadingZeroes) + num1;
        dp = new int[maxLength][2][2][401];
        for (int i = 0; i < maxLength; i++) {
            for (int j = 0; j < 2; j++) {
                for (int k = 0; k < 2; k++) {
                    Arrays.fill(dp[i][j][k], -1);
                }
            }
        }
        int total = countStrings(0, true, true, maxSum, num1extended, num2);
        int unnecessary = countStrings(0, true, true, minSum - 1, num1extended, num2);
        int ans = (total - unnecessary) % MOD;
        if (ans < 0) {
            ans += MOD;
        }
        return ans;
    }
}
```