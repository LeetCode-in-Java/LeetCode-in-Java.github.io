[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3149\. Find the Minimum Cost Array Permutation

Hard

You are given an array `nums` which is a permutation of `[0, 1, 2, ..., n - 1]`. The **score** of any permutation of `[0, 1, 2, ..., n - 1]` named `perm` is defined as:

`score(perm) = |perm[0] - nums[perm[1]]| + |perm[1] - nums[perm[2]]| + ... + |perm[n - 1] - nums[perm[0]]|`

Return the permutation `perm` which has the **minimum** possible score. If _multiple_ permutations exist with this score, return the one that is lexicographically smallest among them.

**Example 1:**

**Input:** nums = [1,0,2]

**Output:** [0,1,2]

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/04/04/example0gif.gif)**

The lexicographically smallest permutation with minimum cost is `[0,1,2]`. The cost of this permutation is `|0 - 0| + |1 - 2| + |2 - 1| = 2`.

**Example 2:**

**Input:** nums = [0,2,1]

**Output:** [0,2,1]

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/04/04/example1gif.gif)**

The lexicographically smallest permutation with minimum cost is `[0,2,1]`. The cost of this permutation is `|0 - 1| + |2 - 2| + |1 - 0| = 2`.

**Constraints:**

*   `2 <= n == nums.length <= 14`
*   `nums` is a permutation of `[0, 1, 2, ..., n - 1]`.

## Solution

```java
import java.util.Arrays;

public class Solution {
    private int findMinScore(int mask, int prevNum, int[] nums, int[][] dp) {
        int n = nums.length;
        if (Integer.bitCount(mask) == n) {
            dp[mask][prevNum] = Math.abs(prevNum - nums[0]);
            return dp[mask][prevNum];
        }
        if (dp[mask][prevNum] != -1) {
            return dp[mask][prevNum];
        }
        int minScore = Integer.MAX_VALUE;
        for (int currNum = 0; currNum < n; currNum++) {
            if ((mask >> currNum & 1 ^ 1) == 1) {
                int currScore =
                        Math.abs(prevNum - nums[currNum])
                                + findMinScore(mask | 1 << currNum, currNum, nums, dp);
                minScore = Math.min(minScore, currScore);
            }
        }
        dp[mask][prevNum] = minScore;
        return dp[mask][prevNum];
    }

    private int[] constructMinScorePermutation(int n, int[] nums, int[][] dp) {
        int[] permutation = new int[n];
        int i = 0;
        permutation[i++] = 0;
        int prevNum = 0;
        for (int mask = 1; i < n; mask |= 1 << prevNum) {
            for (int currNum = 0; currNum < n; currNum++) {
                if ((mask >> currNum & 1 ^ 1) == 1) {
                    int currScore =
                            Math.abs(prevNum - nums[currNum]) + dp[mask | 1 << currNum][currNum];
                    int minScore = dp[mask][prevNum];
                    if (currScore == minScore) {
                        permutation[i++] = currNum;
                        prevNum = currNum;
                        break;
                    }
                }
            }
        }
        return permutation;
    }

    public int[] findPermutation(int[] nums) {
        int n = nums.length;
        int[][] dp = new int[1 << n][n];
        for (int[] row : dp) {
            Arrays.fill(row, -1);
        }
        findMinScore(1, 0, nums, dp);
        return constructMinScorePermutation(n, nums, dp);
    }
}
```