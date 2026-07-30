[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3869\. Count Fancy Numbers in a Range

Hard

You are given two integers `l` and `r`.

An integer is called **good** if its digits form a **strictly monotone** sequence, meaning the digits are **strictly increasing** or **strictly decreasing**. All single-digit integers are considered good.

An integer is called **fancy** if it is good, or if the **sum of its digits** is good.

Return an integer representing the number of fancy integers in the range `[l, r]` (inclusive).

A sequence is said to be **strictly increasing** if each element is **strictly greater** than its previous one (if exists).

A sequence is said to be **strictly decreasing** if each element is **strictly less** than its previous one (if exists).

**Example 1:**

**Input:** l = 8, r = 10

**Output:** 3

**Explanation:**

*   8 and 9 are single-digit integers, so they are good and therefore fancy.
*   10 has digits `[1, 0]`, which form a strictly decreasing sequence, so 10 is good and thus fancy.

Therefore, the answer is 3.

**Example 2:**

**Input:** l = 12340, r = 12341

**Output:** 1

**Explanation:**

*   12340
    *   12340 is not good because `[1, 2, 3, 4, 0]` is not strictly monotone.
    *   The digit sum is `1 + 2 + 3 + 4 + 0 = 10`.
    *   10 is good as it has digits `[1, 0]`, which is strictly decreasing. Therefore, 12340 is fancy.
*   12341
    *   12341 is not good because `[1, 2, 3, 4, 1]` is not strictly monotone.
    *   The digit sum is `1 + 2 + 3 + 4 + 1 = 11`.
    *   11 is not good as it has digits `[1, 1]`, which is not strictly monotone. Therefore, 12341 is not fancy.

Therefore, the answer is 1.

**Example 3:**

**Input:** l = 123456788, r = 123456788

**Output:** 0

**Explanation:**

*   123456788 is not good because its digits are not strictly monotone.
*   The digit sum is `1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 8 = 44`.
*   44 is not good as it has digits `[4, 4]`, which is not strictly monotone. Therefore, 123456788 is not fancy.

Therefore, the answer is 0.

**Constraints:**

*   <code>1 <= l <= r <= 10<sup>15</sup></code>

## Solution

```java
import java.util.Arrays;
import java.util.HashSet;
import java.util.Set;

public class Solution {
    private char[] low;
    private char[] high;
    private long[][][][] dp;
    private boolean[] good;

    public long countFancy(long l, long r) {
        initBounds(l, r);
        initGood();
        initDp();
        long ans = dfs(0, 0, 1, 1);
        Set<Long> seen = new HashSet<>();
        addIncreasingNumbers(seen, l, r);
        addDecreasingNumbers(seen, l, r);
        return ans + seen.size();
    }

    private void initBounds(long l, long r) {
        String left = Long.toString(l);
        String right = Long.toString(r);
        int n = right.length();
        left = "0".repeat(n - left.length()) + left;
        low = left.toCharArray();
        high = right.toCharArray();
    }

    private void initGood() {
        good = new boolean[136];
        for (int i = 1; i <= 135; i++) {
            good[i] = isGood(i);
        }
    }

    private void initDp() {
        dp = new long[high.length + 1][136][2][2];
        for (long[][][] a : dp) {
            for (long[][] b : a) {
                for (long[] c : b) {
                    Arrays.fill(c, -1);
                }
            }
        }
    }

    private void addIncreasingNumbers(Set<Long> seen, long l, long r) {
        for (int mask = 1; mask < (1 << 9); mask++) {
            long x = 0;
            int sum = 0;
            for (int i = 0; i < 9; i++) {
                if ((mask & (1 << i)) != 0) {
                    x = x * 10 + i + 1;
                    sum += i + 1;
                }
            }
            if (x >= l && x <= r && !good[sum]) {
                seen.add(x);
            }
        }
    }

    private void addDecreasingNumbers(Set<Long> seen, long l, long r) {
        for (int mask = 1; mask < (1 << 10); mask++) {
            long x = 0;
            int sum = 0;
            boolean started = false;
            for (int i = 0; i < 10; i++) {
                if ((mask & (1 << i)) != 0) {
                    int d = 9 - i;
                    if (started || d != 0) {
                        started = true;
                        x = x * 10 + d;
                        sum += d;
                    }
                }
            }
            if (started && x >= l && x <= r && !good[sum]) {
                seen.add(x);
            }
        }
    }

    private long dfs(int pos, int sum, int tl, int tr) {
        if (pos == high.length) {
            return good[sum] ? 1 : 0;
        }
        if (dp[pos][sum][tl][tr] != -1) {
            return dp[pos][sum][tl][tr];
        }
        int lo = tl == 1 ? low[pos] - '0' : 0;
        int hi = tr == 1 ? high[pos] - '0' : 9;
        long res = 0;
        for (int d = lo; d <= hi; d++) {
            res += dfs(pos + 1, sum + d, nextTight(tl, d, lo), nextTight(tr, d, hi));
        }
        dp[pos][sum][tl][tr] = res;
        return res;
    }

    private int nextTight(int tight, int digit, int limit) {
        return tight == 1 && digit == limit ? 1 : 0;
    }

    private boolean isGood(int x) {
        if (x < 10) {
            return true;
        }
        char[] s = Integer.toString(x).toCharArray();
        boolean inc = true;
        boolean dec = true;
        for (int i = 1; i < s.length; i++) {
            if (s[i] <= s[i - 1]) {
                inc = false;
            }
            if (s[i] >= s[i - 1]) {
                dec = false;
            }
        }
        return inc || dec;
    }
}
```