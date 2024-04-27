[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3116\. Kth Smallest Amount With Single Denomination Combination

Hard

You are given an integer array `coins` representing coins of different denominations and an integer `k`.

You have an infinite number of coins of each denomination. However, you are **not allowed** to combine coins of different denominations.

Return the <code>k<sup>th</sup></code> **smallest** amount that can be made using these coins.

**Example 1:**

**Input:** coins = [3,6,9], k = 3

**Output:** 9

**Explanation:** The given coins can make the following amounts:   

 Coin 3 produces multiples of 3: 3, 6, 9, 12, 15, etc.   

 Coin 6 produces multiples of 6: 6, 12, 18, 24, etc.   

 Coin 9 produces multiples of 9: 9, 18, 27, 36, etc.   

 All of the coins combined produce: 3, 6, <ins>**9**</ins>, 12, 15, etc.

**Example 2:**

**Input:** coins = [5,2], k = 7

**Output:** 12

**Explanation:** The given coins can make the following amounts:   

 Coin 5 produces multiples of 5: 5, 10, 15, 20, etc.   

 Coin 2 produces multiples of 2: 2, 4, 6, 8, 10, 12, etc.   

 All of the coins combined produce: 2, 4, 5, 6, 8, 10, <ins>**12**</ins>, 14, 15, etc.

**Constraints:**

*   `1 <= coins.length <= 15`
*   `1 <= coins[i] <= 25`
*   <code>1 <= k <= 2 * 10<sup>9</sup></code>
*   `coins` contains pairwise distinct integers.

## Solution

```java
import java.util.Arrays;

@SuppressWarnings("java:S1119")
public class Solution {
    public long findKthSmallest(int[] coins, int k) {
        int minC = Integer.MAX_VALUE;
        for (int c : coins) {
            minC = Math.min(minC, c);
        }
        long[] cc = coins(coins);
        long max = (long) minC * k;
        long min = max / coins.length;
        while (min < max) {
            long mid = (min + max) / 2;
            final long cnt = count(cc, mid);
            if (cnt > k) {
                max = mid - 1;
            } else if (cnt < k) {
                min = mid + 1;
            } else {
                max = mid;
            }
        }
        return min;
    }

    private long count(long[] coins, long v) {
        long r = 0;
        for (long c : coins) {
            r += v / c;
        }
        return r;
    }

    private long[] coins(int[] coins) {
        Arrays.sort(coins);
        int len = 1;
        a:
        for (int i = 1; i < coins.length; i++) {
            final int c = coins[i];
            for (int j = 0; j < len; j++) {
                if (c % coins[j] == 0) {
                    continue a;
                }
            }
            coins[len++] = c;
        }
        coins = Arrays.copyOf(coins, len);
        long[] res = new long[(1 << coins.length) - 1];
        iterate(coins, res, 1, 0, 0, true);
        return res;
    }

    private int iterate(int[] coins, long[] res, long mult, int start, int idx, boolean positive) {
        for (int i = start; i < coins.length; i++) {
            long next = mult * coins[i] / gcd(mult, coins[i]);
            res[idx++] = positive ? next : -next;
            idx = iterate(coins, res, next, i + 1, idx, !positive);
        }
        return idx;
    }

    private long gcd(long a, long b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```