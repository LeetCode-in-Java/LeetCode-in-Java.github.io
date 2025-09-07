[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3671\. Sum of Beautiful Subsequences

Hard

You are given an integer array `nums` of length `n`.

For every **positive** integer `g`, we define the **beauty** of `g` as the **product** of `g` and the number of **strictly increasing** **subsequences** of `nums` whose greatest common divisor (GCD) is exactly `g`.

Return the **sum** of **beauty** values for all positive integers `g`.

Since the answer could be very large, return it modulo <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** 10

**Explanation:**

All strictly increasing subsequences and their GCDs are:

| Subsequence | GCD |
|-------------|-----|
| [1]         | 1   |
| [2]         | 2   |
| [3]         | 3   |
| [1,2]       | 1   |
| [1,3]       | 1   |
| [2,3]       | 1   |
| [1,2,3]     | 1   |

Calculating beauty for each GCD:

| GCD | Count of subsequences | Beauty (GCD × Count)  |
|-----|------------------------|----------------------|
| 1   | 5                      | 1 × 5 = 5            |
| 2   | 1                      | 2 × 1 = 2            |
| 3   | 1                      | 3 × 1 = 3            |

Total beauty is `5 + 2 + 3 = 10`.

**Example 2:**

**Input:** nums = [4,6]

**Output:** 12

**Explanation:**

All strictly increasing subsequences and their GCDs are:

| Subsequence | GCD |
|-------------|-----|
| [4]         | 4   |
| [6]         | 6   |
| [4,6]       | 2   |

Calculating beauty for each GCD:

| GCD | Count of subsequences | Beauty (GCD × Count) |
|-----|------------------------|----------------------|
| 2   | 1                      | 2 × 1 = 2            |
| 4   | 1                      | 4 × 1 = 4            |
| 6   | 1                      | 6 × 1 = 6            |

Total beauty is `2 + 4 + 6 = 12`.

**Constraints:**

*   <code>1 <= n == nums.length <= 10<sup>4</sup></code>
*   <code>1 <= nums[i] <= 7 * 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    private static final int MOD = 1000000007;

    public int totalBeauty(int[] nums) {
        int maxV = 0;
        for (int v : nums) {
            if (v > maxV) {
                maxV = v;
            }
        }
        // index by g
        Fenwick[] fenwicks = new Fenwick[maxV + 1];
        // FDiv[g] = # inc subseq with all elements multiple of g
        long[] fDiv = new long[maxV + 1];
        // temp buffer for divisors (max divisors of any number <= ~128 for this constraint)
        int[] divisors = new int[256];
        // Left-to-right DP restricted to multiples of each divisor g
        for (int x : nums) {
            int cnt = 0;
            int r = (int) Math.sqrt(x);
            for (int d = 1; d <= r; d++) {
                if (x % d == 0) {
                    divisors[cnt++] = d;
                    int d2 = x / d;
                    if (d2 != d) {
                        divisors[cnt++] = d2;
                    }
                }
            }
            for (int i = 0; i < cnt; i++) {
                int g = divisors[i];
                // coordinate in [1..maxV/g] for this g
                int idxQ = x / g;
                Fenwick fw = fenwicks[g];
                if (fw == null) {
                    // size needs to be >= max index (maxV/g). Use +2 for safety and 1-based
                    // indexing.
                    fw = new Fenwick(maxV / g + 2);
                    fenwicks[g] = fw;
                }
                long dp = 1 + fw.query(idxQ - 1);
                if (dp >= MOD) {
                    dp -= MOD;
                }
                fw.add(idxQ, dp);
                fDiv[g] += dp;
                if (fDiv[g] >= MOD) {
                    fDiv[g] -= MOD;
                }
            }
        }
        // Inclusion–exclusion to get exact gcd counts
        long[] exact = new long[maxV + 1];
        for (int g = maxV; g >= 1; g--) {
            long s = fDiv[g];
            for (int m = g + g; m <= maxV; m += g) {
                s -= exact[m];
                if (s < 0) {
                    s += MOD;
                }
            }
            exact[g] = s;
        }
        long ans = 0;
        for (int g = 1; g <= maxV; g++) {
            if (exact[g] != 0) {
                ans += exact[g] * g % MOD;
                if (ans >= MOD) {
                    ans -= MOD;
                }
            }
        }
        return (int) ans;
    }

    private static final class Fenwick {
        private final long[] tree;

        Fenwick(int size) {
            this.tree = new long[size];
        }

        void add(int indexOneBased, long delta) {
            for (int i = indexOneBased; i < tree.length; i += i & -i) {
                long v = tree[i] + delta;
                if (v >= MOD) {
                    v -= MOD;
                }
                tree[i] = v;
            }
        }

        long query(int indexOneBased) {
            long sum = 0;
            for (int i = indexOneBased; i > 0; i -= i & -i) {
                sum += tree[i];
                if (sum >= MOD) {
                    sum -= MOD;
                }
            }
            return sum;
        }
    }
}
```