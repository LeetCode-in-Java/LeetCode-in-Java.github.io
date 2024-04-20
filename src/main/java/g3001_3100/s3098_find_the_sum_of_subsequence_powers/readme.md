[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3098\. Find the Sum of Subsequence Powers

Hard

You are given an integer array `nums` of length `n`, and a **positive** integer `k`.

The **power** of a subsequence is defined as the **minimum** absolute difference between **any** two elements in the subsequence.

Return _the **sum** of **powers** of **all** subsequences of_ `nums` _which have length_ **_equal to_** `k`.

Since the answer may be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [1,2,3,4], k = 3

**Output:** 4

**Explanation:**

There are 4 subsequences in `nums` which have length 3: `[1,2,3]`, `[1,3,4]`, `[1,2,4]`, and `[2,3,4]`. The sum of powers is `|2 - 3| + |3 - 4| + |2 - 1| + |3 - 4| = 4`.

**Example 2:**

**Input:** nums = [2,2], k = 2

**Output:** 0

**Explanation:**

The only subsequence in `nums` which has length 2 is `[2,2]`. The sum of powers is `|2 - 2| = 0`.

**Example 3:**

**Input:** nums = [4,3,-1], k = 2

**Output:** 10

**Explanation:**

There are 3 subsequences in `nums` which have length 2: `[4,3]`, `[4,-1]`, and `[3,-1]`. The sum of powers is `|4 - 3| + |4 - (-1)| + |3 - (-1)| = 10`.

**Constraints:**

*   `2 <= n == nums.length <= 50`
*   <code>-10<sup>8</sup> <= nums[i] <= 10<sup>8</sup></code>
*   `2 <= k <= n`

## Solution

```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;

public class Solution {
    private static final int MOD = 1_000_000_007;
    private int len;

    private int dfs(int lastIdx, int k, int minDiff, Map<Long, Integer> dp, int[] nums) {
        if (k == 0) {
            return minDiff;
        }
        long key = (((long) minDiff) << 12) + ((long) lastIdx << 6) + k;
        if (dp.containsKey(key)) {
            return dp.get(key);
        }
        int res = 0;
        for (int i = lastIdx + 1; i <= len - k; i++) {
            res = (res + dfs(i, k - 1, Math.min(minDiff, nums[i] - nums[lastIdx]), dp, nums)) % MOD;
        }
        dp.put(key, res);
        return res;
    }

    public int sumOfPowers(int[] nums, int k) {
        len = nums.length;
        Arrays.sort(nums);
        Map<Long, Integer> dp = new HashMap<>();
        int res = 0;
        for (int i = 0; i <= len - k; i++) {
            res = (res + dfs(i, k - 1, nums[len - 1] - nums[0], dp, nums)) % MOD;
        }
        return res;
    }
}
```