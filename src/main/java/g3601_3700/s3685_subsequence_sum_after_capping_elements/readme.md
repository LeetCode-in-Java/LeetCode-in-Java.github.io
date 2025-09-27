[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3685\. Subsequence Sum After Capping Elements

Medium

You are given an integer array `nums` of size `n` and a positive integer `k`.

An array **capped** by value `x` is obtained by replacing every element `nums[i]` with `min(nums[i], x)`.

For each integer `x` from 1 to `n`, determine whether it is possible to choose a **subsequence** from the array capped by `x` such that the sum of the chosen elements is **exactly** `k`.

Return a **0-indexed** boolean array `answer` of size `n`, where `answer[i]` is `true` if it is possible when using `x = i + 1`, and `false` otherwise.

**Example 1:**

**Input:** nums = [4,3,2,4], k = 5

**Output:** [false,false,true,true]

**Explanation:**

*   For `x = 1`, the capped array is `[1, 1, 1, 1]`. Possible sums are `1, 2, 3, 4`, so it is impossible to form a sum of `5`.
*   For `x = 2`, the capped array is `[2, 2, 2, 2]`. Possible sums are `2, 4, 6, 8`, so it is impossible to form a sum of `5`.
*   For `x = 3`, the capped array is `[3, 3, 2, 3]`. A subsequence `[2, 3]` sums to `5`, so it is possible.
*   For `x = 4`, the capped array is `[4, 3, 2, 4]`. A subsequence `[3, 2]` sums to `5`, so it is possible.

**Example 2:**

**Input:** nums = [1,2,3,4,5], k = 3

**Output:** [true,true,true,true,true]

**Explanation:**

For every value of `x`, it is always possible to select a subsequence from the capped array that sums exactly to `3`.

**Constraints:**

*   `1 <= n == nums.length <= 4000`
*   `1 <= nums[i] <= n`
*   `1 <= k <= 4000`

## Solution

```java
public class Solution {
    public boolean[] subsequenceSumAfterCapping(int[] nums, int k) {
        int n = nums.length;
        boolean[] answer = new boolean[n];
        int[] freq = new int[n + 2];
        for (int v : nums) {
            if (v <= n) {
                freq[v]++;
            }
        }
        int[] cntGe = new int[n + 2];
        cntGe[n] = freq[n];
        for (int x = n - 1; x >= 1; x--) {
            cntGe[x] = cntGe[x + 1] + freq[x];
        }
        boolean[] dp = new boolean[k + 1];
        dp[0] = true;
        for (int x = 1; x <= n; x++) {
            int cnt = cntGe[x];
            boolean ok = false;
            int maxM = cnt;
            int limit = k / x;
            if (maxM > limit) {
                maxM = limit;
            }
            for (int m = 0; m <= maxM; m++) {
                int rem = k - m * x;
                if (rem >= 0 && dp[rem]) {
                    ok = true;
                    break;
                }
            }
            answer[x - 1] = ok;
            int c = freq[x];
            if (c == 0) {
                continue;
            }
            int power = 1;
            while (c > 0) {
                int take = Math.min(power, c);
                int weight = take * x;
                for (int s = k; s >= weight; s--) {
                    if (!dp[s] && dp[s - weight]) {
                        dp[s] = true;
                    }
                }
                c -= take;
                power <<= 1;
            }
        }
        return answer;
    }
}
```