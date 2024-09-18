[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3281\. Maximize Score of Numbers in Ranges

Medium

You are given an array of integers `start` and an integer `d`, representing `n` intervals `[start[i], start[i] + d]`.

You are asked to choose `n` integers where the <code>i<sup>th</sup></code> integer must belong to the <code>i<sup>th</sup></code> interval. The **score** of the chosen integers is defined as the **minimum** absolute difference between any two integers that have been chosen.

Return the **maximum** _possible score_ of the chosen integers.

**Example 1:**

**Input:** start = [6,0,3], d = 2

**Output:** 4

**Explanation:**

The maximum possible score can be obtained by choosing integers: 8, 0, and 4. The score of these chosen integers is `min(|8 - 0|, |8 - 4|, |0 - 4|)` which equals 4.

**Example 2:**

**Input:** start = [2,6,13,13], d = 5

**Output:** 5

**Explanation:**

The maximum possible score can be obtained by choosing integers: 2, 7, 13, and 18. The score of these chosen integers is `min(|2 - 7|, |2 - 13|, |2 - 18|, |7 - 13|, |7 - 18|, |13 - 18|)` which equals 5.

**Constraints:**

*   <code>2 <= start.length <= 10<sup>5</sup></code>
*   <code>0 <= start[i] <= 10<sup>9</sup></code>
*   <code>0 <= d <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int maxPossibleScore(int[] start, int d) {
        Arrays.sort(start);
        int n = start.length;
        int l = 0;
        int r = start[n - 1] - start[0] + d + 1;
        while (l < r) {
            int m = l + (r - l) / 2;
            if (isPossible(start, d, m)) {
                l = m + 1;
            } else {
                r = m;
            }
        }
        return l - 1;
    }

    private boolean isPossible(int[] start, int d, int score) {
        int pre = start[0];
        for (int i = 1; i < start.length; i++) {
            if (start[i] + d - pre < score) {
                return false;
            }
            pre = Math.max(start[i], pre + score);
        }
        return true;
    }
}
```