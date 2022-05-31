## 1494\. Parallel Courses II

Hard

You are given an integer `n`, which indicates that there are `n` courses labeled from `1` to `n`. You are also given an array `relations` where <code>relations[i] = [prevCourse<sub>i</sub>, nextCourse<sub>i</sub>]</code>, representing a prerequisite relationship between course <code>prevCourse<sub>i</sub></code> and course <code>nextCourse<sub>i</sub></code>: course <code>prevCourse<sub>i</sub></code> has to be taken before course <code>nextCourse<sub>i</sub></code>. Also, you are given the integer `k`.

In one semester, you can take **at most** `k` courses as long as you have taken all the prerequisites in the **previous** semester for the courses you are taking.

Return _the **minimum** number of semesters needed to take all courses_. The testcases will be generated such that it is possible to take every course.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2020/05/22/leetcode_parallel_courses_1.png)**

**Input:** n = 4, dependencies = \[\[2,1],[3,1],[1,4]], k = 2

**Output:** 3

**Explanation:** The figure above represents the given graph. In the first semester, you can take courses 2 and 3. In the second semester, you can take course 1. In the third semester, you can take course 4.

**Example 2:**

**![](https://assets.leetcode.com/uploads/2020/05/22/leetcode_parallel_courses_2.png)**

**Input:** n = 5, dependencies = \[\[2,1],[3,1],[4,1],[1,5]], k = 2

**Output:** 4

**Explanation:** The figure above represents the given graph. In the first semester, you can take courses 2 and 3 only since you cannot take more than two per semester. In the second semester, you can take course 4. In the third semester, you can take course 1. In the fourth semester, you can take course 5.

**Example 3:**

**Input:** n = 11, dependencies = [], k = 2

**Output:** 6

**Constraints:**

*   `1 <= n <= 15`
*   `1 <= k <= n`
*   `0 <= relations.length <= n * (n-1) / 2`
*   `relations[i].length == 2`
*   <code>1 <= prevCourse<sub>i</sub>, nextCourse<sub>i</sub> <= n</code>
*   <code>prevCourse<sub>i</sub> != nextCourse<sub>i</sub></code>
*   All the pairs <code>[prevCourse<sub>i</sub>, nextCourse<sub>i</sub>]</code> are **unique**.
*   The given graph is a directed acyclic graph.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int minNumberOfSemesters(int n, int[][] relations, int k) {
        int[] pres = new int[n];
        for (int[] r : relations) {
            int prev = r[0] - 1;
            int next = r[1] - 1;
            pres[next] |= (1 << prev);
        }
        int[] dp = new int[1 << n];
        Arrays.fill(dp, n);
        dp[0] = 0;
        for (int mask = 0; mask < dp.length; mask++) {
            int canTake = 0;
            for (int i = 0; i < n; i++) {
                // already taken
                if ((mask & (1 << i)) != 0) {
                    continue;
                }
                // satisfy all pres
                if ((mask & pres[i]) == pres[i]) {
                    canTake |= (1 << i);
                }
            }
            // loop each sub-masks
            for (int take = canTake; take > 0; take = (take - 1) & canTake) {
                if (Integer.bitCount(take) > k) {
                    continue;
                }
                dp[take | mask] = Math.min(dp[take | mask], dp[mask] + 1);
            }
        }
        return dp[(1 << n) - 1];
    }
}
```