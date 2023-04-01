[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1007\. Minimum Domino Rotations For Equal Row

Medium

In a row of dominoes, `tops[i]` and `bottoms[i]` represent the top and bottom halves of the <code>i<sup>th</sup></code> domino. (A domino is a tile with two numbers from 1 to 6 - one on each half of the tile.)

We may rotate the <code>i<sup>th</sup></code> domino, so that `tops[i]` and `bottoms[i]` swap values.

Return the minimum number of rotations so that all the values in `tops` are the same, or all the values in `bottoms` are the same.

If it cannot be done, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/14/domino.png)

**Input:** tops = [2,1,2,4,2,2], bottoms = [5,2,6,2,3,2]

**Output:** 2

**Explanation:** The first figure represents the dominoes as given by tops and bottoms: before we do any rotations. If we rotate the second and fourth dominoes, we can make every value in the top row equal to 2, as indicated by the second figure.

**Example 2:**

**Input:** tops = [3,5,1,2,3], bottoms = [3,6,3,3,4]

**Output:** -1

**Explanation:** In this case, it is not possible to rotate the dominoes to make one row of values equal.

**Constraints:**

*   <code>2 <= tops.length <= 2 * 10<sup>4</sup></code>
*   `bottoms.length == tops.length`
*   `1 <= tops[i], bottoms[i] <= 6`

## Solution

```java
public class Solution {
    public int minDominoRotations(int[] tops, int[] bottoms) {
        int top = tops[0];
        int tCount = 0;
        int bCount = 0;
        int tSwaps;
        int bSwaps;
        int swaps = 0;
        boolean valid = true;
        for (int i = 0; i < tops.length; i++) {
            if (tops[i] == top) {
                tCount++;
            }
            if (bottoms[i] == top) {
                bCount++;
            }
            if (tops[i] != top && bottoms[i] != top) {
                valid = false;
                swaps = -1;
                break;
            }
        }
        if (valid) {
            tSwaps = tops.length - tCount;
            bSwaps = bottoms.length - bCount;
            swaps = Math.min(tSwaps, bSwaps);
        }
        int bottom = bottoms[0];
        int tCount1 = 0;
        int bCount1 = 0;
        int tSwaps1;
        int bSwaps1;
        int swaps1 = 0;
        boolean valid1 = true;
        for (int i = 0; i < bottoms.length; i++) {
            if (tops[i] == bottom) {
                tCount1++;
            }
            if (bottoms[i] == bottom) {
                bCount1++;
            }
            if (tops[i] != bottom && bottoms[i] != bottom) {
                valid1 = false;
                swaps1 = -1;
                break;
            }
        }
        if (valid1) {
            tSwaps1 = tops.length - tCount1;
            bSwaps1 = bottoms.length - bCount1;
            swaps1 = Math.min(tSwaps1, bSwaps1);
        }
        int[] ans = new int[2];
        if (swaps1 < swaps) {
            ans[0] = swaps1;
            ans[1] = swaps;
        } else {
            ans[0] = swaps;
            ans[1] = swaps1;
        }
        if (ans[0] != -1) {
            return ans[0];
        } else {
            return ans[1];
        }
    }
}
```