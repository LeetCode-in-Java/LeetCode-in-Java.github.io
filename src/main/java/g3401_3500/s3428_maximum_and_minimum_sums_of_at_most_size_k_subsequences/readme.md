[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3428\. Maximum and Minimum Sums of at Most Size K Subsequences

Medium

You are given an integer array `nums` and a positive integer `k`. Return the sum of the **maximum** and **minimum** elements of all

**subsequences**

of `nums` with **at most** `k` elements.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [1,2,3], k = 2

**Output:** 24

**Explanation:**

The subsequences of `nums` with at most 2 elements are:

| **Subsequence** | Minimum | Maximum | Sum  |
|-----------------|---------|---------|------|
| `[1]`           | 1       | 1       | 2    |
| `[2]`           | 2       | 2       | 4    |
| `[3]`           | 3       | 3       | 6    |
| `[1, 2]`        | 1       | 2       | 3    |
| `[1, 3]`        | 1       | 3       | 4    |
| `[2, 3]`        | 2       | 3       | 5    |
| **Final Total** |         |         | 24   |

The output would be 24.

**Example 2:**

**Input:** nums = [5,0,6], k = 1

**Output:** 22

**Explanation:**

For subsequences with exactly 1 element, the minimum and maximum values are the element itself. Therefore, the total is `5 + 5 + 0 + 0 + 6 + 6 = 22`.

**Example 3:**

**Input:** nums = [1,1,1], k = 2

**Output:** 12

**Explanation:**

The subsequences `[1, 1]` and `[1]` each appear 3 times. For all of them, the minimum and maximum are both 1. Thus, the total is 12.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= k <= min(70, nums.length)`

## Solution

```java
import java.util.Arrays;

public class Solution {
    private static final int MOD = (int) 1e9 + 7;
    private long[] fact;
    private long[] invFact;

    public int minMaxSums(int[] nums, int k) {
        int n = nums.length;
        Arrays.sort(nums);
        if (fact == null || fact.length < n + 1) {
            buildFactorials(n);
        }
        long[] sum = new long[n + 1];
        sum[0] = 1;
        for (int i = 0; i < n; i++) {
            long val = (2L * sum[i]) % MOD;
            if (i + 1 >= k) {
                val = (val - comb(i, k - 1) + MOD) % MOD;
            }
            sum[i + 1] = val;
        }
        long res = 0;
        for (int i = 0; i < n; i++) {
            long add = (sum[i] + sum[n - 1 - i]) % MOD;
            res = (res + (nums[i] % MOD) * add) % MOD;
        }
        return (int) res;
    }

    private void buildFactorials(int n) {
        fact = new long[n + 1];
        invFact = new long[n + 1];
        fact[0] = 1;
        for (int i = 1; i <= n; i++) {
            fact[i] = (fact[i - 1] * i) % MOD;
        }
        invFact[n] = pow(fact[n], MOD - 2);
        for (int i = n - 1; i >= 0; i--) {
            invFact[i] = (invFact[i + 1] * (i + 1)) % MOD;
        }
    }

    private long comb(int n, int r) {
        return fact[n] * invFact[r] % MOD * invFact[n - r] % MOD;
    }

    private long pow(long base, int exp) {
        long ans = 1L;
        while (exp > 0) {
            if ((exp & 1) == 1) {
                ans = (ans * base) % MOD;
            }
            base = (base * base) % MOD;
            exp >>= 1;
        }
        return ans;
    }
}
```