[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3130\. Find All Possible Stable Binary Arrays II

Hard

You are given 3 positive integers `zero`, `one`, and `limit`.

A binary array `arr` is called **stable** if:

*   The number of occurrences of 0 in `arr` is **exactly** `zero`.
*   The number of occurrences of 1 in `arr` is **exactly** `one`.
*   Each subarray of `arr` with a size greater than `limit` must contain **both** 0 and 1.

Return the _total_ number of **stable** binary arrays.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** zero = 1, one = 1, limit = 2

**Output:** 2

**Explanation:**

The two possible stable binary arrays are `[1,0]` and `[0,1]`.

**Example 2:**

**Input:** zero = 1, one = 2, limit = 1

**Output:** 1

**Explanation:**

The only possible stable binary array is `[1,0,1]`.

**Example 3:**

**Input:** zero = 3, one = 3, limit = 2

**Output:** 14

**Explanation:**

All the possible stable binary arrays are `[0,0,1,0,1,1]`, `[0,0,1,1,0,1]`, `[0,1,0,0,1,1]`, `[0,1,0,1,0,1]`, `[0,1,0,1,1,0]`, `[0,1,1,0,0,1]`, `[0,1,1,0,1,0]`, `[1,0,0,1,0,1]`, `[1,0,0,1,1,0]`, `[1,0,1,0,0,1]`, `[1,0,1,0,1,0]`, `[1,0,1,1,0,0]`, `[1,1,0,0,1,0]`, and `[1,1,0,1,0,0]`.

**Constraints:**

*   `1 <= zero, one, limit <= 1000`

## Solution

```java
public class Solution {
    private static final int MOD = (int) 1e9 + 7;
    private static final int N = 1000;
    private long[] factorial;
    private long[] reverse;

    public int numberOfStableArrays(int zero, int one, int limit) {
        if (factorial == null) {
            factorial = new long[N + 1];
            reverse = new long[N + 1];
            factorial[0] = 1;
            reverse[0] = 1;
            long x = 1;
            for (int i = 1; i <= N; ++i) {
                x = (x * i) % MOD;
                factorial[i] = (int) x;
                reverse[i] = getInverse(x, MOD);
            }
        }
        long ans = 0;
        long[] s = new long[one + 1];
        int n = Math.min(zero, one) + 1;
        for (int groups0 = (zero + limit - 1) / limit; groups0 <= Math.min(zero, n); ++groups0) {
            long s0 = calc(groups0, zero, limit);
            for (int groups1 = Math.max(groups0 - 1, (one + limit - 1) / limit);
                    groups1 <= Math.min(groups0 + 1, one);
                    ++groups1) {
                long s1;
                if (s[groups1] != 0) {
                    s1 = s[groups1];
                } else {
                    s1 = s[groups1] = calc(groups1, one, limit);
                }
                ans = (ans + s0 * s1 * (groups1 == groups0 ? 2 : 1)) % MOD;
            }
        }
        return (int) ((ans + MOD) % MOD);
    }

    long calc(int groups, int x, int limit) {
        long s = 0;
        int sign = 1;
        for (int k = 0; k * limit <= x - groups && k <= groups; k++) {
            s = (s + sign * comb(groups, k) * comb(x - k * limit - 1, groups - 1)) % MOD;
            sign *= -1;
        }
        return s;
    }

    public long comb(int n, int k) {
        return (factorial[n] * reverse[k] % MOD) * reverse[n - k] % MOD;
    }

    public long getInverse(long n, long mod) {
        long p = mod;
        long x = 1;
        long y = 0;
        while (p > 0) {
            long quotient = n / p;
            long remainder = n % p;
            long tempY = x - quotient * y;
            x = y;
            y = tempY;
            n = p;
            p = remainder;
        }
        return ((x % mod) + mod) % mod;
    }

    public long quickPower(long base, long power, long p) {
        long result = 1;
        while (power > 0) {
            if ((power & 1) == 1) {
                result = result * base % p;
            }
            power >>= 1;
            base = base * base % p;
        }
        return result;
    }
}
```