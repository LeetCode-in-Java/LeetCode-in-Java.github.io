[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2271\. Maximum White Tiles Covered by a Carpet

Medium

You are given a 2D integer array `tiles` where <code>tiles[i] = [l<sub>i</sub>, r<sub>i</sub>]</code> represents that every tile `j` in the range <code>l<sub>i</sub> <= j <= r<sub>i</sub></code> is colored white.

You are also given an integer `carpetLen`, the length of a single carpet that can be placed **anywhere**.

Return _the **maximum** number of white tiles that can be covered by the carpet_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/03/25/example1drawio3.png)

**Input:** tiles = \[\[1,5],[10,11],[12,18],[20,25],[30,32]], carpetLen = 10

**Output:** 9

**Explanation:** Place the carpet starting on tile 10. 

It covers 9 white tiles, so we return 9. 

Note that there may be other places where the carpet covers 9 white tiles. 

It can be shown that the carpet cannot cover more than 9 white tiles.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/03/24/example2drawio.png)

**Input:** tiles = \[\[10,11],[1,1]], carpetLen = 2

**Output:** 2

**Explanation:** Place the carpet starting on tile 10. 

It covers 2 white tiles, so we return 2.

**Constraints:**

*   <code>1 <= tiles.length <= 5 * 10<sup>4</sup></code>
*   `tiles[i].length == 2`
*   <code>1 <= l<sub>i</sub> <= r<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>1 <= carpetLen <= 10<sup>9</sup></code>
*   The `tiles` are **non-overlapping**.

## Solution

```java
import java.util.Arrays;
import java.util.Comparator;

public class Solution {
    public int maximumWhiteTiles(int[][] tiles, int carpetLength) {
        Arrays.sort(tiles, Comparator.comparingInt(x -> x[0]));
        int currentCover = Math.min(tiles[0][1] - tiles[0][0] + 1, carpetLength);
        int maxCover = currentCover;
        int head = 1;
        int tail = 0;
        while (tail < tiles.length && head < tiles.length && maxCover < carpetLength) {
            if (tiles[head][1] - tiles[tail][0] + 1 <= carpetLength) {
                currentCover += tiles[head][1] - tiles[head][0] + 1;
                maxCover = Math.max(maxCover, currentCover);
                ++head;
            } else {
                int possiblePartialCoverOverCurrentHead =
                        carpetLength - (tiles[head][0] - tiles[tail][0]);
                maxCover = Math.max(maxCover, currentCover + possiblePartialCoverOverCurrentHead);
                currentCover = currentCover - (tiles[tail][1] - tiles[tail][0] + 1);
                ++tail;
            }
        }
        return maxCover;
    }
}
```