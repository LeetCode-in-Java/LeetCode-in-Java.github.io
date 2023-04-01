[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 473\. Matchsticks to Square

Medium

You are given an integer array `matchsticks` where `matchsticks[i]` is the length of the <code>i<sup>th</sup></code> matchstick. You want to use **all the matchsticks** to make one square. You **should not break** any stick, but you can link them up, and each matchstick must be used **exactly one time**.

Return `true` if you can make this square and `false` otherwise.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/09/matchsticks1-grid.jpg)

**Input:** matchsticks = [1,1,2,2,2]

**Output:** true

**Explanation:** You can form a square with length 2, one side of the square came two sticks with length 1.

**Example 2:**

**Input:** matchsticks = [3,3,3,3,4]

**Output:** false

**Explanation:** You cannot find a way to form a square with all the matchsticks.

**Constraints:**

*   `1 <= matchsticks.length <= 15`
*   <code>1 <= matchsticks[i] <= 10<sup>8</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public boolean makesquare(int[] matchsticks) {
        if (matchsticks.length < 4) {
            return false;
        }
        int per = 0;
        for (int ele : matchsticks) {
            per = ele + per;
        }
        if (per % 4 != 0) {
            return false;
        }
        Arrays.sort(matchsticks);
        int side = per / 4;
        int[] sides = new int[] {side, side, side, side};
        return help(matchsticks, matchsticks.length - 1, sides);
    }

    private boolean help(int[] nums, int i, int[] side) {
        if (i == -1) {
            return side[0] == 0 && side[1] == 0 && side[2] == 0 && side[3] == 0;
        }
        for (int j = 0; j < 4; j++) {
            if (nums[i] > side[j]) {
                continue;
            }
            side[j] -= nums[i];
            if (help(nums, i - 1, side)) {
                return true;
            }
            side[j] += nums[i];
        }
        return false;
    }
}
```