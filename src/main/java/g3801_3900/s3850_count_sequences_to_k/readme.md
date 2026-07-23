[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3850\. Count Sequences to K

Hard

You are given an integer array `nums`, and an integer `k`.

Start with an initial value `val = 1` and process `nums` from left to right. At each index `i`, you must choose **exactly one** of the following actions:

*   Multiply `val` by `nums[i]`.
*   Divide `val` by `nums[i]`.
*   Leave `val` unchanged.

After processing all elements, `val` is considered **equal** to `k` only if its final rational value **exactly** equals `k`.

Return the count of **distinct** sequences of choices that result in `val == k`.

**Note:** Division is rational (exact), not integer division. For example, `2 / 4 = 1 / 2`.

**Example 1:**

**Input:** nums = [2,3,2], k = 6

**Output:** 2

**Explanation:**

The following 2 distinct sequences of choices result in `val == k`:

| Sequence | Operation on `nums[0]` | Operation on `nums[1]` | Operation on `nums[2]` | Final `val` |
|----------|-------------------------|-------------------------|-------------------------|-------------|
| 1 | Multiply: `val = 1 * 2 = 2` | Multiply: `val = 2 * 3 = 6` | Leave `val` unchanged | 6 |
| 2 | Leave `val` unchanged | Multiply: `val = 1 * 3 = 3` | Multiply: `val = 3 * 2 = 6` | 6 |

**Example 2:**

**Input:** nums = [4,6,3], k = 2

**Output:** 2

**Explanation:**

The following 2 distinct sequences of choices result in `val == k`:

| Sequence | Operation on `nums[0]` | Operation on `nums[1]` | Operation on `nums[2]` | Final `val` |
|----------|-------------------------|-------------------------|-------------------------|-------------|
| 1 | Multiply: `val = 1 * 4 = 4` | Divide: `val = 4 / 6 = 2 / 3` | Multiply: `val = (2 / 3) * 3 = 2` | 2 |
| 2 | Leave `val` unchanged | Multiply: `val = 1 * 6 = 6` | Divide: `val = 6 / 3 = 2` | 2 |

**Example 3:**

**Input:** nums = [1,5], k = 1

**Output:** 3

**Explanation:**

The following 3 distinct sequences of choices result in `val == k`:

| Sequence | Operation on `nums[0]` | Operation on `nums[1]` | Final `val` |
|----------|-------------------------|-------------------------|-------------|
| 1 | Multiply: `val = 1 * 1 = 1` | Leave `val` unchanged | 1 |
| 2 | Divide: `val = 1 / 1 = 1` | Leave `val` unchanged | 1 |
| 3 | Leave `val` unchanged | Leave `val` unchanged | 1 |

**Constraints:**

*   `1 <= nums.length <= 19`
*   `1 <= nums[i] <= 6`
*   <code>1 <= k <= 10<sup>15</sup></code>

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {

    private Map<Double, Integer>[] dp;

    private int fun(int[] nums, int pos, double val, double k) {
        if (pos == nums.length) {
            if (Math.abs(val - k) <= 0.000000009) {
                return 1;
            }
            return 0;
        }
        if (dp[pos].containsKey(val)) {
            return dp[pos].get(val);
        }
        int ret = fun(nums, pos + 1, val, k);
        ret += fun(nums, pos + 1, val * nums[pos], k);
        ret += fun(nums, pos + 1, val / nums[pos], k);
        dp[pos].put(val, ret);
        return ret;
    }

    public int countSequences(int[] nums, long k) {
        dp = new HashMap[22];
        for (int i = 0; i < 22; i++) {
            dp[i] = new HashMap<>();
        }
        return fun(nums, 0, 1.0, 1.00 * k);
    }
}
```