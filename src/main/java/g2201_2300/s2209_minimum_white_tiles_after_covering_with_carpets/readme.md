[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2209\. Minimum White Tiles After Covering With Carpets

Hard

You are given a **0-indexed binary** string `floor`, which represents the colors of tiles on a floor:

*   `floor[i] = '0'` denotes that the <code>i<sup>th</sup></code> tile of the floor is colored **black**.
*   On the other hand, `floor[i] = '1'` denotes that the <code>i<sup>th</sup></code> tile of the floor is colored **white**.

You are also given `numCarpets` and `carpetLen`. You have `numCarpets` **black** carpets, each of length `carpetLen` tiles. Cover the tiles with the given carpets such that the number of **white** tiles still visible is **minimum**. Carpets may overlap one another.

Return _the **minimum** number of white tiles still visible._

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/02/10/ex1-1.png)

**Input:** floor = "10110101", numCarpets = 2, carpetLen = 2

**Output:** 2

**Explanation:**

The figure above shows one way of covering the tiles with the carpets such that only 2 white tiles are visible.

No other way of covering the tiles with the carpets can leave less than 2 white tiles visible. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/02/10/ex2.png)

**Input:** floor = "11111", numCarpets = 2, carpetLen = 3

**Output:** 0

**Explanation:**

The figure above shows one way of covering the tiles with the carpets such that no white tiles are visible.

Note that the carpets are able to overlap one another. 

**Constraints:**

*   `1 <= carpetLen <= floor.length <= 1000`
*   `floor[i]` is either `'0'` or `'1'`.
*   `1 <= numCarpets <= 1000`

## Solution

```java
public class Solution {
    public int minimumWhiteTiles(String floor, int numCarpets, int carpetLen) {
        int len = floor.length();
        int[][] dp = new int[numCarpets + 1][len + 1];
        int[] prefix = new int[len];
        int tiles = 0;
        int total = 0;
        for (int i = 0; i < len; i++) {
            // calculate total no of Tiles within the Carpet Length Window
            tiles += floor.charAt(i) - '0';
            // start excluding tiles which are not in the Range anymore of the Carpet Length given
            if (i - carpetLen >= 0) {
                tiles -= floor.charAt(i - carpetLen) - '0';
            }
            // the total no of tiles covered within the Carpet Length range for current index
            prefix[i] = tiles;
            total += floor.charAt(i) - '0';
        }
        for (int i = 1; i <= numCarpets; i++) {
            for (int j = 0; j < len; j++) {
                // if we do not wish to cover current Tile
                int doNot = dp[i][j];
                // if we do wish to cover current tile
                int doTake = dp[i - 1][Math.max(0, j - carpetLen + 1)] + prefix[j];
                // we should go back the Carpet length & check for tiles not covered before j -
                // carpet Length distance
                dp[i][j + 1] = Math.max(doTake, doNot);
            }
        }
        return total - dp[numCarpets][len];
    }
}
```