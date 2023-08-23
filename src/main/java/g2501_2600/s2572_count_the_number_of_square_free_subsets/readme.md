[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2572\. Count the Number of Square-Free Subsets

Medium

You are given a positive integer **0-indexed** array `nums`.

A subset of the array `nums` is **square-free** if the product of its elements is a **square-free integer**.

A **square-free integer** is an integer that is divisible by no square number other than `1`.

Return _the number of square-free non-empty subsets of the array_ **nums**. Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

A **non-empty** **subset** of `nums` is an array that can be obtained by deleting some (possibly none but not all) elements from `nums`. Two subsets are different if and only if the chosen indices to delete are different.

**Example 1:**

**Input:** nums = [3,4,4,5]

**Output:** 3

**Explanation:** There are 3 square-free subsets in this example: 

- The subset consisting of the 0<sup>th</sup> element [3]. The product of its elements is 3, which is a square-free integer.

- The subset consisting of the 3<sup>rd</sup> element [5]. The product of its elements is 5, which is a square-free integer. 

- The subset consisting of 0<sup>th</sup> and 3<sup>rd</sup> elements [3,5]. The product of its elements is 15, which is a square-free integer. 

It can be proven that there are no more than 3 square-free subsets in the given array.

**Example 2:**

**Input:** nums = [1]

**Output:** 1

**Explanation:** There is 1 square-free subset in this example: 

- The subset consisting of the 0<sup>th</sup> element [1]. The product of its elements is 1, which is a square-free integer. 

It can be proven that there is no more than 1 square-free subset in the given array.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   `1 <= nums[i] <= 30`

## Solution

```java
public class Solution {
    private final int[] count = new int[31];
    private final int[] masks = new int[31];
    private final long[][] cache = new long[31][1 << 6];
    private static final long MOD = 1_000_000_007;

    public int squareFreeSubsets(int[] nums) {
        int[] p = {1, 2, 3, 5, 7, 11, 13};
        for (int i = 0; i < 7; ++i) {
            int mask = i == 0 ? 0 : 1 << (i - 1);
            for (int j = i + 1; j < 7; ++j) {
                if (p[i] * p[j] > 30) {
                    break;
                }
                masks[p[i] * p[j]] = mask | (1 << (j - 1));
            }
        }
        masks[30] = 7;
        for (int k : nums) {
            if (k % 4 != 0 && k % 9 != 0 && k % 25 != 0) {
                ++count[k];
            }
        }
        count[1] = powof2(count[1]);
        return (int) ((dfs(30, 0) + MOD - 1) % MOD);
    }

    private long dfs(int k, int mask) {
        if (k == 1) {
            return count[1];
        }
        if (cache[k][mask] != 0) {
            return cache[k][mask];
        }
        long res = dfs(k - 1, mask);
        if (count[k] > 0 && (masks[k] & mask) == 0) {
            res = (res + (count[k] * dfs(k - 1, mask | masks[k])) % MOD) % MOD;
        }
        cache[k][mask] = res;
        return cache[k][mask];
    }

    private int powof2(int k) {
        long pow2 = 2;
        long res = 1;
        while (k > 0) {
            if (k % 2 == 1) {
                res = (res * pow2) % MOD;
            }
            pow2 = (pow2 * pow2) % MOD;
            k >>= 1;
        }
        return (int) res;
    }
}
```