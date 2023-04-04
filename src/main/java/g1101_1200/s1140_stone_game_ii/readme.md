[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1140\. Stone Game II

Medium

Alice and Bob continue their games with piles of stones. There are a number of piles **arranged in a row**, and each pile has a positive integer number of stones `piles[i]`. The objective of the game is to end with the most stones.

Alice and Bob take turns, with Alice starting first. Initially, `M = 1`.

On each player's turn, that player can take **all the stones** in the **first** `X` remaining piles, where `1 <= X <= 2M`. Then, we set `M = max(M, X)`.

The game continues until all the stones have been taken.

Assuming Alice and Bob play optimally, return the maximum number of stones Alice can get.

**Example 1:**

**Input:** piles = [2,7,9,4,4]

**Output:** 10

**Explanation:** If Alice takes one pile at the beginning, Bob takes two piles, then Alice takes 2 piles again. Alice can get 2 + 4 + 4 = 10 piles in total. If Alice takes two piles at the beginning, then Bob can take all three piles left. In this case, Alice get 2 + 7 = 9 piles in total. So we return 10 since it's larger.

**Example 2:**

**Input:** piles = [1,2,3,4,5,100]

**Output:** 104

**Constraints:**

*   `1 <= piles.length <= 100`
*   <code>1 <= piles[i] <= 10<sup>4</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    private int[][] dp = new int[105][105];

    private int help(int i, int m, int[] p) {
        if (i >= p.length) {
            dp[i][m] = 0;
            return 0;
        }
        if (dp[i][m] != -1) {
            return dp[i][m];
        }
        int ans = Integer.MIN_VALUE;
        int total = 0;
        for (int j = 0; j < 2 * m; j++) {
            if (i + j < p.length) {
                total += p[i + j];
                ans = Math.max(ans, total - help(i + j + 1, Math.max(m, j + 1), p));
            }
        }
        dp[i][m] = ans;
        return ans;
    }

    public int stoneGameII(int[] piles) {
        int sum = 0;
        for (int[] arr1 : dp) {
            Arrays.fill(arr1, -1);
        }
        for (int z : piles) {
            sum += z;
        }
        return (sum + help(0, 1, piles)) / 2;
    }
}
```