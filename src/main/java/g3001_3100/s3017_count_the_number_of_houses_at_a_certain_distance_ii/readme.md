[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3017\. Count the Number of Houses at a Certain Distance II

Hard

You are given three **positive** integers `n`, `x`, and `y`.

In a city, there exist houses numbered `1` to `n` connected by `n` streets. There is a street connecting the house numbered `i` with the house numbered `i + 1` for all `1 <= i <= n - 1` . An additional street connects the house numbered `x` with the house numbered `y`.

For each `k`, such that `1 <= k <= n`, you need to find the number of **pairs of houses** <code>(house<sub>1</sub>, house<sub>2</sub>)</code> such that the **minimum** number of streets that need to be traveled to reach <code>house<sub>2</sub></code> from <code>house<sub>1</sub></code> is `k`.

Return _a **1-indexed** array_ `result` _of length_ `n` _where_ `result[k]` _represents the **total** number of pairs of houses such that the **minimum** streets required to reach one house from the other is_ `k`.

**Note** that `x` and `y` can be **equal**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/12/20/example2.png)

**Input:** n = 3, x = 1, y = 3

**Output:** [6,0,0]

**Explanation:** Let's look at each pair of houses: 
- For the pair (1, 2), we can go from house 1 to house 2 directly. 
- For the pair (2, 1), we can go from house 2 to house 1 directly. 
- For the pair (1, 3), we can go from house 1 to house 3 directly. 
- For the pair (3, 1), we can go from house 3 to house 1 directly. 
- For the pair (2, 3), we can go from house 2 to house 3 directly. 
- For the pair (3, 2), we can go from house 3 to house 2 directly.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/12/20/example3.png)

**Input:** n = 5, x = 2, y = 4

**Output:** [10,8,2,0,0]

**Explanation:** For each distance k the pairs are: 
- For k == 1, the pairs are (1, 2), (2, 1), (2, 3), (3, 2), (2, 4), (4, 2), (3, 4), (4, 3), (4, 5), and (5, 4). 
- For k == 2, the pairs are (1, 3), (3, 1), (1, 4), (4, 1), (2, 5), (5, 2), (3, 5), and (5, 3). 
- For k == 3, the pairs are (1, 5), and (5, 1). 
- For k == 4 and k == 5, there are no pairs.

**Example 3:**

![](https://assets.leetcode.com/uploads/2023/12/20/example5.png)

**Input:** n = 4, x = 1, y = 1

**Output:** [6,4,2,0]

**Explanation:** For each distance k the pairs are: 
- For k == 1, the pairs are (1, 2), (2, 1), (2, 3), (3, 2), (3, 4), and (4, 3).
- For k == 2, the pairs are (1, 3), (3, 1), (2, 4), and (4, 2).
- For k == 3, the pairs are (1, 4), and (4, 1). 
- For k == 4, there are no pairs.

**Constraints:**

*   <code>2 <= n <= 10<sup>5</sup></code>
*   `1 <= x, y <= n`

## Solution

```java
public class Solution {
    public long[] countOfPairs(int n, int x, int y) {
        long[] result = new long[n];
        int leftCount = Math.min(x, y) - 1;
        int rightCount = n - Math.max(x, y);
        int circleCount = n - leftCount - rightCount;
        circleInternal(circleCount, result);
        lineToCircle(leftCount, circleCount, result);
        lineToCircle(rightCount, circleCount, result);
        lineToLine(leftCount, rightCount, x == y ? 1 : 2, result);
        lineInternal(leftCount, result);
        lineInternal(rightCount, result);
        return result;
    }

    private void lineToCircle(int lineCount, int circleCount, long[] curRes) {
        int circleLen = circleCount / 2 + 1;
        int curModifier = 0;
        for (int i = 1; i < circleLen + lineCount; ++i) {
            if (i <= Math.min(lineCount, circleLen)) {
                curModifier += 4;
            } else if (i > Math.max(lineCount, circleLen)) {
                curModifier -= 4;
            }
            curRes[i - 1] += curModifier;
            if (i <= lineCount) {
                curRes[i - 1] -= 2;
            }
            if (i >= circleLen && circleCount % 2 == 0) {
                curRes[i - 1] -= 2;
            }
        }
    }

    private void lineToLine(int lineCount1, int lineCount2, int initialDis, long[] curRes) {
        int curModifier = 0;
        for (int i = 1; i < lineCount1 + lineCount2; ++i) {
            if (i <= Math.min(lineCount1, lineCount2)) {
                curModifier += 2;
            } else if (i > Math.max(lineCount1, lineCount2)) {
                curModifier -= 2;
            }
            curRes[i - 1 + initialDis] += curModifier;
        }
    }

    private void lineInternal(int lineCount, long[] curRes) {
        for (int i = 1; i < lineCount; ++i) {
            curRes[i - 1] += (lineCount - i) * 2L;
        }
    }

    private void circleInternal(int circleCount, long[] curRes) {
        for (int i = 0; i < circleCount / 2; ++i) {
            if (circleCount % 2 == 0 && i + 1 == circleCount / 2) {
                curRes[i] += circleCount;
            } else {
                curRes[i] += circleCount * 2L;
            }
        }
    }
}
```