[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3539\. Find Sum of Array Product of Magical Sequences

Hard

You are given two integers, `m` and `k`, and an integer array `nums`.

A sequence of integers `seq` is called **magical** if:

*   `seq` has a size of `m`.
*   `0 <= seq[i] < nums.length`
*   The **binary representation** of <code>2<sup>seq[0]</sup> + 2<sup>seq[1]</sup> + ... + 2<sup>seq[m - 1]</sup></code> has `k` **set bits**.

The **array product** of this sequence is defined as `prod(seq) = (nums[seq[0]] * nums[seq[1]] * ... * nums[seq[m - 1]])`.

Return the **sum** of the **array products** for all valid **magical** sequences.

Since the answer may be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

A **set bit** refers to a bit in the binary representation of a number that has a value of 1.

**Example 1:**

**Input:** m = 5, k = 5, nums = [1,10,100,10000,1000000]

**Output:** 991600007

**Explanation:**

All permutations of `[0, 1, 2, 3, 4]` are magical sequences, each with an array product of 10<sup>13</sup>.

**Example 2:**

**Input:** m = 2, k = 2, nums = [5,4,3,2,1]

**Output:** 170

**Explanation:**

The magical sequences are `[0, 1]`, `[0, 2]`, `[0, 3]`, `[0, 4]`, `[1, 0]`, `[1, 2]`, `[1, 3]`, `[1, 4]`, `[2, 0]`, `[2, 1]`, `[2, 3]`, `[2, 4]`, `[3, 0]`, `[3, 1]`, `[3, 2]`, `[3, 4]`, `[4, 0]`, `[4, 1]`, `[4, 2]`, and `[4, 3]`.

**Example 3:**

**Input:** m = 1, k = 1, nums = [28]

**Output:** 28

**Explanation:**

The only magical sequence is `[0]`.

**Constraints:**

*   `1 <= k <= m <= 30`
*   `1 <= nums.length <= 50`
*   <code>1 <= nums[i] <= 10<sup>8</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    private static final int MOD = 1_000_000_007;
    private static final int[][] C = precomputeBinom(31);
    private static final int[] P = precomputePop(31);

    public int magicalSum(int m, int k, int[] nums) {
        int n = nums.length;
        long[][] pow = new long[n][m + 1];
        for (int j = 0; j < n; j++) {
            pow[j][0] = 1L;
            for (int c = 1; c <= m; c++) {
                pow[j][c] = pow[j][c - 1] * nums[j] % MOD;
            }
        }
        long[][][] dp = new long[m + 1][k + 1][m + 1];
        long[][][] next = new long[m + 1][k + 1][m + 1];
        dp[0][0][0] = 1L;
        for (int i = 0; i < n; i++) {
            for (int t = 0; t <= m; t++) {
                for (int o = 0; o <= k; o++) {
                    Arrays.fill(next[t][o], 0L);
                }
            }
            for (int t = 0; t <= m; t++) {
                for (int o = 0; o <= k; o++) {
                    for (int c = 0; c <= m; c++) {
                        if (dp[t][o][c] == 0) {
                            continue;
                        }
                        for (int cc = 0; cc <= m - t; cc++) {
                            int total = c + cc;
                            if (o + (total & 1) > k) {
                                continue;
                            }
                            next[t + cc][o + (total & 1)][total >>> 1] =
                                    (next[t + cc][o + (total & 1)][total >>> 1]
                                                    + dp[t][o][c]
                                                            * C[m - t][cc]
                                                            % MOD
                                                            * pow[i][cc]
                                                            % MOD)
                                            % MOD;
                        }
                    }
                }
            }
            long[][][] tmp = dp;
            dp = next;
            next = tmp;
        }
        long res = 0;
        for (int o = 0; o <= k; o++) {
            for (int c = 0; c <= m; c++) {
                if (o + P[c] == k) {
                    res = (res + dp[m][o][c]) % MOD;
                }
            }
        }
        return (int) res;
    }

    private static int[][] precomputeBinom(int max) {
        int[][] res = new int[max][max];
        for (int i = 0; i < max; i++) {
            res[i][0] = res[i][i] = 1;
            for (int j = 1; j < i; j++) {
                res[i][j] = (res[i - 1][j - 1] + res[i - 1][j]) % MOD;
            }
        }
        return res;
    }

    private static int[] precomputePop(int max) {
        int[] res = new int[max];
        for (int i = 1; i < max; i++) {
            res[i] = res[i >> 1] + (i & 1);
        }
        return res;
    }
}
```