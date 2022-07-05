[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1866\. Number of Ways to Rearrange Sticks With K Sticks Visible

Hard

There are `n` uniquely-sized sticks whose lengths are integers from `1` to `n`. You want to arrange the sticks such that **exactly** `k` sticks are **visible** from the left. A stick is **visible** from the left if there are no **longer** sticks to the **left** of it.

*   For example, if the sticks are arranged `[1,3,2,5,4]`, then the sticks with lengths `1`, `3`, and `5` are visible from the left.

Given `n` and `k`, return _the **number** of such arrangements_. Since the answer may be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** n = 3, k = 2

**Output:** 3

**Explanation:** [1,3,2], [2,3,1], and [2,1,3] are the only arrangements such that exactly 2 sticks are visible. The visible sticks are underlined.

**Example 2:**

**Input:** n = 5, k = 5

**Output:** 1

**Explanation:** [1,2,3,4,5] is the only arrangement such that all 5 sticks are visible. The visible sticks are underlined.

**Example 3:**

**Input:** n = 20, k = 11

**Output:** 647427950

**Explanation:** There are 647427950 (mod 10<sup>9</sup> \+ 7) ways to rearrange the sticks such that exactly 11 sticks are visible.

**Constraints:**

*   `1 <= n <= 1000`
*   `1 <= k <= n`

## Solution

```java
import java.util.Arrays;

public class Solution {
    private static final int MOD = 1_000_000_007;

    public int rearrangeSticks(int n, int k) {
        if (k > n || k < 1) {
            return 0;
        }
        if (k == n) {
            return 1;
        }
        long[] dp = new long[k + 1];
        Arrays.fill(dp, 1);
        for (int i = 1; i + k <= n; i++) {
            long[] dp2 = new long[k + 1];
            for (int j = 1; j <= k; j++) {
                dp2[j] = (dp2[j - 1] + (i + j - 1) * dp[j]) % MOD;
            }
            dp = dp2;
        }
        return (int) dp[k];
    }
}
```