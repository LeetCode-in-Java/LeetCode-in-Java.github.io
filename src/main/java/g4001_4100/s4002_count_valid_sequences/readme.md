[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 4002\. Count Valid Sequences

Medium

You are given two **positive** integers `n` and `k`.

A **valid sequence** is a sequence of `k` positive integers such that:

*   The **sum** of all integers in the sequence is equal to `n`.
*   The **product** of all integers in the sequence is **even**.

Return the number of valid sequences. Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

Two sequences are considered **different** if they differ at any index. For example, `[1, 1, 2]` and `[1, 2, 1]` are considered different sequences.

**Example 1:**

**Input:** n = 5, k = 3

**Output:** 3

**Explanation:**

The sequences of length `k = 3` whose sum is 5 are:

| Sequence | Product | Parity |
| --- | --- | --- |
| `[1, 1, 3]` | `1 * 1 * 3 = 3` | Odd |
| `[1, 2, 2]` | `1 * 2 * 2 = 4` | Even |
| `[2, 1, 2]` | `2 * 1 * 2 = 4` | Even |
| `[2, 2, 1]` | `2 * 2 * 1 = 4` | Even |
| `[1, 3, 1]` | `1 * 3 * 1 = 3` | Odd |
| `[3, 1, 1]` | `3 * 1 * 1 = 3` | Odd |

There are 3 sequences with an even product, thus the answer is 3.

**Example 2:**

**Input:** n = 3, k = 2

**Output:** 2

**Explanation:**

The sequences of length `k = 2` whose sum is 3 are:

| Sequence | Product | Parity |
| --- | --- | --- |
| `[1, 2]` | `1 * 2 = 2` | Even |
| `[2, 1]` | `2 * 1 = 2` | Even |

There are 2 sequences with an even product, thus the answer is 2.

**Example 3:**

**Input:** n = 5, k = 5

**Output:** 0

**Explanation:**

The only possible sequence of length `k = 5` whose sum is 5 is `[1, 1, 1, 1, 1]`, which has an odd product. Thus, the answer is 0.

**Constraints:**

*   <code>1 <= n <= 5 * 10<sup>5</sup></code>
*   `1 <= k <= n`

## Solution

```java
public class Solution {
    private static final int MOD = 1000000007;

    private long power(long b, long e) {
        long r = 1;
        for (; e > 0; e >>= 1, b = b * b % MOD) {
            if ((e & 1) == 1) {
                r = r * b % MOD;
            }
        }
        return r;
    }

    private long comb(int n, int k) {
        if (k < 0 || k > n) {
            return 0;
        }
        k = Math.min(k, n - k);
        long res = 1;
        long den = 1;
        for (int i = 1; i <= k; ++i) {
            res = res * (n - i + 1) % MOD;
            den = den * i % MOD;
        }
        return res * power(den, MOD - 2L) % MOD;
    }

    public int countValidSequences(int n, int k) {
        long res = comb(n - 1, k - 1);
        if (n % 2 == k % 2) {
            res = (res - comb((n + k) / 2 - 1, k - 1) + MOD) % MOD;
        }
        return (int) res;
    }
}
```