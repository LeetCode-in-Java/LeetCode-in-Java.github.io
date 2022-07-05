[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

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
public class Solution {
    public boolean makesquare(int[] matchsticks) {
        if (matchsticks.length < 4) {
            return false;
        }
        int totalSum = 0;
        for (int m : matchsticks) {
            totalSum += m;
        }
        if (totalSum % 4 != 0) {
            return false;
        }
        int target = totalSum / 4;
        boolean[] used = new boolean[matchsticks.length];
        return backtracking(matchsticks, used, target, 0, 0, 0);
    }

    private boolean backtracking(
            int[] matchsticks, boolean[] used, int target, int count, int currSum, int start) {
        if (count == 3) {
            return true;
        }
        if (currSum > target) {
            return false;
        }
        if (currSum == target) {
            return backtracking(matchsticks, used, target, count + 1, 0, 0);
        }
        for (int i = start; i < matchsticks.length; i++) {
            if (!used[i]) {
                used[i] = true;
                if (backtracking(
                        matchsticks, used, target, count, currSum + matchsticks[i], i + 1)) {
                    return true;
                }
                used[i] = false;
            }
        }
        return false;
    }
}
```