[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1223\. Dice Roll Simulation

Hard

A die simulator generates a random number from `1` to `6` for each roll. You introduced a constraint to the generator such that it cannot roll the number `i` more than `rollMax[i]` (**1-indexed**) consecutive times.

Given an array of integers `rollMax` and an integer `n`, return _the number of distinct sequences that can be obtained with exact_ `n` _rolls_. Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

Two sequences are considered different if at least one element differs from each other.

**Example 1:**

**Input:** n = 2, rollMax = [1,1,2,2,2,3]

**Output:** 34

**Explanation:** There will be 2 rolls of die, if there are no constraints on the die, there are 6 \* 6 = 36 possible combinations. In this case, looking at rollMax array, the numbers 1 and 2 appear at most once consecutively, therefore sequences (1,1) and (2,2) cannot occur, so the final answer is 36-2 = 34.

**Example 2:**

**Input:** n = 2, rollMax = [1,1,1,1,1,1]

**Output:** 30

**Example 3:**

**Input:** n = 3, rollMax = [1,1,1,2,2,3]

**Output:** 181

**Constraints:**

*   `1 <= n <= 5000`
*   `rollMax.length == 6`
*   `1 <= rollMax[i] <= 15`

## Solution

```java
public class Solution {
    private static final long MOD = 1000000007;

    public int dieSimulator(int n, int[] rollMax) {
        long[][] all = new long[6][15 + 1];
        long[] countsBySide = new long[6];
        long total = 0;
        long newTotal;
        int max;
        for (int j = 0; j < all.length; j++) {
            all[j][1] = 1;
            countsBySide[j] = 1;

            total = 6;
        }
        for (int i = 1; i < n; i++) {
            newTotal = total;
            for (int j = 0; j < all.length; j++) {
                all[j][0] = (total - countsBySide[j]) % MOD;
                max = rollMax[j];
                newTotal = (newTotal - all[j][max] + all[j][0]);
                countsBySide[j] = (total - all[j][max]) % MOD;
                System.arraycopy(all[j], 0, all[j], 1, max);
            }
            total = newTotal;
        }
        return (int) (total % MOD);
    }
}
```