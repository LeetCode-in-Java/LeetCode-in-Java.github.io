[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3186\. Maximum Total Damage With Spell Casting

Medium

A magician has various spells.

You are given an array `power`, where each element represents the damage of a spell. Multiple spells can have the same damage value.

It is a known fact that if a magician decides to cast a spell with a damage of `power[i]`, they **cannot** cast any spell with a damage of `power[i] - 2`, `power[i] - 1`, `power[i] + 1`, or `power[i] + 2`.

Each spell can be cast **only once**.

Return the **maximum** possible _total damage_ that a magician can cast.

**Example 1:**

**Input:** power = [1,1,3,4]

**Output:** 6

**Explanation:**

The maximum possible damage of 6 is produced by casting spells 0, 1, 3 with damage 1, 1, 4.

**Example 2:**

**Input:** power = [7,1,6,6]

**Output:** 13

**Explanation:**

The maximum possible damage of 13 is produced by casting spells 1, 2, 3 with damage 1, 6, 6.

**Constraints:**

*   <code>1 <= power.length <= 10<sup>5</sup></code>
*   <code>1 <= power[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public long maximumTotalDamage(int[] power) {
        int maxPower = 0;
        for (int p : power) {
            if (p > maxPower) {
                maxPower = p;
            }
        }
        return (maxPower <= 1_000_000) ? smallPower(power, maxPower) : bigPower(power);
    }

    private long smallPower(int[] power, int maxPower) {
        int[] counts = new int[maxPower + 6];
        for (int p : power) {
            counts[p]++;
        }
        long[] dp = new long[maxPower + 6];
        dp[1] = counts[1];
        dp[2] = Math.max(counts[2] * 2L, dp[1]);
        for (int i = 3; i <= maxPower; i++) {
            dp[i] = Math.max((long) counts[i] * i + dp[i - 3], Math.max(dp[i - 1], dp[i - 2]));
        }
        return dp[maxPower];
    }

    private long bigPower(int[] power) {
        Arrays.sort(power);
        int n = power.length;
        long[] prevs = new long[4];
        int curPower = power[0];
        int count = 1;
        long result = 0;
        for (int i = 1; i <= n; i++) {
            int p = (i == n) ? 1_000_000_009 : power[i];
            if (p == curPower) {
                count++;
            } else {
                long curVal =
                        Math.max((long) curPower * count + prevs[3], Math.max(prevs[1], prevs[2]));
                int diff = Math.min(p - curPower, prevs.length - 1);
                long nextCurVal = (diff == 1) ? 0 : Math.max(prevs[3], Math.max(curVal, prevs[2]));
                // Shift the values in prevs[].
                int k = prevs.length - 1;
                if (diff < prevs.length - 1) {
                    while (k > diff) {
                        prevs[k] = prevs[k-- - diff];
                    }
                    prevs[k--] = curVal;
                }
                while (k > 0) {
                    prevs[k--] = nextCurVal;
                }
                curPower = p;
                count = 1;
            }
        }
        for (long v : prevs) {
            if (v > result) {
                result = v;
            }
        }
        return result;
    }
}
```