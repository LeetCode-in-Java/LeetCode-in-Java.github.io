[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3892\. Minimum Operations to Achieve At Least K Peaks

Hard

You are given a circular integer array `nums` of length `n`.

An index `i` is a **peak** if its value is **strictly greater** than its neighbors:

*   The **previous** neighbor of `i` is `nums[i - 1]` if `i > 0`, otherwise `nums[n - 1]`.
*   The **next** neighbor of `i` is `nums[i + 1]` if `i < n - 1`, otherwise `nums[0]`.

You are allowed to perform the following operation **any** number of times:

*   Choose any index `i` and **increase** `nums[i]` by 1.

Return an integer denoting the **minimum** number of operations required to make the array contain **at least** `k` peaks. If it is impossible, return -1.

**Example 1:**

**Input:** nums = [2,1,2], k = 1

**Output:** 1

**Explanation:**

*   To achieve at least `k = 1` peak, we can increase `nums[2] = 2` to 3.
*   After this operation, `nums[2] = 3` is strictly greater than its neighbors `nums[0] = 2` and `nums[1] = 1`.
*   Therefore, the minimum number of operations required is 1.

**Example 2:**

**Input:** nums = [4,5,3,6], k = 2

**Output:** 0

**Explanation:**

*   The array already contains at least `k = 2` peaks with zero operations.
*   Index 1: `nums[1] = 5` is strictly greater than its neighbors `nums[0] = 4` and `nums[2] = 3`.
*   Index 3: `nums[3] = 6` is strictly greater than its neighbors `nums[2] = 3` and `nums[0] = 4`.
*   Therefore, the minimum number of operations required is 0.

**Example 3:**

**Input:** nums = [3,7,3], k = 2

**Output:** \-1

**Explanation:**

It is impossible to have at least `k = 2` peaks in this array. Therefore, the answer is -1.

**Constraints:**

*   `2 <= n == nums.length <= 5000`
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>
*   `0 <= k <= n`

## Solution

```java
import java.util.Arrays;

public class Solution {
    private static final long INF = Long.MAX_VALUE / 4;

    public int minOperations(int[] nums, int k) {
        int n = nums.length;
        if (k == 0) {
            return 0;
        }
        if (k > n / 2) {
            return -1;
        }
        long[] c = new long[n];
        for (int i = 0; i < n; i++) {
            int left = nums[(i - 1 + n) % n];
            int right = nums[(i + 1) % n];
            c[i] = Math.max(0L, (long) Math.max(left, right) + 1 - nums[i]);
        }
        long best = pathMin(c, 1, n - 1, k);
        long withZero = pathMin(c, 2, n - 2, k - 1);
        if (withZero < INF) {
            best = Math.min(best, c[0] + withZero);
        }
        return (int) best;
    }

    private long pathMin(long[] c, int lo, int hi, int need) {
        if (need == 0) {
            return 0;
        }
        int m = hi - lo + 1;
        if (m <= 0 || need > (m + 1) / 2) {
            return INF;
        }
        long[] prev2 = new long[need + 1];
        long[] prev1 = new long[need + 1];
        long[] cur = new long[need + 1];
        Arrays.fill(prev2, INF);
        prev2[0] = 0;
        Arrays.fill(prev1, INF);
        prev1[0] = 0;
        for (int i = 0; i < m; i++) {
            long w = c[lo + i];
            cur[0] = 0;
            for (int j = 1; j <= need; j++) {
                cur[j] = Math.min(prev1[j], prev2[j - 1] + w);
            }
            long[] t = prev2;
            prev2 = prev1;
            prev1 = cur;
            cur = t;
        }
        return prev1[need];
    }
}
```