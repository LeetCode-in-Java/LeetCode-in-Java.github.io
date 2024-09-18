[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3290\. Maximum Multiplication Score

Medium

You are given an integer array `a` of size 4 and another integer array `b` of size **at least** 4.

You need to choose 4 indices <code>i<sub>0</sub></code>, <code>i<sub>1</sub></code>, <code>i<sub>2</sub></code>, and <code>i<sub>3</sub></code> from the array `b` such that <code>i<sub>0</sub> < i<sub>1</sub> < i<sub>2</sub> < i<sub>3</sub></code>. Your score will be equal to the value <code>a[0] * b[i<sub>0</sub>] + a[1] * b[i<sub>1</sub>] + a[2] * b[i<sub>2</sub>] + a[3] * b[i<sub>3</sub>]</code>.

Return the **maximum** score you can achieve.

**Example 1:**

**Input:** a = [3,2,5,6], b = [2,-6,4,-5,-3,2,-7]

**Output:** 26

**Explanation:**   
 We can choose the indices 0, 1, 2, and 5. The score will be `3 * 2 + 2 * (-6) + 5 * 4 + 6 * 2 = 26`.

**Example 2:**

**Input:** a = [-1,4,5,-2], b = [-5,-1,-3,-2,-4]

**Output:** \-1

**Explanation:**   
 We can choose the indices 0, 1, 3, and 4. The score will be `(-1) * (-5) + 4 * (-1) + 5 * (-2) + (-2) * (-4) = -1`.

**Constraints:**

*   `a.length == 4`
*   <code>4 <= b.length <= 10<sup>5</sup></code>
*   <code>-10<sup>5</sup> <= a[i], b[i] <= 10<sup>5</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public long maxScore(int[] a, int[] b) {
        long[] dp = new long[4];
        Arrays.fill(dp, (long) -1e11);
        for (int bi : b) {
            for (int i = 3; i >= 0; i--) {
                dp[i] = Math.max(dp[i], (i > 0 ? dp[i - 1] : 0) + (long) a[i] * bi);
            }
        }
        return dp[3];
    }
}
```