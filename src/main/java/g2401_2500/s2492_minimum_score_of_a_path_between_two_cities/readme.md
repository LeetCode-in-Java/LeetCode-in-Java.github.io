[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2492\. Minimum Score of a Path Between Two Cities

Medium

You are given a positive integer `n` representing `n` cities numbered from `1` to `n`. You are also given a **2D** array `roads` where <code>roads[i] = [a<sub>i</sub>, b<sub>i</sub>, distance<sub>i</sub>]</code> indicates that there is a **bidirectional** road between cities <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code> with a distance equal to <code>distance<sub>i</sub></code>. The cities graph is not necessarily connected.

The **score** of a path between two cities is defined as the **minimum** distance of a road in this path.

Return _the **minimum** possible score of a path between cities_ `1` _and_ `n`.

**Note**:

*   A path is a sequence of roads between two cities.
*   It is allowed for a path to contain the same road **multiple** times, and you can visit cities `1` and `n` multiple times along the path.
*   The test cases are generated such that there is **at least** one path between `1` and `n`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/10/12/graph11.png)

**Input:** n = 4, roads = \[\[1,2,9],[2,3,6],[2,4,5],[1,4,7]]

**Output:** 5

**Explanation:** The path from city 1 to 4 with the minimum score is: 1 -> 2 -> 4. The score of this path is min(9,5) = 5. It can be shown that no other path has less score.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/10/12/graph22.png)

**Input:** n = 4, roads = \[\[1,2,2],[1,3,4],[3,4,7]]

**Output:** 2

**Explanation:** The path from city 1 to 4 with the minimum score is: 1 -> 2 -> 1 -> 3 -> 4. The score of this path is min(2,2,4,7) = 2.

**Constraints:**

*   <code>2 <= n <= 10<sup>5</sup></code>
*   <code>1 <= roads.length <= 10<sup>5</sup></code>
*   `roads[i].length == 3`
*   <code>1 <= a<sub>i</sub>, b<sub>i</sub> <= n</code>
*   <code>a<sub>i</sub> != b<sub>i</sub></code>
*   <code>1 <= distance<sub>i</sub> <= 10<sup>4</sup></code>
*   There are no repeated edges.
*   There is at least one path between `1` and `n`.

## Solution

```java
import java.util.Arrays;

public class Solution {
    private int[] dsu;

    public int minScore(int n, int[][] roads) {
        dsu = new int[n + 1];
        int[] ans = new int[n + 1];
        for (int i = 0; i <= n; i++) {
            dsu[i] = i;
        }
        Arrays.fill(ans, Integer.MAX_VALUE);
        for (int[] r : roads) {
            int a = find(r[0]);
            int b = find(r[1]);
            dsu[a] = dsu[b];
            ans[a] = ans[b] = Math.min(r[2], Math.min(ans[a], ans[b]));
        }
        return ans[find(1)];
    }

    private int find(int i) {
        return dsu[i] == i ? i : find(dsu[i]);
    }
}
```