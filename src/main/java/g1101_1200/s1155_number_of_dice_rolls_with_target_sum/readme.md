[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1155\. Number of Dice Rolls With Target Sum

Medium

You have `n` dice and each die has `k` faces numbered from `1` to `k`.

Given three integers `n`, `k`, and `target`, return _the number of possible ways (out of the_ <code>k<sup>n</sup></code> _total ways)_ _to roll the dice so the sum of the face-up numbers equals_ `target`. Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** n = 1, k = 6, target = 3

**Output:** 1

**Explanation:** You throw one die with 6 faces. 

There is only one way to get a sum of 3.

**Example 2:**

**Input:** n = 2, k = 6, target = 7

**Output:** 6

**Explanation:** You throw two dice, each with 6 faces. 

There are 6 ways to get a sum of 7: 1+6, 2+5, 3+4, 4+3, 5+2, 6+1.

**Example 3:**

**Input:** n = 30, k = 30, target = 500

**Output:** 222616187

**Explanation:** The answer must be returned modulo 10<sup>9</sup> + 7.

**Constraints:**

*   `1 <= n, k <= 30`
*   `1 <= target <= 1000`

## Solution

```java
import java.util.Arrays;

public class Solution {
    private int[][] memo;
    private int k;

    private int dp(int diceLeft, int targetLeft) {
        if (diceLeft == 0) {
            if (targetLeft == 0) {
                return 1;
            }
            return 0;
        }
        if (memo[diceLeft][targetLeft] == -1) {
            int res = 0;
            for (int i = 1; i <= Math.min(k, targetLeft); i++) {
                res += dp(diceLeft - 1, targetLeft - i);
                int modulo = 1000000007;
                res %= modulo;
            }
            memo[diceLeft][targetLeft] = res;
        }
        return memo[diceLeft][targetLeft];
    }

    public int numRollsToTarget(int n, int k, int target) {
        this.k = k;
        this.memo = new int[n + 1][target + 1];
        for (int[] i : memo) {
            Arrays.fill(i, -1);
        }
        return dp(n, target);
    }
}
```