[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3405\. Count the Number of Arrays with K Matching Adjacent Elements

Hard

You are given three integers `n`, `m`, `k`. A **good array** `arr` of size `n` is defined as follows:

*   Each element in `arr` is in the **inclusive** range `[1, m]`.
*   _Exactly_ `k` indices `i` (where `1 <= i < n`) satisfy the condition `arr[i - 1] == arr[i]`.

Return the number of **good arrays** that can be formed.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** n = 3, m = 2, k = 1

**Output:** 4

**Explanation:**

*   There are 4 good arrays. They are `[1, 1, 2]`, `[1, 2, 2]`, `[2, 1, 1]` and `[2, 2, 1]`.
*   Hence, the answer is 4.

**Example 2:**

**Input:** n = 4, m = 2, k = 2

**Output:** 6

**Explanation:**

*   The good arrays are `[1, 1, 1, 2]`, `[1, 1, 2, 2]`, `[1, 2, 2, 2]`, `[2, 1, 1, 1]`, `[2, 2, 1, 1]` and `[2, 2, 2, 1]`.
*   Hence, the answer is 6.

**Example 3:**

**Input:** n = 5, m = 2, k = 0

**Output:** 2

**Explanation:**

*   The good arrays are `[1, 2, 1, 2, 1]` and `[2, 1, 2, 1, 2]`. Hence, the answer is 2.

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= m <= 10<sup>5</sup></code>
*   `0 <= k <= n - 1`

## Solution

```java
public class Solution {
    private static final int MOD = (int) (1e9 + 7);

    public int countGoodArrays(int n, int m, int k) {
        long[] f = new long[n + 1];
        f[0] = 1;
        f[1] = 1;
        for (int i = 2; i < f.length; i++) {
            f[i] = (f[i - 1] * i % MOD);
        }
        long ans = comb(n - 1, k, f);
        ans = ans * m % MOD;
        ans = ans * ex(m - 1L, n - k - 1L) % MOD;
        return (int) ans;
    }

    private long ex(long b, long e) {
        long ans = 1;
        while (e > 0) {
            if (e % 2 == 1) {
                ans = (ans * b) % MOD;
            }
            b = (b * b) % MOD;
            e = e >> 1;
        }
        return ans;
    }

    private long comb(int n, int r, long[] f) {
        return f[n] * (ff(f[r])) % MOD * ff(f[n - r]) % MOD;
    }

    private long ff(long x) {
        return ex(x, MOD - 2L);
    }
}
```